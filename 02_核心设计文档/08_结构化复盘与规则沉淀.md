# Run Review Learning Log

这份文件是公开版复盘样例，展示项目如何把单局结果沉淀为可检索、可验证、可人工审核的知识。完整私有日志未包含在公开仓库中。

## Structured Write Schema

```yaml
- run_id: <YYYY-MM-DD-A{n}-{seq}>
  result: <won | loss | act_cleared | key_failure_experience>
  facts:
    final_floor: <int>
    final_hp: <int>
    deck_final: [<card list>]
    deck_size: <int>
    relics: [<relic list>]
    potions_unused: [<potion list>]
    key_decision_floors: [<floor list>]
  ai_attribution:
    primary_cause: <text>
    confidence: <low | medium | high>
    confidence_basis: <text>
  patterns_to_test:
    - hypothesis: <text>
      verification_method: <text>
  rules_proposed:
    - <candidate rule; must be reviewed before entering playbook>
```

## Example 1 - Tactical Correction: Draw Order and Battle Trance

```yaml
- run_id: public-example-A10-act2-tactical-correction-01
  result: key_failure_experience
  facts:
    source: "user correction + MCP combat decision process"
    act: 2
    floor: 30
    ascension: 10
    deck_context:
      - "Dark Embrace"
      - "Feel No Pain"
      - "Offering+"
      - "Bloodletting"
      - "Battle Trance"
    correction:
      - "Battle Trance conflicts with the current exhaust-draw engine because it blocks later draw."
      - "The MCP draw_pile list should be treated as a remaining-card set, not as guaranteed draw order."
  ai_attribution:
    primary_cause: "The Agent incorrectly treated draw_pile ordering as deterministic and considered Battle Trance as a search tool, which would cut off Dark Embrace and other draw effects."
    confidence: high
    confidence_basis: "Mechanic error was explicitly identified and then converted into playbook rules."
  patterns_to_test:
    - hypothesis: "In a Dark Embrace / Feel No Pain exhaust deck, Battle Trance is usually anti-synergistic unless it is needed for immediate lethal or survival."
      verification_method: "Track whether skipping Battle Trance preserves exhaust-driven draw and block generation."
    - hypothesis: "MCP draw_pile display should not be used as draw-order evidence unless a controlled top-deck effect and a later GET confirm it."
      verification_method: "In future draw-planning decisions, compare predicted draw order with actual post-action hand states."
  rules_proposed:
    - id: consume_engine_avoid_battle_trance
      condition: "deck relies on Dark Embrace / Feel No Pain / exhaust draw loop"
      action: "Treat Battle Trance as normally unplayable unless no further draw is needed, or immediate survival/lethal requires it."
      confidence: high
    - id: mcp_draw_pile_not_ordered
      condition: "planning any draw effect from MCP draw_pile list"
      action: "Do not infer draw order from listed draw_pile; treat it as unordered unless verified by card effect and later GET."
      confidence: high
```

## Example 2 - Act 2 Boss Clear: Exhaust Engine Stabilization

```yaml
- run_id: public-example-A10-act2-boss-clear-01
  result: act_cleared
  facts:
    act: 2
    floor: 33
    ascension: 10
    post_boss_hp: "55/74"
    deck_size_after_rewards: 23
    boss: "Knowledge Demon"
    key_cards:
      - "Dark Embrace+"
      - "Feel No Pain"
      - "Burning Pact"
      - "Offering+"
      - "Bloodletting"
      - "Body Slam+"
      - "Fiend Fire"
  ai_attribution:
    primary_cause: "The deck won after establishing Dark Embrace+ and multiple block-on-exhaust effects, then converting exhaust actions into draw, block and Body Slam damage."
    confidence: high
    confidence_basis: "MCP states confirmed post-boss HP, reward resolution, deck count and transition into Act 3 map."
  patterns_to_test:
    - hypothesis: "Fiend Fire can be taken even above a 20-card deck size when Dark Embrace and Feel No Pain are established, because it provides damage, exhaust triggers and bad-hand cleanup."
      verification_method: "Track Fiend Fire turns by hand size, exhaust count, block gained, cards drawn and whether it solves a dangerous turn."
  rules_proposed:
    - id: fiend_fire_pick_when_exhaust_engine_established_even_above_20
      condition: "Act 2 boss reward offers Fiend Fire and deck already has Dark Embrace plus Feel No Pain-style payoff."
      action: "Take Fiend Fire unless energy is so constrained that a 2-cost exhaust attack is effectively unplayable."
      confidence: high
```

## Example 3 - Final Boss Clear: High Block to Body Slam

```yaml
- run_id: public-example-A10-act3-final-clear-01
  result: final_boss_defeated
  facts:
    act: 3
    floor: 49
    ascension: 10
    post_final_hp: "71/80"
    deck_size_after_clear: 26
    boss_sequence:
      - "Experiment #C16"
      - "Queen"
    key_cards:
      - "Dark Embrace+"
      - "Feel No Pain"
      - "Feel No Pain+"
      - "Burning Pact+"
      - "Body Slam+"
      - "Fiend Fire"
  ai_attribution:
    primary_cause: "The Agent prioritized setup powers first, then used exhaust cards to raise block and turn high block into Body Slam damage. It focused the leader instead of spending the main burst on a minion."
    confidence: high
    confidence_basis: "Final MCP states confirmed reward screen after boss combat, post-combat HP and deck size."
  rules_confirmed:
    - id: queen_focus_leader_not_minion
      condition: "Queen and minion are both present"
      action: "Focus main damage on Queen unless the minion creates unavoidable lethal pressure."
      confidence: high
    - id: high_block_body_slam_finish
      condition: "Block engine is online and Body Slam+ is available"
      action: "Raise block first, then use Body Slam+ as the main damage conversion."
      confidence: high
```
