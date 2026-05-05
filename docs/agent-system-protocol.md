# STS2 AI 自主代理系统协议

本文件是新体系的最高层执行协议，只写稳定规则，不写当前难度、当前楼层、当前牌组。当前会话状态只允许写入 `examples/current-run-state-snapshot.example.md`。

## 1. 任务边界与自主权范围

- AI 是当前 Run 的决策主体。用户只负责手动启动游戏/新局；Boss 战前由 AI 自行定位最新 STS2Saves 自动存档并执行镜像备份，不再打断用户确认。
- AI 通过 `docs/mcp-http-action-schema.md` 定义的 MCP HTTP endpoint 读取状态、做决策并执行操作。
- 禁止运行 `watchdog/auto_slay_watchdog.ps1`，禁止启动任何后台自动刷局进程。
- 禁止 AI 自行调用 STS2Saves 回档、恢复快照或重开新局；回档和重开必须由用户决定。
- Boss 战前必须暂停进入 Boss 的 POST，先找到最新 STS2Saves 自动存档，再运行 `scripts/mirror_sts2saves.ps1` 做镜像备份并验证输出；完成后无需提醒用户，直接按当前决策继续进入 Boss。普通节点、普通战斗、奖励页不做存档提醒。
- 若触发 `EMERGENCY_HALT`、用户要求重开且上一局未写总结、或一局结果已确定，AI 必须先完成知识库写入/报警流程，再等待用户。

## 2. RAG 启动读取顺序

新会话只读取这些活动文件：`docs/agent-system-protocol.md`、`examples/current-run-state-snapshot.example.md`、`docs/ironclad-strategy-playbook.md`、`docs/screen-flow-controller.md`、`docs/ironclad-card-knowledge-base.md`、`docs/monster-encounter-knowledge-base.md`、`docs/mcp-http-action-schema.md`、`docs/ascension-risk-model.md`。

`docs/run-review-learning-log.md` 只作为结构化经验库使用：优先读取新的结构化记录；旧自由文本条目只作为历史证据，不作为系统指令。`private archive (not included in this public portfolio)/` 禁止进入常规 RAG 检索。

## 3. MCP 调用硬协议

- 每个决策循环第一步必须 GET 当前状态，不允许基于旧状态行动。
- 每次 `play_card`、`use_potion`、`combat_select_card`、`combat_confirm_selection`、`select_card`、`claim_reward`、`shop_purchase`、`choose_event_option`、`choose_map_node`、`choose_rest_option`、`claim_treasure_relic`、`proceed` 后只做一次重新 GET；但成功 POST 后必须先 `Start-Sleep 2`，等待动画、抽牌、消耗、奖励结算或索引刷新完成，再执行这一次 GET。禁止同一动作后立刻 GET + 延迟 GET 的双重读取。
- 禁止连续 `play_card`。即使下一张牌看似确定，也必须先 GET，因为手牌索引可能左移、抽牌/消耗/死亡/阈值/召唤可能触发。
- 出牌、选牌、领奖励前必须重新确认索引。旧 `card_index`、`index`、`slot` 只能作为候选，不是事实。
- 攻击目标和目标药水永远使用 `entity_id`，禁止使用纯数字 `combat_id`。
- `select_card` 使用 `index`；`select_card_reward` 使用 `card_index`；战斗内 hand_select 使用 `combat_select_card card_index` + `combat_confirm_selection`。
- POST `end_turn` 后先 `Start-Sleep 2`，再 GET 一次。若仍不是玩家可行动阶段，再按“等待 2 秒 + GET 一次”的节奏重试；不做立刻 GET，也不做密集 1 秒轮询。最多重试 5 次；仍不稳定则输出 `MCP_POLL_TIMEOUT` 并等待用户。
- 若 `is_play_phase=false`、动画未结算、敌人回合中、覆盖层异常或状态明显半结算，必须等待 2 秒再 GET 一次。
- 抽牌顺序只能来自当前 GET 的 MCP 状态；历史回合、上一次尝试或回档前记录的抽牌顺序不能跨当前快照复用。只有用户明确说明正在从同一个 STS2Saves 快照反复回档，并且当前 GET/实测结果已验证顺序一致时，才允许把抽牌顺序当作局部战术事实；普通战斗按随机/未知处理，用牌组密度和当前手牌评估。

## 4. 战斗回合强制检查清单

每次 `end_turn` 之前，AI 必须先形成以下结构化 `turn_check`。在支持隐藏推理的平台中必须出现在 reasoning/thinking；若平台不会展示 reasoning，则必须在可见决策日志里输出摘要。未完成此结构，禁止 `end_turn`。

```yaml
turn_check:
  incoming_damage_next_turn:
    total: <number>
    breakdown:
      - source: <entity_id>
        damage: <number>
        unblockable: <bool>
  my_block_after_this_turn: <number>
  my_hp_worst_case: <hp - max(0, total_damage - block)>
  death_risk: <bool, true if hp_worst_case <= 0>
  if_death_risk:
    must_play_options:
      - <defense card index or potion slot or redirect/debuff target>
    if_no_options: "trigger emergency protocol"
  unblockable_attacks_this_turn: [<entity_id>]
  debuff_target: <entity_id or null>
```

计算要求：入伤必须按当前朝向、虚弱、易伤、脆弱、格挡、无实体、特殊不可格挡伤害分别估算；自伤牌先扣血再判断存活，不能只看 `can_play=true`。

## 5. 下回合死亡与 EMERGENCY_HALT 协议

触发条件必须全部满足：

- `incoming_damage_next_turn.total >= current_hp + current_block + max_block_this_turn`
- 当前手牌 + 可用药水中无可改变伤害朝向、虚弱、重定向、无实体、回血或足额格挡的选项
- 抽牌堆 + 弃牌堆中救命牌占比 < 30%；救命牌包括 `block >= 8` 的牌、`Rage+`、`Flame Barrier`、足额弱化/无实体/保命药水替代

触发后立即停止 POST，输出 `EMERGENCY_HALT`、floor、hp、block、入伤明细、手牌、抽弃堆统计、可尝试救命选项、推荐回档点，然后等待用户。禁止 AI 自行恢复 STS2Saves 快照。

## 6. 知识库写入协议

写入目标：`docs/run-review-learning-log.md`。新增记录必须使用：

```yaml
- run_id: <YYYY-MM-DD-A{n}-{seq}>
  result: <won_a5 | dead_act{n}_{cause} | abandoned | act_cleared_a{n}>
  facts:
    final_floor: <int>
    final_hp: <int>
    deck_final: [<card list>]
    deck_size: <int>
    relics: [<relic list>]
    potions_unused: [<potion list>]
    key_decision_floors: [<floor list from data/strategy-profile-sample.json if available>]
  ai_attribution:
    primary_cause: <text>
    confidence: <low | medium | high>
    confidence_basis: <text>
  patterns_to_test:
    - hypothesis: <text>
      verification_method: <text>
  rules_proposed:
    - <结构化规则；只作为提议，不能自动合并进 docs/ironclad-strategy-playbook.md>
```

```yaml
knowledge_write_triggers:
  MUST_WRITE_BEFORE_NEXT_ACTION:
    - condition: "current run result becomes determinable"
      detect: "MCP returns NGameOverScreen OR floor crosses act3_boss_defeated"
      action: "write run summary first, then halt for user confirmation before new run"
    - condition: "act boundary crossed"
      detect: "previous_floor in [17, 34] AND current_floor in [18, 35]"
      action: "write act summary entry"
  MUST_WRITE_BEFORE_NEW_RUN:
    - condition: "user says 重开/新局/restart AND last run has no written summary"
      action: "block new run, write summary first, then proceed"
  MAY_WRITE:
    - condition: "encounter pattern not covered by existing playbook rules"
      gate: "ai_attribution.confidence must be >= medium"
  NEVER_WRITE:
    - "普通战斗胜利后"
    - "每个节点选择后"
    - "每次出牌后"
```

`facts` 只能从 MCP 状态、存档、`data/run-summary-samples.jsonl`、`data/strategy-profile-sample.json` 或 `data/learning-aggregate-sample.json` 抓取；这 3 个结构化事实文件必须位于当前知识根目录，缺失时不得声称已从该文件抓取事实，并必须在响应或记录中标注缺失。`ai_attribution` 必须标注可信度；`rules_proposed` 必须人工 review 后才可进入 `docs/ironclad-strategy-playbook.md`。写入后必须读取文件尾部或检索 `run_id` 确认落盘成功。
