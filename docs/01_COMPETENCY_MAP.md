# 01_COMPETENCY_MAP — 机器人全栈 / 具身智能能力树

## 1. 使用方式

本文件定义后续课程的能力边界。所有 Module、Day、LAB、Project 都应映射到这里。

等级统一采用：

- L1：见过；
- L2：能解释；
- L3：能计算 / 推导；
- L4：能实现 / Debug；
- L5：能迁移 / 修改 / 设计。

最终结构不是“所有方向都一样深”，而是形成：

> **机器人理论达到研究生核心课程可用深度；系统/导航/调试为强项；感知、估计、控制、Manipulation、Robot Learning、VLA 能独立理解和落地。**

---

# A. Robot System / Software Stack

**目标：L4→L5**

## A1. C++ / Python Engineering

- object lifetime / RAII；
- pointer / reference / smart pointer；
- move / copy；
- STL；
- lambda；
- template 基础；
- thread / mutex / condition variable；
- async / callback；
- shared library / ABI 基础；
- CMake / colcon；
- Python 工程、NumPy、数据处理、训练脚本。

## A2. Linux / OS / Runtime

- process / thread；
- virtual memory 基础；
- file descriptor；
- IPC / socket；
- signal；
- scheduling；
- realtime 基础；
- CPU / memory / IO profiling；
- systemd / Docker；
- concurrency / synchronization。

## A3. ROS2

- Node / Topic / Service / Action；
- DDS / QoS；
- Executor / Callback Group；
- TF2；
- Lifecycle；
- Component / Composition；
- pluginlib；
- parameter / launch；
- rosbag / diagnostics / tracing；
- ros2_control / hardware interface。

## A4. System Architecture

- module responsibility；
- sync / async pipeline；
- latency budget；
- freshness / watchdog；
- state machine / behavior tree；
- fallback / degraded mode；
- data contract；
- system-level failure propagation。

---

# B. Mathematical & Computational Foundations

**目标：核心内容 L3，支撑专业模块 L4/L5；不以纯数学研究为目标。**

## B1. Linear Algebra

- vector / matrix；
- basis / coordinate；
- linear transformation；
- inverse / rank；
- dot / cross / norm；
- projection；
- eigenvalue / eigenvector；
- quadratic form；
- SVD；
- covariance matrix。

## B2. Calculus

- function / derivative；
- partial derivative；
- chain rule；
- gradient；
- Jacobian；
- Hessian 基础；
- Taylor expansion；
- integral；
- differential equation 基础。

## B3. Probability / Statistics

- random variable；
- probability distribution；
- joint / conditional probability；
- Bayes；
- Gaussian；
- expectation / variance / covariance；
- MLE / MAP；
- sampling；
- uncertainty / confidence。

## B4. Numerical Methods / Optimization

- numerical integration；
- finite difference；
- conditioning / numerical error；
- least squares / weighted LS；
- nonlinear least squares；
- gradient descent；
- Newton / Gauss-Newton / LM；
- constrained optimization；
- convexity 基础；
- stochastic optimization。

## B5. Robot Geometry

- 2D / 3D coordinate frame；
- rotation matrix；
- quaternion；
- homogeneous transform；
- SO(2) / SO(3)；
- SE(2) / SE(3)；
- Lie group / Lie algebra 工程级理解。

## B6. Graph / Algorithm Foundations

- graph / tree；
- queue / priority queue；
- BFS / DFS；
- complexity；
- heuristic；
- dynamic programming 基础；
- sampling / search structures。

---

# C. Mechanics / Kinematics / Dynamics

**目标：L3→L4**

- position / velocity / acceleration；
- force / torque；
- rigid-body motion；
- mobile robot kinematics；
- DH / POE 基础；
- forward kinematics；
- inverse kinematics；
- Jacobian / velocity mapping；
- singularity / manipulability；
- inertia；
- Newton-Euler；
- Lagrange；
- manipulator dynamics；
- ODE / state-space；
- linearization / discretization。

---

# D. Sensors / Actuators / Hardware Interface

**目标：L2→L3**

## D1. Sensors

- Camera / RGB-D；
- LiDAR；
- IMU；
- Encoder / Wheel Odom；
- GNSS / RTK；
- Force / Torque sensor。

## D2. Measurement Quality

- sampling rate；
- resolution / accuracy / precision；
- noise / bias / drift；
- latency / jitter；
- timestamp；
- hardware / software synchronization；
- intrinsic / extrinsic calibration。

## D3. Communication / Actuation

- UART / Serial；
- CAN；
- Ethernet / USB 基础；
- motor / BLDC / servo 基础；
- encoder feedback；
- position / velocity / torque control；
- gearbox / actuator limitations。

目标不是硬件研发，而是能解释“数据为什么错、动作为什么偏”。

---

# E. Simulation

**目标：L2→L3**

- URDF / Xacro；
- link / joint / inertial / collision；
- Gazebo / Isaac Sim；
- physics / sensor simulation；
- ros2_control simulation；
- ground truth；
- repeatable scenario；
- synthetic data；
- domain randomization；
- sim-to-real gap。

---

# F. Vision / 3D Perception / World Representation

**目标：L3→L4**

## F1. Vision Geometry

- pinhole camera；
- intrinsic / extrinsic；
- distortion；
- projection / back-projection；
- calibration；
- stereo / RGB-D；
- PnP 基础。

## F2. Deep Vision

- CNN；
- detection；
- YOLO；
- semantic / instance segmentation；
- depth estimation；
- feature representation；
- tracking 基础。

## F3. 3D Perception

- point cloud；
- filtering / downsampling；
- KD-tree 概念；
- voxel；
- clustering；
- 3D detection；
- BEV；
- occupancy；
- free-space。

## F4. Robot World Representation

- metric / semantic representation；
- obstacle / dynamic obstacle；
- costmap / occupancy；
- short-term temporal context；
- perception output → planning / manipulation interface。

---

# G. State Estimation / SLAM

**目标：L3→L4**

- state / process / measurement model；
- Bayesian filtering；
- KF / EKF；
- covariance propagation；
- IMU / Wheel / GNSS fusion；
- observability 基础；
- outlier / dropout；
- ICP / scan matching；
- LiDAR odometry；
- pose graph；
- factor graph；
- IMU preintegration 概念；
- LiDAR SLAM / LIO；
- VIO；
- loop closure；
- degeneracy；
- localization failure detection / recovery。

要求能够回答：位置从哪里来、可信度多大、为什么会漂、为什么会跳。

---

# H. Planning / Navigation / Behavior

**目标：L4→L5**

## H1. Graph Search

- BFS / Dijkstra；
- A*；
- heuristic；
- open / closed set；
- hierarchical abstraction / HPA。

## H2. Motion Planning

- occupancy / costmap；
- footprint / collision checking；
- inflation / distance field；
- state lattice；
- Hybrid A*；
- motion primitive；
- RRT / RRT*；
- OMPL 基础；
- trajectory smoothing / optimization 基础。

## H3. Behavior

- Behavior Tree；
- replanning；
- path switching；
- recovery；
- dynamic obstacle handling；
- global / local responsibility boundary。

源码主线：真实工程实现 + Nav2 官方实现 + 必要最小自实现。

---

# I. Control / Optimal Control

**目标：L4，关键部分向 L5**

- feedback；
- PID；
- error dynamics；
- state-space；
- equilibrium / linearization；
- stability；
- eigenvalue；
- controllability / observability；
- LQR；
- Riccati 思想；
- MPC；
- prediction horizon；
- constraint；
- receding horizon；
- MPPI；
- sampling / rollout / noise / cost / weighting；
- trajectory tracking；
- saturation / rate limit；
- collision / safety constraint。

---

# J. Manipulation

**目标：L3→L4**

- robot model / URDF / SRDF；
- FK / IK；
- Jacobian；
- workspace / singularity；
- Planning Scene；
- MoveIt2；
- OMPL；
- collision checking；
- grasp pose / pre-grasp / approach / retreat；
- trajectory generation / execution；
- joint / Cartesian control；
- force control；
- impedance control；
- vision → object pose → base → EEF transform chain；
- failure recovery。

---

# K. Deep Learning

**目标：L3→L4**

- tensor / computation graph；
- PyTorch；
- Autograd / backpropagation；
- loss；
- optimizer；
- batch / learning rate；
- normalization / regularization；
- CNN；
- embedding；
- attention；
- Q / K / V；
- Transformer；
- training / validation；
- overfit / underfit；
- checkpoint；
- inference / deployment。

---

# L. Robot Learning

**目标：L3→L4**

- MDP；
- state / action / reward；
- policy / value；
- PPO / TD3 等 RL 基础复盘；
- Behavior Cloning；
- Imitation Learning；
- DAgger 概念；
- covariate shift；
- Offline RL 基础；
- demonstrations / dataset distribution；
- ACT；
- action chunking；
- Diffusion Policy；
- policy rollout / evaluation；
- sim2real / robustness。

---

# M. VLM

**目标：L3→L4**

- token / embedding；
- language Transformer；
- Vision Encoder / ViT；
- visual token；
- multimodal projection / alignment；
- multimodal fusion；
- instruction tuning；
- grounding；
- object / region / spatial relation；
- scene / task understanding；
- VLM 与传统 perception 的边界。

---

# N. VLA

**目标：L3→L4，长期向 L5**

- Vision + Language + Robot State；
- proprioception / history；
- state representation；
- action representation；
- joint / EEF / base action；
- continuous / discrete action；
- action token；
- action chunk；
- temporal context；
- demonstration normalization；
- training / fine-tuning；
- closed-loop inference；
- VLA → trajectory / controller interface；
- safety filter / fallback。

---

# O. Mobile Manipulation / Embodied System

**目标：L4**

- language instruction；
- task / skill decomposition；
- object search；
- navigation；
- base positioning；
- object relocalization；
- manipulation；
- VLA / classical skill hybrid architecture；
- long-horizon state；
- replanning；
- recovery；
- execution feedback。

最终完成从自然语言任务到真实 action 的完整链路。

---

# P. Deployment / Data / Evaluation / Sim2Real

**目标：L4→L5**

- Orin；
- GPU / CUDA execution model 基础；
- TensorRT；
- FP16 / INT8 概念；
- profiling；
- Docker / systemd；
- rosbag → dataset / episode；
- synchronization / cleaning / annotation；
- dataset / model / config versioning；
- benchmark；
- success rate / latency / collision / task metrics；
- regression；
- failure mining；
- sensor / action noise；
- latency randomization；
- sim2real validation。

---

# Q. Safety / Reliability / Owner

**目标：L4→L5**

- watchdog / heartbeat；
- timeout / freshness；
- rate limit / saturation；
- uncertainty；
- safe stop；
- fallback；
- degraded mode；
- collision safety；
- braking / safety margin；
- human takeover；
- fault propagation；
- single-point failure；
- responsibility boundary；
- architecture trade-off；
- evidence chain；
- regression test；
- version risk。

---

# R. Research Methodology

**目标：L3→L4**

- literature search；
- paper reading；
- problem definition；
- related work；
- baseline；
- hypothesis；
- experiment design；
- controlled variable；
- metric；
- dataset split；
- ablation；
- repeated experiment；
- statistical comparison 基础；
- reproducibility；
- technical report；
- limitation / failure analysis。

最终通过 Capstone 将理论、算法、工程和研究方法统一起来。