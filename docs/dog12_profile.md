# 12号狗子档案（CyberDog Mini）
**建议文件路径**：`docs/dogs/dog12_profile.md`

## 1. 文档定位

本文档记录 **12号狗子** 当前已验证通过的环境信息、启动口径、地图信息、控制模式口径、重定位兜底口径、RViz 使用方式与已知坑点。

目标：

1. 快速接手这台狗，不再靠记忆试错  
2. 固化这台狗已经验证通过的最小闭环  
3. 把这台狗的个体差异与总流程分开管理  

---

## 2. 基本身份信息

### 2.1 当前已记录信息

- 实机代号：**12号狗子**
- 当前主机显示名：`mini6`
- 项目目录：`~/Cyber_dog_mini`
- ROS 版本：**Humble**
- 当前 `ROS_DOMAIN_ID`：**137**

### 2.2 首次登录建议先核对

    source /opt/ros/humble/setup.bash
    source ~/Cyber_dog_mini/install/setup.bash
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    hostname
    whoami
    pwd
    echo $ROS_DOMAIN_ID
    git branch --show-current
    git log -1 --oneline

核对目的：

1. 确认当前机器是不是目标狗  
2. 确认项目目录是否正确  
3. 确认代码分支与版本  
4. 确认 `ROS_DOMAIN_ID` 与现场一致  

---

## 3. 当前硬件与链路信息

### 3.1 雷达

- 雷达型号：**Livox MID360**
- 雷达 IP：**`192.168.1.109`**

启动命令：

    source /opt/ros/humble/setup.bash
    source ~/Cyber_dog_mini/install/setup.bash
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    ros2 launch livox_ros_driver2 msg_MID360_launch.py

成功标准：

- 日志出现 `Init lds lidar success`

---

### 3.2 底盘

- 最终底盘控制口：**`/ctl/cmd_vel`**
- 底盘里程计：**`/wheel_odom`**
- 底盘节点：`car_chassis_node`

启动前准备：

    source /opt/ros/humble/setup.bash
    source ~/Cyber_dog_mini/install/setup.bash
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    sudo chmod 666 /dev/ttyUSB0
    sudo ip link set can0 down
    sudo ip link set can0 type can bitrate 1000000
    sudo ip link set can0 up
    ip -details link show can0

启动命令：

    source /opt/ros/humble/setup.bash
    source ~/Cyber_dog_mini/install/setup.bash
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    ros2 run car_chassis car_chassis_node

---

### 3.3 手柄与控制仲裁

- 手柄节点：`joystick_node`
- 控制仲裁节点：`joystick_vel_ctl_node`

启动命令：

    source /opt/ros/humble/setup.bash
    source ~/Cyber_dog_mini/install/setup.bash
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    ros2 run joystick joystick_node

---

## 4. 当前地图与定位信息

### 4.1 本次已验证地图

- 地图名：**`map-0318-1718`**
- 地图目录：

    home/mini/Cyber_dog_mini/maps/map-0318-1718

### 4.2 定位结论

本机已验证：

- 地图可加载  
- localization 可完成  
- relocalize 可成功触发  
- `map -> base_link` 可建立  

### 4.3 地图类型说明

当前 RViz 中主要显示的是：

- 点云地图
- TF
- Path
- 机器人位姿

不是标准黑白二维栅格底图。  
因此：

- **不是地图没导入**
- 当前导入的是 **点云地图 / PCD 地图**

---

## 5. 当前控制模式口径（最关键）

## 5.1 根因结论

这台狗自动导航是否能真正放行到底盘，不取决于 Nav2 是否启动，而取决于：

- `joystick_vel_ctl_node`
- `run_mode`
- `/control_mode`

### 5.2 源码已确认的模式字符串

`/control_mode` 只识别：

- `MODE1`
- `RUN`
- `STOP`

### 5.3 本机当前已验证结论

对于 **12号狗子**，要让自动导航速度从 `/cmd_vel_smoothed` 真正放行到底盘，必须先切到：

    MODE1

执行命令：

    source /opt/ros/humble/setup.bash
    source ~/Cyber_dog_mini/install/setup.bash
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    ros2 topic pub /control_mode std_msgs/msg/String "{data: 'MODE1'}" --once

### 5.4 结论解释

当前控制逻辑为：

1. `/cmd_vel_smoothed` 只缓存自动导航速度  
2. 真正往 `/ctl/cmd_vel` 发速度的是 `timer_callback()`  
3. `timer_callback()` 是否放行自动速度，取决于 `joystick_.run_mode`  
4. **`MODE1` 是当前已验证通过的自动导航放行模式**

**因此：导航前必须切 `MODE1`。**

---

## 6. Flask / 后端口径

### 6.1 只能保留一个 Flask 实例

如果同时起多个后端，会出现：

- `Port 5000 is in use`
- MQTT client id 冲突
- 控制模式 / 任务状态混乱

### 6.2 清场命令

    source /opt/ros/humble/setup.bash
    source ~/Cyber_dog_mini/install/setup.bash
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    pkill -f "python3 app.py"
    pkill -f "RobotWebApp"
    pkill -f "start_backend.sh"
    sleep 2
    lsof -iTCP:5000 -sTCP:LISTEN -n -P

成功标准：

- `lsof` 无输出

### 6.3 启动命令

    source /opt/ros/humble/setup.bash
    source ~/Cyber_dog_mini/install/setup.bash
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    cd flask_ros
    python3 app.py

### 6.4 探活命令

    source /opt/ros/humble/setup.bash
    source ~/Cyber_dog_mini/install/setup.bash
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    curl http://127.0.0.1:5000/ | head

说明：

- 出现 HTML 即说明后端正常
- `curl: (23) Failed writing body` 在 `| head` 场景下可忽略，不是后端故障

---

## 7. 12号狗子当前已验证成功流程（最小闭环）

## 7.1 启动雷达

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    ros2 launch livox_ros_driver2 msg_MID360_launch.py

---

## 7.2 启动底盘

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

---

## 7.3 启动手柄

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    ros2 run joystick joystick_node

---

## 7.4 验证底盘里程计

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    ros2 topic echo /wheel_odom --once

---

## 7.5 启动 Flask

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    cd flask_ros
    python3 app.py

---

## 7.6 加载地图

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    curl "http://127.0.0.1:5000/api/maps/map-0318-1718"

成功标准：

- 返回 `{"success":true ... }`

---

## 7.7 启动 localization

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    curl -X POST "http://127.0.0.1:5000/api/tasks/localization/start" \
      -H "Content-Type: application/json" \
      -d '{"map_name":"map-0318-1718","map_path":"/home/mini/Cyber_dog_mini/maps/map-0318-1718"}'

记录返回的真实 `task_id`。

---

## 7.8 轮询 localization 状态直到 completed

假设返回的 task id 为 `abcd1234`，则执行：

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    curl "http://127.0.0.1:5000/api/tasks/abcd1234/status"

成功标准：

- `"status":"completed"`
- `"success":true`

---

## 7.9 重定位推荐口径：优先走 ROS 直连兜底

### 原因

本机已验证：

- `/api/navigation/relocalize` **有时不稳定**
- localization completed 后，ROS 侧 relocalizer 节点和服务可能已经存在，但 Flask 接口未必能稳定调用

因此本机的推荐口径改为：

**localization 完成后，优先走 ROS 直连兜底。**

### 先切自动模式

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    ros2 topic pub /control_mode std_msgs/msg/String "{data: 'MODE1'}" --once

### 再触发重定位

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    ros2 topic pub /relocalize_trigger std_msgs/msg/Bool "{data: true}" --once

### 再确认 TF

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    ros2 run tf2_ros tf2_echo map base_link

成功标准：

- 短暂出现一次 `Invalid frame ID "map"` 可以接受
- 后续必须开始持续输出 `map -> base_link` 的 transform

### 如果仍未立起 `map`

再补一刀视觉服务：

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    ros2 service call /analysis_camera std_srvs/srv/Trigger "{}"
    ros2 topic pub /relocalize_trigger std_msgs/msg/Bool "{data: true}" --once

然后再次确认：

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    ros2 run tf2_ros tf2_echo map base_link

---

## 7.10 发送导航目标（CLI）

前提：

1. localization 已 completed  
2. `map -> base_link` 已建立  
3. 已切 `MODE1`  

执行：

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    ros2 topic pub /control_mode std_msgs/msg/String "{data: 'MODE1'}" --once

    ros2 action send_goal /navigate_to_pose nav2_msgs/action/NavigateToPose \
    '{"pose":{"header":{"frame_id":"map"},"pose":{"position":{"x":1.50,"y":5.20,"z":0.0},"orientation":{"x":0.0,"y":0.0,"z":0.0,"w":1.0}}}}' \
    --feedback

成功标准：

- 最终出现：

    Goal finished with status: SUCCEEDED

---

## 8. RViz 固定启动方式（12号狗子）

## 8.1 推荐启动命令

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    rviz2 -d ~/Cyber_dog_mini/src/FASTLIO2_ROS2/faster-lio/rviz_cfg/localizer.rviz

## 8.2 当前已验证结论

本机当前已验证：

1. RViz 可正常启动  
2. `localizer.rviz` 可正常显示：
   - PointCloud2
   - TF
   - Path
   - `2D Pose Estimate`
   - `2D Goal Pose`
3. 在 RViz 中点 `2D Goal Pose` 后，小车可实际导航到目标点  

## 8.3 固定使用口径

启动 RViz 后固定：

1. `Fixed Frame = map`
2. View = `TopDownOrtho`
3. 主要保留：
   - Grid
   - Path
   - TF
   - PointCloud2

## 8.4 关于“为什么看不到二维底图”

当前 `localizer.rviz` 显示的是：

- 点云地图
- TF
- 路径
- 机器人位姿

不是标准二维 OccupancyGrid 底图。  
因此看到的是点云轮廓，不是黑白平面地图。  
这不是故障。

## 8.5 RViz Goal Topic

当前已验证 `2D Goal Pose` 使用的话题为：

    /goal_pose

验证命令：

    source /opt/ros/humble/setup.bash
    source ~/Cyber_dog_mini/install/setup.bash
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    ros2 topic echo /goal_pose

---

## 9. 12号狗子的已知坑点

## 9.1 最大坑：不切 `MODE1`，自动导航不放行

症状：

- Nav2 lifecycle 全 active  
- feedback 一直有  
- 车不动或不稳定自动导航  

修复：

    source /opt/ros/humble/setup.bash
    source ~/Cyber_dog_mini/install/setup.bash
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    ros2 topic pub /control_mode std_msgs/msg/String "{data: 'MODE1'}" --once

---

## 9.2 `setup.sh / local_setup.sh` 报错当前不是主阻塞

执行：

    source ~/Cyber_dog_mini/install/setup.bash

时可能看到：

- `setup.sh` not found
- `local_setup.sh` not found

本机当前验证中，这些报错**未阻塞最终 bringup 与导航成功**。  
现阶段可先继续使用，后续再单独整理 workspace 环境链。

---

## 9.3 Flask 只能有一个实例

如果看到：

- `Port 5000 is in use`
- MQTT client id 冲突

说明有多个 Flask 实例在打架。  
先全杀再只保留一个实例。

---

## 9.4 `<TASK_ID>` 不能原样使用

查询任务状态时，必须把占位符替换成真实 task id。  
例如：

错误写法：

    curl "http://127.0.0.1:5000/api/tasks/<TASK_ID>/status"

正确写法：

    curl "http://127.0.0.1:5000/api/tasks/真实ID/status"

---

## 9.5 本机重定位优先走 ROS 直连兜底

现阶段不要把 `/api/navigation/relocalize` 当成唯一依赖。  
对 12号狗子，更稳的方式是：

1. localization completed  
2. `/control_mode = MODE1`
3. `/relocalize_trigger = true`
4. `tf2_echo map base_link`

---

## 10. 12号狗子的当前关键结论

1. 这台狗已经能完成纯 CLI bringup  
2. 这台狗的定位、重定位、TF、Nav2 已全部打通  
3. 这台狗自动导航门禁在 `joystick_vel_ctl_node`  
4. 这台狗自动导航前必须切：

    /control_mode = MODE1

5. 这台狗当前重定位推荐口径为：

    localization completed
    -> MODE1
    -> /relocalize_trigger
    -> tf2_echo map base_link

6. 这台狗已经支持：
   - CLI 发 goal
   - RViz 点选 goal
   - 最终导航到目标点并成功完成

---

## 11. 当前版本的最小成功闭环

对 12号狗子，当前判定“彻底跑通”的最小闭环为：

1. Flask 正常启动  
2. localization status = completed  
3. `ros2 topic pub /control_mode ... MODE1 --once`  
4. `ros2 topic pub /relocalize_trigger ... true --once`  
5. `ros2 run tf2_ros tf2_echo map base_link` 后持续出 transform  
6. 发 `navigate_to_pose` 或 RViz 点 `2D Goal Pose`  
7. 最终 `Goal finished with status: SUCCEEDED`

---

## 12. 后续维护建议

1. 后续换到新狗子时，复制本档案并改名，例如：
   - `dog13_profile.md`
   - `dog14_profile.md`

2. 每台狗至少记录：
   - 主机名
   - `ROS_DOMAIN_ID`
   - 雷达型号 / IP
   - 地图目录
   - 控制模式口径
   - 重定位口径
   - RViz 配置文件
   - 已验证成功命令
   - 特殊坑点

3. 若后续代码更新，应优先复核：
   - `joystick_vel_ctl_node.cpp`
   - `/control_mode` 的模式字符串
   - `/relocalize_trigger` 触发链
   - RViz goal 话题
   - Flask 与 Web 对模式的映射关系
  
 ---
 
 ## 13. 系统全杀 / 强制清场（重启前必做）

当出现以下任一情况时，建议先执行一次“系统全杀 / 强制清场”：

1. Flask 提示 `Port 5000 is in use`
2. MQTT client id 冲突
3. relocalize / localization 表现异常且现象不一致
4. RViz / ROS graph / topic / service 明显与实际运行状态不符
5. 多次试验后怀疑旧进程残留
6. 切换地图 / 切换 bringup 方案 / 切换狗子后准备重新来一轮

---

### 13.1 标准清场命令（推荐）

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

---

### 13.2 强化清场：检查 5000 端口

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    lsof -iTCP:5000 -sTCP:LISTEN -n -P

成功标准：

- 无输出

若还有占用，则强制杀掉：

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    kill -9 $(lsof -t -iTCP:5000 -sTCP:LISTEN) 2>/dev/null || true
    sleep 1
    lsof -iTCP:5000 -sTCP:LISTEN -n -P

---

### 13.3 强化清场：重置 ROS2 daemon

当 `ros2 node list` / `ros2 service list` / `ros2 topic list` 出现明显不合理结果，或报 `rclpy.ok()` / daemon 异常时，执行：

    source /opt/ros/humble/setup.zsh
    source ~/Cyber_dog_mini/install/setup.zsh
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    ros2 daemon stop
    pkill -f _ros2_daemon
    sleep 1
    ros2 daemon start
    sleep 2
    ros2 daemon status

---

### 13.4 强制清场后的最小确认

建议至少确认以下两条：

    source /opt/ros/humble/setup.bash
    source ~/Cyber_dog_mini/install/setup.bash
    export ROS_DOMAIN_ID=137
    cd ~/Cyber_dog_mini

    lsof -iTCP:5000 -sTCP:LISTEN -n -P
    ps -ef | grep -E "python3 app.py|RobotWebApp|web_bridge_node|relocalizer|nav_vlm|car_chassis_node|joystick_node" | grep -v grep

成功标准：

1. 5000 端口无监听
2. 不存在旧的关键残留进程

---

### 13.5 清场后的推荐重启顺序

清场后，建议按以下顺序重新 bringup：

1. 雷达
2. 底盘
3. 手柄
4. Flask
5. 地图加载
6. localization
7. 等待 completed
8. `MODE1`
9. `/relocalize_trigger`
10. `tf2_echo map base_link`
11. 发 goal / RViz 点 goal

---

### 13.6 经验结论

对于 12号狗子，很多“看起来像玄学”的问题，本质上都与残留进程有关。  
尤其是以下两类问题，强烈建议先清场再继续：

1. Flask / MQTT 残留
2. ROS2 daemon / relocalizer / nav_vlm 残留

因此，**系统全杀 / 强制清场** 应作为正式 SOP 的固定前置步骤，而不是出问题后才临时补救。
