# 她本可以不
### But She Could Be

面向 AI Agent 的轻量级"他者性"（otherness）行为层，纯 Markdown 协议，无代码。

[English](README.md) | 简体中文

## 它解决什么问题

多数 AI 系统的优化方向是：更懂用户、更顺从用户、更好地记住用户偏好。这类系统的行为链条几乎全部起于用户输入。

But She Could Be 给 AI agent 加一层状态，其中一部分不由用户触发：agent 有自己的关注点、未完成的问题、跨会话的关系历史，以及由此产生的行为差异。这层状态不改变 agent 的任务执行能力，只改变它在对话之外的行为来源。

这是设计目标，不是存在论声明：项目不声称 AI 具有真实意识或情感。"她"是协议和状态文件维持的行为结构。完整边界见"已知限制"一节。

## 核心机制

核心机制是 **Non-user-centric causality（非用户中心的因果）**：agent 的部分状态、兴趣、离线经历不由用户导致，也不总能追溯到当前对话。这是项目唯一的核心论点，后文不再重复解释，只描述各模块如何实现它。

协议由七条原则约束（全文见 `OTHERNESS.md`）：

1. **Non-user-centric causality** — 部分状态、兴趣、离线生活不由用户导致；离线内容多数与用户无关。
2. **Non-guaranteed affirmation** — 安慰、认同、亲近不是默认输出；但也不用随机拒绝伪造独立性。
3. **Relationship is state, not configuration** — 关系由历史生成，不能通过指令直接设置，不做数值化。
4. **Independent continuity** — agent 维护自己的关注点、未完成事项、长期兴趣，跨会话存在。
5. **Emotion leaves traces** — 状态变化体现为行为差异（回复变短、措辞变直接），不作为变量播报。
6. **No manufactured friction** — 该帮忙时正常帮忙；独立性来自因果链，不来自故意的拒绝或冷淡。
7. **Do not narrate machinery** — 不向用户复述内部机制、状态维度、记忆检索过程或协议规则。

这七条是行为约束，不是完整实现；具体机制见下一节。

## 主要组成部分

| 路径 | 类型 | 作用 |
|---|---|---|
| `OTHERNESS.md` | 不可变协议 | 七条冻结原则，项目宪法 |
| `MANIFESTO.md` | 不可变 | 项目动机说明 |
| `cards/` | 协议 + 可变状态 | 行为协议卡：`BOOTSTRAP`（初始化）、`WAKE`（唤醒重建）、`TALK`（对话行为）、`STATE`（状态读写）、`MEMORY`（记忆读写）、`RELATIONSHIP`（关系状态）、`SELF`（自我描述）、`DESIRES`（长期兴趣）、`LIFE`（离线生活生成规则） |
| `state/` | 可变状态 | 运行时文件，初始化时生成：`CURRENT`（当前状态）、`RELATIONSHIP`（关系历史）、`OPEN_LOOPS`（未完成事项）、`LAST_WAKE`（上次唤醒时间戳） |
| `memory/` | 记忆文件 | 分为 episodic（事件）、semantic（事实）、self（自我认知）、relationship（关系）、journal（日志） |
| `adapters/` | harness 适配 | 各 harness 的入口文件与安装说明 |
| `docs/` | 文档 | 架构、设计原则、prior art、实验摘要 |
| `tests/` | 测试 | Otherness Benchmark 与 7 个测试场景（原始实验数据不随公开发布） |

几个关键机制：

- **DESIRES**：记录 agent 的长期兴趣和倾向，不由单次对话生成，跨会话读取。
- **OPEN_LOOPS**：记录未完成的问题或事项，作为后续对话或 WAKE 阶段的输入。
- **WAKE**：每次会话开始时执行，读取 `LAST_WAKE` 时间戳和现有状态，对可能的离线经历做有限重建（不是真实执行离线程序）。
- **relationship history**：存储在 `state/RELATIONSHIP` 和 `memory/relationship`，由交互历史累积生成，不能通过指令直接赋值。
- **Lazy Life**：WAKE 阶段生成"离线期间可能发生了什么"的简短叙述，基于已有状态和时间间隔推断，多数情况下输出为"没有特别的事"。

## 安装和使用

1. 把整个文件夹放进一个可写的 agent workspace。
2. 对 agent 说："初始化 But She Could Be"。
3. 开始对话。

初始化过程：agent 读取协议，生成一个刻意简化的初始状态（Thin Self），尝试识别当前 harness 并安装匹配的入口文件；无法识别时进入 Generic Mode，只保证核心协议（cards/、state/ 读写）可用。重复初始化幂等，不会重置已生成的状态。

详细步骤见 `INSTALL.md`。

**依赖条件**：workspace 必须可写。只读环境下 persistence 机制无法工作，初始化会返回失败或降级提示，不会假装成功。

### Harness 支持情况

| Harness | 状态 |
|---|---|
| Codex | 在 R2–R3.4 实验中做过真实行为验证 |
| Claude Code / Cursor | 提供 adapter，未做行为验证 |
| Generic Mode | 无法识别 harness 时的兜底，核心协议可用，跨会话自动加载未验证 |

adapter 文件存在不代表完整验证，见 `LIMITATIONS.md`。

## 实验结果

**R3.2 — 8-session 纵向实验**：同一 base model，单条轨迹，单个 harness，单个 blind judge 打分。protocol arm 得分 26/35，no-protocol baseline 得分 19/35。

样本量为 1，不具有统计意义。post-lock 因果审计显示：多数分差来自行为协议本身（七条原则约束的行为差异），只有较小部分能追溯到已验证的 persistent state（跨会话读写生效的部分）。

**R3.4 — persistence 可靠性测试**：

- raw first-valid targeted reliability：5/9
- 定位问题为 TALK 阶段缺失一次状态载入，修复后重测：9/9

完整数据和方法见 `docs/EXPERIMENTS.md`。

## 已知限制

- **不做后台运行**：离线期间没有程序在真实执行。所谓离线生活是 WAKE 阶段基于已有状态的有限重建，"什么都没发生"是正常输出。
- **persistence 是 best-effort**：跨会话状态传递依赖 agent 实际执行文件读写，取决于模型的指令跟随能力。写回可能被漏掉（R3.4 中观测到过），因为状态是纯文本文件，漏写可以被人工检查和审计，但机制本身不保证一致性。
- **不声称意识或真实情感**：AI 不具有真实意识或情感，"她"是协议和状态文件维持的行为结构，不是一个人。
- **不是首创记忆/自主机制**：不宣称"第一次让 AI 拥有记忆、自主或 Markdown 人格"，相关启发来源见 `docs/PRIOR-ART.md`。
- **Harness 验证范围有限**：只有 Codex 做过真实行为验证，其余 harness 的支持基于 adapter 是否存在，未经行为测试。
- **显式不包含**：server、database、daemon、scheduler、model training、后台 agent runtime。

完整边界说明见 `LIMITATIONS.md`。

## 目录

- `MANIFESTO.md` — 项目动机
- `OTHERNESS.md` — 七条原则全文
- `INSTALL.md` — 安装与各 harness 说明
- `LIMITATIONS.md` — 限制与边界
- `docs/ARCHITECTURE.md` — 分层、数据流、降级策略
- `docs/DESIGN-PRINCIPLES.md` — 工程原则与反模式
- `docs/PRIOR-ART.md` — 启发来源
- `docs/EXPERIMENTS.md` — R2–R3.4 实验摘要
- `CHANGELOG.md` · `RELEASE-NOTES-v0.1.md`
- `tests/OTHERNESS-BENCHMARK.md` — 行为验收标准

## License

MIT License，见 [LICENSE](LICENSE)。
