# PROGRESS — 当前课程设计与学习状态

## 1. 当前阶段

当前处于：**定位 + 视觉理论专项学习（ACTIVE）**。

专项导航文件：`docs/TEMP_POSITIONING_VISION_PLAN.md`。

当前正式学习位置：

```text
定位 + 视觉理论专项
Phase 1 — Mathematical Foundations I

M02 Mathematical Foundations I
Day8  — Vector / Matrix / Dimension：COMPLETED / PASS
Day9  — Basis / Coordinate / Linear Transformation：COMPLETED / PASS
Day10 — Dot / Cross / Norm / Projection / Geometry：COMPLETED / PASS
Day11 — Eigenvalue / Eigenvector / Quadratic Form：COMPLETED / PASS
Day12 — SVD / Rank / Conditioning：COMPLETED / PASS
Next：Day13 — Derivative / Differential / Numerical Integration
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

**注意：当前不是按 Day1–Day135 机械顺序推进。** Curriculum v1.0 主结构不变，专项只改变当前学习优先级；M02 完成后进入 M03，M03 后按专项直接进入 M05，M04 Simulation 暂不作为当前专项前置主线。

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
Day8–Day12 学习结束后，**无未关闭 P0/P1 Foundation Debt**。

保留若干 **P2 Review Debt**，用于后续 M02 Module Graduation Exam、M08/M10 对应知识重现时和 Foundation Cleanup 前复测；这些不会阻塞 Day13。

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

### Day12 暴露并已纠正
- 初始将 eigenvalue 与 singular value 区别表述为“eigenvalue 有方向耦合，singular value 没有”；已纠正：eigenvalue 只描述 invariant eigen-directions，而 SVD 在一般输入方向中寻找长度作用最清晰的正交方向；
- 初始把小 singular value 的 noise amplification 理解为“一个方向误差会放大到其他方向”；已纠正：在 singular basis 中各方向已解耦，弱方向自己的 measurement noise 会在反解该弱方向 state 时被 `1/σ` 放大；
- 初始把 conditioning 与“矩阵特征值是否正交”混淆；已纠正：invertibility 看是否存在 `σ=0`，conditioning 看 `σ_max/σ_min` 与反解敏感度；
- 平墙退化场景中一度补充“需要更多垂直墙移动”；已纠正：垂直墙方向本来信息强，真正缺失的是沿墙方向约束，需要其他几何、sensor 或约束补充。

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
M02 / Day12 — SVD / Rank / Conditioning — COMPLETED / PASS

Specialty Context:
- TEMP_POSITIONING_VISION_PLAN：ACTIVE
- 当前 Phase：Phase 1 — Mathematical Foundations I
- Phase 1 范围：M02 Day8–15
- Day13 之后仍继续 M02 Day14–15；完成 M02 后进入 M03 Day16–19
- M03 完成后按专项进入 M05 Day22–26；M04 Day20–21 当前不作为专项前置主线

Mastered:
- 一般矩阵的最大/最小长度拉伸由 singular value 描述，不能直接看 max eigenvalue
- A=UΣV^T 的几何链：V^T 换到右奇异输入基 → Σ 各方向独立伸缩 → U 放到左奇异输出方向
- Av_i=σ_i u_i：v_i 为特殊输入方向，u_i 为对应输出方向，σ_i 为长度缩放倍数
- rank(A) 等于非零 singular values 数量
- σ=0 表示该输入方向信息完全丢失；σ≈0 表示工程上该方向信息极弱、接近退化
- κ(A)=σ_max/σ_min 描述反解敏感度与不同方向信息强弱差异
- 可逆只说明没有方向被完全压成0；conditioning 进一步说明解是否数值可靠
- 小 σ 会使该弱奇异方向自己的 measurement noise 在反解 state 时被 1/σ 放大
- 理想无限平墙 point-to-plane ICP：垂直墙方向约束强，沿墙平移方向约束弱、singular value 小
- pseudoinverse 用于普通 inverse 不适用时的 least-squares / 有效方向反解直觉

Weak:
- 小 singular value / inverse sensitivity 在第一次问答中反复出现“误差串到其他方向”的错误表述；定向复测已通过，但需在后续 LS / Jacobian / SLAM 再次 review
- invertibility 与 conditioning 已区分，但后续需在真实 singular spectrum / Hessian 场景闭卷判断
- SVD 三矩阵几何作用已掌握，不要求复杂手算；后续重点是迁移到 Jacobian、SLAM degeneracy、calibration

Wrong Understanding:
- 曾把 eigenvalue vs singular value 区别说成“eigenvalue 有方向耦合、singular value 没有”
- 曾把弱奇异方向的 noise amplification 理解为一个方向误差跑到其他方向
- 曾把可逆性解释成“矩阵特征值是正交的”
- 平墙退化场景中曾错误认为应“增加更多垂直墙移动”来解决沿墙方向信息不足

Corrected:
- eigenvalue 只描述 invariant eigen-directions；singular value 描述一般矩阵特殊输入方向的长度作用
- singular basis 中方向已经解耦；弱方向自己的 measurement noise 在反解该方向时被 1/σ 放大
- invertibility：是否存在 σ=0；conditioning：σ_max/σ_min 是否导致逆问题高度敏感
- 平墙场景缺的是沿墙方向的几何约束，需其他几何结构 / sensor / prior，而不是继续强化已强约束的垂直方向

Retest:
- σ=0.01，弱方向 measurement noise Δb=0.002 → Δx=0.2
- 能明确说明被放大的是“该弱奇异方向本身的 state error”
- Retest PASS

Source Reading Progress:
- M02 Day12 Teaching Contract 已读取
- M02 Day13 Teaching Contract 已读取
- TEMP_POSITIONING_VISION_PLAN 已复核，专项执行顺序保持不变

LAB / Project Progress:
- Day12 无 LAB

Foundation Debt:
- 无未关闭 P0/P1 debt
- P2 Review：Day11 diagonalization / quadratic-form / positive definite；Day12 weak singular direction / conditioning

Lesson:
- docs/lessons/day012.md

Next:
- 定位 + 视觉理论专项 / Phase 1
- M02 / Day13 — Derivative / Differential / Numerical Integration
```

---

## 8. 下一步

```text
定位 + 视觉理论专项 / Phase 1
读取 M02 Day13 Teaching Contract
→ 使用完整 Day13 讲义
→ 正式教学
→ Daily Quiz
→ targeted remediation / retest（如需要）
→ 更新 PROGRESS

Phase 1 后续：Day13 → Day14 → Day15
Phase 1 完成后：M03 Day16–19
M03 完成后：按专项进入 M05 Day22–26（M04 Simulation 暂不作为当前专项前置）
```
