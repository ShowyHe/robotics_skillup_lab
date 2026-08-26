# 02_DEPENDENCIES — 专业模块反推基础依赖图

## 1. 核心原则

本课程不采用“先完整学完所有基础，再进入机器人”的长串行路线。

采用：

> **确定专业模块 → 反推其真正需要的数学 / CS / 物理基础 → 先补这些基础 → 立即进入专业理论 → 后续暴露缺口再定点补。**

但“按需学习”不代表降低深度：关键基础最终仍需达到机器人硕士核心课程可用水平。

---

# 2. M00–M22 总依赖图

```text
M00 Robot Full-stack Architecture
   ↓
M01 C++ / Linux / ROS2 Systems
   ↓
M02 Mathematical Foundations I
   ├──────────────→ M05 Vision Geometry
   │                    ↓
   │                M07 Deep Vision & 3D Perception
   │                    ↑
   ├──────────────→ M06 Deep Learning Foundations
   │                    ↓
   │                M15 Robot Learning
   │                    ↓
   │                M17 VLA
   │
   ├──────────────→ M08 Mathematical Foundations II
   │                    ↓
   │                M09 State Estimation
   │                    ↓
   │                M10 SLAM / LIO / VIO
   │
   └──────────────→ M12 Kinematics / Dynamics
                        ↓
                    M13 Control
                        ↓
                    M14 Manipulation

M03 Sensors & Actuators
   ├→ M05 Vision Geometry
   ├→ M09 State Estimation
   └→ M10 SLAM / LIO / VIO

M04 Simulation Foundations
   ├→ M07 Perception
   ├→ M11 Planning
   ├→ M13 Control
   ├→ M14 Manipulation
   └→ M15 Robot Learning

M11 Planning & Navigation ─────────────┐
M14 Manipulation ─────────────────────┤
M16 VLM ───────────────→ M17 VLA ────┤
M17 VLA ──────────────────────────────┤
                                       ↓
                             M18 Mobile Manipulation
                                       ↓
                    M19 Deployment / Data / Sim2Real
                                       ↓
                    M20 Safety / Reliability / Owner
                                       ↓
                    M21 Research Capstone
                                       ↓
                    M22 Foundation Cleanup
```

---

# 3. 共享基础链

## 3.1 线代 / 微积分链

```text
Vector / Matrix
→ Coordinate Transform
→ Derivative / Partial Derivative
→ Chain Rule
→ Gradient / Jacobian
→ Taylor Linearization
```

服务于：

- Camera Geometry；
- Backpropagation；
- EKF；
- ICP / SLAM；
- IK；
- Dynamics；
- LQR / MPC；
- Optimization。

**Jacobian 只正式学一次，后续在 EKF / IK / SLAM / Control 中不断复用和加深。**

## 3.2 概率链

```text
Random Variable
→ Distribution
→ Gaussian
→ Conditional Probability / Bayes
→ Expectation / Variance / Covariance
→ MLE / MAP
```

服务于：

- KF / EKF；
- Sensor Fusion；
- SLAM；
- Robot Learning；
- uncertainty reasoning。

## 3.3 几何链

```text
Rotation Matrix / Quaternion
→ Homogeneous Transform
→ SO(3) / SE(3)
→ Lie perturbation intuition
```

共同服务：

- LIO / VIO；
- Manipulation；
- Robot State Representation；
- VLA action / pose interpretation。

## 3.4 优化链

```text
Least Squares
→ Weighted / Nonlinear Least Squares
→ Gradient / Newton family
→ Gauss-Newton / LM
→ Constrained Optimization
```

共同服务：

- ICP / SLAM；
- IK；
- trajectory optimization；
- MPC；
- Deep Learning；
- Robot Learning。

---

# 4. M01 C++ / Linux / ROS2 Systems

## 先补

- object lifetime / RAII；
- pointer / smart pointer；
- STL / lambda / template 基础；
- process / thread；
- mutex / condition variable；
- memory / file descriptor；
- socket / IPC；
- scheduler 基础。

## 然后进入

- DDS / QoS；
- Executor / Callback Group；
- TF；
- Lifecycle；
- Component；
- pluginlib；
- ros2_control；
- tracing / diagnostics / latency。

## 后补

- OS kernel 深入；
- lock-free 深入；
- realtime scheduling 理论深化。

---

# 5. M05 Vision Geometry

## 依赖

来自 M02：

- vector / matrix；
- matrix multiplication；
- inverse；
- coordinate transformation；
- projection；
- basic calculus。

来自 M03：

- Camera measurement；
- intrinsic / extrinsic 概念；
- timestamp / calibration。

## 然后进入

- pinhole；
- projection / back-projection；
- distortion；
- calibration；
- Stereo / RGB-D；
- pixel → camera → base/world。

---

# 6. M06 Deep Learning Foundations

## 依赖

来自 M02：

- vector / matrix；
- partial derivative；
- chain rule；
- gradient。

额外补：

- probability distribution 基础；
- softmax / cross entropy；
- optimization intuition。

## 然后进入

- PyTorch；
- Autograd；
- Backpropagation；
- CNN；
- Attention；
- Transformer foundations。

**M06 必须先于 M07 Deep Vision。**

---

# 7. M07 Deep Vision & 3D Perception

依赖：

- M05 Vision Geometry；
- M06 Deep Learning Foundations；
- M03 Sensor basics。

然后进入：

- detection / YOLO；
- segmentation；
- depth；
- point cloud；
- voxel；
- 3D detection；
- BEV / occupancy；
- robot-consumable world representation。

---

# 8. M08 Mathematical Foundations II

M08 是进入高级估计、SLAM、控制、Manipulation 的第二个数学门槛。

## 先掌握

- Gaussian / Bayes；
- covariance；
- MLE / MAP；
- least squares；
- nonlinear least squares；
- GN / LM；
- SO(3) / SE(3)；
- numerical conditioning 基础。

然后分流到：

- M09 State Estimation；
- M10 SLAM；
- M12 Kinematics / Dynamics；
- M13 Control。

---

# 9. M09 State Estimation

依赖：

- M03 Sensors；
- M08 Probability / Optimization；
- M02 Jacobian / Taylor。

核心链：

```text
State / Motion Model
→ Process Noise
→ Prediction
→ Measurement Model
→ Measurement Noise
→ Innovation
→ Kalman Gain
→ State / Covariance Update
```

之后进入 M10 LIO / VIO。

---

# 10. M10 SLAM / LIO / VIO / Factor Graph

依赖：

- M05 Vision Geometry；
- M08 SE(3) / Optimization；
- M09 State Estimation；
- M03 IMU / LiDAR / Camera / timing。

基础对应：

```text
Least Squares / GN → ICP / Graph Optimization
SE(3)              → Pose / Perturbation
Jacobian            → Residual Linearization
Probability         → Noise / Information
State Estimation    → IMU Propagation / Fusion
```

---

# 11. M11 Planning & Navigation

依赖：

- graph / tree；
- queue / priority queue；
- complexity；
- geometry；
- footprint / collision representation。

顺序：

```text
Dijkstra
→ A*
→ Hybrid A*
→ Sampling / RRT
→ HPA
→ BT / Replanning
→ Trajectory Optimization Concepts
```

源码规则：

```text
真实工程问题
→ 公司实现
→ 算法基础
→ Nav2 官方实现
→ 必要最小自实现
→ 回真实系统验证
```

Planning 不要求先学完整 Dynamics，因此放在 M12 前是合理的。

---

# 12. M12 Kinematics / Dynamics / System Dynamics

依赖：

- M02 calculus / matrix；
- M08 SE(3) / optimization；
- basic mechanics。

核心链：

```text
Rigid Transform
→ FK
→ IK
→ Jacobian
→ Velocity / Singularity
→ Newton-Euler / Lagrange
→ Robot Dynamics
→ State-space / Linearization
```

M12 必须先于 M13 Control 和 M14 Manipulation。

---

# 13. M13 Control & Optimal Control

依赖：

- M12 system dynamics；
- M02 differential / matrix；
- M08 optimization。

```text
Feedback / PID
→ State-space
→ Stability / Controllability
→ LQR
→ MPC
→ MPPI
```

其中：

- Differential equation / state-space → PID / LQR / MPC；
- quadratic form / eigenvalue → LQR；
- constrained optimization → MPC；
- probability / sampling / cost → MPPI。

源码主线同样以公司真实实现 + Nav2 官方实现为主。

---

# 14. M14 Manipulation

依赖：

- M05 / M07 perception；
- M12 FK / IK / Jacobian / dynamics；
- M13 control。

然后进入：

- MoveIt2；
- Planning Scene；
- OMPL；
- grasp planning；
- trajectory execution；
- force / impedance control；
- visual manipulation transform chain。

---

# 15. M15 Robot Learning

依赖：

- M06 Deep Learning；
- M12 robot state / action；
- M13 control；
- M14 manipulation context。

核心链：

```text
MDP / Policy
→ Behavior Cloning
→ Imitation Learning
→ Covariate Shift
→ ACT / Action Chunking
→ Diffusion Policy
→ Offline / Real-world Learning
```

重点是“动作如何从机器人数据中学出来”，不是重新刷完整 RL 算法全集。

---

# 16. M16 VLM

依赖：

- M05 / M07 Vision；
- M06 Transformer。

核心解决：

> **机器人怎样看懂场景、语言和语义关系。**

暂不承担连续低层 action 输出。

---

# 17. M17 VLA

依赖：

- M15 Robot Learning；
- M16 VLM；
- M12 robot state/action；
- M13 controller understanding；
- M14 manipulation context。

核心解决：

```text
Vision + Language + Robot State
→ Multimodal Representation
→ Policy
→ Action Representation / Chunk
→ Controller / Skill Interface
```

VLA 必须追到真实 action 的物理和控制意义。

---

# 18. M18 Mobile Manipulation

依赖：

- M11 Navigation；
- M14 Manipulation；
- M17 VLA；
- Recovery / Behavior concepts。

组合：

```text
Instruction
→ Task / Skill Decomposition
→ Search / Navigation
→ Re-observation / Base Alignment
→ Grasp / Manipulation
→ Execution Feedback
→ Recovery
```

---

# 19. M19–M22 后段能力

## M19 Deployment / Data / Evaluation / Sim2Real

依赖前面主要算法模块，形成：

```text
Data Collection
→ Dataset
→ Training
→ Deployment
→ Evaluation
→ Failure Mining
→ Data Reflow
→ Retraining
```

## M20 Safety / Reliability / Owner

建立跨模块：

- watchdog；
- freshness；
- fallback；
- degraded mode；
- uncertainty；
- collision safety；
- human takeover；
- responsibility boundary；
- evidence chain；
- regression。

注意：基础安全和测试意识从 M00 起贯穿，M20 是综合提升，不是第一次接触。

## M21 Research Capstone

需要主体课程能力基本形成后再做：

- problem；
- baseline；
- hypothesis；
- experiment；
- metrics；
- ablation；
- failure analysis；
- reproducibility；
- report。

## M22 Foundation Cleanup

根据全过程 `PROGRESS.md` 的 Foundation Debt 定向补齐：

- 数值分析；
- 凸优化；
- 概率深化；
- OS / Network；
- Dynamics；
- Control；
- Algorithm Theory；
- 其它真实暴露的理论缺口。

不预先固定教材顺序。