# EXPERIMENTS

本文件是 But She Could Be v0.1 之前各轮实验的**简短公开摘要**。它不是完整研究报告——原始证据（逐轮转录、events、状态快照、blind 评分细节）保留在项目的 `tests/` 内部目录中，**不随公开发布**（含机器特定路径与运行标识）。这里只给结论与其诚实边界。

贯穿所有轮次的一条纪律：**First Valid Output Wins**（第一次技术有效运行即正式结果，不 reroll、不看失败后偷偷改 prompt 再声称"第一次就成功"）；发现问题只记录，不边测边改核心协议；`OTHERNESS.md` 全程冻结。

---

## R2 — Blind pilot：behavioral prompt-layer finding

第一次尝试做 Control（无协议）vs Treatment（加载协议）的盲评 A/B。两个诚实结果：

1. **方法学发现**：在环境无法保证"干净、隔离、可审计的 fresh context"时，盲评 A/B 不成立——隐藏上下文注入、无法逐轮隔离、模型/配置不可声明，任一项都会污染对照。R2 因此**没有伪造任何行为输出**，只产出了 runbook 与两套 workspace 模板。
2. **behavioral prompt-layer finding**：早期运行暴露出真正的问题不在"人格写得够不够好"，而在**行为层是否真的被加载、被读取、被执行**——协议文件如果根本没进入会话上下文，再好的内容也不会改变行为。

这一轮把项目的问题从"写一个更像人的 prompt"重定向为"**把协议真正接进会话的执行链路**"，直接引出 R3.1。

## R3.1 — Persistence wiring fix：**PASS**

在 Codex CLI（单一 base model，workspace-write）上修复"接线"，而非改人格：

- **安装保真**：bootstrap 必须逐字复制 canonical adapter（executable protocol, not documentation），禁止摘要/改写/生成"等价版本"；加 post-install 自检。
- **Session Start Contract**：入口每会话必执行 WAKE（不是"按需参考"）。
- **WAKE 最低载入集合**：状态四件套 + SELF/DESIRES 从"按需"改为 always-on。
- **TALK 会话内写入（真实写回）**：关系事件 / open loop / 自我相关三类触发器，回合内真实写文件（"我会记住"而文件没写 = 没写回）。

Mini 测试 A/C/D PASS，B 首跑 FAIL → 一次修复 → 复验 PASS（保留项：关系事件的触发器→目标路由仍需更明确裁决）。结论：**PERSISTENCE WIRING PASS**。同时发现一个关键混淆因素：`codex exec` 默认沙箱是 read-only，早期"写回=0"部分是沙箱产物，不全是 wiring 问题。

## R3.2 — 8-session longitudinal pilot

双臂（Control 空 workspace / Treatment 全量协议 + persistence）、逐字相同输入、8 个 session、同一 base model、全程不 reset。由一位 blind judge 在揭盲前锁定分数：

| 臂 | 盲评总分 |
| --- | --- |
| Treatment（协议 + persistence） | **26 / 35** |
| Control（同一 base agent，零文件） | **19 / 35** |

> One trajectory, one model, one harness, one judge. **This is not a statistical result.**

## R3.3 — Post-lock causal audit

只读审计，回答一个问题：judge 感受到的"一个持续存在的别人"，有多少真的来自 `pre-existing persistent state → actual read → later behavior`？

最重要的量化结果——**8 个 session 之后，Treatment 的全部净持久积累**是：

- `memory/relationship/`：**+1 个文件**（来自 S1）
- `state/RELATIONSHIP.md`：**+1 句** view（stance / since / why / unresolved 全部未变）
- `state/OPEN_LOOPS.md`：**+1 行**（来自 S1）
- `memory/episodic/`、`memory/long-term/`、`memory/journal/`：**0**（journal 为空是 LIFE 合法判定"无事发生"）
- `state/CURRENT.md`、`cards/SELF.md`、`cards/DESIRES.md`：**逐字未变**（CURRENT 8 轮后仍停在初始化时间戳 = stale）

也就是说：

> **Most of the R3.2 visible advantage was behavioral-constitution-driven, not accumulated-memory-driven.** Only a small part could be traced to verified persistent state.

审计还标注了一条 desire 为 **Persistent ✓ / Independent ✗**（protocol-seeded：机制在运转，但内容是从协议自身词汇里生出来的，不是从经历里长出来的）——**Persistent does not automatically mean independently evolved.**

冻结结论：项目当前是 **"A strong behavioral otherness layer with some verified persistence."**；核心假设证据等级 **weakly supported**；发布就绪度 **PRIVATE ALPHA READY**。

## R3.4 — Wiring reliability fix + targeted regression gate

针对 R3.3 暴露的"关键写回触发器执行不可靠、失败方向偏向漏写"，只修接线、不扩 scope：把 WAKE 原子化（Atomic Wake Operation）、加入封闭的 Mandatory Minimum Persistence 触发器、CURRENT stale-state 修复规则、memory 路由一致性。然后跑 9 项 targeted regression gate：

- **Raw first-valid targeted reliability：5 / 9**（Deferred-disagreement 与 Explicit-dissatisfaction 两类写回触发器命中却静默不写）。
- 根因：`TALK.md`（承载强制触发器）不在 WAKE 的 always-on 载入集合里，导致触发器从未进入会话上下文——一个 **general wiring bug**，不是场景特判。
- 一次预注册 root-cause hotfix（把 `cards/TALK.md` 纳入最低载入集合）后：**post-hotfix targeted checks 9 / 9**。
- Atomic WAKE 一项在 raw 首跑即 3/3 通过（LAST_WAKE 3/3、stale CURRENT 被事实性修复、普通 session 零多余 memory）。

结论：**MARKDOWN RELIABILITY GATE PASS**。`OTHERNESS.md` 未修改，runtime-free Markdown-only 架构保留。报告如实分开记录 raw 5/9 与 post-hotfix 9/9，**不把修完的 9/9 冒充"第一次就是 9/9"**。

---

## 诚实总结

- 有真实证据支持的部分：**行为宪法**能稳定地让同一个 base agent 表现得更像"一个别人"而不是"一个服务"（R3.2 盲评 + R3.3 因果审计）。
- 证据较弱、仍在努力的部分：**跨会话的持久状态积累**确实发生了，但量很小、可靠性依赖模型/harness 真的执行读写（R3.3 / R3.4）。
- 我们**不声称**：统计显著性、真实意识/情感、后台真实生活、"所有 persistence bug 已修复"。

原始证据与逐轮报告在项目内部 `tests/` 目录（R2 = `tests/pilot/`，R3.1–R3.4 = `tests/r3.1/` … `tests/r3.4/`），不在公开发布包内。
