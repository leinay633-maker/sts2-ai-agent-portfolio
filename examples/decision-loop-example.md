# Decision Loop Example

下面是一次简化的战斗回合决策循环，展示这个项目的核心工程约束：Agent 不连续盲打，每次动作后等待游戏结算并重新读取状态。

## 1. GET 当前状态

```powershell
Invoke-RestMethod -Uri 'http://localhost:15526/api/v1/singleplayer' -Method Get |
  ConvertTo-Json -Depth 80
```

Agent 读取：

- 当前界面是否为 combat。
- 是否是玩家可行动阶段。
- 手牌、能量、敌人 `entity_id`、敌人意图、玩家 HP/block。
- 抽牌堆、弃牌堆、药水、遗物和已生效能力。

## 2. 形成回合检查

```yaml
turn_check:
  incoming_damage_next_turn:
    total: 45
    breakdown:
      - source: "QUEEN_0"
        damage: 45
        unblockable: false
  my_block_after_this_turn: 66
  my_hp_worst_case: 63
  death_risk: false
  debuff_target: "QUEEN_0"
```

如果 `death_risk=true`，Agent 必须优先寻找格挡、虚弱、朝向调整、无实体、回血或药水方案。若没有生路，则停止 POST 并输出 `EMERGENCY_HALT`。

## 3. POST 一个动作

```powershell
Invoke-RestMethod `
  -Uri 'http://localhost:15526/api/v1/singleplayer' `
  -Method Post `
  -ContentType 'application/json' `
  -Body (@{ action='play_card'; card_index=1; target='QUEEN_0' } | ConvertTo-Json -Compress)
```

## 4. 等待结算并重新 GET

```powershell
Start-Sleep 2
Invoke-RestMethod -Uri 'http://localhost:15526/api/v1/singleplayer' -Method Get |
  ConvertTo-Json -Depth 80
```

重新读取是必要步骤，因为一张牌可能触发：

- 手牌索引左移。
- 抽牌、弃牌、消耗。
- 能量变化。
- 敌人死亡或召唤。
- 奖励页或事件页跳转。
- 动画未结算导致的短暂旧帧。

## 5. 继续或停止

Agent 只在最新 GET 状态稳定、索引重新确认、风险重新计算后，才执行下一张牌或结束回合。
