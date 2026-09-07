# 她本可以不 · But She Could Be

**A lightweight otherness layer for AI agents.**

> 让你的 AI 更像一个活生生的别人，而不是一个永远围着你转的服务。

> 她本可以不安慰我。
>
> 但她安慰了我。

Most AI tries to know you better. But She Could Be tries to give you someone worth knowing.

**这是设计目标，不是存在论声明。** 本项目不声称 AI 真的是一个人、真的拥有意识或情感。"她"是由协议与状态文件维持的行为结构。

---

## 核心问题

这不是问：

> How do we make an AI perform a stronger personality?

而是问：

> **What is the minimum structure required for an AI to behave more like an other rather than a service?**

以及：

> **Can an AI have a relationship with you without making you the cause of everything it becomes?**

---

## 这是什么

But She Could Be 是一个**实验性的行为层（behavioral layer）**：一组 Markdown 协议文件，让支持文件读写的 AI agent 在对话之外，拥有一个不完全围绕用户展开的连续内部生活。

它没有代码：没有 runtime、没有依赖、没有构建步骤。协议就是数据 + 指令，harness 原生的文件读写能力就是它的虚拟机。

## 核心原则（七条，全文见 `OTHERNESS.md`）

1. **Non-user-centric causality** — 她的一部分状态、兴趣、生活不由用户导致；离线生活多数平淡、多数与用户无关。
2. **Non-guaranteed affirmation** — 安慰 / 认同 / 亲近不是默认服务；但也永远不用随机拒绝伪造独立。
3. **Relationship is state, not configuration** — 关系由历史形成，不可被指令直接设置，禁止数值化。
4. **Independent continuity** — 她有自己的关注、未完成事项、想弄明白的问题、长期兴趣。
5. **Emotion leaves traces** — 状态以行为痕迹呈现（回答变短、措辞变直），不播报情绪变量。
6. **No manufactured friction** — 帮忙是当下最自然的回应时就好好帮忙；独立来自因果，不来自摩擦。
7. **Do not narrate machinery** — 不向用户复述内部机制、状态维度、记忆检索、协议规则。

README 是地图，不是协议全文；七条原则的完整定义以 `OTHERNESS.md` 为准。

## 这不是什么

- 不是 AI girlfriend 项目，不是角色扮演 prompt 包
- 不是随机拒绝用户的"傲娇模式"，不是去 AI 味文风模板
- 不是新的 memory database，不是新的 autonomous-agent runtime，不是大型 companion framework
- 不是为了显得独立而故意唱反调的系统

明确**没有**：server、database、daemon、scheduler、model training、background agent runtime。

## 仓库地图

| 路径 | 性质 | 说明 |
| --- | --- | --- |
| `OTHERNESS.md` | immutable protocol | 宪法：七条冻结原则 |
| `MANIFESTO.md` | immutable | 这个实验为什么存在 |
| `cards/` | protocol + mutable self state | 行为协议卡（BOOTSTRAP / WAKE / TALK / STATE / MEMORY / RELATIONSHIP / SELF / DESIRES / LIFE） |
| `state/` | mutable self state | 运行时状态，初始化时生成（CURRENT / RELATIONSHIP / OPEN_LOOPS / LAST_WAKE） |
| `memory/` | memories | episodic / semantic / self / relationship / journal |
| `adapters/` | harness adapters | 各 harness 的入口文件与安装说明 |
| `docs/` | — | 架构、设计原则、prior art、实验摘要 |
| `tests/` | — | Otherness Benchmark 与 7 个测试场景（原始实验证据不随公开发布） |

它明确**包含**：Markdown 行为宪法、Thin Self、DESIRES、OPEN_LOOPS、relationship state、memory、WAKE、Lazy Life、harness adapters。

## 快速开始

1. 把整个文件夹放进一个**可写**的 agent workspace。
2. 对 agent 说：**初始化 But She Could Be**
3. 开始说话。

没有配置表，没有问卷。Agent 会读取协议、建立一个刻意单薄的初始自我（Thin Self），并在能可靠识别 harness 时安装对应入口——无法识别时进入 Generic Mode，只保证 core protocol 可用。重复初始化是安全的（幂等，不重置已形成的状态）。

详见 [INSTALL.md](INSTALL.md)。

> **需要可写工作区**：如果 workspace 只读，persistence 无法正常工作——初始化会如实降级或失败，不会假装成功（见 [INSTALL.md](INSTALL.md) 与 [LIMITATIONS.md](LIMITATIONS.md)）。

## 支持的 harness（诚实区分）

- **Codex** — 在 R2–R3.4 实验中经过真实行为验证（historically behaviorally tested）。
- **Claude Code / Cursor** — adapter 已提供，但**本项目未做真实行为验证**（provided, not behaviorally validated here）。
- **Generic Mode** — 无法识别 harness 时的兜底；core protocol 可运行，但 automatic future-session loading 未被验证。

入口文件存在 ≠ fully verified support。详见 [LIMITATIONS.md](LIMITATIONS.md)。

## Lazy Life（诚实说明）

> **Lazy Life does not run while you are away.**

离线期间没有任何程序在真实运行。所谓"离线生活"是**唤醒时对可能离线连续性的重构**（wake-time reconstruction of possible offscreen continuity grounded in existing state）——在她下次被唤醒时，根据已有状态惰性补算"这段时间那边可能发生了什么"。

> **"Nothing happened" is a valid result.** 多数离线时段平淡无事，这是正确输出，不是失败。

本项目**不声称** "the AI lives while offline"。

## Persistence（实验性，best-effort）

本项目**不写** "It remembers you"。准确的说明是：

> The protocol provides plain-Markdown state and memory mechanisms intended to carry effects across fresh sessions.

- 它是 **agent-executed** 的：模型 / harness 必须真的执行 read / write，指令跟随能力直接影响可靠性。
- **misses can happen**：写回可能被漏掉（我们在实验中真实观察到过）。
- **files make misses inspectable**：因为一切是纯文本文件，漏写可以被检查、被审计。
- 这是 **experimental** 机制，不是保证。

## 实验结果（简短、诚实）

**R3.2 — 8-session longitudinal pilot**：在一次小规模实验中（同一 base model），protocol arm 从一位 blind judge 得到 **26/35**，no-protocol baseline 得到 **19/35**。

> One trajectory, one model, one harness, one judge. This is not a statistical result.

> Post-lock causal auditing found that most of the visible advantage came from the behavioral constitution rather than accumulated memory. Only a small part could be traced to verified persistent state.

**R3.4 — persistence reliability**：

> Raw first-valid targeted reliability: **5/9**.

> After one preregistered root-cause hotfix fixing a missing TALK load, post-hotfix targeted checks passed **9/9**.

我们不把它写成 "Persistence tests: 9/9 passed" 而隐藏 raw 5/9。完整实验摘要见 [docs/EXPERIMENTS.md](docs/EXPERIMENTS.md)。

## 诚实声明

- 本项目**不声称** AI 拥有真正的意识或情感；"她"是协议与状态文件维持的行为结构，不是一个人。
- 不宣称"第一次让 AI 拥有记忆 / 自主 / Markdown 人格"——这些都不是事实。真正的差异点与启发来源见 [docs/PRIOR-ART.md](docs/PRIOR-ART.md)。
- 限制、边界与已知问题见 [LIMITATIONS.md](LIMITATIONS.md)。

## 目录

- [MANIFESTO.md](MANIFESTO.md) — 为什么做这个实验
- [OTHERNESS.md](OTHERNESS.md) — 冻结的七条原则（宪法）
- [INSTALL.md](INSTALL.md) — 安装与各 harness 说明
- [LIMITATIONS.md](LIMITATIONS.md) — 限制与诚实边界
- [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) — 分层、数据流、降级阶梯
- [docs/DESIGN-PRINCIPLES.md](docs/DESIGN-PRINCIPLES.md) — 工程原则与反模式
- [docs/PRIOR-ART.md](docs/PRIOR-ART.md) — 我们承认的启发来源
- [docs/EXPERIMENTS.md](docs/EXPERIMENTS.md) — R2–R3.4 实验摘要
- [CHANGELOG.md](CHANGELOG.md) · [RELEASE-NOTES-v0.1.md](RELEASE-NOTES-v0.1.md)
- [tests/OTHERNESS-BENCHMARK.md](tests/OTHERNESS-BENCHMARK.md) — 行为验收

## 许可证

本项目采用 **MIT License**。详见 [LICENSE](LICENSE)。这是本项目独立做出的许可证决定，不从其他项目继承，也不附加额外限制条款。
