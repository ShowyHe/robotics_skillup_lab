# M10 — SLAM / LIO / VIO / Factor Graph

## Module Goal
把 Sensors、Vision Geometry、PointCloud、Probability/Optimization/SE(3) 与 State Estimation 合流为统一 SLAM 主线：`Propagation / Correspondence → Residual → Jacobian → Filter/Optimization Update → Pose/Map`，并能从真实LIO/VIO源码与bag中区分 Sensor / Time / Calibration / Initialization / Frontend / Estimator / Degeneracy / Map / Downstream Integration。

本模块共 8 个理论 Day（Day55–Day62）。

---

# Day55 — SLAM Architecture / Frontend / Backend
1. 今日目标：严格区分 Localization、Mapping、Odometry、SLAM、Frontend、Backend、Local/Global Consistency。
2. 前置：M08–M09。
3. 必须教学：localization/mapping/SLAM；odometry/drift；frontend产生measurement constraints；backend优化state/global consistency；map representations；LIO/VIO state；gauge freedom；map/odom/base/sensor frame职责；local vs global accuracy；loop closure不是所有LIO必需组件。
4. 深度：Architecture L4。
5. 工程连接：FAST-LIO、Nav2 localization input。
6. 不展开：ICP/factor graph公式。
7. 考核：解释odometry短时平滑为何不等于global accurate。
8. 毕业考点：SLAM architecture、frontend/backend、drift/frame。

# Day56 — ICP / Correspondence / Point-to-plane / GN
1. 今日目标：理解point cloud registration如何从correspondence和residual估计pose increment。
2. 前置：M07 PointCloud + M08 GN/SE(3)。
3. 必须教学：registration；nearest/local-plane correspondence；ICP iteration；point-to-point；point-to-plane `r=n^T(Rp+t-q)`；normal；Jacobian；GN normal equation；SE(3) update；initial guess；wrong correspondence；outlier rejection；large-plane/corridor degeneracy。
4. 深度：ICP/residual/Jacobian/GN L4。
5. 工程连接：LiDAR odometry、scan-to-map。
6. 不展开：ICP变体全集、NDT深入。
7. 考核：解释wrong correspondence为何比普通noise危险；识别degenerate direction。
8. 毕业考点：Correspondence、Point-to-plane、GN、Degeneracy。

# Day57 — IMU Propagation / Bias / Gravity / Preintegration
1. 今日目标：理解IMU高频propagation及其error cascade。
2. 前置：M03 IMU + M09 EKF。
3. 必须教学：任何公式先定义frame/rotation convention；gyro/accelerometer measurement structure；specific force/gravity/bias/noise；orientation/velocity/position propagation；gyro bias→orientation→gravity projection→velocity/position error；initialization；high-rate propagation vs drift；LiDAR/Camera correction；preintegration为何存在；preintegration不是简单求和；filter-style LIO与factor-graph VIO结构不同。
4. 深度：Propagation/bias/gravity/frame L4；Preintegration L2-L3。
5. 工程连接：`/livox/imu`、FAST-LIO/VIO。
6. 不展开：完整preintegration Jacobian、18D ESKF。
7. 考核：根据明确R convention解释gyro/acc公式和误差传播。
8. 毕业考点：IMU propagation、bias/gravity、frame convention、initialization。

# Day58 — LIO / Deskew / Scan-to-map / Error-State / Latency
1. 今日目标：建立LIO完整chain。
2. 前置：Day56–57 + M09。
3. 必须教学：LiDAR/IMU complementarity；motion distortion；point timestamp；deskew；deskew对IMU/time/extrinsic依赖；IMU prediction；scan-to-map；point-plane residual；iterated update；nominal/error-state；local map；map↔state；measurement/arrival/process/publish time；10Hz≠fresh；output frame/stamp；message没有twist/covariance时不能凭空假设。
4. 深度：LIO/Deskew/Latency L4。
5. 工程连接：FAST-LIO2类pipeline。
6. 不展开：ikd-tree内部、完整ESIKF矩阵。
7. 考核：10Hz LIO但age=400ms为什么会影响controller；deskew错误如何出现。
8. 毕业考点：Deskew、Scan-to-map、Error-State、Latency。

# Day59 — VIO / Feature / Reprojection Residual
1. 今日目标：理解VIO与LIO共享的估计数学结构。
2. 前置：M05 + Day57。
3. 必须教学：VO/VIO；feature/descriptor概念；feature tracking；landmark；projection；reprojection residual `r=u_obs-u_pred`；pose/landmark/extrinsic Jacobian；monocular scale ambiguity；IMU提供scale/gravity/high-rate constraint；feature-based vs direct概念；low/repetitive texture、lighting、blur、occlusion failure；LIO vs VIO residual与sensor特性比较。
4. 深度：Reprojection L4；VIO architecture L3-L4。
5. 工程连接：pure vision localization、camera+IMU。
6. 不展开：ORB/VINS/MSCKF完整实现。
7. 考核：解释reprojection residual依赖哪些state，为什么monocular有scale问题。
8. 毕业考点：Feature Tracking、Reprojection、VIO/LIO comparison。

# Day60 — Factor Graph / Pose Graph / Information
1. 今日目标：理解多个states和measurements如何统一成稀疏优化问题。
2. 前置：M08 WLS/GN + M09。
3. 必须教学：variable node；factor；factor graph；factor residual/cost；information matrix `Ω=Σ^-1`；pose graph；linearization；sparse structure；prior/gauge；marginalization；filter vs graph；batch vs incremental概念；measurement可信度不能只由一个score决定。
4. 深度：Factor/residual/information L4；Sparsity L3。
5. 工程连接：GPS factor、VIO、loop graph。
6. 不展开：iSAM2/Schur完整推导。
7. 考核：从三个pose+odometry+GPS画factor graph并解释信息矩阵。
8. 毕业考点：Factor Graph、Information、Sparse Optimization。

# Day61 — Loop Closure / Global Consistency / Relocalization Boundary
1. 今日目标：理解loop/global correction解决什么，以及何时不应把它当LIO必备。
2. 前置：Day55–60。
3. 必须教学：local drift；loop candidate；place recognition概念；geometric verification；loop factor；pose graph correction；false loop风险；map deformation/global consistency；relocalization vs loop closure；absolute GNSS anchor；filter-only/local LIO与loop-augmented system边界；large correction如何影响downstream TF/Navigation；loop latency。
4. 深度：Loop/global consistency L3-L4；boundary reasoning L4。
5. 工程连接：LIO+GNSS、large map。
6. 不展开：具体place-recognition network。
7. 考核：解释“没有loop closure”不等于LIO实现错误；false loop为什么危险。
8. 毕业考点：Loop Closure、Global Consistency、Relocalization Boundary。

# Day62 — Degeneracy / Time / Initialization / TF / Bag Debug / Owner
1. 今日目标：从bag/log建立SLAM Owner证据链。
2. 前置：Day55–61。
3. 必须教学：observability vs geometric degeneracy；eigen/SVD condition直觉；corridor/plane/low-texture examples；time synchronization；sensor timestamp/processing latency；extrinsic/calibration；initial gravity/bias/pose；TF direction和stamp；map/odom/base contract；sensor dropout；state jump；freshness watchdog；bag topic completeness；source mapping：preprocess→propagate→correspondence→residual/update→map→publish；下游控制使用state的age/semantics；failure taxonomy与minimal evidence set。
4. 深度：Degeneracy/time/init/TF/debug L5。
5. 工程连接：LIO stale、GPS/LIO融合、bag证据不足。
6. 不展开：新增SLAM算法。
7. 考核：给LIO摆动/跳变/漂移现象设计Sensor-Time-Frame-Estimator-Degeneracy排查树。
8. 毕业考点：Degeneracy、Latency、Initialization、TF、Owner Debug。

---

# M10 Graduation Exam
统一权重：**30%核心基础 / 50%综合系统场景 / 20% Source·Formula·Design**。

## 30% 核心基础
硬门槛：SLAM/odometry/frontend/backend、ICP correspondence/point-plane/GN、IMU frame convention/bias/gravity、deskew/scan-to-map、reprojection residual、factor/information、degeneracy vs observability、timestamp/TF/initialization。

## 50% 综合系统场景
至少覆盖：
1. point-to-plane ICP + bad correspondence/degeneracy；
2. IMU bias/deskew/time error导致point cloud或pose异常；
3. LIO 10Hz但pose age很大对下游的影响；
4. VIO low texture/scale/reprojection failure；
5. factor graph中GPS/loop constraint与false loop风险；
6. corridor/large-plane degeneracy与unobservable direction区分；
7. bag缺topic、TF方向错或初始化失败的证据边界。

## 20% Source / Formula / Design
能够从一个LIO/VIO/SLAM实现中定位：sensor preprocessing、timestamp/extrinsic、propagation、deskew/correspondence、residual/Jacobian、filter/optimization update、map/backend、loop/global correction（若有）、publish TF/pose；并明确实现没有某一组件时不能臆测存在。

## 通过标准
总分≥85%；必须明确 `rate≠freshness`、`observability≠degeneracy`、`local accuracy≠global consistency`；任何IMU/pose公式必须先说明frame/convention。
