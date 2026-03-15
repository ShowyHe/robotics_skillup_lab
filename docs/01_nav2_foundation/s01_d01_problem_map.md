# s01_d01_problem_map.md

# Sprint 1 Day 1｜问题定义 + 执行链骨架图 v0

## 1. 当天计划

### 1.1 今天的主题

问题定义 + `goal → motion` 执行链骨架图 v0

### 1.2 今天的核心问题

今天的唯一核心问题，是先讲清 **Nav2 导航任务主链**：

> 一个导航任务从 `NavigateToPose` 发出，到机器人最终产生控制输出，中间经过哪些核心对象，它们分别处于什么位置、承担什么职责。

### 1.3 今天在当前 Sprint 中的位置

今天不是做细节深挖，也不是做源码深读，更不是做配置实验。  
今天只做三件事：

1. 把 Sprint 1 的边界钉死  
2. 把 `goal → motion` 主链的一级骨架画出来  
3. 把当前最容易混淆的边界记录下来，作为后续几天继续拆解的入口  

### 1.4 今天明确不做什么

今天不做以下内容：

- 不深挖 BT / Action / Lifecycle 的调度细节
- 不深挖 Planner / Controller / Behavior 的契约边界
- 不深挖 Costmap / Footprint / TF / Localization 的系统级耦合
- 不展开参数、插件装载、默认 BT、运行角色的一一对应
- 不展开大段源码阅读
- 不做复杂实验
- 不讨论 QoS / DDS / executor / callback group 等后续模块内容

一句话说清：

> 今天只负责先把主链立起来，不抢后面几天的活。

---

## 2. 当天输入

### 2.1 仓库内输入

- `docs/00_overview/README.md`
- `docs/00_overview/phase_01_to_06_master_plan.md`
- `docs/01_nav2_foundation_7d/01_plan.md`
- `docs/01_nav2_foundation_7d/s01_overview.md`

### 2.2 外部输入

- Nav2 官方文档中与整体架构、BT Navigator、planner、controller、behavior 直接相关的资料
- 默认 BT XML
- 默认参数文件
- 必要时查看 `navigation2` 仓库中的最小入口结构

### 2.3 今天输入的使用原则

今天看输入，不是为了“多看资料”，而是为了回答一个问题：

> 主链里到底应该放哪些一级对象，它们大致处于哪一层？

---

## 3. 当天输出

### 3.1 必交输出

今天必须完成以下 3 个输出：

1. `fig_01_goal_to_motion_chain_v0.png`
2. `note_01_problem_statement.md`
3. `s01_d01_problem_map.md`

### 3.2 输出要求

#### 输出 1：执行链骨架图 v0

这张图必须至少包含以下对象：

- `NavigateToPose`
- `bt_navigator`
- `planner_server`
- `controller_server`
- `behavior_server`
- `/cmd_vel`
- `costmap`
- `TF`
- `map`
- `localization`

这张图今天不追求完全正确，但必须满足：

- 主链方向清晰
- 一级对象齐全
- 支撑层和主执行层不要混在一起
- 能作为 Day 2 运行时证据采集的底图

#### 输出 2：问题说明笔记

这份笔记至少要写清：

- Sprint 1 要解决的核心问题是什么
- Day 1 为什么先做骨架图
- 当前最容易混淆的边界有哪些
- 今天完成后，明天要靠运行时证据验证什么

#### 输出 3：Day 1 总文档

这份文档需要收口今天的全部内容，至少包括：

- 问题边界
- 执行链骨架图 v0
- 核心对象卡片清单 v0
- 主链口述 v0
- 当天闭卷
- 原始回答
- GPT 修正后的标准回答
- 当天复盘

---

## 4. Day 1 的问题边界说明

1. 今天的唯一核心问题是什么  
今天的唯一核心问题，是先讲清 **Nav2 导航任务主链**：一个任务从 `NavigateToPose` 发出，到机器人最终产生控制输出，中间经过哪些核心对象，它们分别处于什么位置、承担什么职责。

2. 为什么今天先画主链  
因为 01 模块后续的学习，都是围绕这条主链逐步展开的。先有主链，后面才能继续拆调度骨架、模块边界、支撑层耦合和配置映射。主链就是后续学习的总地图。

3. 为什么今天不深挖源码  
因为当前阶段的任务是先建立系统骨架，而不是直接扎进实现细节。若没有主链总图，直接看源码很容易只记住局部文件和类名，却讲不清整个系统是如何协作的。

4. 为什么今天不去做 QoS / DDS / executor  
因为这些内容属于后续模块的运行机制层，不属于今天的主问题。今天先把 Nav2 导航任务主链立起来，再在后续模块中继续下钻系统底层机制。

5. 今天的产出最后要服务 Sprint 1 的哪个目标  
今天的产出，服务于 Sprint 1 的核心目标：建立 `NavigateToPose → 控制输出` 的第一版总执行链，并形成后续几天继续用运行证据、文档和实现入口去验证与修正的骨架图。

---

## 5. 执行链骨架图 v0

### 5.1 文字版骨架图

    NavigateToPose
       ↓
    bt_navigator
       ↓
    planner_server
       ↓
    controller_server
       ↓
    /cmd_vel

    behavior_server：在受阻 / 恢复时提供支持
    costmap / TF / map / localization：作为支撑层影响规划、控制与恢复

### 5.2 当前版图下注释

当前我把 `NavigateToPose` 看作任务入口，把 `bt_navigator` 看作流程组织者，把 `planner_server`、`controller_server`、`behavior_server` 看作能力提供者，把 `costmap`、`TF`、`map`、`localization` 看作支撑层。

### 5.3 当前版主链解释

在当前理解下，导航任务不是直接发给 `planner_server` 或 `controller_server`，而是先从 `NavigateToPose` 这个任务入口进入系统。真正接收并组织这个任务的是 `bt_navigator`。随后，系统会在主流程中调用 `planner_server` 生成路径，再调用 `controller_server` 输出控制，最终落到 `/cmd_vel` 这一类运动控制结果上。

`behavior_server` 目前不先画成线性主干固定一步，而是作为受阻、恢复或特定行为阶段的支持模块出现。`costmap`、`TF`、`map`、`localization` 不直接站在主干箭头上，但会持续影响规划、控制和恢复行为，因此必须作为支撑层一起出现在骨架图里。

---

## 6. 核心对象卡片清单 v0

### 6.1 NavigateToPose

`NavigateToPose` 在 Nav2 中首先应理解为一个高层导航任务的 ROS 2 Action 接口，而不是具体的算法模块。它用于接收外部系统提交的单目标导航请求，任务输入通常包括目标位姿 `pose`，并可选携带本次任务使用的行为树 XML。该接口在任务执行过程中还会持续返回导航反馈信息，并在结束时返回成功或失败结果。

### 6.2 bt_navigator

`bt_navigator` 是 Nav2 中负责执行高层导航任务的核心导航器。它实现了 `NavigateToPose` 等导航任务接口，本身不是“行为树本体”，而是行为树的运行器与任务调度者。其主要职责是：接收导航目标、加载对应的行为树 XML、初始化运行上下文与黑板数据、循环 tick 行为树节点，并通过行为树节点进一步调用 `planner_server`、`controller_server` 与 `behavior_server` 完成规划、控制与恢复等任务。

### 6.3 行为树本体

行为树本体不是 `bt_navigator` 节点本身，而是由行为树 XML、BT 节点插件以及运行时黑板共同构成的可执行流程结构。它描述了导航任务的执行逻辑，例如：何时进行路径规划、何时开始路径跟踪、何时触发重规划、规划失败后如何执行上下文恢复、系统级失败后如何执行旋转、等待、后退、清图等恢复动作。

### 6.4 三者关系

从系统分层看，`NavigateToPose` 属于任务入口接口，`bt_navigator` 属于流程组织与任务执行层，行为树属于导航流程逻辑描述层。`bt_navigator` 接收 `NavigateToPose` 任务后，并不是直接硬编码顺序调用 planner 或 controller，而是按照行为树定义的流程循环执行；当行为树 tick 到规划节点时调用 `planner_server`，tick 到跟踪节点时调用 `controller_server`，tick 到恢复节点时调用 `behavior_server`。

### 6.5 planner_server

- 我当前把它放在：能力执行层
- 我当前理解：它负责根据目标和环境信息生成全局路径
- 我还不确定：它和 planner plugin 的关系是什么

### 6.6 controller_server

- 我当前把它放在：能力执行层
- 我当前理解：它负责根据路径、局部环境和机器人状态计算控制输出
- 我还不确定：它与 local costmap 的依赖细节是什么

### 6.7 behavior_server

- 我当前把它放在：能力执行层
- 我当前理解：它在受阻、恢复或特定行为阶段提供支持，例如旋转、后退、等待等
- 我还不确定：它何时介入主链，何时不介入

### 6.8 /cmd_vel

- 我当前把它放在：控制输出层
- 我当前理解：它可以作为主链尾部锚点，因为导航最后要落到运动控制输出
- 我还不确定：输出是否总是直接表现为这里的速度命令

### 6.9 costmap

- 我当前把它放在：支撑层
- 我当前理解：它提供环境代价信息，支持规划和控制判断哪里能走、哪里危险
- 我还不确定：global costmap 和 local costmap 的分工细节

### 6.10 TF

- 我当前把它放在：支撑层
- 我当前理解：它提供坐标变换关系，让各模块能在统一参考系下理解位置和姿态
- 我还不确定：哪些关键模块最依赖 TF

### 6.11 map

- 我当前把它放在：支撑层
- 我当前理解：它提供全局地图信息，是全局规划的重要基础
- 我还不确定：它与 global costmap 的边界怎么区分

### 6.12 localization

- 我当前把它放在：支撑层
- 我当前理解：它提供机器人当前位姿估计，决定系统“知道自己在哪”
- 我还不确定：它和 TF、map 的边界如何拆开

---

## 7. 当前最容易混淆的边界

1. `NavigateToPose` 到底是节点、话题、服务还是 action 任务入口？它为什么不是一个普通节点名？
2. `bt_navigator` 是“行为树本体”，还是“运行行为树并组织导航流程的节点”？
3. `planner_server` 和 `controller_server` 都属于 server，但它们在主链中的职责边界到底是什么？
4. `behavior_server` 是始终参与主链，还是只在失败、受阻或恢复阶段介入？
5. `costmap`、`TF`、`map`、`localization` 为什么不在主干箭头上，却又会共同影响最终导航行为？

---

## 8. 我当前理解的 NavigateToPose → motion 主链 v0

`NavigateToPose` 不是节点，而是导航任务入口。外部系统把目标发送进来后，真正接收并组织这个导航任务的是 `bt_navigator`。`bt_navigator` 不是“行为树本体”，而是运行行为树并组织导航流程的核心导航器，它会根据流程去调用后面的能力模块。

在主链中，`planner_server` 负责根据目标和环境信息生成全局路径，`controller_server` 负责根据全局路径、局部环境信息和机器人当前状态输出控制，最终形成 `/cmd_vel` 这一类运动控制结果。`behavior_server` 主要在导航受阻、失败或需要恢复动作时提供支持，例如旋转、后退、等待等。

`costmap`、`TF`、`map` 和 `localization` 不直接作为主链主干去接收任务或输出控制，但它们作为支撑层会持续影响规划、控制和恢复行为，因此必须一起出现在执行链骨架图中。

---

## 9. 当天闭卷测试

### 9.1 闭卷题目

1. `NavigateToPose` 为什么不是节点？
2. `bt_navigator` 到底是什么？
3. 为什么 `bt_navigator` 不等于“行为树本体”？
4. `localization` 在导航里解决的最朴素问题是什么？
5. 为什么 `costmap / TF / map / localization` 先放支撑层，而不是主干箭头上？
6. 为什么 `behavior_server` 现在先写成“提供支持”，而不是直接写成主干固定一步？

### 9.2 闭卷要求

- 先写“我的原始回答”
- 不查资料，先暴露真实理解
- 再写“GPT 修正后的标准回答”
- 修正重点不是追求术语堆砌，而是修正分层、对象位置和职责关系

---

## 10. 我的原始回答

### 10.1 `NavigateToPose` 为什么不是节点？

我还是不知道这个玩意是什么，但是它不是节点，它的作用是接受导航任务，接受目标点。

### 10.2 `bt_navigator` 到底是什么？

是调度层，行为树是它的一个节点，它包括行为树还有其他节点，这个东西负责调度什么时候运行 `planner`、`controller`、`behavior`。

### 10.3 为什么 `bt_navigator` 不等于“行为树本体”？

这个东西包含行为树，行为树是它的一个节点。

### 10.4 `localization` 在导航里解决的最朴素问题是什么？

解决的是定位，Nav2 里面常用的是 AMCL。

### 10.5 为什么 `costmap / TF / map / localization` 先放支撑层，而不是主干箭头上？

因为这些不参与导航任务，而是提供代价地图、坐标转换、全局地图、定位等技术支持。

### 10.6 为什么 `behavior_server` 现在先写成“提供支持”，而不是直接写成主干固定一步？

因为这个也不是导航的主链，而是在导航受阻时提供后退、旋转、清图、等待等行为，帮助导航继续进行。

---

## 11. GPT 修正后的标准回答

### 11.1 `NavigateToPose` 为什么不是节点？

`NavigateToPose` 不是节点，因为它表示的是一个导航任务接口 / action 任务类型，不是一个运行中的 ROS 节点名。外部系统把目标点作为导航任务发出去时，真正接收这个任务的是 `bt_navigator` 实现的 `NavigateToPose` action server，而不是一个叫 `NavigateToPose` 的节点。

### 11.2 `bt_navigator` 到底是什么？

`bt_navigator` 是 Nav2 里的 Behavior-Tree Navigator 模块 / 导航器节点。它负责实现 `NavigateToPose`、`NavigateThroughPoses` 等任务接口，并通过行为树来组织整个导航流程。它本身不是单纯“一个行为树节点集合”，而是运行行为树、接收任务、协调后续规划 / 控制 / 恢复流程的核心导航器。

### 11.3 为什么 `bt_navigator` 不等于“行为树本体”？

`bt_navigator` 不等于“行为树本体”，因为它是用行为树实现导航任务的模块，而不是“行为树这个抽象概念本身”。行为树是它用来组织流程的机制，而 `bt_navigator` 是承载这个机制、接收导航任务并驱动整条导航链运行的模块。

### 11.4 `localization` 在导航里解决的最朴素问题是什么？

`localization` 在导航里解决的最朴素问题是：机器人现在到底在哪里、朝向哪里。如果系统不知道机器人当前位姿，规划和控制就没有可靠基准。像 AMCL 这样的定位方法，就是在帮助系统持续估计机器人在地图中的位置和朝向。

### 11.5 为什么 `costmap / TF / map / localization` 先放支撑层，而不是主干箭头上？

因为它们通常不直接接收导航任务，也不直接作为主流程组织者或最终控制输出者，但会为规划、控制和恢复提供环境代价信息、坐标变换、全局地图和位姿估计，因此会持续影响主链是否能正确运行。它们不是主干上的“流程执行者”，而是主干背后的“运行条件提供者”。

### 11.6 为什么 `behavior_server` 现在先写成“提供支持”，而不是直接写成主干固定一步？

因为 `behavior_server` 更适合被理解为在受阻、失败、恢复或特定行为阶段被调用的能力提供者，而不是每次导航主链都必须按固定顺序经过的一步。它的介入通常依赖上下文，而不是像规划和控制那样先被画成一条线性主干。

---

## 12. 当天复盘

### 12.1 今天最容易混淆的概念

今天最容易混淆的概念有两个：

1. `NavigateToPose` 是什么  
2. `bt_navigator` 和“行为树”到底是什么关系

这两个点一混，就会把任务入口、流程组织者和能力提供者三类对象讲成一锅。

### 12.2 今天的主链图里哪些位置还是“先猜的”

当前主链图里，以下位置仍然属于“第一版猜测”：

- `NavigateToPose` 和 `bt_navigator` 的接口关系
- `behavior_server` 在主链中的介入条件
- `costmap`、`map`、`TF`、`localization` 各自与主链的具体依赖方式
- `planner_server` 与 `controller_server` 的更细职责边界

### 12.3 今天哪些对象已经能比较稳地放到正确层级

今天已经能比较稳地放到层级上的对象包括：

- `NavigateToPose`：任务入口层
- `bt_navigator`：流程组织层
- `planner_server`、`controller_server`、`behavior_server`：能力执行层
- `/cmd_vel`：控制输出层
- `costmap`、`TF`、`map`、`localization`：支撑层

### 12.4 明天 Day 2 要用运行时证据验证什么

明天 Day 2 需要重点验证以下几点：

1. `NavigateToPose` 是如何进入 `bt_navigator` 的
2. `bt_navigator` 是否确实处于流程组织位置
3. `planner_server`、`controller_server`、`behavior_server` 在运行时分别如何出现
4. `/cmd_vel` 是否能作为当前主链尾部锚点
5. 支撑层对象虽然不在主干箭头上，但是否真的持续影响主链行为

### 12.5 今天的最低结论

今天已经拿到了 `goal → motion` 主链的第一版骨架，但它还只是“结构图”，还没有被运行时证据真正钉住。  
明天要做的事情，不是继续加概念，而是用运行系统输出去验证这条链。

---