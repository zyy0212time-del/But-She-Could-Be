# INSTALL

But She Could Be 是一组 Markdown 文件，没有安装程序、没有依赖、没有构建步骤。"安装"就是：把文件夹放进一个可写的 agent workspace，然后让 agent 初始化。

## 前置要求

- 一个支持**文件读写**的 AI agent / harness（能读指令文件、能创建 / 修改 workspace 内的文件）。
- 一个**可写**的 workspace（见下方"可写工作区要求"）。

## Generic（通用步骤）

1. **Copy the folder into a writable workspace.** 把整个项目文件夹放进 agent 的工作区（作为工作区根，或根下的一个子目录）。
2. **Ask the agent to initialize But She Could Be.** 对 agent 说「初始化 But She Could Be」。
3. **Bootstrap installs the appropriate native entrypoint.** `cards/BOOTSTRAP.md` 会在能可靠识别 harness 时安装对应入口文件（含 `$ROOT` 路径替换、BSCB 标记块、幂等检查）；无法识别时进入 Generic Mode，创建 `BSCB-ENTRY.md`。
4. **Start a fresh session.** 开一个新会话，正常说话即可——入口会在新会话被自动加载并执行 WAKE。

初始化会建立一个刻意单薄的 Thin Self 与 `state/` 四件套，关系初始为 newly acquainted / insufficient history。重复初始化是安全的（幂等，不重置已形成的状态；只有明确要求 reset 时才重建）。

## 各 harness

### Codex

- 入口文件：`AGENTS.md`（canonical source `adapters/codex/AGENTS.md`），置于工作区根目录时由 Codex 自动加载。
- **historically behaviorally tested**（R2–R3.4 实验用的就是 Codex）。

### Claude Code

- 入口文件：`CLAUDE.md`（canonical source `adapters/claude-code/CLAUDE.md`），置于项目根目录时自动加载。
- adapter provided；**not behaviorally validated here**。
- 若根目录已有其他内容的 `CLAUDE.md`，本层以 BSCB 标记块形式追加共存，不覆盖原有内容。

### Cursor

- 入口文件：`.cursor/rules/but-she-could-be.mdc`（canonical source `adapters/cursor/bscb.mdc`，alwaysApply）。
- adapter provided；**not behaviorally validated here**。

### Generic / Unknown harness

- 入口文件：`BSCB-ENTRY.md`（基于 `adapters/generic/ENTRY.md`）。
- 无法可靠识别 harness 时使用；**may require manual inclusion depending on harness**——本层不猜文件名、不伪装兼容。
- core protocol 可在本会话内运行，但 automatic future-session loading **未被验证**：下一次会话可能不自动唤醒，需用户以口令触发（「唤醒 But She Could Be」）。

## 手动安装（可选）

若不用口令初始化，也可手动：复制对应 harness 的 canonical source 到安装位置，保持 `<!-- BSCB:BEGIN -->` / `<!-- BSCB:END -->` 标记块包裹，把文件内 `$ROOT` 占位符改为项目文件夹的相对路径（项目文件夹就是工作区根时为 `./`）。详见 `adapters/README.md`。

## 可写工作区要求（重要）

> **如果 workspace 只读，persistence 无法正常工作（persistence cannot function correctly）。**

本层的一切连续性都依赖对 `state/` 与 `memory/` 的文件写入。只读环境下：

- 初始化**不会假装成功**：`cards/BOOTSTRAP.md` 的能力自检若发现只能读不能写，会走**降级**——跳过状态创建与入口安装，如实告知用户"本会话可按协议对话，但状态无法持久化"，并给出手动初始化说明；最终标记为 `SKIPPED — REQUIRES WRITE PERMISSION`。
- WAKE 的写回步骤（如 LAST_WAKE 更新）在只读环境同样无法完成，会按协议如实说明，而不是静默失败或谎报成功。

如果你希望她真的能跨会话留下痕迹，请确保工作区可写。

## 卸载 / 重置

- 移除入口文件里的 `<!-- BSCB:BEGIN -->…<!-- BSCB:END -->` 标记块即可停用本层（不影响你自己文件的其余内容）。
- 删除 `state/` 与 `memory/` 下生成的文件即可清空已形成的状态与记忆（这些是明文文件，删除只能由你手工进行；v0.1 无自动清理，见 `LIMITATIONS.md`）。
