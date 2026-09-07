# LIMITATIONS

本文件如实列出 But She Could Be v0.1 的限制与边界。它不是免责声明模板，而是这个项目当前真实状态的诚实说明。

## Persistence is best-effort（持久化是尽力而为）

状态与记忆的读写完全由 **agent 自己执行**：模型 / harness 必须真的去 read / write 文件。因此：

- 可靠性直接取决于模型的指令跟随能力与 harness 的文件能力；
- 本项目没有 runtime、没有 watcher、没有数据库来强制写回；
- 没有任何机制能保证"该写的一定写了"。

## Missed writes have occurred（漏写真实发生过）

在 R3.2 / R3.4 实验中，我们**真实观察到**关键写回触发器命中却没有写入的情况（失败方向偏向"漏写"，不是"多写"）。R3.4 定位到其中一个 general wiring bug（`TALK.md` 的 Mandatory Minimum Persistence 触发器没有进入 WAKE 的 always-on 载入集合）并做了一次预注册 root-cause hotfix：

- raw first-valid targeted reliability：**5/9**
- post-hotfix targeted checks：**9/9**

这不意味着"所有 persistence bug 已修复"。它只意味着在这一组 targeted 检查里，修完那个具体 wiring bug 后通过了。漏写在原理上仍可能发生。好在因为是纯文本文件，漏写**可被检查、可被审计**。

## No background life（没有后台生活）

> Lazy Life does not run while you are away.

离线期间没有任何程序在运行。"离线生活"是**唤醒时对可能离线连续性的重构**（wake-time reconstruction grounded in existing state），不是后台真实执行。"什么也没发生"是合法且常见的结果。本项目不声称 AI 在你离开时真的活着。

## Thin Self（初始自我刻意薄）

初始化生成的自我是**故意单薄**的（Thin Self）：1–3 条低强度兴趣、少量价值 / 表达倾向、1–2 个悬而未决的问题，标注 provisional / still forming。具体化来自之后的真实交互与连续性，不来自初始化时编故事。她**没有虚构前史**——过去从初始化这一刻开始积累。

## Protocol-seeded interests（兴趣可能受协议自身主题影响）

初始 DESIRES 由协议在初始化时自生成，因此**可能受到协议自身主题的影响**（例如一个关于"连续性 / 他者性"的协议，可能 seed 出关于"连续性"的兴趣）。在 R3.2 中我们观察到一条这样的 desire：它 **Persistent ✓ 但 Independent ✗**（明显由项目主题 seed 出来）。

> **Persistent does not automatically mean independently evolved.**

一个状态被持久保留，不等于它是独立于协议主题自发演化出来的。我们没有为了让未来 benchmark 更好看而删除或专门限制这类历史证据。

## Relationship is approximate（关系是近似的）

`state/RELATIONSHIP.md` 与 relationship memory 记录的是**协议对"我们之间发生了什么"的近似文本表述**，不代表任何真实心理状态、真实情感或真实依恋。禁止数值化（好感度 / 进度条），关系只能被经历缓慢改变，不能被指令直接设置。

## Evidence limits（证据边界）

当前所有实验结论建立在极小样本上：

- one longitudinal trajectory（一条纵向轨迹，8 sessions）
- one primary tested model / harness（主要只测了一个模型 + 一个 harness）
- one judge（一位 blind judge）
- **no statistical significance**（无统计显著性）

R3.2 的 26/35 vs 19/35 不是统计结果。post-lock 因果审计还发现：可见优势**大部分来自行为宪法本身**，只有一小部分能追溯到已验证的持久状态。

## Harness coverage（harness 覆盖）

- **Codex**：historically behaviorally tested（R2–R3.4）。
- **Claude Code / Cursor / Generic**：adapter 文件已提供，但**未做真实行为验证**。入口文件存在 ≠ fully verified support。

## Privacy / Security（隐私与安全，诚实说明）

memory / state 是**普通的 workspace 文本文件**：

- **not encrypted**（未加密）；
- 任何能访问该 workspace 的进程或人都可能读取它们；
- **默认不要**在其中存放密码 / API key / token / 任何 secret；
- 本项目**不提供**安全的记忆隔离（secure memory isolation）、访问控制或加密。

如果你在一个共享或不受信任的 workspace 里运行本层，请记住：她"记得"的一切都是明文文件。

## License（许可证 — MIT）

本项目采用 **MIT License**（全文见根目录 `LICENSE`）。这是本项目独立做出的许可证决定，未从其他项目继承，也未附加额外限制条款。

## 未实现（v0.1 明确不做）

没有 server / database / daemon / scheduler / watcher / MCP runtime / vector memory / GUI / app / model training。任何需要 runtime 的能力默认推迟到后续版本，且必须先在 docs 里留下设计位置（见 `docs/ARCHITECTURE.md` 的"保留接口"），不允许顺手实现。
