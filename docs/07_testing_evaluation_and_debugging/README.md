# 07｜复杂机器人系统的评测科学、故障科学与性能调试

## 定位

本目录虽然名称仍为 `testing_evaluation_and_debugging`，但实际内容不再是“会测一点、会排障一点”的工程目录，而是把前面已经具备的评估、failure case、bag 回放和结果记录能力，正式升级成一套方法学。

目标不是继续积累零散 case，而是回答下面这些更深层的问题：

- 一个复杂机器人系统，到底应该如何被测试、比较和验证
- failure 应该如何分类，而不是只在文档里一个个堆着
- 调试为什么不能停留在经验救火，而必须进入“现象 → 证据 → 根因 → 修复 → 验证”的链路
- 为什么“能跑起来”和“可复现、可比较、可证明”不是一回事
- profiling、tracing、timing、resource metrics 为什么决定高阶系统工作的质量

## 为什么这个模块必须存在

当前已经具备：

- 最小 A/B 对照
- 三地图泛化 runs
- live vs replay 对比
- failure case 记录
- 结果表、日志、评估文档

这说明已经不是不会做评估，而是需要把“工程化评估意识”升级成真正的：

- 评测科学
- 故障科学
- 性能分析方法
- 高置信度调试与验证链路

如果这一步不补齐，后面做源码改造、插件扩展、baseline 复现和原创方法时，很容易陷入：

- 改了东西，但不知道到底变好了还是变坏了
- 有结果，但没有可信度
- 有 failure case，但没有一般化结论
- 有测试动作，但没有测试方法学

## 模块目标

完成本模块后，应逐步具备以下能力：

- 能系统区分功能测试、回归测试、鲁棒性测试、压力测试、性能测试
- 能建立一套 failure taxonomy，而不是散点式失败记录
- 能把 debugging 升级成方法学，而不是经验式排错
- 能用 profiling、tracing、timing、resource metrics 支撑判断
- 能设计 regression / stress / robustness / fault injection 等高级验证动作
- 能说明“为什么这个实验可信、为什么这个结论成立”

## 模块主要产物

本模块后续应重点沉淀：

- 测试类型总图
- failure taxonomy
- debugging as science 方法框架
- 回归 / 压力 / 鲁棒性 / 性能测试模板
- profiling / tracing / timing / resource metrics 基础框架
- 模块总结
- 模块总验收
- 证据索引

## 学习与执行方式

本模块采用 Sprint 机制推进，每个 Sprint 按以下节奏执行：

1. 问题定义与评测图谱建立  
2. 日志、trace、bag、result 等证据采集  
3. 文档、源码、实验设计与性能分析资料深读  
4. regression、fault injection、profiling、对照等动作  
5. 总结、闭卷与复盘  

每天必须保留：

- 今天的核心评测或故障问题
- 今天抓到的系统证据
- 今天的测试动作或性能分析动作
- 今天最容易混淆的概念
- 当天闭卷问答
- 明日衔接点

## 验收标准

完成本模块时，至少应满足以下条件：

- 能用测试类型而不是“随便跑跑”来组织验证
- 能把 failure case 上升成 failure taxonomy
- 能把现象和根因之间建立证据链，而不是靠直觉
- 能初步完成 profiling / tracing / timing / resource analysis
- 能用结构化方式说明“为什么这次改动有效”或“为什么这次改动失败”
- 能通过闭卷稳定表达评测与调试方法学

## 与后续模块的关系

本模块是后面高阶模块的重要桥梁：

- 为 `08｜大代码库阅读方法与架构重建` 提供“如何从 failure 追到实现层”的视角
- 为 `09｜高级插件体系与系统定制化设计` 提供“插件改动后如何做严谨验证”的框架
- 为 `10｜文献矩阵、baseline 地图、论文复现与 benchmark 建立` 提供 benchmark / regression / reproducibility 底盘
- 为 `11｜原创方法、实验叙事与论文管线` 提供实验可信度基础