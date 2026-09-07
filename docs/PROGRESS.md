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
M02 Day14 — Partial Derivative / Gradient / Chain Rule：COMPLETED / PASS
M02 Day15 — Jacobian / Hessian / Taylor / Linearization：COMPLETED / PASS
Next：M02 Module Graduation Exam — NOT STARTED
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

**当前不是按 Day1–Day135 机械顺序推进。** Curriculum v1.0 主结构不变；专项只调整优先级。Day15 已完成，先进行 M02 Module Graduation Exam，通过后进入 M03，M03 后直接进入 M05；M04 Simulation 暂不作为本专项前置主线。Day15 PASS 不等于 M02 模块毕业。

---

## 2. 最终目标与学习方式

> 研究生级机器人理论基础 + 真实机器人全栈工程能力 + VLA / Mobile Manipulation 具身智能能力 + 系统 Owner 能力。

正常 2–3h / Day；理论、数学、公式、算法、源码理解优先；已有真实机器人经验用于映射但不替代 Hard Gate。不要求每天代码或 LAB；必要 LAB 独立安排。专项不以交付日期压缩核心理论。数学与机器人讲解以中文术语为主，英文首次出现时括号注释；题目独立列全条件、变量维度、单位和所求量。

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

Day8–Day15 已完成，**无未关闭 P0/P1 Foundation Debt**。保留 P2 Review Debt，供 M02 Module Graduation Exam、M08/M09/M10 对应知识重现和 Foundation Cleanup 复测，不阻塞模块考试。以下保留历史错误、纠正和未关闭点，不因结项删除。M02 考试尚未完成，不能提前宣布模块毕业或关闭全部 Review Debt。

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

### Day14 暴露并已纠正

- 初次把 `4(y-1)^2` 的偏导算成 `8y-4`，导致梯度误写 `[2,20]^T`；已纠正为 `8y-8`、`[2,16]^T`。
- 曾把方向导数写成 `J·d`；已纠正为 `∇J·d`，J 是标量。
- 曾认为梯度下降应使用加号、负梯度必须是负数；已纠正为加号用于局部上升，最小化沿负梯度，变量增大不等于代价增大。
- 对 α 的作用和梯度下降用途不清楚；通过一维目标 `J(p)=(p-2)^2` 理解导数给方向、α 控制更新幅度。
- 原计算图箭头连写难以阅读；拆成完整变量定义和各层计算后完成理解，不将题目表达问题误记为不会求导。
- 二维代价中曾漏乘 `p_y=2u_y` 的内层导数，误写 `4u_y-8`；最终独立复测得到 `8u_y-8`、更新值 1.2。
- 最初将梯度的用途与 noise/bias 分析混同；已纠正为梯度提供优化变量对目标的局部敏感度。

### Day15 暴露并已纠正

- 曾把 `R³→R²` 的雅可比维度写成3×2；已纠正为输出决定行、输入决定列。
- 曾把列梯度写成1×4、海森写成1×3，或把海森只写成对角元素向量；已纠正完整 n×n 二阶偏导矩阵。
- 曾把4关节→3维末端位置的雅可比写成4×3；已纠正为3×4，须检查矩阵乘法维度。
- 曾用梯度不为0判断非线性；自行纠正为梯度变化率，并进一步明确某点二阶导为0不证明全局线性。
- 曾把雅可比泛称为梯度矩阵；已纠正为向量函数的一阶导数矩阵，海森是标量梯度的雅可比。
- EKF、SLAM 的英文术语妨碍理解；已补中文名称、测量模型、残差模型及符号约定。
- Day15 核心复测通过，但新的矩阵维度、完整海森及跨场景迁移仍需模块考试独立验证。

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
Current Level: L3（Day14 定义复测通过）
Target Level: L3
Retest: 在 M02 Module Exam 的局部变化或 Jacobian 场景中复测，正确写 rad / rad/s
Status: RETEST
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

### P2 Review Debt — Day14

```text
Knowledge: Multivariable chain rule / inner derivative
Exposed At: M02 / Day14
Wrong / Weak Understanding: 内层系数容易漏乘，参数与位置之间的偏导需强化单位和维度检查
Debt Type: Calculation / Derivation / Transfer
Priority: P2
Current Level: L3（本日复测通过）
Target Level: L3
Retest: M02 Module Exam 使用不同的参数→状态→代价链独立求导，并检查单位
Status: RETEST
```

```text
Knowledge: Gradient descent sign / objective vs variable
Exposed At: M02 / Day14
Wrong / Weak Understanding: 曾认为加号才是梯度下降、负梯度必须为负数，混淆变量大小与代价下降
Debt Type: Definition / Transfer
Priority: P2
Current Level: L3（本日复测通过）
Target Level: L3
Retest: 给定不同工作点和正负导数，独立判断更新方向及代价值变化
Status: RETEST
```

```text
Knowledge: Computational graph dependency / notation
Exposed At: M02 / Day14
Wrong / Weak Understanding: 箭头连写和未完整声明的中间变量造成阅读困难，拆开后已能解释
Debt Type: Notation / Transfer
Priority: P2
Current Level: L2-L3
Target Level: L3
Retest: 给完整独立条件，写出正向中间值和各层局部导数，再组合总导数
Status: OPEN
```

### P2 Review Debt — Day15

```text
Knowledge: Jacobian dimensions / output rows / input columns / gradient convention
Exposed At: M02 / Day15
Wrong / Weak Understanding: 多次将m×n写反，混淆列梯度与行雅可比；机械臂4→3维度初次写反
Debt Type: Dimension / Definition / Transfer
Priority: P2
Current Level: L2-L3（本日概念纠正）
Target Level: L3
Retest: 在新向量模型中独立求雅可比、检查乘法维度，并区分列梯度和行雅可比
Status: OPEN
```

```text
Knowledge: Hessian matrix / second partial derivatives / curvature
Exposed At: M02 / Day15
Wrong / Weak Understanding: 曾只写对角元素向量，漏交叉偏导；海森维度曾误写1×3
Debt Type: Calculation / Dimension / Definition
Priority: P2
Current Level: L2-L3（4×4结构复述通过）
Target Level: L3
Retest: 对含交叉项的不同标量函数独立求完整海森，解释行列和曲率意义
Status: OPEN
```

```text
Knowledge: Jacobian vs gradient vs Hessian / local linearization
Exposed At: M02 / Day15
Wrong / Weak Understanding: 曾把雅可比泛称为梯度矩阵，把梯度非零与非线性混同
Debt Type: Definition / Transfer
Priority: P2
Current Level: L2-L3（概念复盘通过）
Target Level: L3
Retest: 区分非代价向量模型与标量代价的导数，并解释局部二阶导为0不能证明全局线性
Status: RETEST
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
M02 / Day15 — Jacobian / Hessian / Taylor / Linearization — COMPLETED / PASS
M02 Module Graduation Exam — NOT STARTED

Specialty Context:
- TEMP_POSITIONING_VISION_PLAN：ACTIVE
- 当前 Phase：Phase 1 — Mathematical Foundations I
- Phase 1 范围：M02 Day8–15
- Day15 已完成；M02考试通过后进入 M03 Day16–19
- M03 完成后按专项进入 M05 Day22–26；M04 Day20–21 当前不作为专项前置主线

Mastered:
- 向量函数的雅可比收集每个输出对每个输入的一阶偏导
- 独立求出 f(x,y)=[x²+y,xy]^T 的雅可比及两个工作点的数值
- 独立使用 Δy≈J_fΔx 计算二维小变化
- 一阶泰勒用当前函数值、当前导数和输入变化估算新值
- 能解释工作点变化后雅可比可能变化，线性化不是永久线性化
- 能求简单标量代价的梯度，理解海森是梯度的雅可比
- 能解释4输入标量函数的海森为何为4×4
- 能说明关节变化→末端变化、状态变化→预测测量/残差变化的局部敏感度意义
- 已区分梯度不为0与非线性，理解某点二阶导为0不能证明全局线性

Weak:
- 雅可比输出行/输入列的维度顺序需在新模型中独立复测
- 完整海森矩阵、交叉偏导及列梯度/行雅可比约定需强化
- 雅可比、梯度、海森的区别及跨 EKF/SLAM/IK/Control 迁移需继续复盘
- 延续 Day11–14 P2：特征分解、二次型、奇异值退化、积分单位、链式法则内层系数

Wrong Understanding:
- 曾把3输入2输出的雅可比写成3×2，4关节到3维位置写成4×3
- 曾把海森写成标量或对角元素向量，漏掉交叉偏导
- 曾混淆 ∇C 与 ΔC、列梯度与行雅可比
- 曾以梯度非零判断非线性，后自行纠正为梯度变化率并补充局部/全局边界
- 曾把雅可比泛称为所有场景的梯度矩阵

Corrected:
- J_f∈R^(m×n)，输出决定行、输入决定列；列梯度∈R^n，行雅可比为其转置
- H_C=J_(∇C)∈R^(n×n)，必须包含所有二阶偏导
- 线性化只在工作点附近近似；模型误差不会被线性化自动消除
- 非零二阶导说明局部非仿射，某点二阶导为0不能证明整体线性
- 中文术语为主，英文首次出现时括号注释；题目完整给出条件

Retest:
- f=[x²+y,xy]^T：雅可比 [[2x,1],[y,x]]，PASS
- 在(1,2)处 Δx=[0.01,0.02]^T：Δy≈[0.04,0.04]^T，PASS
- f(x)=x²、x0=2、Δx=0.1：一阶估算4.4，PASS；真实值4.41已补全
- J_f(1,2)=[[2,1],[2,1]]、J_f(3,4)=[[6,1],[4,3]]，PASS
- f:R^4→R^3：雅可比3×4，PASS
- C=x²+3y²：梯度[2x,6y]^T正确；完整海森经纠正为[[2,0],[0,6]]
- 4输入标量函数海森4×4：能解释每个梯度分量再对4个输入求偏导，概念PASS
- 线性化不会永久改变非线性函数，原函数梯度随工作点变化，PASS
- 新模型的雅可比维度、含交叉项海森及跨场景迁移保留 P2，不提前记稳定掌握

Source Reading Progress:
- M02 Day15 Teaching Contract 与 M02 Graduation Exam Specification 已读取
- LEARNING_RULES、PROGRESS 与当前专项路线已复核
- 无公司源码阅读；工程连接仅为概念映射，未声明具体源码实现事实

LAB / Project Progress:
- Day15 无 LAB，无公司源码修改

Foundation Debt:
- 无未关闭 P0/P1 debt
- P2 Review：Day11 diagonalization / quadratic form / positive definite；Day12 weak singular direction / conditioning；Day13 integration units / sampling；Day14 chain rule / gradient sign / computational graph；Day15 Jacobian dimensions / Hessian / derivative relations

Lesson:
- docs/lessons/day015.md

Next:
- M02 Mathematical Foundations I — Module Graduation Exam（Day8–15，NOT STARTED）
- 考试总分≥85%，Hard Gate独立通过；失败项定向补课和复测
- 正式毕业后按专项进入 M03 Day16–19
```

---

## 8. 下一步

```text
定位 + 视觉理论专项 / Phase 1
M02 Module Graduation Exam — Day8–15
→ 30%核心基础 / 50%综合系统场景 / 20%公式与设计
→ 总分≥85%，Hard Gate独立通过
→ 定向处理未通过的 Foundation Debt
→ 通过后更新 M02毕业状态与 PROGRESS

Phase 1 毕业后：M03 Day16–19
M03 完成后：按专项进入 M05 Day22–26（M04 Simulation 暂不作为当前专项前置）
```