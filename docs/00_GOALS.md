# 00_GOALS — 最终目标与学习边界

## 1. 最终目标

本学习体系的最终目标是：

> **成为能够独立负责真实机器人从传感器、感知、状态估计、规划、控制、Manipulation、Robot Learning 到 VLA 部署与数据闭环的机器人全栈 / 具身智能工程师，并具备承担系统 Owner 的能力。**

VLA 是最终上层方向，但不是唯一能力。目标不是“会跑一个 VLA Demo”，而是能够判断：

- 模型为什么在真实机器人上失败；
- 失败来自感知、状态估计、规划、控制、执行还是模型策略；
- 哪一层应该修；
- 怎样修改才不会破坏系统其它部分；
- 怎样通过实验、日志、bag、指标证明修改有效。

## 2. 目标角色画像

最终应具备以下四类能力。

### A. 单模块能力

能独立完成：

- ROS2 节点 / 插件 / Action / Lifecycle / ros2_control 集成；
- Camera / LiDAR / IMU / GPS 等传感器接入；
- 视觉检测、分割、深度、3D 感知；
- SLAM / LIO / VIO / GPS 融合；
- Global / Local Planning；
- PID / LQR / MPC / MPPI 等控制；
- Manipulation / MoveIt2 / IK / grasp pipeline；
- PyTorch 训练、模型导出和 Orin/GPU 部署；
- Imitation Learning / Diffusion Policy / VLA 基本训练和推理链路。

### B. 跨模块系统能力

能够处理跨层问题，例如：

```text
感知误差
  → costmap / world model
  → planner 路径
  → controller clearance
  → 底盘执行
```

以及：

```text
Camera / Language / Robot State
  → VLA / Policy
  → Skill / Trajectory
  → Navigation / Manipulation
  → Controller
  → Hardware
  → Feedback
```

要求能够区分：

- 上游观测错误；
- 模型错误；
- 表示层错误；
- 规划错误；
- 控制错误；
- 执行器限制；
- 时延 / 同步 / 系统调度问题。

### C. Owner 能力

不只是修 bug，还要能：

- 定义模块输入输出和责任边界；
- 设计安全边界和 fallback；
- 选择算法，而不是只接受已有算法；
- 判断参数问题还是架构问题；
- 建立测试矩阵；
- 建立 regression / benchmark；
- 用真实数据做方案取舍；
- 在速度、稳定性、安全、算力之间做工程权衡。

### D. 具身智能能力

最终能够让机器人完成：

```text
自然语言任务
    ↓
场景理解
    ↓
任务分解 / Skill Selection
    ↓
移动到目标区域
    ↓
识别并定位目标
    ↓
操作 / 抓取 / 交互
    ↓
检测执行结果
    ↓
失败恢复 / 重新规划
```

这就是 Mobile Manipulation 与 VLA 真正落地后的系统形态。

## 3. 学习深度定义

后续所有模块统一使用以下等级。

### L0 — 知道

知道术语是什么，但不能解释原理。

### L1 — 会用

能调用库、跑 Demo、修改配置。

### L2 — 会实现 / 会调试

能读关键源码、实现简化版本、通过日志和实验定位问题。

### L3 — 会设计

能修改算法结构、设计 cost / constraint / 状态 / 数据流，并解释为什么。

### L4 — Owner

能跨模块设计方案、安全边界、评测体系和 fallback，并对真实机器人结果负责。

最终要求不是所有领域都达到 L4。

目标结构：

- ROS2 / Robot System：L4
- Navigation / Planning：L4（保持已有优势并继续深化）
- Debug / Evaluation / Deployment：L4
- Perception：L3
- Localization / State Estimation：L3
- Control：L3
- Manipulation：L3
- Deep Learning：L3
- Robot Learning：L3
- VLM / VLA：L3→L4 方向持续发展
- 纯理论数学：达到支撑上述算法设计与论文阅读所需深度，不以数学研究为目标。

## 4. 当前起点（后续需要通过测试校准）

当前计划按以下已知情况设计，但正式课程开始后必须通过测试修正，而不能把这里当成永久结论。

### 已有优势

- 有真实 ROS2 机器人开发和故障排查经验；
- 已接触 Nav2、行为树、全局规划、MPPI、LIO、GPS/RTK 等真实链路；
- 能结合源码、日志和 rosbag 分析系统行为；
- 已有 C++ / ROS2 持续学习基础；
- 已学过 DQN、PPO、TD3 等强化学习内容。

### 明显待补方向

- 数学体系不完整，尤其是微积分、线性代数、概率、数值方法、优化与几何之间的连接；
- 视觉几何、深度视觉、3D 感知体系需要建立；
- Deep Learning 需要从“知道模型”提升到训练、修改、部署；
- 控制理论需要补状态空间、LQR、MPC 等系统知识；
- Manipulation / MoveIt2 / 机械臂运动学需要从基础建立；
- Robot Learning、VLM、VLA 需要建立完整体系；
- 仿真、数据闭环、自动评测和大规模 regression 需要成为长期能力。

## 5. 不采用的学习方式

### 不先完整学完所有数学

原因：大量内容当前没有工程挂点，容易学完后仍不知道如何使用。

### 不直接跳 VLA

原因：如果底层感知、状态、控制、Manipulation 和 Robot Learning 不懂，只能停留在调用模型。

### 不按课程名称机械学习

例如“线性代数 30 天”不是目标。真正的问题是：

> 为了理解 Camera、EKF、SLAM、机械臂和 Transformer，现在需要哪些线代？

### 不以天数替代掌握

Day 结束不等于知识完成。关键前置不过关时插入 Foundation Patch。

## 6. 学习原则

1. **专业目标驱动基础。**
2. **基础必须先于依赖它的专业内容。**
3. **基础只学到当前专业模块足够使用的深度。**
4. **后续专业学习暴露缺口时，再定点补基础。**
5. **每个模块必须形成数学/原理→算法→源码→实验→机器人闭环。**
6. **真实工程证据优先于“听起来合理”。**
7. **所有结论必须能说明依据、适用条件和失败边界。**
8. **已经掌握的内容允许通过考试跳过，不机械重复。**
9. **项目验收优先于课程数量。**
10. **最终进行基础扫尾，补齐未能自然挂靠专业模块但长期重要的知识。**

## 7. 最终毕业能力

完成主学习体系后，应能够独立回答并处理以下问题：

1. 一台新机器人如何从硬件和传感器开始搭建软件栈？
2. 为什么某个传感器观测能或不能用于状态估计？
3. 如何从像素 / 点云得到机器人可使用的世界表示？
4. SLAM / LIO / VIO / GPS 融合为什么会漂、跳、失效？
5. Planner 为什么选这条路？怎样改变其决策原则？
6. Controller 为什么输出这个动作？怎样修改其 cost / constraint？
7. 机械臂怎样从目标位姿变成关节动作？
8. 一个机器人策略怎样从 demonstrations 学出来？
9. Diffusion Policy / ACT / VLA 输出动作的逻辑是什么？
10. VLA 应直接输出低层 action，还是调用传统 Skill？为什么？
11. 机器人失败时如何快速定位责任层？
12. 怎样建立自动评测、失败数据回流和持续训练闭环？

达到这些要求，才视为从“会使用机器人算法”进入“能负责机器人智能系统”。