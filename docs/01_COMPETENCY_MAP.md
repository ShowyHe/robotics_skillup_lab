# 01_COMPETENCY_MAP — 机器人全栈 / 具身智能能力树

## 1. 使用方法

这份文件不是课程表，而是后续所有课程的“能力边界”。

后续每个 Module、Day、项目都必须能映射到这里的一个或多个能力节点；无法映射的内容原则上不进入主学习路线。

等级统一采用：

- L0：知道术语；
- L1：会调用、会跑 Demo；
- L2：会实现、会调试；
- L3：会设计算法 / 系统；
- L4：能作为 Owner 跨模块负责结果。

最终目标不是所有节点都 L4，而是形成 **“系统/导航/调试部署为强项，感知/定位/控制/Manipulation/Robot Learning/VLA 达到可独立设计和落地”** 的 T 型全栈结构。

---

# 2. A — Robot System / Software Stack

**目标：L4**

## A1. Linux

- process / thread；
- scheduling；
- memory；
- file / socket / IPC；
- signal；
- systemd；
- realtime 基础；
- performance profiling。

## A2. C++

- object lifetime / RAII；
- pointer / smart pointer；
- move / copy；
- template；
- STL；
- lambda；
- thread / mutex / condition variable；
- shared library / ABI；
- CMake；
- debugging / sanitizer / profiler。

## A3. ROS2

- Node / Topic / Service / Action；
- TF2；
- QoS；
- Executor / Callback Group；
- DDS；
- Lifecycle；
- Component / Composition；
- pluginlib；
- ros2_control；
- launch / parameter；
- bag / tracing / diagnostics。

## A4. 系统架构

- 模块责任边界；
- synchronous / asynchronous pipeline；
- freshness / watchdog；
- fallback；
- state machine / behavior tree；
- latency budget；
- safety boundary；
- degraded mode。

---

# 3. B — Math / Physics Foundation

**目标：支撑 L3/L4 工程与算法设计，不追求纯数学研究。**

## B1. 微积分

- function / derivative；
- partial derivative；
- chain rule；
- gradient；
- Jacobian；
- Hessian 基本概念；
- integral；
- differential equation 基础。

## B2. 线性代数

- vector / matrix；
- dot product / norm / projection；
- linear transformation；
- rank；
- eigenvalue / eigenvector；
- quadratic form；
- SVD；
- covariance matrix。

## B3. 概率统计

- random variable；
- distribution；
- Gaussian；
- expectation / variance / covariance；
- conditional probability；
- Bayes；
- MLE / MAP；
- sampling；
- uncertainty。

## B4. 数值与优化

- numerical integration；
- numerical error；
- conditioning 基础；
- least squares；
- gradient descent；
- Newton / Gauss-Newton；
- constrained optimization；
- convexity 基础；
- stochastic optimization。

## B5. 机器人几何

- 2D/3D coordinate frames；
- rotation matrix；
- quaternion；
- homogeneous transformation；
- SO(2) / SO(3)；
- SE(2) / SE(3)；
- Lie group / Lie algebra 的工程级理解。

## B6. 力学与控制基础

- velocity / acceleration；
- force / torque；
- kinematics / dynamics；
- state-space；
- feedback / stability。

---

# 4. C — Sensors / Hardware / Robot Interface

**目标：L2~L3**

- Camera / RGB-D；
- LiDAR；
- IMU；
- Encoder / Wheel Odom；
- GNSS / RTK；
- timestamp / synchronization；
- extrinsic calibration；
- CAN / Serial / Ethernet 基础；
- motor / actuator 基础；
- hardware interface；
- sensor failure / stale data / dropout。

目标不是成为硬件电路专家，而是能判断机器人数据从物理世界进入软件系统时发生了什么。

---

# 5. D — Perception / World Model

**目标：L3**

## D1. Vision Geometry

- pinhole camera；
- intrinsic / extrinsic；
- distortion；
- projection / back-projection；
- calibration；
- stereo geometry；
- depth。

## D2. Classical Vision

- filtering；
- edge / corner；
- feature；
- matching；
- optical flow；
- OpenCV 工程。

## D3. Deep Perception

- CNN；
- detection；
- semantic segmentation；
- instance segmentation；
- depth estimation；
- feature extraction。

## D4. 3D Perception

- PointCloud；
- voxel；
- clustering；
- ICP 基础；
- occupancy；
- BEV；
- multi-sensor fusion 表示。

## D5. World Model

- metric world representation；
- semantic world representation；
- dynamic obstacle；
- free-space；
- occupancy / costmap；
- short-term temporal context。

---

# 6. E — Localization / State Estimation / SLAM

**目标：L3**

- odometry；
- Bayesian filtering；
- Kalman Filter；
- EKF / error-state 基本思想；
- IMU propagation；
- scan matching；
- ICP / point-to-plane；
- LiDAR Odometry；
- VIO；
- LIO；
- loop closure；
- pose graph；
- factor graph；
- GNSS / RTK fusion；
- covariance / observability 基本理解；
- localization failure detection。

要求最终能回答“位置从哪里来、可信度多大、为什么会漂、怎样恢复”。

---

# 7. F — Planning / Navigation / Behavior

**目标：L4**

## F1. Graph Search

- BFS / Dijkstra；
- A*；
- heuristic；
- graph abstraction；
- hierarchical planning。

## F2. Motion Planning

- footprint / collision checking；
- state lattice；
- Hybrid A*；
- Dubins / Reeds-Shepp 基本思想；
- RRT / RRT*；
- trajectory optimization。

## F3. Navigation System

- global / local costmap；
- planner / controller / smoother；
- BT / state machine；
- route / corridor / HPA；
- keep / switch；
- dynamic obstacle；
- social navigation；
- recovery / fallback。

目标不只是“会调 Nav2”，而是能设计整个导航决策层。

---

# 8. G — Control

**目标：L3**

- feedback；
- PID；
- state-space；
- controllability 基本概念；
- stability 基本概念；
- LQR；
- MPC；
- stochastic optimal control 基本思想；
- MPPI；
- trajectory tracking；
- constraint / saturation；
- actuator delay；
- safety stop / recovery。

要求能从“为什么产生这个控制量”解释到源码和实机行为。

---

# 9. H — Manipulation / Mobile Manipulation

**目标：L3**

## H1. Arm Kinematics

- joint / link；
- FK；
- IK；
- Jacobian；
- singularity；
- workspace。

## H2. Motion Planning

- configuration space；
- collision checking；
- RRT / PRM / OMPL；
- MoveIt2 planning scene；
- trajectory generation。

## H3. Grasp / Interaction

- grasp pose；
- object pose；
- approach / retreat；
- force / torque sensing 基础；
- impedance / compliant control 基础。

## H4. Mobile Manipulation

把：

```text
Navigation + Perception + Arm Planning + Control
```

统一成一个任务系统，例如“找到杯子→走过去→抓取→送回”。

---

# 10. I — Deep Learning Engineering

**目标：L3**

- tensor；
- computational graph；
- autograd；
- loss；
- optimizer；
- backpropagation；
- Dataset / DataLoader；
- CNN；
- embedding；
- Attention；
- Transformer；
- training / validation；
- overfit / regularization；
- checkpoint；
- export；
- ONNX / TensorRT；
- GPU memory / inference profiling。

目标是能训练、修改、部署模型，而不是只调用模型接口。

---

# 11. J — Robot Learning

**目标：L3**

- MDP；
- policy / value；
- RL 基础体系；
- imitation learning；
- behavior cloning；
- demonstrations；
- sequence modeling for action；
- ACT；
- Diffusion Policy；
- offline RL 基础；
- reward / dataset bias；
- action representation；
- policy evaluation；
- sim2real / real2sim 基础。

重点是“机器人如何从数据学习动作”。

---

# 12. K — VLM / VLA / Embodied AI

**目标：L3，长期向 L4 发展**

## K1. VLM

- vision encoder；
- language model；
- multimodal alignment；
- visual token；
- grounding；
- reasoning；
- task understanding。

## K2. VLA

- vision + language + robot state；
- action token / continuous action；
- action chunk；
- policy head；
- temporal context；
- embodiment adaptation；
- fine-tuning；
- inference latency；
- safety / fallback。

## K3. System Integration

重点研究两种架构：

```text
VLA → low-level action
```

和：

```text
VLM/VLA → high-level skill
           ↓
Navigation / Manipulation / Controller
```

要求能根据任务、安全性、数据和实时性选择架构，而不是默认“端到端最好”。

---

# 13. L — Simulation / Data / Evaluation / Deployment

**目标：L4**

- Gazebo / Isaac Sim；
- dataset schema；
- rosbag / trajectory dataset；
- labeling；
- benchmark；
- regression test；
- failure taxonomy；
- automatic metrics；
- experiment design；
- ablation；
- versioning；
- Docker；
- Orin / CUDA / TensorRT；
- monitoring；
- OTA / deployment 基础；
- data flywheel。

真正的全栈 Owner 必须能把“算法有效”变成“系统长期可验证地有效”。

---

# 14. 最终能力结构

最终不是平均分配精力，而是：

```text
                         VLA / Embodied AI   L3→L4
                                  ▲
                    Robot Learning / VLM     L3
                                  ▲
       Perception L3 ─── World Model ─── Manipulation L3
              ▲                             ▲
     Localization L3                Planning/Control L4/L3
              \                         /
               \                       /
               Robot System / ROS2 / Linux L4
                          ▲
                Sensors / Hardware L2~L3

贯穿全部：Math Foundation + Data/Evaluation/Deployment
```

导航仍然是强项，但最终价值来自 **跨层连接能力**。