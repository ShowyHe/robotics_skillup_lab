# 03_MASTER_PLAN — M00–M22 主课程总纲

> 本文件是整个课程的总索引。当前先锁定 Module 顺序和职责；**Day 数、Day1–DayN 仍未设计，不在本次落库中提前猜测。**

## 1. 主课程顺序

| Module | 名称 | 核心作用 |
|---|---|---|
| M00 | Robot Full-stack Architecture | 建立完整机器人系统视角、责任边界、Research / Safety / Evaluation 基本规则 |
| M01 | C++ / Linux / ROS2 Systems | 机器人工程运行时、ROS2架构、并发、通信、ros2_control |
| M02 | Mathematical Foundations I | 线代、微积分、Jacobian、Taylor、数值基础 |
| M03 | Sensors & Actuators | Camera/LiDAR/IMU/GNSS/Encoder、通信、同步、标定、执行器 |
| M04 | Robot Simulation Foundations | URDF、Gazebo/Isaac、传感器/控制仿真、repeatable scenario |
| M05 | Vision Geometry | 成像模型、内外参、投影/反投影、Stereo/RGB-D |
| M06 | Deep Learning Foundations | PyTorch、Backprop、CNN、Attention、Transformer基础 |
| M07 | Deep Vision & 3D Perception | Detection、Seg、Depth、PointCloud、3D、BEV、Occupancy |
| M08 | Mathematical Foundations II | 概率、Bayes、Covariance、优化、Least Squares、SO(3)/SE(3) |
| M09 | State Estimation | KF/EKF、模型、协方差传播、Wheel/IMU/GNSS融合 |
| M10 | SLAM / LIO / VIO / Factor Graph | ICP、优化、LiDAR/Visual-Inertial、图优化与工程失效分析 |
| M11 | Planning & Navigation | A*、Hybrid A*、RRT、Costmap、HPA、BT、Nav2/真实实现 |
| M12 | Robot Kinematics / Dynamics / System Dynamics | FK/IK/Jacobian、动力学、ODE、State-space |
| M13 | Control & Optimal Control | PID、稳定性、LQR、MPC、MPPI、约束与轨迹跟踪 |
| M14 | Manipulation | MoveIt2/OMPL、抓取、轨迹、力/阻抗控制 |
| M15 | Robot Learning | BC、IL、ACT、Diffusion Policy、Offline RL、Sim2Real基础 |
| M16 | VLM | Vision Encoder、语言、多模态融合、Grounding、场景理解 |
| M17 | VLA | Robot State/Action、Action Chunk、训练/微调/推理、Controller接口 |
| M18 | Mobile Manipulation | VLA + Navigation + Manipulation + Recovery 长时任务闭环 |
| M19 | Deployment / Data / Evaluation / Sim2Real | Orin、TensorRT、数据集、Benchmark、Failure Mining、数据回流 |
| M20 | Safety / Reliability / Owner | Watchdog、Fallback、安全边界、故障传播、系统决策 |
| M21 | Research Methodology & Capstone | Baseline、Hypothesis、Experiment、Ablation、可复现综合项目 |
| M22 | Foundation Cleanup | 根据全过程 Foundation Debt 补齐剩余理论缺口 |

---

## 2. 课程阶段

### Phase A — 系统与基础

`M00 → M04`

目标：建立机器人计算系统视角，补齐工程运行时、第一层数学、传感器与仿真基础。

### Phase B — 感知

`M05 → M07`

目标：从像素几何到 Deep Vision / 3D World Representation。

### Phase C — 估计与 SLAM

`M08 → M10`

目标：补概率、优化、SE(3)，再进入状态估计和 SLAM / LIO / VIO。

### Phase D — 规划、动力学与控制

`M11 → M13`

目标：规划负责“去哪/怎么走”，运动学动力学负责“机器人如何运动”，控制负责“如何稳定执行”。

### Phase E — Manipulation

`M14`

目标：建立从视觉目标、IK、规划到控制执行的机械臂完整链路。

### Phase F — Learning / VLM / VLA

`M15 → M17`

目标：从 demonstration 学 action，再建立 Vision-Language 理解，最终进入 Vision-Language-Action。

### Phase G — Embodied Full-stack

`M18 → M20`

目标：Mobile Manipulation、部署数据闭环、安全可靠性、Owner能力。

### Phase H — Research & Cleanup

`M21 → M22`

目标：完成硕士级 Capstone，并清理全过程留下的关键理论债务。

---

## 3. Day 规划规则

后续设计 Day 时遵守：

1. 普通 Day 默认 **2–3h**；
2. 理论知识是绝对主线；
3. Day 主要安排理论、数学、推导、算法、源码理解、案例映射、闭卷题；
4. 不要求每天代码或实验；
5. 必要 LAB / PROJECT 单独编号和安排；
6. 已掌握内容通过入门测试跳过；
7. 核心前置不过关时不机械推进；
8. Day 数由知识容量与掌握情况决定，而不是预先为了凑学期长度平均分配。

---

## 4. 后续 Day 索引的保存位置

本文件未来追加：

```text
M00: Day001–Day00x
M01: Day00x–Day00x
...
M22: Dayxxx–Dayxxx
```

以及总索引：

```text
Day001  ...
Day002  ...
...
DayNNN  ...
```

但具体每日讲义不写在这里。

- Module 详细 Day 规划：`docs/modules/Mxx_*.md`
- 每日详细讲义：`docs/lessons/dayXXX.md`
- 必要实验：`docs/labs/LABxx_*.md`
- 当前进度：`docs/PROGRESS.md`

---

## 5. 当前状态

- M00–M22 顺序：**已确认**；
- 能力树：**已确认**；
- 核心依赖：**已确认**；
- 每个 Module 的学习范围和毕业标准：**已完成第一版并确认**；
- 理论优先 / 实验按需：**已确认**；
- Day 数：**待设计**；
- Day1–DayN：**待设计**；
- 详细讲义：**待课程索引确认后逐日生成**。