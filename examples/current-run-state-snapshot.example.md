# Current Run State Snapshot Example

这是一份公开版示例，展示 Agent 在跨窗口或换模型时如何交接局面。实际项目中，实时事实仍以 MCP GET 返回为准，不能直接沿用旧快照里的手牌索引。

```yaml
character: Ironclad
ascension: 10
current_act: 3
current_floor: 49
state_source: "MCP GET snapshot; stale after any game action"
run_goal: "A10 clear with exhaust + block + Body Slam engine"
run_mode: "manual-started run, AI-controlled decisions"

latest_mcp_snapshot:
  state_type: combat
  battle:
    act: 3
    floor: 49
    round: 2
    turn: player
    is_play_phase: true
    enemy:
      entity_id: "QUEEN_0"
      name: "Queen"
      hp: 134
      intent: "multi-hit attack"
  player:
    hp: 63
    max_hp: 80
    block: 10
    energy: 4
    deck_count: 26
    key_powers:
      - "Dark Embrace+"
      - "Feel No Pain"
      - "Feel No Pain+"
    hand_summary:
      - "Burning Pact+"
      - "Uppercut+"
      - "Body Slam+"
      - "Fiend Fire"

current_build_plan:
  archetype: "exhaust draw + block conversion"
  priorities:
    - "Keep Dark Embrace and Feel No Pain online."
    - "Use exhaust cards to generate block and draw."
    - "Apply Weak/Vulnerable before high-damage Body Slam turns."
    - "Reconfirm hand index after every POST."

handoff_protocol:
  - "Read system protocol and screen-flow controller first."
  - "GET current MCP state before any action."
  - "Do not trust card indexes from this snapshot."
```
