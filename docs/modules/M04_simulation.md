# M04 — Robot Simulation Foundations

## Module Goal
建立仿真系统的正确认知：Robot Model 如何表示、Physics Engine 在做什么、Sensor/Controller 如何接入，以及 Simulation 能证明什么、不能证明什么。

本模块共 2 个理论 Day（Day20–Day21），不要求此时搭建完整 Gazebo / Isaac 环境。

---

# Day20 — URDF / Robot Model / Simulation Architecture
1. 今日目标：理解机器人在仿真器中的结构、物理与 ROS2 接口。
2. 前置：M00、M01、M03。
3. 必须教学：Link；Joint；URDF/Xacro；robot tree；visual/collision/inertial；mass/CoM/inertia；physics engine；sensor simulation；controller simulation；simulation clock/real-time factor；Gazebo vs Isaac Sim 工具定位。
4. 深度：Robot Model / Simulation Architecture L3；具体工具操作 L1-L2。
5. 工程连接：footprint、collision model、ros2_control、TF、sensor plugin、`/clock`。
6. 不展开：Gazebo/Isaac 安装、USD 内部、physics solver 数值实现。
7. 考核：解释 visual/collision/inertial 为什么分开；URDF 与 TF 的关系；physics engine 负责什么。
8. 毕业考点：Robot representation、collision/inertia、simulation architecture。

# Day21 — Domain Gap / Ground Truth / Experiment Validity
1. 今日目标：理解“仿真成功不等于实机成功”，同时掌握仿真的正确实验价值。
2. 前置：Day20。
3. 必须教学：simulation assumptions；visual/sensor/dynamics/timing domain gap；ground truth；repeatability/determinism；controlled variables；system metrics；simulation validity boundary；domain randomization intro；仿真适合 regression / dangerous edge cases；仿真不能单独证明 real-time、真实安全和 Sim2Real 成功。
4. 深度：Domain Gap / Experiment Reasoning L3；Domain Randomization L1-L2。
5. 工程连接：Nav/MPPI 仿真正常但实机仍可能因 LIO latency、底盘 feedback、footprint、friction 失败。
6. 不展开：完整 Sim2Real、RL training、复杂统计。
7. 考核：给“仿真避障100%成功、实机擦碰”构造 model/sensor/time/control 证据树。
8. 毕业考点：Domain Gap、Ground Truth、Repeatability、Validity Boundary。

---

# M04 Graduation Exam
统一采用课程默认结构；M04题量较轻，但权重仍为：**30%核心基础 / 50%综合系统场景 / 20% Source·Formula·Design**。

## 30% 核心基础
Link/Joint、visual/collision/inertial、URDF/TF relation、physics engine、simulation clock、sensor/controller simulation、Domain Gap、Ground Truth、Repeatability、Validity Boundary。

## 50% 综合系统场景
至少覆盖：
1. 简化 URDF/robot model 中 collision/inertia/TF 错误如何影响下游；
2. 仿真成功但实机失败：从 sensor noise/latency、state estimation、actuator/dynamics、friction、footprint 分析；
3. 设计一个可重复 simulation regression scenario，并说明 controlled variables 与 metrics。

## 20% Source / Formula / Design
能够读一个简化 URDF / launch / simulation config，定位 link/joint、collision/inertial、sensor/controller plugin、clock 与关键参数，并指出哪些设置属于仿真假设。

## 通过标准
总分≥85%；必须理解 **simulation is a model, not reality**；不能只说“仿真和实机不一样”，必须指出具体假设、影响链和验证证据。
