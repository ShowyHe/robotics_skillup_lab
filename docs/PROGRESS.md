# PROGRESS — 当前课程设计与学习状态

## 1. 当前阶段

当前处于：**完整机器人全栈 / 具身智能学习体系设计阶段**。

尚未正式开始新课程 Day1。

---

## 2. 已确认内容

### 最终目标

已确认最终目标为：

> **研究生级机器人理论基础 + 真实机器人全栈工程能力 + VLA / Mobile Manipulation 具身智能能力 + 系统 Owner 能力。**

### 学习方式

已确认：

- 正常学习时间约 **2–3h / Day**；
- 理论知识是绝对主线；
- 数学、公式、算法原理、源码理解优先；
- 已有 ROS2 / Nav2 / 测试开发 / 实机经验用于帮助理解理论；
- 不要求每天做实验；
- 不要求每天写代码；
- 必要 LAB / PROJECT 单独安排完整时间；
- 专业模块反推基础，而不是先完整学完所有基础课。

### 源码学习

已有真实工程对应模块默认：

```text
真实问题
→ 公司真实实现
→ 反推缺失基础
→ 数学 / 算法本体
→ 官方标准实现
→ 必要最小自实现
→ 回真实系统验证
```

默认不为了横向比较刷大量第三方仓库。

---

## 3. 已锁定课程架构

已确认 **M00–M22**：

- M00 Robot Full-stack Architecture
- M01 C++ / Linux / ROS2 Systems
- M02 Mathematical Foundations I
- M03 Sensors & Actuators
- M04 Robot Simulation Foundations
- M05 Vision Geometry
- M06 Deep Learning Foundations
- M07 Deep Vision & 3D Perception
- M08 Mathematical Foundations II
- M09 State Estimation
- M10 SLAM / LIO / VIO / Factor Graph
- M11 Planning & Navigation
- M12 Robot Kinematics / Dynamics / System Dynamics
- M13 Control & Optimal Control
- M14 Manipulation
- M15 Robot Learning
- M16 VLM
- M17 VLA
- M18 Mobile Manipulation
- M19 Deployment / Data / Evaluation / Sim2Real
- M20 Safety / Reliability / Owner
- M21 Research Methodology & Capstone
- M22 Foundation Cleanup

---

## 4. 已完成课程设计层

以下内容已完成并确认：

- 最终角色与能力边界；
- 完整能力树；
- 研究生理论补强要求；
- 专业模块 → 基础依赖图；
- M00–M22 总顺序；
- 每个 Module 的主要知识范围；
- 每个 Module 的毕业标准第一版；
- 理论优先教学模式；
- 实验按必要性独立安排原则；
- L1–L5 掌握等级；
- 跳级 / 补课 / 复测原则；
- 后续 GitHub 文档分层。

---

## 5. 当前未完成

### A. Module Day 数

尚未确定：

- 每个 M00–M22 需要多少理论 Day；
- 哪些内容可以通过入门考试跳过；
- 哪些模块需要独立 LAB / PROJECT。

### B. Day1–DayN 总索引

尚未生成。

后续应先完成 Module Day 数设计并进行全局审查，再生成 Day 总索引。

### C. Module 详细计划

`docs/modules/` 尚未建立具体内容。

未来每个 Module 文件负责：

- Module 内 Day 顺序；
- 每 Day 主题；
- 前置；
- 核心理论；
- 考核节点；
- 必要 LAB 引用。

### D. Daily Lessons

`docs/lessons/` 尚未生成。

讲义应在真正学习到对应 Day 时逐日生成，不提前批量生成大量低质量讲义。

### E. LAB / Project

`docs/labs/` 尚未确定具体列表。

实验数量由知识价值决定，不提前为了课程形式凑数量。

---

## 6. 当前能力起点（待入门诊断校准）

### 已有工程优势

- ROS2 真实机器人开发 / 调试；
- Nav2 / Navigation；
- Behavior Tree / Planner / Costmap；
- MPPI / Controller 实际问题分析；
- LiDAR / LIO / GPS / RTK 等系统接触；
- rosbag / log / 源码证据链分析；
- 测试开发经验；
- DQN / PPO / TD3 等 RL 接触。

### 重点补强

- 系统化数学；
- 概率 / 优化 / SE(3)；
- 机器人运动学 / 动力学；
- 状态空间与控制理论；
- Vision Geometry / 3D Perception；
- Manipulation；
- Deep Learning 系统基础；
- Robot Learning；
- VLM / VLA；
- Research Methodology。

以上只是课程设计起点，正式进入各 Module 时通过入门测试重新校准。

---

## 7. 下一步

下一步课程设计任务：

```text
逐个 M00–M22 估算理论知识容量
→ 根据每天 2–3h 确定理论 Day 数
→ 标记可以考试跳过的部分
→ 只标记真正必要的 LAB / PROJECT
→ 全局检查总长度、重复、前置关系
→ 生成 Day1–DayN 总索引
→ 再建立 docs/modules/
```

当前不要直接开始批量生成详细 Daily Lessons。

---

## 8. Foundation Debt

当前尚未进入正式课程，暂不建立具体债务清单。

未来记录格式：

```text
知识点：
来源 Module / Day：
问题：
当前等级：
需要达到等级：
计划复测：
状态：OPEN / FIXED / RETEST
```

M22 将基于这里累计的 Foundation Debt 动态生成，而不是提前写死。