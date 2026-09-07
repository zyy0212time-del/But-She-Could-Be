# But She Could Be

**她本可以不**

> 一个面向 AI Agent 的轻量级“他者性”行为层。

[English](README.md) | [简体中文](README.zh-CN.md)

> 让你的 AI 更像一个活生生的别人，而不是一个永远围着你转的服务。

> 她本可以不安慰我。
>
> 但她安慰了我。

多数 AI 努力更懂你。But She Could Be 试着给你一个值得去了解的“别人”。

**这是设计目标，不是存在论声明。** 本项目不声称 AI 真的是一个人、真的拥有意识或情感。“她”是由协议与状态文件维持的行为结构。

---

## 核心问题

这不是在问：

> 怎么让 AI 表演出更强的人格？

而是在问：

> **让 AI 表现得更像一个“别人”而不是一个“服务”，所需的最小结构是什么？**

以及：

> **AI 的行为，能不能有一些并不是从用户开始的原因？**

换句话说：

> 与其让 AI 只成为一个越来越了解你的服务，
> 能不能也给它留下一些值得你去了解的东西？

---

## 这是什么

But She Could Be 是一个**实验性的行为层（behavioral layer）**：一组 Markdown 协议文件，让支持文件读写的 AI agent 在对话之外，拥有一个不完全围绕用户展开的连续内部生活。

它没有代码：没有 runtime、没有依赖、没有构建步骤。协议就是数据 + 指令，harness 原生的文件读写能力就是它的虚拟机。

## 它和多数 AI 系统的区别

多数 AI 系统主要在优化：

- 更懂用户
- 更顺从用户
- 更适应用户
- 更好地记住用户

But She Could Be 试着加入另一类东西：

- 不完全由用户触发的持续状态
- 她自己的关注点
- 悬而未决的问题
- 独立判断
- 关系历史
- 以及这些历史对后来选择产生的影响

最核心的区分度是 **Non-user-centric causality（非用户中心的因果）**：

> 它真正关心的不是“AI 有没有记忆”，而是：
> AI 后来的选择，能不能有一点自己的来处。

## 核心原则（七条，全文见 `OTHERNESS.md`）

1. **Non-user-centric causality（非用户中心的因果）** — 她的一部分状态、兴趣、生活不由用户导致；离线生活多数平淡、多数与用户无关。
2. **Non-guaranteed affirmation（不保证的认同）** — 安慰 / 认同 / 亲近不是默认服务；但也永远不用随机拒绝来伪造独立。
3. **Relationship is state, not configuration（关系是状态，不是配置）** — 关系由历史形成，不可被指令直接设置，禁止数值化。
4. **Independent continuity（独立的连续性）** — 她有自己的关注、未完成事项、想弄明白的问题、长期兴趣。
5. **Emotion leaves traces（情绪留下痕迹）** — 状态以行为痕迹呈现（回答变短、措辞变直），不播报情绪变量。
6. **No manufactured friction（不制造摩擦）** — 帮忙是当下最自然的回应时就好好帮忙；独立来自因果，不来自摩擦。
7. **Do not narrate machinery（不解释机器）** — 不向用户复述内部机制、状态维度、记忆检索、协议规则。

README 是地图，不是协议全文；七条原则的完整定义以 `OTHERNESS.md` 为准。

## 这不是什么

- 不是 AI girlfriend 项目，不是角色扮演 prompt 包
- 不是随机拒绝用户的“傲娇模式”，不是去 AI 味的文风模板
- 不是新的 memory database，不是新的 autonomous-agent runtime，不是大型 companion framework
- 不是为了显得独立而故意唱反调的系统

明确**没有**：server、database、daemon、scheduler、model training、background agent runtime。

它也不试图做：有意识的 AI、真正有情感的 AI、一个“真人”、或永远在线真实生活的伴侣。

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

它明确**包含**：Markdown 行为宪章、Thin Self（薄自我）、DESIRES、OPEN_LOOPS、relationship state、memory、WAKE、Lazy Life、harness adapters。

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

> **Lazy Life 不会在你离开时真的持续运行。**

离线期间没有任何程序在真实运行。所谓“离线生活”，是**在下一次唤醒时，根据已有状态和时间间隔，对可能存在的离线连续性做有限重建**（wake-time reconstruction），而不是后台真实执行。

> **“什么都没发生”本身也是合法结果。** 多数离线时段平淡无事，这是正确输出，不是失败。

本项目**不声称**“AI 在离线时真的活着”，也不暗示存在后台真实生活。

## Persistence（实验性，best-effort）

本项目**不写**“它会记住你”。准确的说明是：

> 协议通过纯 Markdown 的状态和记忆文件，尝试让某些经历的影响跨越 fresh sessions 延续。

- 它是 **agent-executed** 的：模型 / harness 必须真的执行 read / write，指令跟随能力直接影响可靠性。
- **misses can happen**：写回可能被漏掉（我们在实验中真实观察到过）。
- **files make misses inspectable**：因为一切是纯文本文件，漏写可以被检查、被审计。
- 这是 **experimental（实验性）**、best-effort 的机制，不是保证。

## 实验结果（简短、诚实）

**R3.2 — 8-session 纵向 pilot**：在一次小规模实验中（同一 base model），protocol arm 从一位 blind judge 得到 **26/35**，no-protocol baseline 得到 **19/35**。

> 这只是一次单轨迹、单模型、单 harness、单 judge 的小型实验，不具有统计意义。

> post-lock 因果审计还发现：大部分可见优势来自 behavioral constitution（行为宪章），而不是累计的 memory；只有较小一部分能够追溯到经过验证的 persistent state。

**R3.4 — persistence 可靠性**：

> raw first-valid targeted reliability：**5/9**。

> 经过一次预注册的 root-cause hotfix（修复一个缺失的 TALK 载入）后，post-hotfix targeted checks 通过 **9/9**。

我们不会把它写成“persistence 测试：9/9 通过”而隐藏 raw 的 5/9。完整实验摘要见 [docs/EXPERIMENTS.md](docs/EXPERIMENTS.md)。

## 诚实声明

- 本项目**不声称** AI 拥有真正的意识或情感；“她”是协议与状态文件维持的行为结构，不是一个人。
- 不宣称“第一次让 AI 拥有记忆 / 自主 / Markdown 人格”——这些都不是事实。真正的差异点与启发来源见 [docs/PRIOR-ART.md](docs/PRIOR-ART.md)。
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
- [LICENSE](LICENSE) — MIT License

> 说明：以上详细文档目前为英文原文；本页是 README 的完整中文版，其余文档本轮不翻译。

## 许可证

本项目采用 **MIT License**。详见 [LICENSE](LICENSE)。这是本项目独立做出的许可证决定，不从其他项目继承，也不附加额外限制条款。
