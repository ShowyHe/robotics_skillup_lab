# s01_d03_source_reading.md

# Sprint 1 Day 3｜按主链重写：文档、XML、参数与运行链对齐

## 1. 今日目标

Day 3 的任务，不是继续堆概念，也不是继续刷运行现象，而是把已经在 Day 2 观察到的主链，严格按下面这条顺序，分别在官方文档、默认 BT XML、默认参数和运行日志上对齐：

    /navigate_to_pose action
    -> /bt_navigator
    -> planner_server
    -> controller_server
    -> /cmd_vel_nav
    -> velocity_smoother
    -> /cmd_vel
    -> turtlebot3_diff_drive
    -> 机器人运动

今天不按“我打开了哪几个网页”的顺序写，而是按这条链本身来写。  
目标只有一个：

> 让这条主链从“运行时看起来像这样”，升级为“文档、XML、参数和运行现象都能互相对上”。  

## 今日总结一段话（外加易混淆概念）
CLI 或 RViz 作为 `NavigateToPose` 的 action client 发出导航目标；`bt_navigator` 作为 `/navigate_to_pose` 的 action server 接收 goal，并执行默认行为树 `navigate_to_pose_w_replanning_and_recovery.xml`。这里需要先分清几个容易混淆的概念：**ROS node** 是 ROS 图里的计算单元，可以通过 topics、services、actions、parameters 与其他节点交互；**LifecycleNode** 是被生命周期状态机管理的 ROS node，具有 configure、activate、deactivate、cleanup 等受控状态；而 **Planner Server / Controller Server** 则不是新的 ROS 基元类型，而是这些 node 在 Nav2 架构里的**职责名称**，表示它们分别负责处理规划请求和控制请求。换句话说，`bt_navigator`、`planner_server`、`controller_server` 首先都是 ROS node，在 Nav2 中通常又是 LifecycleNode，同时还分别承担导航编排、路径规划、路径跟踪这些 server 角色。:contentReference[oaicite:0]{index=0}

`bt_navigator` 接到 `/navigate_to_pose` 任务后，并不是自己直接算路径再直接发速度，而是去执行一棵行为树（BT）。行为树里包含很多 **BT 节点**，例如 `ComputePathToPose` 和 `FollowPath`；它们**不是 ROS node**，而是行为树 XML 里的 **BT action 节点**。所谓“对应 ROS action server 的 client 节点”，意思是：这些 BT action 节点在被行为树 tick 到时，会扮演 **ROS 2 action client** 的角色，去调用远端的 action server。Nav2 官方文档对这两个节点的定义非常直接：`ComputePathToPose` 是 *ComputePathToPose Action Server Client (Planner Interface)*，`FollowPath` 是 *FollowPath Action Server Client (Controller Interface)*；BT plugins 也明确是作为行为树 XML 中的节点，由 `BT Navigator` 处理。:contentReference[oaicite:1]{index=1}

在正常主流程中，行为树首先 tick 到 `ComputePathToPose`。这个 BT 节点会向规划侧接口发起请求；真正接请求并执行的是 `planner_server`。`planner_server` 负责处理规划请求，并维护一组 planner plugins；它会根据 `planner_id` 选择对应的 planner plugin 来计算路径。这里的 `planner_id` 不是官方固定死的编号，而是**配置里的插件映射名 / 别名**，例如常见的 `GridBased`；真正干活的是这个映射名所对应的具体 plugin 类型。Nav2 官方也明确区分了 servers 运行时使用的 **names (aliases)** 与 **types**：name 是你配置里引用算法用的别名，type 才是 pluginlib 注册的实际插件类型。算出来的 `path` 也不是 XML 文件、不是 Linux 路径，而是 `nav_msgs/Path` 类型的**路径消息**，本质上是一串供机器人跟踪的位姿点。:contentReference[oaicite:2]{index=2}

`ComputePathToPose` 计算出的 `path` 不会凭空飘到下一个节点，而是会写入**行为树黑板**中的共享变量，例如 `{path}`。黑板可以理解为行为树内部的共享变量区或中间结果仓库，供不同 BT 节点读写同一份数据；Nav2 的 `ComputePathToPose` 和 `FollowPath` 文档都明确使用这种端口写法，例如 `path="{path}"`，而 `bt_navigator` 也提供 `goal_blackboard_id`、`path_blackboard_id` 这类黑板变量配置。随后，行为树 tick 到 `FollowPath`，它从黑板读取 `{path}`，再作为 ROS 2 action client 去调用 `controller_server`。`controller_server` 同样既是 ROS node，也是 LifecycleNode；从 Nav2 角色上看，它是 Controller Server，负责处理控制请求，并根据 `controller_id` 以及相关 checker 的映射名选择对应插件，基于 `path` 持续生成速度命令。这里的 `controller_id` 与 `planner_id` 一样，也不是神秘固定编号，而是配置里的 controller plugin 映射名，例如常见的 `FollowPath`。:contentReference[oaicite:3]{index=3}

在你当前系统的运行链里，`controller_server` 将速度命令（通常是 `Twist`）发布到 `/cmd_vel_nav`；`velocity_smoother` 作为一个独立的生命周期节点订阅 `/cmd_vel_nav`，对 Nav2 输出的速度进行平滑处理，再发布到 `/cmd_vel`；最后由 `turtlebot3_diff_drive` 订阅 `/cmd_vel`，接收 `Twist` 并驱动机器人运动。于是，整条链可以压缩成一句人话：`/navigate_to_pose` 负责接任务，`bt_navigator` 负责按行为树组织流程，`ComputePathToPose` 负责去要路径，`planner_server` 负责真正把路径算出来，`FollowPath` 负责拿着路径去请求跟踪，`controller_server` 负责真正把路径变成速度命令，黑板负责在行为树节点之间传递 `{goal}`、`{path}` 这类中间数据，而 `velocity_smoother` 和底盘驱动则负责把控制命令最终落到机器人运动上。:contentReference[oaicite:4]{index=4}

---

## 2. 主链第 1 段：`/navigate_to_pose action`

## 2.1 这一段在主链中的位置

这是整条导航链的任务入口。  
它不是一个普通节点名，也不是 planner 或 controller 的内部对象，而是上层向 Nav2 发起导航任务的入口。

## 2.2 官方文档对齐

官方文档中，`NavigateToPose` 页面明确说明：该 `NavigateToPose` ROS 2 action server 由 `bt_navigator` 模块实现；其输入中还包含 `behavior_tree`，若未指定该字段，则会使用默认行为树。这个信息直接说明了两件事：第一，入口层确实是 action；第二，真正接住这个 goal 的不是 planner 或 controller，而是 `bt_navigator` 这一层。 :contentReference[oaicite:0]{index=0}

## 2.3 运行时日志对齐

Day 2 已得到以下运行时输出：

    ros2 action info /navigate_to_pose

    Action: /navigate_to_pose
    Action clients: 4
        /bt_navigator
        /waypoint_follower
        /rviz2
        /rviz_navigation_dialog_action_client
    Action servers: 1
        /bt_navigator

## 2.4 这一段形成的结论

可以把这一步先稳定写成：

    RViz / CLI 作为 action client 发出导航目标
    -> /navigate_to_pose 作为 action 入口
    -> /bt_navigator 作为 action server 接收 goal

也就是说，主链不是从 `planner_server` 开始的，而是先从 action 入口进入 `bt_navigator`。

---

## 3. 主链第 2 段：`/bt_navigator`

## 3.1 这一段在主链中的位置

`bt_navigator` 不是具体规划器，也不是具体控制器。  
它在这条主链里的角色，是接收 `NavigateToPose` 的 goal，然后根据默认行为树去组织后续流程。

## 3.2 官方文档对齐

官方文档已经给出两个关键点：

1. `NavigateToPose` 由 `bt_navigator` 实现为 action server  
2. 若未指定 `behavior_tree`，则使用默认行为树  

因此，从文档层面就可以得到：

> `bt_navigator` 的核心职责不是自己直接“算路径 + 发速度”，而是承接 action goal，并把后续流程交给 BT 组织。 :contentReference[oaicite:1]{index=1}

## 3.3 本机参数日志对齐

执行命令：

    ros2 param get /bt_navigator default_nav_to_pose_bt_xml

实际输出：

    String value is: /opt/ros/humble/share/nav2_bt_navigator/behavior_trees/navigate_to_pose_w_replanning_and_recovery.xml

说明：

- 本机默认 `NavigateToPose` 的行为树文件已经被直接查到
- 文件名中明确包含：
  - `navigate_to_pose`
  - `w_replanning`
  - `and_recovery`

## 3.4 这一段形成的结论

这一步已经可以稳定写成：

    /navigate_to_pose action
    -> /bt_navigator
    -> 默认行为树：navigate_to_pose_w_replanning_and_recovery.xml

也就是说，`bt_navigator` 的下一跳不是“直接到 planner_server”，而是先通过默认 BT 组织主流程与恢复流程。

---

## 4. 主链第 3 段：`planner_server`

## 4.1 这一段在主链中的位置

当 `bt_navigator` 驱动默认 BT 进入规划步骤时，真正承接规划请求的是 `planner_server`。

它不是具体规划算法本体，而是规划请求的 server 层对象。

## 4.2 官方文档对齐

`planner_server` 的官方页面明确说明：它是处理路径规划请求的服务器，会接收目标与规划插件名称，然后调用相应插件计算路径。文档示例还明确给出 `planner_plugins` 的结构示例，例如 `planner_plugins: ["GridBased"]`，再由 `GridBased.plugin` 映射到具体实现类。 :contentReference[oaicite:2]{index=2}

这意味着：

- `planner_server` 是 **server 层**
- 它托管的是 planner plugins
- 具体规划算法要靠 plugin 实现

## 4.3 XML 日志对齐

执行命令：

    grep -nE "ComputePathToPose|FollowPath|BackUp|Spin|Wait|Clear|Recovery" /opt/ros/humble/share/nav2_bt_navigator/behavior_trees/navigate_to_pose_w_replanning_and_recovery.xml

关键输出：

    12:          <RecoveryNode number_of_retries="1" name="ComputePathToPose">
    13:            <ComputePathToPose goal="{goal}" path="{path}" planner_id="GridBased"/>
    14:            <ClearEntireCostmap name="ClearGlobalCostmap-Context" service_name="global_costmap/clear_entirely_global_costmap"/>

这里最关键的是：

    <ComputePathToPose ... planner_id="GridBased"/>

说明默认 BT 中的规划步骤，不是抽象的“去规划一下”，而是明确使用别名 `GridBased`。

## 4.4 本机参数日志对齐

执行命令：

    ros2 param get /planner_server planner_plugins
    ros2 param get /planner_server GridBased.plugin

实际输出：

    String values are: ['GridBased']
    String value is: nav2_navfn_planner/NavfnPlanner

## 4.5 这一段形成的完整链条

现在规划段已经可以完整对齐成：

    ComputePathToPose
    -> planner_id = GridBased
    -> planner_server 的 planner_plugins = ['GridBased']
    -> GridBased.plugin = nav2_navfn_planner/NavfnPlanner

## 4.6 这一段形成的结论

规划段的最稳口径应写为：

> 默认行为树中的 `ComputePathToPose` 节点并不直接执行算法，而是通过 `planner_id="GridBased"` 把请求交给 `planner_server`；`planner_server` 再通过参数映射，把 `GridBased` 解析为具体插件实现 `nav2_navfn_planner/NavfnPlanner`。这说明 `planner_server` 是 server 层，`NavfnPlanner` 是 planner plugin 层。  

---

## 5. 主链第 4 段：`controller_server`

## 5.1 这一段在主链中的位置

当默认 BT 进入路径跟踪 / 控制阶段时，真正承接控制请求的是 `controller_server`。

它也不是具体控制算法本体，而是控制请求的 server 层对象。

## 5.2 官方文档对齐

`controller_server` 的官方页面明确说明：它是处理控制请求的服务器，并承载插件实现的映射表。文档中不仅出现了 `controller_plugins`，还出现了 `progress_checker_plugins` 与 `goal_checker_plugins`，说明 `controller_server` 管的不是单一控制算法，而是控制相关插件的统一托管层。 :contentReference[oaicite:3]{index=3}

这意味着：

- `controller_server` 是 **server 层**
- 它托管 controller plugins
- 还托管 progress checker / goal checker 等检查插件

## 5.3 XML 日志对齐

同一份默认 BT XML 中，关键输出为：

    17:        <RecoveryNode number_of_retries="1" name="FollowPath">
    18:          <FollowPath path="{path}" controller_id="FollowPath"/>
    19:          <ClearEntireCostmap name="ClearLocalCostmap-Context" service_name="local_costmap/clear_entirely_local_costmap"/>

这里最关键的是：

    <FollowPath ... controller_id="FollowPath"/>

说明默认行为树中的控制步骤，明确使用别名 `FollowPath`。

## 5.4 本机参数日志对齐

执行命令：

    ros2 param get /controller_server controller_plugins
    ros2 param get /controller_server FollowPath.plugin

实际输出：

    String values are: ['FollowPath']
    String value is: dwb_core::DWBLocalPlanner

## 5.5 这一段形成的完整链条

现在控制段已经可以完整对齐成：

    FollowPath
    -> controller_id = FollowPath
    -> controller_server 的 controller_plugins = ['FollowPath']
    -> FollowPath.plugin = dwb_core::DWBLocalPlanner

## 5.6 这一段形成的结论

控制段的最稳口径应写为：

> 默认行为树中的 `FollowPath` 节点并不直接向底盘发速度，而是通过 `controller_id="FollowPath"` 把请求交给 `controller_server`；`controller_server` 再通过参数映射，把 `FollowPath` 解析为具体控制插件 `dwb_core::DWBLocalPlanner`。这说明 `controller_server` 是 server 层，`DWBLocalPlanner` 是 controller plugin 层。  

---

## 6. 主链第 5 段：`/cmd_vel_nav`

## 6.1 这一段在主链中的位置

`/cmd_vel_nav` 不是最终给底盘的速度话题，而是控制器段输出后的中间速度话题。

它位于：

- `controller_server` 之后
- `velocity_smoother` 之前

## 6.2 Day 2 运行日志对齐

Day 2 已经抓到：

    ros2 topic info /cmd_vel_nav -v

关键输出：

    Publisher count: 1
    Node name: controller_server
    Endpoint type: PUBLISHER

    Subscription count: 1
    Node name: velocity_smoother
    Endpoint type: SUBSCRIPTION

## 6.3 这一段形成的结论

这一步已经很明确：

    controller_server
    -> /cmd_vel_nav
    -> velocity_smoother

也就是说，`controller_server` 的输出不会直接跳到底盘，而是先落到 `/cmd_vel_nav`。

---

## 7. 主链第 6 段：`velocity_smoother`

## 7.1 这一段在主链中的位置

`velocity_smoother` 位于控制输出链中间，用来把 Nav2 上游给出的速度命令做平滑处理，再输出到底盘真正使用的话题。

## 7.2 官方文档对齐

官方文档说明，`nav2_velocity_smoother` 是一个生命周期组件节点，用于平滑 Nav2 发送给机器人控制器的速度。也就是说，它不是 planner 或 controller 的内部小参数，而是控制链中的独立节点。 :contentReference[oaicite:4]{index=4}

## 7.3 Day 2 运行日志对齐

执行命令：

    ros2 node info /velocity_smoother

关键输出：

    Subscribers:
      /cmd_vel_nav: geometry_msgs/msg/Twist

    Publishers:
      /cmd_vel: geometry_msgs/msg/Twist

再结合 Day 2 的 `/cmd_vel_nav` 和 `/cmd_vel` 的 topic info，可得到：

- `velocity_smoother` 订阅 `/cmd_vel_nav`
- `velocity_smoother` 发布 `/cmd_vel`

## 7.4 这一段形成的完整链条

    controller_server
    -> /cmd_vel_nav
    -> velocity_smoother
    -> /cmd_vel

## 7.5 这一段形成的结论

这一步可以稳定写成：

> `velocity_smoother` 是控制链中的独立生命周期节点，它接收 `controller_server` 通过 `/cmd_vel_nav` 给出的速度命令，再输出平滑后的 `/cmd_vel`。  

---

## 8. 主链第 7 段：`/cmd_vel`

## 8.1 这一段在主链中的位置

`/cmd_vel` 是底盘真正订阅的速度输出话题，是主链的末端控制话题。

## 8.2 Day 2 运行日志对齐

执行命令：

    ros2 topic info /cmd_vel -v

关键输出：

    Publisher count: 5

    Node name: velocity_smoother
    Endpoint type: PUBLISHER

    Node name: behavior_server
    Endpoint type: PUBLISHER

    Subscription count: 1

    Node name: turtlebot3_diff_drive
    Endpoint type: SUBSCRIPTION

## 8.3 这一段形成的结论

主链中的正常控制输出路径可以写成：

    /cmd_vel_nav
    -> velocity_smoother
    -> /cmd_vel
    -> turtlebot3_diff_drive

同时也要注意：

- `behavior_server` 也能向 `/cmd_vel` 发布
- 所以 `/cmd_vel` 不只是正常控制链的出口
- 也是恢复 / 行为阶段可能直接介入的控制出口

---

## 9. 主链第 8 段：`turtlebot3_diff_drive`

## 9.1 这一段在主链中的位置

`turtlebot3_diff_drive` 是底盘 / 仿真驱动侧的接收者。  
它不是 Nav2 的导航逻辑对象，而是控制输出真正落地的执行端。

## 9.2 Day 2 运行日志对齐

同样来自：

    ros2 topic info /cmd_vel -v

关键输出：

    Subscription count: 1
    Node name: turtlebot3_diff_drive
    Endpoint type: SUBSCRIPTION

## 9.3 这一段形成的结论

可以稳定写成：

    /cmd_vel
    -> turtlebot3_diff_drive
    -> 机器人底盘执行运动

---

## 10. 主链第 9 段：机器人运动

## 10.1 这一段在主链中的位置

这是整条导航链的外部行为结果，也是 Day 2 成功链的终点。

## 10.2 Day 2 运行日志对齐

Day 2 运行时已经观察到：

- 发出可达 goal 后
- `/cmd_vel_nav` 刷出数据
- `/cmd_vel` 刷出数据
- 机器人实际开始运动

## 10.3 这一段形成的结论

这说明整条链不是停留在文档和参数层，而是确实跑到了实际运动结果：

    /navigate_to_pose action
    -> /bt_navigator
    -> planner_server
    -> controller_server
    -> /cmd_vel_nav
    -> velocity_smoother
    -> /cmd_vel
    -> turtlebot3_diff_drive
    -> 机器人运动

---

## 11. 主链旁支：恢复 / 行为分支

这部分不是主干固定一步，但必须挂在主链旁边解释清楚。

## 11.1 `behavior_server` 的文档定义

官方文档说明，`behavior_server` 是处理恢复 / 行为请求的 server 层对象，并托管行为插件，还共享 costmap 与 TF 等资源。 :contentReference[oaicite:5]{index=5}

## 11.2 本机参数日志

执行命令：

    ros2 param get /behavior_server behavior_plugins

输出：

    String values are: ['spin', 'backup', 'drive_on_heading', 'wait']

再执行：

    ros2 param get /behavior_server spin.plugin
    ros2 param get /behavior_server backup.plugin
    ros2 param get /behavior_server drive_on_heading.plugin
    ros2 param get /behavior_server wait.plugin

输出：

    String value is: nav2_behaviors/Spin
    String value is: nav2_behaviors/BackUp
    String value is: nav2_behaviors/DriveOnHeading
    String value is: nav2_behaviors/Wait

## 11.3 默认 BT XML 日志

同一份默认 XML 中，关键输出为：

    22:      <ReactiveFallback name="RecoveryFallback">
    24:        <RoundRobin name="RecoveryActions">
    25:          <Sequence name="ClearingActions">
    26:            <ClearEntireCostmap .../>
    27:            <ClearEntireCostmap .../>
    29:          <Spin spin_dist="1.57"/>
    30:          <Wait wait_duration="5"/>
    31:          <BackUp backup_dist="0.30" backup_speed="0.05"/>

## 11.4 恢复 / 行为分支形成的链条

恢复分支可以写成：

    RecoveryNode / RecoveryFallback
    -> ClearEntireCostmap
    -> Spin / Wait / BackUp
    -> behavior_server
    -> nav2_behaviors/*
    -> 条件介入 /cmd_vel 输出链

## 11.5 这一段需要特别说明的点

### 说明 1：`behavior_server` 是 server 层，不是某一个行为动作
它托管的是一组 behavior plugins，而不是只对应一个动作。

### 说明 2：默认 BT 使用了 `Spin / Wait / BackUp`，但不等于所有已加载插件都必然被当前默认 BT 使用
例如本机 `behavior_plugins` 中还有：

    drive_on_heading -> nav2_behaviors/DriveOnHeading

但当前默认 XML grep 结果中并未直接出现 `DriveOnHeading`。

因此必须区分：

- **已加载的 plugin 集合**
- **当前默认 BT 实际调用到的 plugin 子集**

### 说明 3：`ClearEntireCostmap` 属于恢复逻辑，但不能简单粗暴地和 `spin / wait / backup` 全部当成同一层的 behavior plugin
更稳的表述是：

> 默认恢复逻辑既包含 behavior plugin 对应的动作节点，也包含清图类 BT 节点 / 服务动作。

---

## 12. 支撑层在主链中的位置

虽然今天文档与参数对齐的主轴是主链，但支撑层也要在主链旁边交代清楚。

## 12.1 Day 2 已有运行时证据

Day 2 已看到：

- `/map`
- `/amcl_pose`
- global costmap 相关 topic
- local costmap 相关 topic

这说明：

- `map`
- `localization`
- `costmap`

都真实存在于系统中。

## 12.2 文档层补充

`behavior_server` 文档明确提到其共享资源包括：

- costmaps
- TF 缓冲区  

这说明支撑层不是“和主链无关的背景板”，而是会直接支撑行为 / 恢复请求。 :contentReference[oaicite:6]{index=6}

## 12.3 支撑层在主链中的角色

更稳的表述应写为：

> `map`、`localization`、`costmap`、`TF` 不直接构成主干箭头，但会持续影响规划、控制与恢复行为，是主链运行所依赖的支撑层。

---

## 13. 整条主链的 Day 3 版总图

把 Day 2 的运行链和 Day 3 的文档 / XML / 参数对齐结果合并后，主链可以更完整地写成：

    /navigate_to_pose action
    -> /bt_navigator
    -> 默认 BT: navigate_to_pose_w_replanning_and_recovery.xml

    主流程：
      ComputePathToPose
      -> planner_id = GridBased
      -> planner_server
      -> nav2_navfn_planner/NavfnPlanner

      FollowPath
      -> controller_id = FollowPath
      -> controller_server
      -> dwb_core::DWBLocalPlanner
      -> /cmd_vel_nav
      -> velocity_smoother
      -> /cmd_vel
      -> turtlebot3_diff_drive
      -> 机器人运动

    恢复 / 行为旁支：
      RecoveryNode / RecoveryFallback
      -> ClearEntireCostmap
      -> Spin / Wait / BackUp
      -> behavior_server
      -> nav2_behaviors/*
      -> 条件介入 /cmd_vel 输出链

    支撑层：
      map / localization / costmap / TF

---

## 14. 这些东西是如何相互验证的

这一节是今天最重要的收口部分。

## 14.1 文档验证“角色定义”

文档告诉你：

- `NavigateToPose` 是什么
- `bt_navigator` 为什么能接 action
- `planner_server` / `controller_server` / `behavior_server` 各自是不是 server 层
- `velocity_smoother` 为什么是独立节点

所以文档负责回答：

> **这些对象在系统设计里分别是什么角色。** :contentReference[oaicite:7]{index=7}

## 14.2 XML 验证“流程组织”

默认 BT XML 告诉你：

- 主流程里确实有 `ComputePathToPose`
- 主流程里确实有 `FollowPath`
- 恢复逻辑里确实有 `Spin / Wait / BackUp / ClearEntireCostmap`

所以 XML 负责回答：

> **这条导航链内部到底是怎么被组织起来的。**

## 14.3 参数验证“server 如何连到 plugin”

参数直接告诉你：

- `planner_server` 当前加载的 planner plugin 别名是什么
- `controller_server` 当前加载的 controller plugin 别名是什么
- `behavior_server` 当前加载的 behavior plugins 是什么
- 每个别名具体映射到哪一个 plugin 类名

所以参数负责回答：

> **server 层和 plugin 层在本机配置中是如何连接起来的。**

## 14.4 运行日志验证“这条链真的跑起来了”

Day 2 的运行日志已经说明：

- action server 确实是 `/bt_navigator`
- `controller_server` 确实发布 `/cmd_vel_nav`
- `velocity_smoother` 确实订阅 `/cmd_vel_nav` 并发布 `/cmd_vel`
- `turtlebot3_diff_drive` 确实订阅 `/cmd_vel`
- 发 goal 后机器人确实开始运动

所以运行日志负责回答：

> **这条链不只是文档和配置上的结构，而是真的在你机器上跑起来了。**

## 14.5 四层信息如何形成闭环

以规划段为例，闭环是：

    文档：
      planner_server 是处理规划请求的 server

    XML：
      ComputePathToPose planner_id="GridBased"

    参数：
      planner_plugins = ['GridBased']
      GridBased.plugin = nav2_navfn_planner/NavfnPlanner

    运行链：
      规划结果继续被下游控制段使用

控制段也是同样的逻辑：

    文档：
      controller_server 是处理控制请求的 server
      velocity_smoother 是独立平滑节点

    XML：
      FollowPath controller_id="FollowPath"

    参数：
      controller_plugins = ['FollowPath']
      FollowPath.plugin = dwb_core::DWBLocalPlanner

    运行链：
      controller_server -> /cmd_vel_nav -> velocity_smoother -> /cmd_vel

恢复段也是一样：

    文档：
      behavior_server 处理行为 / 恢复请求

    XML：
      Spin / Wait / BackUp / ClearEntireCostmap

    参数：
      spin.plugin / backup.plugin / wait.plugin / drive_on_heading.plugin

    运行链：
      behavior_server 可直接参与 /cmd_vel 输出链

也就是说：

> 文档给角色，XML 给流程，参数给映射，运行日志给落地。  
> 四层一起对上之后，主链才算真正被解释清楚。

---

## 15. 今日结论

今天最大的成果，不是“看了很多页面”，而是把整条主链按顺序对齐了。

### 15.1 现在可以稳定讲出的主链

现在可以较稳地讲出：

    /navigate_to_pose action
    -> /bt_navigator
    -> 默认 BT: navigate_to_pose_w_replanning_and_recovery.xml
    -> planner_server
    -> controller_server
    -> /cmd_vel_nav
    -> velocity_smoother
    -> /cmd_vel
    -> turtlebot3_diff_drive
    -> 机器人运动

并能进一步展开：

- 规划段由 `ComputePathToPose -> GridBased -> NavfnPlanner` 支撑
- 控制段由 `FollowPath -> FollowPath.plugin -> DWBLocalPlanner -> /cmd_vel_nav -> velocity_smoother -> /cmd_vel` 支撑
- 恢复段由 `Spin / Wait / BackUp / ClearEntireCostmap -> behavior_server -> nav2_behaviors/*` 支撑

### 15.2 今天最重要的认知升级

今天最重要的认知升级有三点：

1. `NavigateToPose`、`bt_navigator`、`planner_server`、`controller_server`、`behavior_server` 不再是散乱模块名，而是已经能被按主链顺序串起来  
2. `planner_server` / `controller_server` / `behavior_server` 与具体 plugin 不再混层  
3. 默认行为树、参数映射和运行日志已经能互相闭环验证，而不是各看各的

---

## 16. 今日复盘

## 16.1 今天最容易混淆但已经比昨天更清楚的点

### 点 1：server 和 plugin 不是一层
今天已经能比较稳地拆开：

- `planner_server` ≠ `NavfnPlanner`
- `controller_server` ≠ `DWBLocalPlanner`
- `behavior_server` ≠ 某一个具体行为动作

### 点 2：默认 BT 使用的插件不等于 server 已加载的全部插件
`DriveOnHeading` 已加载，但当前默认 `navigate_to_pose` XML 没直接出现。  
这说明“已加载”和“当前被默认流程用到”不是一回事。

### 点 3：恢复逻辑不只是一组 behavior plugin
`ClearEntireCostmap` 也在恢复逻辑里，但它不应和 `Spin / Wait / BackUp` 粗暴地当成同一种 plugin 对象。

## 16.2 仍未完全解决的问题

今天仍然没有彻底解决的问题包括：

1. `bt_navigator` 内部如何把 action goal 转成 BT tick 流程  
2. `goal_checker` 与 `progress_checker` 在控制链中的实际参与方式  
3. TF 的直接运行证据仍未单独补抓  
4. 包级实现入口与源码结构还没有系统整理成独立映射表

## 16.3 对 Day 4 的输入

Day 4 最自然的方向是：

- 不再继续纯阅读
- 围绕主链中的一个关键判断做最小修改 / 最小对照验证

最合适的切入点有两个：

### 方向 A：验证默认 BT 中某个关键节点对主链的作用
例如围绕：
- `ComputePathToPose`
- `FollowPath`
- 恢复逻辑

做最小验证

### 方向 B：验证某个配置入口对主链表现的影响
例如围绕：
- planner plugin
- controller plugin
- velocity_smoother 相关参数

做最小对照

## 16.4 今日最低结论

今天已经完成了 Sprint 1 Day 3 的关键任务：

> 按主链顺序，把入口、流程组织、规划段、控制段、恢复段、速度平滑段、底盘执行段分别在文档、XML、参数和运行日志上对齐，并形成了能互相验证的闭环解释。

---
