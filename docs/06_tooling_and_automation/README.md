# 06｜评测科学、性能分析与研究实验纪律

## 定位

本目录虽然名称仍为 `tooling_and_automation`，但实际内容不再是工具脚本集合，而是把前面已有的工程评估能力，正式抬升为研究级评测与实验纪律。

目标不是“多写几个自动化脚本”，而是系统建立：

- benchmark 设计
- 指标体系
- regression / robustness / stress / fault injection
- profiling / tracing / timing / resource metrics
- reproducibility
- ablation / negative results
- claim-evidence map 的前置能力

## 为什么这个模块必须存在

当前已经有：

- 基线评估
- 最小 A/B 对照
- 三地图 runs
- live vs replay
- failure case 文档

这些说明已经具备工程评测的雏形。  
但如果不进一步升级，就会卡在：

- 能做评估，但不够严谨
- 能做对照，但不够系统
- 能看 failure case，但没有方法论
- 改了系统，却不能正式证明它是好是坏
- 做出来的实验难以支撑更高层的研究工作

所以本模块的任务，就是把现有“工程化评估意识”正式抬升为“研究级评测与实验纪律”。

## 模块目标

完成本模块后，应逐步具备以下能力：

- 能定义 benchmark 和指标体系，而不是只记录 success / timeout
- 能设计对照、回归、鲁棒性、压力测试
- 能系统分析 failure，而不是只积累 case
- 能用 profiling、tracing、timing、resource metrics 看系统
- 能解释“为什么这个实验可信”
- 能把结论与证据对应起来，为后续论文工作做桥

## 模块主要产物

本模块后续应重点沉淀：

- benchmark / metric 体系
- regression / robustness / stress / fault injection 模板
- profiling / tracing / timing / resource metrics 基础框架
- 自动化实验执行骨架
- claim-evidence map 前置模板
- 模块总结
- 模块总验收
- 证据索引

## 学习与执行方式

本模块采用 Sprint 机制推进，每个 Sprint 按以下节奏执行：

1. 问题定义与实验图谱建立  
2. 运行证据、结果证据和 performance 证据采集  
3. benchmark、metric、trace、failure science 相关资料深读  
4. 实验、对照、回归、profiling、fault injection 等动作  
5. 总结、闭卷与复盘  

每天必须保留：

- 今天的实验设计或评测问题
- 今天采集的指标或性能证据
- 今天的对照、回归、failure、profiling 动作
- 今天最容易混淆的概念
- 当天闭卷问答
- 明日衔接点

## 验收标准

完成本模块时，至少应满足以下条件：

- 能定义一类 benchmark 与指标集
- 能设计回归、鲁棒性、压力测试
- 能做 profiling / tracing 基础分析
- 能用实验和证据而不是主观感觉判断改动价值
- 能把 failure case 升级成系统性 failure science
- 能通过闭卷稳定表达实验纪律与评测逻辑

## 与后续模块的关系

本模块是后面研究层的关键桥梁：

- 为 `07–12` 的文献矩阵、baseline 复现、benchmark、ablation、原创方法和论文叙事提供实验纪律底盘
- 没有这一层，后面很多所谓“研究”都会停留在看起来很像研究的工程自嗨