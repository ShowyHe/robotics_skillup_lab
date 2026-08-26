# PROGRESS — 当前课程设计与学习状态

## 1. 当前阶段
当前处于：**Curriculum v1.0设计与二次审计完成，准备进入正式学习/入口诊断阶段。**

尚未正式从新课程 Day1 开始。

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

## 7. 当前能力起点（正式学习时仍需入口诊断）
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

CMake/colcon、Python/NumPy等工具基础不预设熟练度：先入口诊断，若真实阻塞源码构建/AI数据处理，则进入M22 Candidate Debt Pool。

---

## 8. Foundation Debt
正式学习尚未开始，因此当前没有具体Debt条目。

未来统一记录：
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

## 10. 下一步
课程设计阶段结束。下一步：
```text
读取 M00 Teaching Contract
→ M00 / Day1 入口检查（如需要）
→ 正式生成 lessons/day001.md 或直接进行Day1教学
→ Daily Quiz
→ 更新PROGRESS
→ 按Module Graduation Exam推进
```

已有内容可以通过入口诊断跳过/压缩；Day编号是课程逻辑索引，不强制机械消耗135个自然日。
