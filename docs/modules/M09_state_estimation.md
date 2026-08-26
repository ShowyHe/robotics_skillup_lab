# M09 — State Estimation

## Module Goal
把 M03 的传感器与 M08 的概率/优化连接起来，建立机器人状态估计完整理论链：

```text
上一时刻状态
+
Control / Motion Model
+
Process Noise
↓
Prediction
↓
Predicted State + Predicted Covariance
+
Sensor Measurement
+
Measurement Model
+
Measurement Noise
↓
Innovation
↓
Kalman Gain
↓
Correction
↓
Updated State + Updated Covariance
```

最终必须能从数学上解释 `x / P / F / G / Q / H / R / K` 的意义，而不是只会改融合参数。

本模块共 6 个理论 Day（Day49–Day54），默认不安排强制实验。Day51/Day52 为重理论日，必要时允许跨学习时段继续，但不增加 Day 编号。

---

# Day49 — State、Motion Model、Measurement Model 与 Noise

## 1. 今日目标
严格区分 true state、estimated state、control、process model、measurement、measurement model、process noise、measurement noise、state covariance。

## 2. 前置知识
M03 Sensor/Noise/Bias/Timestamp + M08 Bayes/Gaussian/Covariance。

## 3. 必须教学内容
1. State：算法选择的一组待估计变量，不是固定模板；可含 position、velocity、orientation、bias、gravity、extrinsic等。
2. True State `x` vs Estimated State `x_hat`。
3. Control Input `u_k`：command/velocity等，但 command 不自动等于真实运动。
4. Linear Motion Model：`x_k=F_k x_(k-1)+B_k u_k+w_k`。
5. Nonlinear Motion Model：`x_k=f(x_(k-1),u_k)+w_k`。
6. Measurement Model：linear `z_k=H_k x_k+v_k`；nonlinear `z_k=h(x_k)+v_k`。
7. Process Noise `w~N(0,Q)`：描述motion/process model未建模不确定性，如slip、terrain、unmodeled acceleration。
8. Measurement Noise `v~N(0,R)`：描述sensor measurement uncertainty。
9. **Q vs R硬门槛**：Q=不信运动模型多少；R=不信观测多少。
10. State Covariance `P`：当前state estimate的不确定性；与Q/R不同。
11. Markov Assumption工程直觉：当前state若已包含未来所需信息，可不保存完整历史。

## 4. 深度要求
State/measurement model L4；Q/R/P L4；Markov L2-L3。

## 5. 工程连接
Wheel odom、IMU、GNSS/RTK、LIO pose、chassis feedback；必须分析把上一周期command当真实velocity的风险。

## 6. 明确不展开
Kalman Gain、EKF Jacobian、observability数学、IMU preintegration。

## 7. 本日考核点
State vs measurement；x_hat vs true x；F/H；Q/R/P；wheel slip对应哪类uncertainty；GPS精度差对应Q还是R；command为何不能直接当state；设计Wheel+GPS系统state/measurement。

### M09毕业考试核心考点
State、motion model、measurement model、Q/R/P。

---

# Day50 — Kalman Filter Prediction：状态和不确定性传播

## 1. 今日目标
理解没有新measurement时，如何根据上一state和motion model预测当前state与uncertainty。

## 2. 前置知识
Day49 + M02 matrix。

## 3. 必须教学内容
1. Classic KF条件：linear system + Gaussian noise。
2. Prediction State：`x_hat_k^- = F x_hat_(k-1)^+ + B u_k`；解释 `-` / `+`。
3. Prediction物理意义：上一position + velocity×dt → 当前预测。
4. Constant Velocity Model：`x=[p,v]^T`，`F=[[1,dt],[0,1]]`，必须手算。
5. Covariance Prediction基础式：`P^- = F P^+ F^T + Q`。
6. 为什么是 `FPF^T`：state线性变换时uncertainty也传播。
7. 为什么加Q：每次prediction引入新的model uncertainty；长期无measurement时P应增长。
8. dt同时影响state prediction和covariance propagation。
9. Prediction frequency高不自动等于准确。
10. Open-loop Drift：连续predict、无absolute correction时model error/bias/integration error累计。

## 4. 深度要求
State prediction L4；covariance propagation L4；constant velocity F L3-L4。

## 5. 工程连接
Wheel integration、dead reckoning、GPS断流、IMU propagation、controller feedback。

## 6. 明确不展开
Measurement update、nonlinear EKF、IMU连续动力学。

## 7. 本日考核点
x^- / x^+；F；构造constant-velocity F；FPF^T；Q；GPS断流时P；dt；frequency≠accuracy；wheel-only drift。

### M09毕业考试核心考点
Prediction、F、Q、P propagation。

---

# Day51 — Kalman Filter Update：Innovation 与 Kalman Gain

## 1. 今日目标
理解sensor来了以后，filter如何根据prediction与measurement各自uncertainty决定 correction 强度。

## 2. 前置知识
Day49–50。

## 3. 必须教学内容
1. Measurement Prediction：`z_hat=H x_hat^-`。
2. Innovation / Residual：`y=z-H x_hat^-`。
3. Innovation Covariance：`S=H P^- H^T + R`，同时包含prediction与measurement uncertainty。
4. Kalman Gain：`K=P^- H^T S^-1`。
5. State Update：`x_hat^+=x_hat^-+Ky`；K决定 residual 的多少用于修正state。
6. P大/R小 → 更偏measurement；P小/R大 → 更偏prediction；必须理解相对uncertainty而非死背K大小。
7. Covariance Update基础式：`P^+=(I-KH)P^-`。
8. Joseph Form：认识更数值稳定形式及工程原因。
9. **完整手算1D KF**：prediction、P、measurement、R → innovation、S、K、x^+、P^+。
10. Fusion不是simple average，而是uncertainty-aware weighting。

## 4. 深度要求
Innovation L4；Kalman Gain L4；1D KF L4；covariance update L3-L4。

## 5. 工程连接
RTK FIX/FLOAT、LIO、wheel、GPS covariance；强调“R很小”只有在covariance模型合理且没有systematic failure时才安全。

## 6. 明确不展开
EKF、asynchronous multi-rate、gating、observability。

## 7. 本日考核点
Innovation；S；K；R/P变化；fusion≠平均；P update；手算1D KF；covariance设太小而measurement错误的危险。

### M09毕业考试核心考点
Innovation、S、K、state update、covariance update。

---

# Day52 — EKF：Nonlinear Model、Jacobian 与 Error-State Intro

## 1. 今日目标
理解EKF的核心不是一套全新filter，而是把nonlinear model在当前estimate附近线性化后继续使用KF思想。

## 2. 前置知识
M02 Jacobian/Taylor + M08 probability + Day49–51。

## 3. 必须教学内容
1. Nonlinear system：`x_k=f(x_(k-1),u_k)+w`，`z_k=h(x_k)+v`。
2. Nonlinear Prediction：`x_hat^- = f(x_hat^+,u)`。
3. Prediction Jacobian：`F=∂f/∂x`，在当前state计算。
4. Measurement Prediction：`z_hat=h(x_hat^-)`。
5. Measurement Jacobian：`H=∂h/∂x`。
6. Innovation：`y=z-h(x_hat^-)`。
7. EKF Gain结构与KF一致：`K=P^-H^T(HP^-H^T+R)^-1`。
8. Euclidean update：`x_hat^+=x_hat^-+Ky`；rotation/manifold state以后需error-state/manifold update。
9. Linearization Error：initial estimate差、nonlinearity强、uncertainty大时Taylor近似可能很差。
10. Angle residual必须wrap，例如179°与-179°不能当358°。
11. Error-State EKF：nominal state + small error state → inject correction；只建立架构。
12. **Process noise mapping G（审查修正）**：当process noise定义在独立noise space时，covariance propagation应写成 `P^- = FPF^T + GQG^T`；若Q已直接定义在state space，可使用简化 `+Q`。必须理解G是“noise如何进入state”的Jacobian/映射。

## 4. 深度要求
EKF full chain L4；F/H L4；linearization limitations L4；G noise mapping L3-L4；error-state concept L2-L3。

## 5. 工程连接
Wheel+IMU、GNSS、orientation、FAST-LIO error state、robot_localization思想。

## 6. 明确不展开
Full 15/18D ESKF推导、UKF、particle filter、Lie EKF严格理论。

## 7. 本日考核点
KF为何不够；EKF F/H；linearization point；Jacobian错误；angle wrap；error-state；`FPF^T+Q` 与 `FPF^T+GQG^T`何时使用；G表示什么。

### M09毕业考试核心考点
EKF、F/H/G Jacobian、linearization、error-state intuition。

---

# Day53 — Wheel + IMU + GNSS / RTK Multi-sensor Fusion

## 1. 今日目标
把KF/EKF映射到真实机器人，设计和解释Wheel+IMU+GNSS/RTK融合。

## 2. 前置知识
Day49–52 + M03。

## 3. 必须教学内容
1. Sensor constraints：Wheel local motion/velocity；IMU angular velocity/specific force/high-rate；GNSS/RTK absolute global position。
2. Relative vs Absolute Measurement。
3. Drift Correction：relative sensor短时平滑但drift；absolute sensor提供long-term anchor但可能noise/dropout。
4. State Design：如 `[px,py,vx,vy,θ,bg,ba]^T`；bias进入state是为了长期估计/纠正。
5. Measurement Model：GNSS只观察position；wheel可约束forward velocity/yaw rate等。
6. Multi-rate Sensors：例如IMU 200Hz、Wheel 50Hz、GNSS 5Hz，不要求同频。
7. Asynchronous Update：高频prediction，哪个measurement到就更新哪个，具体实现依系统。
8. Timestamp：measurement必须尽量更新到它真正发生的state时刻；旧measurement不能直接当当前state。
9. Frame：earth/map、odom、base、imu等先对齐再fusion。
10. Covariance Tuning：Q/R是model与measurement uncertainty假设，不是“调平滑度”。
11. RTK FIX/FLOAT：solution quality应影响acceptance、R、gating，不只看topic有数据。
12. Heading：single GNSS position静止时不能天然提供可靠heading；motion-derived heading低速不稳定；dual-antenna可提供heading constraint。
13. Wheel Slip：measurement错误却R很小会把filter强行拉走。
14. Sensor Dropout：继续predict，P增长，confidence下降，必要时degraded mode。
15. Sensor Recovery：重新获得GNSS/RTK后需要consistency check/gating/reset/reinitialize策略，不能无条件猛拉state。

## 4. 深度要求
Sensor complementarity L4；multi-rate/asynchronous L3-L4；timestamp/frame L4；covariance reasoning L4。

## 5. 工程连接
`/wheel_odom`、`/gps/fix`、`/fastlio2/lio_odom`、RTK FIX/FLOAT、LIO latency、chassis feedback。

## 6. 明确不展开
GPS coordinate conversion细节、IMU preintegration、full ESKF equations。

## 7. 本日考核点
Wheel/GNSS complementarity；IMU drift；multi-rate；timestamp/frame；bias state；FIX/FLOAT R；wheel slip；dropout/recovery；single-GNSS heading限制。

### M09毕业考试核心考点
Multi-sensor fusion、multi-rate、timestamp/frame、covariance tuning。

---

# Day54 — Observability、Outlier、Gating、Dropout、Reset 与 Filter Debug

## 1. 今日目标
从“会算filter”提升到能判断什么时候根本估不出来、什么时候measurement该拒绝、什么时候该降级/重置。

## 2. 前置知识
Day49–53全部内容。

## 3. 必须教学内容
1. Observability：state里有某变量不代表sensor组合一定能估出来。
2. Observable vs Unobservable Direction：无measurement constraint的方向依赖model传播，uncertainty/drift增长。
3. Linear Observability Matrix：`O=[H;HF;HF^2;...]`，rank与可观测性；只做简单例子。
4. Innovation作为health signal：持续大residual可能来自sensor、state、frame、time、model错误。
5. Innovation Covariance S：判断residual异常不能只看绝对值，要结合预期uncertainty。
6. Mahalanobis Distance：`d²=y^T S^-1 y`，表示考虑uncertainty后的异常程度。
7. Gating：measurement→innovation→consistency test→accept/reject。
8. Outlier：GPS multipath等即使message状态正常也不应无条件接收。
9. Stale Measurement：数值可能正确但过旧；与outlier区分。
10. Dropout：继续prediction、P增长、confidence下降、必要时degraded mode。
11. Reset/Reinitialization：区分correction、reset、reinitialize；estimate完全失效时普通update未必能恢复。
12. Covariance Consistency：state明显错但P仍很小=overconfident/inconsistent。
13. Filter Divergence：residual增大、state跳变、covariance不合理、measurement连续reject等。
14. Common Root Causes：wrong Q/R、timestamp、frame、Jacobian、bias、initialization、measurement model、outlier、stale data。
15. Debug Evidence Chain：raw measurement→timestamp→frame/extrinsic→measurement model→innovation→Q/R→state/P→observability/degeneracy。

## 4. 深度要求
Observability L3-L4；gating/Mahalanobis L3；failure analysis L4-L5；reset/fallback L4。

## 5. 工程连接
RTK FLOAT→FIX、GPS multipath、LIO断流、wheel slip、LIO pose延迟、设备重启pose jump。

## 6. 明确不展开
Nonlinear observability Lie derivative、chi-square统计表深入、fault-tolerant filter完整设计。

## 7. 本日考核点
State存在≠observable；single GNSS heading；observability matrix rank；innovation与S；Mahalanobis/gating；stale vs outlier；dropout与P；reset；overconfidence；EKF drift排查顺序；GPS跳变故障树。

---

# M09 Graduation Exam Specification

## A. 核心理论专项 — 30%
必须覆盖：State/Measurement、F/H/G、Q/R/P、KF Prediction、Covariance Propagation、Innovation、Kalman Gain、KF Update、EKF、Jacobian、Linearization、Multi-rate Fusion、Timestamp/Frame、Observability、Mahalanobis/Gating、Dropout/Reset。F/H/Q/R/P/K 与 timestamp/frame 属硬门槛。

## B. 数学综合题 — 30%
1. 1D KF：给 `x^-、P^-、z、R`，完整计算innovation、S、K、x^+、P^+并解释结果。
2. Constant Velocity：构造F、prediction、P维度和Q影响。
3. EKF：给非线性h(x)，求H、measurement prediction、说明linearization point和bad initialization风险；解释GQG^T。

## C. 真实机器人综合场景 — 40%
至少3组：RTK/LIO/Wheel；LIO rate正常但pose age很大；GPS dropout/recovery。要求明确sensor constraint、R/Q/P、timestamp/frame、innovation/gating、degraded/reset策略。

## Owner综合题要求
拿rosbag按以下结构分析：State是什么？每个sensor直接测什么？frame？timestamp？Q/R/P？innovation？stale？unobservable direction？overconfidence？Sensor/Time/Frame/Model/Filter哪一层是根因？

## M09通过标准
- 总分 ≥85%；
- `F/H/G/Q/R/P/K`必须能解释；
- 必须完整手算简化KF；
- 必须解释EKF为何需要Jacobian以及G的noise mapping作用；
- Timestamp、Frame、Covariance为硬门槛；
- 不允许 covariance小 = 真实误差一定小；
- 不允许只通过“调Q/R看效果”诊断filter。

## Day49–Day54索引
```text
Day49  State / Motion Model / Measurement Model / Q-R-P
Day50  KF Prediction / Covariance Propagation
Day51  KF Update / Innovation / Kalman Gain
Day52  EKF / F-H-G Jacobian / Linearization / Error-State Intro
Day53  Wheel + IMU + GNSS/RTK Multi-sensor Fusion
Day54  Observability / Gating / Dropout / Reset / Debug
```
