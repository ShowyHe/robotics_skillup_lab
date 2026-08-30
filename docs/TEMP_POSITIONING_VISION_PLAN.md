# TEMP_POSITIONING_VISION_PLAN — 定位 + 视觉理论专项学习计划

> 状态：ACTIVE（临时学习主线）
>
> 目的：在不修改 Curriculum v1.0 主课程结构、不压缩核心理论的前提下，优先系统补齐 **机器人定位 + 机器人视觉 / 纯视觉 BEV** 所需理论。
>
> 本文件是当前阶段的学习导航索引，不替代 `03_MASTER_PLAN.md`、`04_MODULE_SPECS.md`、`modules/`、`LEARNING_RULES.md` 和 `PROGRESS.md`。

---

## 1. 当前学习目标

当前阶段不以项目交付日期倒推课程，不以“尽快跑通工程”为学习完成标准。

目标是建立两条完整理论主线：

```text
视觉主线
Mathematical Foundations I
→ Sensors / Camera
→ Vision Geometry
→ Deep Learning Foundations
→ Detection / Segmentation / Depth
→ PointCloud / Occupancy / BEV
→ Costmap / Navigation Consumer
```

```text
定位主线
Mathematical Foundations I
→ Sensors / IMU / GNSS / RTK / Time
→ Probability / Covariance / Optimization / SE(3)
→ KF / EKF / Multi-sensor Fusion
→ LIO / VIO / SLAM / Factor Graph
```

两条主线最终在以下问题上汇合：
- coordinate frame / TF；
- timestamp / latency / synchronization；
- intrinsic / extrinsic calibration；
- uncertainty / covariance；
- SE(3) pose；
- world representation；
- perception / localization 输出如何被 Navigation 消费。

---

## 2. 学习规则

本专项完全遵守 `docs/LEARNING_RULES.md`：

1. 正常学习日按 2–3 小时设计；
2. 理论理解 > 数学/公式 > 算法机制 > 源码理解 > 工程映射 > 必要实现/实验；
3. 不要求每天写代码，不要求每天做 LAB；
4. 数学公式必须说明变量维度、物理/几何意义、frame/convention；
5. 已有工程经验用于映射理论，但不能替代数学与算法 Hard Gate；
6. 未读取的公司源码不得臆测；本专项理论学习不依赖公司源码；
7. 每个 Module 可做入口诊断，但核心数学掌握不足时不得跳过；
8. Daily Quiz 与 Module Graduation Exam 仍按原课程规则执行；
9. 学习结果与薄弱点继续记录到 `PROGRESS.md`；
10. 本文件只改变当前学习优先顺序，不改变原 Module Teaching Contract。

---

## 3. 专项学习顺序

### Phase 1 — Mathematical Foundations I
**对应：M02 / Day8–Day15**

目标：补齐后续视觉几何、状态估计、优化和 SLAM 共同依赖的数学底座。

重点：
- vector / matrix / coordinate；
- linear transform；
- derivative / partial derivative；
- gradient；
- Jacobian；
- Taylor expansion；
- eigen / SVD 等原课程要求。

计划：**8 个理论 Day**。

毕业要求：核心数学至少 L3，能在机器人问题中正确做 dimension / frame / derivative reasoning。

---

### Phase 2 — Sensors / GNSS / RTK / Camera / Time
**对应：M03 / Day16–Day19**

定位重点：
- measurement ≠ truth；
- noise / bias / drift / covariance；
- IMU、Wheel、GNSS / RTK；
- RTK FIX / FLOAT；
- single-antenna position ≠ reliable static heading；
- timestamp / latency / jitter / synchronization；
- intrinsic / extrinsic；
- command ≠ actual feedback。

视觉重点：
- Camera 直接测量的是 image / pixel；
- 3D / semantic / depth 都是后续估计；
- Camera 与 robot frame 的外参关系。

计划：**4 个理论 Day**。

---

### Phase 3 — Vision Geometry
**对应：M05 / Day22–Day26**

这是纯视觉导航不可跳过的几何底座。

必须掌握：
- pinhole camera model；
- normalized coordinate；
- intrinsic matrix `K`；
- extrinsic；
- distortion / calibration / reprojection error；
- stereo / depth；
- pixel + depth → camera-frame 3D；
- Camera → Base → World；
- transform direction；
- timestamp alignment；
- visual geometry error chain。

计划：**5 个理论 Day**。

核心目标：能够独立解释并计算

```text
Pixel
→ Ray / Depth
→ Camera 3D
→ Base 3D
→ World 3D
```

---

### Phase 4 — Deep Learning Foundations
**对应：M06 / Day27–Day33**

目标不是只会调用模型，而是读懂视觉模型的训练与推理逻辑。

重点：
- tensor / shape；
- dataset / dataloader；
- model / parameter / hyperparameter；
- loss；
- computational graph；
- chain rule / backpropagation；
- optimizer；
- CNN；
- classification / generalization；
- train / validation / inference；
- Transformer 相关基础按原 M06 Contract 学习。

计划：**7 个理论 Day**。

---

### Phase 5 — Deep Vision / 3D Perception / BEV
**对应：M07 / Day34–Day39**

这是当前视觉专项核心模块。

学习链：

```text
Image
→ Detection / Segmentation
→ Depth
→ 3D Geometry / PointCloud
→ Occupancy / Semantic BEV
→ Traversability
→ Costmap / World Model
```

必须掌握：
- Detection / YOLO / IoU / NMS；
- Semantic / Instance Segmentation；
- traversability；
- metric vs relative depth；
- PointCloud / voxel / clustering；
- occupied / free / unknown；
- unknown ≠ free；
- BEV vs Occupancy vs Costmap；
- Semantic BEV → rule/fusion → Costmap；
- temporal fusion / ego-motion；
- perception freshness / tracking / failure attribution。

计划：**6 个理论 Day**。

纯视觉导航核心链：

```text
RGB
→ Segmentation / Depth
→ Semantic BEV
→ Traversability / Occupancy
→ Costmap
→ Planner / Controller
```

---

### Phase 6 — Probability / Optimization / SO(3) / SE(3)
**对应：M08 / Day40–Day48**

这是定位主线的核心数学模块，不做日期压缩。

必须掌握：
- Probability / Conditional Probability / Bayes；
- Expectation / Variance / Covariance / Gaussian；
- Likelihood / MLE / MAP；
- Residual / LS / WLS / Information；
- Nonlinear Least Squares；
- Gauss-Newton / LM / Robust Loss；
- Rotation Matrix / Quaternion；
- SO(3) / SE(3)；
- Exp / Log；
- pose perturbation；
- residual → Jacobian → weighted optimization → pose update。

计划：**9 个理论 Day**。

核心 Hard Gate：
`Covariance ≠ Actual Error`，并能够把 `Σ^-1`、Jacobian、GN、SE(3) update 解释到机器人定位问题。

---

### Phase 7 — State Estimation
**对应：M09 / Day49–Day54**

学习链：

```text
State / Motion Model / Measurement Model
→ Prediction
→ Covariance Propagation
→ Innovation
→ Kalman Gain
→ Update
→ EKF
→ Multi-sensor Fusion
→ Gating / Reset / Health
```

必须掌握：
- `x / P / F / G / Q / H / R / K`；
- KF prediction / update；
- EKF Jacobian / linearization；
- Wheel / IMU / GNSS / RTK multi-rate fusion；
- FIX / FLOAT quality；
- timestamp / frame；
- observability；
- innovation / Mahalanobis gating；
- dropout / recovery / reset；
- covariance consistency。

计划：**6 个理论 Day**。

---

### Phase 8 — SLAM / LIO / VIO / Factor Graph
**对应：M10 / Day55–Day62**

这是定位理论向完整机器人定位系统的收口模块。

必须掌握：
- Localization / Odometry / Mapping / SLAM；
- frontend / backend；
- ICP / correspondence / point-to-plane；
- GN update；
- IMU propagation / bias / gravity；
- LIO / deskew / scan-to-map；
- VIO / feature / reprojection residual；
- monocular scale ambiguity；
- Factor Graph / Information Matrix；
- loop / global consistency boundary；
- observability vs geometric degeneracy；
- time / initialization / calibration / TF；
- bag/log evidence chain。

计划：**8 个理论 Day**。

---

### Phase 9 — BEV → Navigation Interface
**对应：M11 重点学习 Day65，并复盘 M07 Day38–39**

本阶段不重新完整学习 Navigation，而是把视觉和定位结果接到规划系统责任边界。

重点：
- Occupancy vs Costmap；
- free / occupied / unknown；
- footprint / inflation / safety margin；
- BEV/world representation 到 planner 的数据语义；
- localization pose 与 perception world frame 对齐；
- stale BEV / stale pose；
- Perception / Localization / Costmap / Planner / Controller failure attribution。

计划：**1–2 个理论 Day**。

---

## 4. 总体学习量

理论主线预计：

| Phase | Module | Days |
|---|---|---:|
| 1 | M02 | 8 |
| 2 | M03 | 4 |
| 3 | M05 | 5 |
| 4 | M06 | 7 |
| 5 | M07 | 6 |
| 6 | M08 | 9 |
| 7 | M09 | 6 |
| 8 | M10 | 8 |
| 9 | M11局部 | 1–2 |
| **总计** |  | **54–55** |

这里的 Day 是课程逻辑索引，不等于必须连续 54–55 个自然日；若入口诊断证明某项已达到原 Module Hard Gate，可按 `LEARNING_RULES.md` 规则跳过已稳定掌握部分。

---

## 5. 当前执行顺序

```text
M02 Day8–15
→ M03 Day16–19
→ M05 Day22–26
→ M06 Day27–33
→ M07 Day34–39
→ M08 Day40–48
→ M09 Day49–54
→ M10 Day55–62
→ M11 Day65 + M07 Day38–39复盘
```

M04 Simulation 暂不作为本专项前置主线；若后续视觉/定位实验确实需要仿真，再按原课程补回。

---

## 6. 每日执行方式

每个学习 Day：

```text
读取对应 Module Teaching Contract
→ 必要入口检查
→ 正式教学（核心点≤20）
→ 数学 / 公式 / frame / dimension 推理
→ 机器人实际场景映射
→ Daily Quiz
→ 纠错 / targeted remediation
→ 更新 PROGRESS
```

学习中禁止仅靠“看懂解释”判定掌握；定位和视觉核心内容应至少达到：
- 数学基础：L3；
- Vision Geometry：L3–L4；
- BEV / Perception integration：L4；
- State Estimation：L4；
- SLAM / LIO / VIO owner reasoning：L4–L5。

---

## 7. 当前起点

从 **M02 / Day8** 开始。

若 Day8–Day15 中某项通过当天入口诊断，可定向压缩，但不提前假定已掌握。

当前专项完成标准不是日期，而是：
1. 能用数学解释定位算法，而不仅能调参数；
2. 能从 sensor measurement 追到 fused pose 与 uncertainty；
3. 能解释 KF / EKF / LIO / VIO / Factor Graph 的统一估计思想与边界；
4. 能从 pixel 追到 camera/base/world geometry；
5. 能解释 Detection / Segmentation / Depth / BEV / Occupancy / Costmap 的职责边界；
6. 能构造 `RGB → Semantic BEV → Costmap → Navigation` 的完整理论链；
7. 能从 frame / timestamp / calibration / uncertainty / stale data 角度定位视觉与定位系统错误；
8. 对证据不足的问题明确保留结论，不把模型输出、covariance 或 topic frequency直接等同真实世界正确性。
