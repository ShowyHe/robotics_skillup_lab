# 03_MASTER_PLAN — M00–M22 主课程总纲与Day索引

> 本文件是整个课程总索引。Module顺序、Day范围、教学边界与动态M22规则已经锁定；真正每日长篇讲义仍在学习到对应Day时写入 `docs/lessons/`。

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
| M07 | Deep Vision & 3D Perception | 34–39 | Detection/Seg/Depth/PointCloud/BEV/Occupancy |
| M08 | Mathematical Foundations II | 40–48 | Probability/Bayes/LS/GN/SE(3) |
| M09 | State Estimation | 49–54 | KF/EKF、Fusion、Observability、Health |
| M10 | SLAM / LIO / VIO / Factor Graph | 55–62 | ICP、LIO/VIO、Graph、Loop、Degeneracy |
| M11 | Planning & Navigation | 63–70 | A*/Hybrid A*/RRT/HPA/Nav2/Path Switch |
| M12 | Robot Kinematics / Dynamics / System Dynamics | 71–79 | FK/IK/Jacobian/Dynamics/ODE/State-space |
| M13 | Control & Optimal Control | 80–89 | PID/Stability/LQR/MPC/MPPI |
| M14 | Manipulation | 90–96 | MoveIt2/Collision/Grasp/Trajectory/Impedance |
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

### Phase A — 系统与第一层基础：M00–M04
建立Robot Full-stack、ROS2 Runtime、Math I、Sensors、Simulation。

### Phase B — 感知：M05–M07
从Camera Geometry进入Deep Vision / 3D World Representation。

### Phase C — 概率估计与SLAM：M08–M10
补Probability/Optimization/SE(3)，再进入KF/EKF、LIO/VIO/Factor Graph。

### Phase D — Planning / Dynamics / Control：M11–M13
规划“怎么走”、Dynamics“机器人如何运动”、Control“如何闭环执行”。

### Phase E — Manipulation：M14
Object Pose→IK→Planning→Trajectory→Controller→Contact。

### Phase F — Robot Learning / VLM / VLA：M15–M17
从Robot Dataset/Policy Learning进入Vision-Language Understanding，再进入Action。

### Phase G — Embodied Full-stack / Deployment / Safety：M18–M20
Mobile Manipulation、long-horizon recovery、deployment/data/Sim2Real、安全可靠性。

### Phase H — Research & Cleanup：M21–M22
研究方法/Capstone，并按真实考试结果清理Foundation Debt。

---

## 3. 总Day索引

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
Day037  PointCloud / Filter / KD-tree / Voxel / 3D Detection
Day038  BEV / Occupancy / 3D Representation
Day039  Tracking / Metrics / Perception→Robot Integration

Day040  Probability / Conditional / Bayes
Day041  Expectation / Variance / Covariance / Gaussian
Day042  Likelihood / MLE / MAP
Day043  Residual / LS / WLS
Day044  Nonlinear LS / Newton
Day045  Gauss-Newton / LM / Robust
Day046  Rotation / Euler / Quaternion
Day047  SO(3) / SE(3) / Exp-Log / Perturbation
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
Day067  RRT / RRT* / OMPL
Day068  HPA / Hierarchical Planning / Dynamic Edge
Day069  Nav2 / BT / Replanning / Path Switching
Day070  Dynamic Navigation / Path Quality / Planning Owner

Day071  Rigid Body / Joint / DOF / Kinematic Chain
Day072  Forward Kinematics / DH / POE
Day073  Jacobian / Velocity Kinematics
Day074  Inverse Kinematics
Day075  Singularity / SVD / Manipulability
Day076  Mobile Robot Kinematics
Day077  Force / Torque / Mass / Inertia / Newton-Euler
Day078  Lagrangian / Manipulator Dynamics
Day079  ODE / State-space / Linearization / Discretization / Action

Day080  Feedback / PID
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
Day092  OMPL / Joint-Cartesian Motion / Time Parameterization
Day093  Object Pose / Grasp Pose / Pre-grasp
Day094  Contact / Friction / Force Closure / Verification
Day095  Cartesian / Force / Impedance Control
Day096  Pick-and-Place / Recovery / Owner

Day097  MDP / Return / V-Q-A / Bellman
Day098  PPO / TD3 / On-policy vs Off-policy Review
Day099  IL / BC / DAgger / Covariate Shift
Day100  Robot Dataset / Temporal Alignment / Action Semantics
Day101  ACT / Action Chunking / Temporal Aggregation
Day102  Diffusion Policy / Multimodal Action
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

## 4. Day / Lesson / LAB职责
- `docs/modules/Mxx_*.md`：Day教学合同，定义目标、前置、必须内容、深度、边界、考核和Module毕业要求；
- `docs/lessons/dayXXX.md`：真正学习时生成的详细讲义，不提前批量生成；
- `docs/labs/`：少量理论不可替代的LAB；
- `docs/PROGRESS.md`：当前学习状态与Foundation Debt。

---

## 5. 教学长度与跳级
1. 普通Day默认2–3h；
2. 理论、公式、算法、源码、案例为主；
3. 每日核心知识点不超过20个；
4. 已掌握内容可入口诊断后跳过/压缩；
5. 核心前置不过关不机械推进；
6. LAB独立安排；
7. Day编号表示课程逻辑位置，不代表必须逐日机械消耗135个自然日。

---

## 6. 正式LAB
当前锁定：
- `LAB01_manipulation_pick_and_place.md`：M14模块级Pick-and-Place全链；
- `LAB02_mobile_manipulation_capstone.md`：M18 Mobile Manipulation端到端Capstone。

其它最小自实现（A*/EKF/PID/LQR/Attention/MPPI等）只有当它明显提升理论理解时安排，不默认成为LAB。

---

## 7. 统一毕业考试
默认Module Graduation Exam：
- **30% 核心基础**
- **50% 综合系统场景**
- **20% Source / Formula / Design**

默认通过：总分≥85%；Hard Gate不能以总分补偿。单个关键concept失败→定向补课→targeted retest。

---

## 8. 当前状态
- M00–M22顺序：已锁定；
- Day1–Day129固定主课程：已完成设计；
- Day130–Day135：动态Foundation Cleanup；
- M00–M22详细Module Teaching Contract：已完成；
- 正式LAB：当前2个；
- Daily Lessons：按实际学习进度逐日生成；
- 正式学习尚未从Day1开始，后续可先做入口诊断压缩已掌握内容。
