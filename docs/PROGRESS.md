# PROGRESS — 当前课程设计与学习状态

## 1. 当前阶段

当前处于：**定位 + 视觉理论专项学习（ACTIVE）**。

专项导航文件：`docs/TEMP_POSITIONING_VISION_PLAN.md`。

当前正式学习位置：

```text
M02 Mathematical Foundations I
Day8  — Vector / Matrix / Dimension：COMPLETED / PASS
Day9  — Basis / Coordinate / Linear Transformation：COMPLETED / PASS
Day10 — Dot / Cross / Norm / Projection / Geometry：COMPLETED / PASS
Day11 — Eigenvalue / Eigenvector / Quadratic Form：COMPLETED / PASS
Next：Day12 — SVD / Rank / Conditioning
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

Curriculum v1.0 主结构不变；专项只调整当前学习优先级。

---

## 2. 最终目标
> **研究生级机器人理论基础 + 真实机器人全栈工程能力 + VLA / Mobile Manipulation具身智能能力 + 系统Owner能力。**

学习方式：
- 正常2–3h / Day；
- 理论/数学/公式/算法/源码优先；
- 已有真实机器人经验作为理论映射；
- 不要求每天实验或代码；
- 必要LAB独立安排；
- 专业模块反推基础；
- Hard Gate不过关时定向补课和复测。

---

## 3. 已锁定课程架构与Day范围
```text
M00  Robot Full-stack Architecture                         Day1
M01  C++ / Linux / ROS2 Systems                           Day2–7
M02  Mathematical Foundations I                           Day8–15
M03  Sensors & Actuators                                  Day16–19
M04  Robot Simulation Foundations                         Day20–21
M05  Vision Geometry                                      Day22–26
M06  Deep Learning Foundations                            Day27–33
M07  Deep Vision & 3D Perception                          Day34–39
M08  Mathematical Foundations II                          Day40–48
M09  State Estimation                                     Day49–54
M10  SLAM / LIO / VIO / Factor Graph                      Day55–62
M11  Planning & Navigation                                Day63–70
M12  Robot Kinematics / Dynamics / System Dynamics         Day71–79
M13  Control & Optimal Control                             Day80–89
M14  Manipulation                                          Day90–96
M15  Robot Learning                                        Day97–104
M16  VLM                                                   Day105–109
M17  VLA                                                   Day110–115
M18  Mobile Manipulation                                   Day116–118
M19  Deployment / Data / Evaluation / Sim2Real            Day119–122
M20  Safety / Reliability / Owner                          Day123–126
M21  Research Methodology & Capstone                       Day127–129
M22  Foundation Cleanup                                    Day130–135 dynamic
```

固定主课程：Day1–Day129。M22为动态槽位，只根据真实Foundation Debt生成。

---

## 4. 已锁定评估规则
普通Module Graduation Exam默认：
- **30% 核心基础**；
- **50% 综合系统场景**；
- **20% Source / Formula / Design**。

默认通过：
- 总分≥85%；
- Hard Gate不能有基础性错误；
- 单个critical concept失败：targeted remediation + targeted retest；
- 已稳定掌握内容不机械重考。

---

## 5. 当前正式LAB
- LAB01 — Manipulation Pick-and-Place
- LAB02 — Mobile Manipulation Capstone
- LAB03 — Robot Policy / VLA Action Interface

其它A*/EKF/PID/LQR/Attention/MPPI等最小实现只在明显帮助理论理解时安排。

---

## 6. Foundation Debt

### 当前状态
Day8–Day11 学习结束后，**无未关闭 P0/P1 Foundation Debt**。

保留若干 **P2 Review Debt**，用于后续 M02 module exam / Foundation Cleanup 前复测；这些不会阻塞 Day12。

### Day8 暴露但已纠正
- Identity Matrix 与 Unit Vector 混淆；
- Transpose 与 Inverse 区别遗忘；
- `y=Ax` 偏向理解为坐标/维度转换，而非一般线性映射；
- 构造 measurement matrix `H` 时，曾把 state variable 写进 H，而不是写线性系数。

### Day9 暴露但已纠正
- 将所有 `Ax` 过度理解为坐标变换；
- 基/坐标与“绝对/相对坐标”概念混淆；
- 对 rank 的独立方向含义不够明确；
- 对零空间一度仅理解为“整体降维”，未明确具体被消掉的输入方向；
- 已区分：列空间描述输出能张成到哪里；零空间描述哪些输入方向被完全消掉；一般情况下 `N(A)` 与行空间正交。

### Day10 暴露但已纠正
- 一度把 `a^T b` 的记法误读成普通矩阵变换；后续统一优先写 `a·b` 表示点积，并注明 `a·b=a^T b`；
- 区分了标量投影与投影向量；
- 初始未能解释 `R^-1=R^T`，后续通过“旋转后的基仍单位且正交，转置通过点积取回各基方向分量”完成复测。

### Day11 暴露并已纠正
- `x=cv` 初始讲解未显式区分 scalar / vector / dimension，后续固定：`c,λ∈R`，`v,x∈R^n`；
- 一度把 eigenvector 的价值理解成“用来拆任意向量”；已纠正：普通基也能拆，eigen basis 的关键是拆后各方向在 A 下不耦合；
- 一度把“任意向量经过 A”说成整体缩放；已纠正：不同 eigen components 乘不同 λ 后，合成向量一般会改变方向；
- 一度认为两个 eigenvectors 一般都正交；已纠正：一般矩阵不保证，实对称矩阵可选正交特征基；
- 一度把对角矩阵的 off-diagonal 0 归因于“正交”；已纠正：真正原因是 eigen direction 经过 A 后不产生其他 eigen direction 分量；
- 一度认为“最大 eigenvalue 就是 general matrix 最大拉伸”；已纠正：general matrix 最大/最小长度拉伸看 singular value；
- 协方差 ellipse 中已区分：eigenvalue 是主方向方差，轴尺度与 `sqrt(λ)` 成正比；
- 已区分 Gaussian elimination / row reduction 求 rank 与 eigen diagonalization 换 basis 的不同目的。

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
Exposed At: M02 / Day11
Wrong / Weak Understanding: 曾把 general matrix 最大 eigenvalue 当作最大拉伸
Debt Type: Definition / Transfer
Priority: P2
Current Level: L2
Target Level: L3
Retest: 给非对称矩阵解释为何 max eigenvalue 不能代表 max stretch，并说明 singular value 的职责
Status: LEARNING
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
M02 / Day11 — Eigenvalue / Eigenvector / Quadratic Form — COMPLETED / PASS

Mastered:
- Av=λv：eigenvector 是 A 的特殊不变方向，eigenvalue 是该方向缩放系数
- (v, λ) 是配对关系；c,λ 为 scalar，v,x 为 vector
- 任意向量可由一组基分解；eigen basis 的特殊价值是 A 作用后各 eigen direction 不互相耦合
- x=c1v1+c2v2 时，Ax=c1λ1v1+c2λ2v2
- 一般输入向量并非整体缩放；不同 eigen components 比例改变后输出方向通常改变
- 可对角化直觉：V=[v1...vn]，AV=VΛ，V^-1AV=Λ
- diagonal matrix 的 off-diagonal 0 表示 eigen directions 不互相串扰，不是“因为正交”
- 一般矩阵 eigenvectors 不一定正交；实对称矩阵可取 orthonormal eigenbasis，并有 Q^TAQ=Λ
- 实对称矩阵是可对角化矩阵中的更特殊类型
- 离散系统中 |λ|<1 衰减、|λ|>1 放大、λ<0 每步翻转方向
- covariance：eigenvector 为主不确定方向，eigenvalue 为该方向方差，轴尺度 ∝ sqrt(λ)
- general matrix 最大/最小拉伸不能直接看 eigenvalue，应看 singular value

Weak:
- eigen diagonalization 从 AV=VΛ 到 V^-1AV=Λ 的推导需要后续闭卷复述
- covariance ellipse / LQR cost contour / Hessian curvature 三种 quadratic-form 应用需后续再次对比
- positive definite 已理解定义和基本 cost 直觉，但独立复测不足
- eigenvalue 与 singular value 的职责边界将在 Day12 继续强化

Wrong Understanding:
- 曾把 eigenvector 理解为“必须用它才能拆任意向量”
- 曾把 general matrix 最大 eigenvalue 当作最大拉伸倍数
- 曾认为 eigenvectors 一般正交
- 曾把 diagonal matrix 的 0 归因于 orthogonality
- 曾把 covariance eigenvalue 直接当椭圆轴长度
- 曾把 Gaussian elimination 求 rank 与 eigen diagonalization 混在一起

Corrected:
- 普通基也能表示任意向量；eigen basis 价值在于 A 下各方向独立缩放、不发生耦合
- 对称矩阵的 orthogonality 是额外性质；diagonalization 的根本条件是有足够多线性无关 eigenvectors
- covariance eigenvalue=variance，标准差/轴尺度与 sqrt(λ) 成正比
- general matrix 最大/最小长度拉伸由 singular values 描述
- row reduction 用于 rank / solve equations；eigen diagonalization 是换 basis 描述同一 linear transformation

Retest:
- 能解释 λ=-0.5：每步翻转、幅值减半、长期趋近0
- x=3v1-2v2，Av1=5v1，Av2=2v2 → Ax=15v1-4v2
- 能说明“特征向量不是为了能拆，而是为了拆完不耦合”
- 能说明实对称矩阵的不同 eigen directions 正交
- 能说明实对称矩阵 ⊂ 可对角化矩阵
- Retest PASS

Source Reading Progress:
- M02 Day11 Teaching Contract 已读取
- M02 Day12 Teaching Contract 已读取

LAB / Project Progress:
- Day11 无 LAB

Foundation Debt:
- 无未关闭 P0/P1 debt
- P2 Review：diagonalization、quadratic-form applications、eigen vs singular、positive definite

Lesson:
- docs/lessons/day011.md

Next:
- M02 / Day12 — SVD / Rank / Conditioning
```

---

## 8. 下一步

```text
读取 M02 Day12 Teaching Contract
→ 使用完整 Day12 讲义
→ 正式教学
→ Daily Quiz
→ targeted remediation / retest（如需要）
→ 更新 PROGRESS
```
