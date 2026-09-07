# PRIOR ART

本项目**不声称**发明以下任何一项：persistent AI identity、Markdown identity files、agent memory、persona systems、persistent / autonomous companions、generative-agent memory & reflection、workspace instruction files、role cards、long-term memory、agent heartbeat。以下是本项目知道并承认启发的来源。

## 登记表

| 项目 / 工作 | 我们借鉴的思想 | 是否复制其文本 / 代码 |
| --- | --- | --- |
| Generative Agents（Park et al., 2023, 斯坦福） | 记忆流 + 反思（reflection）分层；行为由记忆与状态推导而非即时配置 | 否，仅思想 |
| SillyTavern | 角色卡与长期会话持久化的形态；社区对 companion prompt 工程的大量实践 | 否。其许可证约束严格（以仓库标注为准），未来任何复制前必须先过许可证审查 |
| OpenClaw | "workspace 文件即状态"的思路；heartbeat / 持续性的设计方向；AGENTS.md 约定 | 否，仅思想 |
| OpenMino | 持久化 AI companion / 内在生活方向（未深入审阅，仅登记方向） | 否 |
| ai-companion-pi | companion 持久化方向（未深入审阅，仅登记方向） | 否 |
| Crescent Grove | 长期陪伴 / 连续性方向（未深入审阅，仅登记方向） | 否 |
| YuriOS | AI 伴侣操作系统方向（未深入审阅，仅登记方向） | 否 |
| Letta（原 MemGPT） | stateful agents 方向：长期记忆、身份与跨会话连续性；agent 行为由记忆与状态推导而非即时配置；把 memory / context / state 当作可复用基础设施来做 | 否，仅思想 |
| Mem0 / OpenMemory | 通用持久化记忆层方向：自动 capture / retrieval、跨工具共享记忆、以 MCP server 形式接入各类 agent 客户端 | 否，仅思想 |

## 许可证政策

- 各项目许可证以其实际仓库标注为准；本表不对未逐项核实的许可证下结论。
- v0.1 **未复制任何外部 prompt、文本或源码**；全部文件为本项目原创表述。
- 若未来引入任何第三方文本 / 代码，必须依次：检查许可证 → 明确记录来源与版本 → 满足 attribution / license 要求 → 无必要则优先重新实现思想而非复制实现。

## 本项目真正的差异点

本项目只是 **a small behavioral protocol combining known primitives around one design question: otherness**——不是以上任何能力，而是把一个此前很少被作为**核心问题**对待的问题放在正中央：

> What is the minimum structure required for an AI to feel like an other rather than a service?

> Can an AI have a relationship with you without making you the cause of everything it becomes?

即：其他项目问"怎么让 AI 更会陪伴 / 更有记忆 / 更像人"；本项目问"AI 的行为能否拥有一个不完全由当前用户请求解释的因果来源"。"她本可以不"衡量的是这个因果结构存在与否，而不是文风像不像真人。

因此上表不是竞品比较：Letta 与 Mem0 / OpenMemory 做的是 **memory / context / state 基础设施**，本项目不竞争这一层，也不声称在记忆技术上更新。本项目借用的只是同一个思想前提——持久状态可以支撑一个不完全由当前用户请求解释的连续主体。
