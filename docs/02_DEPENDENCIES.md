# 02_DEPENDENCIES — 专业目标反推基础依赖图

## 1. 这份文件解决什么问题

本仓库不采用：

```text
高数 → 线代 → 概率 → 优化 → 机器人
```

这种长串行路线。

采用：

> **先明确要学的专业模块，再反推它真正需要的基础；先补够这些基础，再立即进入专业内容。**

因此，后续课程顺序必须服从依赖关系，而不是服从传统大学课程顺序。

---

# 2. 总依赖图

```text
Robot System / ROS2 / Linux / C++
              │
              ├───────────────┐
              │               │
              ▼               ▼
      Vision Geometry     Sensors / Timing
              │               │
              ▼               │
      Deep Perception          │
              │               │
              ├───────┐       │
              ▼       ▼       ▼
        World Model   State Estimation / SLAM
              │          │
              └────┬─────┘
                   ▼
          Planning / Navigation
                   │
                   ▼
                Control
                   │
        ┌──────────┴──────────┐
        ▼                     ▼
 Manipulation          Real-Robot Skills
        │                     │
        └──────────┬──────────┘
                   ▼
            Robot Learning
                   │
                   ▼
            VLM / VLA
                   │
                   ▼
     Mobile Manipulation / Embodied AI

贯穿全部：Math / Simulation / Data / Evaluation / Deployment
```

---

# 3. Robot System / ROS2 前置依赖

## 专业目标

达到能够设计和调试真实机器人软件架构的水平。

## 先补基础

### CS / Linux

- process / thread；
- memory 基本模型；
- file descriptor；
- socket / IPC；
- scheduler；
- concurrency；
- mutex / condition variable。

### C++

- object lifetime；
- pointer / reference；
- RAII；
- smart pointer；
- lambda；
- template 基础；
- STL；
- thread / lock。

## 然后进入

- ROS2 Executor；
- Callback Group；
- DDS / QoS；
- lifecycle；
- composition；
- pluginlib；
- ros2_control；
- latency / watchdog / degraded mode。

## 可后补

- OS 内核细节；
- lock-free 深入；
- realtime scheduling 理论深化。

---

# 4. Vision Geometry 前置依赖

## 专业目标

理解“像素如何对应真实世界”。

## 先补基础

### 线代

- vector；
- matrix multiplication；
- coordinate transformation；
- inverse；
- projection。

### 数学

- 三角函数；
- 相似三角形；
- 基础微积分概念。

## 然后进入

- pinhole camera；
- intrinsic / extrinsic；
- distortion；
- calibration；
- projection / back-projection；
- stereo；
- RGB-D；
- Camera→base_link TF。

## 可后补

- projective geometry 深入；
- epipolar geometry 严格推导。

---

# 5. Deep Learning / Deep Perception 前置依赖

## 专业目标

能理解、训练、修改和部署视觉模型。

## 先补基础

### 微积分

- derivative；
- partial derivative；
- chain rule；
- gradient。

### 线代

- vector / matrix；
- dot product；
- matrix multiplication；
- tensor 的线代直觉。

### 概率

- probability；
- expectation；
- distribution 基础；
- likelihood 基本概念。

## 然后进入

```text
PyTorch
→ Autograd
→ Training Loop
→ CNN
→ Detection
→ Segmentation
→ Depth
→ Feature Representation
→ BEV / Occupancy
→ ONNX / TensorRT
```

## Transformer 前额外补

- embedding；
- softmax；
- dot-product similarity；
- probability distribution；
- matrix batch operation。

## 可后补

- information theory；
- optimization convergence 严格理论；
- statistical learning theory。

---

# 6. State Estimation / SLAM 前置依赖

## 专业目标

真正理解机器人“位置从哪里来、为什么可信、为什么会漂”。

## 先补基础

### 概率

- Gaussian；
- expectation；
- variance / covariance；
- conditional probability；
- Bayes；
- MLE / MAP。

### 微积分

- derivative；
- partial derivative；
- Jacobian；
- first-order approximation。

### 线代

- matrix；
- covariance matrix；
- inverse；
- eigenvalue 基本直觉。

### 几何

- rotation matrix；
- quaternion；
- homogeneous transform；
- SO(3) / SE(3) 工程级理解。

### 优化

- least squares；
- nonlinear least squares；
- Gauss-Newton 基础。

## 然后进入

```text
Kalman Filter
→ EKF
→ IMU propagation
→ ICP / scan matching
→ LiDAR Odometry
→ VIO / LIO
→ Pose Graph / Factor Graph
→ GNSS / RTK Fusion
```

## 后续 Foundation Patch

如果进入 FAST-LIO / VIO 源码时遇到：

- Lie algebra；
- error-state；
- manifold update；
- observability；

再专项补，不在前期一次学完。

---

# 7. Planning / Navigation 前置依赖

## 专业目标

从“会使用 Planner”提升到“能设计规划决策”。

## 先补基础

### CS

- data structure；
- graph；
- queue / priority queue；
- search；
- time complexity 基础。

### 数学 / 几何

- Euclidean distance；
- coordinate geometry；
- angle；
- collision geometry；
- cost function。

### 优化基础

- objective；
- constraint；
- local/global optimum 基本概念。

## 然后进入

```text
Dijkstra
→ A*
→ heuristic design
→ footprint / collision checking
→ Hybrid A*
→ State Lattice
→ RRT / RRT*
→ HPA / Hierarchical Planning
→ Trajectory Optimization
→ Behavior / BT
```

## 后补

- computational geometry 深入；
- advanced motion planning theory；
- optimization-based planner 深层数学。

---

# 8. Control 前置依赖

## 专业目标

从“调 controller 参数”提升到“能设计控制目标、状态和约束”。

## 先补基础

### 微积分

- derivative / integral；
- differential equation 基础。

### 线代

- matrix multiplication；
- state vector；
- quadratic form。

### 系统基础

- state；
- input；
- output；
- feedback；
- dynamic model。

### 优化

- cost function；
- gradient；
- constraint；
- numerical optimization；
- sampling 基础。

## 然后进入

```text
PID
→ State Space
→ Stability 基本概念
→ LQR
→ MPC
→ Sampling-based Control
→ MPPI
```

## MPPI 前专项补

- Gaussian sampling；
- Monte Carlo；
- importance weighting；
- exponential weighting；
- stochastic optimization 基本直觉。

## 后补

- Lyapunov 严格证明；
- nonlinear control 深入；
- robust control。

---

# 9. Manipulation 前置依赖

## 专业目标

让机器人不仅“会走”，还“会操作”。

## 先补基础

### 几何

- rotation / transformation；
- SE(3)；
- frame chain。

### 微积分 / 线代

- derivative；
- Jacobian；
- matrix rank；
- inverse / pseudo-inverse 基本概念。

### 力学

- force；
- torque；
- velocity / acceleration；
- kinematics / dynamics 区别。

## 然后进入

```text
Joint / Link
→ Forward Kinematics
→ Inverse Kinematics
→ Jacobian
→ Singularity
→ Configuration Space
→ OMPL / MoveIt2
→ Grasp Pose
→ Trajectory Execution
→ Force / Impedance Basics
```

## 再进入 Mobile Manipulation

```text
Navigation
+ Perception
+ Object Pose
+ Arm Planning
+ Grasp
+ Task Recovery
```

---

# 10. Robot Learning 前置依赖

## 专业目标

理解机器人如何从 demonstrations / interaction 数据学习 action policy。

## 必须先完成

- Deep Learning 基础；
- Transformer 基础；
- 概率；
- optimization；
- robot state / action；
- kinematics / control 基础。

## 再补

### MDP

- state；
- action；
- transition；
- reward；
- policy；
- value。

### Learning from Demonstrations

- dataset distribution；
- supervised policy learning；
- covariate shift 基本概念。

## 然后进入

```text
Behavior Cloning
→ Imitation Learning
→ Sequence Action Prediction
→ ACT
→ Diffusion Policy
→ Offline RL
→ Policy Evaluation
```

RL 不是从零开始重学，而是在已有 DQN / PPO / TD3 经验上重新组织为机器人学习体系。

---

# 11. VLM / VLA 前置依赖

## 专业目标

最终能理解并落地：

```text
Vision + Language + Robot State → Action
```

## VLM 前必须有

- vision encoder 基础；
- embedding；
- Attention；
- Transformer；
- token / sequence；
- multimodal representation 基础。

## VLA 前必须有

- VLM 基础；
- Robot Learning；
- action representation；
- robot state；
- kinematics / control；
- perception；
- navigation / manipulation 至少一个真实执行链；
- dataset / training / evaluation 基础。

## 然后进入

- RT 系列思想；
- OpenVLA 类架构；
- action tokenization；
- continuous action head；
- action chunking；
- temporal context；
- embodiment adaptation；
- fine-tuning；
- inference / latency；
- safety / fallback。

## 最终系统问题

必须能够比较：

### 架构 A

```text
VLA → low-level action
```

### 架构 B

```text
VLM / VLA
   ↓
Skill / Subgoal
   ↓
Navigation / MoveIt / Controller
```

并根据任务、数据量、实时性和安全要求决定用哪一种，而不是把“端到端”当作默认答案。

---

# 12. Simulation / Data / Deployment 的依赖位置

这些不是最后才学，而是随着模块插入。

### 第一次视觉模型

同时学习：

- dataset；
- label；
- train / val；
- inference metrics。

### 第一次状态估计

同时学习：

- bag replay；
- timestamp；
- trajectory evaluation。

### 第一次规划控制

同时学习：

- benchmark scenario；
- collision / success / latency metrics。

### 第一次 Robot Learning

同时学习：

- trajectory dataset；
- demonstration quality；
- offline evaluation。

### 第一次 VLA

同时学习：

- GPU deployment；
- latency；
- safety wrapper；
- failure data collection。

---

# 13. Foundation Patch 规则

当专业学习遇到以下情况时，暂停当前小节并插入补课：

1. 公式中的符号看得懂，但不知道为什么这样算；
2. 源码出现 Jacobian / covariance / matrix update，却只能机械跟代码；
3. 能调参数但解释不了参数为什么影响行为；
4. 无法从数学模型预测参数变化趋势；
5. 论文关键推导完全依赖记结论；
6. 实验失败时无法建立变量之间的因果假设。

Foundation Patch 一般控制在 1~3 Day；特别大的基础缺口再升级成独立 Mini-Module。

---

# 14. 建议的主模块顺序

根据上述依赖，后续 `03_MASTER_PLAN.md` 应按大致顺序展开：

```text
M00 全栈系统地图 / 基线测试
M01 C++ + Linux + ROS2 系统基础补强
M02 视觉几何前置基础 + Camera / OpenCV
M03 DL 前置基础 + PyTorch / CNN / Detection / Segmentation
M04 3D 感知 / Depth / PointCloud / BEV / World Model
M05 状态估计前置基础 + KF / EKF / SLAM / LIO / Fusion
M06 规划基础补强 + A* / Hybrid A* / HPA / Trajectory Planning
M07 控制基础补强 + PID / LQR / MPC / MPPI
M08 Manipulation 前置基础 + FK / IK / MoveIt2 / Grasp
M09 Deep Learning 深化 + Transformer / Multimodal
M10 Robot Learning + BC / ACT / Diffusion Policy
M11 VLM / VLA
M12 Mobile Manipulation / Embodied AI 全栈项目
M13 基础扫尾 + 系统化补全
```

具体天数暂不在这里决定，避免在能力依赖尚未审核完时过早锁定 Day 数。

---

# 15. 最后统一扫尾的基础

以下内容如果前面没有自然触发，不应丢掉，但可以最后统一补：

- 数值稳定性 / conditioning 深化；
- convex optimization 更系统的理论；
- probability statistics 更完整体系；
- information theory 基础；
- algorithm complexity 深化；
- operating system 深层机制；
- computer network 深层机制；
- rigid-body dynamics 深化；
- control stability 理论深化；
- GPU architecture / CUDA 深化；
- software architecture / distributed system 补充。

原则：**先保证专业主链不被不必要的基础课阻塞，再补齐高级工程师长期应该掌握的基础。**