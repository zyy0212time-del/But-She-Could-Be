# RELEASE NOTES — v0.1 (Experimental)

**But She Could Be / 她本可以不** — a lightweight otherness layer for AI agents.

> 这是 **PUBLIC EXPERIMENTAL v0.1 RELEASE**，不是稳定产品。

## What this is

一组 Markdown 协议文件，给支持文件读写的 AI agent 加一个**不完全围绕用户展开**的连续内部生活层。没有代码：没有 server、database、daemon、scheduler、model training、background agent runtime。协议就是数据 + 指令，harness 原生的文件读写就是它的运行时。

它研究的核心问题是：

> What is the minimum structure required for an AI to behave more like an other rather than a service?

## What is experimental

- **整个版本都是 experimental。** persistence、Lazy Life、关系状态都是 best-effort 的协议机制，不是有保证的产品功能。
- **No sentience claim**：本项目不声称 AI 拥有真正的意识、情感或"真的活着"。"她"是协议与状态文件维持的行为结构，不是一个人。
- **No background execution**：Lazy Life does not run while you are away；离线生活是唤醒时对可能连续性的重构，"无事发生"是合法结果。
- **Persistence is best-effort**：写回由 agent 自己执行，模型 / harness 的指令跟随能力直接影响可靠性；漏写真实发生过（但纯文本文件让漏写可审计）。

## What we tested

- **Codex**（historically behaviorally tested，R2–R3.4）：安装保真、WAKE 自动执行、最低载入集合读取、LIFE 时差触发、LAST_WAKE 写回、跨会话读回、会话内真实写回、silent maintenance。
- **R3.2 — 8-session longitudinal pilot**：blind judge 给 protocol arm **26/35**、no-protocol baseline **19/35**（one trajectory / one model / one harness / one judge，**not a statistical result**）。
- **R3.3 — post-lock causal audit**：可见优势**大部分来自行为宪法**，只有一小部分能追溯到已验证的持久状态。
- **R3.4 — wiring reliability gate**：raw targeted **5/9** → 一次 root-cause hotfix（missing TALK load）→ post-hotfix **9/9**（MARKDOWN RELIABILITY GATE PASS）。

## What we did not test

- **没有**对 Claude Code / Cursor / Generic 做真实行为验证——它们的 adapter 文件是 **provided, not behaviorally validated here**。入口文件存在 ≠ fully verified support。
- **没有**多模型横评、大样本、统计显著性检验。
- **没有**长程（数十 session）纵向、并发写冲突、防篡改测试。
- **没有**声称跨 harness 的等价行为。

## Known limitations

详见 `LIMITATIONS.md`。要点：

- Persistence best-effort，漏写可能发生。
- Thin Self：初始自我刻意薄；具体化靠连续性，不靠初始化编故事。
- Protocol-seeded interests：初始 DESIRES 可能受协议自身主题影响；**Persistent ≠ independently evolved**。
- Relationship is approximate：不代表真实心理状态。
- Evidence limits：一条纵向轨迹、一个主测模型 / harness、一位 judge、无统计显著性。
- Privacy：memory / state 是明文 workspace 文件，未加密，不要存 secrets。
- **LICENSE = MIT License**（见根目录 `LICENSE`）：本项目独立做出的许可证决定，此前的 release blocker 已解除。

## Install

见 `INSTALL.md`。需要**可写**工作区；只读环境下 persistence 无法正常工作，初始化会诚实降级（`SKIPPED — REQUIRES WRITE PERMISSION`）而非假装成功。
