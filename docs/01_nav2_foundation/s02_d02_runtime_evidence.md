# S02 Day 2｜BT、Action、Lifecycle 调度骨架｜运行时证据采集

## 1. 今日定位

Sprint 2 Day 1 已经完成了调度骨架的第一版问题定义。  
今天的任务不是继续停留在概念层，而是通过运行时证据，把以下几件事正式钉住：

1. `/navigate_to_pose` 的 action server 到底是谁；
2. `bt_navigator / planner_server / controller_server / behavior_server` 是否属于 lifecycle 管理对象；
3. `bt_navigator` 当前实际加载的是哪棵默认行为树；
4. 当前默认行为树中，恢复逻辑的真实结构到底是什么；
5. `behavior_server` 在恢复链中实际承担什么职责。

---

## 2. 今日验证目标

今天围绕下面四个假设开展验证：

### 2.1 假设 1
`/navigate_to_pose` 的 action server 是 `bt_navigator`。

### 2.2 假设 2
`bt_navigator / planner_server / controller_server / behavior_server` 都属于 lifecycle 管理体系中的关键对象。

### 2.3 假设 3
`bt_navigator` 当前加载的是默认的 NavigateToPose recovery 行为树，而不是抽象意义上的“某棵 BT”。

### 2.4 假设 4
当前默认行为树中的恢复逻辑，可以明确区分为：

- 主流程节点；
- 上下文恢复节点；
- 系统级恢复子树；
- 行为恢复动作节点；
- 清图服务节点。

---

## 3. 运行时证据采集结果

## 3.1 `/navigate_to_pose` 的 action server 已确认

通过运行时查询 `/navigate_to_pose` 的 action 信息，可以确认：

- `/navigate_to_pose` 是导航任务入口；
- `/bt_navigator` 是该 action 的 server。

这意味着：

- 导航任务不是直接交给 `planner_server` 或 `controller_server`；
- 真正承接导航任务、进入流程组织阶段的对象是 `bt_navigator`。

### 当前结论
`/navigate_to_pose` 是任务入口，`bt_navigator` 是任务承接者与流程组织者。

---

## 3.2 lifecycle 管理对象已确认

通过查询 lifecycle 相关信息，可以确认：

- `bt_navigator`
- `planner_server`
- `controller_server`
- `behavior_server`

都处在 lifecycle 管理体系中，并且关键执行对象处于可工作状态。

这说明：

- lifecycle manager 不直接承担规划、控制或恢复动作执行；
- 但它确实在管理这些关键节点的状态，使其进入并维持在可工作阶段。

### 当前结论
lifecycle 不是算法执行层，但它是系统调度骨架中不可忽略的状态管理层。

---

## 3.3 当前默认 BT XML 已确认

运行时参数显示，`bt_navigator` 当前加载的默认 NavigateToPose 行为树为：

    /opt/ros/humble/share/nav2_bt_navigator/behavior_trees/navigate_to_pose_w_replanning_and_recovery.xml

这说明：

- “默认行为树”不是抽象概念；
- 当前系统确实存在一棵被 `bt_navigator` 实际装载和执行的 XML 流程定义文件；
- 后续对恢复逻辑、节点层级、父子关系的理解，必须以这棵实际 XML 为准，而不是仅凭文档印象。

### 当前结论
`bt_navigator` 通过参数装载真实存在的默认 BT XML，并以此组织主流程与恢复流程。

---

## 4. 当前默认树中的真实恢复结构

今天最重要的收获，是把当前系统里的恢复结构从“印象理解”修正为“运行时对应的真实结构”。

## 4.1 主流程仍然成立

当前默认树中的主流程仍然可以概括为：

`/navigate_to_pose`
→ `bt_navigator`
→ 默认 BT XML
→ `ComputePathToPose`
→ `planner_server`
→ 生成路径并写入 blackboard
→ `FollowPath`
→ `controller_server`
→ 输出控制命令
→ 机器人运动

这部分与 Sprint 1 形成的主链认知是一致的。

---

## 4.2 规划失败时的上下文恢复

当前默认树中，若失败发生在 `ComputePathToPose` 这一段，则不会直接进入系统级恢复。

更准确地说：

- `ComputePathToPose` 外层有对应的局部 `RecoveryNode`；
- 若规划失败，则会触发清理 **global costmap** 的上下文恢复动作；
- 清理完成后，再重新尝试规划。

### 当前修正
今天最关键的一点是：  
在当前 Humble 默认加载的这棵 XML 中，并没有出现 `WouldAPlannerRecoveryHelp` 这个判断节点。  
也就是说，当前系统里的规划侧上下文恢复，不是“先通过 `WouldAPlannerRecoveryHelp` 判断，再决定是否清图”，而是通过 **局部 `RecoveryNode + ClearEntireCostmap(name="ClearGlobalCostmap-Context")`** 的方式直接组织的。

---

## 4.3 控制失败时的上下文恢复

如果失败发生在 `FollowPath` 这一段，当前默认树同样会先进入局部恢复，而不是立刻进入系统级恢复。

更准确地说：

- `FollowPath` 外层也存在对应的局部 `RecoveryNode`；
- 若跟踪失败，则会触发清理 **local costmap** 的上下文恢复动作；
- 清理完成后，再重新尝试路径跟踪。

### 当前修正
与规划侧一致，当前实际 XML 中也没有出现 `WouldAControllerRecoveryHelp`。  
因此，当前系统里的控制侧上下文恢复，同样不是“先通过 `WouldAControllerRecoveryHelp` 判断再清图”，而是通过 **局部 `RecoveryNode + ClearEntireCostmap(name="ClearLocalCostmap-Context")`** 的结构直接实现的。

---

## 4.4 上下文恢复无效后进入系统级恢复

如果规划侧和控制侧的上下文恢复都未能解决问题，默认树才会进入系统级 recovery subtree。

当前实际 XML 中，可以明确识别到系统级恢复子树中包含：

- `RoundRobin`
- `Spin`
- `Wait`
- `BackUp`
- `ClearingActions`
  - `ClearLocalCostmap-Subtree`
  - `ClearGlobalCostmap-Subtree`

这说明当前系统的系统级恢复结构并不是“失败后立刻随机恢复”，而是：

1. 先做规划侧或控制侧的上下文恢复；
2. 上下文恢复无效后，进入系统级恢复子树；
3. 在系统级恢复子树中，由 `RoundRobin` 组织多个恢复动作轮流尝试；
4. 每完成一个恢复动作，再重新尝试主导航流程；
5. 若多轮恢复后仍然失败，则任务最终进入失败或 abort。

---

## 5. `behavior_server` 的真实职责

今天还通过运行时信息确认了 `behavior_server` 的角色。

## 5.1 `behavior_server` 执行哪些恢复行为

运行时查询表明，`behavior_server` 侧实际承载了以下行为动作：

- `/spin`
- `/wait`
- `/backup`

同时，参数中也能对应看到：

- `spin.plugin`
- `wait.plugin`
- `backup.plugin`

这说明：

- `Spin`
- `Wait`
- `BackUp`

这些恢复动作并不是 XML 自己“直接执行”的，  
而是由行为树中的 BT action node 发起请求，再由 `behavior_server` 侧的行为插件真正执行。

## 5.2 什么不属于 `behavior_server` 的职责

今天也进一步确认：

- `ClearGlobalCostmap`
- `ClearLocalCostmap`
- `ClearEntireCostmap`

这类节点不属于 `behavior_server` 执行的行为恢复动作。  
它们本质上是清图服务调用节点，不走 `behavior_server`。

### 当前结论
必须明确区分：

- **行为恢复动作**：`Spin / Wait / BackUp`，走 `behavior_server`
- **清图恢复动作**：`Clear*Costmap`，走清图服务，不走 `behavior_server`

---

## 6. 今日最重要的修正结论

今天最大的进展，不是又多记住了几个节点名，而是通过运行时证据修正了一个先前理解偏差：

> 当前 Humble 默认加载的 `navigate_to_pose_w_replanning_and_recovery.xml` 中，并没有 `WouldAPlannerRecoveryHelp / WouldAControllerRecoveryHelp` 这两个条件节点；当前系统中的上下文恢复，是通过局部 `RecoveryNode + ClearEntireCostmap` 直接实现的。

这意味着：

- 之前基于部分文档形成的“WouldA* 先判断，再清图”的理解，不能直接套用到当前机器上的实际默认树；
- 对当前系统进行分析时，必须以运行时参数和实际 XML 为准，而不是机械照搬概念版本的恢复树结构。

---

## 7. 基于今日证据的当前口述版本

截至今天，可以将当前系统的导航与恢复链稳定口述为：

导航任务从 `/navigate_to_pose` 进入系统，由 `bt_navigator` 作为 action server 接住，并加载默认的 `navigate_to_pose_w_replanning_and_recovery.xml`。  
在主流程中，行为树先 tick 到 `ComputePathToPose` 去请求 `planner_server` 生成路径，再 tick 到 `FollowPath` 去请求 `controller_server` 执行路径跟踪。  
如果规划失败，当前默认树会在 `ComputePathToPose` 外层的局部 `RecoveryNode` 中调用 `ClearGlobalCostmap-Context`，然后重新尝试规划；如果路径跟踪失败，则会在 `FollowPath` 外层的局部 `RecoveryNode` 中调用 `ClearLocalCostmap-Context`，然后重新尝试跟踪。  
若这些上下文恢复仍然无效，则进入系统级 recovery subtree，由 `RoundRobin` 轮流尝试 `ClearingActions`、`Spin`、`Wait`、`BackUp`。  
其中，`Spin / Wait / BackUp` 通过 `behavior_server` 的 action server 真正执行，而 clear costmap 不经过 `behavior_server`。

---

## 8. 今日收口

今天通过运行时证据，已经正式钉住了以下五件事：

1. `/navigate_to_pose` 的 action server 是 `bt_navigator`；
2. `bt_navigator / planner_server / controller_server / behavior_server` 都处在 lifecycle 管理体系中；
3. `bt_navigator` 当前实际加载的默认树是 `navigate_to_pose_w_replanning_and_recovery.xml`；
4. 当前默认树中的上下文恢复，是通过局部 `RecoveryNode + ClearEntireCostmap` 直接组织的；
5. `behavior_server` 负责执行 `Spin / Wait / BackUp` 这类行为恢复动作，而 clear costmap 不归它执行。

这意味着 Sprint 2 Day 2 的目标已经达成：  
昨天建立的是调度骨架，今天则通过运行时证据，把“谁接任务、谁管状态、谁组织恢复、谁真正执行恢复动作”正式压实。

---

## 9. 明天继续拆的方向

Sprint 2 Day 3 应继续往下拆：

1. `bt_navigator` 如何在实现层面装载默认 BT XML；
2. `ComputePathToPose / FollowPath / Spin / Wait / BackUp` 这些 BT 节点如何在实现层面作为 client 工作；
3. `RecoveryNode` 与 `RoundRobin` 在源码/实现层面分别承担什么控制逻辑；
4. `lifecycle manager` 如何统一 bringup 和管理这些关键节点。

到这一步，Sprint 2 才会从“运行时证据层”继续进入“实现与源码层”。

---