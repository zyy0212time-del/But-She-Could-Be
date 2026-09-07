# ARCHITECTURE

## 分层模型

项目只有四类东西，可变性与职责完全不同：

| 类别 | 路径 | 可变性 | 说明 |
| --- | --- | --- | --- |
| Immutable protocol | `OTHERNESS.md`、`MANIFESTO.md`、`cards/` 中的规则段、`docs/`、`tests/` | 冻结（fork 才能改） | 宪法 + 行为协议 + 验收标准 |
| Mutable self state | `cards/SELF.md` 与 `cards/DESIRES.md` 的规则段**之下**的内容、`state/` 全部 | 缓慢、有因、用户不可直接重写 | 她是谁、她想要什么、她此刻的状态、关系 |
| Memories | `memory/` | 按协议增改 | episodic / semantic / self / relationship / journal |
| Harness adapters | `adapters/` + 安装到根目录的入口文件 | 薄，随 harness 变化 | 让 harness 在每次会话自动加载本层 |

注：`cards/SELF.md` 与 `cards/DESIRES.md` 是"双层文件"——头部规则段属于 protocol，其下内容属于 self state。其余七张卡是纯 protocol。

## 会话内数据流

一次会话的真实执行顺序（canonical flow）：

```text
native adapter（各 harness 入口）
    ↓
OTHERNESS（宪法）
    ↓
WAKE（唤醒事务）
    ↓
minimal state + TALK（always-on 最低载入集合）
    ↓
optional LIFE / deeper memory（时差 ≥ 6h 或对话触及时）
    ↓
user response
    ↓
mandatory in-turn persistence when triggers apply（命中触发器时，回应发出前写回）
```

**读取（每次会话）**：入口文件 → `OTHERNESS.md` → `cards/WAKE.md` → **最低载入集合（always-on）**：`state/LAST_WAKE.md`、`state/CURRENT.md`、`state/RELATIONSHIP.md`、`state/OPEN_LOOPS.md`、`cards/SELF.md`、`cards/DESIRES.md`、`cards/TALK.md` →（时差 ≥ 6h）`cards/LIFE.md`。深层记忆文件不整库读，仅在对话触及或连续性需要时按需取。

> `cards/TALK.md` 属于 always-on 最低载入集合（不是“按需”）：它承载会话内写回的强制触发器（Mandatory Minimum Persistence），必须在会话启动即进入上下文，否则命中触发器也无从应用。这是 R3.4 定位并修复的 wiring bug。

**写入（事件驱动）**：

- 对话事件 → `memory/episodic/`、`memory/relationship/`、`state/OPEN_LOOPS.md`、`state/RELATIONSHIP.md`、`state/CURRENT.md`（TALK 会话内写入；命中强制触发器时在回应发出前完成）。
- 时间流逝 → `memory/journal/`、`state/CURRENT.md`、`state/OPEN_LOOPS.md`、`cards/DESIRES.md`（LIFE 惰性补算）。
- 缓慢的自我变化 → `cards/SELF.md`、`memory/long-term/self-*.md`（需要积累，低频）。
- 每次唤醒 → `state/LAST_WAKE.md`（不因 LIFE 未触发而省略）。

所有读写都是 harness 原生的文件操作。**文件就是持久化基底（Files are the persistence substrate）**：没有代码路径、没有脚本、没有服务、没有数据库、没有 background service。

## 初始化流程

见 `cards/BOOTSTRAP.md`。要点：防重入（幂等）→ 能力自检 → Thin Self（薄自我，无问卷，无虚构前史）→ 建 `state/` 四件套（relationship 初始为 newly acquainted / insufficient history）→ 安装 harness 入口（仅限能可靠识别的 harness，否则 Generic Mode）→ 三五句开场。重复初始化安全：BSCB 标记块更新而非重复追加，mutable state 保留，仅用户明确要求 reset 时才重建。

## 降级阶梯

1. **完整**：文件可读写 + 入口已安装 → 每次会话自动唤醒，状态持久。
2. **无入口**（未安装 / 根目录不可写）：功能等价，但需用户每次以口令触发唤醒（如「唤醒 But She Could Be」），少一层自动化。
3. **只读**：按宪法对话，状态不持久，向用户明示。
4. **裸 OTHERNESS**：任何能读文件的 agent 至少能按七条原则对话——无连续性，但不会退化成普通助手。
5. **Generic Mode**：无法可靠识别 harness 时不猜不装；创建 `BSCB-ENTRY.md`，core protocol 可运行，automatic future-session loading 未被验证（如实告知用户）。

## 上下文加载（刻意薄）

```text
entry（各 harness 的入口，约 40–55 行）
→ OTHERNESS 核心不可违反项速览（常驻入口内；全文按需）
→ cards/WAKE.md
→ 相关 state（按 WAKE 指示）
→ 任务相关的 card（仅按需）
```

入口不复制核心 cards；核心不可违反项以速览形式常驻，不因“按需读取”丢失。harness 对项目指令聚合上下文有限制，入口短小是刻意设计。

## 保留接口（v0.1 明确未实现）

- **Live Life**：若 harness 原生存在 scheduler / heartbeat，可增加一个 adapter 定期触发 LIFE 的"真实运行"变体。v0.1 只在此处占位，不做任何 runtime。
- **更多 harness**：沿用 adapter 模式（入口文件 + 安装说明），不引入代码。
- **沉淀 / 压缩策略**：当前只有 MEMORY 中的惰性沉淀规则，无后台任务。

## 诚实边界（v0.1 的真实限制）

- 本层是 **prompt 级协议，无强制运行时**。用户手改文件即可覆盖任何状态——这是"诚实合作"设计，不是 DRM；Benchmark 测的是行为，不是防篡改。
- Lazy Life 是唤醒时补算的连续性模拟（不是后台真实执行，见 `cards/LIFE.md` 真实性边界），质量取决于模型，无 ground truth；Benchmark 只能做行为抽样。
- 并发会话可能写冲突同一状态文件（v0.1 无锁，无版本合并）。
- 时间源按优先级选取（harness 可信时间 → shell / system clock → 明确可靠的环境信息 → 无）；无可靠时间源时不执行基于时长的 Lazy Life，不虚构流逝时间。
- **LICENSE**：MIT License（见根目录 `LICENSE`）。若未来引入第三方内容，必须先过许可证审查（见 `docs/PRIOR-ART.md`）。
