# PROGRESS — 当前课程设计与学习状态

## 1. 当前阶段

当前处于：**定位 + 视觉理论专项学习（ACTIVE）**。
专项导航文件：`docs/TEMP_POSITIONING_VISION_PLAN.md`。

```text
定位 + 视觉理论专项 / Phase 1 — Mathematical Foundations I
M02 Day8  — Vector / Matrix / Dimension：COMPLETED / PASS
M02 Day9  — Basis / Coordinate / Linear Transformation：COMPLETED / PASS
M02 Day10 — Dot / Cross / Norm / Projection / Geometry：COMPLETED / PASS
M02 Day11 — Eigenvalue / Eigenvector / Quadratic Form：COMPLETED / PASS
M02 Day12 — SVD / Rank / Conditioning：COMPLETED / PASS
M02 Day13 — Derivative / Differential / Numerical Integration：COMPLETED / PASS
Next：M02 Day14 — Partial Derivative / Gradient / Chain Rule
```

本专项优先顺序：

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

**当前不是按 Day1–Day135 机械顺序推进。** Curriculum v1.0 主结构不变；专项只调整优先级。M02 完成后进入 M03，M03 后直接进入 M05；M04 Simulation 暂不作为本专项前置主线。

---

## 2. 最终目标与学习方式

> 研究生级机器人理论基础 + 真实机器人全栈工程能力 + VLA / Mobile Manipulation 具身智能能力 + 系统 Owner 能力。

正常 2–3h / Day；理论、数学、公式、算法、源码理解优先；已有真实机器人经验用于映射但不替代 Hard Gate。不要求每天代码或 LAB；必要 LAB 独立安排。专项不以交付日期压缩核心理论。

---

## 3. 已锁定课程架构与 Day 范围

```text
M00  Robot Full-stack Architecture                         Day1
M01  C++ / Linux / ROS2 Systems                           Day2–7
M02  Mathematical Foundations I                           Day8–15
M03  Sensors & Actuators                                  Day16–19
M04  Robot Simulation Foundations                         Day20–21
M05  Vision Geometry                                      Day22–26
M06  Deep Learning Foundations                            Day27–33
M07  Deep Vision / 3D Perception                          Day34–39
M08  Mathematical Foundations II                          Day40–48
M09  State Estimation                                     Day49–54
M10  SLAM / LIO / VIO / Factor Graph                      Day55–62
M11  Planning & Navigation                                Day63–70
M12  Robot Kinematics / Dynamics / System Dynamics         Day71–79
M13  Control & Optimal Control                             Day80–89
M14  Manipulation                                          Day90–96
M15  Robot Learning                                       Day97–104
M16  VLM                                                   Day105–109
M17  VLA                                                   Day110–115
M18  Mobile Manipulation                                   Day116–118
M19  Deployment / Data / Evaluation / Sim2Real            Day119–122
M20  Safety / Reliability / Owner                          Day123–126
M21  Research Methodology & Capstone                       Day127–129
M22  Foundation Cleanup                                    Day130–135 dynamic
```

固定主课程 Day1–129；M22 根据真实 Foundation Debt 动态生成。

---

## 4. 已锁定评估规则

Module Graduation Exam 默认：30% 核心基础、50% 综合系统场景、20% Source / Formula / Design；总分 ≥85%，Hard Gate 基础错误不能靠其他题抵消。Critical concept 失败则 targeted remediation + retest；已稳定掌握内容不机械重考。

---

## 5. 当前正式 LAB

LAB01 — Manipulation Pick-and-Place；LAB02 — Mobile Manipulation Capstone；LAB03 — Robot Policy / VLA Action Interface。其他最小实现只在明显帮助理论理解时安排。

---

## 6. Foundation Debt

### 当前状态

Day8–Day13 已完成，**无未关闭 P0/P1 Foundation Debt**。保留 P2 Review Debt，供 M02 Module Graduation Exam、M08/M09/M10 对应知识重现和 Foundation Cleanup 复测，不阻塞 Day14。以下保留历史错误、纠正和未关闭点，不因结项删除。

### Day8 暴露但已纠正

- Identity Matrix 与 Unit Vector 混淆；Transpose 与 Inverse 区别遗忘。
- `y=Ax` 曾过度理解为坐标/维度转换，已纠正为一般线性映射。
- 构造 measurement matrix H 时曾把 state variable 写进 H，已纠正为写线性系数。

### Day9 暴露但已纠正

- 曾把所有 Ax 理解成坐标变换，混淆基/坐标与绝对/相对坐标。
- rank 的独立方向意义、null space 具体消掉的输入方向曾不明确。
- 已区分列空间描述输出可张成范围、零空间描述被消掉的输入方向；一般 `N(A)` 与行空间正交。

### Day10 暴露但已纠正

- 曾把 `a^T b` 误读成普通矩阵变换；后续优先用 `a·b` 表示点积，必要时说明矩阵表示。
- 已区分标量投影与投影向量。
- 通过旋转基保持单位正交、转置用点积取回基方向分量，完成 `R^-1=R^T` 复测。

### Day11 暴露并已纠正

- `x=cv` 初始讲解未显式区分 scalar/vector/dimension；后续必须先说明 `c,λ∈R`、`v,x∈R^n`。
- 曾认为必须用 eigenvectors 才能拆任意向量；已纠正为普通基也能拆，eigen basis 的价值是各方向在 A 下不耦合。
- 曾把任意向量经过 A 说成整体缩放；已纠正为不同 eigen components 比例改变后，合成方向通常改变。
- 曾认为 eigenvectors 一般正交；已纠正为一般矩阵不保证，实对称矩阵可取正交特征基。
- 曾把对角矩阵的 0 归因于正交；已纠正为 eigen direction 不产生其他 eigen direction 分量。
- 曾把 general max eigenvalue 当作最大拉伸；已纠正为一般矩阵看 singular value。
- covariance eigenvalue 是主方向方差，椭圆轴尺度与 `sqrt(λ)` 成正比。
- 已区分 Gaussian elimination / row reduction 求 rank 与 eigen diagonalization 换基。

### Day12 暴露并已纠正

- 曾把 eigenvalue/singular value 区别说成“前者有耦合、后者没有”；已纠正为 eigenvalue 描述不变方向，SVD 描述一般矩阵的长度作用。
- 曾把小 σ 的噪声放大理解为串到其他方向；已纠正为 singular basis 中弱方向自己的 measurement noise 在反解同一 state 方向时被 `1/σ` 放大。
- 曾把 conditioning 与特征值正交性混淆；已纠正为方阵可逆性看是否有零奇异值，conditioning 看奇异值比值与反解敏感度。
- 平墙场景曾认为应增加垂直墙移动；已纠正为缺少沿墙约束，需要其他几何、sensor 或 prior。

### Day13 暴露并已纠正

- 曾混淆连续导数、有限差分和积分；已纠正为瞬时变化率、有限采样近似、变化率累计。
- 曾把一般 `dx` 理解为时间；已纠正为自变量变化，只有自变量是 t 时才表示时间变化。
- 曾把 `dθ` 说成角速度；已纠正为角度变化，`dθ/dt` 才是角速度。
- 曾把求导的小 dt 噪声放大错误套到积分；已纠正为积分误差必须乘 dt。
- 曾认为固定加速度 bias 使速度误差维持常数；已纠正为速度误差线性增长、位置误差平方增长。
- 曾认为每 Hz 直接加一次 bias；已纠正为 `Σb_vΔt_k=b_vT`。
- 固定速度 bias 的位置误差数值正确但单位误写 m/s；正确单位 m。

### P2 Review Debt — Day11

```text
Knowledge: Eigen diagonalization / basis change
Exposed At: M02 / Day11
Wrong / Weak Understanding: 容易从 Av=λv 直接跳到 diagonal matrix，推导链条还需闭卷复述
Debt Type: Derivation / Transfer
Priority: P2
Current Level: L2-L3
Target Level: L3
Retest: V=[v1...vn] → AV=VΛ → V^-1AV=Λ，并解释 off-diagonal 为 0 的原因
Status: OPEN
```

```text
Knowledge: Quadratic form applications
Exposed At: M02 / Day11
Wrong / Weak Understanding: covariance ellipse、LQR cost contour、Hessian curvature 的 λ 含义容易混；cost 等高线概念理解较晚
Debt Type: Transfer / System Reasoning
Priority: P2
Current Level: L2
Target Level: L3
Retest: 分别解释 covariance / LQR / Hessian 的 eigenvector 与 eigenvalue
Status: OPEN
```

```text
Knowledge: Eigenvalue vs Singular Value
Exposed At: M02 / Day11 → reinforced at Day12
Wrong / Weak Understanding: 曾把 general matrix 最大 eigenvalue 当作最大拉伸
Debt Type: Definition / Transfer
Priority: P2
Current Level: L3（Day12 已强化）
Target Level: L3
Retest: M02 Module Exam 闭卷解释为何 general max stretch 看 singular value，不看 max eigenvalue
Status: RETEST
```

```text
Knowledge: Positive definite quadratic cost
Exposed At: M02 / Day11
Wrong / Weak Understanding: 定义与 cost 直觉已理解，但独立复测不足
Debt Type: Definition / Transfer
Priority: P2
Current Level: L2
Target Level: L2-L3
Retest: 闭卷解释 x!=0 => x^TQx>0 与 quadratic cost 的关系
Status: OPEN
```

### P2 Review Debt — Day12

```text
Knowledge: Small singular value and inverse sensitivity
Exposed At: M02 / Day12
Wrong / Weak Understanding: 曾把弱方向 noise amplification 理解成误差串到其他方向
Debt Type: Transfer / System Reasoning
Priority: P2
Current Level: L2-L3
Target Level: L3
Retest: 在 LS / Jacobian / SLAM 场景中再次解释 Δx≈Δb/σ，并明确是同一弱奇异方向
Status: OPEN
```

```text
Knowledge: Invertibility vs numerical conditioning
Exposed At: M02 / Day12
Wrong / Weak Understanding: 初始把可逆性与 eigenvector orthogonality 混淆
Debt Type: Definition / Transfer
Priority: P2
Current Level: L2-L3
Target Level: L3
Retest: 给定 singular spectrum，独立判断 rank / invertibility / condition number / reliability
Status: OPEN
```

### P2 Review Debt — Day13

```text
Knowledge: Differential vs angular velocity
Exposed At: M02 / Day13
Wrong / Weak Understanding: 曾把 dθ 当作角速度
Debt Type: Definition / Dimension
Priority: P2
Current Level: L2-L3
Target Level: L3
Retest: 在 Day14/15 解释 dθ 与 dθ/dt，正确写 rad / rad/s
Status: OPEN
```

```text
Knowledge: Integration units and sampling frequency
Exposed At: M02 / Day13
Wrong / Weak Understanding: 固定速度 bias 的位置误差曾写成 m/s；曾把每 Hz 加一次 bias 与正确时间积分混淆
Debt Type: Calculation / Dimension / Transfer
Priority: P2
Current Level: L2-L3
Target Level: L3
Retest: 计算 Σb_vΔt=b_vT，检查 m/s×s=m；解释同一 T 下高频不自动使固定 bias 误差增加
Status: OPEN
```

未来 Foundation Debt 统一记录：
```text
Knowledge:
Exposed At (Module / Day / Exam / Source):
Wrong / Weak Understanding:
Debt Type: Definition / Calculation / Derivation / Transfer / Source-reading / System Reasoning
Priority: P0 / P1 / P2 / P3
Current Level:
Target Level:
Retest:
Status: OPEN / LEARNING / RETEST / CLOSED
```

---

## 7. 当前 Daily Learning Record

```text
Current Module / Day:
M02 / Day13 — Derivative / Differential / Numerical Integration — COMPLETED / PASS

Specialty Context:
- TEMP_POSITIONING_VISION_PLAN：ACTIVE
- 当前 Phase：Phase 1 — Mathematical Foundations I
- Phase 1 范围：M02 Day8–15
- Day14 之后继续 Day15；完成 M02 后进入 M03 Day16–19
- M03 完成后按专项进入 M05 Day22–26；M04 Day20–21 当前不作为专项前置主线

Mastered:
- 导数是连续函数的瞬时变化率；有限差分是用有限采样近似导数；积分是变化率累计
- p(t)∈R² 时 dp/dt∈R²，是二维速度向量
- 导数与切线斜率、局部敏感度的关系
- dy≈f'(x)dx 的局部近似意义；dx 是自变量变化，不一定是时间
- 位置、速度、加速度之间的导数与积分关系
- 有限差分速度计算及小 dt 可能放大测量噪声
- Forward Euler 用起点速度近似区间速度，属于数值近似
- 按可靠 timestamp 计算实际 dt，不盲用名义频率
- 高频不自动消除 bias/noise/model error
- 固定速度 bias 的位置误差按 b_v T 累计
- 固定加速度 bias 的速度误差线性增长、位置误差按时间平方增长
- Wheel Odom / IMU 积分误差会累计；模型 dt 与传感器实际 dt 需区分

Weak:
- dθ 与 dθ/dt 的定义和单位需后续闭卷复测
- 积分结果单位检查需强化，尤其速度误差与位置误差
- 固定 bias 与采样次数的关系需在 M03/M09 再次迁移复测

Wrong Understanding:
- 曾混淆导数、差分与积分
- 曾把 dx 固定理解为时间，把 dθ 说成角速度
- 曾把求导的小 dt 放大噪声套到积分
- 曾认为固定加速度 bias 使速度保持常数
- 曾把固定速度 bias 的位置误差写成 m/s

Corrected:
- 连续导数、有限差分近似、积分累计分别定义
- dθ 是角度变化；dθ/dt 才是角速度
- 正确积分误差必须乘实际 dt；Σb_vΔt=b_vT
- e_v=b_aT，e_p=0.5b_aT²（零初始误差、固定 bias）

Retest:
- dp/dt：导数、二维，PASS
- b_v=0.1m/s,T=10s：数值1正确，单位误写，正确为1m
- b_a=0.05m/s²,T=10s：0.5m/s、2.5m，线性与平方增长，PASS
- dθ 的角速度混淆已纠正，保留 P2 Review
- 核心 PASS；未独立复测的符号和单位细节不记为完全稳定掌握

Source Reading Progress:
- M02 Day13 Teaching Contract 已读取
- M02 Day14 Teaching Contract 已读取
- TEMP_POSITIONING_VISION_PLAN 已复核，专项顺序保持不变

LAB / Project Progress:
- Day13 无 LAB，无源码修改

Foundation Debt:
- 无未关闭 P0/P1 debt
- P2 Review：Day11 diagonalization / quadratic form / positive definite；Day12 weak singular direction / conditioning；Day13 differential / units / bias accumulation

Lesson:
- docs/lessons/day013.md

Next:
- 定位 + 视觉理论专项 / Phase 1
- M02 / Day14 — Partial Derivative / Gradient / Chain Rule
```

---

## 8. 下一步

```text
定位 + 视觉理论专项 / Phase 1
读取 M02 Day14 Teaching Contract
→ 正式教学与 Daily Quiz
→ targeted remediation / retest（如需要）
→ 更新 PROGRESS

Phase 1 后续：Day14 → Day15
Phase 1 完成后：M03 Day16–19
M03 完成后：按专项进入 M05 Day22–26（M04 Simulation 暂不作为当前专项前置）
```
