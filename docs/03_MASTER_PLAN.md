# 03_MASTER_PLAN — M00–M22 主课程总纲与Day索引

> 本文件是整个课程总索引。Module顺序、Day范围、教学边界与动态M22规则已经锁定；真正每日长篇讲义只在学习到对应Day时写入 `docs/lessons/`。

## 1. 主课程顺序

| Module | 名称 | Day | 核心作用 |
|---|---|---:|---|
| M00 | Robot Full-stack Architecture | 1 | 全系统视角、责任边界、Owner思维 |
| M01 | C++ / Linux / ROS2 Systems | 2–7 | 运行时、并发、DDS/QoS、Executor、TF |
| M02 | Mathematical Foundations I | 8–15 | 线代、微积分、Jacobian、Taylor |
| M03 | Sensors & Actuators | 16–19 | Sensor/Actuator、时间、标定、执行语义 |
| M04 | Robot Simulation Foundations | 20–21 | URDF、Simulation、Ground Truth、Domain Gap |
| M05 | Vision Geometry | 22–26 | Camera Geometry、Calibration、Depth→3D |
| M06 | Deep Learning Foundations | 27–33 | PyTorch、Backprop、CNN、Attention、Transformer |
| M07 | Deep Vision & 3D Perception | 34–39 | Detection/Seg/Depth/PointCloud/Clustering/BEV |
| M08 | Mathematical Foundations II | 40–48 | Probability/Bayes/LS/GN/SO(3)/SE(3)，为SLAM与M12刚体运动打底 |
| M09 | State Estimation | 49–54 | KF/EKF、Fusion、Observability、Health |
| M10 | SLAM / LIO / VIO / Factor Graph | 55–62 | ICP、LIO/VIO、Graph、Loop、Degeneracy |
| M11 | Planning & Navigation | 63–70 | A*/Hybrid A*/C-space/RRT/HPA/Nav2/Path Switch |
| M12 | Robot Kinematics / Dynamics / System Dynamics | 71–79 | Screw/Twist/POE/Space-Body Jacobian/IK/Dynamics/Mobile Kinematics |
| M13 | Control & Optimal Control | 80–89 | Trajectory/Time Scaling/PID/Stability/LQR/MPC/MPPI |
| M14 | Manipulation | 90–96 | MoveIt2/Collision/Grasp Map/Closure/Trajectory/Impedance |
| M15 | Robot Learning | 97–104 | RL复盘、BC/IL、ACT、Diffusion、Offline RL |
| M16 | VLM | 105–109 | LM/Vision Tokens/Alignment/Fusion/Grounding |
| M17 | VLA | 110–115 | Multimodal Policy、Action、Chunk、Fine-tuning |
| M18 | Mobile Manipulation | 116–118 | Base+Arm、Handoff、Long-horizon/Recovery |
| M19 | Deployment / Data / Evaluation / Sim2Real | 119–122 | Orin/TensorRT、Data、Benchmark、Release |
| M20 | Safety / Reliability / Owner | 123–126 | Hazard、Watchdog、Degraded、FMEA/FTA、Safety Case |
| M21 | Research Methodology & Capstone | 127–129 | Problem/Baseline/Hypothesis/Experiment/Defense |
| M22 | Foundation Cleanup | 130–135* | 动态清理Foundation Debt |

`*` M22只是预留动态槽位；可少于6天、扩展或跨学习时段，不能为了凑Day制造内容。

---

## 2. 课程阶段
- **Phase A M00–M04**：系统、ROS2 Runtime、Math I、Sensors、Simulation；
- **Phase B M05–M07**：Camera Geometry → Deep Vision / 3D World Representation；
- **Phase C M08–M10**：Probability/Optimization/SE(3) → KF/EKF → LIO/VIO/Factor Graph；
- **Phase D M11–M13**：Planning → Modern Robot Kinematics/Dynamics → Control；
- **Phase E M14**：Manipulation / Contact / Grasp Mechanics；
- **Phase F M15–M17**：Robot Learning → VLM → VLA；
- **Phase G M18–M20**：Mobile Manipulation → Deployment/Sim2Real → Safety；
- **Phase H M21–M22**：Research/Capstone → Dynamic Foundation Cleanup。

---

## 3. Modern Robotics 教材集成

《Modern Robotics: Mechanics, Planning, and Control》（Kevin M. Lynch / Frank C. Park）作为课程中“刚体运动—运动学—动力学—控制—操作”的主要理论骨架之一，但**不按整本书顺序机械学习，也不替代ROS2/Nav2/MoveIt/VLA工程主线**。

| 课程模块 | 教材定位 | 章节 |
|---|---|---|
| M08 | 核心参考：Rigid-Body Motion / SE(3)基础 | Ch3 |
| M11 | 辅助参考：Configuration Space / Motion Planning | Ch2、Ch10 |
| M12 | **主要教材**：Configuration、POE、Jacobian、IK、Dynamics、Wheeled Robot | Ch2–8、Ch13 |
| M13 | 核心参考：Trajectory Generation / Robot Control；课程另补LQR/MPC/MPPI | Ch9、Ch11 |
| M14 | 核心理论参考：Trajectory + Grasping / Contact；工程仍以MoveIt2为主 | Ch9、Ch12 |

教材取舍原则：
1. 只纳入对机器人全栈 / Manipulation / VLA / System Owner有长期复用价值的基础；
2. Screw/Twist/Wrench/Adjoint/POE/Space-Body Jacobian正式放M12，不提前硬塞进M08；
3. Closed-chain深推、BCH、复杂Lie Jacobian、完整Grasp Wrench Space等暂不作为硬门槛；
4. 教材公式必须回到frame、dimension、源码变量和真实robot行为，不接受只会书面推导。

---

## 4. 总Day索引
```text
Day001  Robot Full-stack Architecture
Day002  C++ Lifetime / Ownership / RAII
Day003  STL / Lambda / Callback
Day004  Concurrency / Thread / Mutex
Day005  Linux Runtime / Process / IPC
Day006  ROS2 DDS / QoS
Day007  Executor / Callback Group / Future / TF / Runtime Integration
Day008  Vector / Matrix / Dimension
Day009  Basis / Coordinate / Transform
Day010  Dot / Cross / Norm / Projection
Day011  Eigen / Quadratic Form
Day012  SVD / Rank / Conditioning
Day013  Derivative / Differential / Numerical Integration
Day014  Gradient / Chain Rule
Day015  Jacobian / Hessian / Taylor / Linearization
Day016  Sensor Model / Noise / Bias / Measurement Quality
Day017  IMU / Encoder / LiDAR / Camera
Day018  GNSS / RTK / Time / Latency / Calibration
Day019  Actuator / Command → Physical Motion
Day020  URDF / Robot Simulation Architecture
Day021  Ground Truth / Domain Gap / Experiment Validity
Day022  Pinhole Camera
Day023  Intrinsic / Extrinsic
Day024  Distortion / Calibration
Day025  Stereo / RGB-D / Depth
Day026  Pixel → Camera → Base → World
Day027  Tensor / Dataset / DataLoader
Day028  Backprop / Autograd
Day029  Training Loop / SGD / Adam
Day030  Softmax / Cross Entropy
Day031  CNN
Day032  Generalization / Normalization / Distribution Shift
Day033  Attention / Transformer
Day034  Classification / Detection / YOLO / IoU / NMS
Day035  Semantic / Instance Segmentation / Traversability
Day036  Monocular Depth / Stereo / RGB-D / Learned Depth
Day037  PointCloud / Filter / KD-tree / Clustering / 3D Detection
Day038  BEV / Occupancy / 3D Representation
Day039  Tracking / Metrics / Perception→Robot Integration
Day040  Probability / Conditional / Bayes
Day041  Expectation / Variance / Covariance / Gaussian
Day042  Likelihood / MLE / MAP
Day043  Residual / LS / WLS
Day044  Nonlinear LS / Newton
Day045  Gauss-Newton / LM / Robust
Day046  Rotation / Euler / Quaternion
Day047  SO(3) / SE(3) / Exp-Log / Perturbation / M12 Bridge
Day048  Probability + Optimization + SE(3) Integration
Day049  State / Motion / Measurement / Q-R-P / Bayesian Filter
Day050  Kalman Prediction
Day051  Kalman Update / Innovation / Gain
Day052  EKF / Linearization / Error-state Intro
Day053  Wheel / IMU / GNSS / RTK Fusion
Day054  Observability / Gating / Dropout / Reset / Debug
Day055  SLAM Architecture / Frontend / Backend
Day056  ICP / Correspondence / Point-to-plane / GN
Day057  IMU Propagation / Bias / Gravity / Preintegration
Day058  LIO / Deskew / Scan-to-map / Error-State / Latency
Day059  VIO / Feature / Reprojection Residual
Day060  Factor Graph / Pose Graph / Information
Day061  Loop Closure / Global Consistency / LIO Boundary
Day062  Degeneracy / Observability / Latency / TF / Initialization / Bag Debug
Day063  Graph / BFS / Dijkstra
Day064  A* / Heuristic
Day065  Occupancy / Costmap / Footprint / Inflation
Day066  Hybrid A* / Motion Primitive
Day067  Configuration Space / C-obstacle / RRT / RRT* / OMPL
Day068  HPA / Hierarchical Planning / Dynamic Edge
Day069  Nav2 / BT / Replanning / Path Switching
Day070  Dynamic Navigation / Path Quality / Planning Owner
Day071  Configuration / Screw Axis / Twist / Wrench
Day072  Forward Kinematics / POE Mainline / DH
Day073  Space-Body Jacobian / Adjoint / Velocity Kinematics / Statics
Day074  Inverse Kinematics / Newton / DLS
Day075  Singularity / SVD / Manipulability / Redundancy
Day076  Wheeled Mobile Kinematics / Nonholonomic Constraint
Day077  Force / Wrench / Inertia / Newton-Euler
Day078  Lagrangian / Manipulator Dynamics / Forward-Inverse Dynamics
Day079  ODE / State-space / Linearization / Discretization / Action
Day080  Trajectory / Time Scaling / Feedback / PID
Day081  Equilibrium / Stability / Eigenvalue
Day082  Controllability / Observability
Day083  Optimal Control / LQR
Day084  MPC / Horizon / Constraint / Receding Horizon
Day085  MPPI / Sampling / Rollout / Weight / Update
Day086  MPPI Noise / Temperature / Warm Start
Day087  Cost / Critic / Constraint / Behavior
Day088  Tracking / Latency / Saturation / Model Mismatch
Day089  Control Owner / MPPI Source
Day090  Manipulation Architecture / RobotState / MoveIt2
Day091  Planning Scene / Collision / Attached Object
Day092  OMPL / Joint-Cartesian Motion / Trajectory / Time Parameterization
Day093  Object Pose / Grasp Pose / Pre-grasp
Day094  Contact / Friction Cone / Wrench / Grasp Map / Closure / Verification
Day095  Cartesian / Force / Impedance Control
Day096  Pick-and-Place / Recovery / Owner
Day097  MDP / Return / V-Q-A / Bellman
Day098  PPO / TD3 / On-policy vs Off-policy Review
Day099  IL / BC / DAgger / Covariate Shift
Day100  Robot Dataset / Temporal Alignment / Action Semantics
Day101  ACT / Action Chunking / CVAE / Temporal Aggregation
Day102  Diffusion Policy / Conditional Denoising / Multimodal Action
Day103  Offline RL / Dataset Support / OOD
Day104  Policy Evaluation / Sim2Real / Owner
Day105  Token / Embedding / Autoregressive LM
Day106  Vision Encoder / Patch Token
Day107  CLIP / Contrastive Alignment
Day108  Projector / Cross Attention / Fusion
Day109  Grounding / Spatial Reasoning / Hallucination / Robot Interface
Day110  VLA Architecture / Proprioception / Action Representation
Day111  VLA Dataset / Sequence / Temporal Context
Day112  Action Token / Autoregressive Action
Day113  Action Chunk / Closed-loop Execution
Day114  Training / Fine-tuning / Action Head / Embodiment
Day115  Evaluation / Safety / Generalization / Owner
Day116  Base + Arm Geometry / Reachability / Base Placement
Day117  Nav→Manipulation Handoff / Re-perception / Whole-body / Long-horizon State
Day118  End-to-End Mobile Manipulation / VLA Integration / Owner
Day119  Orin / CUDA-TensorRT / Runtime / Profiling / Latency
Day120  Robot Data Pipeline / Logging / Version / Failure Mining / Data Reflow
Day121  Evaluation / Benchmark / Regression / Ablation
Day122  Sim2Real / Progressive Deployment / Monitoring / Rollback / Owner
Day123  Hazard / Risk / Safety Requirement / Fail-safe
Day124  Fault / Failure / Watchdog / Interlock / E-stop / Human Takeover
Day125  Reliability / Redundancy / Degraded Mode / FMEA / Fault Tree
Day126  Incident / Safety Case / Fault Injection / Release Gate / Owner
Day127  Problem Definition / Hypothesis / Baseline / Literature
Day128  Experimental Design / Ablation / Statistics / Reproducibility
Day129  Technical Report / Capstone Defense / Contribution / Research Owner
Day130  Dynamic Foundation Debt #1
Day131  Dynamic Foundation Debt #2
Day132  Dynamic Foundation Debt #3
Day133  Dynamic Foundation Debt #4
Day134  Dynamic Foundation Debt #5
Day135  Dynamic Foundation Debt #6
```

---

## 5. Day / Lesson / LAB职责
- `docs/modules/Mxx_*.md`：Teaching Contract；
- `docs/lessons/dayXXX.md`：真正学习时生成的详细讲义；
- `docs/labs/`：少量理论不可替代的实践；
- `docs/PROGRESS.md`：当前学习状态与Foundation Debt。

## 6. 学习长度与跳级
1. 普通Day默认2–3h；
2. 理论、公式、算法、源码、案例为主；
3. 每日核心知识点≤20；
4. 已掌握内容可入口诊断后跳过/压缩；
5. 核心前置不过关不机械推进；
6. LAB独立安排；
7. Day编号是逻辑位置，不等于必须消耗135个自然日。

## 7. 正式LAB
当前锁定3个：
- `LAB01_manipulation_pick_and_place.md`：M14 Pick-and-Place全链；
- `LAB02_mobile_manipulation_capstone.md`：M18 Mobile Manipulation端到端，并可扩展为M21最终Research Capstone；
- `LAB03_robot_policy_vla_action_interface.md`：M15/M17 learned policy action→safety→controller闭环。

其它A*/EKF/PID/LQR/Attention/MPPI等最小自实现只在明显提升理解时安排。

## 8. 统一毕业考试
默认Module Graduation Exam：
- **30% 核心基础**
- **50% 综合系统场景**
- **20% Source / Formula / Design**

默认总分≥85%，Hard Gate不能被总分补偿。**M00作为1-Day总纲模块保留轻量Owner场景考核例外。**

## 9. v1.0冻结规则
M00–M22顺序、Day1–Day129固定主课程、M22动态槽位和3个正式LAB继续视为 **Curriculum v1.0结构冻结**。本次Modern Robotics集成属于既有P0/P1理论骨架补强，**不增加Module或Day**。

后续只有以下情况才改大纲：
- 正式学习/考试暴露P0/P1结构性缺口；
- 真实项目或目标岗位发生重大变化；
- Teaching Contract存在明确错误/循环依赖/安全风险。

普通薄弱点进入 `PROGRESS.md` Foundation Debt 或M22，不重构主线。

## 10. 当前状态
- M00–M22：结构锁定；
- Day1–Day129：固定主课程已设计；
- Day130–Day135：动态Foundation Cleanup；
- Modern Robotics：已嵌入M08/M11/M12/M13/M14；
- 正式LAB：3个；
- Daily Lessons：按实际进度逐日生成；
- 下一步：入口诊断 / Day1正式学习。
