# M00 — Robot Full-stack Architecture

## Module Goal
建立完整机器人全栈与 Owner 视角，能够从物理世界到执行闭环划分模块责任、数据流、控制流与故障传播。

## Day1 — 机器人全栈系统与 Owner 视角

### 1. 今日目标
学完后必须能够：
- 从物理世界到机器人动作，画出完整信息与控制链；
- 区分 observation、state、world model、plan、trajectory、command；
- 判断机器人异常可能属于哪一层；
- 理解传统机器人架构、Robot Learning、VLA之间的关系。

### 2. 前置知识
无。

### 3. 必须教学内容

#### ① 完整机器人闭环
必须讲清：

```text
Physical World
↓
Sensor
↓
Driver
↓
ROS2 Runtime
↓
Perception
↓
State Estimation
↓
World Model
↓
Task / Behavior
↓
Planning
↓
Control
↓
Actuator
↓
Physical Motion
↓
Feedback
```

必须说明每层的输入、输出、数据含义、典型更新频率、坐标系、延迟来源、常见失败模式。

#### ② Observation ≠ State
必须区分：真实世界状态、sensor measurement、perception result、estimated robot state、world model。

必须理解：算法使用的是对现实世界的观测或估计，而不是真实世界本身。

#### ③ Planning / Control / Actuation边界
必须区分：

```text
Goal
→ Path
→ Trajectory
→ Control Command
→ Physical Motion
```

不能把规划路线和控制机器人执行路线混为一谈。

#### ④ 传统模块化机器人架构
必须解释：感知、定位、规划、控制为什么通常拆开；模块化优势；模块化延迟与接口代价；真实机器人为什么仍大量采用该架构。

#### ⑤ Robot Learning / VLA
必须建立 Classical Stack vs Learned Policy vs VLA 的初始概念；理解 VLA 可以替代部分决策/感知/策略模块，但不意味着底层状态、控制、安全、硬件全部消失；VLA action 最终仍要进入真实机器人执行接口。

#### ⑥ 故障传播
必须讲清：

```text
Sensor误差
→ State错误
→ Planner错误判断
→ Controller行为异常
```

以及：

```text
Controller输出正确
→ Actuator跟不上
→ Robot实际轨迹错误
```

#### ⑦ Owner责任边界
必须区分参数问题、数据问题、算法问题、接口问题、系统调度问题、架构问题。

### 4. 深度要求
- 完整机器人系统链：L3
- 模块功能：L2
- Owner责任判断：L3
- 具体算法内部：L1

### 5. 工程连接
必须联系真实机器人中的 LiDAR / GPS / LIO、Costmap、HPA / Planner、BT、MPPI、chassis command / feedback，并把已有工程经验映射到完整理论架构。

### 6. 明确不展开
EKF公式、SLAM优化、A*/Hybrid A*、MPPI公式、Manipulation、Transformer、VLA内部网络。

### 7. 本日考核点
1. Sensor measurement 和 robot state 有什么区别？
2. Localization 和 Perception 为什么不是一回事？
3. Path、Trajectory、Command 有什么区别？
4. 为什么 controller 正确也可能撞障碍？
5. 为什么 planner 错误可能表现成 controller 行为异常？
6. VLA 能否完全替代传统控制器？为什么？
7. 给一个机器人“不动”的现象，至少列出5类可能责任层。

### 8. Module毕业考试核心考点
- 全链路责任划分；
- 故障传播；
- Classical Stack 与 VLA 边界；
- 跨模块证据链。

## M00 Graduation Exam Specification
M00作为总纲模块，毕业考不单独追求公式计算，而要求给出1–2个完整机器人故障场景，完成：

```text
现象
→ 责任层候选
→ 数据/接口链路
→ 故障传播
→ 需要的证据
→ Owner判断
```

通过标准：能够完整划分系统责任边界，不直接凭经验猜单个节点。