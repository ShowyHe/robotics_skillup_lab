# M03 — Sensors & Actuators

## Module Goal
建立完整的物理—软件链：

```text
Physical Truth
→ Sensor Measurement
→ Noise / Bias / Delay
→ Driver / ROS Message
→ Algorithm
→ Command
→ Controller / Driver
→ Actuator
→ Physical Motion
→ Feedback
```

目标不是学设备接线，而是理解“数据为什么会错、命令为什么不等于实际动作”。本模块仍以 2–3h 理论学习为主，不安排日常实验。

---

## Day16 — Measurement、Accuracy、Noise、Bias 与 Sensor Model

### 今日目标
能够严格区分 truth、measurement、accuracy、precision、resolution、noise、bias、drift、covariance、outlier，并理解 `/topic` 有数据不代表数据可直接相信。

### 前置知识
M02 基础：vector、mean/变化量基本直觉。概率严格定义留到 M08。

### 必须教学内容
1. Physical Truth vs Measurement：`z=h(x)+error`；measurement 是对真实物理量的观测，而非 truth 本身。
2. Accuracy：测量距离真实值有多近。
3. Precision：重复测量彼此有多集中；允许“高 precision + 低 accuracy”。
4. Resolution：设备能够分辨的最小变化；不能与 accuracy 混淆。
5. Random Noise：单次随机波动、重复测量分散，对算法稳定性的影响。
6. Bias：固定或缓慢变化的系统性偏置。
7. Drift：bias / model error 经时间积分后形成的累计偏移。
8. Outlier：必须区分普通 noise、异常值和 sensor failure。
9. Sampling Rate：10Hz/100Hz/400Hz 的含义；发布频率不等于真实有效信息带宽。
10. Covariance 初步概念：只理解其用于表达不确定性结构；**本日不把 covariance 当成“实际误差”，也不展开概率数学。**
11. Sensor Specification：range、resolution、accuracy、repeatability、rate、latency，以及 noise density / bias stability 的概念位置。

### 深度要求
Accuracy / precision / resolution L3；noise / bias / drift L3；measurement model L2-L3；covariance L1-L2。

### 工程连接
GNSS covariance、RTK FIX/FLOAT、IMU bias、LiDAR range noise、wheel odometry、真实机器人定位漂移。

### 明确不展开
Gaussian 概率理论、Allan Variance 推导、EKF、sensor calibration 优化、covariance propagation。

### 本日考核点
1. Accuracy 和 precision 有什么区别？
2. Resolution 高是否代表 accuracy 高？
3. Noise 和 bias 区别？
4. Bias 为什么会导致 IMU 积分漂移？
5. Drift 和瞬时 noise 的影响有什么不同？
6. Outlier 和普通 noise 区别？
7. 100Hz sensor 是否意味着 100Hz 独立有效信息？
8. `z=h(x)+error` 每部分是什么意思？
9. 为什么 covariance 不能直接等于真实绝对误差？
10. 给多组测量数据判断 accuracy / precision 表现。

### M03毕业考试核心考点
Measurement model、accuracy/precision/resolution、noise/bias/drift、sensor uncertainty。

---

## Day17 — IMU、Encoder、LiDAR、Camera：到底测量了什么

### 今日目标
能够回答每种传感器“直接测到了什么”，并区分 direct measurement 与 derived estimate。

### 前置知识
Day16。

### 必须教学内容
1. IMU 组成：accelerometer 与 gyroscope。
2. Gyroscope：直接测 angular velocity `ω`，不是 orientation；orientation 由角速度随时间积分得到。
3. Accelerometer：不能简单理解为 world-frame 线加速度；必须理解 specific force、gravity、body frame、orientation / gravity compensation 的关系。
4. IMU Bias + Integration：acceleration error → velocity error → position error；gyro error → orientation error → gravity projection error → position 进一步恶化。
5. Encoder：joint/wheel encoder、tick、angular position、difference / differentiation 得到 velocity、wheel radius、gear ratio。
6. Wheel Odometry：encoder → wheel rotation → kinematic model → robot motion estimate；wheel odom 不是 encoder 直接测出的 robot pose。
7. Wheel Odom 误差：slip、wheel radius、terrain、kinematic model。
8. LiDAR：主要直接测 range、beam direction/angle、intensity（视设备）；XYZ point 是坐标转换后的表示。
9. Camera：直接产生 image / pixel intensity measurement；object distance、3D coordinate、semantic label 都是后续算法结果。
10. RGB-D：理解 RGB + depth，以及 Stereo / Structured Light / ToF 的基本位置。
11. Complementary Sensors：IMU 高频但漂、GNSS 绝对但环境敏感、LiDAR 几何稳定但会退化、Camera 语义丰富但受光照、Encoder 短时运动好但会 slip。

### 深度要求
IMU measurement L3；Encoder/Wheel Odom L3；LiDAR/Camera measurement L3；sensor complementarity L3。

### 工程连接
`/livox/imu`、`/livox/lidar`、`/wheel_odom`、FAST-LIO、Camera perception；分析 ROS message 中 pose/twist 究竟是直接测量还是推导结果。

### 明确不展开
IMU preintegration、LiDAR SLAM、Computer Vision 算法、EKF 融合。

### 本日考核点
1. Gyro 直接测 orientation 吗？
2. Accelerometer 直接测 world-frame acceleration 吗？
3. 为什么 gyro bias 会间接导致 position 漂移？
4. Wheel odom 的 robot pose 谁算出来的？
5. Wheel slip 为什么造成 odom 误差？
6. LiDAR 直接测 XYZ 吗？
7. Camera 直接测 3D 位置吗？
8. 为什么 IMU 与 LiDAR 互补？
9. 为什么 GNSS 与 LIO 互补？
10. 给一个 ROS topic 字段，判断其属于 direct measurement 还是 derived state。

### M03毕业考试核心考点
不同 sensor measurement 的物理来源；direct measurement vs derived estimate。

---

## Day18 — GNSS / RTK、Timestamp、Latency、Synchronization 与 Calibration

### 今日目标
理解“数据本身对，但时间错了、坐标错了，也等于数据错了”。

### 前置知识
Day16–17。

### 必须教学内容
1. GNSS 基本原理：satellite signal → range-like measurement → receiver position，工程级理解。
2. GNSS 误差来源：satellite geometry、atmosphere、multipath、blockage、receiver noise。
3. RTK 基本原理：利用基准/差分修正与载波相位信息显著提高定位精度；理解 Single / Float / Fixed 的工程意义。
4. RTK Position ≠ Heading：单天线 RTK 的高精度位置不自动等于静止时高精度 heading；双天线 heading 依赖 baseline。
5. Timestamp：应代表 measurement 对应的物理时刻，而非“程序什么时候收到消息”。
6. Measurement Time / Arrival Time / Processing / Publish Time 的区别。
7. Latency：数据反映的是过去状态；运动机器人为什么对 latency 敏感。
8. Jitter：平均延迟与延迟波动必须区分。
9. Synchronization：Camera/LiDAR/IMU 如果对应不同物理时刻，会产生错误融合。
10. Hardware Sync vs Software Sync：理解方法与精度层级差异。
11. Intrinsic Calibration：只理解传感器自身参数是什么，不展开数学求解。
12. Extrinsic Calibration：Sensor frame 与 robot frame 的固定几何关系；`Sensor measurement + T_base_sensor → robot-usable measurement`。
13. Calibration Error：外参误差在运动、远距离目标时可能被放大。

### 深度要求
GNSS/RTK 工程原理 L2-L3；timestamp/latency/sync L3-L4；intrinsic/extrinsic 概念 L2-L3。

### 工程连接
GNSS FIX/FLOAT、NTRIP/RTCM 的系统位置、FAST-LIO 延迟、Camera/LiDAR 同步、TF extrinsic、旧 pose 对控制的影响。

### 明确不展开
GNSS 载波相位数学、PPP、NTRIP 协议细节、Camera calibration 数学、hand-eye、SE(3) 优化。Camera calibration 数学留 Day24，SE(3)/优化留 M08。

### 本日考核点
1. RTK Fixed 意味着什么？
2. 单天线 RTK 静止时能否天然提供可靠 heading？
3. Timestamp 应代表什么时刻？
4. arrival time 为什么不能替代 measurement time？
5. latency 和 jitter 区别？
6. 两 sensor 频率相同是否代表同步？
7. hardware sync 与 software sync 区别？
8. intrinsic/extrinsic 区别？
9. 外参错误为什么可能看起来像 perception 算法错？
10. 给“LIO 落后 400ms”写出对 planner/controller 的影响链。

### M03毕业考试核心考点
RTK 状态、measurement time、latency/jitter、sync、extrinsic。

---

## Day19 — Actuator、Motor Loop、Communication 与 Command→Motion

### 今日目标
理解算法输出 `vx/wz`、joint position/velocity/torque 后，到真实运动之间还发生了什么。

### 前置知识
Day16–18。

### 必须教学内容
1. Actuator：区分 motor、actuator、transmission、mechanism。
2. DC / BLDC / Servo：只做工程级差异理解，重点是软件最终作用于真实执行机构。
3. Position / Velocity / Torque Control：区分不同 command 层级。
4. Cascaded Control Loop：Position Loop → Velocity Loop → Current/Torque Loop → Motor；理解高层 command 不会瞬间成为实际运动。
5. Encoder Feedback：Command → Motor → Encoder → Feedback → Controller 的闭环意义。
6. Saturation：max velocity / max torque / acceleration limit 等物理约束。
7. Rate Limit / Acceleration Limit：与 velocity limit 区分。
8. Gearbox：gear ratio、speed/torque trade-off、backlash 基础。
9. Commanded State vs Actual State：`commanded velocity ≠ measured velocity`；错误地拿 command 当真实 feedback 的风险。
10. UART：serial、baud rate、framing、point-to-point 基本概念。
11. CAN：bus、message ID、arbitration 基本思想与机器人常见用途。
12. Ethernet：bandwidth、packet/network，与 CAN/UART 的工程差异。
13. Communication Delay / Drop：driver 层问题如何表现成 controller 问题。

### 深度要求
Position/velocity/torque control L3；cascaded loop L3；command vs feedback L4；communication L2-L3。

### 工程连接
chassis command、wheel feedback、ros2_control、controller 输出、急停、velocity saturation。

### 明确不展开
Motor electromagnetic equations、FOC 数学、PCB、CAN 位级协议实现。

### 本日考核点
1. Motor 和 actuator 是否完全相同？
2. Position/velocity/torque command 区别？
3. 为什么 servo 常有多层 control loop？
4. command 0.5m/s 是否意味着机器人马上 0.5m/s？
5. Saturation 是什么？
6. acceleration limit 有什么意义？
7. command 和 feedback 为什么必须区分？
8. Gear ratio 如何影响 speed/torque？
9. CAN 与 UART 基本区别？
10. 通信延迟为什么可能表现为 controller 震荡或滞后？

### M03毕业考试核心考点
Actuator limits、closed loop、command vs feedback、communication effects。

---

# M03 Graduation Exam Specification

## A. 核心专项 — 30%
必须覆盖：accuracy/precision/resolution、noise/bias/drift、IMU、wheel odometry、LiDAR/Camera direct measurement、RTK、timestamp、latency/jitter、synchronization、extrinsic、actuator command/feedback。

## B. 综合场景 — 50%
至少 3 个场景：

### 场景1：定位异常
给 IMU、Wheel、RTK、LIO 数据与现象，要求区分 measurement error、bias、latency、fusion issue，并提出验证证据。

### 场景2：时间问题
LiDAR 与 LIO 都显示约 10Hz，但 LIO 整体滞后 350ms。要求解释为什么 frequency 看不出 latency、如何验证、对 planner/controller 的影响。

### 场景3：底盘执行异常
MPPI 输出 0.5m/s，底盘反馈长期只有 0.32m/s。要求从 controller、saturation、acceleration limit、motor、feedback、communication 构造证据链。

## C. 数据 / 规格题 — 20%
给 sensor datasheet、ROS message 或 topic 统计，判断哪些指标代表 precision、rate、covariance、latency、physical limit，并指出不能从这些指标直接推出什么。

## Knowledge Coverage Matrix
- Accuracy / Precision / Resolution：必考
- Noise / Bias / Drift：**核心必考**
- IMU measurement：**核心必考**
- Encoder / Wheel Odom：必考
- LiDAR / Camera measurement：必考
- GNSS / RTK：必考
- Timestamp：**核心必考**
- Latency / Jitter：**核心必考**
- Synchronization：**核心必考**
- Intrinsic / Extrinsic：必考
- Command vs Feedback：**核心必考**
- Actuator Limits：必考
- UART/CAN/Ethernet：理解题

## 通过标准
- 总分 ≥85%；
- Noise/Bias、Timestamp/Latency、Command/Feedback 不得出现根本性错误；
- 综合题必须形成“现象 → 假设 → 证据 → 验证 → 结论”链路；
- 单点核心错误采用定向补课 + 复测，不机械重学整个模块。
