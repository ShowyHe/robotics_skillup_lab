# TEMP_POSITIONING_VISION_PLAN — 导航优先强化 + 定位突破专项学习计划

> 状态：ACTIVE（临时学习主线）
>
> 目的：在**不修改 Curriculum v1.0 的 Module / Day 编号、不修改 03_MASTER_PLAN 总结构**的前提下，按当前能力收益重新安排实际学习顺序：
>
> **先把 Navigation / Planning → Kinematics / Dynamics → Control 做成主攻强项，再集中突破 Localization / State Estimation / SLAM 最大短板。**
>
> 本文件只负责当前阶段的执行顺序，不替代 03_MASTER_PLAN.md、04_MODULE_SPECS.md、modules/、LEARNING_RULES.md 和 PROGRESS.md。

---

## 1. 当前战略

当前能力优先级：

1. **Navigation / Planning：主攻强项，目标 L4→L5**
   - 不停留在“会用 Nav2 / 会调参数”；
   - 要能解释搜索、碰撞、C-space、Hybrid A*、HPA、replan / path switch 的数学与算法依据；
   - 能修改规划决策原则，并从真实现象建立 Planner / Costmap / BT / Controller 证据链。

2. **Localization / State Estimation / SLAM：最大能力门槛，目标 L3→L4**
   - 真正打通 probability / covariance / residual / Jacobian / optimization / SE(3)；
   - 能从 sensor → prediction → measurement → residual → covariance → update → pose 理解定位；
   - 最终能读懂并分析 KF / EKF / LIO / VIO / Factor Graph。

3. **Control：Navigation 的执行闭环，目标 L4；MPPI 关键部分 L5**
   - 从 trajectory / feedback / stability / LQR / MPC 进入 MPPI；
   - 把已有真实 MPPI 工程经验重新映射到数学、模型、cost、constraint、latency 和 feedback。

当前不是平均用力，而是：

~~~text
Navigation：强化优势
Localization：突破上限
Control：连接规划与真实运动
~~~

---

## 2. 已完成基础

当前已完成并通过的专项基础：

~~~text
M02 Day8–15   Mathematical Foundations I
M03 Day16–19  Sensors / Actuators / GNSS / RTK / Time
M05 Day22–26  Vision Geometry
M06 Day27–33  Deep Learning Foundations
M07 Day34–39  Deep Vision / 3D Perception
M08 Day40–42  Probability / Covariance / Likelihood / MLE / MAP
~~~

说明：
- M07 Module Graduation Exam 仍按用户要求 DEFERRED，不宣告 M07 模块毕业；
- M08 Day43 已开始教学，但尚未完成 Quiz / Retest，因此当前标记为 **PAUSED / NOT COMPLETED**；
- M08 Day43–48 会在 M11–M13 完成后继续；
- M04 Simulation 仍不作为当前专项前置主线。

---

## 3. 当前正式执行顺序

~~~text
Stage A — Navigation / Motion / Control Strengthening

M11 Day63–70  Planning & Navigation
→
M12 Day71–79  Robot Kinematics / Dynamics / System Dynamics
→
M13 Day80–89  Control & Optimal Control

Stage B — Localization Mathematical Foundation

M08 Day43–48  Residual / Optimization / Rotation / SO(3) / SE(3)

Stage C — State Estimation

M09 Day49–54  KF / EKF / Multi-sensor Fusion / Observability

Stage D — SLAM / LIO / VIO

M10 Day55–62  ICP / LIO / VIO / Factor Graph / Degeneracy
~~~

课程编号保持原编号，不因为执行顺序变化而重编号。

---

## 4. Stage A1 — M11 Planning & Navigation（当前）

**对应：Day63–Day70**

### 目标
把 Navigation 从已有工程强项提升到 **Graduate-level Planning Theory + Production Navigation Owner**。

### 必须覆盖
- Graph / BFS / Dijkstra；
- A* / admissible / consistency / Weighted A*；
- **Anytime Search（任意时刻搜索）**：ARA* 思想、质量—时间 trade-off；
- Occupancy / Costmap / Footprint / Collision / Inflation；
- Hybrid A* / Motion Primitive / State Lattice；
- **Kinodynamic Planning（动力学约束规划）边界**；
- Configuration Space / RRT / RRT* / OMPL；
- PRM / RRT-Connect 只做概念，正式深化留 Manipulation / OMPL；
- HPA / Dynamic Edge / TTL / Attach；
- **Incremental Search（增量搜索）**：LPA* / D* Lite 核心思想；
- Nav2 / BT / Replanning / Path Switching；
- Dynamic Navigation / Path Quality；
- **Path → Trajectory → Trajectory Optimization bridge**；
- Planning Under Uncertainty 只建立 uncertainty → feasibility / safety margin 边界，Belief-space / POMDP 等留定位完成后深化。

### 深度
- Dijkstra / A*：L4；
- Costmap / Collision：L4–L5；
- Hybrid A*：L4–L5；
- HPA / Dynamic Edge / Path Switching：L4–L5；
- Nav2 responsibility boundary / Owner Debug：L5；
- Anytime / Incremental Search：L3–L4；
- Kinodynamic / Trajectory Optimization bridge：L2–L3。

---

## 5. Stage A2 — M12 Kinematics / Dynamics

**对应：Day71–Day79**

### 主线
~~~text
Configuration / Rigid Body
→ Screw / Twist / Wrench
→ FK / POE
→ Jacobian / Adjoint
→ IK / Singularity
→ Mobile Robot Kinematics
→ Dynamics
→ State-space
~~~

### 当前路线的前置桥
因为 M08 Day46–48 尚未正式学习，而 M12 的 Screw / POE 需要刚体变换基础，所以 **Day71 开始前必须先补一个最小 SE(3) Entry Bridge**：

- Rotation Matrix；
- homogeneous rigid transform；
- transform composition / inverse / frame direction；
- SO(3) / SE(3) 表示是什么；
- skew / hat operator 最小直觉；
- Exp 映射只讲 M12 必需语义。

这个 Entry Bridge：
- **不新增 Day 编号**；
- **不视为 M08 Day46–48 已完成**；
- M08 后续仍正式教学 Rotation / Quaternion / SO(3) / SE(3) / Exp-Log / Pose Perturbation。

### 当前重点
Day76 Mobile Robot Kinematics 必须达到 L4：
- unicycle；
- differential drive；
- body/world velocity；
- nonholonomic constraint；
- curvature / turning radius；
- wheel / chassis feedback；
- 与 Hybrid A* primitive、MPPI rollout 的统一连接。

---

## 6. Stage A3 — M13 Control & Optimal Control

**对应：Day80–Day89**

### 主线
~~~text
Path
→ Timed Trajectory / Reference
→ Feedback / PID
→ Stability
→ Controllability / Observability
→ LQR
→ MPC
→ MPPI
→ Real Robot Feedback / Owner Debug
~~~

### 当前强化
- Day80 正式建立 **Trajectory Optimization 基本形式**：最小化 trajectory cost，同时满足 dynamics / boundary / state-input constraints；
- Day82 的 linear observability 在本模块自包含教学，**不依赖尚未学习的 M09**；
- Day83–84 建立 LQR → MPC；
- Day85–89 将 MPPI 压到 L4→L5：rollout / sampling / weight / update / warm start / critic / constraints / saturation / model mismatch / latency / stale feedback / source mapping。

---

## 7. Stage B — 回补 M08 Day43–48

M11–M13 完成后立即回到数学基础 II：

~~~text
Day43 Residual / LS / WLS / Information
Day44 Nonlinear LS / Newton
Day45 Gauss-Newton / LM / Robust
Day46 Rotation / Euler / Quaternion
Day47 SO(3) / SE(3) / Exp-Log / Perturbation
Day48 Estimation Math Integration
~~~

Hard Gate：

~~~text
Sensor
→ Covariance
→ Residual
→ Weight
→ Jacobian
→ GN / LM
→ SE(3) Update
~~~

只有这条链真正打通，才进入 M09 / M10。

---

## 8. Stage C — M09 State Estimation

**对应：Day49–Day54**

重点：
- state / motion / measurement model；
- Q / R / P；
- Kalman prediction / update；
- innovation / Kalman Gain；
- EKF / Jacobian；
- Wheel / IMU / GNSS / RTK；
- multi-rate / asynchronous fusion；
- bias / dropout / gating / reset；
- observability；
- covariance consistency。

目标不是背公式，而是能够解释：

~~~text
State
→ Prediction
→ Covariance Propagation
→ Measurement
→ Innovation
→ Gain
→ Update
~~~

目标深度：L4。

---

## 9. Stage D — M10 SLAM / LIO / VIO

**对应：Day55–Day62**

主线：

~~~text
SLAM Architecture
→ ICP
→ IMU Propagation
→ LIO
→ VIO
→ Factor Graph
→ Loop Closure
→ Degeneracy / Owner Debug
~~~

核心 Hard Gate：

~~~text
IMU
→ Prediction
→ Point-time Deskew
→ Correspondence
→ Residual
→ Jacobian
→ State Update
~~~

最终要求能从真实 bag / log 判断：
- drift；
- initialization；
- bias；
- degeneracy；
- timestamp / latency；
- calibration / TF；
- sensor dropout；
- estimator vs downstream responsibility。

---

## 10. 每日执行规则

M11–M13 当前阶段严格执行：

~~~text
Day Goal
→ Foundation（基础层）
→ Core Theory（核心理论层）
→ Advanced / Owner（进阶 / Owner层）
→ Daily Quiz
→ targeted remediation / retest
→ PASS 后再写 lesson / progress
~~~

具体规则：
- **Day Goal 与完整覆盖优先；单个 Day 最多 35 个编号教学知识单元**：整合紧密相关的概念，压缩重复叙述，但不删除基础、核心理论、Hard Gate、必要推导或 Owner 连接；
- 一个 Day 必须完成一个完整知识目标，**不拆成两个 Day、不新增 A/B Day、不把后半内容拖到下一 Day**；
- 已在本课程正式学习并 PASS 的知识，才按 Knowledge Reuse Rule 简短 Review；
- 工作中用过、调过、读过源码但未系统学过的基础，仍必须正式覆盖一次；
- 已会的基础允许快速通过，但“快速通过 ≠ 跳过”；
- 每个复杂 Day 都应保持一条主链，禁止为了增加数量引入与当天目标无关的主题；
- 数学必须说明 dimension / frame / assumptions / physical meaning；
- Navigation 强项不因“已经做过项目”跳过理论 Hard Gate；
- 公司项目用于验证理论，不代替理论；
- Localization 不允许只会调 covariance / 参数而解释不清 estimator 数学；
- Control 不允许把 Planner Path、Predicted Trajectory、Controller Command、Actual Motion 混为一谈；
- 未完成的 Day 不写成 COMPLETED / PASS。

---

## 11. 当前学习位置

当前已完成：

~~~text
M11 / Day63 — Graph / BFS / Dijkstra — COMPLETED / PASS
~~~

Day63 已按完整系统基础完成正式讲授、Quiz 与定向复测；完整记录见 `docs/lessons/day063.md`，早先预热版不计为正式通过。

当前进入：

~~~text
M11 / Day64 — A* / Heuristic / Optimality / Anytime Search — IN PROGRESS
~~~

### Day63 Foundation
- Graph / Vertex / Edge；
- Directed / Undirected；
- Weighted / Unweighted；
- Neighbor / Degree；
- Path / Walk / Cycle；
- Reachability / Connectivity；
- Adjacency List / Adjacency Matrix；
- Queue / FIFO；
- BFS；
- Visited / Parent；
- BFS shortest-path 的成立条件。

### Day63 Core Theory
- Weighted Shortest Path；
- Dijkstra；
- `g(n)`；
- Relaxation；
- Priority Queue / Min-Heap；
- stale queue entry；
- nonnegative edge condition；
- shortest-path optimality intuition；
- path reconstruction；
- complexity。

### Day63 Advanced / Owner
- Grid Graph；
- Road Graph；
- Costmap → Edge Cost；
- HPA Abstract Graph；
- Dijkstra → A* → HPA → Global Planner 的统一关系。

以上内容已在同一个 Day63 内完成并通过；新讲义从 Day65 起按每个 Day 最多35个教学知识单元的结构编排，已经 PASS 的旧讲义不追溯修改。

当前专项最终完成标准：

1. Navigation 达到 L4→L5，能修改算法决策与建立 Owner 证据链；
2. Kinematics / Dynamics 能把 path / motion / state / action / physical motion 接起来；
3. MPPI 从工程经验提升到数学 + source + behavior attribution 的 L5；
4. Localization 能从概率、残差、协方差、Jacobian、SE(3) 解释 KF/EKF/LIO/VIO；
5. 对跨模块问题能明确区分 Perception / Localization / Planner / Controller / Actuator 责任。
