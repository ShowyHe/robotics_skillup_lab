# 2026-03-27 Dog12 大场景 HPA 测试 Runbook（英文地图名版）

## 1. 适用范围

本文用于 12 号狗在大场景地图 `heiniu_big_0327` 上执行以下任务：

1. 完整 bringup 底层链路  
2. 通过 Flask 加载地图并启动 localization  
3. 切换 `MODE1`、触发重定位、确认 `map -> base_link`  
4. 从开启 HPA 前开始录制 rosbag  
5. 启动 HPA，执行 `plan_path` 与 `execute_path`  
6. 记录日志、抽取关键证据、必要时执行返程测试  

---

## 2. 固定环境

- 机器：12号狗（mini6）
- ROS 版本：Humble
- shell：`zsh`
- 主项目目录：`~/Cyber_dog_mini`
- HPA 工作区：`~/Cyber_dog_mini/src/traj_devel`
- ROS Domain ID：`137`

---

## 3. 本次统一地图

本次测试统一使用英文地图名，所有涉及地图的地方必须保持一致：

- 地图名：`heiniu_big_0327`
- 地图目录：`/home/mini/Cyber_dog_mini/maps/heiniu_big_0327`
- 地图 yaml：`/home/mini/Cyber_dog_mini/maps/heiniu_big_0327/map.yaml`

注意：  
Flask 加载地图、localization 的 `map_name/map_path`、HPA 的 `source_map_yaml` 三处必须一致。  
禁止一处用旧图、一处用中文名、一处用英文名。

---

## 4. 测试前检查英文地图目录

先确认英文地图目录存在。

    cd ~/Cyber_dog_mini/maps
    ls

如果没有 `heiniu_big_0327`，先创建：

    cd ~/Cyber_dog_mini/maps
    cp -r 黑牛产业园长方形建图测试26-03-19 heiniu_big_0327
    ls

---

## 5. 清场

正式测试前先清场，防止 Flask、localization、relocalizer、底盘、雷达等旧进程残留。

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    pkill -f "python3 app.py"
    pkill -f "RobotWebApp"
    pkill -f "start_backend.sh"
    pkill -f "web_bridge_node"
    pkill -f "dlo_loc"
    pkill -f "nav_vlm"
    pkill -f "navigation2_no_amcl.launch.py"
    pkill -f "segmentation_launch.py"
    pkill -f "ground_segmentation"
    pkill -f "relocalizer"
    pkill -f "livox_ros_driver2"
    pkill -f "car_chassis_node"
    pkill -f "joystick_node"
    pkill -f "_ros2_daemon"

    sleep 2
    lsof -iTCP:5000 -sTCP:LISTEN -n -P

如果 5000 端口仍被占用，再执行：

    kill -9 $(lsof -t -iTCP:5000 -sTCP:LISTEN) 2>/dev/null || true
    sleep 1
    lsof -iTCP:5000 -sTCP:LISTEN -n -P

判定标准：  
5000 端口无残留监听。

---

## 6. 启动底层链路

### 6.1 终端A：启动雷达

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    ros2 launch livox_ros_driver2 msg_MID360_launch.py

判定标准：  
终端中出现 `Init lds lidar success` 后再继续。

### 6.2 终端B：启动底盘

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    sudo chmod 666 /dev/ttyUSB0
    sudo ip link set can0 down
    sudo ip link set can0 type can bitrate 1000000
    sudo ip link set can0 up
    ip -details link show can0

    ros2 run car_chassis car_chassis_node

### 6.3 终端C：启动手柄

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    ros2 run joystick joystick_node

### 6.4 终端D：确认底盘里程计

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    ros2 topic echo /wheel_odom --once

判定标准：  
`/wheel_odom` 能正常输出数据。

---

## 7. 启动 Flask，并加载新地图

### 7.1 终端E：启动 Flask

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini/flask_ros

    python3 app.py

### 7.2 终端F：探活 Flask

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    curl http://127.0.0.1:5000/ | head

判定标准：  
看到 HTML 内容即可。  
若出现 `curl: (23)` 且前面已有 HTML 输出，可忽略。

### 7.3 终端F：加载地图

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    curl "http://127.0.0.1:5000/api/maps/heiniu_big_0327"

---

## 8. 启动 localization

### 8.1 终端F：发启动请求

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    curl -X POST "http://127.0.0.1:5000/api/tasks/localization/start" \
      -H "Content-Type: application/json" \
      -d '{"map_name":"heiniu_big_0327","map_path":"/home/mini/Cyber_dog_mini/maps/heiniu_big_0327"}'

返回结果中会包含真实 `task_id`，需要记录下来。

### 8.2 终端F：轮询 localization 状态

将下方 `真实ID` 替换为上一步返回的 `task_id`：

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    curl "http://127.0.0.1:5000/api/tasks/真实ID/status"

判定标准：  
返回结果中出现：

- `"status":"completed"`
- `"success":true`

---

## 9. 切 MODE1、触发重定位、确认 TF

12号狗自动导航前必须切 `MODE1`。  
如果未切 `MODE1`，常见现象是有反馈但车不动。

### 9.1 终端G：切 MODE1

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    ros2 topic pub /control_mode std_msgs/msg/String "{data: 'MODE1'}" --once

### 9.2 终端G：触发重定位

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    ros2 topic pub /relocalize_trigger std_msgs/msg/Bool "{data: true}" --once

### 9.3 终端H：确认 `map -> base_link`

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    ros2 run tf2_ros tf2_echo map base_link

判定标准：

- 开头偶尔出现一次 `Invalid frame ID "map"` 可以接受
- 后面必须持续输出 `map -> base_link`

若 TF 仍未建立，再执行一次兜底流程：

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    ros2 service call /analysis_camera std_srvs/srv/Trigger "{}"
    ros2 topic pub /relocalize_trigger std_msgs/msg/Bool "{data: true}" --once

然后再次检查：

    ros2 run tf2_ros tf2_echo map base_link

---

## 10. 从开启 HPA 前开始录 rosbag

本次要求从 HPA 启动前开始录制 rosbag。  
该终端启动后保持不关闭，直到去终点/返程测试结束。

### 10.1 终端I：启动 rosbag 录制

    source /opt/ros/humble/setup.zsh
    cd ~/Cyber_dog_mini/src/traj_devel
    source install/setup.zsh
    export ROS_DOMAIN_ID=137

    RUN=run_0327_hpa_01
    ROOT=~/testing/hpa/heiniu_big_0327/$RUN
    mkdir -p "$ROOT"

    ros2 bag record -a --include-hidden-topics -o "$ROOT/rosbag_from_hpa_start"

---

## 11. 启动 HPA

### 11.1 参数说明

本次地图为大场景，不再使用旧配置 `tile_size=5.0 overlap=3.0`。  
第一轮统一使用：

- `tile_size:=25.0`
- `overlap:=5.0`

### 11.2 终端J：启动 HPA

    source /opt/ros/humble/setup.zsh
    cd ~/Cyber_dog_mini/src/traj_devel
    source install/setup.zsh
    export ROS_DOMAIN_ID=137

    RUN=run_0327_hpa_01
    ROOT=~/testing/hpa/heiniu_big_0327/$RUN
    mkdir -p "$ROOT"

    rm -rf /tmp/split_maps
    mkdir -p /tmp/split_maps

    ros2 launch multi_map_switcher hpa_planner.launch.py \
      source_map_yaml:=/home/mini/Cyber_dog_mini/maps/heiniu_big_0327/map.yaml \
      tile_size:=25.0 \
      overlap:=5.0 \
      output_dir:=/tmp/split_maps \
      use_sim_time:=false 2>&1 | tee "$ROOT/00_hpa_launch.log"

该终端保持开启。

---

## 12. 检查 HPA 是否正常起来

### 12.1 终端K：检查 service、action、切图结果

    source /opt/ros/humble/setup.zsh
    cd ~/Cyber_dog_mini/src/traj_devel
    source install/setup.zsh
    export ROS_DOMAIN_ID=137

    RUN=run_0327_hpa_01
    ROOT=~/testing/hpa/heiniu_big_0327/$RUN

    ros2 service list | grep hpa | tee "$ROOT/01_hpa_services.log"
    ros2 action list | grep hpa | tee "$ROOT/02_hpa_actions.log"
    find /tmp/split_maps | head -n 50 | tee "$ROOT/03_split_maps_head.log"
    find /tmp/split_maps | wc -l | tee "$ROOT/04_split_maps_count.log"

判定重点：

- 是否存在 `/hpa_planner/plan_path`
- 是否存在 `/hpa_executor/execute_path`
- `/tmp/split_maps` 是否已生成切图结果

若 service/action 未出现，不要急着规划，先查看 `00_hpa_launch.log`。

---

## 13. 记录起点

### 13.1 终端L：记录起点 TF

    source /opt/ros/humble/setup.zsh
    cd ~/Cyber_dog_mini/src/traj_devel
    source install/setup.zsh
    export ROS_DOMAIN_ID=137

    RUN=run_0327_hpa_01
    ROOT=~/testing/hpa/heiniu_big_0327/$RUN

    timeout 5 ros2 run tf2_ros tf2_echo map base_link 2>&1 | tee "$ROOT/05_start_tf.log"

从日志中记录稳定的：

- `start_x`
- `start_y`

---

## 14. 记录终点

### 14.1 终端M：监听 `/clicked_point`

    source /opt/ros/humble/setup.zsh
    cd ~/Cyber_dog_mini/src/traj_devel
    source install/setup.zsh
    export ROS_DOMAIN_ID=137

    RUN=run_0327_hpa_01
    ROOT=~/testing/hpa/heiniu_big_0327/$RUN

    ros2 topic echo /clicked_point --once | tee "$ROOT/06_clicked_point.log"

然后去 RViz：

1. 选择 `Publish Point`
2. 在地图上点击终点

从日志中记录：

- `goal_x`
- `goal_y`

---

## 15. 执行 plan_path

将下方 `START_X`、`START_Y`、`GOAL_X`、`GOAL_Y` 替换为实测值。

### 15.1 终端N：规划

    source /opt/ros/humble/setup.zsh
    cd ~/Cyber_dog_mini/src/traj_devel
    source install/setup.zsh
    export ROS_DOMAIN_ID=137

    RUN=run_0327_hpa_01
    ROOT=~/testing/hpa/heiniu_big_0327/$RUN

    ros2 service call /hpa_planner/plan_path multi_map_switcher/srv/PlanHpaPath \
    "{start: {header: {frame_id: 'map'}, pose: {position: {x: START_X, y: START_Y, z: 0.0}, orientation: {w: 1.0}}}, goal: {header: {frame_id: 'map'}, pose: {position: {x: GOAL_X, y: GOAL_Y, z: 0.0}, orientation: {w: 1.0}}}}" \
    2>&1 | tee "$ROOT/07_plan_to_goal.log"

### 15.2 立即抽取规划关键字段

    grep -E "success=|chunk_names=|anchor_ids=|waypoints=" "$ROOT/07_plan_to_goal.log"

重点查看：

- `success=True`
- `chunk_names=[...]`
- `anchor_ids=[...]`
- `waypoints=[...]`

---

## 16. 执行 execute_path

仅在 `plan_path` 成功后执行。

### 16.1 终端O：执行

    source /opt/ros/humble/setup.zsh
    cd ~/Cyber_dog_mini/src/traj_devel
    source install/setup.zsh
    export ROS_DOMAIN_ID=137

    RUN=run_0327_hpa_01
    ROOT=~/testing/hpa/heiniu_big_0327/$RUN

    ros2 action send_goal /hpa_executor/execute_path \
      multi_map_switcher/action/ExecuteHpaPath \
      "{goal: {header: {frame_id: 'map'}, pose: {position: {x: GOAL_X, y: GOAL_Y, z: 0.0}, orientation: {w: 1.0}}}}" \
      --feedback 2>&1 | tee "$ROOT/08_execute_to_goal.log"

### 16.2 记录结果

若成功：

    echo "result=success" | tee "$ROOT/09_result.txt"

若失败：

    echo "result=failed" | tee "$ROOT/09_result.txt"

---

## 17. 失败时立即抽取证据

### 17.1 终端K 或新终端：抽日志关键字段

    source /opt/ros/humble/setup.zsh
    cd ~/Cyber_dog_mini/src/traj_devel
    source install/setup.zsh
    export ROS_DOMAIN_ID=137

    RUN=run_0327_hpa_01
    ROOT=~/testing/hpa/heiniu_big_0327/$RUN

    grep -E "success=|chunk_names=|waypoints=|anchor_ids=|Plan succeeded|NavigateThroughPoses failed|Navigation failed|current_waypoint_index|current_chunk|maps_switched|Goal finished|ABORT|failed" \
    "$ROOT/07_plan_to_goal.log" \
    "$ROOT/08_execute_to_goal.log"

用途：

- 判断是规划失败还是执行失败
- 判断卡在第几个中间点
- 判断当前 chunk 与规划 chunk 是否一致
- 判断是否属于单 anchor 或 multi-anchor 问题

---

## 18. 去终点成功后执行返程

仅在去终点成功后执行返程测试。

### 18.1 终端P：返程规划

    source /opt/ros/humble/setup.zsh
    cd ~/Cyber_dog_mini/src/traj_devel
    source install/setup.zsh
    export ROS_DOMAIN_ID=137

    RUN=run_0327_hpa_01
    ROOT=~/testing/hpa/heiniu_big_0327/$RUN

    ros2 service call /hpa_planner/plan_path multi_map_switcher/srv/PlanHpaPath \
    "{start: {header: {frame_id: 'map'}, pose: {position: {x: GOAL_X, y: GOAL_Y, z: 0.0}, orientation: {w: 1.0}}}, goal: {header: {frame_id: 'map'}, pose: {position: {x: START_X, y: START_Y, z: 0.0}, orientation: {w: 1.0}}}}" \
    2>&1 | tee "$ROOT/10_plan_back.log"

### 18.2 终端Q：返程执行

    source /opt/ros/humble/setup.zsh
    cd ~/Cyber_dog_mini/src/traj_devel
    source install/setup.zsh
    export ROS_DOMAIN_ID=137

    RUN=run_0327_hpa_01
    ROOT=~/testing/hpa/heiniu_big_0327/$RUN

    ros2 action send_goal /hpa_executor/execute_path \
      multi_map_switcher/action/ExecuteHpaPath \
      "{goal: {header: {frame_id: 'map'}, pose: {position: {x: START_X, y: START_Y, z: 0.0}, orientation: {w: 1.0}}}}" \
      --feedback 2>&1 | tee "$ROOT/11_execute_back.log"

### 18.3 记录返程结果

若成功：

    echo "result=success" | tee "$ROOT/12_back_result.txt"

若失败：

    echo "result=failed" | tee "$ROOT/12_back_result.txt"

---

## 19. 停 rosbag 并检查 bag

### 19.1 回到 rosbag 终端停止录制

按：

    Ctrl + C

### 19.2 检查 bag 信息

    source /opt/ros/humble/setup.zsh
    export ROS_DOMAIN_ID=137

    RUN=run_0327_hpa_01
    ROOT=~/testing/hpa/heiniu_big_0327/$RUN

    ros2 bag info "$ROOT/rosbag_from_hpa_start"

---

## 20. 现场最短执行顺序

如果现场时间紧，按下面顺序执行：

1. 确认 `heiniu_big_0327` 目录存在  
2. 清场  
3. 启动雷达  
4. 启动底盘  
5. 启动手柄  
6. 验证 `/wheel_odom`  
7. 启动 Flask  
8. 加载 `heiniu_big_0327`  
9. 启动 localization  
10. 轮询到 `completed`  
11. 发布 `MODE1`  
12. 发布 `/relocalize_trigger`  
13. 确认 `map -> base_link`  
14. 开始录 rosbag  
15. 启动 HPA  
16. 检查 `/hpa_planner/plan_path` 与 `/hpa_executor/execute_path`  
17. 记录起点  
18. 记录终点  
19. 执行 `plan_path`  
20. 执行 `execute_path`  
21. 失败则抽日志  
22. 成功则返程  
23. 停 rosbag  

---

## 21. 最容易出错的点

### 21.1 地图三处未统一

必须统一为：

- `heiniu_big_0327`
- `/home/mini/Cyber_dog_mini/maps/heiniu_big_0327`
- `/home/mini/Cyber_dog_mini/maps/heiniu_big_0327/map.yaml`

### 21.2 忘记切 `MODE1`

症状：有反馈但车不动。

### 21.3 `task_id` 未替换成真实值

轮询时必须替换成启动 localization 返回的真实 `task_id`。

### 21.4 5000 端口仍被旧 Flask 占用

症状：Flask 启动失败、端口冲突、状态混乱。

### 21.5 HPA 仍引用旧图路径

禁止继续使用：

    /home/mini/Cyber_dog_mini/maps/map-0318-1718/map.yaml

本次统一使用：

    /home/mini/Cyber_dog_mini/maps/heiniu_big_0327/map.yaml

### 21.6 大图仍使用小 chunk 参数

本次第一轮统一使用：

- `tile_size:=25.0`
- `overlap:=5.0`

不要再使用 `tile_size:=5.0 overlap:=3.0`。

---

## 22. 本次测试目标

第一轮测试目标不是一次性把所有问题都解决，而是先确认以下链路成立：

1. 新地图可正常加载  
2. localization 可正常完成  
3. `map -> base_link` 可稳定建立  
4. HPA 能正常切图  
5. `/hpa_planner/plan_path` 与 `/hpa_executor/execute_path` 可正常出现  
6. 能成功完成一轮去终点规划与执行  
7. rosbag 与文本日志均完整落盘  

若第一轮已能完成上述闭环，再继续分析 chunk 切换、中间点旋转、anchor 行为及 multi-anchor 表现。

