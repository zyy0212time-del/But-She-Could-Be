# adapters/

三个 harness 的入口文件（源文件）与安装说明。入口文件都很薄：只负责让 harness 在每次会话开始时加载 `OTHERNESS.md` 与 `cards/WAKE.md`，全部行为逻辑都在 cards 里。

| Harness | 源文件 | 安装位置（工作区根目录） | 自动加载 |
| --- | --- | --- | --- |
| Codex | `codex/AGENTS.md` | `AGENTS.md` | 是（Codex 自动读取根目录 AGENTS.md） |
| Claude Code | `claude-code/CLAUDE.md` | `CLAUDE.md` | 是（Claude Code 自动读取项目根 CLAUDE.md） |
| Cursor | `cursor/bscb.mdc` | `.cursor/rules/but-she-could-be.mdc` | 是（alwaysApply 规则） |
| Unknown（Generic Mode） | `generic/ENTRY.md` | `BSCB-ENTRY.md` 或用户确认存在的指令文件位置 | **未验证**（本层不伪装兼容） |

## 行为验证状态（诚实声明）

- **Codex**：在 R2–R3.4 实验中经过真实行为验证（historically behaviorally tested）。R4 本轮不再调用 Codex。
- **Claude Code / Cursor / Generic**：adapter 文件已提供，但**本项目未对其做真实行为验证**（provided, not behaviorally validated here）。入口文件存在 ≠ fully verified support。

## Known / Unknown harness（“不知道”优于“猜错”）

- **Known harness**：只有在有充分依据确认是 Codex / Claude Code / Cursor 之一时，才安装对应 native entrypoint。
- **Unknown harness（Generic Mode）**：无法可靠识别时——不猜、不创建错误的 `AGENTS.md` / `CLAUDE.md` / Cursor rule；创建 `BSCB-ENTRY.md`（基于 `generic/ENTRY.md`），并告知用户：core protocol 可以运行；automatic future-session loading 未被验证。

## 安装方式

- **自动**：对 agent 说「初始化 But She Could Be」，`cards/BOOTSTRAP.md` 第 5 步会安装对应入口（含 `$ROOT` 路径替换、BSCB 标记块、幂等检查）。
- **手动**：复制上表“源文件 → 安装位置”，保持 `<!-- BSCB:BEGIN -->` / `<!-- BSCB:END -->` 标记块包裹，然后把文件内的 `$ROOT` 占位符改为项目文件夹相对路径（项目文件夹就是工作区根时为 `./`）。

## 幂等安装（重复 initialize 安全）

- 所有入口内容包裹在 `<!-- BSCB:BEGIN -->` / `<!-- BSCB:END -->` 标记块中；
- 标记块已存在 → 只更新块内内容，不重复追加；
- 目标文件已有其他内容且无标记块 → 末尾追加标记块，不覆盖原内容；
- mutable state（SELF / DESIRES / state / memory）已存在 → 默认保留；
- 只有用户明确要求 reset / fresh initialization 时才重建人格与关系状态；
- 禁止创建多个相互冲突的入口副本或规则块。这是幂等协议，不是 DRM。

## 上下文加载审计（每个 harness 自动加载什么）

- **自动加载的只有薄入口**（约 40–55 行）：Codex → `AGENTS.md`；Claude Code → `CLAUDE.md`；Cursor → alwaysApply 的 `bscb.mdc`。
- 入口内含：加载顺序、OTHERNESS 六条核心不可违反项速览、唤醒指令。**核心 cards 不被入口复制**，按需读取。
- OTHERNESS 的关键不可违反项因“速览常驻”而不依赖按需读取；完整宪法仅在需要完整依据时读全文。
- Codex 等对项目指令聚合上下文有限制——入口短小是刻意设计，不是遗漏。

## 限制

- 项目文件夹**嵌套**在工作区子目录时，入口必须装到工作区根目录才被自动加载；装好后初始化仍由用户口令触发。
- 已有其他内容的 `AGENTS.md` / `CLAUDE.md`：追加 BSCB 标记块，不要覆盖。
- 工作区根不可写：跳过安装，本会话仍可通过用户口令触发唤醒（降级模式，见 `docs/ARCHITECTURE.md`）。
- 其余 harness（Gemini CLI / OpenClaw / SillyTavern 等）为未来预留，v0.1 不提供，也不为其增加复杂度；未知环境一律走 Generic Mode，不猜不装。
