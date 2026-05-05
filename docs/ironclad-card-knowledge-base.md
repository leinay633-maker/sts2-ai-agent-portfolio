# Ironclad 卡牌结构化索引

> 本文件由上一版结构化卡牌索引转换而来，保留原 tier、评价、联动、注意事项和特殊语境。上一版数据文件已归档，不参与常规 RAG。

## 使用规则

- `id` 使用英文卡名 snake_case。
- 原攻略没有逐张给出 `cost` 和 `type`，因此本文件保留为“未提供”，不臆造。
- tier 只是参考起点；实际选牌必须结合当前牌组、遗物、血量、路线和短板。
- `Playbook 关键牌执行索引` 优先服务战斗决策；若它与原 tier 文本冲突，先按当前 MCP 卡牌描述和执行索引判断，再把冲突写入 `docs/run-review-learning-log.md`。

## Playbook 关键牌执行索引

> 本节补齐 `docs/ironclad-strategy-playbook.md` 已直接依赖、但原卡牌攻略没有完整结构化字段的关键牌。`cost/type` 若不是来自原攻略，使用 `verify_with_mcp_card_text` 提醒实战前以 MCP 当前卡牌描述为准。

### `battle_trance`

```yaml
name_en: Battle Trance
name_zh: 战斗专注
role: "combat_draw"
cost: "verify_with_mcp_card_text"
type: "Skill; verify_with_mcp_card_text"
tier_context: "核心抽牌牌；在低防牌组和同一快照已验证抽牌顺序的局部规划中价值很高"
execution_rules:
  - "在 Dark Embrace/Feel No Pain/消耗链牌组中默认不打 Battle Trance；它会封锁本回合后续抽牌，切断消耗续抽。"
  - "只有没有后续抽牌需求，或当前唯一生路/确认斩杀必须立刻找牌时才可破例。"
  - "破例前必须确认手里没有 Offering、Shrug It Off、Pillage、Burning Pact、Dark Embrace 触发抽牌等更适合先用的续抽来源。"
synergies: ["Offering", "Burning Pact", "Pommel Strike", "Shrug It Off", "Hellraiser", "Fiend Fire"]
risk_notes:
  - "抽牌后手牌索引会变化；play_card 后必须 GET。"
  - "MCP GET 展示的 draw_pile 列表不保证是真实抽牌顺序，不可用来规划 Battle Trance 会抽到哪几张；普通战斗按随机/未知抽牌处理。"
  - "只有同一 STS2Saves 快照反复回档且实测抽牌一致时，才记录 Battle Trance 抽到的局部牌序；仍不可泛化。"
source_docs: ["docs/run-review-learning-log.md", "docs/ironclad-strategy-playbook.md"]
confidence: high
```

### `true_grit`

```yaml
name_en: True Grit
name_zh: 坚毅
role: "block_and_exhaust"
cost: "verify_with_mcp_card_text"
type: "Skill; verify_with_mcp_card_text"
tier_context: "A5 Act2 后经验显示，坚毅+是低防牌组少数能改善坏手牌的防御/瘦身牌"
execution_rules:
  - "若 MCP 进入 hand_select/combat_select_card，优先消耗状态牌、诅咒、低价值基础防御或临时复制牌。"
  - "不要在需要保留 Shrug It Off/Battle Trance 等后续抽防资源时盲目消耗它们。"
  - "若未升级导致消耗随机，只有在手牌没有关键牌或必须补格挡时再打。"
synergies: ["Feel No Pain", "Dark Embrace", "Second Wind", "Fiend Fire", "Ashen Strike"]
risk_notes:
  - "消耗选择后必须 combat_confirm_selection，然后 GET；不要假设手牌索引不变。"
source_docs: ["docs/run-review-learning-log.md", "docs/ironclad-strategy-playbook.md"]
confidence: high
```

### `impervious`

```yaml
name_en: Impervious
name_zh: Impervious / 高额格挡牌
role: "large_block"
cost: "verify_with_mcp_card_text"
type: "Skill; verify_with_mcp_card_text"
tier_context: "A5 失败后用于填补 Act2+ 大攻击回合的稳定防御缺口"
execution_rules:
  - "当下一轮 incoming_damage_next_turn 超过常规防御能力时，Impervious 类高额格挡优先级高于继续拿普通攻击。"
  - "若已有 Barricade/Unmovable/Calipers 类保留格挡能力，高额格挡价值上升。"
  - "如果牌组已经有足够高额格挡但缺抽牌，优先补抽牌而不是继续堆重防。"
synergies: ["Barricade", "Unmovable", "Body Slam", "Juggernaut"]
risk_notes:
  - "高费用防御牌需要能量支持；与 Offering/Bloodletting/Lantern/Happy Flower 共同评估。"
source_docs: ["docs/run-review-learning-log.md", "docs/ironclad-strategy-playbook.md"]
confidence: medium
```

### `shockwave`

```yaml
name_en: Shockwave
name_zh: 震荡波
role: "aoe_debuff"
cost: "verify_with_mcp_card_text"
type: "Skill/Power; verify_with_mcp_card_text"
tier_context: "A1 通关复盘中的关键辅助；易伤/虚弱能同时提高斩杀线和降低入伤"
execution_rules:
  - "多目标或 Boss 爆发前，若能给易伤/虚弱，通常优先于低值攻击。"
  - "面对下回合死亡风险时，虚弱收益必须纳入 turn_check 的 incoming_damage_next_turn 重算。"
  - "若敌人已有足够易伤且本回合能斩杀，则可跳过 Shockwave 直接斩杀。"
synergies: ["Perfected Strike", "Fiend Fire", "Uppercut", "Molten Fist", "Paper Frog"]
risk_notes:
  - "目标/全体效果以 MCP 卡牌描述为准；不要凭旧记忆假设。"
source_docs: ["docs/run-review-learning-log.md", "docs/ironclad-strategy-playbook.md"]
confidence: medium
```

### `brand`

```yaml
name_en: Brand
name_zh: 烙印
role: "exhaust_for_strength_or_scaling"
cost: "verify_with_mcp_card_text"
type: "Skill/Power; verify_with_mcp_card_text"
tier_context: "A5 Act1 低费多攻击路线中表现强，但低血时风险显著"
execution_rules:
  - "优先消耗伤口、腐朽、临时复制牌、低价值基础牌来换强度/收益。"
  - "如果敌人不攻击或本回合可斩杀，Brand 可用于扩大后续输出；若低血且会自伤/失去防御资源，不要贪。"
  - "A5 Act2 失败经验：Brand 不能替代稳定防御，尤其不能在无格挡回合为了启动继续压低血量。"
synergies: ["Juggle", "Feel No Pain", "Dark Embrace", "Ashen Strike", "Ornamental Fan"]
risk_notes:
  - "任何自伤/失血成本必须进入 turn_check；hp_worst_case <= 0 时禁止打，除非本回合确定斩杀。"
source_docs: ["docs/run-review-learning-log.md", "docs/ironclad-strategy-playbook.md"]
confidence: high
```

### `blood_wall`

```yaml
name_en: Blood Wall
name_zh: 血墙
role: "emergency_block_self_damage"
cost: "verify_with_mcp_card_text"
type: "Skill; verify_with_mcp_card_text"
tier_context: "A5 Act1/Act2 复盘中的应急防御；不是无脑可打牌"
execution_rules:
  - "敌人不攻击、净格挡收益低、或自伤会进入死亡风险时，不打。"
  - "若可与易伤降低、灵体或格挡翻倍效果配合，重新计算实际收益后再打。"
  - "同一快照已验证抽牌顺序时，Blood Wall 是否留到 Rocket 大攻击回合比本回合多挡几点更重要；普通战斗不能假设未来抽牌顺序固定。"
synergies: ["Unmovable", "Barricade", "Intangible", "Feel No Pain"]
risk_notes:
  - "低血时即使 MCP 显示 can_play=true，也必须按 self-damage lock 判断。"
source_docs: ["docs/run-review-learning-log.md", "docs/ironclad-strategy-playbook.md"]
confidence: high
```

### `offering`

```yaml
name_en: Offering
name_zh: 祭品
role: "burst_energy_draw_self_damage"
cost: "verify_with_mcp_card_text"
type: "Skill; verify_with_mcp_card_text"
tier_context: "强启动牌；A3/A5 Boss 奖励中多次优先选择"
execution_rules:
  - "只有能转换成斩杀、防御、关键启动或保命时才打。"
  - "低血时先计算自伤后的 hp_worst_case；不能只看 can_play。"
  - "若 Battle Trance 也在手，通常先用 Offering，再考虑 Battle Trance，避免封抽。"
synergies: ["Battle Trance", "Fiend Fire", "Dark Embrace", "Bloodletting", "Ashen Strike"]
risk_notes:
  - "A5 Act2 3-6 血时 Offering 等于自杀，除非本回合已经确定斩杀。"
source_docs: ["docs/run-review-learning-log.md", "docs/ironclad-card-knowledge-base.md"]
confidence: high
```

## 快速索引

| id | 中文名 | 英文名 | tier | 关键词/联动 |
|---|---|---|---|---|
| `offering` | 献祭 | Offering | S | 燃血、自伤、抽牌、能量 |
| `hellraiser` | 召唤地狱 | Hellraiser | S | 柄击、无限、无限连 |
| `feed` | 进食 | Feed | S | 未提供 |
| `break` | 破击 | Break | S | 易伤 |
| `pommel_strike` | 柄击 | Pommel Strike | A | 召唤地狱、抽牌 |
| `shrug_it_off` | 摆脱困境 | Shrug it Off | A | 格挡、抽牌 |
| `bloodletting` | 放血 | Bloodletting | A | 能量 |
| `fiend_fire` | 恶魔之火 | Fiend Fire | A | 抽牌 |
| `stomp` | 跺脚 | Stomp | A | AOE |
| `armaments` | 武装 | Armaments | A | 未提供 |
| `demonic_shield` | 恶魔之盾 | Demonic Shield | A | 消耗、格挡 |
| `whirlwind` | 旋风 | Whirlwind | B | 力量、AOE、能量 |
| `forgotten_ritual` | 被遗忘的仪式 | Forgotten Ritual | B | 消耗、抽牌、能量 |
| `rupture` | 破裂 | Rupture | B | 力量、自伤 |
| `fight_me` | 来打我啊 | Fight Me! | B | 力量 |
| `uppercut` | 上勾拳 | Uppercut | B | 易伤、虚弱 |
| `burning_pact` | 燃烧契约 | Burning Pact | B | 消耗、抽牌、能量 |
| `pacts_end` | 契约终结 | Pact's End | B | 消耗、AOE |
| `mangle` | 撕裂 | Mangle | B | 防御 |
| `howl_from_beyond` | 来自彼岸的咆哮 | Howl from Beyond | B | 消耗 |
| `unmovable` | 屹立不动 | Unmovable | B | 砸压、Body Slam、路障、Barricade、格挡、防御 |
| `inferno` | 炼狱 | Inferno | C | 自伤、格挡 |
| `body_slam` | 砸压 | Body Slam | C | 格挡 |
| `not_yet` | 还未结束 | Not Yet | C | 能量 |
| `pyre` | 火葬堆 | Pyre | C | 未提供 |
| `primal_force` | 原始之力 | Primal Force | C | 未提供 |
| `barricade` | 路障 | Barricade | C | 格挡 |
| `juggernaut` | 主宰 | Juggernaut | C | 路障、格挡 |
| `feel_no_pain` | 麻木 | Feel No Pain | C | 无限、无限连、消耗、格挡 |
| `cruelty` | 残忍 | Cruelty | C | 易伤 |
| `rampage` | 蹂躏 | Rampage | C | 未提供 |
| `inflame` | 燃血 | Inflame | C | 力量 |
| `spite` | 怨恨 | Spite | C | 抽牌 |
| `crimson_mantle` | 赤色斗篷 | Crimson Mantle | C | 自伤、格挡 |
| `second_wind` | 二次呼吸 | Second Wind | C | 无限 |
| `stampede` | 踩踏 | Stampede | C | 召唤地狱 |
| `conflagration` | 大火 | Conflagration | C | AOE |
| `expect_a_fight` | 备战 | Expect a Fight | C | 能量 |
| `flame_barrier` | 烈焰屏障 | Flame Barrier | C | 格挡 |
| `stoke` | 煽火 | Stoke | C | 未提供 |
| `one_two_punch` | 组合拳 | One-Two Punch | C | 未提供 |
| `molten_fist` | 熔岩拳 | Molten Fist | C | 易伤 |
| `infernal_blade` | 炼狱之刃 | Infernal Blade | C | 能量 |
| `rage` | 暴怒 | Rage | C | 格挡、抽牌 |
| `unrelenting` | 无情 | Unrelenting | C | 未提供 |
| `grapple` | 抓握 | Grapple | C | 不痛、格挡 |
| `anger` | 怒火 | Anger | C | 未提供 |
| `hemokinesis` | 血液操控 | Hemokinesis | C | 自伤 |
| `setup_strike` | 蓄势打击 | Setup Strike | C | 召唤地狱、跺脚、怒火、完美打击、打击、低费攻击 |
| `twin_strike` | 双击 | Twin Strike | C | 力量 |
| `perfected_strike` | 完美打击 | Perfected Strike | C | 召唤地狱 |
| `colossus` | 巨像 | Colossus | C | 格挡、易伤 |
| `taunt` | 挑衅 | Taunt | C | 格挡、易伤 |
| `breakthrough` | 突破 | Breakthrough | C | 自伤、AOE |
| `ashen_strike` | 灰烬打击 | Ashen Strike | C | 无限、消耗 |
| `dark_embrace` | 黑暗拥抱 | Dark Embrace | D | 消耗 |
| `pillage` | 掠夺 | Pillage | D | 未提供 |
| `sword_boomerang` | 飞剑回旋 | Sword Boomerang | D | 力量 |
| `blood_wall` | 血墙 | Blood Wall | D | 自伤、格挡 |
| `cinder` | 余烬 | Cinder | D | 消耗 |
| `tank` | 坦克 | Tank | D | 未提供 |
| `thunderclap` | 雷鸣 | Thunderclap | D | AOE、易伤 |
| `stone_armor` | 石甲 | Stone Armor | D | 格挡 |
| `iron_wave` | 铁波 | Iron Wave | D | 未提供 |
| `bully` | 欺凌 | Bully | D | 易伤、抽牌 |
| `vicious` | 恶毒 | Vicious | D | 易伤、抽牌、能量 |
| `havoc` | 浩劫 | Havoc | F | 消耗 |

## S 级

### 献祭 / Offering

- id: `offering`
- raw_heading: 献祭（Offering）
- base_tier: S (无脑拿)
- cost: 未提供
- type: 未提供
- aliases_zh: 献祭
- synergy_terms_zh: 燃血、自伤、抽牌、能量
- special_context_zh: 在这游戏里，"额外抽牌 + 额外能量"几乎是最有价值的事情，而自伤在铁甲身上甚至常常算不上真正的代价（有"燃血"遗物配合）。、除非你的牌组当下急需别的东西，否则永远拿。
- cautions_zh: 除非你的牌组当下急需别的东西，否则永远拿。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

铁甲牌池里**最强的一张牌**。在这游戏里，"额外抽牌 + 额外能量"几乎是最有价值的事情，而自伤在铁甲身上甚至常常算不上真正的代价（有"燃血"遗物配合）。除非你的牌组当下急需别的东西，否则永远拿。

### 召唤地狱 / Hellraiser

- id: `hellraiser`
- raw_heading: 召唤地狱（Hellraiser）
- base_tier: S (无脑拿)
- cost: 未提供
- type: 未提供
- aliases_zh: 召唤地狱
- synergy_terms_zh: 柄击、无限、无限连
- special_context_zh: 配合几张"柄击"就能轻松启动**无限循环**。、无论是公平节奏还是无限连招套路，都能白嫖一大堆伤害。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

配合几张"柄击"就能轻松启动**无限循环**。无论是公平节奏还是无限连招套路，都能白嫖一大堆伤害。极强。

### 进食 / Feed

- id: `feed`
- raw_heading: 进食（Feed）
- base_tier: S (无脑拿)
- cost: 未提供
- type: 未提供
- aliases_zh: 进食
- synergy_terms_zh: 未提供
- special_context_zh: 这张牌的价值在整个 Run 中持续累积，绝大多数牌组都该拿。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

拿到越早越赚。哪怕只触发几次最大生命提升，就足以回本。这张牌的价值在整个 Run 中持续累积，绝大多数牌组都该拿。**越晚拿越亏**，早期看到必拿。

### 破击 / Break

- id: `break`
- raw_heading: 破击（Break）
- base_tier: S (无脑拿)
- cost: 未提供
- type: 未提供
- aliases_zh: 破击
- synergy_terms_zh: 易伤
- special_context_zh: 1 费 20 伤害 + 5 层易伤，**性价比离谱**。、任何牌组都会很乐意收一张。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

1 费 20 伤害 + 5 层易伤，**性价比离谱**。任何牌组都会很乐意收一张。

## A 级

### 柄击 / Pommel Strike

- id: `pommel_strike`
- raw_heading: 柄击（Pommel Strike）
- base_tier: A (核心强卡)
- cost: 未提供
- type: 未提供
- aliases_zh: 柄击
- synergy_terms_zh: 召唤地狱、抽牌
- special_context_zh: "伤害 + 抽牌"几乎是所有牌组都想要的属性，加上和**召唤地狱**的天然联动，价值进一步拉高。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

"伤害 + 抽牌"几乎是所有牌组都想要的属性，加上和**召唤地狱**的天然联动，价值进一步拉高。

### 摆脱困境 / Shrug it Off

- id: `shrug_it_off`
- raw_heading: 摆脱困境（Shrug it Off）
- base_tier: A (核心强卡)
- cost: 未提供
- type: 未提供
- aliases_zh: 摆脱困境
- synergy_terms_zh: 格挡、抽牌
- special_context_zh: 高效格挡 + 抽牌，**任何流派都通用**。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

高效格挡 + 抽牌，**任何流派都通用**。

### 放血 / Bloodletting

- id: `bloodletting`
- raw_heading: 放血（Bloodletting）
- base_tier: A (核心强卡)
- cost: 未提供
- type: 未提供
- aliases_zh: 放血
- synergy_terms_zh: 能量
- special_context_zh: 几乎没有牌组会拒绝它。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

用血换能量的兑换比极其划算，关键是**普通卡（Common）稀有度**意味着 Run 里很容易出现。几乎没有牌组会拒绝它。

### 恶魔之火 / Fiend Fire

- id: `fiend_fire`
- raw_heading: 恶魔之火（Fiend Fire）
- base_tier: A (核心强卡)
- cost: 未提供
- type: 未提供
- aliases_zh: 恶魔之火
- synergy_terms_zh: 抽牌
- special_context_zh: 配合一定的抽牌量，能打出**爆炸级伤害**，顺便还能精简牌库。、如果你在意精简，这是双赢。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

配合一定的抽牌量，能打出**爆炸级伤害**，顺便还能精简牌库。如果你在意精简，这是双赢。

### 跺脚 / Stomp

- id: `stomp`
- raw_heading: 跺脚 / 踩踏（Stomp）
- base_tier: A (核心强卡)
- cost: 未提供
- type: 未提供
- aliases_zh: 跺脚、踩踏
- synergy_terms_zh: AOE
- special_context_zh: 1 费甚至 0 费的 AOE，**绝大多数牌组都会很高兴有一张**。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

1 费甚至 0 费的 AOE，**绝大多数牌组都会很高兴有一张**。

### 武装 / Armaments

- id: `armaments`
- raw_heading: 武装（Armaments）
- base_tier: A (核心强卡)
- cost: 未提供
- type: 未提供
- aliases_zh: 武装
- synergy_terms_zh: 未提供
- special_context_zh: 未提供
- cautions_zh: 基础形态偏弱，但只要不是顺风局或拿了"神化"，**升级版**的"武装"就足以补回你在篝火里没升级到的牌。
- upgrade_notes_zh: 基础形态偏弱，但只要不是顺风局或拿了"神化"，**升级版**的"武装"就足以补回你在篝火里没升级到的牌。
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

基础形态偏弱，但只要不是顺风局或拿了"神化"，**升级版**的"武装"就足以补回你在篝火里没升级到的牌。是个隐藏 MVP。

### 恶魔之盾 / Demonic Shield

- id: `demonic_shield`
- raw_heading: 恶魔之盾（Demonic Shield）—— 仅多人模式
- base_tier: A (核心强卡)
- cost: 未提供
- type: 未提供
- aliases_zh: 恶魔之盾
- synergy_terms_zh: 消耗、格挡
- special_context_zh: 仅多人模式、0 费 + 1 血，给队友最多 20 格挡，**离谱性价比**。、要求你自己格挡值高，且这张是消耗牌。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

0 费 + 1 血，给队友最多 20 格挡，**离谱性价比**。要求你自己格挡值高，且这张是消耗牌。

## B 级

### 旋风 / Whirlwind

- id: `whirlwind`
- raw_heading: 旋风（Whirlwind）
- base_tier: B (流派支柱)
- cost: 未提供
- type: 未提供
- aliases_zh: 旋风
- synergy_terms_zh: 力量、AOE、能量
- special_context_zh: **力量流、加能量、缺 AOE** 三种场景下都很出色。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

经典高效。**力量流、加能量、缺 AOE** 三种场景下都很出色。

### 被遗忘的仪式 / Forgotten Ritual

- id: `forgotten_ritual`
- raw_heading: 被遗忘的仪式（Forgotten Ritual）
- base_tier: B (流派支柱)
- cost: 未提供
- type: 未提供
- aliases_zh: 被遗忘的仪式
- synergy_terms_zh: 消耗、抽牌、能量
- special_context_zh: 需要抽牌 + 消耗牌的搭配才能用满，但**门槛不高，上限极高**。
- cautions_zh: 需要抽牌 + 消耗牌的搭配才能用满，但**门槛不高，上限极高**。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

能量牌总是非常强，这张也不例外。需要抽牌 + 消耗牌的搭配才能用满，但**门槛不高，上限极高**。

### 破裂 / Rupture

- id: `rupture`
- raw_heading: 破裂（Rupture）
- base_tier: B (流派支柱)
- cost: 未提供
- type: 未提供
- aliases_zh: 破裂
- synergy_terms_zh: 力量、自伤
- special_context_zh: 新版本加入大量自伤牌后，这张是**力量堆叠的高效来源**，配合自伤牌组堆速度飞快。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

新版本加入大量自伤牌后，这张是**力量堆叠的高效来源**，配合自伤牌组堆速度飞快。

### 来打我啊 / Fight Me!

- id: `fight_me`
- raw_heading: 来打我啊（Fight Me!）
- base_tier: B (流派支柱)
- cost: 未提供
- type: 未提供
- aliases_zh: 来打我啊
- synergy_terms_zh: 力量
- special_context_zh: 给敌人加力量这点不太理想，但你赚的远比对面赚的多。
- cautions_zh: 给敌人加力量这点不太理想，但你赚的远比对面赚的多。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

给敌人加力量这点不太理想，但你赚的远比对面赚的多。哪怕对手是一只快死的小怪，也是**纯粹的价值**。

### 上勾拳 / Uppercut

- id: `uppercut`
- raw_heading: 上勾拳（Uppercut）
- base_tier: B (流派支柱)
- cost: 未提供
- type: 未提供
- aliases_zh: 上勾拳
- synergy_terms_zh: 易伤、虚弱
- special_context_zh: 伤害 + 两种 debuff（虚弱 + 易伤），**非常全面**，缺点只是 2 费略紧张。
- cautions_zh: 伤害 + 两种 debuff（虚弱 + 易伤），**非常全面**，缺点只是 2 费略紧张。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

伤害 + 两种 debuff（虚弱 + 易伤），**非常全面**，缺点只是 2 费略紧张。

### 燃烧契约 / Burning Pact

- id: `burning_pact`
- raw_heading: 燃烧契约（Burning Pact）
- base_tier: B (流派支柱)
- cost: 未提供
- type: 未提供
- aliases_zh: 燃烧契约
- synergy_terms_zh: 消耗、抽牌、能量
- special_context_zh: 高效抽牌，顺带消耗掉手里最弱的牌。
- cautions_zh: 要求你**有富余能量**，但只要满足，价值很高。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

高效抽牌，顺带消耗掉手里最弱的牌。要求你**有富余能量**，但只要满足，价值很高。

### 契约终结 / Pact's End

- id: `pacts_end`
- raw_heading: 契约终结（Pact's End）
- base_tier: B (流派支柱)
- cost: 未提供
- type: 未提供
- aliases_zh: 契约终结
- synergy_terms_zh: 消耗、AOE
- special_context_zh: 0 费 17 伤害（消耗堆要够 3 张），**对 AOE 终结局非常顶**。、需要少许铺垫，但门槛低。
- cautions_zh: 需要少许铺垫，但门槛低。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

0 费 17 伤害（消耗堆要够 3 张），**对 AOE 终结局非常顶**。需要少许铺垫，但门槛低。

### 撕裂 / Mangle

- id: `mangle`
- raw_heading: 撕裂 / 重击（Mangle）
- base_tier: B (流派支柱)
- cost: 未提供
- type: 未提供
- aliases_zh: 撕裂、重击
- synergy_terms_zh: 防御
- special_context_zh: 未提供
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

偏防御版的"猛击棒"，3 费 20 伤害还能打断对方一次攻击。**性价比可以接受**，且具有功能性。

### 来自彼岸的咆哮 / Howl from Beyond

- id: `howl_from_beyond`
- raw_heading: 来自彼岸的咆哮（Howl from Beyond）
- base_tier: B (流派支柱)
- cost: 未提供
- type: 未提供
- aliases_zh: 来自彼岸的咆哮
- synergy_terms_zh: 消耗
- special_context_zh: 在消耗流牌组里最强，但**即便没有消耗联动也不至于不能玩**——这点比很多"消耗触发"类卡牌都要好。
- cautions_zh: 在消耗流牌组里最强，但**即便没有消耗联动也不至于不能玩**——这点比很多"消耗触发"类卡牌都要好。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

在消耗流牌组里最强，但**即便没有消耗联动也不至于不能玩**——这点比很多"消耗触发"类卡牌都要好。

### 屹立不动 / Unmovable

- id: `unmovable`
- raw_heading: 屹立不动 / 不可动摇（Unmovable）
- base_tier: B (流派支柱)
- cost: 未提供
- type: 未提供
- aliases_zh: 屹立不动、不可动摇
- synergy_terms_zh: 砸压、Body Slam、路障、Barricade、格挡、防御
- special_context_zh: 每回合首张格挡牌翻倍。、在重防御牌组、缺好格挡牌组、**砸压（Body Slam）/ 路障（Barricade）** 套路里都很顶。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

每回合首张格挡牌翻倍。在重防御牌组、缺好格挡牌组、**砸压（Body Slam）/ 路障（Barricade）** 套路里都很顶。

## C 级

### 炼狱 / Inferno

- id: `inferno`
- raw_heading: 炼狱（Inferno）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 炼狱
- synergy_terms_zh: 自伤、格挡
- special_context_zh: 多怪战里很强，吃自伤牌堆叠。、**搭配稳定的格挡**，是个不错的 build-around。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

多怪战里很强，吃自伤牌堆叠。**搭配稳定的格挡**，是个不错的 build-around。

### 砸压 / Body Slam

- id: `body_slam`
- raw_heading: 砸压（Body Slam）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 砸压
- synergy_terms_zh: 格挡
- special_context_zh: 格挡多就是大伤害。、哪怕格挡量一般，**升级版**也还能打。
- cautions_zh: 哪怕格挡量一般，**升级版**也还能打。
- upgrade_notes_zh: 哪怕格挡量一般，**升级版**也还能打。
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

格挡多就是大伤害。哪怕格挡量一般，**升级版**也还能打。

### 还未结束 / Not Yet

- id: `not_yet`
- raw_heading: 还未结束 / 未到时候（Not Yet）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 还未结束、未到时候
- synergy_terms_zh: 能量
- special_context_zh: 能回血很香，但需要能量和合适的牌组才能用好——**高风险高回报**。
- cautions_zh: 能回血很香，但需要能量和合适的牌组才能用好——**高风险高回报**。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

铁甲最新加入的牌。能回血很香，但需要能量和合适的牌组才能用好——**高风险高回报**。

### 火葬堆 / Pyre

- id: `pyre`
- raw_heading: 火葬堆（Pyre）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 火葬堆
- synergy_terms_zh: 未提供
- special_context_zh: 基础版 2 费当回合不出效果太残酷（高升华尤其难受），但**特定套路里可以很强**。
- cautions_zh: 基础版 2 费当回合不出效果太残酷（高升华尤其难受），但**特定套路里可以很强**。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

基础版 2 费当回合不出效果太残酷（高升华尤其难受），但**特定套路里可以很强**。

### 原始之力 / Primal Force

- id: `primal_force`
- raw_heading: 原始之力（Primal Force）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 原始之力
- synergy_terms_zh: 未提供
- special_context_zh: 把烂攻击变好攻击，前期开荒很舒服，**但后期 scale 不太行**。
- cautions_zh: 把烂攻击变好攻击，前期开荒很舒服，**但后期 scale 不太行**。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

把烂攻击变好攻击，前期开荒很舒服，**但后期 scale 不太行**。

### 路障 / Barricade

- id: `barricade`
- raw_heading: 路障（Barricade）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 路障
- synergy_terms_zh: 格挡
- special_context_zh: 要有足够高效的格挡牌才能发挥。、**配合得好极强，配不上就是废牌**。
- cautions_zh: **配合得好极强，配不上就是废牌**。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

要有足够高效的格挡牌才能发挥。**配合得好极强，配不上就是废牌**。

### 主宰 / Juggernaut

- id: `juggernaut`
- raw_heading: 主宰（Juggernaut）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 主宰
- synergy_terms_zh: 路障、格挡
- special_context_zh: 和路障类似要堆格挡，但**最差情况也不算太差**——不会让你后悔拿了它。
- cautions_zh: 和路障类似要堆格挡，但**最差情况也不算太差**——不会让你后悔拿了它。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

和路障类似要堆格挡，但**最差情况也不算太差**——不会让你后悔拿了它。

### 麻木 / Feel No Pain

- id: `feel_no_pain`
- raw_heading: 麻木 / 不痛（Feel No Pain）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 麻木、不痛
- synergy_terms_zh: 无限、无限连、消耗、格挡
- special_context_zh: 需要大量消耗牌才能值回票价。、一旦堆到位，**这是铁甲除"无限连"以外最稳定的格挡引擎**。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

需要大量消耗牌才能值回票价。一旦堆到位，**这是铁甲除"无限连"以外最稳定的格挡引擎**。

### 残忍 / Cruelty

- id: `cruelty`
- raw_heading: 残忍（Cruelty）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 残忍
- synergy_terms_zh: 易伤
- special_context_zh: 没多个易伤来源就别拿，**有了就很强**。
- cautions_zh: 没多个易伤来源就别拿，**有了就很强**。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

没多个易伤来源就别拿，**有了就很强**。

### 蹂躏 / Rampage

- id: `rampage`
- raw_heading: 蹂躏（Rampage）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 蹂躏
- synergy_terms_zh: 未提供
- special_context_zh: 前期清场神器，但牌库越大越弱。
- cautions_zh: 前期清场神器，但牌库越大越弱。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

前期清场神器，但牌库越大越弱。**速通早期战的可靠手段**。

### 燃血 / Inflame

- id: `inflame`
- raw_heading: 燃血（Inflame）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 燃血
- synergy_terms_zh: 力量
- special_context_zh: 1 费换 2~3 力量在通用牌组里很划算，多攻击牌组里更香。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

1 费换 2~3 力量在通用牌组里很划算，多攻击牌组里更香。**作者认为这张被略微高估**。

### 怨恨 / Spite

- id: `spite`
- raw_heading: 怨恨（Spite）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 怨恨
- synergy_terms_zh: 抽牌
- special_context_zh: 未提供
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

0 费 6 伤害不算差，**有很多 0 费攻击的联动**，再加上潜在抽牌，整体强度不低。

### 赤色斗篷 / Crimson Mantle

- id: `crimson_mantle`
- raw_heading: 赤色斗篷（Crimson Mantle）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 赤色斗篷
- synergy_terms_zh: 自伤、格挡
- special_context_zh: 自伤流非常强，普通牌组里**1 自伤换 8 格挡**也是好买卖。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

自伤流非常强，普通牌组里**1 自伤换 8 格挡**也是好买卖。

### 二次呼吸 / Second Wind

- id: `second_wind`
- raw_heading: 二次呼吸 / 重整旗鼓（Second Wind）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 二次呼吸、重整旗鼓
- synergy_terms_zh: 无限
- special_context_zh: **精简牌库进入无限的关键牌**，要点铺垫但即便普通牌组也还行。
- cautions_zh: **精简牌库进入无限的关键牌**，要点铺垫但即便普通牌组也还行。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

**精简牌库进入无限的关键牌**，要点铺垫但即便普通牌组也还行。

### 踩踏 / Stampede

- id: `stampede`
- raw_heading: 踩踏 / 蹂躏（Stampede）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 踩踏、蹂躏
- synergy_terms_zh: 召唤地狱
- special_context_zh: 未提供
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

和召唤地狱类似，让攻击牌**白嫖出场**就是非常顶。可以围绕它构筑爆发。

### 大火 / Conflagration

- id: `conflagration`
- raw_heading: 大火 / 燃烧（Conflagration）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 大火、燃烧
- synergy_terms_zh: AOE
- special_context_zh: 1 费高 AOE 潜力，**收尾战非常好用**。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

1 费高 AOE 潜力，**收尾战非常好用**。

### 备战 / Expect a Fight

- id: `expect_a_fight`
- raw_heading: 备战（Expect a Fight）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 备战
- synergy_terms_zh: 能量
- special_context_zh: 未提供
- cautions_zh: 未提供
- upgrade_notes_zh: 比其他能量牌**多一点点门槛**，升级后非常稳定。
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

比其他能量牌**多一点点门槛**，升级后非常稳定。

### 烈焰屏障 / Flame Barrier

- id: `flame_barrier`
- raw_heading: 烈焰屏障（Flame Barrier）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 烈焰屏障
- synergy_terms_zh: 格挡
- special_context_zh: 格挡效率不算高，但反伤在某些战斗里**非常关键**（如二章蜂精英、蟑螂群等）。
- cautions_zh: 格挡效率不算高，但反伤在某些战斗里**非常关键**（如二章蜂精英、蟑螂群等）。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

格挡效率不算高，但反伤在某些战斗里**非常关键**（如二章蜂精英、蟑螂群等）。

### 煽火 / Stoke

- id: `stoke`
- raw_heading: 煽火（Stoke）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 煽火
- synergy_terms_zh: 未提供
- special_context_zh: 未提供
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

组合启动器，**起手很顶，普通局面下中庸**。

### 组合拳 / One-Two Punch

- id: `one_two_punch`
- raw_heading: 组合拳（One-Two Punch）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 组合拳
- synergy_terms_zh: 未提供
- special_context_zh: 合适的牌组里效率高，普通牌组里只是"还行"，**整体仍可玩**。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

合适的牌组里效率高，普通牌组里只是"还行"，**整体仍可玩**。

### 熔岩拳 / Molten Fist

- id: `molten_fist`
- raw_heading: 熔岩拳（Molten Fist）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 熔岩拳
- synergy_terms_zh: 易伤
- special_context_zh: 易伤翻倍 = 大量额外伤害，**易伤流和"统御（Dominate）" build 里非常顶**。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

易伤翻倍 = 大量额外伤害，**易伤流和"统御（Dominate）" build 里非常顶**。

### 炼狱之刃 / Infernal Blade

- id: `infernal_blade`
- raw_heading: 炼狱之刃（Infernal Blade）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 炼狱之刃
- synergy_terms_zh: 能量
- special_context_zh: 未提供
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

作者承认自己**比应该的更喜欢这张**——大概率能量持平甚至超出。

### 暴怒 / Rage

- id: `rage`
- raw_heading: 暴怒（Rage）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 暴怒
- synergy_terms_zh: 格挡、抽牌
- special_context_zh: 攻击多的牌组里很顶，**配合抽牌可以叠出大量格挡**。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

攻击多的牌组里很顶，**配合抽牌可以叠出大量格挡**。

### 无情 / Unrelenting

- id: `unrelenting`
- raw_heading: 无情（Unrelenting）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 无情
- synergy_terms_zh: 未提供
- special_context_zh: **贵牌很多的牌组**里非常好。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

**贵牌很多的牌组**里非常好。即便降到 1 费攻击也不是惨案。

### 抓握 / Grapple

- id: `grapple`
- raw_heading: 抓握（Grapple）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 抓握
- synergy_terms_zh: 不痛、格挡
- special_context_zh: 有多次格挡触发（如不痛）才能起飞，**普通牌组里就是攻防兼备**。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

有多次格挡触发（如不痛）才能起飞，**普通牌组里就是攻防兼备**。

### 怒火 / Anger

- id: `anger`
- raw_heading: 怒火（Anger）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 怒火
- synergy_terms_zh: 未提供
- special_context_zh: 一章里如果你没什么好攻击，可以当过渡铺垫精英 / Boss。、**进入二三章后就过气**，除非很特殊的牌组。
- cautions_zh: **进入二三章后就过气**，除非很特殊的牌组。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

一章里如果你没什么好攻击，可以当过渡铺垫精英 / Boss。**进入二三章后就过气**，除非很特殊的牌组。

### 血液操控 / Hemokinesis

- id: `hemokinesis`
- raw_heading: 血液操控（Hemokinesis）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 血液操控
- synergy_terms_zh: 自伤
- special_context_zh: 1 费 14 伤害仍然划算，**前期或自伤流最佳**。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

作者认为不该被砍。1 费 14 伤害仍然划算，**前期或自伤流最佳**。

### 蓄势打击 / Setup Strike

- id: `setup_strike`
- raw_heading: 蓄势打击 / 架势打击（Setup Strike）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 蓄势打击、架势打击
- synergy_terms_zh: 召唤地狱、跺脚、怒火、完美打击、打击、低费攻击
- special_context_zh: 配合大量低费攻击（怒火、跺脚等）很强，**且名字带 "Strike" 能联动召唤地狱、完美打击**。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

配合大量低费攻击（怒火、跺脚等）很强，**且名字带 "Strike" 能联动召唤地狱、完美打击**。

### 双击 / Twin Strike

- id: `twin_strike`
- raw_heading: 双击（Twin Strike）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 双击
- synergy_terms_zh: 力量
- special_context_zh: 普通牌组里 OK，**配合力量堆叠会很强**。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

普通牌组里 OK，**配合力量堆叠会很强**。

### 完美打击 / Perfected Strike

- id: `perfected_strike`
- raw_heading: 完美打击（Perfected Strike）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 完美打击
- synergy_terms_zh: 召唤地狱
- special_context_zh: 作者一般不推荐围绕它构筑，但 2 费打 20+ 在一章很有用，**公平派召唤地狱牌组里后期仍有价值**。
- cautions_zh: 作者一般不推荐围绕它构筑，但 2 费打 20+ 在一章很有用，**公平派召唤地狱牌组里后期仍有价值**。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

作者一般不推荐围绕它构筑，但 2 费打 20+ 在一章很有用，**公平派召唤地狱牌组里后期仍有价值**。

### 巨像 / Colossus

- id: `colossus`
- raw_heading: 巨像（Colossus）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 巨像
- synergy_terms_zh: 格挡、易伤
- special_context_zh: 易伤流里超高效格挡，**普通局也比 Defend 略好**。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

易伤流里超高效格挡，**普通局也比 Defend 略好**。

### 挑衅 / Taunt

- id: `taunt`
- raw_heading: 挑衅 / 嘲讽（Taunt）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 挑衅、嘲讽
- synergy_terms_zh: 格挡、易伤
- special_context_zh: 格挡 + 1 易伤，**性价比合格**。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

格挡 + 1 易伤，**性价比合格**。

### 突破 / Breakthrough

- id: `breakthrough`
- raw_heading: 突破（Breakthrough）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 突破
- synergy_terms_zh: 自伤、AOE
- special_context_zh: 1 费 + 1 自伤 = 9 AOE，**前期非常划算**。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

1 费 + 1 自伤 = 9 AOE，**前期非常划算**。

### 灰烬打击 / Ashen Strike

- id: `ashen_strike`
- raw_heading: 灰烬打击（Ashen Strike）
- base_tier: C (看局势取舍)
- cost: 未提供
- type: 未提供
- aliases_zh: 灰烬打击
- synergy_terms_zh: 无限、消耗
- special_context_zh: 设计上很拧巴：消耗越多越强，但消耗够多通常就走无限了。、需要**"消耗一些但不无限"的中间地带**才好用。
- cautions_zh: 设计上很拧巴：消耗越多越强，但消耗够多通常就走无限了。、需要**"消耗一些但不无限"的中间地带**才好用。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

设计上很拧巴：消耗越多越强，但消耗够多通常就走无限了。需要**"消耗一些但不无限"的中间地带**才好用。

## D 级

### 黑暗拥抱 / Dark Embrace

- id: `dark_embrace`
- raw_heading: 黑暗拥抱（Dark Embrace）
- base_tier: D (特定情境)
- cost: 未提供
- type: 未提供
- aliases_zh: 黑暗拥抱
- synergy_terms_zh: 消耗
- special_context_zh: **就连消耗流也不一定想要它**，更绝不会想要第二张。
- cautions_zh: 下限极低，上限很高。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

**就连消耗流也不一定想要它**，更绝不会想要第二张。下限极低，上限很高。

### 掠夺 / Pillage

- id: `pillage`
- raw_heading: 掠夺（Pillage）
- base_tier: D (特定情境)
- cost: 未提供
- type: 未提供
- aliases_zh: 掠夺
- synergy_terms_zh: 未提供
- special_context_zh: 特定牌组找特定牌时还行，**单看不会很激动**。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

特定牌组找特定牌时还行，**单看不会很激动**。

### 飞剑回旋 / Sword Boomerang

- id: `sword_boomerang`
- raw_heading: 飞剑回旋 / 剑刃回旋（Sword Boomerang）
- base_tier: D (特定情境)
- cost: 未提供
- type: 未提供
- aliases_zh: 飞剑回旋、剑刃回旋
- synergy_terms_zh: 力量
- special_context_zh: 能堆力量就还行，**没力量就很平庸**。
- cautions_zh: 能堆力量就还行，**没力量就很平庸**。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

能堆力量就还行，**没力量就很平庸**。

### 血墙 / Blood Wall

- id: `blood_wall`
- raw_heading: 血墙（Blood Wall）
- base_tier: D (特定情境)
- cost: 未提供
- type: 未提供
- aliases_zh: 血墙
- synergy_terms_zh: 自伤、格挡
- special_context_zh: 2 费 16 格挡 + 2 自伤，**性价比一般**，自伤流里勉强可以。
- cautions_zh: 2 费 16 格挡 + 2 自伤，**性价比一般**，自伤流里勉强可以。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

2 费 16 格挡 + 2 自伤，**性价比一般**，自伤流里勉强可以。

### 余烬 / Cinder

- id: `cinder`
- raw_heading: 余烬（Cinder）
- base_tier: D (特定情境)
- cost: 未提供
- type: 未提供
- aliases_zh: 余烬
- synergy_terms_zh: 消耗
- special_context_zh: **随机消耗手牌很难受**，伤害本身在缺主力攻击时还行。
- cautions_zh: **随机消耗手牌很难受**，伤害本身在缺主力攻击时还行。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

**随机消耗手牌很难受**，伤害本身在缺主力攻击时还行。

### 坦克 / Tank

- id: `tank`
- raw_heading: 坦克（Tank）
- base_tier: D (特定情境)
- cost: 未提供
- type: 未提供
- aliases_zh: 坦克
- synergy_terms_zh: 未提供
- special_context_zh: **特定牌组里很顶，否则接近不能玩**。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

**特定牌组里很顶，否则接近不能玩**。

### 雷鸣 / Thunderclap

- id: `thunderclap`
- raw_heading: 雷鸣（Thunderclap）
- base_tier: D (特定情境)
- cost: 未提供
- type: 未提供
- aliases_zh: 雷鸣
- synergy_terms_zh: AOE、易伤
- special_context_zh: 急需 AOE 或易伤时的兜底，**通常不会让你高兴**。
- cautions_zh: 未提供
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

急需 AOE 或易伤时的兜底，**通常不会让你高兴**。

### 石甲 / Stone Armor

- id: `stone_armor`
- raw_heading: 石甲（Stone Armor）
- base_tier: D (特定情境)
- cost: 未提供
- type: 未提供
- aliases_zh: 石甲
- synergy_terms_zh: 格挡
- special_context_zh: 1 费 10 格挡的确便宜，但仅此而已。
- cautions_zh: 被低估了，但确实**不算好**。、1 费 10 格挡的确便宜，但仅此而已。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

被低估了，但确实**不算好**。1 费 10 格挡的确便宜，但仅此而已。

### 铁波 / Iron Wave

- id: `iron_wave`
- raw_heading: 铁波（Iron Wave）
- base_tier: D (特定情境)
- cost: 未提供
- type: 未提供
- aliases_zh: 铁波
- synergy_terms_zh: 未提供
- special_context_zh: 未提供
- cautions_zh: **没人喜欢这张**，但攻防兼备时还能凑合，相当于半张"冲刺"。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

**没人喜欢这张**，但攻防兼备时还能凑合，相当于半张"冲刺"。

### 欺凌 / Bully

- id: `bully`
- raw_heading: 欺凌（Bully）
- base_tier: D (特定情境)
- cost: 未提供
- type: 未提供
- aliases_zh: 欺凌
- synergy_terms_zh: 易伤、抽牌
- special_context_zh: **多易伤 + 多抽牌**才能用，条件相当苛刻。
- cautions_zh: **多易伤 + 多抽牌**才能用，条件相当苛刻。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

**多易伤 + 多抽牌**才能用，条件相当苛刻。

### 恶毒 / Vicious

- id: `vicious`
- raw_heading: 恶毒（Vicious）
- base_tier: D (特定情境)
- cost: 未提供
- type: 未提供
- aliases_zh: 恶毒
- synergy_terms_zh: 易伤、抽牌、能量
- special_context_zh: 抽牌很好，**绑定易伤就很难受**。、多易伤 + 多余能量时不错。
- cautions_zh: 抽牌很好，**绑定易伤就很难受**。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

抽牌很好，**绑定易伤就很难受**。多易伤 + 多余能量时不错。

## F 级

### 浩劫 / Havoc

- id: `havoc`
- raw_heading: 浩劫（Havoc）
- base_tier: F (最难用)
- cost: 未提供
- type: 未提供
- aliases_zh: 浩劫
- synergy_terms_zh: 消耗
- special_context_zh: **不知道会出什么牌还顺便消耗**，对普通牌组风险太高。
- cautions_zh: **不知道会出什么牌还顺便消耗**，对普通牌组风险太高。、这游戏没有真正的"无法使用"的牌，但浩劫是最接近的。
- upgrade_notes_zh: 未提供
- source_missing_fields: cost、type
- source_doc: private archive (not included in this public portfolio)/杀戮尖塔2》铁甲战士（Ironclad）卡牌强度榜详细攻略.md

#### 原评价

**不知道会出什么牌还顺便消耗**，对普通牌组风险太高。这游戏没有真正的"无法使用"的牌，但浩劫是最接近的。
