# M09 — State Estimation

## Module Goal
把 M03 Sensors 与 M08 Probability/Optimization连接起来，建立 `Motion Model → Prediction → Measurement → Innovation → Gain/Optimization → Corrected State + Uncertainty` 主线，并能解释 `x/P/F/G/Q/H/R/K`、timestamp、frame、observability 与 health。

本模块共 6 个理论 Day（Day49–Day54）。

---

# Day49 — State / Motion Model / Measurement Model / Q-R-P
1. 今日目标：严格区分 true state、estimate、control、measurement、model 与 uncertainty。
2. 前置：M03 + M08 Bayes/Gaussian/Covariance。
3. 必须教学：state设计；true `x` vs estimate `x_hat`；control `u`；linear/nonlinear motion model；measurement model；process noise `Q`；measurement noise `R`；state covariance `P`；Q/R/P边界；Markov intuition；command≠actual state。
4. 深度：State/Model/Q-R-P L4。
5. 工程连接：Wheel/IMU/GNSS/RTK/LIO/chassis feedback。
6. 不展开：Kalman Gain/EKF Jacobian。
7. 考核：设计Wheel+GPS state/measurement并解释Q/R/P。
8. 毕业考点：State、Motion/Measurement Model、Q/R/P。

# Day50 — Kalman Prediction / Covariance Propagation
1. 今日目标：理解无新measurement时state和uncertainty怎样传播。
2. 前置：Day49 + M02 matrix。
3. 必须教学：linear Gaussian KF条件；`x^- = Fx^+ + Bu`；constant-velocity model；`P^- = F P^+ F^T + Q`；linear covariance transform；Q为何累积；dt作用；open-loop drift；frequency≠accuracy；dimension checks。
4. 深度：Prediction/Covariance L4。
5. 工程连接：wheel DR、GPS dropout、IMU propagation。
6. 不展开：measurement update、ESKF。
7. 考核：手算CV prediction和P传播。
8. 毕业考点：Prediction、F、Q、P propagation。

# Day51 — Kalman Update / Innovation / Kalman Gain
1. 今日目标：理解measurement如何按相对uncertainty修正prediction。
2. 前置：Day49–50。
3. 必须教学：`z_hat=Hx^-`；innovation `y=z-Hx^-`；`S=HP^-H^T+R`；`K=P^-H^TS^-1`；`x^+=x^-+Ky`；P/R变化对K的影响；`P^+=(I-KH)P^-`；Joseph form概念；完整1D KF；fusion≠simple average；R小但systematic error的危险。
4. 深度：Innovation/K/1D KF L4。
5. 工程连接：RTK FIX/FLOAT、LIO/GPS covariance。
6. 不展开：EKF/gating。
7. 考核：完整手算一次1D update并解释每项。
8. 毕业考点：Innovation、S、K、State/P Update。

# Day52 — EKF / F-H-G Jacobian / Error-State Intro
1. 今日目标：理解nonlinear model如何局部线性化后继续使用KF思想。
2. 前置：M02 Jacobian/Taylor + Day49–51。
3. 必须教学：`x^-=f(x,u)`；`F=∂f/∂x`；`z_hat=h(x^-)`；`H=∂h/∂x`；innovation；EKF gain；linearization point/error；angle residual wrap；nominal+error-state概念；process noise mapping `G`；当Q在noise space时 `P^-=FPF^T+GQG^T`，Q已在state space时可简化 `+Q`；manifold state普通加法的边界。
4. 深度：EKF/F-H L4；G mapping L3-L4；ESKF concept L2-L3。
5. 工程连接：Wheel+IMU、robot_localization、FAST-LIO error state。
6. 不展开：15/18D ESKF完整推导、UKF/PF。
7. 考核：求简单H/Jacobian；解释GQG^T与bad initialization。
8. 毕业考点：EKF、F/H/G、Linearization、Error-State intuition。

# Day53 — Wheel / IMU / GNSS / RTK Multi-sensor Fusion
1. 今日目标：把Filter数学映射到真实多sensor机器人。
2. 前置：Day49–52 + M03。
3. 必须教学：relative vs absolute measurement；sensor complementarity；state含bias；GNSS position/wheel velocity等measurement model；multi-rate/asynchronous update；measurement timestamp；frame alignment；Q/R不是“平滑度旋钮”；RTK FIX/FLOAT quality；single-GNSS static heading限制；dual-antenna heading；wheel slip；dropout时predict+P增长；recovery consistency/gating/reset；stale measurement不能当current state。
4. 深度：Fusion/timestamp/frame/covariance reasoning L4。
5. 工程连接：`/wheel_odom`、`/gps/fix`、`/fastlio2/lio_odom`。
6. 不展开：GNSS坐标转换细节、full IMU preintegration。
7. 考核：设计IMU/Wheel/GNSS multi-rate fusion与dropout/recovery策略。
8. 毕业考点：Multi-sensor Fusion、Timestamp/Frame、Covariance Tuning。

# Day54 — Observability / Gating / Dropout / Reset / Debug
1. 今日目标：判断什么时候“根本估不出来”、measurement何时应拒绝、何时需要降级/重置。
2. 前置：Day49–53。
3. 必须教学：observability；observable/unobservable direction；linear observability matrix；innovation as health signal；innovation covariance；Mahalanobis `d²=y^TS^-1y`；gating；outlier vs stale；dropout；reset/reinitialize；covariance consistency/overconfidence；filter divergence；common roots：Q/R/time/frame/Jacobian/bias/init/model；debug evidence chain。
4. 深度：Observability L3-L4；Gating L3；Failure analysis L4-L5。
5. 工程连接：RTK recovery、GPS multipath、LIO dropout/latency、wheel slip。
6. 不展开：nonlinear Lie observability、chi-square表深入。
7. 考核：给GPS jump + LIO stale构造innovation/gating/reset排查树。
8. 毕业考点：Observability、Gating、Dropout/Reset、Consistency。

---

# M09 Graduation Exam
统一权重：**30%核心基础 / 50%综合系统场景 / 20% Source·Formula·Design**。

## 30% 核心基础
硬门槛：State/Measurement、F/H/G、Q/R/P、KF Prediction/Update、Innovation/K、EKF/Jacobian、Timestamp/Frame、Observability、Gating、Dropout/Reset。

## 50% 综合系统场景
至少覆盖：
1. 1D KF完整手算并解释P/R/K；
2. Constant-velocity F与covariance propagation；
3. 非线性measurement求H并解释linearization/GQG^T；
4. RTK/LIO/Wheel multi-rate fusion、FIX/FLOAT、heading限制；
5. LIO rate正常但pose age很大；
6. GPS dropout/recovery与gating/reset；
7. covariance很小但state明显错误的consistency问题。

## 20% Source / Formula / Design
能够在一个真实/官方state-estimation implementation中定位：state definition、prediction、F/G/Q、measurement/H/R、innovation/K或优化update、timestamp/frame、gating/reset/health；并能用bag/log设计证据链。

## 通过标准
总分≥85%；`F/H/G/Q/R/P/K`、timestamp、frame为硬门槛；必须会手算简化KF；不得把 covariance 小直接等同真实误差一定小，也不得只通过“调Q/R看效果”诊断filter。
