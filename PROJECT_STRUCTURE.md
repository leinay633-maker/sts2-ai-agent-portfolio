# Project Structure

## docs

- `agent-system-protocol.md`: Agent 最高层协议。定义行动边界、RAG 启动读取顺序、MCP 调用纪律、死亡风险中止协议和知识库写入规则。
- `mcp-http-action-schema.md`: MCP HTTP 接口说明。列出 GET/POST 调用方式、动作参数、索引字段、错误语义和 POST 后置流程。
- `screen-flow-controller.md`: 界面流转控制器。按 combat、map、reward、event、shop、rest site 等界面拆分动作入口、知识检索和退出条件。
- `ironclad-strategy-playbook.md`: 铁甲战士策略手册。包含战斗、路线、构筑、商店、升级和外部攻略引用规则。
- `ironclad-card-knowledge-base.md`: 铁甲战士卡牌知识库。用于选牌、升级、删牌、商店和战斗内关键牌执行判断。
- `monster-encounter-knowledge-base.md`: 怪物与 Boss 机制知识库。用于战斗前机制检索和危险回合判断。
- `ascension-risk-model.md`: 进阶难度的资源压力模型，用于路线和风险偏好调整。
- `run-review-learning-log.md`: 公开版结构化复盘样例，展示事实、归因、假设和候选规则如何分离。

## examples

- `current-run-state-snapshot.example.md`: 一份脱敏的当前局状态快照示例，展示 Agent 如何记录跨窗口交接信息。
- `decision-loop-example.md`: 一次简化的决策循环，展示 GET、评估、POST、等待、再 GET 的操作闭环。

## data

- `run-summary-samples.jsonl`: 脱敏后的运行摘要样例。每行是一局的 compact telemetry。
- `strategy-profile-sample.json`: 从多局结果中沉淀的路线、选牌、商店和休息偏好样例。
- `learning-aggregate-sample.json`: 聚合统计样例，展示总局数、平均楼层、早死率和卡组指标。

## Public Version Notes

公开版做了以下处理：

- 移除了原始本地目录、运行种子、私有日志路径和异常堆栈。
- 不包含私有 archive 目录和完整流水日志。
- 保留能体现项目能力的系统设计、知识库结构、决策协议和样例数据。
