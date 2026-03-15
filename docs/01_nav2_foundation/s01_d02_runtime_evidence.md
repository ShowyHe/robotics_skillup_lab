# s01_d02_runtime_evidence.md

# Sprint 1 Day 2｜运行时证据采集

## 1. 今日目标

今天的任务，不是继续补概念，而是用运行系统输出去验证 Day 1 画出的 `goal → motion` 主链骨架。

今天重点验证以下问题：

1. 导航任务是否确实从 `/navigate_to_pose` 这类 action 入口进入系统
2. `bt_navigator`、`planner_server`、`controller_server`、`behavior_server` 是否确实存在并处于运行状态
3. 控制输出链是否真能从导航任务走到底盘控制
4. `map`、`localization`、`costmap` 这些对象是否确实作为支撑层存在
5. Day 1 的简化主链是否需要根据运行证据进行修正

---

## 2. 今日运行证据

### 2.1 导航任务入口证据：`/navigate_to_pose` 是 action，不是普通节点

执行命令：

    ros2 action list | grep navigate
    ros2 action info /navigate_to_pose

关键输出：

    /navigate_to_pose

    Action: /navigate_to_pose
    Action clients: 4
        /bt_navigator
        /waypoint_follower
        /rviz2
        /rviz_navigation_dialog_action_client
    Action servers: 1
        /bt_navigator

基于以上输出，可以得到以下判断：

- `/navigate_to_pose` 确实存在于 action 列表中
- `/navigate_to_pose` 的 action server 是 `/bt_navigator`
- 因此，导航任务入口不是一个普通节点名，而是一个 action 入口
- 真正接收导航任务并组织流程的对象是 `/bt_navigator`

---

### 2.2 核心执行对象存在性证据

执行命令：

    ros2 node list | grep -E "bt_navigator|planner_server|controller_server|behavior_server|amcl|map_server"

关键输出：

    /amcl
    /behavior_server
    /bt_navigator
    /bt_navigator_navigate_through_poses_rclcpp_node
    /bt_navigator_navigate_to_pose_rclcpp_node
    /controller_server
    /map_server
    /planner_server

基于以上输出，可以得到以下判断：

- `bt_navigator`、`planner_server`、`controller_server`、`behavior_server` 均真实存在于运行系统中
- `amcl` 与 `map_server` 也存在，说明定位与地图服务链路已经启动
- Day 1 主链骨架中的核心对象不是抽象猜测，而是当前系统里的实际运行对象

---

### 2.3 核心生命周期状态证据：关键对象均处于 active

执行命令：

    ros2 lifecycle get /bt_navigator
    ros2 lifecycle get /planner_server
    ros2 lifecycle get /controller_server
    ros2 lifecycle get /behavior_server

关键输出：

    active [3]
    active [3]
    active [3]
    active [3]

基于以上输出，可以得到以下判断：

- `bt_navigator`、`planner_server`、`controller_server`、`behavior_server` 当前均处于 active 状态
- 这些对象不只是“存在于图中”，而是已经激活并可参与运行
- 因此，Day 1 主链中的流程组织者与能力提供者，当前都处于有效运行状态

---

### 2.4 支撑层存在性证据：地图、定位、代价地图已出现

执行命令：

    ros2 topic list | grep -E "^/map$|costmap|amcl_pose|cmd_vel"

关键输出：

    /amcl_pose
    /cmd_vel
    /cmd_vel_nav
    /downsampled_costmap
    /downsampled_costmap_updates
    /global_costmap/clearing_endpoints
    /global_costmap/costmap
    /global_costmap/costmap_raw
    /global_costmap/costmap_updates
    /global_costmap/footprint
    /global_costmap/global_costmap/transition_event
    /global_costmap/published_footprint
    /global_costmap/voxel_grid
    /global_costmap/voxel_marked_cloud
    /local_costmap/clearing_endpoints
    /local_costmap/costmap
    /local_costmap/costmap_raw
    /local_costmap/costmap_updates
    /local_costmap/footprint
    /local_costmap/local_costmap/transition_event
    /local_costmap/published_footprint
    /local_costmap/voxel_grid
    /local_costmap/voxel_marked_cloud
    /map

基于以上输出，可以得到以下判断：

- `/map` 已出现，说明全局地图链路存在
- `/amcl_pose` 已出现，说明定位位姿输出存在
- global costmap 与 local costmap 相关 topic 均大量存在，说明代价地图支撑层正在工作
- 因此，Day 1 将 `map`、`localization`、`costmap` 放入支撑层的判断是成立的

说明：

- 今天没有直接补抓 `/tf` 的运行证据，因此 TF 仍然保留为“逻辑上应存在、但今日未直接验证”的支撑层对象
- 后续可再单独补查 `/tf` 与相关 frame 关系

---

### 2.5 控制输出链证据：`controller_server -> /cmd_vel_nav -> velocity_smoother -> /cmd_vel`

执行命令：

    ros2 topic info /cmd_vel_nav -v
    ros2 topic info /cmd_vel -v
    ros2 node info /velocity_smoother

关键输出 1：`/cmd_vel_nav`

    Type: geometry_msgs/msg/Twist

    Publisher count: 1

    Node name: controller_server
    Endpoint type: PUBLISHER

    Subscription count: 1

    Node name: velocity_smoother
    Endpoint type: SUBSCRIPTION

关键输出 2：`/cmd_vel`

    Type: geometry_msgs/msg/Twist

    Publisher count: 5

    Node name: behavior_server
    Endpoint type: PUBLISHER
    ...
    Node name: velocity_smoother
    Endpoint type: PUBLISHER

    Subscription count: 1

    Node name: turtlebot3_diff_drive
    Endpoint type: SUBSCRIPTION

关键输出 3：`/velocity_smoother`

    /velocity_smoother
      Subscribers:
        /cmd_vel_nav: geometry_msgs/msg/Twist
      Publishers:
        /cmd_vel: geometry_msgs/msg/Twist

基于以上输出，可以得到以下判断：

- `controller_server` 不是直接发布 `/cmd_vel`
- `controller_server` 发布的是 `/cmd_vel_nav`
- `velocity_smoother` 订阅 `/cmd_vel_nav`，并发布 `/cmd_vel`
- `turtlebot3_diff_drive` 订阅 `/cmd_vel`
- 因此，正常控制输出链已经被运行证据明确修正为：

    controller_server
      -> /cmd_vel_nav
      -> velocity_smoother
      -> /cmd_vel
      -> turtlebot3_diff_drive

这说明 Day 1 中“`controller_server -> /cmd_vel`”的画法过于简化，需要在链路图中修正。

---

### 2.6 `behavior_server` 的运行位置证据

从 `/cmd_vel -v` 的输出可见，`behavior_server` 也是 `/cmd_vel` 的 publisher。

这说明：

- `behavior_server` 不只是抽象上的“恢复模块”
- 在运行系统中，它确实可以直接参与控制输出链
- 因此，更稳妥的理解不是把它强行画成线性主干固定一步
- 更合适的表达是：它在恢复阶段、行为阶段或特定条件下介入，并可能直接向 `/cmd_vel` 输出控制命令

---

### 2.7 动态成功链证据：goal 发出后，控制话题刷出且机器人运动

运行现象：

- 发出可达 goal 后，`/cmd_vel_nav` 刷出数据
- 同时 `/cmd_vel` 也刷出数据
- 机器人实际开始运动

基于该现象，可以得到以下判断：

- 导航任务不是停留在 action 接收层
- 主链确实向下推进到了控制输出阶段
- `controller_server -> /cmd_vel_nav -> velocity_smoother -> /cmd_vel -> 底盘` 这条动态链已经得到成功运行现象支撑

---

## 3. 修正后的执行链路图 v1

### 3.1 Day 1 简化版主链

    NavigateToPose
       ↓
    bt_navigator
       ↓
    planner_server
       ↓
    controller_server
       ↓
    /cmd_vel

这个版本用于建立第一版骨架是合理的，但在控制输出部分过于简化。

### 3.2 Day 2 基于运行证据修正后的链路图

    [RViz / CLI 发送 NavigateToPose goal]
                    |
                    v
           [/navigate_to_pose action]
                    |
                    v
              [/bt_navigator]
        接收 goal，组织 BT 导航流程
                    |
          +---------+-------------------+
          |                             |
          v                             v
    [planner_server]             [behavior_server]
      生成全局路径                 恢复 / 行为阶段介入
          |
          v
    [controller_server]
      根据 path + 当前状态计算控制
          |
          v
       [/cmd_vel_nav]
          |
          v
    [/velocity_smoother]
          |
          v
         [/cmd_vel]
          |
          v
    [turtlebot3_diff_drive]
          |
          v
       [机器人运动]

    支撑层：
    map / localization / global_costmap / local_costmap / TF(待补直接证据)

总体上，RViz 或 CLI 作为 action client 发出目标点后，/navigate_to_pose 这个 action 的 goal 会被 bt_navigator 作为 action server 接收。bt_navigator 随后开始组织导航流程，通过默认 BT 先调用 planner_server 生成全局路径，再调用 controller_server 根据全局路径、局部环境和机器人当前状态计算速度控制。controller_server 发布 /cmd_vel_nav，velocity_smoother 订阅 /cmd_vel_nav 并发布 /cmd_vel，最后 /cmd_vel 被 turtlebot3_diff_drive 接收，机器人执行运动。若导航受阻或进入恢复阶段，behavior_server 也可以介入，并直接参与控制输出链。与此同时，map、localization、costmap、TF 作为支撑层持续影响规划、控制与恢复行为。

### 3.3 当前链路图的解释

根据今天抓到的运行证据，导航任务入口应更准确地写成 `/navigate_to_pose action`，因为真正进入系统的是 action goal，而不是某个普通节点对象。

`/bt_navigator` 负责接收 goal 并组织 BT 导航流程。`planner_server` 提供路径规划能力，`controller_server` 提供路径跟踪与控制能力。但 `controller_server` 的输出并不是直接到底盘，而是先发布到 `/cmd_vel_nav`，再由 `velocity_smoother` 平滑处理后发布到 `/cmd_vel`，最后由 `turtlebot3_diff_drive` 订阅并驱动机器人运动。

`behavior_server` 不适合画成线性主干中的固定一步，因为它更像是在恢复或行为阶段按条件介入；但今天的运行证据已经证明，它可以直接向 `/cmd_vel` 输出控制命令，因此它不是“旁观者”，而是可直接进入控制输出链的动作提供者。

---

## 4. 对 Day 1 骨架的修正结论

### 4.1 已被 Day 2 运行证据钉实的内容

以下判断已被 Day 2 运行证据支撑：

1. `NavigateToPose` 是 action 入口，不是普通节点名
2. 真正接收导航任务的是 `/bt_navigator`
3. `bt_navigator`、`planner_server`、`controller_server`、`behavior_server` 在系统中真实存在，且均处于 active
4. `map`、`localization`、`costmap` 属于支撑层，不是主干执行者
5. 正常控制输出链不是 `controller_server -> /cmd_vel`，而是经过 `velocity_smoother`
6. `behavior_server` 在运行时可直接参与 `/cmd_vel` 输出链

### 4.2 仍未完全钉死的内容

以下问题仍需后续继续验证：

1. `bt_navigator` 内部具体通过哪些 BT 节点组织流程
2. `behavior_server` 在什么条件下被触发、介入顺序如何
3. TF 的直接运行证据今天尚未补抓
4. planner 与 controller 的更细插件层关系尚未展开

---

## 5. 今日结论

今天最重要的收获，不是“看到了很多 topic 和 node”，而是把 Day 1 的主链从概念骨架推进成了有运行证据支撑的结构判断。

截至今天，可以形成以下较稳结论：

- 导航任务通过 `/navigate_to_pose` action 进入系统
- `/bt_navigator` 是接收 goal 并组织导航流程的核心对象
- `planner_server`、`controller_server`、`behavior_server` 均真实存在并处于 active
- `map`、`localization`、`costmap` 等对象作为支撑层存在
- 正常控制输出链应修正为：

    controller_server
      -> /cmd_vel_nav
      -> velocity_smoother
      -> /cmd_vel
      -> turtlebot3_diff_drive

- `behavior_server` 在恢复 / 行为阶段可以直接参与控制输出链
- 在可达 goal 下，`/cmd_vel_nav` 与 `/cmd_vel` 都刷出数据，机器人也实际运动，说明主链已经被动态成功链证据支撑

---

## 6. 今日复盘

### 6.1 今天最大的理解升级

今天最大的理解升级，是把“控制输出”从 Day 1 的简化理解：

    controller_server -> /cmd_vel

修正成了更符合实际系统的工程链路：

    controller_server -> /cmd_vel_nav -> velocity_smoother -> /cmd_vel -> turtlebot3_diff_drive

这说明 Day 1 骨架图的作用是建立方向，不是一次到位。Day 2 的职责，就是拿运行证据去修正这些过于简化的地方。

### 6.2 今天仍然容易混淆的点

今天仍然容易混淆的点包括：

1. `bt_navigator` 的“接任务”与“执行 BT”之间到底如何分层理解
2. `behavior_server` 是“恢复模块”还是“可直接输出控制的动作模块”
3. TF 在主链中的证据位置还没有直接补抓
4. `planner_server`、`controller_server` 与具体 plugin 的关系还没有展开

### 6.3 对 Day 3 的输入

明天 Day 3 不应再停留在“对象存在性”层面，而应开始做“文档与实现入口对齐”，重点应包括：

1. 默认 BT XML 如何对应今天看到的主链
2. `bt_navigator` 如何承载 `NavigateToPose` 这类 action
3. `planner_server`、`controller_server`、`behavior_server` 在文档和实现入口中的角色如何与今天的运行证据对应
4. `velocity_smoother` 在默认 bringup 中的角色如何归位到总链中

### 6.4 今日最低结论

今天已经拿到了主链的第一批运行时硬证据。  
Day 1 的主链不是推翻重来，而是经 Day 2 修正后进入了更稳的 v1 状态。

---