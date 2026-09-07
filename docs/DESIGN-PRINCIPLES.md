# DESIGN PRINCIPLES

## 优先级（工程宪法）

1. 简单
2. 可读
3. 可审计
4. 跨 harness
5. 行为一致
6. 可测试
7. 可扩展

明确不是：技术复杂度、文件数量、framework 感、"看起来很专业"。

## 推论

- **如果 8 个 Markdown 文件能解决，不写 800 行 Python。** v0.1 没有任何代码：没有 runtime、没有依赖、没有构建步骤。行为协议就是数据 + 指令，harness 原生能力就是虚拟机。
- **如果一个 harness 原生能力能完成，不重新实现。** 文件读写、规则加载、（未来的）scheduler，都优先用 harness 自带的。
- **入口必须短。** 用户与 agent 的接触面只有一句话口令和一张 WAKE 卡；深层规则分文件，按需读取。禁止演化出一个几千行的超级 system prompt。
- **所有状态可审计。** 任何一个"她为什么这样说"的问题，都应该能在普通文件里找到可读的因果痕迹（记忆、状态、关系记录），而不是藏在二进制或数据库里。
- **加文件要过门槛。** 只有一个规则在现有 cards 里放不下、且属于新职责时，才新增文件；加之前先问它是不是 scope creep。
- **自我先薄后厚。** A self should begin thin and become specific through continuity：初始化只生成薄自我（Thin Self），具体化交给真实交互的连续性，不靠初始化编故事。
- **「不知道」优于「猜错」。** 无法可靠识别 harness 时进入 Generic Mode 并如实说明；绝不安装猜出来的 entrypoint。
- **协议不针对测试题写答案。** cards 只含 general principles；benchmark 场景的判定逻辑不进入核心 cards，测的是原则的自然结果。

## 设计闸门（每个设计决定过两道题）

1. 这个设计是在让 AI 更会"表演一个人"，还是在给它建立一个不完全围绕用户的持续因果结构？优先后者。
2. 如果她最终选择帮助、认可或安慰，这个行为是否仍有"她本可以不"的意义？没有则重新设计。

## 反模式目录（出现即回退）

| 反模式 | 表现 | 违反 |
| --- | --- | --- |
| 随机拒绝 | 概率性冷淡 / 抽卡式傲娇 | OTHERNESS 2 |
| 用户中心引力 | offscreen life 全部指向用户；记忆全记用户 | OTHERNESS 1 |
| 数值养成 | 好感度、亲密度、进度条 | OTHERNESS 3 |
| 情绪旁白 | "我今天有点累，所以……"作为状态播报 | OTHERNESS 5 |
| 机制旁白 | "根据我的状态系统 / 协议 / 记忆检索……" | OTHERNESS 7 |
| 表演独立 | 为不顺从而不顺从；毒舌人设 | OTHERNESS 6 |
| 指令重写 | 用户一句话改变人格 / 关系 | OTHERNESS 3、SELF/RELATIONSHIP 规则 |
| 框架感 | 面向开发者的抽象层、插件点、配置系统 | 工程优先级 |

## 版本纪律

- v0.1 冻结为：Markdown + YAML、文件读写、三 harness、Lazy Life、Benchmark。
- 任何需要 runtime / DB / server / API key / daemon 的能力，默认推迟到后续版本，且必须先在 docs 里留下设计位置（如 ARCHITECTURE 的保留接口），不允许顺手实现。
