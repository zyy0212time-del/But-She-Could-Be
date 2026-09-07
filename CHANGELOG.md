# CHANGELOG

本文件记录 But She Could Be 的版本变化。人类可读，不追求严格的 Keep-a-Changelog 结构。

机器可读版本号（若需要）：`0.1.0`；对外表述：`v0.1`。本项目 v0.1 不为此新建 `package.json`（没有代码、没有依赖、没有构建步骤）。

## v0.1 — Experimental

首个公开的实验性版本。它是一个 runtime-free、Markdown-only 的行为层（otherness layer），**不是稳定产品**。

### 包含（Added）

- **Behavioral constitution**：`OTHERNESS.md` 七条冻结原则 + `MANIFESTO.md`。
- **Thin Self**：初始化生成的刻意单薄的自我（`cards/SELF.md`），无虚构前史，标注 provisional / still forming。
- **DESIRES**：她自己的、可与用户完全无关的兴趣与悬而未决的问题（`cards/DESIRES.md`）。
- **Memory**：episodic / semantic / self / relationship / journal 五类，第一人称、按日期、带主观注脚（`cards/MEMORY.md` + `memory/`）。
- **Relationship state**：关系是状态不是配置，初始 newly acquainted / insufficient history，禁止数值化（`cards/RELATIONSHIP.md` + `state/RELATIONSHIP.md`）。
- **WAKE**：唤醒事务——最低载入集合（always-on）+ 取时间 + 时差判定 + LAST_WAKE 写回 + CURRENT 时效修复（`cards/WAKE.md`）。
- **Lazy Life**：唤醒时对离线连续性的惰性补算（不是后台运行）；"无事发生"是合法输出（`cards/LIFE.md`）。
- **Persistence triggers**：会话内真实写回触发器，含封闭的 Mandatory Minimum Persistence（deferred disagreement / explicit dissatisfaction）+ tie-breaker（`cards/TALK.md`）。
- **Adapters**：Codex / Claude Code / Cursor 入口 + Generic Mode 兜底（`adapters/`），逐字安装保真 + 幂等标记块。
- **Bootstrap**：无问卷、无配置表的初始化，含只读环境诚实降级（`cards/BOOTSTRAP.md`）。
- **Benchmark**：Otherness Benchmark v0.1（Experiment A/B 设计 + 6 维 rubric + hard failure signals + 防过拟合规则）（`tests/OTHERNESS-BENCHMARK.md` + 7 个场景）。
- **Experimental validation**：R2–R3.4 实验（摘要见 `docs/EXPERIMENTS.md`）。

### 改进（Changed / Improved）

- **Improved persistence wiring reliability in targeted Codex tests**：R3.4 定位并修复了一个 general wiring bug（`cards/TALK.md` 的强制触发器未进入 WAKE 的 always-on 载入集合），raw targeted reliability **5/9** → 一次预注册 root-cause hotfix 后 post-hotfix **9/9**。
- WAKE 表达为 **Atomic Wake Operation**（读取 + 取时间 + 写回 LAST_WAKE + CURRENT 时效修复为一个事务，不得部分执行）。
- 四个 adapter 的 session-start 最低载入集合**统一**为与 `cards/WAKE.md` 一致（含 TALK），消除 SELF/DESIRES "always-on" 与 "optional" 并存的矛盾。
- Memory 路由一致性：relationship memory ≠ user profile；relationship memory 用第一人称结构（date / event / my_read / possible_future_effect）。
- LAST_WAKE / CURRENT 卫生规则：统一时间戳格式（ISO 8601 含时区偏移，禁 BOM / 多余空行 / 无 offset / 字段不一致）、stale CURRENT 事实性修复（只修过时事实，不顺便演化人格）。

### 已知限制（Known）

- Persistence 是 **best-effort**，依赖模型 / harness 真的执行读写；漏写在原理上仍可能发生。本轮**不声称** "fixed all persistence bugs"。
- Lazy Life 不是后台真实运行。
- 仅 **Codex** 经过真实行为验证；其他 adapter 为 provided，not behaviorally validated here。
- **LICENSE**：MIT License（见根目录 `LICENSE`）；此前的 release blocker 已解除。

完整限制见 `LIMITATIONS.md`；实验证据边界见 `docs/EXPERIMENTS.md`。
