# M10 — SLAM / LIO / VIO / Factor Graph

## Module Goal
把 Sensors、Vision Geometry、PointCloud/3D、Probability/Optimization/SE(3)、State Estimation 合流到统一 SLAM 主线：

```text
Sensor
↓
State Propagation
↓
Data Association / Correspondence
↓
Measurement Prediction
↓
Residual
↓
Jacobian
↓
Optimization / Filter Update
↓
Pose / Map
```

最终看到 LIO/VIO 源码时，要能判断每一段代码处于整条估计链的哪一层，以及故障是 Sensor / Time / Calibration / Initialization / Frontend / Estimator / Optimization / Degeneracy / Map / Downstream Integration 中哪一类。

本模块共 8 个理论 Day（Day55–Day62），默认不安排强制实验。Day58/Day62 为重理论日，必要时允许跨学习时段继续，不增加 Day 编号。

---

# Day55 — SLAM 总体架构：Localization、Mapping、Odometry、Frontend、Backend

## 1. 今日目标
理解 localization、mapping、odometry、SLAM 的责任边界，以及 frontend/backend/local/global consistency。

## 2. 前置知识
M08–M09。

## 3. 必须教学内容
1. Localization：`Map + Sensor → Robot Pose`。
2. Mapping：`Pose + Sensor → Map`。
3. SLAM：同时估计trajectory与map，两者相互依赖。
4. Odometry：相对运动估计，通常随时间drift；wheel/visual/LiDAR odometry。
5. Local vs Global Consistency：短时平滑不代表长期global accurate。
6. Frontend：sensor preprocessing、feature/correspondence、scan matching、local motion estimation；核心是产生measurement constraints。
7. Backend：state optimization、pose graph、loop closure correction、global consistency。
8. Map Representation：point cloud、voxel、surfel、feature、occupancy等。
9. LIO/VIO State：position、velocity、orientation、gyro/acc bias、gravity、extrinsic等，不同算法不同。
10. Gauge Freedom：无absolute reference时地图整体平移/旋转可能没有唯一绝对解。
11. Frames：map / odom / base_link / sensor 的典型职责。
12. Drift本质：小estimation error不断累积，缺少足够absolute/global constraint纠正。

## 4. 深度要求
SLAM architecture L4；frontend/backend L3-L4；localization/mapping/odometry区别 L4。

## 5. 工程连接
FAST-LIO、`/fastlio2/lio_odom`、TF `map→body`、Nav2 localization input。

## 6. 明确不展开
ICP公式、factor graph数学、preintegration、loop detector具体算法。

## 7. 本日考核点
Odometry vs SLAM；Localization vs Mapping；frontend/backend；local/global；drift；frames；为何LIO state不只是pose。

### M10毕业考试核心考点
SLAM架构、frontend/backend、drift、state/map/frame。

---

# Day56 — ICP：Correspondence、Point-to-Plane Residual 与 Pose Optimization

## 1. 今日目标
理解两帧/scan-to-map point cloud 如何通过 correspondence + residual + Jacobian + GN 反推出 pose increment。

## 2. 前置知识
M07 PointCloud/KD-tree/normal + M08 LS/GN/SE(3)。

## 3. 必须教学内容
1. Registration：给 source P、target Q，求 T 使 `TP≈Q`。
2. Correspondence：nearest neighbor、local plane 等匹配机制；必须先有对应关系才能形成constraint。
3. ICP Iteration：initial pose→transform source→find correspondence→residual→solve Δpose→update→repeat。
4. Point-to-Point Residual：`r_i=q_i-(Rp_i+t)`。
5. Point-to-Plane Residual：`r_i=n_i^T(Rp_i+t-q_i)`，只惩罚沿normal方向距离。
6. Point-to-plane常收敛更快的geometry intuition。
7. Jacobian：`J_i=∂r_i/∂δξ`，pose微小变化如何改变residual。
8. GN：`J^TJ Δξ=-J^Tr`，`Δξ→Exp→SE(3) pose update`。
9. Wrong Correspondence：数学残差可计算但物理constraint错误，比普通noise更危险。
10. Initial Guess：太差会导致wrong correspondence/local minimum/divergence。
11. Outlier Rejection：distance threshold、robust loss、normal consistency。
12. ICP Degeneracy：长走廊/大平面导致某些方向constraint不足，`J^TJ` condition差。

## 4. 深度要求
ICP full chain L4；point-to-plane L4；residual/Jacobian/GN L4；correspondence failure L4。

## 5. 工程连接
LiDAR odometry、scan-to-map、FAST-LIO geometry matching、corridor degeneracy。

## 6. 明确不展开
ICP全部变体、NDT详细算法、KD-tree源码。

## 7. 本日考核点
ICP估计什么；correspondence；point-to-point vs point-to-plane；normal；initial guess；Jacobian；J^TJ degeneracy；wrong correspondence；iteration；SE(3) update。

### M10毕业考试核心考点
Correspondence、point-to-plane residual、Jacobian、GN、degeneracy。

---

# Day57 — IMU Propagation、Bias、Gravity 与 Preintegration 思想

## 1. 今日目标
理解为什么LIO/VIO里IMU负责高频propagation，而LiDAR/Camera提供geometry correction。

## 2. 前置知识
M03 IMU + M09 EKF。

## 3. 必须教学内容
1. **任何IMU公式出现前必须先定义 frame 和 rotation convention**。例如若 `R_wb` 定义为 body→world rotation，measurement model与其他convention会不同；未来GPT不得脱离定义背公式。
2. 在明确convention后讲gyro model：`ω_m=ω+b_g+n_g`。
3. Accelerometer measurement structure：specific force、gravity、bias、noise；公式形式必须与当前R方向一致。
4. Gyro/acc bias为何进入state。
5. Orientation propagation：gyro×dt → SO(3) increment。
6. Velocity propagation：orientation + specific force + gravity → velocity。
7. Position propagation：velocity + acceleration → position。
8. Error Cascade：gyro bias→orientation error→gravity projection error→acc error→velocity→position error。
9. High-frequency propagation：连续motion estimate和alignment initial guess，但会drift。
10. Measurement Correction：LiDAR/Camera geometry constraint纠正propagation drift。
11. IMU Initialization：gravity、initial orientation、bias、velocity等；bad initialization会污染后续state。
12. Preintegration：many IMU samples→relative motion constraint，避免graph optimization中每次重复积分；不是简单求和，还涉及rotation/bias/covariance。
13. 不同LIO结构不同：FAST-LIO类error-state/filter与典型factor-graph VIO不应混为同一preintegration架构。

## 4. 深度要求
IMU propagation L4；bias/gravity L4；frame/convention L4；preintegration concept L2-L3。

## 5. 工程连接
`/livox/imu`、FAST-LIO、IMU缺失、timestamp、initialization。

## 6. 明确不展开
Full preintegration Jacobian、18D ESKF完整推导、Forster preintegration论文证明。

## 7. 本日考核点
IMU formula为何必须先定义R/frame；gyro/acc measurement含义；gravity；gyro bias→position chain；IMU high-rate优势与drift；initialization；preintegration；为何不是所有LIO都同一结构。

### M10毕业考试核心考点
IMU propagation、bias/gravity、frame convention、initialization、preintegration intuition。

---

# Day58 — LIO：LiDAR + IMU、Deskew、Scan-to-Map 与 Error-State

## 1. 今日目标
建立LIO完整架构：`IMU propagation→predicted pose` 与 `LiDAR deskew→scan-to-map→geometry residual` 合并更新pose/map。

## 2. 前置知识
Day56–57 + M09 EKF。

## 3. 必须教学内容
1. LiDAR与IMU互补：geometry稳定但rate较低/有motion distortion；IMU高频但drift。
2. Motion Distortion：一帧scan并非同一瞬间，各point对应不同pose。
3. Deskew：point timestamp + motion estimate → 同一reference time下的corrected point。
4. Deskew Failure：IMU、timestamp、extrinsic错会把point cloud拉弯。
5. IMU Propagation：orientation、velocity、position、covariance/error-state预测。
6. Scan-to-Map：当前scan对local map建立correspondence，不只与上一frame。
7. Geometric Residual：典型point-to-plane。
8. Iterated Update：同一LiDAR measurement可能多次linearization/update，因为measurement model nonlinear。
9. Error-State：nominal state + small error → corrected nominal，适合rotation/bias。
10. Map Update：state影响map，map又约束下一次state。
11. Local Map：维护附近active region以控制计算量。
12. **Latency分层**：measurement timestamp、arrival time、processing time、publish time必须区分；10Hz output ≠ fresh 10Hz pose。
13. LIO Output：必须确认frame、timestamp、pose含义；不能因topic名含odom就假定等同wheel odom。
14. No Twist / No Covariance：若message不提供，downstream不能凭空假设存在。

## 4. 深度要求
LIO full chain L4；deskew L4；scan-to-map L4；error-state L3-L4；latency reasoning L4。

## 5. 工程连接
FAST-LIO/FAST-LIO2类结构：`IMU propagation→deskew→correspondence→residual→update→map`。

## 6. 明确不展开
FAST-LIO每个状态方程源码、ikd-tree内部、ESIKF完整矩阵推导。

## 7. 本日考核点
LiDAR为何配IMU；motion distortion；deskew；timestamp/extrinsic影响；scan-to-map；point-to-plane；error-state；map↔state；10Hz但400ms age；lio_odom无twist时如何处理。

### M10毕业考试核心考点
Deskew、propagation、scan-to-map、error-state、latency。

---

# Day59 — VIO：Feature、Reprojection Residual 与 Camera + IMU Fusion

## 1. 今日目标
理解VIO与LIO虽然sensor不同，但统一数学主线都是prediction/correspondence/residual/Jacobian/update。

## 2. 前置知识
M05 Camera Geometry + Day57。

## 3. 必须教学内容
1. Visual Odometry：camera frame之间估计relative motion。
2. Feature：corner/keypoint/descriptor概念，不深入ORB。
3. Feature Tracking：同一physical point在连续image中的pixel correspondence。
4. Landmark：3D environmental feature。
5. Projection Model：`u_hat=π(TP_world)`，复用M05。
6. Reprojection Residual：`r=u_observed-u_predicted`。
7. Residual Jacobian可能对pose、landmark、extrinsic求导。
8. Monocular challenges：scale ambiguity、low texture、lighting、motion blur。
9. IMU提供scale、gravity、high-rate motion/prediction等constraint。
10. LIO vs VIO：point-plane residual vs reprojection residual；LiDAR metric range vs monocular scale weakness；light sensitivity差异。
11. Feature-based vs Direct：只建立概念。
12. Visual Failure：low/repetitive texture、lighting change、blur、occlusion。

## 4. 深度要求
Reprojection residual L4；VIO architecture L3-L4；LIO/VIO统一理解 L4。

## 5. 工程连接
Visual localization、pure vision robot、Camera+IMU、mobile manipulation。

## 6. 明确不展开
ORB/SIFT细节、MSCKF/VINS完整推导、photometric error深入。

## 7. 本日考核点
VO vs VIO；feature tracking；landmark；reprojection residual；依赖state；scale ambiguity；IMU作用；LIO/VIO统一点；low texture failure。

### M10毕业考试核心考点
Feature tracking、reprojection residual、VIO architecture、LIO/VIO comparison。

---

# Day60 — Factor Graph、Pose Graph 与 Information Form

## 1. 今日目标
理解一长串robot states和sensor measurements如何表示成factor graph并统一优化。

## 2. 前置知识
M08 WLS/GN + M09。

## 3. 必须教学内容
1. Variable Node：pose、velocity、bias、landmark等待估计变量。
2. Factor：odometry、IMU、GPS、reprojection、loop closure等measurement constraint。
3. Factor Graph：variables↔measurement factors，是概率/优化problem representation。
4. Cost：每factor产生 `r_i(x)` 和 `r_i^TΩ_i r_i`，总cost为sum。
5. Information Matrix：`Ω=Σ^-1`，更可信factor信息更强。
6. Pose Graph：node多为robot pose，edge是relative pose/loop等constraint。
7. Linearization：每factor计算residual/Jacobian，组合global system。
8. Sparse Structure：每measurement只连接少量states，因此normal system稀疏。
9. Prior Factor：固定reference/gauge freedom。
10. Marginalization：移出旧state但保留其信息，控制active optimization规模。
11. Filter vs Graph：sequential/current-state oriented vs history states jointly re-optimized。
12. Batch vs Incremental概念。

## 4. 深度要求
Factor graph L4；factor/residual/information L4；sparsity L3；filter vs graph L4。

## 5. 工程连接
VIO、pose graph、GPS factor、loop closure、bundle adjustment概念。

## 6. 明确不展开
iSAM2源码、sparse Cholesky、Schur complement深入、GTSAM API。

## 7. 本日考核点
Variable/factor；residual；information；pose graph；sparse；prior；marginalization；filter vs graph；GPS factor。

### M10毕业考试核心考点
Factor、information、pose graph、sparsity、filter vs optimization。

---

# Day61 — Loop Closure、Global Consistency 与 Drift Correction

## 1. 今日目标
理解机器人重访旧地点后如何新增long-range constraint纠正历史trajectory，并明确loop closure与LIO本体的责任边界。

## 2. 前置知识
Day55 + Day60。

## 3. 必须教学内容
1. Loop Closure解决长期odometry drift。
2. Place Recognition：看起来像旧地方 ≠ 已确认是同一地方。
3. Candidate Detection：visual/LiDAR/global descriptor概念。
4. Geometric Verification：避免perceptual aliasing/相似走廊误匹配。
5. Loop Factor：确认后在pose graph添加 `pose_i↔pose_j` long-range constraint。
6. Global Optimization：历史trajectory整体调整，不只是当前pose瞬间拉回。
7. Map Correction：pose correction后历史map也需要一致调整/重建表达。
8. False Positive Loop：可破坏整张map；false negative通常只是失去纠正机会。
9. Localization Jump：global pose correction可能影响downstream navigation，需要frame architecture隔离local continuity与global correction。
10. map→odom思想：global correction与local continuous odometry分层。
11. **边界硬规则（审查修正）**：Loop Closure是SLAM系统能力，但**不是所有LIO实现天然包含**。使用 FAST-LIO / 某LIO odometry 不代表系统自动具备loop closure、global pose graph或map correction；必须以实际系统模块为准。

## 4. 深度要求
Loop pipeline L3-L4；pose graph correction L4；false loop risk L4；system boundary L4。

## 5. 工程连接
Large map、Nav2 map/odom、global correction、localization jump、FAST-LIO与外部loop模块的边界。

## 6. 明确不展开
Scan Context公式、DBoW、loop detector网络细节。

## 7. 本日考核点
Loop解决什么；recognition vs verification；false positive risk；loop factor；history correction；map correction；global pose跳变；map/odom；为什么“用了FAST-LIO”不等于“有loop closure”。

### M10毕业考试核心考点
Loop detection/verification、pose graph correction、global/local consistency、LIO/loop boundary。

---

# Day62 — Degeneracy、Observability、Latency、TF、Initialization 与 Real-System Debug

## 1. 今日目标
拿到实际LIO/VIO故障，区分观测不足、当前环境退化、数据过期、TF/外参、初始化、frontend/optimizer、sensor本身等问题，并形成证据链。

## 2. 前置知识
Day55–61全部内容。

## 3. 必须教学内容
1. Degeneracy：当前环境/局部optimization对某些state directions缺少足够独立constraint。
2. Geometry Degeneracy：long corridor、large plane、feature-poor environment等。
3. Mathematical Signature：`J^TJ` poor condition、small eigenvalue、rank deficiency、information不均衡。
4. **Observability vs Degeneracy（审查修正）**：Observability更偏系统/传感器组合在时间上是否能估计某状态；Degeneracy更偏当前环境/当前局部问题是否缺某方向constraint。二者相关但不能混成同义词。
5. VIO degeneracy：low texture、pure rotation、little parallax等。
6. Initialization Failure：initial pose、gravity、velocity、bias、scale；错误可能导致“稳定地错”。
7. Timestamp Error：measurement timestamp、arrival time、processing delay分开。
8. Latency：正确但旧的pose仍是实时控制错误输入。
9. Delay Chain：scan time→processing→LIO publish→controller receives stale pose→correction迟到→oscillation/S-shape。
10. Extrinsic Error：LiDAR↔IMU、Camera↔IMU外参错会形成motion-dependent residual/map deformation/systematic drift。
11. Deskew Failure：moving/high angular speed时明显恶化，static可能正常。
12. IMU Failure：rate、bias、axis、units、timestamp、frame、dropout。
13. Outlier / Wrong Correspondence：sudden jump、residual spike、map tearing。
14. Degeneracy vs Sensor Failure：sensor正常但environment不给constraint vs measurement本身异常。
15. Algorithm Frequency vs Freshness：`10Hz output ≠ 10Hz fresh pose`；必须同时测rate、latency、age。
16. **TF evidence chain（审查修正）**：必须明确检查 TF source/target direction、timestamp、availability、staleness；TF数值方向或时间错会伪装成perception/LIO/downstream control问题。
17. Bag Analysis固定框架：topic existence→rate→measurement timestamp→end-to-end latency/age→TF direction/time→extrinsic→raw sensor quality→state continuity→residual/innovation(if available)→environment geometry→initialization→map consistency→downstream behavior。
18. Failure Attribution：Sensor / Time / TF-Calibration / Initialization / Frontend / Estimator / Optimization / Environment Degeneracy / Map / Downstream Integration。

## 4. 深度要求
Degeneracy L4-L5；observability-vs-degeneracy L4；timestamp/latency/TF L5；failure attribution L5；bag evidence chain L4-L5。

## 5. 工程连接
必须能解释“LIO约9.9Hz但median pose age/latency约0.39s”这种情况：轨迹可平滑但controller实际消费的是过去状态，因此实时控制仍会滞后。

## 6. 明确不展开
本日不增加新算法，只做综合Owner分析。

## 7. 本日考核点
Degeneracy；corridor；small eigenvalue；observability vs degeneracy；initialization；extrinsic；deskew；rate≠freshness；latency；sensor failure vs geometry degeneracy；TF direction/timestamp；bag故障树；LIO stale pose→controller anomaly完整传播。

---

# M10 Graduation Exam Specification

## A. 核心理论专项 — 30%
必须覆盖：SLAM/Odom/Localization/Mapping、Frontend/Backend、Correspondence（硬门槛）、Point-to-plane Residual（硬门槛）、ICP Jacobian/GN（硬门槛）、IMU Propagation（硬门槛）、Bias/Gravity、Frame/Rotation Convention、Preintegration、Deskew（硬门槛）、Scan-to-map（硬门槛）、Error-state、Reprojection Residual（硬门槛）、LIO vs VIO、Factor Graph（硬门槛）、Information Matrix、Loop Closure与LIO边界、Degeneracy（硬门槛）、Observability vs Degeneracy、Timestamp/Latency/TF（硬门槛）、Initialization（核心必考）。

## B. 数学/算法综合题 — 30%
1. ICP：source point + target plane + normal + pose，写point-to-plane residual、变量/Jacobian意义、GN update structure。
2. VIO Reprojection：3D landmark + camera pose + K + observed pixel，完成world→camera→projection→residual，并解释约束pose。
3. Factor Graph：识别variables/factors、absolute/global constraint、loop对trajectory影响，并判断哪些能力不属于纯LIO odometry本体。

## C. 真实系统综合题 — 40%
至少3组：
1. LIO rate正常但pose age约400ms：区分rate与freshness，分析LiDAR→processing→LIO→TF→Controller延迟链。
2. 长走廊退化：raw sensor正常但某方向漂，分析geometry、Hessian/eigen、observability/degeneracy与sensor failure区别。
3. 高速转弯地图变形：从IMU、timestamp、deskew、extrinsic、processing latency、TF检查构造排查优先级。

## M10 Owner毕业题
给rosbag topic rate/timestamp/TF/LIO trajectory/raw LiDAR/IMU/controller behavior，必须输出：`现象→系统链→候选根因→证据需求→排除过程→主根因→短期修复→长期修复→Regression指标`。

## M10通过标准
- 总分 ≥85%；
- 必须独立解释 `Propagation→Correspondence→Residual→Jacobian→Update`；
- ICP residual、VIO reprojection residual、factor residual必须统一理解为measurement vs prediction constraint；
- Degeneracy、Timestamp、Latency、TF direction/time为硬门槛；
- 不允许“LIO=LiDAR直接算pose”或“VIO=Camera直接算pose”的错误理解；
- 必须能够从真实bag建立跨Sensor/Time/TF/Estimator/Downstream证据链。

## Day55–Day62索引
```text
Day55  SLAM Architecture / Frontend / Backend
Day56  ICP / Correspondence / Point-to-plane / GN
Day57  IMU Propagation / Bias / Gravity / Preintegration / Frame Convention
Day58  LIO / Deskew / Scan-to-map / Error-State / Latency
Day59  VIO / Feature / Reprojection Residual
Day60  Factor Graph / Pose Graph / Information
Day61  Loop Closure / Global Consistency / LIO Boundary
Day62  Degeneracy / Observability / Latency / TF / Initialization / Bag Debug
```
