# 00_GOALS — 最终目标、边界与毕业能力

## 1. 最终目标

本学习体系的最终目标是：

> **形成研究生级机器人理论基础、真实机器人全栈工程能力、VLA / Mobile Manipulation 具身智能能力，并逐步达到系统 Owner 水平。**

目标不是只会运行 Demo，也不是只会调参，而是能够从真实机器人失败现象一路追到：

- 传感器 / 执行器；
- ROS2 / 调度 / 通信；
- 感知；
- 状态估计；
- 规划；
- 控制；
- Manipulation；
- Robot Learning；
- VLM / VLA；
- 部署与数据闭环。

并判断究竟应该由哪一层解决问题。

---

## 2. 三条主能力线

### A. 机器人理论

目标达到机器人硕士核心课程可用深度，覆盖：

- 线性代数、微积分、概率统计；
- 数值计算、最小二乘、优化；
- SO(3) / SE(3) 与机器人几何；
- 状态估计；
- 规划；
- 运动学 / 动力学；
- 控制与最优控制。

重点不是数学证明本身，而是能够计算、推导、理解公式假设，并把数学用于算法设计。

### B. 机器人工程

覆盖：

- C++ / Python；
- Linux / process / thread / memory / network；
- ROS2 / DDS / QoS / Executor / TF / Lifecycle / Plugin；
- ros2_control；
- Sensor / Actuator / Calibration / Synchronization；
- Simulation；
- Orin / TensorRT / Deployment；
- rosbag / logging / benchmark / regression；
- Safety / Recovery / Owner-level debugging。

### C. 具身智能

覆盖：

- Vision / 3D Perception；
- Manipulation；
- Deep Learning；
- Robot Learning；
- VLM；
- VLA；
- Mobile Manipulation；
- 数据采集、训练、部署、失败回流。

最终要理解 VLA 输出的 action 如何真正落到机器人 state / trajectory / controller / actuator。

---

## 3. 最终角色画像

最终目标角色：

**Robot Full-stack / Embodied AI Engineer**

进一步目标：

**VLA + Mobile Manipulation 方向的系统 Owner。**

Owner 能力包括：

- 划分模块责任边界；
- 设计输入输出与数据契约；
- 判断参数问题还是架构问题；
- 选择算法并解释 trade-off；
- 设计安全边界与 fallback；
- 建立 benchmark / regression；
- 用日志、bag、指标和实验建立证据链；
- 对跨模块真实机器人结果负责。

---

## 4. 学习深度等级

统一采用：

- **L1 — 见过**：认识术语和基本用途。
- **L2 — 能解释**：能够说明机制、输入输出和物理意义。
- **L3 — 能计算 / 推导**：能够处理核心公式、推导与定量问题。
- **L4 — 能实现 / Debug**：能够实现关键部分、读源码、定位真实问题。
- **L5 — 能迁移 / 修改 / 设计**：能够修改算法、迁移新系统、做架构和参数设计。

最终深度不是所有领域都一样：

- Robot System / ROS2 / Debug：L4→L5；
- Navigation / Planning：L4→L5；
- Control：L4，关键部分向 L5；
- State Estimation / SLAM：L3→L4；
- Perception：L3→L4；
- Kinematics / Dynamics / Manipulation：L3→L4；
- Deep Learning / Robot Learning：L3→L4；
- VLM / VLA：L3→L4，并作为长期主方向向 L5；
- 纯数学：达到支撑上述专业模块的 L3，不以数学研究为目标。

---

## 5. 学习方式

### 5.1 专业模块反推基础

不采用：

```text
先完整学高数
→ 再完整学线代
→ 再完整学概率
→ 最后才碰机器人
```

采用：

```text
确定专业模块
→ 反推该模块真正需要的基础
→ 先学这些基础
→ 进入专业理论
→ 用真实系统 / 源码建立联系
→ 暴露新基础缺口时定点补课
```

但“按需学习”不等于降低数学要求：最终关键基础深度仍需达到机器人硕士核心课程的可用水平。

### 5.2 理论优先

正常学习日每天约 **2–3 小时**，主要用于：

- 理论；
- 数学；
- 公式推导；
- 算法机制；
- 源码阅读；
- 真实案例映射；
- 闭卷题与复测。

不要求每天写代码，也不要求每天做实验。

已有的测试、ROS2、Nav2、真实机器人经验应作为理论理解的案例资产，而不是重复做低价值实操。

### 5.3 实验按必要性安排

只有当以下情况出现时才单独安排 LAB / PROJECT：

- 理论理解必须通过运行结果验证；
- 必须亲手实现才能建立关键直觉；
- 新方向缺少既有实操经验；
- 需要形成综合能力闭环。

实验不塞进普通理论 Day，也不为了课程形式强行制造实验。

---

## 6. 源码学习原则

对于已有真实工程实现的模块，特别是 Navigation / Control：

```text
真实问题
→ 公司真实实现
→ 反推缺失基础
→ 学数学 / 算法本体
→ 官方标准实现
→ 必要时最小自实现
→ 回到真实系统验证
```

默认不要求横向刷第三、第四套代码。

对于 Robot Learning / VLM / VLA 等没有公司实现的方向：

```text
论文 / 官方项目
→ 理论
→ 官方实现
→ 必要的最小修改或复现
→ 训练 / 推理 / 系统验证
```

---

## 7. 不采用的学习方式

- 不为了“完整”先学大量暂时无挂点的基础课；
- 不直接跳过底层机器人理论去学 VLA；
- 不把会调用 API 当作掌握；
- 不把看完代码当作理解算法；
- 不把每天做实验当作学习质量；
- 不用固定 Day 数强迫推进关键前置未掌握的内容；
- 不为了源码数量横向刷大量实现；
- 不以 SOTA 刷榜、纯大模型预训练基础设施、芯片 / PCB 研发为当前主线。

---

## 8. 最终综合能力

课程完成后应能够回答并处理：

1. 一台新机器人如何从 Sensor / Actuator 搭建到完整软件栈？
2. 一个传感器观测为什么可信或不可信？
3. 像素 / 点云怎样转成机器人可使用的世界表示？
4. EKF / LIO / VIO 为什么会漂、跳、退化？
5. Planner 为什么选择某条路径？怎样改变其决策原则？
6. Controller 为什么输出某个动作？cost / constraint / horizon 改变意味着什么？
7. 机械臂如何从目标位姿变成 joint / torque action？
8. Robot Policy 如何从 demonstrations 学出来？
9. ACT / Diffusion Policy / VLA 的 state 和 action 分别如何表示？
10. VLA 与传统 Skill / Planner / Controller 应如何划分责任？
11. 机器人失败时，如何快速建立跨模块故障传播链？
12. 如何建立数据采集、部署、评测、failure mining、再训练闭环？

---

## 9. 最终毕业项目

必须完成一个接近机器人硕士 Capstone / Dissertation 强度的综合项目，理想形态为：

```text
自然语言任务
→ 场景理解
→ VLM / VLA / Task & Skill Planning
→ Navigation
→ Object Localization
→ Manipulation
→ Control
→ Execution Feedback
→ Recovery
→ Evaluation
→ Failure Data Loop
```

毕业要求包含：

- 明确的问题定义；
- baseline；
- 方法与系统设计；
- 可量化指标；
- 对照 / ablation；
- 失败分析；
- 可复现配置；
- 技术报告。

完成这些要求，才视为真正形成“机器人硕士理论底座 + 全栈工程 + 具身智能”的能力结构。