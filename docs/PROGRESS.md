# PROGRESS — 当前课程设计与学习状态

## 1. 当前阶段

当前处于：**导航优先强化 + 定位突破专项学习（ACTIVE）**。
专项导航文件：`docs/TEMP_POSITIONING_VISION_PLAN.md`。

```text
M02 Day8  — Vector / Matrix / Dimension：COMPLETED / PASS
M02 Day9  — Basis / Coordinate / Linear Transformation：COMPLETED / PASS
M02 Day10 — Dot / Cross / Norm / Projection / Geometry：COMPLETED / PASS
M02 Day11 — Eigenvalue / Eigenvector / Quadratic Form：COMPLETED / PASS
M02 Day12 — SVD / Rank / Conditioning：COMPLETED / PASS
M02 Day13 — Derivative / Differential / Numerical Integration：COMPLETED / PASS
M02 Day14 — Partial Derivative / Gradient / Chain Rule：COMPLETED / PASS
M02 Day15 — Jacobian / Hessian / Taylor / Linearization：COMPLETED / PASS
M02 Module Graduation Exam：INCOMPLETE / DEFERRED（第8题与部分综合题待回补）
M03 Day16 — Sensor Model / Noise / Bias / Measurement Quality：COMPLETED / PASS
M03 Day17 — IMU / Encoder / LiDAR / Camera：COMPLETED / PASS
M03 Day18 — GNSS / RTK / Timestamp / Latency / Synchronization / Calibration：COMPLETED / PASS
M03 Day19 — Actuator / Motor Loop / Communication / Command→Motion：COMPLETED / PASS
M03 Module Graduation Exam：DEFERRED（后续遗忘后回测）
M05 Day22 — Pinhole Camera Model：COMPLETED / PASS
M05 Day23 — Intrinsic / Extrinsic / Full 3D→Pixel Projection：COMPLETED / PASS
M05 Day24 — Lens Distortion / Camera Calibration：COMPLETED / PASS
M05 Day25 — Stereo / RGB-D / Depth Geometry：COMPLETED / PASS
M05 Day26 — Pixel→Camera→Base→World：COMPLETED / PASS
M06 Day27 — Neural Network / Tensor / Dataset / DataLoader / Loss：COMPLETED / PASS
M06 Day28 — Backpropagation / Computational Graph / Autograd：COMPLETED / PASS
M06 Day29 — Training Loop / Gradient Descent / SGD / Momentum / Adam：COMPLETED / PASS
M06 Day30 — Classification / Logit / Softmax / Cross Entropy：COMPLETED / PASS
M06 Day31 — CNN Foundations：COMPLETED / PASS
M06 Day32 — Generalization / Normalization / Distribution Shift：COMPLETED / PASS
M06 Day33 — Attention / Transformer Foundations：COMPLETED / PASS
M06 Module Graduation Exam：DEFERRED（用户选择留空，后续回测）
M07 Day34 — Classification / Detection / YOLO / IoU / NMS：COMPLETED / PASS
M07 Day35 — Semantic / Instance Segmentation / Traversability：COMPLETED / PASS
M07 Day36 — Monocular Depth / Stereo / RGB-D / Learned Depth：COMPLETED / PASS
M07 Day37 — PointCloud / Filtering / KD-tree / Clustering / 3D Detection：COMPLETED / PASS
M07 Day38 — Voxel Representation / Occupancy / BEV / Costmap Boundary：COMPLETED / PASS
M07 Day39 — Tracking / Metrics / Perception→Robot Integration：COMPLETED / PASS
M07 Module Graduation Exam：DEFERRED（用户选择留空，后续回测；当前不宣告 M07 模块毕业）
M08 Day40 — Probability / Conditional Probability / Bayes：COMPLETED / PASS
M08 Day41 — Expectation / Variance / Covariance / Gaussian：COMPLETED / PASS
M08 Day42 — Likelihood / MLE / MAP：COMPLETED / PASS
M08 Day43 — Residual / Least Squares / Weighted Least Squares：PAUSED / NOT COMPLETED（已教学，Quiz 尚未完成；路线调整后回补）
M08 Day44–48：DEFERRED BY ROUTE CHANGE
M11 Day63 — Graph / BFS / Dijkstra：COMPLETED / PASS（见 docs/lessons/day063.md）
Current / Next：M11 Day64 — A* / Heuristic / Optimality / Anytime Search
```

> 重要：M02 Day8–15 的每日课程已完成，但 **M02 模块毕业考试尚未完成，因此不宣告 M02 模块毕业**。用户选择先继续 M03，后续回补考试剩余题目与 Hard Gate 复测。

当前专项执行顺序已按能力收益调整：

```text
已完成基础：
M02 Day8–15
→ M03 Day16–19
→ M05 Day22–26
→ M06 Day27–33
→ M07 Day34–39
→ M08 Day40–42

当前主线：
M11 Day63–70
→ M12 Day71–79
→ M13 Day80–89
→ M08 Day43–48
→ M09 Day49–54
→ M10 Day55–62
```

说明：只改变当前执行顺序，不改变 Curriculum v1.0 的 Module / Day 编号。Navigation 作为主攻强项目标 L4→L5；Localization / State Estimation / SLAM 作为最大能力门槛，在 M11–M13 后集中突破。

M04 Simulation 暂不作为当前专项前置主线。

---

## 2. 学习方式与表达规则

- 正常学习日按 2–3h 设计；理论、数学、公式、算法与源码理解优先。
- 不要求每天代码或 LAB；必要 LAB 独立安排。
- 中文术语为主，英文首次出现时括号注释，例如：雅可比矩阵（Jacobian）、海森矩阵（Hessian）。
- 题目必须独立列全已知条件、变量维度、单位和所求量。
- 物理意义优先用真实机器人、传感器、几何对象解释；若连续两次未懂必须更换讲解方式。
- 已稳定掌握内容不机械重考；错误项做 targeted remediation / retest。
- Daily Quiz 按 `LEARNING_RULES.md`：通常 5–10 题；知识点多时可以更多。Day Goal 优先于知识点数量，不再设置“核心知识点≤20”的硬上限；复杂 Day 可按需要扩展，但不拆 Day。

---

## 3. 当前正式 LAB

- LAB01 — Manipulation Pick-and-Place
- LAB02 — Mobile Manipulation Capstone
- LAB03 — Robot Policy / VLA Action Interface

---

## 4. M02 模块考试状态

M02 Graduation Exam 默认结构：30% 核心基础、50% 综合系统场景、20% Source / Formula / Design；总分 ≥85%，Hard Gate 基础错误不能靠其他题抵消。

当前考试已完成第1–7题的大部分作答，第8题与部分综合纠错尚未完成。考试结果暂不评分、不判定毕业。

已暴露、后续需在回补考试中复测的重点：

- 矩阵维度与雅可比输出行/输入列。
- 条件数、奇异值与最大拉伸。
- 二次型正定性。
- 点到直线法向残差的点积物理意义。
- 海森完整矩阵与交叉偏导。
- 链式法则中内层导数。
- 局部线性化边界。

---

## 5. Foundation Debt

当前 **无未关闭 P0/P1**。保留 P2 Review Debt，不因每日课程 PASS 自动关闭。

### P2 — Day11 特征分解 / 二次型

```text
Knowledge: Eigen diagonalization / basis change
Wrong / Weak Understanding: 容易从 Av=λv 直接跳到 diagonal matrix
Retest: V=[v1...vn] → AV=VΛ → V^-1AV=Λ，并解释 off-diagonal 为0的原因
Status: OPEN
```

```text
Knowledge: Covariance ellipse / quadratic cost / Hessian curvature
Wrong / Weak Understanding: 三种 eigenvalue 的物理含义容易混
Retest: 分别解释 covariance、LQR cost、Hessian 中 eigenvector/eigenvalue
Status: OPEN
```

```text
Knowledge: Eigenvalue vs Singular Value
Wrong / Weak Understanding: 曾把一般矩阵最大 eigenvalue 当作最大拉伸
Retest: 解释为何一般最大拉伸看 singular value
Status: RETEST
```

```text
Knowledge: Positive definite quadratic cost
Wrong / Weak Understanding: 定义与 cost 直觉已理解，但独立复测不足
Retest: 解释 x!=0 => x^TQx>0 与 quadratic cost 的关系
Status: OPEN
```

### P2 — Day12 SVD / Conditioning

```text
Knowledge: Small singular value and inverse sensitivity
Wrong / Weak Understanding: 曾把弱方向 noise amplification 理解成串到其他方向
Retest: 在 LS / Jacobian / SLAM 场景解释 Δx≈Δb/σ，明确同一弱奇异方向
Status: OPEN
```

```text
Knowledge: Invertibility vs numerical conditioning
Wrong / Weak Understanding: 初始混淆可逆性与 eigenvector orthogonality
Retest: 给 singular spectrum 判断 rank / invertibility / condition number / reliability
Status: OPEN
```

### P2 — Day13 微分 / 积分 / 采样

```text
Knowledge: dθ vs dθ/dt
Wrong / Weak Understanding: 曾把 dθ 当作角速度
Retest: 在后续 IMU/局部变化场景检查 rad / rad/s
Status: RETEST
```

```text
Knowledge: Integration units and sampling frequency
Wrong / Weak Understanding: 曾把固定 bias 按采样次数累计；单位曾误写
Retest: Σb_vΔt=b_vT；解释同一 T 下高频不自动增加固定 bias 误差
Status: OPEN
```

### P2 — Day14 梯度 / 链式法则

```text
Knowledge: Multivariable chain rule / inner derivative
Wrong / Weak Understanding: 内层系数容易漏乘
Retest: 新参数→状态→代价链独立求导并检查单位
Status: RETEST
```

```text
Knowledge: Gradient descent sign
Wrong / Weak Understanding: 曾混淆变量大小、代价下降和负梯度符号
Retest: 新工作点独立判断更新方向与代价变化
Status: RETEST
```

### P2 — Day15 雅可比 / 海森 / 线性化

```text
Knowledge: Jacobian dimensions
Wrong / Weak Understanding: 多次将 m×n 写反，机械臂4→3初次写反
Retest: 新向量模型中独立求雅可比并检查乘法维度
Status: OPEN
```

```text
Knowledge: Hessian matrix
Wrong / Weak Understanding: 曾只写对角元素，漏交叉偏导；维度曾写错
Retest: 对含交叉项标量函数求完整海森并解释曲率
Status: OPEN
```

```text
Knowledge: Jacobian vs gradient vs Hessian / local linearization
Wrong / Weak Understanding: 曾把雅可比泛称为梯度矩阵，把梯度非零与非线性混同
Retest: 区分向量模型与标量代价的导数，并说明局部二阶导为0不能证明全局线性
Status: RETEST
```

### P2 — Day16 协方差 / 标准差

```text
Knowledge: covariance / variance / standard deviation
Exposed At: M03 / Day16
Wrong / Weak Understanding: 初次把0.0004 m²开平方成0.002 m，并把covariance表述为简单置信度阈值
Debt Type: Calculation / Definition / Transfer
Priority: P2
Current Level: L2-L3（定向复测通过）
Target Level: L3
Retest: M03 Graduation Exam 在新 GNSS / IMU 场景区分 variance / standard deviation / actual error
Status: RETEST
```

---

## 6. Day16 Learning Record

```text
Current Module / Day:
M03 / Day16 — Measurement / Accuracy / Precision / Resolution / Noise / Bias — COMPLETED / PASS

Mastered:
- z=h(x)+e：truth、ideal measurement model、measurement、error
- accuracy 与 precision 的区别；能解释“稳定但整体偏”
- resolution 不等于 accuracy
- noise / bias / drift / outlier 的工程区别
- 400Hz发布不等于400份完全独立有效信息
- covariance 是不确定性结构，不等于当前真实绝对误差
- 方差到标准差需要开平方
- GNSS status高、covariance小、topic稳定仍不能单独证明绝对位置完全正确

Weak / Corrected:
- 分辨率题曾引入置信度作为必要条件；已纠正
- 高频题曾主要从 subscriber 频率解释；已补相邻样本相关性
- covariance 曾表述为“置信度阈值”；已纠正为不确定性尺度/结构
- σ²=0.0004 m² 初算0.002 m；纠正为0.02 m

Retest:
- 1mm分辨率 + ±10cm准确度：不能称毫米级定位精度，PASS
- 400Hz IMU：静止时大量数据高度相关，不代表400份独立信息，PASS
- GNSS稳定偏1m：accuracy低、precision高、bias，covariance小不能排除系统误差，PASS
- σ²=0.09 m² → σ=0.3 m，PASS

Lesson:
- docs/lessons/day016.md
```

---

## 7. Day17 Learning Record

```text
Current Module / Day:
M03 / Day17 — IMU / Encoder / LiDAR / Camera — COMPLETED / PASS

Mastered:
- gyro 直接测 angular velocity，不直接测 orientation
- accelerometer 不是简单 world-frame acceleration；gravity / body frame / orientation 会参与解释
- encoder tick → motor angle → gear ratio → wheel angle → displacement → kinematics → odom pose
- wheel slip 可在 encoder 正常时使 wheel odom 错误
- LiDAR range + beam direction → LiDAR-frame XYZ；XYZ 不自动是 base_link
- ToF 的往返传播与 r=cΔt/2
- intensity 是单次回波信号强度，不是点云密度
- raw optical measurement / raw cloud / processed cloud 的层级
- deskew = per-point time + motion estimate → 补偿到同一参考时刻
- RGB Camera 直接提供 pixel / image measurement，不直接提供 semantic / 3D
- YOLO / monocular depth / FAST-LIO pose 属于算法推导结果
- Stereo disparity 是同一3D点在左右图的 pixel position difference
- Structured Light = known projector pattern + Camera + triangulation；与 ToF 区分

Weak / Corrected:
- 曾将 motor / wheel 差异归因于“半径不同”；纠正为明确 gear ratio
- 差速 yaw 示例将 0.2/0.5 算成4 rad；纠正为0.4 rad
- 曾把 LiDAR XYZ 默认理解成 base_link；纠正为先在 LiDAR frame
- 曾把 intensity 理解成点聚集程度；纠正为回波强度
- 曾把 deskew 理解成按时间分类；纠正为运动补偿到同一参考时刻
- 曾把 YOLO feature 与 descriptor / eigenvalue 混淆；纠正为 learned visual feature
- 曾把 disparity 理解成左右图大小差；纠正为位置差
- 曾把 Structured Light 与 ToF 混淆；纠正为 triangulation vs time/phase ranging
- 曾把 LiDAR XYZ 与 Stereo 原理混淆；定向复测通过

Deferred:
- Camera intrinsic / pinhole / pixel+depth→Camera XYZ /完整Stereo推导属于 M05 Day22–25，本日不作为未掌握项

Retest:
- LiDAR 已有 r,θ,φ：不需要第二个 Camera；range + beam geometry 可直接得到 LiDAR-frame XYZ，PASS
- Stereo：已知 baseline + disparity，通过三角测量求 depth，PASS

Lesson:
- docs/lessons/day017.md

Next:
- M03 / Day18 — GNSS / RTK / Timestamp / Latency / Synchronization / Calibration
```

---

## 8. Day18 Learning Record

```text
Current Module / Day:
M03 / Day18 — GNSS / RTK / Timestamp / Latency / Synchronization / Calibration — COMPLETED / PASS

Mastered:
- GNSS 伪距包含 receiver clock error 与环境误差，不是纯几何距离
- RTK Single / Float / Fixed 的工程意义；Fixed 不等于真实绝对误差被硬保证
- 单天线 RTK position ≠ 静止 absolute heading；双天线 baseline 可提供静止方向
- measurement time / arrival time / processing time / publish time 的区别
- latency（延迟）与 jitter（延迟抖动）的区别
- frequency（频率）与 latency（延迟）相互独立
- same frequency ≠ synchronization
- hardware synchronization vs software synchronization
- intrinsic / extrinsic 概念边界
- LIO latency → old pose → error calculation wrong → command wrong → overshoot / oscillation

Weak / Corrected:
- 曾把 covariance 小简单表述成“不抖”；纠正为估计不确定性尺度/结构，不等于真实误差
- 曾认为 10Hz 与 400ms latency 矛盾；纠正为 frequency 描述帧间隔，latency 描述数据落后真实物理时刻多少
- LIO latency 到 MPPI 异常的错误链初次不完整；已补齐

Retest:
- 0.05s 发布周期 → 20Hz；同时每帧可落后 0.3s，二者无矛盾，核心概念 PASS
- old yaw → 路径/航向误差计算错误 → vx/wz 不合适 → 纠偏滞后/过冲，PASS

Lesson:
- docs/lessons/day018.md

Next:
- M03 / Day19 — Actuator / Motor Loop / Communication / Command→Motion
```

---

## 9. Day19 Learning Record

```text
Current Module / Day:
M03 / Day19 — Actuator / Motor Loop / Communication / Command→Motion — COMPLETED / PASS

Mastered:
- Motor / Actuator / Transmission / Mechanism 边界
- BLDC（无刷直流电机）与 Servo（三环伺服控制）边界
- Position / Velocity / Current-Torque 三环
- current → torque → acceleration → velocity → position 物理链
- commanded state ≠ actual state
- saturation / velocity limit / acceleration limit
- gearbox：speed ÷ ratio、torque × ratio
- backlash
- UART / CAN / Ethernet 基本工程差异
- CAN Frame 与上层 Protocol 的职责边界
- LIO latency → old pose；communication latency → old command

Weak / Corrected:
- 曾把 BLDC 与三环控制混同；已纠正
- Gearbox 转矩计算首次出错；定向复测通过
- 曾把 communication latency 写成三环依次延迟；已纠正为消息传输链旧 command

Retest:
- G=50, ωm=500rad/s, τm=0.2N·m → 10rad/s, 10N·m，PASS
- MPPI command 0.5m/s 但 actual 0.3m/s：能构造多种合理执行链原因，PASS
- 定位延迟与通信延迟责任边界，PASS

Lesson:
- docs/lessons/day019.md

Next:
- M03 Module Graduation Exam
```

---

## 10. Day22 Learning Record

```text
Current Module / Day:
M05 / Day22 — Pinhole Camera Model — COMPLETED / PASS

Mastered:
- x=fX/Z, y=fY/Z 与相似三角形
- perspective：projection scale ∝ 1/Z
- normalized coordinate：X/Z, Y/Z
- one pixel → one camera ray，单像素不能唯一确定3D点
- principal point 概念
- focal length ↑ → FOV ↓；远处目标占更多像素
- Camera 3D → Normalized → Image Plane → Pixel 四层坐标
- YOLO pixel ≠ 3D position

Weak / Corrected:
- 曾把 image-plane x 当成 pixel u；已纠正
- 曾把 Camera-frame XYZ 说成相对 base_link；已纠正
- xn / x / u 层级混淆；定向复测通过
- “焦距大→看得更近”措辞已纠正

Retest:
- X/Z = Normalized Coordinate，PASS
- fX/Z = Image Plane Coordinate，PASS

Lesson:
- docs/lessons/day022.md

Next:
- M05 / Day23 — Intrinsic / Extrinsic / Full 3D→Pixel Projection
```

---

## 11. Day23 Learning Record

```text
Current Module / Day:
M05 / Day23 — Intrinsic / Extrinsic / Full 3D→Pixel Projection — COMPLETED / PASS

Mastered:
- intrinsic / extrinsic 职责边界
- Rigid Transform = Rotation + Translation
- homogeneous coordinate 与4×4 transform
- intrinsic matrix K：fx/fy/cx/cy
- World → Camera → Normalized → Pixel
- full projection 与矩阵维度
- transform direction 与 inverse transform
- K 只处理 Camera Frame 中的点

Weak / Corrected:
- 反方向 transform 首次未写 inverse；已纠正
- 曾写错 Pb / Pc 左右关系；定向复测通过
- 曾把 World 不能直接乘 K 的原因理解成主点偏移；已纠正为坐标系错误

Retest:
- Pb = T(c←b)^-1 Pc = T(b←c)Pc，PASS
- World 不能直接乘 K，因为坐标系不对，PASS

Lesson:
- docs/lessons/day023.md

Next:
- M05 / Day24 — Lens Distortion / Camera Calibration
```

---

## 12. Day24 Learning Record

```text
Current Module / Day:
M05 / Day24 — Lens Distortion / Camera Calibration — COMPLETED / PASS

Mastered:
- ideal pinhole vs real camera
- radial / tangential distortion
- k1/k2/k3 与 p1/p2
- intrinsic calibration 参数
- calibration board 几何约束
- reprojection error
- calibration success ≠ calibration quality
- multi-pose calibration coverage
- undistortion
- intrinsic / extrinsic calibration boundary
- angular extrinsic error grows with distance in position space

Weak / Corrected:
- radial distortion 初次解释成“不平行光”；已纠正
- reprojection error 初次未明确是 pixel-vs-pixel；已纠正
- yaw 外参误差初次按图像占比解释；已纠正为 Δy≈LΔθ

Retest:
- Reprojection Error = predicted pixel vs observed pixel，PASS
- 8m × 0.035rad = 0.28m，PASS

Lesson:
- docs/lessons/day024.md

Next:
- M05 / Day25 — Stereo / RGB-D / Depth Geometry
```

---

## 13. Day25 Learning Record

```text
Current Module / Day:
M05 / Day25 — Stereo / RGB-D / Depth Geometry — COMPLETED / PASS

Mastered:
- monocular ambiguity
- camera ray
- baseline / disparity
- Z=fB/d
- far-range disparity sensitivity
- baseline trade-off
- pixel + depth → Camera 3D
- depth vs range
- RGB-D ranging principles
- missing / invalid depth

Weak / Corrected:
- 远距离误差初次按“像素偏得更多”解释；已纠正
- 曾误解 B 增大对 Z 的关系；已纠正
- 远处 disparity 更小的几何直觉已补齐

Retest:
- d=3px 相比 d=30px 对同样1px误差更敏感，PASS
- 解释 Z↑ → d↓，PASS

Lesson:
- docs/lessons/day025.md

Next:
- M05 / Day26 — Pixel→Camera→Base→World
```

---

## 14. Day26 Learning Record

```text
Current Module / Day:
M05 / Day26 — Pixel→Camera→Base→World — COMPLETED / PASS

Mastered:
- undistortion
- pixel + depth → Camera 3D
- Camera → Base
- Base → World
- transform chain 与右侧先执行
- timestamp alignment
- time offset → spatial error
- pixel/depth/intrinsic/extrinsic/pose/time error chain
- PnP 基本作用
- hand-eye calibration 基本作用
- Navigation / Manipulation / VLM / VLA geometry interface

Weak / Corrected:
- 曾把3D Position写成完整Pose；已纠正
- 最终四层链复述曾过于概括；已补齐

Retest:
- Pixel + Depth → Camera → base_link → map/world 全链独立复述，PASS

Lesson:
- docs/lessons/day026.md

Next:
- M06 / Day27 — Tensor / Dataset / DataLoader
```

---

## 15. Day27 Learning Record

```text
Current Module / Day:
M06 / Day27 — Neural Network / Tensor / Dataset / DataLoader / Loss — COMPLETED / PASS

Mastered:
- x / θ / ŷ / y 的职责
- parameter vs hyperparameter
- Tensor 的 shape / dtype / device
- N×C×H×W
- y=Wx+b 的维度推理
- activation 的必要性
- loss 与 dataset objective
- Dataset / DataLoader / Sample / Batch
- forward pass
- training vs inference
- Robot Policy 的 input / prediction / target / parameter

Weak / Corrected:
- 首次混淆 x / ŷ / y
- 首次误把 Channel 说成 feature vector
- Dataset / DataLoader 职责初次不够准确
- Robot Policy 首次答成 Detection 任务

Retest:
- 四项定向复测全部 PASS

Lesson:
- docs/lessons/day027.md

Next:
- M06 / Day28 — Backpropagation / Computational Graph / Autograd
```

---

## 16. Day28 Learning Record

```text
Current Module / Day:
M06 / Day28 — Backpropagation / Computational Graph / Autograd — COMPLETED / PASS

Mastered:
- computational graph
- local derivative
- chain rule through network
- backward pass
- gradient as local sensitivity
- parameter tensor / gradient tensor shape
- gradient accumulation
- reverse-mode automatic differentiation
- loss.backward() vs parameter update
- requires_grad / .grad / no_grad / detach concept
- robot policy early-layer gradient propagation

Weak / Corrected:
- 曾把 gradient 说成 parameter 对 loss 的“贡献”；已纠正为 loss 对 parameter 的局部敏感度
- 曾把 reverse-mode 说成每个参数单独一条链；已纠正为复用同一计算图及中间结果
- 早期视觉层 gradient 解释曾缺完整路径；已补齐

Retest:
- gradient sensitivity：PASS
- reverse-mode reasoning：PASS
- multi-path gradient accumulation：PASS

Lesson:
- docs/lessons/day028.md

Next:
- M06 / Day29 — Training Loop / Gradient Descent / SGD / Momentum / Adam
```

---

## 17. Day29 Learning Record

Current Module / Day:
M06 / Day29 — Training Loop / Gradient Descent / SGD / Momentum / Adam — COMPLETED / PASS

Mastered:
- Loss / Objective 才是训练目标
- Gradient 是局部敏感度和方向信息，不是训练目标
- Gradient Descent / Learning Rate
- 新 Parameter 必须重新 Forward 才能判断新 Loss
- Full Batch / Mini-batch / Gradient Noise
- Batch / Iteration / Epoch
- zero_grad → forward → loss → backward → optimizer.step
- backward 计算 Gradient；optimizer.step 更新 Parameter
- Momentum / Adam 基本作用
- Gradient Explosion / Vanishing
- Gradient Clipping 限制幅值，不是异常点剔除

Weak / Corrected:
- 曾混淆“降低 Loss”和“降低 Gradient”
- 曾把 zero_grad 顺序写错
- 曾把 optimizer 说成优化 Learning Rate
- 曾把 Gradient Clipping 类比成 RTK 异常点删除

Retest:
- Loss vs Gradient：PASS
- Training Loop 顺序：PASS
- backward vs optimizer.step：PASS
- Gradient Clipping：PASS

Lesson:
- docs/lessons/day029.md

---

## 18. Day30 Learning Record

Current Module / Day:
M06 / Day30 — Classification / Logit / Softmax / Cross Entropy — COMPLETED / PASS

Mastered:
- Regression vs Classification
- Logit 不是 Probability
- Softmax 归一化与类别概率分布
- Cross Entropy：L = -log p_y
- One-hot Target
- Sigmoid + BCE 二分类
- Class Imbalance 基本问题
- Threshold 与训练 Loss 的边界
- Softmax Confidence != Robot Safety Confidence

Weak / Corrected:
- 曾把多分类 One-hot 写成“car / not car”，与二分类混淆

Retest:
- person / car / dog / bicycle，真实类别 dog → [0,0,1,0]：PASS

Lesson:
- docs/lessons/day030.md

---

## 19. Day31 Learning Record

Current Module / Day:
M06 / Day31 — CNN Foundations — COMPLETED / PASS

Mastered:
- N / C / H / W
- Locality / Parameter Sharing
- Convolution / Kernel / Filter
- Weight Tensor Shape
- Feature Map / Feature Channel
- Stride / Padding
- Output Shape Calculation
- Receptive Field
- Pooling / Downsampling
- Hierarchical Features
- Translation Equivariance
- Backbone / Task Head

Weak / Corrected:
- CNN 优势初次未明确 Locality
- Output Channel 数量初次写错
- 输出尺寸初次忘记 Floor

Retest:
- Locality + Parameter Sharing：PASS
- Weight Tensor Shape：PASS
- Output Shape Calculation：PASS

Lesson:
- docs/lessons/day031.md

---

## 20. Day32 Learning Record

Current Module / Day:
M06 / Day32 — Generalization / Normalization / Regularization / Distribution Shift — COMPLETED / PASS

Mastered:
- Train / Validation / Test 职责
- Generalization
- Overfitting / Underfitting
- Overfitting vs Distribution Shift
- Data Leakage
- Input Normalization
- BatchNorm / LayerNorm
- Weight Decay / Dropout / Data Augmentation
- Robot augmentation 的物理一致性
- Model Metric != Robot Task Success

Weak / Corrected:
- 曾把 Distribution Shift 误答成 Overfitting
- Validation / Test 边界初次不严谨
- Input Normalization 曾理解成“数据更小”
- BatchNorm 曾误说成参数归一化，并曾答反 Batch 依赖
- Dropout 曾误认为推理阶段继续开启
- 镜像增强后 Target 变化未写清

Retest:
- Input Normalization：PASS
- BN / LN：PASS
- Dropout train/eval：PASS
- Robot Policy 镜像 Target：PASS

Lesson:
- docs/lessons/day032.md

Pending:
- M06 Day30
- M06 Day31

Next（按当前学习顺序）:
- M06 Day33 — Attention / Transformer Foundations

---

## 21. Day33 Learning Record

Current Module / Day:
M06 / Day33 — Attention / Transformer Foundations — COMPLETED / PASS

Mastered:
- Token / Embedding
- Q / K / V
- Scaled Dot-Product Attention
- Attention Matrix Dimension
- Attention Weight 与 Weighted V
- Self-Attention / Cross-Attention
- Multi-Head Attention 基本概念
- Positional Information
- Transformer Block
- Attention vs FFN
- Causal Mask
- Robotics / VLA interface

Weak / Corrected:
- Robot State Token 与 Action Token 曾混淆
- K 的匹配职责与 V 的内容职责初次混淆
- Attention Weight 曾泛称为概率
- Self/Cross Attention 初次按“怎么乘”定义
- Attention 与 FFN 的职责初次表述不准

Retest:
- Q/K/V：PASS
- Self/Cross Attention：PASS
- Attention Matrix (i,j)：PASS
- Attention vs FFN：PASS

Lesson:
- docs/lessons/day033.md

M06 Graduation Exam:
- DEFERRED / 留空
- 当前不宣告 M06 模块毕业

---

## 22. Day34 Learning Record

Current Module / Day:
M07 / Day34 — Classification / Detection / YOLO / IoU / NMS — COMPLETED / PASS

Mastered:
- Classification vs Detection
- Bounding Box
- Class / Score / Threshold 边界
- IoU
- TP / FP / FN
- Confidence Threshold 对 FP/FN 的影响
- NMS
- YOLO Backbone / Neck / Head
- Multi-scale Detection
- Small Object 问题
- Anchor / Anchor-Free 基本概念
- 2D Detection Geometry Boundary
- BBox → Depth → Camera 3D → TF → Costmap

Weak / Corrected:
- 曾把 Threshold 误认为 Detection 输出
- IoU 并集计算初次错误
- FP / FN 初次答反
- BBox→Costmap 链路初次漏掉 Depth

Retest:
- Detection Output / Threshold：PASS
- IoU：PASS
- FP / FN：PASS
- 2D BBox → 3D 需要 Depth：PASS

Lesson:
- docs/lessons/day034.md

---

## 23. Day35 Learning Record

Current Module / Day:
M07 / Day35 — Semantic / Instance Segmentation / Traversability — COMPLETED / PASS

Mastered:
- Semantic vs Instance Segmentation
- Mask / Pixel Logit / Pixel Class
- Segmentation Tensor Shape
- Class Imbalance
- Boundary Error
- Traversability
- Semantic != Geometry
- Semantic != Traversability
- Blind Path Semantic != Safe Free Space
- Unknown != Free
- Mask + Depth → Camera 3D → base_link → map/world/BEV
- Distribution Shift
- Temporal Consistency

Quiz:
- 8/8 PASS
- 无额外定向复测项

Lesson:
- docs/lessons/day035.md

---

## 24. Day36 Learning Record

Current Module / Day:
M07 / Day36 — Monocular Depth / Stereo / RGB-D / Learned Depth — COMPLETED / PASS

Reuse Review:
- Stereo / RGB-D
- Pixel + Depth → Camera 3D
- Timestamp Alignment
- Invalid / Noisy Depth

Mastered New Knowledge:
- Metric vs Relative Depth
- Monocular Scale Ambiguity
- Learned Monocular Depth
- Visual Prior
- Scale Calibration
- Learned-depth Distribution Shift
- Depth Edge
- BBox Center Depth Risk
- Mask / Multi-point Sampling
- Depth Source → Metric 3D Interface

Weak / Corrected:
- Scale Ambiguity 初次表述不完整
- Learned Visual Prior 与 Scale Calibration 初次混淆
- Depth Source 链路第一空初次答错

Retest:
- Scale Ambiguity：PASS
- Learned Visual Prior：PASS
- Depth → Camera 3D → base_link → map/BEV：PASS

Lesson:
- docs/lessons/day036.md

---

## 25. Day37 Learning Record

Current Module / Day:
M07 / Day37 — PointCloud / Filtering / KD-tree / Clustering / 3D Detection — COMPLETED / PASS

Mastered:
- Point Fields / Frame / Timestamp
- Organized vs Unorganized PointCloud
- Crop / Range / Outlier Filter
- Voxel Downsampling
- Voxel Size trade-off
- Nearest Neighbor / KNN
- KD-tree spatial indexing
- Local Normal
- Euclidean Clustering
- DBSCAN concept
- Over-segmentation / Under-segmentation
- Semantic PointCloud
- 3D Box position / size / yaw
- 3D Result = Geometry + Frame + Timestamp

Weak / Corrected:
- Organized PointCloud 曾误解为按颜色或物体组织
- Voxel Size 增大后的 Point Count 初次答反
- Voxel / Clustering / Class 职责曾混淆
- KD-tree 初次只描述空间切分，未说清核心职责是近邻搜索
- 3D Box 与 3D Result 必需字段初次不完整

Retest:
- Organized PointCloud：PASS
- Voxel Size：Point Count↓ / Computation↓ / Geometry Detail↓，PASS
- Voxel = 体素网格降采样，PASS
- KD-tree = 加速近邻搜索，PASS
- Clustering = 空间几何分组，PASS
- Tolerance 太小→Over-segmentation；太大→Under-segmentation，PASS
- Frame + Timestamp：PASS

Lesson:
- docs/lessons/day037.md

---

## 26. Day38 Learning Record

Current Module / Day:
M07 / Day38 — Voxel Representation / Occupancy / BEV / Costmap Boundary — COMPLETED / PASS

Mastered:
- Voxel Downsampling vs Voxel Representation
- Occupied / Free / Unknown
- Unknown != Free
- No Point != Free
- Ray Casting
- 2D Occupancy Grid
- BEV / Semantic BEV
- LiDAR / Camera → BEV
- Occupancy Prediction
- YOLO vs BEV
- BEV vs Costmap
- Cost Mapping Rule
- Resolution / Range / Memory trade-off
- Temporal Fusion
- Ego-motion Compensation
- Coordinate Alignment

Weak / Corrected:
- Voxel Downsampling 初次混入 KD-tree 近邻搜索职责
- BEV 初次描述越界到 Traversability / Costmap
- Temporal Fusion 初次只说确认 t1/t2 位姿，已补自运动补偿
- Semantic BEV → Costmap 的“每个物体代价”答案本质正确，统一术语为 Cost Mapping Rule

Retest:
- Voxel Downsampling vs Representation：PASS
- Semantic BEV → Cost Mapping → Costmap：PASS
- YOLO vs BEV：PASS
- Ego-motion Compensation：PASS

Lesson:
- docs/lessons/day038.md

---

## 27. Day39 Learning Record

Current Module / Day:
M07 / Day39 — Tracking / Metrics / Perception→Robot Integration — COMPLETED / PASS

Reuse Review:
- TP / FP / FN
- IoU
- Confidence Threshold 与 FP/FN

Mastered New Knowledge:
- Precision / Recall
- Confidence Threshold vs IoU Threshold
- AP / mAP
- Class IoU / mIoU
- Depth Metric basics
- Tracking
- Data Association
- Track ID / Position / Velocity / Age / Confidence / Freshness
- Persistence / Timeout
- Stale Perception
- Model Score vs Robot Decision Threshold
- Component Metric vs End-to-End Metric
- Detection→Depth→Calibration/TF→Tracking→Freshness→World/BEV→Costmap→Planner

Weak / Corrected:
- Precision / Recall 初次计算错误
- AP 与 mAP 初次混淆
- Tracking 核心缺口初次答为 Motion Prediction，已纠正为 Data Association
- Segmentation 指标初次未答 Class IoU / mIoU
- 系统链路题初次混入 Detection 本身的泛化问题

Retest:
- Precision / Recall：PASS
- AP vs mAP：PASS
- Class IoU / mIoU：PASS
- Data Association：PASS
- Detection 后系统故障链：PASS

Lesson:
- docs/lessons/day039.md

M07 Graduation Exam:
- DEFERRED / 留空
- 当前不宣告 M07 模块毕业

---

## 28. Day40 Learning Record

Current Module / Day:
M08 / Day40 — Probability / Conditional Probability / Bayes — COMPLETED / PASS

Mastered:
- Random Variable vs Observation
- Probability Distribution
- Discrete vs Continuous
- Joint Probability
- Marginal Probability
- Conditional Probability
- Independence
- Bayes Theorem
- Prior / Likelihood / Posterior / Evidence
- Likelihood != Posterior
- Sequential Bayes Update
- Robot Belief / Sensor Fusion interpretation

Weak / Corrected:
- X 初次答为“预测值”，已纠正为描述未知真实状态的随机变量
- 条件概率方向初次说成“绝不可能相等”，已纠正为一般不相等、不可直接交换
- Posterior 在下一轮角色初次回答不完整，已补 Posterior_t → Next Prior

Retest:
- Random Variable vs Observation：PASS
- Posterior_t → Next Prior：PASS

Lesson:
- docs/lessons/day040.md

---

## 29. Day41 Learning Record

Current Module / Day:
M08 / Day41 — Expectation / Variance / Covariance / Gaussian — COMPLETED / PASS

Reuse Review:
- Variance
- Standard Deviation
- Covariance basics
- Covariance != Actual Error

Mastered New Knowledge:
- Expectation
- Covariance Matrix
- Diagonal Variance
- Cross-covariance
- Gaussian
- Multivariate Gaussian
- Covariance Ellipse
- Eigenvector / Eigenvalue uncertainty geometry
- Correlation vs Covariance
- Systematic Bias vs Covariance
- State Vector + Covariance Matrix

Weak / Corrected:
- Cov(x,theta) 单位初次写错，已纠正为 m·rad
- Sigma 初次只解释“集中程度”，已补状态间相关结构
- Covariance Ellipse 中 Eigenvector / Eigenvalue 初次未一一对应
- GNSS 小 covariance 场景初次未点名 Systematic Bias

Retest:
- Cov(x,theta) 单位：PASS
- mu / Sigma 物理意义：PASS
- Eigenvector / Eigenvalue / sqrt(lambda)：PASS
- Systematic Bias：PASS

Lesson:
- docs/lessons/day041.md

---

## 30. Day42 Learning Record

Current Module / Day:
M08 / Day42 — Likelihood / MLE / MAP — COMPLETED / PASS

Reuse Review:
- Bayes
- Gaussian

Mastered New Knowledge:
- Probability vs Likelihood
- L(theta)=p(D|theta)
- Conditional Independent Measurements
- Product Likelihood
- MLE
- Log Likelihood
- Negative Log Likelihood
- MAP
- MLE vs MAP
- Prior semantics
- Gaussian Noise → Squared Residual
- Gaussian + MLE → Least Squares
- Measurement Uncertainty → Weight

Weak / Corrected:
- Probability vs Likelihood 初次边界表述不准，已纠正
- log 与 argmax 的原因初次表述成 theta 单调增大，已纠正
- Prior 初次说成“以前事件发生的概率”，已纠正为当前观测前对当前参数 / 状态的判断
- Gaussian→LS 初次数学链不完整，已补 NLL → Squared Residual
- residual weighting 初次判断反，已纠正为 weight 与 1/sigma^2 成正比

Retest:
- Probability vs Likelihood：PASS
- log 单调性：PASS
- Prior：PASS
- uncertainty weighting：PASS

Lesson:
- docs/lessons/day042.md

---

## 31. Route Change Record — Navigation First

Decision:
- 用户明确要求先学习 M11–M13｜规划、运动学 / 动力学、控制。
- Navigation 是当前主攻强项，要从“工程很强”继续提升到 Graduate-level Theory + L5 Owner。
- Localization / State Estimation / SLAM 是成为机器人全栈的最大能力门槛，放在 M11–M13 后集中突破。

Execution Order:
```text
M11 Day63–70
→ M12 Day71–79
→ M13 Day80–89
→ M08 Day43–48
→ M09 Day49–54
→ M10 Day55–62
```

M11 Strengthening:
- Day64：Anytime Search / ARA* 思想
- Day66：Kinodynamic Planning boundary
- Day67：PRM / RRT-Connect concept only
- Day68：Incremental Search / LPA* / D* Lite
- Day70：Path→Trajectory / Trajectory Optimization bridge；Planning Under Uncertainty boundary

M12 Route Adaptation:
- Day71 前增加最小 SE(3) Entry Bridge，不新增 Day、不视为 M08 Day46–48 已完成。
- Day76 Mobile Robot Kinematics 作为 Navigation→Control 关键桥梁。

M13 Strengthening:
- Day80 增加 Trajectory Optimization 基本数学形式。
- Day82 Linear Observability 自包含教学，不依赖尚未学习的 M09。
- MPPI Day85–89 目标 L4→L5。

M08 Day43 Status:
- 已完成讲授，尚未完成 Quiz / Retest。
- PAUSED / NOT COMPLETED。
- 不生成 docs/lessons/day043.md，待回补并 PASS 后再记录。

---

## 32. Learning Strategy Update — Systematic Foundations + Day Goal First

Decision:
- 用户已有较强 Navigation / MPPI 工程经验，但没有系统完成 Navigation / Kinematics / Control 理论课程。
- 工程熟练度不能被当作基础理论已经 PASS。
- M11–M13 从现在开始严格执行：
  `Foundation → Core Theory → Advanced / Owner`。

Foundation Rule:
- 工作中用过 / 调过 / 读过源码，但未在本课程系统学过：必须正式覆盖一次；
- 已会内容允许快速推进，但 **快速通过 ≠ 跳过**；
- 只有已经在本课程正式学习并 PASS 的知识才按 Knowledge Reuse Rule 简短 Review。

Day Capacity Rule:
- **Day Goal 优先于知识点数量**；
- 不再使用“核心知识点 ≤20”硬上限；
- 20 个不够就扩展到 30、40 个或更多，只要都服务于同一个 Day Goal；
- 不为了控制数量删除 Foundation / Hard Gate / 推导 / Owner 连接；
- **一个 Day 不拆成两个 Day，不新增 A/B Day，不把半个 Day 拖到下一 Day**；
- 复杂 Day 可以增加当天讲义长度与学习时长。

Day63 Initial Status（历史记录，已由第34节更新）:
- 第一版讲授为预热，基础覆盖不完整，曾暂记 IN PROGRESS；
- 后续已完整补齐基础并完成 Quiz / Retest；
- 最终状态以第34节的 COMPLETED / PASS 为准。

---

## 33. Day63 Knowledge Coverage Record（已完成；详细情况见第34节）

M11 Day63 — Graph / BFS / Dijkstra

Foundation：
Graph / Vertex / Edge
→ Directed / Undirected
→ Weighted / Unweighted
→ Neighbor / Degree
→ Walk / Path / Cycle
→ Reachability / Connectivity
→ Adjacency List / Matrix
→ Queue / FIFO
→ BFS
→ Visited / Parent
→ BFS shortest-path condition

Core：
Weighted Shortest Path
→ Dijkstra
→ g(n)
→ Relaxation
→ Priority Queue / Min-Heap
→ stale queue entry
→ nonnegative edge condition
→ shortest-path optimality
→ path reconstruction
→ complexity

Advanced / Owner：
Grid Graph
→ Road Graph
→ Costmap → Edge Cost
→ HPA Abstract Graph
→ Dijkstra → A* → HPA → Global Planner

保留事项：
- M02 Module Graduation Exam：INCOMPLETE / DEFERRED
- M03 Module Graduation Exam：DEFERRED
- M06 Module Graduation Exam：DEFERRED
- M07 Module Graduation Exam：DEFERRED
- M08 Day43–48：ROUTE-PAUSED，M11–M13 后继续
- M04 Day20–21：按专项路线暂缓

---

## 34. Day63 Learning Record — COMPLETED / PASS

Current Module / Day:
- M11 / Day63 — Graph / BFS / Dijkstra：**COMPLETED / PASS**。
- Lesson：`docs/lessons/day063.md`。

Mastered:
- Graph、Vertex/Edge、Directed/Undirected、Weighted/Unweighted、Neighbor/Degree、Walk/Path/Cycle、Reachability/Connectivity；
- Adjacency List vs Matrix、Grid 4/8 邻接、Queue/FIFO、BFS、Visited、Parent、BFS 最短路成立条件；
- Path Cost、Dijkstra、`g(n)`、Relaxation、Priority Queue/Min-Heap、Stale Entry、nonnegative edge condition、shortest-path optimality、Path Reconstruction；
- Costmap → Edge Cost → Total Cost → Path Choice；Road Graph / Grid Graph / HPA Abstract Graph 的统一表示；Dijkstra → A* 前置。

Weak / Corrected:
- Relaxation 初次计算误把当前 `g(v)` 与新候选直接相加；已纠正为 `g(v)=min(g(v),g(u)+c(u,v))`；
- Negative Edge 与 Negative Cycle 曾混淆；已纠正为单条负权边（即使无环）也能破坏 Dijkstra settle 的最优性保证；
- 曾认为 Dijkstra 首次选定方向后会一直向该方向扩展；已纠正为**每次弹出 Open 中累计 `g` 最小节点**。

Retest:
- Relaxation：`g(P)=8,g(Q)=17,c(P,Q)=5` → `g(Q)=13,parent(Q)=P`：PASS；
- 负权反例 `S→A:4,S→B:9,B→A:-10` → 后续可得到 `g(A)=-1`：PASS；
- 最后确认 `S-B:2,S-A:5,B-C:10,A-C:1` → 下一步 B、再 A、最终 S→A→C Cost=6：PASS。

Hard Gates:
- Foundation：PASS；BFS：PASS；Dijkstra / Relaxation：PASS；Queue Selection / Nonlocking Frontier：PASS；Nonnegative Edge / Optimality：PASS；Navigation Mapping：PASS。

Next:
- **M11 / Day64 — A* / Heuristic / Optimality / Weighted A* / Anytime Search (ARA*)**，当日学习与考核尚未完成，不预记 PASS。

Preserved:
- M08 Day43–48：ROUTE-PAUSED；M02/M03/M06/M07 Graduation Exams 仍 DEFERRED。
