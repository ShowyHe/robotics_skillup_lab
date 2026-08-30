# PROGRESS — 当前课程设计与学习状态

## 1. 当前阶段

当前处于：**定位 + 视觉理论专项学习（ACTIVE）**。

专项导航文件：`docs/TEMP_POSITIONING_VISION_PLAN.md`。

当前正式学习位置：

```text
M02 Mathematical Foundations I
Day8 — Vector / Matrix / Dimension：COMPLETED / PASS
Next：Day9 — Basis / Coordinate / Linear Transformation
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

## 4. Curriculum v1.0审计结论
已完成：
- M00–M22能力树、依赖和Day索引；
- M04/M09/M10毕业考试统一到默认30/50/20；
- 修复M11前置倒挂：Hybrid A*/C-space在本日补最小直觉，M12后续正式化；
- 修复M15 Day100循环前置；
- M13明确 `𝒞/𝒪` 符号并限定eigenvalue稳定性适用语境；
- M14补force/impedance frame/error/sign convention；
- M15增强ACT/CVAE与Diffusion最小数学主链；
- M07补PointCloud clustering；
- M19补Orin power/thermal/memory-bandwidth engineering；
- M21 Day127–129核心点压到≤20，并与最终Capstone闭环；
- M22新增Constrained Optimization、Algorithm Theory、CMake/colcon、Python/NumPy候选Debt池；
- 增加LAB03 learned policy/VLA action-interface闭环；
- LAB02增加M21 Research Extension；
- 主课程进入v1.0冻结，不再无理由扩Module/Day。

---

## 5. 已锁定评估规则
普通Module Graduation Exam默认：
- **30% 核心基础**；
- **50% 综合系统场景**；
- **20% Source / Formula / Design**。

默认通过：
- 总分≥85%；
- Hard Gate不能有基础性错误；
- 单个critical concept失败：targeted remediation + targeted retest；
- 已稳定掌握内容不机械重考。

M00为1-Day总纲，保留轻量Owner场景考试例外；M22使用同一30/50/20权重但采用Foundation Debt Defense题型。

---

## 6. 当前正式LAB
### LAB01 — Manipulation Pick-and-Place
验证 Object Pose / TF / IK / Collision / Planning / Timed Trajectory / Gripper / Attach / Contact / Recovery。

### LAB02 — Mobile Manipulation Capstone
验证 Task→Navigation→Base Verification→Re-perception→Reachability→Pick→Carry→Place→Long-horizon State→Recovery；若作为M21最终Capstone，还必须完成Baseline/Hypothesis/Repeated Trials/Ablation/Reproducibility/Defense。

### LAB03 — Robot Policy / VLA Action Interface
验证 Dataset Alignment→BC/ACT或Diffusion-style Policy→Raw Action→Decode→Safety Filter→Controller→Closed-loop Evaluation，强制区分Offline Action Accuracy与Robot Success。

其它A*/EKF/PID/LQR/Attention/MPPI等最小实现只在明显帮助理论理解时安排。

---

## 7. 当前能力起点
### 已有工程优势
- ROS2真实机器人开发/调试；
- Nav2 / Navigation；
- Behavior Tree / Planner / Costmap；
- MPPI / Controller实际问题分析；
- LiDAR / LIO / GPS / RTK系统接触；
- rosbag/log/源码证据链分析；
- 测试开发经验；
- DQN/PPO/TD3等RL接触。

### 重点补强
- 系统化数学、Probability / Optimization / SE(3)；
- Kinematics / Dynamics / State-space / Control；
- Vision Geometry / 3D Perception / Manipulation；
- Deep Learning / Robot Learning / VLM / VLA；
- Deployment / Sim2Real / Safety / Research Methodology。

CMake/colcon、Python/NumPy等工具基础不预设熟练度：若真实阻塞源码构建/AI数据处理，则进入M22 Candidate Debt Pool。

---

## 8. Foundation Debt

### 当前状态
Day8 学习结束后，**无未关闭 P0/P1 Foundation Debt**。

Day8 暴露但已当日纠正的内容：
- Identity Matrix 与 Unit Vector 混淆；
- Transpose 与 Inverse 区别遗忘；
- `y=Ax` initially 偏向理解为坐标/维度转换，而非一般 linear mapping；
- 构造 measurement matrix `H` 时，曾把 state variable 写进 H，而不是写线性系数。

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

M22只基于这里的真实Debt动态生成。

---

## 9. Daily Learning记录模板
```text
Current Module / Day:
Mastered:
Weak:
Wrong Understanding:
Corrected:
Retest:
Source Reading Progress:
LAB / Project Progress:
Foundation Debt:
Next:
```

---

## 10. 当前 Daily Learning Record

```text
Current Module / Day:
M02 / Day8 — Vector / Matrix / Dimension — COMPLETED / PASS

Mastered:
- scalar / vector / matrix 与 dimension reasoning
- matrix multiplication legality 与结果维度
- matrix × vector calculation
- y=Ax 作为一般 linear mapping
- transpose / identity / inverse 的区别
- invertibility 的信息丢失直觉
- 根据 state 语义构造简单 measurement matrix H

Weak:
- 入口阶段对 identity / inverse / transpose 有遗忘
- 对 linear mapping 的一般性理解需要恢复
- 初次构造 H 时把 coefficient 与 state variable 混淆

Wrong Understanding:
- 曾把 I 与 unit vector 混淆
- 曾把 A^-1 误认为可能是 transpose
- 曾写 H=[0,0,x,0] 选择 θ

Corrected:
- I 是 Identity Matrix，Ix=x；unit vector 满足 ||u||=1
- A^T 是 transpose；A^-1 是 inverse
- y=Ax 表示一般 linear mapping，可改变维度也可不改变
- H 中写 linear coefficients；选择第3维应写 [0,0,1,0]

Retest:
- 对 state [p_x,p_y,v_x,v_y]^T，只观测 v_y，正确构造 H=[0,0,0,1]
- Retest PASS

Source Reading Progress:
- M02 Day8 Teaching Contract 已读取

LAB / Project Progress:
- Day8 无 LAB

Foundation Debt:
- 无未关闭 P0/P1 debt

Lesson:
- docs/lessons/day008.md

Next:
- M02 / Day9 — Basis / Coordinate / Linear Transformation
```

---

## 11. 下一步

```text
读取 M02 Day9 Teaching Contract
→ 必要入口检查
→ 生成 / 使用完整 Day9 讲义
→ 正式教学
→ Daily Quiz
→ targeted remediation / retest（如需要）
→ 更新 PROGRESS
```

Day编号是课程逻辑位置，不等于必须消耗固定自然日；核心前置未掌握时不机械推进。
