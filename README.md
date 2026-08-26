# robotics_skillup_lab

面向 **机器人全栈工程 + 具身智能（Embodied AI）+ VLA** 的长期学习与能力升级仓库。

本仓库不再以单一 Nav2 / ROS2 / 论文复现为主线，而是服务于一个更高层目标：

> 从已有的 ROS2、导航与真实机器人调试能力出发，补齐数学、感知、状态估计、控制、Manipulation、Deep Learning、Robot Learning、VLM/VLA 等能力，最终形成能够把具身智能模型真正落到真实机器人上的全栈能力。

## 1. 最终定位

目标岗位不是单纯的：

- Nav2 调参工程师；
- 单一视觉算法工程师；
- 只训练大模型的纯 AI 工程师；
- 只做论文推导、不负责机器人落地的研究角色。

目标是：

**Robot Full-stack / Embodied AI Engineer**

进一步目标：

**VLA + Mobile Manipulation 方向的系统 Owner。**

最终应能贯通：

```text
Sensors / Hardware
        ↓
ROS2 / Linux / Runtime
        ↓
Perception / World Model
        ↓
Localization / State Estimation
        ↓
Planning / Navigation
        ↓
Control
        ↓
Manipulation
        ↓
Robot Learning
        ↓
VLM / VLA
        ↓
Real-Robot Deployment / Evaluation / Data Flywheel
```

## 2. 核心学习方法

本仓库采用 **“专业目标反推基础”**，而不是先把大学基础课全部学完。

流程固定为：

```text
确定专业能力
    ↓
反推必须前置的数学 / CS / 物理基础
    ↓
只学当前真正需要的基础
    ↓
立即进入专业算法 / 源码 / 实验
    ↓
暴露新的基础缺口
    ↓
Foundation Patch 定点补课
    ↓
回到专业主线
```

例如：

- 学 LIO 前补 Bayes、Gaussian、Jacobian、SE(3)、Least Squares；
- 学 MPC / MPPI 前补状态空间、梯度、约束优化；
- 学机械臂前补 SE(3)、Jacobian、运动学；
- 学 Transformer / VLA 前补链式法则、概率、优化、Attention 所需线代；
- 剩余无法直接挂靠专业模块的基础内容，在主线完成后统一扫尾。

## 3. 学习不以“看完”为完成

每个模块最终都必须达到至少一个可验证结果：

- 能解释完整输入→处理→输出链路；
- 能读关键源码；
- 能实现简化版本；
- 能完成仿真或实机实验；
- 能定位失败原因；
- 能设计修改方案；
- 能用指标验证修改是否有效。

关键前置知识没有掌握时，不因为 Day 数到了就自动进入下一阶段。

## 4. 第一层总纲文件

当前先建立三份约束后续所有课程的文件：

1. [`docs/00_GOALS.md`](docs/00_GOALS.md)  
   定义最终角色、能力边界、学习原则和最终验收标准。
2. [`docs/01_COMPETENCY_MAP.md`](docs/01_COMPETENCY_MAP.md)  
   定义机器人全栈能力树，以及各方向需要达到的深度。
3. [`docs/02_DEPENDENCIES.md`](docs/02_DEPENDENCIES.md)  
   从专业模块反推基础知识依赖，决定以后课程顺序。

只有这三层确认后，才继续生成：

- `docs/03_MASTER_PLAN.md`：模块级总课程；
- `docs/LEARNING_RULES.md`：GPT 教学与考核规则；
- `docs/PROGRESS.md`：当前进度、薄弱点、补课记录；
- `docs/modules/`：每个 Module 的 Day 级详细课程。

## 5. 后续课程的固定粒度

正式 Day 计划不会只写标题。每一天至少要定义：

1. 今日目标；
2. 前置基础；
3. 必须掌握的核心点（不超过 20 个）；
4. 数学 / 原理；
5. 算法或系统机制；
6. 源码 / 论文入口；
7. 代码或实验；
8. 真实机器人对应关系；
9. 理解题 / 故障题；
10. 通过标准；
11. 今日明确不展开的内容。

## 6. 总原则

- **不为完整而学习无关基础。**
- **不为速度而跳过关键前置。**
- **不把会调用 API 当作掌握算法。**
- **不把能解释概念当作能负责模块。**
- **所有重要知识最终必须连接真实机器人。**
- **导航是已有优势，不是最终边界。**
- **VLA 是上层能力，不是替代感知、定位、规划、控制和执行的魔法模块。**

本仓库后续所有课程与项目，都必须服从这套目标和依赖关系。