# 04_MODULE_SPECS — M00–M22 模块规格与毕业标准

> 本文件定义每个 Module 的前置、必须掌握内容和毕业标准。它不是 Day 计划。

## M00 — Robot Full-stack Architecture

**前置：** 无。

**必须掌握：** Sensor→Driver→Perception→State Estimation→World Model→Task/Behavior→Planning→Control→Actuator；ROS2数据链；传统栈 / Policy / VLA 边界；数据闭环；Failure / Safety 基本概念。

**毕业标准：** 给任意移动机器人或移动机械臂需求，能画完整架构并说明输入输出、坐标系、频率、责任边界、故障传播链。

---

## M01 — C++ / Linux / ROS2 Systems

**前置：** 基础 C++ 语法、基本计算机概念。

**必须掌握：** RAII、对象生命周期、智能指针、STL、lambda、template基础、线程同步；Linux process/thread/memory/IPC/socket/scheduling；ROS2 DDS/QoS/Executor/Callback Group/TF/Lifecycle/Component/pluginlib/ros2_control/diagnostics。

**暂不深入：** Linux kernel、DDS协议源码、lock-free理论。

**毕业标准：** 能读陌生 ROS2 核心调用链，解释异步/并发/QoS/TF问题，并通过日志、tracing、系统指标定位延迟或通信故障。

---

## M02 — Mathematical Foundations I

**前置：** 高中数学。

**必须掌握：** Vector/Matrix、线性变换、inverse/rank、dot/cross/norm、eigen/SVD；derivative/partial derivative/chain rule；Taylor、gradient、Jacobian、Hessian概念；numerical integration、finite difference、数值误差直觉。

**暂不深入：** 严格证明、高阶分析。

**毕业标准：** 能手算关键小例题并用 Python 验证；看到机器人公式能判断变量维度、物理意义和基本推导过程。

---

## M03 — Sensors & Actuators

**前置：** M00；基础物理。

**必须掌握：** Camera/LiDAR/IMU/Encoder/GNSS/RTK/RGB-D/F-T；sampling、resolution/accuracy/precision、noise/bias/drift、timestamp/latency/jitter、sync、intrinsic/extrinsic calibration；UART/CAN/Ethernet；motor/BLDC/servo、encoder feedback、position/velocity/torque control。

**暂不深入：** PCB、电机驱动器研发。

**毕业标准：** 能从规格和数据判断测量质量，设计 ROS2 接入链，并区分 sensor、sync、frame、driver、actuator 造成的问题。

---

## M04 — Robot Simulation Foundations

**前置：** M00 + M01；M03可并行。

**必须掌握：** URDF/Xacro、link/joint/inertial/collision；Gazebo/Isaac Sim、physics、sensor simulation、ros2_control simulation、clock、ground truth、repeatable scenario、domain gap。

**毕业标准：** 能理解并搭建可重复机器人仿真场景，为后续 Planning/Control/Manipulation/Learning 提供验证环境。

---

## M05 — Vision Geometry

**前置：** M02 + M03 Camera基础。

**必须掌握：** pinhole、focal/principal point/FOV、pixel/image/camera/base/world frame、intrinsic/extrinsic、distortion、projection/back-projection、calibration、Stereo/RGB-D、PnP基础。

**毕业标准：** 能计算并解释 `pixel → camera 3D → base/world` 全链路，并分析标定/深度误差如何传播。

---

## M06 — Deep Learning Foundations

**前置：** M02。

**必须掌握：** Tensor/Autograd、Dataset/DataLoader、training loop、loss、optimizer、backprop、CNN、activation、normalization、regularization、overfit/underfit、learning rate、Embedding、Q/K/V、Attention、Transformer基础。

**毕业标准：** 能解释完整训练链、梯度传播和 tensor 维度；能够读懂和修改小型 CNN / Transformer 训练代码。

---

## M07 — Deep Vision & 3D Perception

**前置：** M05 + M06。

**必须掌握：** classification/detection/YOLO、semantic/instance segmentation、depth、PointCloud、filter/downsample、KD-tree概念、voxel、3D detection、BEV、occupancy、tracking基础、precision/recall/IoU/mAP/depth metrics；感知输出到 TF / world model / costmap / manipulation。

**毕业标准：** 能解释从 raw sensor 到机器人可消费语义/3D信息的完整链，并能分析网络输出正确但机器人行为仍错误的跨模块问题。

---

## M08 — Mathematical Foundations II

**前置：** M02。

**必须掌握：** random variable/distribution、joint/conditional probability、Bayes、Gaussian、expectation/variance/covariance、MLE/MAP、least squares/weighted LS/nonlinear LS、gradient/Newton/GN/LM、rotation/quaternion、SO(3)/SE(3)、exp/log工程直觉、conditioning。

**毕业标准：** 能计算 Bayes/Gaussian/covariance 和小型 LS/GN；能解释 SLAM/EKF 中 state、residual、Jacobian、noise、pose update。

---

## M09 — State Estimation

**前置：** M03 + M08。

**必须掌握：** state/control/measurement model、process/measurement noise、Bayesian filtering、KF predict/update、Kalman Gain、EKF、Jacobian linearization、covariance propagation、Wheel/IMU/GNSS融合、observability直觉、outlier/dropout/reset。

**毕业标准：** 能从数学解释 `x/P/F/H/Q/R/K`；能够推演简化 KF/EKF，并用 rosbag / 日志判断融合异常根因。

---

## M10 — SLAM / LIO / VIO / Factor Graph

**前置：** M05 + M08 + M09 + M03。

**必须掌握：** frontend/backend、ICP point-to-point/point-to-plane、residual/Jacobian、pose graph、factor/information matrix、IMU bias/gravity/preintegration概念、deskew、scan-to-map、LiDAR SLAM/LIO、feature/reprojection、VIO、loop closure、degeneracy、timestamp/latency/TF。

**毕业标准：** 能从 `state → propagation → residual → Jacobian → optimization/update` 解释 LIO/VIO；能用真实 bag / 日志分析 drift、latency、degeneracy、initialization。

---

## M11 — Planning & Navigation

**前置：** M01 + M02；图论/几何基础。

**必须掌握：** BFS/Dijkstra/A*、heuristic、open/closed、occupancy/costmap、inflation、footprint/collision、distance field、Hybrid A*、motion primitive、RRT/RRT*、OMPL思想、HPA、BT、replanning/path switching、trajectory smoothing/optimization概念、dynamic/local navigation。

**源码原则：** 公司真实实现 + 算法本体 + Nav2官方 + 必要最小自实现。

**毕业标准：** 能解释 planner 为什么选择某条路径，能判断 HPA/Planner/BT/Controller 责任边界，并具备修改规划决策原则的能力。

---

## M12 — Robot Kinematics / Dynamics / System Dynamics

**前置：** M02 + M08 + 基础力学。

**必须掌握：** rigid transform、DH/POE概念、FK、IK、Jacobian、linear/angular velocity、singularity/manipulability、mobile robot kinematics、mass/inertia、Newton-Euler、Lagrange、`M(q)qdd + C(q,qd)qd + g(q) = tau`、ODE、state-space、linearization/discretization。

**毕业标准：** 能推导简化机械臂 FK/Jacobian、解释 IK 和奇异性；能建立简化机器人 state-space；理解 joint/EEF/velocity/torque action 的物理含义。

---

## M13 — Control & Optimal Control

**前置：** M12 + M08。

**必须掌握：** feedback、PID、error dynamics、state-space、equilibrium、eigenvalue/stability、controllability/observability、LQR、quadratic cost/Riccati思想、MPC、prediction horizon/objective/constraint/receding horizon、MPPI sampling/rollout/noise/cost/weighting/warm start、trajectory tracking、saturation/rate limit/collision constraint。

**源码原则：** 公司 MPPI + 数学本体 + Nav2 官方 MPPI + 必要最小自实现。

**毕业标准：** 能从数学预测 horizon/noise/cost/constraint 修改会产生怎样的机器人行为，并能设计验证方式。

---

## M14 — Manipulation

**前置：** M05/M07 + M12 + M13。

**必须掌握：** URDF/SRDF、joint limit/EEF、MoveIt2、Planning Scene、RobotState、kinematics plugin、configuration space、collision checking、OMPL、trajectory generation/time parameterization、grasp/pre-grasp/approach/retreat、joint/Cartesian/force/impedance control、visual transform chain、recovery。

**毕业标准：** 能解释 `感知 → object pose → IK → planning → trajectory → controller → actuator`，并判断失败属于 perception/calibration/IK/planner/controller 哪一层。

---

## M15 — Robot Learning

**前置：** M06 + M12 + M13 + M14。

**必须掌握：** MDP、state/action/reward、policy/value、PPO/TD3等基础复盘、Behavior Cloning、Imitation Learning、DAgger概念、covariate shift、Offline RL、dataset distribution/OOD、ACT、action chunking、Diffusion Policy、rollout/evaluation、Sim2Real基础。

**毕业标准：** 能解释 demonstrations 如何变成 policy，并系统分析“训练集表现好、实机失败”的数据分布和闭环问题。

---

## M16 — VLM

**前置：** M05/M07 + M06。

**必须掌握：** language token/embedding/decoder Transformer、Vision Encoder/ViT、visual token、projection/alignment、multimodal fusion、instruction tuning、grounding、region/object/spatial relation、scene/task understanding、VLM与传统 perception 边界。

**毕业标准：** 能解释 image+prompt 到 semantic/text output 的全过程，并设计其与机器人任务系统的接口。

---

## M17 — VLA

**前置：** M15 + M16 + M12/M13/M14。

**必须掌握：** Vision+Language+Robot State→Policy→Action；proprioception/history；joint/base/EEF state；continuous/discrete action、action token、action chunk、temporal context、normalization、training/fine-tuning、closed-loop inference、VLA→Skill/trajectory/controller接口、安全filter/fallback。

**毕业标准：** 能逐张量解释 observation→encoding→fusion→action→controller；明确 VLA 与经典模块的责任边界。

---

## M18 — Mobile Manipulation

**前置：** M11 + M14 + M17。

**必须掌握：** instruction→task/skill decomposition、object search、navigation、base positioning、re-observation、pre-grasp alignment、grasp/manipulation、BT/FSM/skill/VLA混合架构、long-horizon state、replan/recovery。

**毕业标准：** 能设计并分析“语言任务→找目标→导航→重新观察→对位→抓取→搬运/放置→失败恢复”完整系统。

---

## M19 — Deployment / Data / Evaluation / Sim2Real

**前置：** 前面主要算法模块。

**必须掌握：** Orin、CPU/GPU、CUDA execution model基础、TensorRT、FP16/INT8概念、profiling、Docker/systemd、rosbag→episode/dataset、sync/clean/annotation/versioning、success rate/latency/collision/task metrics、benchmark/regression/failure mining、sensor/action noise、latency/randomization、sim2real calibration。

**毕业标准：** 能设计 `采集→训练→部署→评测→失败分类→数据回流→再训练→回归` 完整闭环。

---

## M20 — Safety / Reliability / Owner

**前置：** M00–M19主要系统能力。

**必须掌握：** watchdog/heartbeat、timeout/freshness、rate limit/saturation、uncertainty、safe stop、fallback/degraded mode、collision safety、human takeover、fault propagation、single-point failure、responsibility boundary、trade-off、evidence chain、regression/version risk。

**毕业标准：** 给一份真实事故资料，独立形成 `现象→证据→传播链→根因→临时措施→长期方案→回归测试→风险评估`。

---

## M21 — Research Methodology & Capstone

**前置：** 主体课程基本完成。

**必须掌握：** literature search、problem definition、related work、baseline、hypothesis、experiment design、controlled variable、dataset split、metrics、ablation、repeatability、statistical comparison基础、reproducibility、technical report、limitation/failure analysis。

**毕业标准：** 完成接近机器人硕士 Capstone / Dissertation 强度的综合项目，包含 baseline、方法、量化实验、ablation、失败分析和可复现报告。

---

## M22 — Foundation Cleanup

**前置：** 主体课程全过程的 PROGRESS / Foundation Debt。

**必须掌握：** 根据真实缺口动态补齐数值分析、凸优化、概率统计深化、OS/Network、Dynamics、Control、Algorithm Theory、AI优化等。

**毕业标准：** 对所有关键“会用但解释不清 / 会调但不能推导 / 看懂代码但无法迁移”的理论债务逐项复测清零。

---

# 统一毕业判断

一个核心知识点的目标不是“看过”，而是逐步达到：

```text
L1 见过
→ L2 能解释
→ L3 能计算 / 推导
→ L4 能实现 / Debug
→ L5 能迁移 / 修改 / 设计
```

普通理论 Module 以 L3 为最低核心线；强工程模块和主方向逐步要求 L4/L5。

是否需要实验由知识本身决定，不由 Module 或 Day 形式决定。