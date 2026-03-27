# S02 Day 1｜BT、Action、Lifecycle 调度骨架｜问题定义与图谱建立

## 1. 今日定位

Sprint 1 解决的是：`goal -> motion` 这条主执行链到底是什么。  
Sprint 2 开始解决的是：这条主链到底是谁组织起来的，失败后又是怎么被恢复逻辑重新拉回主流程的。

因此，今天的重点不是继续解释 planner、controller、costmap 的一般概念，而是先把下面三层关系立住：

- 谁接任务；
- 谁组织流程；
- 谁执行恢复动作与状态管理。

---

## 2. 今日核心问题

今天要回答的核心问题有四个：

1. `/navigate_to_pose` 进入系统后，为什么是 `bt_navigator` 接住任务，而不是 `planner_server`？
2. `bt_navigator`、BT XML、BT node、ROS server 之间到底是什么层级关系？
3. 导航主流程失败后，行为树如何先做上下文恢复，再进入系统级恢复？
4. `behavior_server`、`Spin / Wait / BackUp`、`ClearCostmap`、`RecoveryNode`、`RoundRobin` 之间分别负责什么？

---

## 3. 一级对象分层（当前版本）

### 3.1 任务入口层
- `/navigate_to_pose`

### 3.2 调度组织层
- `bt_navigator`
- `BT XML`

### 3.3 行为树内部节点层
- `ComputePathToPose`
- `FollowPath`
- `RecoveryNode`
- `RoundRobin`
- `WouldAPlannerRecoveryHelp`
- `WouldAControllerRecoveryHelp`
- `Spin`
- `Wait`
- `BackUp`
- `ClearGlobalCostmap`
- `ClearLocalCostmap`
- `ClearEntireCostmap`

### 3.4 ROS 侧执行层
- `planner_server`
- `controller_server`
- `behavior_server`

### 3.5 状态管理层
- `lifecycle manager`

---

## 4. 当前第一版总骨架

当前第一版骨架可以概括为：

`NavigateToPose goal`
→ `/navigate_to_pose`
→ `bt_navigator`
→ 加载并执行 `BT XML`
→ 树内节点按 tick 推进流程
→ 主流程节点调用 ROS 侧执行对象
→ 失败时进入恢复相关节点
→ 恢复成功后再回主流程

其中：

- `bt_navigator` 是任务承接者与流程组织者；
- `BT XML` 是流程定义，不是运行节点；
- `ComputePathToPose`、`FollowPath`、`Spin`、`Wait`、`BackUp` 等，是行为树中的 BT 节点；
- `planner_server`、`controller_server`、`behavior_server` 才是 ROS 侧真正执行规划、控制和行为恢复的 server。

---

## 5. 主流程与受阻后的完整链路

### 5.1 正常主流程

正常情况下，导航任务从 `/navigate_to_pose` 进入系统，由 `bt_navigator` 作为 action server 接住。  
`bt_navigator` 加载默认行为树后，先 tick 到 `ComputePathToPose`，由该 BT action node 发起对 `planner_server` 的规划请求。路径生成后写入 blackboard，随后行为树 tick 到 `FollowPath`，由它发起对 `controller_server` 的路径跟踪请求。控制命令输出后，经速度平滑与底盘驱动，机器人开始运动。

### 5.2 如果规划失败

如果失败发生在 `ComputePathToPose` 这一段，并不意味着立刻进入系统级恢复。  
行为树会先进入 **规划侧的上下文恢复分支**：

- `WouldAPlannerRecoveryHelp` 先判断：这次规划失败是否值得做 planner recovery；
- 如果值得，则调用 `ClearGlobalCostmap` 这类清图服务节点；
- 清完 global costmap 后，重新尝试规划。

这里要注意：

- `WouldAPlannerRecoveryHelp` 是条件节点，负责判断；
- `ClearGlobalCostmap` 是清图服务节点，不属于 `behavior_server` 的行为动作。

### 5.3 如果控制/跟踪失败

如果失败发生在 `FollowPath` 这一段，则行为树进入 **控制侧的上下文恢复分支**：

- `WouldAControllerRecoveryHelp` 先判断：这次控制失败是否值得做 controller recovery；
- 如果值得，则调用 `ClearLocalCostmap` 这类清图服务节点；
- 清完 local costmap 后，重新尝试路径跟踪。

这里同样要注意：

- `WouldAControllerRecoveryHelp` 是条件节点；
- `ClearLocalCostmap` 也是清图服务节点，不走 `behavior_server`。

### 5.4 如果上下文恢复仍然无效

如果规划侧和控制侧的上下文恢复都无法解决问题，行为树才会进入 **系统级 recovery subtree**。

这个 recovery subtree 的核心不是“直接乱试动作”，而是由两个控制结构节点来组织：

- `RecoveryNode`：负责“失败后先恢复，再回主流程重试”；
- `RoundRobin`：负责将多个恢复动作轮流尝试，而不是每次只死磕一个动作。

在这个 recovery subtree 中，常见的系统级恢复动作包括：

- `Spin`
- `Wait`
- `BackUp`
- `ClearEntireCostmap` 或系统级 clear costmap

每完成一个恢复动作，行为树都会重新尝试主导航流程；如果还是失败，再试下一个恢复动作。若多轮恢复后仍然失败，任务最终会进入失败或 abort。

---

## 6. `behavior_server` 与恢复节点的关系

这是今天最容易混淆的点。

### 6.1 什么会调用 `behavior_server`

会调用 `behavior_server` 的，主要是行为恢复类 BT action nodes，例如：

- `Spin`
- `Wait`
- `BackUp`

它们在行为树中属于 BT action node，在 ROS 侧会通过对应行为接口调用 `behavior_server` 去真正执行恢复动作。

### 6.2 什么不会调用 `behavior_server`

下面这些不属于 `behavior_server` 执行的行为恢复动作：

- `WouldAPlannerRecoveryHelp`
- `WouldAControllerRecoveryHelp`
- `ClearGlobalCostmap`
- `ClearLocalCostmap`
- `ClearEntireCostmap`

原因分别是：

- `WouldA*RecoveryHelp` 是条件判断节点；
- `Clear*Costmap` 是清图服务节点，走的是 clear costmap service，不是行为动作接口。

### 6.3 关键结论

所以这里必须明确分开：

- **树负责安排恢复顺序；**
- **条件节点负责判断值不值得恢复；**
- **行为动作节点负责发起恢复动作请求；**
- **`behavior_server` 负责真正执行 `Spin / Wait / BackUp` 这类动作；**
- **clear costmap 不走 `behavior_server`。**

---

## 7. 今天最重要的易混边界

### 7.1 `/navigate_to_pose` vs `bt_navigator`
- `/navigate_to_pose` 是 action 接口；
- `bt_navigator` 是该 action 的 server。

### 7.2 `bt_navigator` vs `BT XML`
- `bt_navigator` 是行为树的执行宿主与流程组织者；
- `BT XML` 是流程定义文件，不是运行节点。

### 7.3 BT node vs ROS node
- `ComputePathToPose`、`FollowPath`、`Spin`、`Wait`、`BackUp` 是 BT node；
- `planner_server`、`controller_server`、`behavior_server` 是 ROS 侧执行对象。

### 7.4 `WouldA*RecoveryHelp` vs `RecoveryNode / RoundRobin`
- `WouldA*RecoveryHelp` 负责判断；
- `RecoveryNode / RoundRobin` 负责组织恢复结构与恢复顺序。

### 7.5 behavior 恢复 vs clear costmap
- `Spin / Wait / BackUp` 属于行为恢复，会调用 `behavior_server`；
- `Clear*Costmap` 属于清图服务调用，不走 `behavior_server`。

---

## 8. 今日一句话结论

今天最重要的收获不是又记住了几个节点名字，而是把恢复逻辑真正拆成了三层：

- **先走主流程；**
- **主流程失败后先做对应的上下文恢复；**
- **上下文恢复不行，再进入系统级 recovery subtree。**

同时还要记住一句最容易考也最容易写错的话：

**不是所有恢复都走 `behavior_server`；只有 `Spin / Wait / BackUp` 这类行为恢复走它，clear costmap 不走它。**

---

## 9. 明天要继续验证的点

明天继续沿着下面几个问题往下拆：

1. `bt_navigator` 在运行时如何装载默认 BT XML？
2. `ComputePathToPose` 与 `FollowPath` 在行为树实现层面到底如何作为 action client 工作？
3. `RecoveryNode` 与 `RoundRobin` 在默认 recovery tree 中的父子关系和执行节奏如何体现？
4. `lifecycle manager` 为什么不参与算法执行，但必须纳入系统调度骨架理解？

---

## 10. 今日收口

截至今天，已经可以比较稳定地区分以下层级：

- 任务入口：`/navigate_to_pose`
- 流程组织者：`bt_navigator`
- 流程定义：`BT XML`
- 树内节点：主流程节点、判断节点、恢复动作节点、控制结构节点
- ROS 执行层：`planner_server`、`controller_server`、`behavior_server`
- 状态管理层：`lifecycle manager`

这意味着 Sprint 2 Day 1 的任务已经完成：  
不是把所有源码都读透，而是先把 **BT、Action、Lifecycle 的调度骨架** 立起来，并把“受阻后如何恢复”的层次关系讲顺。