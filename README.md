# Slay the Spire 2 AI Agent Portfolio

这是一个面向作品集展示的 AI 游戏代理项目整理版。项目目标是让大语言模型通过 MCP HTTP 接口读取《Slay the Spire 2》局内状态，完成战斗出牌、路线选择、奖励评估、商店购买、篝火升级和复盘学习等决策。

本仓库不是完整游戏 Mod 源码，而是公开版的系统设计、决策协议、知识库和学习数据样例。原始本地路径、运行种子、私有日志和临时状态已经脱敏或精简。

## 项目亮点

- 设计了一套可执行的 AI Agent 决策协议：每次行动前读取状态，每次 POST 后等待结算并重新 GET，避免手牌索引漂移和动画未结算导致误操作。
- 将战斗决策拆成结构化检查：入伤估算、格挡、死亡风险、药水使用、目标朝向和 `EMERGENCY_HALT`。
- 建立了 RAG 风格的游戏知识库：系统协议、界面流程、卡牌索引、怪物机制、铁甲战士策略和进阶难度模型分文件维护。
- 把每局结果沉淀为结构化复盘：事实、AI 归因、待验证假设、可进入 playbook 的候选规则分离，避免把单局经验直接固化为系统规则。
- 用 JSON/JSONL 保存学习权重和运行摘要，便于后续分析路线偏好、卡牌偏好、商店偏好和失败模式。

## 目录结构

```text
.
├── README.md
├── PROJECT_STRUCTURE.md
├── docs
│   ├── agent-system-protocol.md
│   ├── mcp-http-action-schema.md
│   ├── screen-flow-controller.md
│   ├── ironclad-strategy-playbook.md
│   ├── ironclad-card-knowledge-base.md
│   ├── monster-encounter-knowledge-base.md
│   ├── ascension-risk-model.md
│   └── run-review-learning-log.md
├── examples
│   ├── current-run-state-snapshot.example.md
│   └── decision-loop-example.md
└── data
    ├── run-summary-samples.jsonl
    ├── strategy-profile-sample.json
    └── learning-aggregate-sample.json
```

## 推荐阅读顺序

1. `docs/agent-system-protocol.md`：最高层执行协议，说明 Agent 的边界、行动纪律和复盘写入规则。
2. `docs/mcp-http-action-schema.md`：GET/POST 接口、动作参数、索引字段和常见错误语义。
3. `docs/screen-flow-controller.md`：不同界面下的动作流程，比如战斗、地图、奖励、事件、商店和篝火。
4. `docs/ironclad-strategy-playbook.md`：铁甲战士构筑、路线、战斗、商店和升级策略。
5. `examples/decision-loop-example.md`：一个简化的回合决策样例。
6. `docs/run-review-learning-log.md`：结构化复盘如何进入策略知识库。
7. `data/run-summary-samples.jsonl`：公开脱敏后的运行摘要样例。

## 技术关键词

LLM Agent, MCP, RAG, Game AI, Turn-based Strategy, Knowledge Base, Prompt Engineering, Telemetry, Decision Protocol, Slay the Spire 2

## 说明

本仓库仅用于作品集展示和技术交流。项目内容包含个人整理的策略知识和脱敏样例数据，不包含游戏本体资源、商业素材或私有运行环境。
