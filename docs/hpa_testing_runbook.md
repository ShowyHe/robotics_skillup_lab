# HPA 测试标准流程（12号狗 / `~/Cyber_dog_mini/src/traj_devel`）

本文档用于规范 HPA 测试流程，覆盖以下内容：

1. 停止旧版 HPA  
2. 删除旧包编译产物  
3. 替换新包  
4. 编译新包  
5. 启动 HPA  
6. 记录起点、终点  
7. 执行 `plan_path`  
8. 执行 `execute_path`  
9. 保存日志与结果  

---

## 一、适用环境

- 机器：12号狗
- shell：`zsh`
- 工作区：`~/Cyber_dog_mini/src/traj_devel`
- ROS 发行版：Humble
- 包名：`multi_map_switcher`
- HPA 源地图：

    /home/mini/Cyber_dog_mini/maps/map-0318-1718/map.yaml

- 默认 Domain ID：

    137

---

## 二、测试前原则

### 1. 只删除 HPA 包自己的旧产物
不要删除整个工作区的 `build/` 和 `install/`，只删除：

    build/multi_map_switcher
    install/multi_map_switcher

### 2. 替换新包时，保证工作区里只有一个生效包
最终应保证目录结构是：

    ~/Cyber_dog_mini/src/traj_devel/multi_map_switcher/package.xml
    ~/Cyber_dog_mini/src/traj_devel/multi_map_switcher/CMakeLists.txt
    ~/Cyber_dog_mini/src/traj_devel/multi_map_switcher/launch/hpa_planner.launch.py

不要出现“多包一层”或“同名旧包和新包共存”导致编译错包。

### 3. 每次换包后，都要重新编译并重新 source
旧终端不会自动变成新代码，必须：

    source install/setup.zsh

---

## 三、停止旧版 HPA

如果已有 HPA 终端在运行，先在对应终端按：

    Ctrl + C

只关 HPA，不要顺手把正常导航也关了。

---

## 四、替换新包

以下流程假设你手里有一个新的 zip，例如：

    ~/Cyber_dog_mini/src/traj_devel/multi_map_switcher0327.zip

### 1. 进入工作区

    source /opt/ros/humble/setup.zsh
    cd ~/Cyber_dog_mini/src/traj_devel

### 2. 备份旧包并删除旧目录

    mv multi_map_switcher multi_map_switcher_backup_$(date +%Y%m%d_%H%M%S) 2>/dev/null || true
    rm -rf multi_map_switcher

### 3. 解压新包到临时目录

    mkdir -p /tmp/mm_unpack
    rm -rf /tmp/mm_unpack/*
    unzip -q ~/Cyber_dog_mini/src/traj_devel/multi_map_switcher0327.zip -d /tmp/mm_unpack

### 4. 复制新包到工作区

如果 zip 顶层结构是：

    multi_map_switcher/
      package.xml
      CMakeLists.txt
      launch/
      src/

则执行：

    cp -r /tmp/mm_unpack/multi_map_switcher ./multi_map_switcher

### 5. 检查包结构是否正确

    test -f ~/Cyber_dog_mini/src/traj_devel/multi_map_switcher/package.xml && echo "package.xml OK"
    test -f ~/Cyber_dog_mini/src/traj_devel/multi_map_switcher/launch/hpa_planner.launch.py && echo "launch OK"

如果这里没有输出 `OK`，不要继续编译，说明新包目录结构有问题。

---

## 五、删除旧编译产物并重新编译

### 1. 删除旧产物

    cd ~/Cyber_dog_mini/src/traj_devel
    rm -rf build/multi_map_switcher
    rm -rf install/multi_map_switcher

### 2. 编译 HPA 包

    source /opt/ros/humble/setup.zsh
    cd ~/Cyber_dog_mini/src/traj_devel
    colcon build --packages-select multi_map_switcher --symlink-install

### 3. 重新 source 工作区

    source install/setup.zsh

### 4. 检查包是否安装成功

    ros2 pkg prefix multi_map_switcher
    ros2 pkg executables multi_map_switcher

正常应至少能看到类似：

    multi_map_switcher hpa_planner_node
    multi_map_switcher hpa_executor_node
    multi_map_switcher map_splitter_node
    multi_map_switcher multi_map_manager_node

### 5. 检查 launch 文件是否进入 install

    ls -lah ~/Cyber_dog_mini/src/traj_devel/install/multi_map_switcher/share/multi_map_switcher/launch

正常应能看到：

    hpa_planner.launch.py

如果这里没有 `hpa_planner.launch.py`，说明新包安装有问题，HPA 启动不了。

---

## 六、日志目录规范

建议每次测试单独建一个目录，例如：

    ~/testing/hpa/test0327
    ~/testing/hpa/test4
    ~/testing/hpa/test_through_candidate_0327

先创建目录：

    mkdir -p ~/testing/hpa/本次测试名

建议至少记录以下几类日志：

- `00_hpa_launch.log`
- `01_plan_to_goal.log`
- `02_execute_to_goal.log`
- `02_execute_to_goal_result.txt`

如果还要返程：

- `03_plan_back.log`
- `04_execute_back.log`
- `04_execute_back_result.txt`

---

## 七、启动 HPA

### 1. 清理旧切图目录

    rm -rf /tmp/split_maps
    mkdir -p /tmp/split_maps

### 2. 启动 HPA，并记录日志

    source /opt/ros/humble/setup.zsh
    cd ~/Cyber_dog_mini/src/traj_devel
    source install/setup.zsh
    export ROS_DOMAIN_ID=137

    ros2 launch multi_map_switcher hpa_planner.launch.py \
      source_map_yaml:=/home/mini/Cyber_dog_mini/maps/map-0318-1718/map.yaml \
      tile_size:=5.0 \
      overlap:=3.0 \
      output_dir:=/tmp/split_maps \
      use_sim_time:=false 2>&1 | tee ~/testing/hpa/本次测试名/00_hpa_launch.log

这个终端保持开启，不要关闭。

---

## 八、获取起点

### 方法：读取机器人当前 TF

    source /opt/ros/humble/setup.zsh
    cd ~/Cyber_dog_mini/src/traj_devel
    source install/setup.zsh
    export ROS_DOMAIN_ID=137

    ros2 run tf2_ros tf2_echo map base_link

看输出里的：

    Translation: [x, y, 0.000]

记录当前稳定的 `x` 和 `y`，作为起点。

例如：

    Translation: [-1.901, -2.150, 0.000]

则起点记为：

    start_x = -1.901
    start_y = -2.150

看两三组稳定数据后按：

    Ctrl + C

退出。

---

## 九、获取终点

### 方法：在 RViz 中用 `Publish Point` 打点

先在终端监听：

    source /opt/ros/humble/setup.zsh
    cd ~/Cyber_dog_mini/src/traj_devel
    source install/setup.zsh
    export ROS_DOMAIN_ID=137

    ros2 topic echo /clicked_point --once

然后到 RViz：

- 点 `Publish Point`
- 在地图上点目标位置

终端会输出：

    point:
      x: 6.420758247375488
      y: 2.2746665477752686
      z: ...

只取：

- `goal_x`
- `goal_y`

例如：

    goal_x = 6.420758247375488
    goal_y = 2.2746665477752686

---

## 十、执行 `plan_path`

### 命令模板

    source /opt/ros/humble/setup.zsh
    cd ~/Cyber_dog_mini/src/traj_devel
    source install/setup.zsh
    export ROS_DOMAIN_ID=137

    ros2 service call /hpa_planner/plan_path multi_map_switcher/srv/PlanHpaPath \
    "{start: {header: {frame_id: 'map'}, pose: {position: {x: START_X, y: START_Y, z: 0.0}, orientation: {w: 1.0}}}, goal: {header: {frame_id: 'map'}, pose: {position: {x: GOAL_X, y: GOAL_Y, z: 0.0}, orientation: {w: 1.0}}}}" \
    2>&1 | tee ~/testing/hpa/本次测试名/01_plan_to_goal.log

把 `START_X/START_Y/GOAL_X/GOAL_Y` 替换成实际值。

### 规划结果重点看什么

重点看返回里是否有：

- `success=True`
- `chunk_names=[...]`
- `anchor_ids=[...]`
- `waypoints=[...]`

### 快速抽重点

    grep -E "success=|chunk_names=|anchor_ids=|waypoints=" \
    ~/testing/hpa/本次测试名/01_plan_to_goal.log

### 如何判断是不是 multi-anchor 样本

#### 单 anchor
如果看到：

    chunk_names=[start, mid1, goal]
    anchor_ids=[-1, a1, -1]

说明只有一个中间 anchor。

#### multi-anchor
如果看到：

    chunk_names=[start, mid1, mid2, goal]
    anchor_ids=[-1, a1, a2, -1]

说明至少有两个中间 anchor，这是标准 multi-anchor 样本。

---

## 十一、执行 `execute_path`

只有在 `plan_path` 成功时才继续。

### 命令模板

    source /opt/ros/humble/setup.zsh
    cd ~/Cyber_dog_mini/src/traj_devel
    source install/setup.zsh
    export ROS_DOMAIN_ID=137

    ros2 action send_goal /hpa_executor/execute_path \
      multi_map_switcher/action/ExecuteHpaPath \
      "{goal: {header: {frame_id: 'map'}, pose: {position: {x: GOAL_X, y: GOAL_Y, z: 0.0}, orientation: {w: 1.0}}}}" \
      --feedback 2>&1 | tee ~/testing/hpa/本次测试名/02_execute_to_goal.log

把 `GOAL_X/GOAL_Y` 换成实际终点。

### 执行结果重点看什么

重点看最后是否为：

    Goal finished with status: SUCCEEDED

如果失败，常见会看到：

- `Navigation failed ...`
- `NavigateThroughPoses failed ...`
- `ABORTED`

### 结果文件

成功：

    echo "result=success" | tee ~/testing/hpa/本次测试名/02_execute_to_goal_result.txt

失败：

    echo "result=failed" | tee ~/testing/hpa/本次测试名/02_execute_to_goal_result.txt

---

## 十二、失败后如何快速抽关键证据

如果 `execute_path` 失败，不要立刻继续返程，先抽关键日志：

    grep -E "success=|chunk_names=|waypoints=|anchor_ids=|Plan succeeded|NavigateThroughPoses failed|Navigation failed|current_waypoint_index|current_chunk|maps_switched|Goal finished|ABORT|failed" \
    ~/testing/hpa/本次测试名/01_plan_to_goal.log \
    ~/testing/hpa/本次测试名/02_execute_to_goal.log

通过这些信息可以快速判断：

- 是规划失败还是执行失败
- 卡在第几个中间点
- `current_chunk` 是否和规划的 chunk 一致
- 是单 anchor 问题还是 multi-anchor 问题

---

## 十三、返程流程（只在去终点成功后执行）

如果去终点已经成功，才做返程。

### 1. 返程规划

    source /opt/ros/humble/setup.zsh
    cd ~/Cyber_dog_mini/src/traj_devel
    source install/setup.zsh
    export ROS_DOMAIN_ID=137

    ros2 service call /hpa_planner/plan_path multi_map_switcher/srv/PlanHpaPath \
    "{start: {header: {frame_id: 'map'}, pose: {position: {x: GOAL_X, y: GOAL_Y, z: 0.0}, orientation: {w: 1.0}}}, goal: {header: {frame_id: 'map'}, pose: {position: {x: START_X, y: START_Y, z: 0.0}, orientation: {w: 1.0}}}}" \
    2>&1 | tee ~/testing/hpa/本次测试名/03_plan_back.log

### 2. 返程执行

    source /opt/ros/humble/setup.zsh
    cd ~/Cyber_dog_mini/src/traj_devel
    source install/setup.zsh
    export ROS_DOMAIN_ID=137

    ros2 action send_goal /hpa_executor/execute_path \
      multi_map_switcher/action/ExecuteHpaPath \
      "{goal: {header: {frame_id: 'map'}, pose: {position: {x: START_X, y: START_Y, z: 0.0}, orientation: {w: 1.0}}}}" \
      --feedback 2>&1 | tee ~/testing/hpa/本次测试名/04_execute_back.log

### 3. 返程结果文件

成功：

    echo "result=success" | tee ~/testing/hpa/本次测试名/04_execute_back_result.txt

失败：

    echo "result=failed" | tee ~/testing/hpa/本次测试名/04_execute_back_result.txt

---

## 十四、一次完整测试的最短执行顺序

### A. 替换并编译新包
1. 关旧 HPA  
2. 备份旧 `multi_map_switcher`  
3. 解压新 zip 到 `multi_map_switcher`  
4. 删除 `build/multi_map_switcher` 和 `install/multi_map_switcher`  
5. `colcon build --packages-select multi_map_switcher --symlink-install`  
6. `source install/setup.zsh`  
7. 确认 `launch/hpa_planner.launch.py` 已进入 install  

### B. 启动 HPA
1. 清 `/tmp/split_maps`  
2. 启动 `hpa_planner.launch.py`  
3. 日志记 `00_hpa_launch.log`  

### C. 记录点位
1. 用 `tf2_echo map base_link` 记起点  
2. 用 `/clicked_point` 记终点  

### D. 去终点
1. `plan_path` 记 `01_plan_to_goal.log`  
2. 抽 `chunk_names / anchor_ids` 判断样本类型  
3. `execute_path` 记 `02_execute_to_goal.log`  
4. 写 `02_execute_to_goal_result.txt`  

### E. 成功后返程
1. `03_plan_back.log`  
2. `04_execute_back.log`  
3. `04_execute_back_result.txt`  

---

## 十五、推荐补充检查项

### 1. 检查当前代码是 `to` 版还是 `through` 版

    cd ~/Cyber_dog_mini/src/traj_devel
    grep -R "NavigateThroughPoses" -n . --include="hpa_executor_node.cpp"
    grep -R "NavigateToPose" -n . --include="hpa_executor_node.cpp"

### 2. 检查安装后的 launch 文件是否存在

    ls -lah ~/Cyber_dog_mini/src/traj_devel/install/multi_map_switcher/share/multi_map_switcher/launch

### 3. 检查当前包是否被 ROS 正确识别

    ros2 pkg prefix multi_map_switcher
    ros2 pkg executables multi_map_switcher

---

## 十六、当前已验证出的经验

### 1. 单个中间点时
- `NavigateToPose` 更稳
- `NavigateThroughPoses([单点])` 容易失败

### 2. 当前 0327 版本
- 中间点逐个 `NavigateToPose`
- 成功率更高
- 但跨 chunk 时更容易出现“停一下、原地转一下、再走”的 stop-go 现象

### 3. 判断 through 是否值得继续验证
- 如果目标只是看“跨 chunk 是否更平滑”，单 anchor 样本也能测
- 如果目标是验证真正的 multi-anchor through，必须让 `chunk_names` 至少有 4 个，且 `anchor_ids` 至少有 2 个不是 `-1`

---
