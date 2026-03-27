# S01 Day 5｜Sprint 1 收口总结与闭卷作答

## 1. 今日定位

今天不是继续扩展新知识点，而是完成 Sprint 1 的阶段收口工作。  
核心目标有三项：

1. 回收并整理 Day 1 到 Day 4 已形成的结论；
2. 对 `NavigateToPose -> motion` 主链进行闭卷口述与修正；
3. 明确 Sprint 1 已解决的问题，以及交接给 Sprint 2 的问题边界。

---

## 2. Day 1 - Day 4 收口结果

### 2.1 Day 1：先建立主链骨架

Day 1 的任务不是把所有细节一次性讲透，而是先建立第一版问题地图。  
当时形成的核心认识是：

- `/navigate_to_pose` 是导航任务入口；
- `bt_navigator` 是流程组织核心；
- `planner_server`、`controller_server`、`behavior_server` 属于能力执行层；
- `map`、`costmap`、`TF`、`localization` 属于支撑层；
- 需要后续通过运行时证据和源码继续修正主链。

Day 1 的价值，在于先把“谁是主干、谁是支撑、谁负责调度”这个分析框架搭起来，而不是停留在“会点 RViz 发点”的层面。

### 2.2 Day 2：用运行时证据修正主链

Day 2 的重点，是把“想当然的主链”改成“被运行时证据支撑的主链”。  
当日最重要的修正包括：

- `/navigate_to_pose` 的 action server 是 `bt_navigator`；
- 控制输出链不是简单的 `controller_server -> /cmd_vel`；
- 实际链路为：

    `controller_server -> /cmd_vel_nav -> velocity_smoother -> /cmd_vel -> 底盘驱动`

- `behavior_server` 不是固定主干中的每一步，而是在特定条件下参与恢复动作；
- `map`、`amcl_pose`、`costmap`、`TF` 等对象必须作为支撑层单独理解。

Day 2 解决的是“主链口述过于粗糙”的问题，使主链从概念图进入到运行时证据图。

### 2.3 Day 3：对齐 BT 视角与源码视角

Day 3 的重点，是区分行为树节点与 ROS 节点，避免层级混乱。  
形成的关键认识包括：

- `bt_navigator` 不是行为树本体，但它是行为树的执行宿主；
- 默认导航流程由默认 BT XML 承载；
- `ComputePathToPose`、`FollowPath` 属于行为树中的 BT action node；
- 它们不是 ROS 图中的普通 node；
- `planner_server`、`controller_server` 才是被这些 BT 节点调用的 ROS 侧执行对象；
- 路径结果通过 blackboard 在 BT 节点之间传递。

Day 3 的核心成果，是把“行为树节点”和“ROS 运行节点”这两层正式拆开。

### 2.4 Day 4：验证默认 BT 不是线性死流程

Day 4 的重点，是验证默认 BT 的运行结构。  
当日形成的关键认识是：

- 默认 BT 不是固定线性流程；
- 正常 case 主要走规划与跟踪主流程；
- 更难 case 或受阻 case 更容易进入恢复逻辑；
- 恢复逻辑包含 `Spin`、`BackUp`、`Wait`、`ClearCostmap` 等动作；
- `behavior_server` 更像条件介入的恢复执行层，而不是主干固定一步。

Day 4 解决的是“为什么有时会触发恢复、有时不会”的结构理解问题。

---

## 3. Sprint 1 当前已经钉住的结论

截至 Day 5，Sprint 1 已经可以稳定口述并解释以下结论：

1. `/navigate_to_pose` 是导航任务入口，`bt_navigator` 是其 action server；
2. `bt_navigator` 接收 goal 后，会加载并执行默认行为树；
3. 行为树中的 `ComputePathToPose` 负责触发规划请求；
4. `planner_server` 负责接收规划请求并调用 planner plugin 生成路径；
5. 路径结果会写入 blackboard，供后续控制阶段读取；
6. 行为树中的 `FollowPath` 负责触发控制请求；
7. `controller_server` 调用 controller plugin 生成 `/cmd_vel_nav`；
8. `velocity_smoother` 对速度进行平滑后发布 `/cmd_vel`；
9. 底盘驱动节点订阅 `/cmd_vel` 后，机器人开始运动；
10. `behavior_server` 主要在恢复逻辑被触发时介入；
11. `map`、`costmap`、`TF`、`localization` 是持续提供约束和状态的支撑层，不应直接混入主链箭头中。

---

## 4. Sprint 1 当前最稳的一版主链口述

`NavigateToPose` 的 goal 发出后，`bt_navigator` 作为 `/navigate_to_pose` 的 action server 接收任务，并执行默认行为树。行为树先 tick 到 `ComputePathToPose`，该 BT action node 作为 action client 向规划侧发起请求；`planner_server` 接收请求后，根据配置选择对应的 planner plugin 计算路径，并将结果写入 blackboard 中的 `{path}`。随后行为树 tick 到 `FollowPath`，它从 blackboard 读取 `{path}`，再作为 action client 调用 `controller_server`；`controller_server` 根据路径和 controller plugin 持续生成速度命令，输出到 `/cmd_vel_nav`。然后 `velocity_smoother` 对速度进行平滑处理并发布到 `/cmd_vel`，最后由底盘驱动节点订阅 `/cmd_vel`，机器人开始运动。

---

## 5. Sprint 1 已有证据支撑的内容

当前已经通过运行时观察和前序整理，得到较稳的证据链：

### 5.1 任务入口证据

- `/navigate_to_pose` 不是普通 topic，而是 action 接口；
- `bt_navigator` 是该 action 的 server，因此导航任务不是直接交给 `planner_server` 或 `controller_server`。

### 5.2 核心运行对象证据

- `bt_navigator`、`planner_server`、`controller_server`、`behavior_server` 等核心对象真实存在于系统中；
- 这些对象在正常 bringup 后处于工作状态。

### 5.3 控制输出链证据

- 真实控制链中存在 `/cmd_vel_nav`；
- `velocity_smoother` 不应被省略；
- 最终到底盘的是 `/cmd_vel`，而不是直接把 `controller_server` 简化为“直接发 `/cmd_vel`”。

### 5.4 支撑层证据

- 地图、定位、TF、costmap 等对象在导航执行中持续起作用；
- 它们不是“偶尔用一下”的附属品，而是规划与控制能够成立的必要支撑条件。

### 5.5 恢复逻辑证据

- 默认 BT 不是固定直线流程；
- 在困难 case 中，更容易触发恢复节点；
- `behavior_server` 的参与具有条件性，而非主干固定步骤。

---

## 6. 当前仍不够稳的点

虽然 Sprint 1 已经完成主链收口，但以下问题仍然没有完全拆透：

1. BT tick、Action 调用、Lifecycle 管理之间的更细粒度调度关系；
2. `planner_server`、`controller_server` 与具体 plugin 的加载、映射和调用细节；
3. blackboard 中关键变量在默认 BT 中的更完整流转过程；
4. 恢复逻辑在不同失败条件下的触发分支与回退机制；
5. 支撑层状态变化如何具体影响主流程执行。

这些问题不属于 Sprint 1 的主要验收范围，但属于 Sprint 2 需要继续解决的内容。

---

## 7. 闭卷题目与作答

## 7.1 题目 1
### 题目
从 `NavigateToPose` 到机器人运动，把整条主链完整口述一遍。

### 我的原始回答
Navigate to Pose这个action发出goal了之后,BTNavigator作为action server接收到goal,然后调用默认行为数Navigate to Pose W Replanning and Recovery,然后行为数tick到compute path to pose,然后这个BTActionNode作为action client向planner server发送请求,planner server调用planner plugin,算出path,然后写入blackboard,然后行为数tick到controller server,controller server调用controller plugin,计算出cmd vel nav,然后传入velocity smoother,输出cmd vel,然后由turtlebot3diff drive接收到这个数据驱动底盘,机器人开始运动。

### 修正后的标准回答
`NavigateToPose` goal 发出后，`bt_navigator` 作为 `/navigate_to_pose` 的 action server 接收任务，并执行默认行为树。行为树先 tick 到 `ComputePathToPose`，该 BT action node 作为 action client 向规划侧发起请求；`planner_server` 接收请求后，根据配置选择对应 planner plugin 计算路径，并将结果写入 blackboard 中的 `{path}`。随后行为树 tick 到 `FollowPath`，它从 blackboard 读取 `{path}`，再作为 action client 调用 `controller_server`；`controller_server` 根据路径和 controller plugin 持续生成速度命令，输出到 `/cmd_vel_nav`。然后 `velocity_smoother` 对速度进行平滑处理并发布到 `/cmd_vel`，最后由底盘驱动节点订阅 `/cmd_vel`，机器人开始运动。

---

## 7.2 题目 2
### 题目
为什么 `/navigate_to_pose` 不是普通节点名，而是 action 入口？

### 我的原始回答
因为是它接受到goal，并将信息传入bt_navigator，至此，整个导航开始启动，所以说是入口，卧槽你的

### 修正后的标准回答
`/navigate_to_pose` 之所以是导航任务入口，是因为它不是普通 topic 或普通节点名，而是一个 action 接口；而实际承接这个 action 的 server 是 `bt_navigator`。也就是说，用户或 RViz 发出的导航 goal，是通过 `/navigate_to_pose` 这个 action 进入 Nav2 系统，并由 `bt_navigator` 接手后续流程组织。

---

## 7.3 题目 3
### 题目
为什么真正接住导航任务的是 `bt_navigator`，不是 `planner_server`？

### 我的原始回答
因为bt_navigator调用行为树，然后启动后面一系列的规控任务，但是planner_server只是进行全局规划，并不能在需要做出速度寻迹或者受阻等等行为管理。

### 修正后的标准回答
真正接住导航任务的是 `bt_navigator`，因为它实现的是 `NavigateToPose` 这类任务接口，并通过行为树组织整个导航流程，包括规划、控制、恢复等阶段。`planner_server` 只负责处理规划请求和调用 planner plugin 计算路径，它是被流程调用的能力提供者，而不是整个导航任务的总调度者。

---

## 7.4 题目 4
### 题目
`bt_navigator` 和行为树本体是什么关系？

### 我的原始回答
bt_navigator不是行为树本体，但是它能决定调用哪个行为树，没有行为树就调用默认的

### 修正后的标准回答
`bt_navigator` 不是行为树本体，但它是行为树的执行宿主和流程组织者。它接收导航任务后，装载并执行相应的 BT XML；如果没有特别指定，就使用默认的导航行为树。行为树中的 BT plugins 作为 XML 节点被注册并由 `bt_navigator` 处理和 tick。

---

## 7.5 题目 5
### 题目
`ComputePathToPose` 和 `FollowPath` 各自处在哪一层，它们为什么不是 ROS node？

### 我的原始回答
他们属于行为树下面的BT node，ComputePathToPose在规划层，调用planner server，另外一个在执行层，调用controller server。他们属于行为树的node，但是不属于ros node

### 修正后的标准回答
`ComputePathToPose` 和 `FollowPath` 都是行为树中的 BT action 节点，不是 ROS 图中的 ROS node。`ComputePathToPose` 处在规划请求这一段，作为 action client 去请求规划侧接口；`FollowPath` 处在路径执行这一段，作为 action client 去请求控制侧接口。它们属于“行为树中的流程节点”，而 `planner_server`、`controller_server` 才是 ROS 系统中的实际运行节点。

---

## 7.6 题目 6
### 题目
`planner_server` 和 planner plugin 的职责边界是什么？

### 我的原始回答
planner_server负责当前位置和目标点，接收global_costmap，然后决定调用哪个planner plugin并传入收到的信息，然后planner plugin根据planner server提供的这些信息计算{path}

### 修正后的标准回答
`planner_server` 的职责是处理规划请求，并托管一组 planner plugins；它根据配置中的 planner 名称映射选择相应的 planner plugin 来计算路径，同时它还托管 global costmap，作为规划算法所依赖的环境表示。真正执行路径搜索和路径生成的是具体 planner plugin，而不是 `planner_server` 自己手写所有规划算法。

---

## 7.7 题目 7
### 题目
`controller_server` 和 controller plugin 的职责边界是什么？

### 我的原始回答
前者接受当前位姿和{path}还有local_costmap，然后决定调用哪个plugin并传入收到的信息，后者根据前者提供的信息计算/cmd_vel_nav

### 修正后的标准回答
`controller_server` 的职责是处理路径跟踪控制请求，并托管 controller plugins、progress checker 和 goal checker 的映射；它根据传入的路径和对应插件名选择合适的 controller plugin，持续生成速度命令。同时它还托管 local costmap，作为局部控制与避障的重要环境支撑。真正把路径转成控制输出的是具体 controller plugin，而不是 `controller_server` 把所有控制算法写死在一个节点里。

---

## 7.8 题目 8
### 题目
为什么 Day 2 要把控制链修正成 `/cmd_vel_nav -> velocity_smoother -> /cmd_vel`？

### 我的原始回答
因为这才是真正的底层导航链，以前都是说controller server输出/cmd_vel其实太简陋了，中间还有velocity_smother

### 修正后的标准回答
之所以要把控制链修正为 `controller_server -> /cmd_vel_nav -> velocity_smoother -> /cmd_vel`，是因为这才是当前系统里被运行时证据支撑的真实输出链。之前把 `controller_server` 直接说成输出 `/cmd_vel`，是过度简化。实际上 `velocity_smoother` 是一个独立节点，它订阅 Nav2 输出的速度命令并进行平滑处理，然后再把结果发布到底盘使用的 `/cmd_vel`。

---

## 7.9 题目 9
### 题目
`behavior_server` 为什么不能粗暴画成每次导航必经的固定主干？

### 我的原始回答
因为当小车顺利到底其实不会触发behavior_server

### 修正后的标准回答
`behavior_server` 不是每次导航都必经的固定主干步骤，因为默认 BT 的主流程主要是“规划 + 跟踪”；只有当主流程受阻、失败或进入恢复分支时，恢复相关节点才更容易被 tick 到，此时 `behavior_server` 才按条件参与执行某些恢复动作。因此它更像“条件介入的恢复执行层”，而不是正常主线中的固定一环。

---

## 7.10 题目 10
### 题目
`costmap / TF / map / localization` 为什么要放支撑层，而不是主干箭头上？

### 我的原始回答
因为这些不是导航主链，只是在导航某个节点需要的时候接受信息

### 修正后的标准回答
`costmap`、`TF`、`map`、`localization` 属于支撑层，是因为它们持续为主执行链提供环境表示、坐标变换、地图信息和位姿状态；规划、控制和恢复都依赖这些支撑对象才能正常工作。但它们本身并不承担“接收导航任务、组织流程、调用规划、发出控制命令”这条主干职责，因此应放在支撑层，而不是主链箭头上。

---

## 7.11 题目 11
### 题目
哪些结论已经有运行时证据支撑，分别是什么证据？

### 我的原始回答
node里面有导航链的所有node，topic有/cmd_vel_nav等输入输出的信息，还有导航只有在受阻时，行为树才调用behavior server，并且能看到spinup，clearcostmap，backup，wait等信息

### 修正后的标准回答
目前已有证据支撑的结论主要包括：

1. `/navigate_to_pose` 是 action 入口，而它的 action server 是 `bt_navigator`；
2. `bt_navigator`、`planner_server`、`controller_server`、`behavior_server` 等核心对象真实存在于运行系统中，并且处于工作状态；
3. 真实控制输出链中存在 `/cmd_vel_nav` 和 `/cmd_vel` 两段，中间有 `velocity_smoother` 参与；
4. `/map`、定位相关对象、global/local costmap 等支撑层对象真实存在，因此支撑层划分是有运行时依据的；
5. 默认 BT 在更难 case 下更容易触发恢复逻辑，因此它不是线性死流程，而是主流程加条件恢复逻辑。

---

## 7.12 题目 12
### 题目
Sprint 1 已经解决了什么，没解决什么，为什么 Sprint 2 该继续往下拆？

### 我的原始回答
解决了真正的导航主链是什么，整个导航的底层逻辑是怎么样的，并且由哪些topic和nodes支承。s2需要更深入地去拆分导航主链，去剖析每个导航的node、topic到底输入输出的是什么，这些生命周期节点是怎么被管理的

### 修正后的标准回答
Sprint 1 已经解决的问题是：我现在能够从 `NavigateToPose` 一路讲到控制输出，知道谁是任务入口、谁是流程组织者、谁负责规划、谁负责控制、恢复行为大致处在什么位置，以及支撑层与主执行层应如何区分；同时，这条主链已经有一部分运行证据和文档证据支撑。  
Sprint 2 之所以要继续往下拆，是因为我虽然已经拿到了 `goal -> motion` 的第一版主链，但还没有真正拆开“流程是谁在调度、状态是谁在管理、BT / Action / Lifecycle 是如何组织这条链”的问题。这些问题正是 Sprint 2 的继续任务。

---

## 8. 今日暴露出来的主要薄弱点

今天的闭卷与修正，暴露出以下几个最容易混淆的点：

### 8.1 容易把 BT node 和 ROS node 混在一起

最典型的问题，是把“行为树 tick 到 `FollowPath`”说成“行为树 tick 到 `controller_server`”。  
这会导致层级混乱。  
正确口径应当是：

- `ComputePathToPose`、`FollowPath` 是 BT action node；
- `planner_server`、`controller_server` 是 ROS 侧执行对象；
- BT 节点通过 action client 方式调用这些 server。

### 8.2 容易把 action 接口和 action server 分开说乱

`/navigate_to_pose` 不是“接到 goal 再传给 bt_navigator 的另一个东西”。  
更准确地说：

- `/navigate_to_pose` 是 action 接口；
- `bt_navigator` 是该 action 的 server；
- 所以导航任务本质上就是由 `bt_navigator` 接住。

### 8.3 容易把支撑层说得太被动

支撑层不是“某个节点需要时才去接信息”。  
更准确地说：

- 支撑层持续提供环境、位姿、地图和坐标变换约束；
- 主链执行时始终依赖它们；
- 只是它们不承担任务总调度职责。

### 8.4 容易把 server 和 plugin 的职责边界说粗

`planner_server`、`controller_server` 都不是“自己实现所有算法”。  
更准确地说：

- server 负责承接请求、组织调用、托管映射和相关环境对象；
- plugin 负责执行具体算法。

---

## 9. Sprint 2 交接说明

Sprint 1 到今天为止，已经完成了第一阶段目标：  
即能够稳定讲清 `goal -> motion` 的主执行链，并能区分任务入口、流程组织者、规划层、控制层、恢复层和支撑层。

但 Sprint 1 还没有完全拆开的内容包括：

- BT 的 tick 机制到底如何驱动流程前进；
- Action 是如何在 BT 节点与 server 之间建立任务调用关系的；
- Lifecycle 节点为什么能被统一 bringup、激活和管理；
- 黑板变量如何在默认行为树中传递；
- 恢复分支具体在什么条件下切入主流程。

因此，Sprint 2 的任务重点应当是：  
继续沿着 `BT / Action / Lifecycle` 这三条线，把当前已经拿到的第一版主链继续拆深，而不是重新回到“主链是什么”这个已经初步解决的问题上。

---

## 10. 今日结论

今天完成的是 Sprint 1 的正式收口，而不是继续扩张知识点。  
经过 Day 1 到 Day 5 的整理与闭卷修正，目前已经能够较稳定地回答以下问题：

- 导航任务从哪里进入系统；
- 谁负责组织导航流程；
- 规划与控制是如何被调用的；
- 真实控制输出为什么不能简化成 `controller_server -> /cmd_vel`；
- `behavior_server` 为什么不是每次必经；
- 支撑层为什么必须单独理解；
- Sprint 2 为什么要继续拆 `BT / Action / Lifecycle`。

这意味着 Sprint 1 的目标基本达成。  
后续进入 Sprint 2 时，应保持当前主链口径不变，在此基础上继续向调度机制和实现细节下钻。

---
