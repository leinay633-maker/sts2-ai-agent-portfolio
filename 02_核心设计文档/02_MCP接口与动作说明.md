# MCP HTTP Schema

Endpoint: `http://localhost:15526/api/v1/singleplayer`

## GET 状态

```powershell
Invoke-RestMethod -Uri 'http://localhost:15526/api/v1/singleplayer' -Method Get | ConvertTo-Json -Depth 80
```

## POST 动作

```powershell
Invoke-RestMethod -Uri 'http://localhost:15526/api/v1/singleplayer' -Method Post -ContentType 'application/json' -Body (@{ action='<action>'; ... } | ConvertTo-Json -Compress) | Out-Null
```

成功动作通常返回 `{ "status": "ok", "message": "..." }`。动作级失败通常返回 HTTP 200 + `{ "status": "error", "error": "..." }`。

HTTP 层错误：`400` Invalid JSON/Missing action；`404` Not found；`405` Method not allowed；`409` 运行状态冲突；`500` Internal error/Action failed。

## POST 后置协议

- 任何成功 POST 都不要立刻 GET；先 `Start-Sleep 2`，等待动画、抽牌、消耗、奖励结算、手牌索引左移或状态刷新完成，再 GET 一次。
- 同一动作后禁止“立刻 GET + 延迟 GET”的双重读取；默认每个成功 POST 只对应一次延迟 GET。
- `end_turn` 也先 `Start-Sleep 2` 再 GET 一次；若仍不是玩家可行动阶段，才继续按“等待 2 秒 + GET 一次”的低频节奏重试。
- 只有当 HTTP 请求失败或动作明确返回 `status=error` 且没有入队执行时，才允许立即 GET 排查错误。

## Actions

| action | params | preconditions | post protocol |
|---|---|---|---|
| `play_card` | card_index:int required; target:string entity_id required when card targets enemy | combat play phase | Start-Sleep 2; then GET once; target must be entity_id |
| `use_potion` | slot:int required; target:string entity_id required for enemy-targeting potion | potion usable; combat potion requires play phase | Start-Sleep 2; then GET once |
| `discard_potion` | slot:int required | potion slot has potion | Start-Sleep 2; then GET once |
| `end_turn` | none | combat play phase; hand not in selection/card-play mode | complete turn_check; Start-Sleep 2; then GET once; retry only if not player-action phase |
| `choose_map_node` | index:int required from next_options | map screen open | Boss node requires latest STS2Saves autosave lookup + mirror backup before POST; no user reminder |
| `choose_event_option` | index:int required | event room open; option enabled | Start-Sleep 2; then GET once |
| `advance_dialogue` | none | event ancient dialogue active | Start-Sleep 2; then GET once |
| `choose_rest_option` | index:int required | rest site open; option enabled | Start-Sleep 2; then GET once |
| `shop_purchase` | index:int required | shop open; enough gold; item not sold out | Start-Sleep 2; then GET once |
| `claim_reward` | index:int required | reward screen open; reward claimable | Start-Sleep 2; then GET once; indexes shift left |
| `select_card_reward` | card_index:int required | card reward selection screen open | Start-Sleep 2; then GET once |
| `skip_card_reward` | none | card reward screen with skip option | Start-Sleep 2; then GET once |
| `proceed` | none | enabled proceed button exists | Start-Sleep 2; then GET once |
| `select_card` | index:int required | card selection grid/screen open | Start-Sleep 2; then GET once |
| `confirm_selection` | none | selection screen confirm enabled | Start-Sleep 2; then GET once |
| `cancel_selection` | none | selection screen cancel/skip enabled | Start-Sleep 2; then GET once |
| `select_bundle` | index:int required | bundle selection screen open | Start-Sleep 2; then GET once |
| `confirm_bundle_selection` | none | bundle confirm enabled | Start-Sleep 2; then GET once |
| `cancel_bundle_selection` | none | bundle cancel enabled | Start-Sleep 2; then GET once |
| `combat_select_card` | card_index:int required | in-combat hand selection active | Start-Sleep 2; then GET once |
| `combat_confirm_selection` | none | in-combat hand selection active; confirm enabled | Start-Sleep 2; then GET once |
| `select_relic` | index:int required | relic selection screen open | Start-Sleep 2; then GET once |
| `skip_relic_selection` | none | relic selection screen has skip | Start-Sleep 2; then GET once |
| `claim_treasure_relic` | index:int required | treasure room open; relic collection visible | Start-Sleep 2; then GET once |
| `crystal_sphere_set_tool` | tool:string required: big|small | Crystal Sphere screen open | Start-Sleep 2; then GET once |
| `crystal_sphere_click_cell` | x:int; y:int required | Crystal Sphere hidden clickable cell exists | Start-Sleep 2; then GET once |
| `crystal_sphere_proceed` | none | Crystal Sphere proceed enabled | Start-Sleep 2; then GET once |
| `return_to_main_menu` | none | NGame ready | Forbidden unless user explicitly asks |
| `start_singleplayer_run` | character:string optional; seed:string optional; ascension:int optional | NGame ready; no run in progress | Must write previous summary first |
| `auto_slay_start_loop` | implementation-specific | auto loop available | FORBIDDEN |
| `auto_slay_stop` | none | auto loop available | Only if stopping accidental loop |

## 常见错误语义

- `No run in progress`: 当前没有正在进行的 Run。
- `Not in combat`: 非战斗场景不能执行战斗动作。
- `Not in play phase - cannot act during enemy turn`: 敌人回合或动画未结算，等待 2 秒后 GET 一次。
- `Player actions are currently disabled`: 动画、结算或动作队列锁定，等待 2 秒后 GET 一次。
- `Missing '<field>'`: 参数字段缺失。
- `<index> out of range`: 索引失效，必须重新 GET 当前列表。
- `Target '<id>' not found among alive enemies`: entity_id 错误或敌人已死亡，重新 GET。
- `Cannot end turn while a card is being played or hand is in selection mode`: 当前手牌处于动画或 hand_select，等待/完成选择后再行动。

## 索引字段速查

- `play_card`: `card_index`
- `combat_select_card`: `card_index`
- `select_card_reward`: `card_index`
- `select_card`: `index`
- `claim_reward`: `index`
- `choose_map_node`: `index`
- `choose_event_option`: `index`
- `choose_rest_option`: `index`
- `shop_purchase`: `index`
- `claim_treasure_relic`: `index`
- `use_potion`: `slot`
