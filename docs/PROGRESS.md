# PROGRESS — 当前课程设计与学习状态

## 1. 当前阶段

当前处于：**定位 + 视觉理论专项学习（ACTIVE）**。
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
Next：M03 Day17 — IMU / Encoder / LiDAR / Camera
```

> 重要：M02 Day8–15 的每日课程已完成，但 **M02 模块毕业考试尚未完成，因此不宣告 M02 模块毕业**。用户选择先继续 M03，后续回补考试剩余题目与 Hard Gate 复测。

专项优先顺序保持：

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

M04 Simulation 暂不作为当前专项前置主线。

---

## 2. 学习方式与表达规则

- 正常学习日按 2–3h 设计；理论、数学、公式、算法与源码理解优先。
- 不要求每天代码或 LAB；必要 LAB 独立安排。
- 中文术语为主，英文首次出现时括号注释，例如：雅可比矩阵（Jacobian）、海森矩阵（Hessian）。
- 题目必须独立列全已知条件、变量维度、单位和所求量。
- 物理意义优先用真实机器人、传感器、几何对象解释；若连续两次未懂必须更换讲解方式。
- 已稳定掌握内容不机械重考；错误项做 targeted remediation / retest。

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

Next:
- M03 / Day17 — IMU / Encoder / LiDAR / Camera
```

---

## 7. 下一步

```text
M03 Day17
→ IMU：gyro / accelerometer 到底直接测什么
→ Encoder / Wheel Odom：直接测量 vs 推导状态
→ LiDAR：range / angle / intensity 与 XYZ point 的区别
→ Camera：pixel intensity 与3D/semantic estimate的区别
→ Daily Quiz + targeted retest

保留事项：
- M02 Module Graduation Exam 尚未完成，后续回补第8题及剩余纠错，不宣告 M02 模块毕业。
```
