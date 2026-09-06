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
Next：Day11 — Eigenvalue / Eigenvector / Quadratic Form
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
Day8–Day10 学习结束后，**无未关闭 P0/P1 Foundation Debt**。

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

以上均已通过定向复测，不保留为 OPEN debt。

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
M02 / Day10 — Dot / Cross / Norm / Projection / Geometry — COMPLETED / PASS

Mastered:
- 2-norm 与速度大小 / 欧氏距离
- 点积 a·b 的几何意义与夹角关系
- 点积为 0 对应正交
- 标量投影与投影向量的区别
- 路径切向/法向投影的机器人意义
- 叉积与右手定则
- 法向量几何意义
- point-to-plane ICP 中 n·(p-q) 作为法向距离误差
- 旋转矩阵列向量保持单位与正交，因此 R^T R=I，R^-1=R^T

Weak:
- 需继续保持“点积”和“矩阵线性变换”的记号区分

Wrong Understanding:
- 曾因 a^T b 记法看不到“点”，把点积与矩阵变换混淆
- 曾把标量投影 3 与投影向量 [3,0]^T 混为同一结果

Corrected:
- 后续优先使用 a·b 表示点积；必要时补 a·b=a^T b
- 标量投影 = a·b_hat；投影向量 = (a·b_hat)b_hat
- R^T 的每一行对应旋转后基向量的转置，用点积取回向量在该基方向的分量

Retest:
- a=[3,4]^T, b=[1,0]^T：标量投影=3，投影向量=[3,0]^T
- 能解释旋转基仍保持单位与正交，从而 R^-1=R^T
- Retest PASS

Source Reading Progress:
- M02 Day10 Teaching Contract 已读取

LAB / Project Progress:
- Day10 无 LAB

Foundation Debt:
- 无未关闭 P0/P1 debt

Lesson:
- docs/lessons/day010.md

Next:
- M02 / Day11 — Eigenvalue / Eigenvector / Quadratic Form
```

---

## 8. 下一步

```text
读取 M02 Day11 Teaching Contract
→ 使用完整 Day11 讲义
→ 正式教学
→ Daily Quiz
→ targeted remediation / retest（如需要）
→ 更新 PROGRESS
```
