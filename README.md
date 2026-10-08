# robotics_skillup_lab

面向 **机器人全栈工程 + 具身智能（Embodied AI）+ VLA** 的长期学习与能力升级仓库。

最终目标：

> **研究生级机器人理论基础 + 真实机器人全栈工程能力 + VLA / Mobile Manipulation具身智能能力 + 系统 Owner 能力。**

当前课程结构已完成审计并进入 **Curriculum v1.0结构冻结**：后续优先正式学习、考试和真实Foundation Debt修复，不再因为“还能再加知识”持续扩Module或Day。

## 1. 总体能力链
```text
Sensors / Actuators
        ↓
C++ / Linux / ROS2 Runtime
        ↓
Perception / World Representation
        ↓
State Estimation / SLAM
        ↓
Planning / Navigation
        ↓
Kinematics / Dynamics / Control
        ↓
Manipulation
        ↓
Robot Learning
        ↓
VLM / VLA
        ↓
Mobile Manipulation
        ↓
Deployment / Data / Evaluation / Safety
        ↓
Research / Capstone / Foundation Cleanup
```

## 2. 学习方式
采用 **“专业模块反推基础”**：
```text
真实问题 / 专业模块
→ 反推真正需要的数学 / CS / 物理基础
→ 先补前置
→ 学专业理论
→ 连接真实源码 / 官方实现
→ 必要时最小自实现或独立 LAB
→ Quiz / Module Graduation Exam
→ Foundation Debt 复测
```

正常学习日按 **2–3小时**设计。理论、数学、公式、算法、源码理解和真实案例映射是主线；**不要求每天代码，不要求每天实验**。

## 3. 源码学习原则
已有真实工程对应模块（尤其 Navigation / Control）：
```text
真实问题
→ 公司真实实现
→ 反推基础
→ 数学 / 算法本体
→ 官方标准实现
→ 必要最小自实现
→ 回真实系统验证
```

Robot Learning / VLM / VLA 等方向若没有公司实现，则以论文、官方项目、官方实现为主。源码数量不是目标。

## 4. Modern Robotics 教材集成

《Modern Robotics: Mechanics, Planning, and Control》（Kevin M. Lynch / Frank C. Park）正式作为运动学—动力学—控制—Manipulation主线的重要教材，但**不按整本书顺序从头刷到尾**。

```text
Modern Robotics Ch3        → M08  SE(3) / Rigid-Body Motion
Ch2 + Ch10                 → M11  C-space / Motion Planning（辅助）
Ch2–8 + Ch13               → M12  Screw / Twist / POE / Jacobian / IK / Dynamics / Mobile Robot
Ch9 + Ch11                 → M13  Trajectory / Robot Control
Ch9 + Ch12                 → M14  Trajectory / Grasping / Contact
```

课程取舍：
- **M12是本书最核心的落点**：Screw Axis、Twist、Wrench、POE、Adjoint、Space/Body Jacobian正式纳入；
- M08只完成SE(3)/Lie/Exp-Log/Perturbation前置，不提前硬塞完整Screw/Adjoint；
- M11仍以Nav2/HPA/Hybrid A*/Costmap等Navigation主线为核心；
- M13继续包含LQR/MPC/MPPI；
- M14继续以MoveIt2真实工程为核心，同时补Grasp Map / Force Closure理论；
- Closed-chain深推、BCH、复杂Lie Jacobian、完整Grasp Wrench Space等暂不作为硬门槛。

教材公式必须最终回到 **frame、dimension、源码变量、controller接口和真实robot behavior**。

## 5. 主课程与Day范围
```text
M00  Day1
M01  Day2–7
M02  Day8–15
M03  Day16–19
M04  Day20–21
M05  Day22–26
M06  Day27–33
M07  Day34–39
M08  Day40–48
M09  Day49–54
M10  Day55–62
M11  Day63–70
M12  Day71–79
M13  Day80–89
M14  Day90–96
M15  Day97–104
M16  Day105–109
M17  Day110–115
M18  Day116–118
M19  Day119–122
M20  Day123–126
M21  Day127–129
M22  Day130–135（动态 Foundation Cleanup）
```

固定主课程 Day1–Day129；M22预留动态槽位，具体内容必须根据真实Foundation Debt生成。

详见 [`docs/03_MASTER_PLAN.md`](docs/03_MASTER_PLAN.md)。

## 6. 当前学习进度

状态：`⬜ 未开始`、`🟨 进行中`、`✅ 完成 / PASS`、`⏸️ 路线调整暂缓`、`🔁 需要重学 / Retest`。

当前学习位置：**导航优先强化 + 定位突破专项 / M11 / Day064 — A* / Heuristic / Optimality / Anytime Search（启发式搜索 / 最优性 / 任意时刻搜索）**。Day063 已完成并通过，完整讲义见 `docs/lessons/day063.md`。M08 Day040–Day042 已完成并通过；Day043 已讲授但 Quiz 尚未完成，因路线调整标记为 ⏸️，将在 M11–M13 完成后回补。当前先完整学习 M11–M13（规划 → 运动学/动力学 → 控制），随后回到 M08 Day043–Day048，再进入 M09 State Estimation 与 M10 SLAM/LIO/VIO。M07、M06、M02、M03 的 Module Graduation Exam 仍按原状态暂缓。M04 Simulation 暂不作为当前专项前置主线。课程 Day 编号与 `03_MASTER_PLAN.md` 总结构不变。

### M00–M02｜系统基础与数学基础 I

| Day | 状态 | 主题 | 记录 |
|---|---|---|---|
| Day001 | ⬜ | Robot Full-stack Architecture | — |
| Day002 | ⬜ | C++ Lifetime / Ownership / RAII | — |
| Day003 | ⬜ | STL / Lambda / Callback | — |
| Day004 | ⬜ | Concurrency / Thread / Mutex | — |
| Day005 | ⬜ | Linux Runtime / Process / IPC | — |
| Day006 | ⬜ | ROS2 DDS / QoS | — |
| Day007 | ⬜ | Executor / Callback Group / Future / TF / Runtime Integration | — |
| Day008 | ✅ | Vector / Matrix / Dimension | [学习记录](docs/lessons/day008.md) |
| Day009 | ✅ | Basis / Coordinate / Transform | [学习记录](docs/lessons/day009.md) |
| Day010 | ✅ | Dot / Cross / Norm / Projection | [学习记录](docs/lessons/day010.md) |
| Day011 | ✅ | Eigen / Quadratic Form | [学习记录](docs/lessons/day011.md) |
| Day012 | ✅ | SVD / Rank / Conditioning | [学习记录](docs/lessons/day012.md) |
| Day013 | ✅ | Derivative / Differential / Numerical Integration | [学习记录](docs/lessons/day013.md) |
| Day014 | ✅ | Gradient / Chain Rule | [学习记录](docs/lessons/day014.md) |
| Day015 | ✅ | Jacobian / Hessian / Taylor / Linearization | [学习记录](docs/lessons/day015.md) |

### M03–M05｜传感器、仿真与视觉几何

| Day | 状态 | 主题 |
|---|---|---|
| Day016 | ✅ | Sensor Model / Noise / Bias / Measurement Quality | [学习记录](docs/lessons/day016.md) |
| Day017 | ✅ | IMU / Encoder / LiDAR / Camera | [学习记录](docs/lessons/day017.md) |
| Day018 | ✅ | GNSS / RTK / Time / Latency / Calibration | [学习记录](docs/lessons/day018.md) |
| Day019 | ✅ | Actuator / Command → Physical Motion | [学习记录](docs/lessons/day019.md) |
| Day020 | ⬜ | URDF / Robot Simulation Architecture |
| Day021 | ⬜ | Ground Truth / Domain Gap / Experiment Validity |
| Day022 | ✅ | Pinhole Camera | [学习记录](docs/lessons/day022.md) |
| Day023 | ✅ | Intrinsic / Extrinsic | [学习记录](docs/lessons/day023.md) |
| Day024 | ✅ | Distortion / Calibration | [学习记录](docs/lessons/day024.md) |
| Day025 | ✅ | Stereo / RGB-D / Depth | [学习记录](docs/lessons/day025.md) |
| Day026 | ✅ | Pixel → Camera → Base → World | [学习记录](docs/lessons/day026.md) |

### M06–M07｜深度学习与视觉 / 3D 感知

| Day | 状态 | 主题 |
|---|---|---|
| Day027 | ✅ | Tensor / Dataset / DataLoader | [学习记录](docs/lessons/day027.md) |
| Day028 | ✅ | Backprop / Autograd | [学习记录](docs/lessons/day028.md) |
| Day029 | ✅ | Training Loop / SGD / Adam | [学习记录](docs/lessons/day029.md) |
| Day030 | ✅ | Softmax / Cross Entropy | [学习记录](docs/lessons/day030.md) |
| Day031 | ✅ | CNN | [学习记录](docs/lessons/day031.md) |
| Day032 | ✅ | Generalization / Normalization / Distribution Shift | [学习记录](docs/lessons/day032.md) |
| Day033 | ✅ | Attention / Transformer | [学习记录](docs/lessons/day033.md) |
| Day034 | ✅ | Classification / Detection / YOLO / IoU / NMS | [学习记录](docs/lessons/day034.md) |
| Day035 | ✅ | Semantic / Instance Segmentation / Traversability | [学习记录](docs/lessons/day035.md) |
| Day036 | ✅ | Monocular Depth / Stereo / RGB-D / Learned Depth | [学习记录](docs/lessons/day036.md) |
| Day037 | ✅ | PointCloud / Filter / KD-tree / Clustering / 3D Detection | [学习记录](docs/lessons/day037.md) |
| Day038 | ✅ | BEV / Occupancy / 3D Representation | [学习记录](docs/lessons/day038.md) |
| Day039 | ✅ | Tracking / Metrics / Perception→Robot Integration | [学习记录](docs/lessons/day039.md) |

> M07 Module Graduation Exam：DEFERRED / 留空；当前不宣告 M07 模块毕业。

### M08–M10｜数学基础 II、状态估计、SLAM

| Day | 状态 | 主题 |
|---|---|---|
| Day040 | ✅ | Probability / Conditional / Bayes | [学习记录](docs/lessons/day040.md) |
| Day041 | ✅ | Expectation / Variance / Covariance / Gaussian | [学习记录](docs/lessons/day041.md) |
| Day042 | ✅ | Likelihood / MLE / MAP | [学习记录](docs/lessons/day042.md) |
| Day043 | ⏸️ | Residual / LS / WLS |
| Day044 | ⬜ | Nonlinear LS / Newton |
| Day045 | ⬜ | Gauss-Newton / LM / Robust |
| Day046 | ⬜ | Rotation / Euler / Quaternion |
| Day047 | ⬜ | SO(3) / SE(3) / Exp-Log / Perturbation / M12 Bridge |
| Day048 | ⬜ | Probability + Optimization + SE(3) Integration |
| Day049 | ⬜ | State / Motion / Measurement / Q-R-P / Bayesian Filter |
| Day050 | ⬜ | Kalman Prediction |
| Day051 | ⬜ | Kalman Update / Innovation / Gain |
| Day052 | ⬜ | EKF / Linearization / Error-state Intro |
| Day053 | ⬜ | Wheel / IMU / GNSS / RTK Fusion |
| Day054 | ⬜ | Observability / Gating / Dropout / Reset / Debug |
| Day055 | ⬜ | SLAM Architecture / Frontend / Backend |
| Day056 | ⬜ | ICP / Correspondence / Point-to-plane / GN |
| Day057 | ⬜ | IMU Propagation / Bias / Gravity / Preintegration |
| Day058 | ⬜ | LIO / Deskew / Scan-to-map / Error-State / Latency |
| Day059 | ⬜ | VIO / Feature / Reprojection Residual |
| Day060 | ⬜ | Factor Graph / Pose Graph / Information |
| Day061 | ⬜ | Loop Closure / Global Consistency / LIO Boundary |
| Day062 | ⬜ | Degeneracy / Observability / Latency / TF / Initialization / Bag Debug |

### M11–M13｜规划、运动学 / 动力学、控制

| Day | 状态 | 主题 |
|---|---|---|
| Day063 | ✅ | Graph / BFS / Dijkstra |
| Day064 | 🟨 | A* / Heuristic / Anytime Search |
| Day065 | ⬜ | Occupancy / Costmap / Footprint / Inflation |
| Day066 | ⬜ | Hybrid A* / Motion Primitive / Kinodynamic Boundary |
| Day067 | ⬜ | Configuration Space / C-obstacle / RRT / RRT* / OMPL |
| Day068 | ⬜ | HPA / Incremental Search / Dynamic Edge |
| Day069 | ⬜ | Nav2 / BT / Replanning / Path Switching |
| Day070 | ⬜ | Dynamic Navigation / Path Quality / Trajectory Bridge / Planning Owner |
| Day071 | ⬜ | Configuration / Screw Axis / Twist / Wrench |
| Day072 | ⬜ | Forward Kinematics / POE Mainline / DH |
| Day073 | ⬜ | Space-Body Jacobian / Adjoint / Velocity Kinematics / Statics |
| Day074 | ⬜ | Inverse Kinematics / Newton / DLS |
| Day075 | ⬜ | Singularity / SVD / Manipulability / Redundancy |
| Day076 | ⬜ | Wheeled Mobile Kinematics / Nonholonomic Constraint |
| Day077 | ⬜ | Force / Wrench / Inertia / Newton-Euler |
| Day078 | ⬜ | Lagrangian / Manipulator Dynamics / Forward-Inverse Dynamics |
| Day079 | ⬜ | ODE / State-space / Linearization / Discretization / Action |
| Day080 | ⬜ | Trajectory / Trajectory Optimization / Time Scaling / Feedback / PID |
| Day081 | ⬜ | Equilibrium / Stability / Eigenvalue |
| Day082 | ⬜ | Controllability / Observability |
| Day083 | ⬜ | Optimal Control / LQR |
| Day084 | ⬜ | MPC / Horizon / Constraint / Receding Horizon |
| Day085 | ⬜ | MPPI / Sampling / Rollout / Weight / Update |
| Day086 | ⬜ | MPPI Noise / Temperature / Warm Start |
| Day087 | ⬜ | Cost / Critic / Constraint / Behavior |
| Day088 | ⬜ | Tracking / Latency / Saturation / Model Mismatch |
| Day089 | ⬜ | Control Owner / MPPI Source |

### M14–M15｜Manipulation 与 Robot Learning

| Day | 状态 | 主题 |
|---|---|---|
| Day090 | ⬜ | Manipulation Architecture / RobotState / MoveIt2 |
| Day091 | ⬜ | Planning Scene / Collision / Attached Object |
| Day092 | ⬜ | OMPL / Joint-Cartesian Motion / Trajectory / Time Parameterization |
| Day093 | ⬜ | Object Pose / Grasp Pose / Pre-grasp |
| Day094 | ⬜ | Contact / Friction Cone / Wrench / Grasp Map / Closure / Verification |
| Day095 | ⬜ | Cartesian / Force / Impedance Control |
| Day096 | ⬜ | Pick-and-Place / Recovery / Owner |
| Day097 | ⬜ | MDP / Return / V-Q-A / Bellman |
| Day098 | ⬜ | PPO / TD3 / On-policy vs Off-policy Review |
| Day099 | ⬜ | IL / BC / DAgger / Covariate Shift |
| Day100 | ⬜ | Robot Dataset / Temporal Alignment / Action Semantics |
| Day101 | ⬜ | ACT / Action Chunking / CVAE / Temporal Aggregation |
| Day102 | ⬜ | Diffusion Policy / Conditional Denoising / Multimodal Action |
| Day103 | ⬜ | Offline RL / Dataset Support / OOD |
| Day104 | ⬜ | Policy Evaluation / Sim2Real / Owner |

### M16–M18｜VLM、VLA、Mobile Manipulation

| Day | 状态 | 主题 |
|---|---|---|
| Day105 | ⬜ | Token / Embedding / Autoregressive LM |
| Day106 | ⬜ | Vision Encoder / Patch Token |
| Day107 | ⬜ | CLIP / Contrastive Alignment |
| Day108 | ⬜ | Projector / Cross Attention / Fusion |
| Day109 | ⬜ | Grounding / Spatial Reasoning / Hallucination / Robot Interface |
| Day110 | ⬜ | VLA Architecture / Proprioception / Action Representation |
| Day111 | ⬜ | VLA Dataset / Sequence / Temporal Context |
| Day112 | ⬜ | Action Token / Autoregressive Action |
| Day113 | ⬜ | Action Chunk / Closed-loop Execution |
| Day114 | ⬜ | Training / Fine-tuning / Action Head / Embodiment |
| Day115 | ⬜ | Evaluation / Safety / Generalization / Owner |
| Day116 | ⬜ | Base + Arm Geometry / Reachability / Base Placement |
| Day117 | ⬜ | Nav→Manipulation Handoff / Re-perception / Whole-body / Long-horizon State |
| Day118 | ⬜ | End-to-End Mobile Manipulation / VLA Integration / Owner |

### M19–M21｜部署、评测、安全与研究

| Day | 状态 | 主题 |
|---|---|---|
| Day119 | ⬜ | Orin / CUDA-TensorRT / Runtime / Profiling / Latency |
| Day120 | ⬜ | Robot Data Pipeline / Logging / Version / Failure Mining / Data Reflow |
| Day121 | ⬜ | Evaluation / Benchmark / Regression / Ablation |
| Day122 | ⬜ | Sim2Real / Progressive Deployment / Monitoring / Rollback / Owner |
| Day123 | ⬜ | Hazard / Risk / Safety Requirement / Fail-safe |
| Day124 | ⬜ | Fault / Failure / Watchdog / Interlock / E-stop / Human Takeover |
| Day125 | ⬜ | Reliability / Redundancy / Degraded Mode / FMEA / Fault Tree |
| Day126 | ⬜ | Incident / Safety Case / Fault Injection / Release Gate / Owner |
| Day127 | ⬜ | Problem Definition / Hypothesis / Baseline / Literature |
| Day128 | ⬜ | Experimental Design / Ablation / Statistics / Reproducibility |
| Day129 | ⬜ | Technical Report / Capstone Defense / Contribution / Research Owner |

### M22｜动态 Foundation Cleanup

| Day | 状态 | 主题 |
|---|---|---|
| Day130 | ⬜ | Dynamic Foundation Debt #1 |
| Day131 | ⬜ | Dynamic Foundation Debt #2 |
| Day132 | ⬜ | Dynamic Foundation Debt #3 |
| Day133 | ⬜ | Dynamic Foundation Debt #4 |
| Day134 | ⬜ | Dynamic Foundation Debt #5 |
| Day135 | ⬜ | Dynamic Foundation Debt #6 |

> README 只用于快速看全局进度；详细掌握情况、错误理解、复测和 Foundation Debt 仍以 [`docs/PROGRESS.md`](docs/PROGRESS.md) 为准。当天完整讲义只在真正学习并结项后写入 `docs/lessons/dayXXX.md`。

## 7. 文档导航与职责

### 顶层文档
- [`00_GOALS.md`](docs/00_GOALS.md)：最终学习目标、课程边界与验收方向；
- [`01_COMPETENCY_MAP.md`](docs/01_COMPETENCY_MAP.md)：机器人全栈能力地图与能力层级；
- [`02_DEPENDENCIES.md`](docs/02_DEPENDENCIES.md)：模块、知识点与前置能力依赖关系；
- [`03_MASTER_PLAN.md`](docs/03_MASTER_PLAN.md)：总Day数、Module范围、总索引与教材映射；
- [`04_MODULE_SPECS.md`](docs/04_MODULE_SPECS.md)：各Module知识边界、Teaching Contract与毕业能力；
- [`LEARNING_RULES.md`](docs/LEARNING_RULES.md)：学习、讲义、考试、复测与记录规则；
- [`PROGRESS.md`](docs/PROGRESS.md)：当前学习状态、错误理解、复测与Foundation Debt；
- [`TEMP_POSITIONING_VISION_PLAN.md`](docs/TEMP_POSITIONING_VISION_PLAN.md)：当前“定位 + 视觉理论专项”的阶段执行计划。

### 目录入口
- [`docs/modules/`](docs/modules/)：M00–M22各Day的Teaching Contract，不是当天长篇讲义；
- [`docs/lessons/`](docs/lessons/)：真正学习并结项后生成的逐日完整讲义；
- [`docs/labs/`](docs/labs/)：不可被理论替代的正式LAB与Capstone。

README只负责快速看全局和当前进度；长期规划看 `03_MASTER_PLAN.md`，真实掌握情况看 `PROGRESS.md`，当前专项执行看 `TEMP_POSITIONING_VISION_PLAN.md`。

## 8. 掌握等级
- **L1：见过**
- **L2：能解释**
- **L3：能计算 / 推导**
- **L4：能实现 / Debug**
- **L5：能迁移 / 修改 / 设计**

核心数学至少L3；核心机器人专业理论L3→L4；Navigation / Control / System Owner / VLA关键能力逐步向L5。

## 9. 统一考试结构
普通 Module Graduation Exam 默认：
- **30% 核心基础**；
- **50% 综合系统场景**；
- **20% Source / Formula / Design**。

默认总分 **≥85%**；Hard Gate基础概念不能靠其他题得分抵消。M00为1-Day总纲，保留轻量Owner场景考核例外。

## 10. 正式LAB
当前锁定3个：
- `docs/labs/LAB01_manipulation_pick_and_place.md`：Manipulation全链；
- `docs/labs/LAB02_mobile_manipulation_capstone.md`：Mobile Manipulation端到端，并可扩展为M21最终Research Capstone；
- `docs/labs/LAB03_robot_policy_vla_action_interface.md`：Robot Learning/VLA的Dataset→Policy→Action→Safety→Controller闭环。

LAB用于验证真实闭环、frame、collision、execution、contact、learned action、recovery和跨模块Owner能力，不为了课程形式凑数量。

## 11. 当前关键理论强化
本轮教材集成不增加Day，只强化既有主线：
- M08：明确SE(3)是M12 Screw/POE体系前置；
- M11：补 `C_free / C_obs / configuration validity`；
- M12：Screw / Twist / Wrench / POE / Adjoint / Space-Body Jacobian成为核心；
- M13：补Path→Timed Trajectory→Reference→Controller桥梁；
- M14：补Friction Cone / Wrench / Grasp Map / Form-Force Closure理论。

## 12. 最终验收
课程完成不以“看完多少Day”为标准，而以是否能够：
1. 从数学/物理解释核心机器人算法；
2. 从sensor一路追到真实action/physical motion；
3. 判断跨模块故障责任；
4. 读懂并修改关键真实/官方实现；
5. 建立Perception→Estimation→Planning→Control→Manipulation→VLA完整联系；
6. 真正理解并使用SE(3)、Screw/Twist、POE、Jacobian、Dynamics、Trajectory、Grasp/Contact理论；
7. 完成Mobile Manipulation / Embodied AI Capstone；
8. 真正跑通至少一次learned policy/VLA action→Safety→Controller闭环；
9. 建立数据、部署、评测、失败回流、Safety、Regression闭环；
10. 对证据不足的问题明确说“当前不能确定”，并指出下一步需要的证据。