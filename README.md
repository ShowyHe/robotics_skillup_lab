# robotics_skillup_lab

面向 **机器人全栈工程 + 具身智能（Embodied AI）+ VLA** 的长期学习与能力升级仓库。

最终目标不是单一 Nav2、单一视觉、单一控制或只会调用 VLA，而是形成：

> **研究生级机器人理论基础 + 真实机器人全栈工程能力 + VLA / Mobile Manipulation 具身智能能力 + 系统 Owner 能力。**

## 1. 总体能力链

```text
Sensors / Actuators
        ↓
C++ / Linux / ROS2 Runtime
        ↓
Perception / World Representation
        ↓
State Estimation / SLAM
        ↓
Planning / Navigation
        ↓
Kinematics / Dynamics / Control
        ↓
Manipulation
        ↓
Robot Learning
        ↓
VLM / VLA
        ↓
Mobile Manipulation
        ↓
Deployment / Data / Evaluation / Safety
```

## 2. 学习方式

本仓库采用 **“专业模块反推基础”**，而不是先把整套大学数学学完。

统一流程：

```text
确定专业模块
→ 反推真正需要的数学 / CS / 物理基础
→ 先补这些前置基础
→ 学专业理论
→ 联系真实系统 / 源码
→ 必要时做最小实现或独立 LAB
→ 理论考试 / 复测
```

### 理论优先

正常学习日按 **2–3 小时**设计，重点用于：

- 概念与机制；
- 数学与公式推导；
- 算法原理；
- 源码理解；
- 真实工程案例映射；
- 闭卷题与复测。

**不要求每天写代码，不要求每天做实验。**

已经具备的 ROS2、Nav2、测试、实机调试经验优先作为理论知识的真实案例，不机械重复基础实操。

只有当实验对理解或能力闭环不可替代时，才单独安排 `LAB / PROJECT`，并预留完整时间，不塞进普通理论 Day。

## 3. 源码学习原则

已有真实工程对应的模块（尤其 Navigation / Control）默认采用：

```text
真实问题
→ 公司真实实现
→ 反推算法与基础
→ 官方标准实现
→ 必要时最小自实现
→ 回到真实系统验证
```

默认不为了“看得多”横向刷第三、第四套仓库。

Robot Learning / VLA 等没有现成公司实现的方向，则以论文、官方项目和官方实现为主。

## 4. 主课程

课程当前固定为 **M00–M22**，详见 [`docs/03_MASTER_PLAN.md`](docs/03_MASTER_PLAN.md)。

涵盖：

- Robot Full-stack Architecture
- C++ / Linux / ROS2 Systems
- Mathematical Foundations I / II
- Sensors & Actuators
- Robot Simulation
- Vision Geometry / Deep Vision / 3D Perception
- State Estimation / SLAM / LIO / VIO
- Planning / Navigation
- Kinematics / Dynamics / Control
- Manipulation
- Deep Learning / Robot Learning
- VLM / VLA
- Mobile Manipulation
- Deployment / Data / Evaluation / Sim2Real
- Safety / Reliability / Owner
- Research Capstone
- Foundation Cleanup

## 5. 文档结构

```text
docs/
├── 00_GOALS.md            # 最终目标、边界、毕业能力
├── 01_COMPETENCY_MAP.md   # 完整能力树
├── 02_DEPENDENCIES.md     # 专业模块与基础依赖图
├── 03_MASTER_PLAN.md      # M00–M22、未来 Day 总索引
├── 04_MODULE_SPECS.md     # 每个 Module 的知识范围与毕业标准
├── LEARNING_RULES.md      # GPT 教学、考试、跳级、实验规则
├── PROGRESS.md            # 当前进度、薄弱点、课程设计状态
├── modules/               # 后续：每个 Module 的详细 Day 规划
├── lessons/               # 后续：每天真正使用的详细讲义
└── labs/                  # 后续：少量必要 LAB / PROJECT
```

### 后续文件职责

- `03_MASTER_PLAN.md`：总 Day 数、每个 Module 的理论 Day 数、Day1–DayN 总索引。
- `modules/`：每个 Module 怎么教，不存当天详细讲义。
- `lessons/`：真正的 Day 级讲义、题目、复测要求。
- `labs/`：只保存必要实验，不要求每天实验。
- `PROGRESS.md`：保证未来任何 GPT 都能从当前学习节点直接继续。

## 6. 掌握等级

统一采用：

- **L1：见过**
- **L2：能解释**
- **L3：能计算 / 推导**
- **L4：能实现 / Debug**
- **L5：能迁移 / 修改 / 设计**

核心专业课程至少达到 L3/L4；Navigation、Control、System Owner、未来 VLA 主方向的关键知识逐步向 L5 提升。

## 7. 最终验收

课程完成不以“看完多少 Day”为标准，而以是否能够：

1. 从数学和物理层解释核心机器人算法；
2. 从传感器一直追踪到机器人最终 action；
3. 判断真实机器人故障究竟应该由哪一层解决；
4. 读懂并修改关键官方/真实工程实现；
5. 建立感知、估计、规划、控制、Manipulation、VLA 的完整系统联系；
6. 完成一个达到机器人硕士 Capstone 强度的 Mobile Manipulation / Embodied AI 项目；
7. 建立部署、评测、失败回流和持续改进的数据闭环。

本仓库所有后续 Day 计划、讲义与项目都必须服从以上原则。