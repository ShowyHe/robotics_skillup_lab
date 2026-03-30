# S03 Day 3｜三大 Server 的实现入口、插件装载与文件机制总整理

## 1. 今日定位

Sprint 3 的主题不是再讲“谁在调度”，而是把 `planner_server / controller_server / behavior_server` 的职责边界继续推进到**实现入口层**。

S03 Day 1 解决的是静态边界。  
S03 Day 2 解决的是运行时证据。  
S03 Day 3 的任务是：

1. 找到三大 server 各自属于哪个包、哪个可执行体  
2. 找到它们的头文件、核心库、plugin 描述文件等实现入口线索  
3. 把 `bt_navigator -> BT XML -> server -> plugin` 这条链条，和 `.hpp / .cpp / .so / package.xml / CMakeLists.txt / plugin XML / YAML` 这些文件角色串起来

今天不要求把算法实现细节全部读完，但要求先把“代码文件层、插件层、构建层、运行层”之间的关系讲顺。

---

## 2. 今日核心问题

今天只回答六个问题：

1. `.hpp / .cpp / .so` 在 Nav2 里分别扮演什么角色  
2. `planner_server / controller_server / behavior_server` 各自的实现入口在哪  
3. plugin 到底是什么，pluginlib 和 plugin XML 又分别负责什么  
4. `GridBased / FollowPath / progress_checker` 这些名字，到底是配置名、实例名还是类名  
5. `BT XML` 和 `Plugin XML` 分别在系统中管什么  
6. `package.xml / CMakeLists.txt` 与运行时的 server/plugin 关系是什么

---

## 3. 包与可执行体映射

通过系统查询，当前四个关键对象已经能明确落到实际包和可执行体上：

- `nav2_planner -> planner_server`
- `nav2_controller -> controller_server`
- `nav2_behaviors -> behavior_server`
- `nav2_bt_navigator -> bt_navigator`

这说明：

- `planner_server` 属于 `nav2_planner`
- `controller_server` 属于 `nav2_controller`
- `behavior_server` 属于 `nav2_behaviors`
- `bt_navigator` 属于 `nav2_bt_navigator`

也就是说，前面口头说的“三大 server”和“总调度者”，不是抽象名词，而是明确对应到实际 ROS 2 包与可执行体。

---

## 4. `.hpp / .cpp / .so` 到底各是什么

## 4.1 `.hpp` 是什么

`.hpp` 是 **C++ 头文件**。它通常放这些内容：

- 类声明
- 函数声明
- 结构体声明
- 接口
- 抽象基类

它解决的是：

> 这个东西叫什么、长什么样、别人该怎么调用它

所以你在安装目录里看到：

- `planner_server.hpp`
- `controller_server.hpp`
- `behavior_server.hpp`
- `bt_navigator.hpp`

可以把它们当成最稳的“实现入口线索”。

---

## 4.2 `.cpp` 是什么

`.cpp` 是 **C++ 实现文件**。通常放：

- 类方法实现
- 具体逻辑
- 插件导出代码
- `main()` 或可执行体逻辑

它解决的是：

> 这个类具体怎么干活

所以：

- `.hpp` 更偏“轮廓 / 接口 / 声明”
- `.cpp` 更偏“代码逻辑 / 具体实现”

---

## 4.3 `.so` 是什么

`.so` 是 Linux 下常见的 **共享库 / 动态库文件**。

更准确地说：

- **动态库 / 共享库**：是一种编译产物类型
- **`.so` 文件**：这种产物在 Linux 上最常见的文件形式

所以：

- 说 `libnav2_spin_behavior.so` 是一个文件，对
- 说它是一个动态库，也对

它不是文件夹，不是“放了很多文件的包”，通常就是**一个二进制文件**。  
但这个二进制文件里，包含了编译、链接后的类实现，可供系统运行时加载。

在 Nav2 里，这很重要，因为 planner / controller / behavior 都希望做到：

- 算法可替换
- 运行时可切换
- server 不用改，只换 plugin

这就是 plugin 往往要被编译成 `.so` 的原因。

---

## 5. 三个核心 server 的实现入口线索

## 5.1 planner_server

安装目录里已经定位到：

- `include/nav2_planner/planner_server.hpp`
- `lib/libplanner_server_core.so`
- `lib/nav2_planner/planner_server`

这说明 `planner_server` 至少有三层可追入口：

1. 头文件入口：`planner_server.hpp`
2. 核心库：`libplanner_server_core.so`
3. 可执行体：`nav2_planner/planner_server`

---

## 5.2 controller_server

安装目录里已经定位到：

- `include/nav2_controller/controller_server.hpp`
- `lib/libcontroller_server_core.so`
- `lib/nav2_controller/controller_server`

此外还看到：

- `include/nav2_controller/plugins/...`
- `share/nav2_controller/plugins.xml`

这说明 `controller_server` 这边不仅有 server 自己的入口，还已经把 controller 侧插件结构暴露出来了。

---

## 5.3 behavior_server

安装目录里已经定位到：

- `include/nav2_behaviors/behavior_server.hpp`
- `lib/libbehavior_server_core.so`
- `lib/nav2_behaviors/behavior_server`

同时还定位到：

- `include/nav2_behaviors/plugins/spin.hpp`
- `include/nav2_behaviors/plugins/wait.hpp`
- `include/nav2_behaviors/plugins/back_up.hpp`

以及：

- `lib/libnav2_spin_behavior.so`
- `lib/libnav2_wait_behavior.so`
- `lib/libnav2_back_up_behavior.so`

这说明 `behavior_server` 不是一个“硬编码死写”的单一模块，而是：

- 有自己的 server 外壳
- 下面再挂多个行为实现插件

---

## 6. plugin、pluginlib、plugin XML 到底是什么

## 6.1 plugin 是什么

在 Nav2 / ROS2 这里，plugin 更准确的定义是：

> 一个实现了统一基类接口，并且可以在运行时被系统加载的功能模块

例如：

- planner plugin
- controller plugin
- behavior plugin
- BT plugin

---

## 6.2 为什么要插件

因为系统不想把算法写死。

比如：

- planner 今天可以用 `NavfnPlanner`
- 明天可以换 `SmacPlanner`

- controller 今天可以用 `DWB`
- 明天可以换别的 controller

- behavior 也可以按需要扩展

这样就能做到：

- server 不用改
- BT 大框架不用改
- 只换配置和 plugin 实现即可

---

## 6.3 pluginlib 是什么

`pluginlib` 可以粗暴理解成：

> ROS2 的插件加载机制 / 加载器

它负责：

- 找到插件描述信息
- 识别插件属于哪个基类接口
- 打开对应 `.so`
- 把里面的类加载出来
- 创建插件对象给 server 使用

关系可以这样记：

- plugin：被加载的功能模块
- pluginlib：负责发现和加载这些模块的机制

---

## 6.4 插件 XML 是什么

插件 XML 不是代码实现，也不是头文件。  
它更像是：

> “插件注册表 / 说明书”

它告诉系统：

- 这个插件在哪个库里
- 真实类名是什么
- 它实现的是哪个基类接口

所以三层关系是：

- `.hpp / .cpp`：写类和实现
- `.so`：编译后的可加载产物
- `plugin XML`：告诉加载器怎么找到并识别这个插件

---

## 7. plugin 描述文件线索

当前系统里已经定位到这些 plugin 描述文件：

### planner plugin
- `/opt/ros/humble/share/nav2_navfn_planner/global_planner_plugin.xml`

### controller plugin
- `/opt/ros/humble/share/dwb_core/local_planner_plugin.xml`

### behavior plugin
- `/opt/ros/humble/share/nav2_behaviors/behavior_plugin.xml`

这说明：

- `nav2_navfn_planner/NavfnPlanner`
- `dwb_core::DWBLocalPlanner`
- `spin / wait / backup` 等行为插件

都不是“只有参数字符串的概念”，而是有实际 plugin 描述文件可追溯。

---

## 8. “名字层”和“真实类层”必须分开

这是今天最容易混的点。

### 8.1 planner 侧
例如：

- `GridBased`：配置名 / plugin ID / 实例名
- `nav2_navfn_planner/NavfnPlanner`：真实 plugin 类型

这两者不是一个东西。

`GridBased` 是你在配置里给这个 planner 对象起的名字；  
`NavfnPlanner` 才是实际装进去的实现类型。

---

### 8.2 controller 侧
例如：

- `FollowPath`：controller plugin 的配置 ID
- `dwb_core::DWBLocalPlanner`：controller plugin 的真实类型

但 `controller_server` 下面不只挂 controller plugin，还会挂：

- `general_goal_checker`
- `progress_checker`

所以 controller 这边不是“只有一个控制器插件”，而是一套组合：

- controller plugin
- goal checker plugin
- progress checker plugin

---

### 8.3 行为树里为什么写的是 ID

在 BT XML 里常见的是：

- `planner_id="GridBased"`
- `controller_id="FollowPath"`

这里写的是 **实例名 / 配置 ID**，不是直接写底层类名。  
server 会根据配置，把这些 ID 映射到真实 plugin 类型。

所以一句话记住：

**`GridBased / FollowPath` 是 ID，`NavfnPlanner / DWBLocalPlanner` 是类型。**

---

## 9. BT Navigator 和 BT XML 到底是什么

## 9.1 bt_navigator 是什么

`bt_navigator` 是上层任务调度器。  
它负责接：

- `NavigateToPose`
- `NavigateThroughPoses`

然后按行为树来组织导航流程。

安装目录里已经定位到：

- `bt_navigator.hpp`
- `navigator.hpp`
- `navigate_to_pose.hpp`
- `bt_navigator` 可执行体
- `libbt_navigator_core.so`

---

## 9.2 BT XML 是什么

BT XML 不是算法实现文件，而是：

> 流程图文件

它描述的是：

- 先规划还是先控制
- 失败怎么恢复
- 重试几次
- 是否清 costmap
- 是否 spin / wait / backup

所以：

- `BT XML = 流程`
- `.cpp / .hpp = 实现`

当前安装目录里已经定位到多份默认 BT XML，例如：

- `navigate_to_pose_w_replanning_and_recovery.xml`
- `navigate_through_poses_w_replanning_and_recovery.xml`

这说明默认行为树本身也是安装在系统里的真实文件，不是运行时凭空生成的。

---

## 10. 三个核心 server 到底怎么分工

## 10.1 Planner Server
负责：

- 接收规划请求
- 根据 `planner_id` 找 planner plugin
- 输出全局路径

它不直接负责：

- 速度控制
- 行为恢复
- 上层流程组织

---

## 10.2 Controller Server
负责：

- 接收 path
- 根据 `controller_id` 选 controller plugin
- 根据 `goal_checker_id` 选 goal checker
- 根据 `progress_checker_id` 选 progress checker
- 输出控制命令

所以它不是“只有一个 controller”，而是一套组合系统。

---

## 10.3 Behavior Server
负责行为 / 恢复动作，例如：

- `Spin`
- `BackUp`
- `Wait`
- `DriveOnHeading`
- `AssistedTeleop`

注意：

系统支持哪些 behavior，和当前 BT XML 实际调用哪些 behavior，不是一回事。

也就是说：

- `behavior_server` 能加载很多 behavior
- 但当前这份 BT XML 不一定都用到

---

## 11. package.xml 和 CMakeLists.txt 到底干嘛

## 11.1 package.xml
它是包的“身份证 + 依赖说明 + 导出信息”。

通常放：

- 包名
- 版本
- 维护者
- 许可证
- 依赖
- 导出信息

它回答的是：

> 这个包是谁，需要依赖谁，向外暴露什么

---

## 11.2 CMakeLists.txt
它决定：

- 这个包怎么编译
- 怎么链接
- 怎么安装
- 生成哪些库 / 可执行文件
- 是否导出插件 XML

可以粗暴理解为：

> `CMakeLists.txt = 构建脚本`

它解决的是：

> 这些 `.hpp / .cpp` 最后怎么变成可执行体、`.so` 和可安装资源

---

## 12. 把整个 Nav2 运行链串起来

现在可以把前面所有东西压成一条完整链：

### 第一步
用户发导航目标，例如 `NavigateToPose`

### 第二步
`bt_navigator` 接到任务，并加载某棵 BT XML，作为导航流程图

### 第三步
BT XML 开始 tick，例如：

- 先 `ComputePathToPose`
- 再 `FollowPath`
- 失败后进入恢复子树

### 第四步
`ComputePathToPose` 节点带着 `planner_id`
例如：

- `planner_id="GridBased"`

这里的 `GridBased` 不是类名，而是 planner 的实例名 / 配置 ID

### 第五步
`planner_server` 根据名字找插件映射：

- `GridBased -> nav2_navfn_planner/NavfnPlanner`

然后由 plugin 生成 path

### 第六步
`FollowPath` 节点带着一组 ID，例如：

- `controller_id="FollowPath"`
- `goal_checker_id="general_goal_checker"`
- `progress_checker_id="progress_checker"`

### 第七步
`controller_server` 根据这些 ID 找对应插件组合：

- controller plugin
- goal checker plugin
- progress checker plugin

然后一起参与路径跟踪与控制输出

### 第八步
如果主线失败，行为树切到恢复子树

例如：

- `Spin`
- `Wait`
- `BackUp`

这些 BT node 会去调用 `behavior_server` 对应行为插件

### 第九步
behavior plugin 也是由：

- `.hpp + .cpp` 实现
- 编译成 `.so`
- 再由 pluginlib 加载

---

## 13. 今日阶段性结论

今天至少确认了：

1. `.hpp` 是头文件入口，`.cpp` 是实现文件，`.so` 是可运行时加载的动态库产物
2. 三个核心 server 都已经能定位到包、可执行体、头文件、核心库
3. plugin 不是纯参数字符串，而是有独立 plugin 描述文件和实现库
4. `GridBased / FollowPath` 这类名字是配置 ID，不是 plugin 真实类型
5. `BT XML` 管流程，`Plugin XML` 管插件注册，它们都叫 XML，但完全不是一个层级
6. `package.xml` 管包身份和依赖，`CMakeLists.txt` 管构建与安装
7. `bt_navigator -> BT XML -> server -> plugin` 这条链，现在已经能从“文件层 + 配置层 + 运行层”三层同时解释

---

## 14. 今日一句话收口

S03 Day 3 的核心不是读懂算法细节，而是把 `planner_server / controller_server / behavior_server / bt_navigator` 从“职责边界”继续推进到“文件机制 + 实现入口 + 插件装载”这三层：  
现在已经能把 `.hpp / .cpp / .so / plugin XML / BT XML / package.xml / CMakeLists.txt` 全部放到 Nav2 主链里解释清楚。

---

## 15. 明天最自然接什么

如果继续按主线推进，S03 Day 4 最自然要做的是：

**从今天的“文件机制 + 实现入口”继续往前，做一个最小修改 / 对照，验证 server 不变时切换 plugin，系统行为会变化。**

也就是正式进入 Sprint 3 Day 4 的“最小变量实验”阶段。