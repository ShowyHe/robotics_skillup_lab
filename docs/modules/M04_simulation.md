# M04 — Robot Simulation Foundations

## Module Goal
建立仿真系统的正确认知：机器人模型如何表示、physics engine 在做什么、sensor/controller 如何接入，以及 simulation 能证明什么、不能证明什么。

本模块只安排 2 个理论 Day，不要求此时花时间搭 Gazebo / Isaac 环境；真正需要仿真时再进入对应专业模块或独立 LAB。

---

## Day20 — URDF、Robot Model 与 Simulation Architecture

### 今日目标
理解机器人在仿真器中的“身体”如何表示，以及 ROS2、physics engine、sensor、controller 如何组成模拟机器人。

### 前置知识
M00、M01、M03。

### 必须教学内容
1. Link：表示刚体部件；必须区分 visual、collision、inertial。
2. Joint：fixed、revolute、continuous、prismatic 的基本意义。
3. Robot Tree：`base_link → joint → link`；与 TF tree 有关联但不是同一概念。
4. URDF：描述 geometry、joint、mass/inertia、collision、sensor mounting 等机器人模型信息。
5. Xacro：用宏与参数减少 URDF 重复；只需理解用途。
6. Visual vs Collision Geometry：外观看起来的模型不一定等于碰撞模型；collision geometry 会直接影响碰撞检测。
7. Inertial：mass、center of mass、inertia；理解其为何影响动力学仿真。
8. Physics Engine：根据 force/torque/constraint 与动力学模型计算 motion；不是简单播放动画。
9. Sensor Simulation：Camera/LiDAR/IMU plugin 生成模拟 measurement。
10. Controller Simulation：ROS command → controller → simulated joint/body → physics engine。
11. Simulation Clock：simulation time ≠ wall time；理解 real-time factor。
12. Gazebo vs Isaac Sim：只建立工具定位；Gazebo 偏传统 ROS / physics 生态，Isaac Sim 在 GPU、视觉、synthetic data、learning 场景更强。

### 深度要求
Robot model L3；simulation architecture L3；Gazebo / Isaac 操作 L1-L2。

### 工程连接
robot footprint、collision model、ros2_control、TF、sensor plugin、`/clock`。

### 明确不展开
Gazebo 环境搭建、Isaac 安装、USD 内部、physics solver 数值实现。

### 本日考核点
1. Link 和 Joint 分别是什么？
2. Visual 和 Collision 为什么分开？
3. Inertial 包含什么？
4. URDF 和 TF 是否完全相同？
5. physics engine 负责什么？
6. sensor plugin 输出是否等于真实 sensor data？
7. simulation time 和 wall time 区别？
8. 为什么 collision model 错误会直接误导导航/避障仿真？

### M04毕业考试核心考点
Robot representation、collision/inertia、simulation architecture。

---

## Day21 — Domain Gap、Ground Truth 与正确的机器人实验思想

### 今日目标
理解“仿真成功不等于实机成功”，同时理解仿真的真正价值：可控、可重复、有 ground truth、适合回归和危险边界测试。

### 前置知识
Day20。

### 必须教学内容
1. Simulation Assumption：friction、mass、inertia、sensor noise、actuator dynamics 等都是模型假设。
2. Domain Gap：仿真世界与真实世界之间的感知、物理、时序和数据分布差异。
3. Sensor Domain Gap：ideal depth/LiDAR、lighting、noise、motion blur、latency 等差异。
4. Dynamics Domain Gap：friction、actuator delay、backlash、compliance、terrain 等差异。
5. Ground Truth：仿真可直接提供 pose/depth/contact 等真实参考，这是评测的重要优势。
6. Determinism / Repeatability：相同 scenario 能反复执行，是 regression / parameter comparison 的基础。
7. Controlled Variables：初步理解 independent variable、dependent metric、controlled variables；完整 Research Methodology 留 M21。
8. Metrics：success rate、path length、collision、tracking error、latency 等。
9. Simulation 能证明：algorithm logic、相对参数效果、regression、edge-case 行为。
10. Simulation 不能单独证明：真实 sensor 可靠性、真实摩擦与执行器行为、real-time 性能、实机安全、sim2real 必成功。
11. Domain Randomization 初步概念：主动随机化仿真参数，减少 policy 对单一模拟世界的过拟合；详细留 M15/M19。

### 深度要求
Domain gap L3；experiment reasoning L3；domain randomization L1-L2。

### 工程连接
必须联系“Nav/MPPI 在仿真正常，但实机仍可能因 LIO latency、底盘 feedback、真实 footprint / friction 不同而失败”。

### 明确不展开
Sim2Real 完整方法、RL training、synthetic dataset pipeline、复杂实验统计。

### 本日考核点
1. Domain gap 是什么？
2. Ground truth 有什么价值？
3. 为什么仿真 LiDAR 可能比实机“太好”？
4. friction 错误会怎样影响 control 实验？
5. repeatability 为什么重要？
6. 一个好的 simulation benchmark 至少应固定/记录什么？
7. 仿真成功是否能证明实机安全？
8. Domain randomization 试图解决什么？

---

# M04 Graduation Exam Specification

M04 采用较轻量毕业考，不为了形式增加额外实验。

## A. Robot Model题 — 40%
给一份简化 robot model / URDF 结构，要求判断 link/joint、visual/collision、inertial、TF 关系、controller 链，并指出错误建模会影响哪些后续算法。

## B. Simulation Validity场景 — 60%
例如：

> 仿真避障 100% 成功，但实机频繁擦碰。

要求从 footprint/collision geometry、sensor noise/latency、state estimation、actuator/dynamics、friction 等方面分析哪些仿真假设可能不成立，并说明需要什么证据验证。

## Knowledge Coverage Matrix
- Link / Joint：必考
- Visual / Collision：**核心必考**
- Inertial：必考
- URDF / TF relation：必考
- Physics engine role：必考
- Simulation clock：理解题
- Sensor / Controller simulation：必考
- Domain Gap：**核心必考**
- Ground Truth / Repeatability：必考
- Simulation validity boundary：**核心必考**
- Domain Randomization：理解题

## 通过标准
- 总分 ≥85%；
- 必须理解“simulation is a model, not reality”；
- 不能只说“仿真和实机不一样”，而要能指出具体模型假设、可能影响与验证证据。
