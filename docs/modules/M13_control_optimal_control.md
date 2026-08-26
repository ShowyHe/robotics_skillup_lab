# M13 — Control & Optimal Control

## Module Goal
建立 `Reference → Error → Feedback → Controller → Control Input → Robot Dynamics → New State → Feedback`，再进入 `Model + Horizon + Cost + Constraints → Optimize Future Controls → Execute First → Re-observe` 的最优控制主线。

本模块共 10 个理论 Day（Day80–Day89）。Navigation/Control源码主线坚持：公司真实实现 + 数学本体 + Nav2官方实现。

---

# Day80 — Feedback / Open-loop / Closed-loop / PID
1. 今日目标：理解控制器不是“给一个速度”，而是基于feedback持续纠偏。
2. 前置：M12 state-space/dynamics。
3. 必须教学：open-loop；closed-loop；`e=r-y`；P `u=Kp e`；I `Ki∫e dt`；D `Kd de/dt`；PID三项作用；steady-state error；overshoot/oscillation；discrete PID；sample time；saturation；rate limit；integral windup/anti-windup；derivative noise；command vs actual actuator input。
4. 深度：Feedback/PID L4；saturation/anti-windup L3-L4。
5. 工程连接：yaw/velocity/arm position loop。
6. 不展开：formal stability/LQR/MPC。
7. 考核：解释P/I/D、windup、sample time、saturation。
8. 毕业考点：Feedback、PID、command vs actual behavior。

# Day81 — Equilibrium / Stability / Eigenvalue / Closed-loop Response
1. 今日目标：理解“能跑”不等于“稳定”。
2. 前置：M12 state-space/eigenvalue。
3. 必须教学：equilibrium `f(x*,u*)=0`；local stability直觉；continuous `x_dot=Ax`；`Re(λ)<0` 衰减、`>0` 增长；discrete `|λ|<1`；pole概念；complex mode/oscillation；rise time/overshoot/settling/steady-state error；state feedback `u=-Kx`；closed-loop `A-BK`；stability≠performance；delay对稳定性影响；Lyapunov quadratic intuition只做认识。
4. 深度：Stability/eigenvalue L4。
5. 工程连接：S形摆动、arm overshoot。
6. 不展开：root locus/Bode、完整Lyapunov证明。
7. 考核：给eigenvalues判断continuous/discrete stability。
8. 毕业考点：Equilibrium、stability、closed-loop dynamics。

# Day82 — Controllability / Observability / State Feedback
1. 今日目标：回答“能不能控制”和“能不能从measurement知道state”。
2. 前置：M09 observability intuition + M12 state-space。
3. 必须教学：`x_dot=Ax+Bu`；controllability；`C=[B,AB,...,A^(n-1)B]`；rank=n；physical coupling；uncontrollable unstable mode；observability；`O=[C;CA;...]`；unobservable mode；actuator/sensor数量不等于rank；state feedback/pole placement概念；与LQR/MPC结构前提关系。
4. 深度：Controllability/Observability L3-L4。
5. 工程连接：mobile robot constraints、sensor feedback缺失。
6. 不展开：Hautus、nonlinear Lie tests。
7. 考核：小矩阵rank判断并解释物理意义。
8. 毕业考点：Controllability、Observability、structural limits。

# Day83 — Optimal Control / LQR
1. 今日目标：从“误差变小”升级为state error与control effort整体trade-off。
2. 前置：M02 quadratic form + Day81。
3. 必须教学：linear discrete dynamics `x_(k+1)=Ax_k+Bu_k`；optimal control objective；LQR `J=Σ(x^TQx+u^TRu)`；Q/R物理意义；`u=-Kx`；Riccati/DARE概念；finite/infinite horizon；regulation vs reference tracking；error state；LQR vs PID；LQR对model依赖；经典LQR不天然处理hard constraints。
4. 深度：Cost/Q/R L4；Riccati L2-L3。
5. 工程连接：trajectory tracking、arm/balance。
6. 不展开：Riccati严格推导、LQG/H∞。
7. 考核：Q/R改变如何影响行为。
8. 毕业考点：Optimal control、LQR、quadratic cost。

# Day84 — MPC / Horizon / Constraint / Receding Horizon
1. 今日目标：理解有限时域受约束最优控制反复在线求解。
2. 前置：Day83。
3. 必须教学：prediction state/action sequence；horizon steps N与duration `T=NΔt`；stage/terminal cost；state/input constraints；speed/acceleration/steering limit；receding horizon；只执行第一步；为什么重新优化；horizon过短/过长；dt trade-off；feasibility；warm start概念；MPC vs LQR。
4. 深度：Receding horizon/constraints L4。
5. 工程连接：local navigation/trajectory/manipulation。
6. 不展开：QP/NMPC solver细节。
7. 考核：N、dt、T换算及约束推理。
8. 毕业考点：MPC、horizon、constraints、receding horizon。

# Day85 — MPPI Theory：Sampling / Rollout / Cost / Weight / Update
1. 今日目标：理解MPPI优化的是control sequence而不是global path。
2. 前置：M12 mobile dynamics + Day84 + probability basics。
3. 必须教学：nominal controls `Ū=[u0...uT-1]`；Gaussian noise `ε~N(0,Σ)`；candidate `u=ū+ε`；rollout `x_(t+1)=f(x_t,u_t)`；samples K；horizon T/dt；trajectory cost `S_k`；exponential weight `w_k∝exp(-S_k/λ)`；`ρ=min S`数值稳定；normalized weights；control update `ū_t←ū_t+Σw_k ε_(k,t)`；不是只选best sample；execute first control；shift/warm start；sampling compute trade-off。
4. 深度：MPPI chain L4。
5. 工程连接：samples/dt/horizon/vx-wz noise/critics。
6. 不展开：path-integral严格随机控制证明、CUDA。
7. 考核：给少量sample cost/noise计算简化weight/update。
8. 毕业考点：rollout、weight、update、warm start。

# Day86 — MPPI Noise / Temperature / Warm Start / Feedback Base
1. 今日目标：理解sampling distribution与上一周期解如何塑造搜索区域。
2. 前置：Day85。
3. 必须教学：`σ_v/σ_ω`；std太小/太大；control limits/clipping；temperature λ对weight尖锐度；effective samples；nominal sequence；warm-start temporal consistency；warm-start stale risk；shift；emergency stop后old command、actual velocity、new nominal sequence三者区别；initial rollout state应来自明确feedback/state source；zero/nominal sample作为implementation detail；sample count与std不是同一个调节维度。
4. 深度：Std/warm start L4；command-state distinction L5。
5. 工程连接：急停恢复、odom反馈、MPPI internal state。
6. 不展开：adaptive covariance/CEM。
7. 考核：解释old command为何不能替代急停后的actual wz。
8. 毕业考点：Sampling distribution、temperature、warm start、feedback semantics。

# Day87 — Cost / Critic / Constraint / Robot Behavior
1. 今日目标：理解机器人行为如何由多个cost/critic的trade-off塑造。
2. 前置：Day85–86。
3. 必须教学：`S=Σw_i C_i`；path distance/alignment；goal cost；obstacle cost；velocity preference；smoothness/acceleration/jerk；soft vs hard constraint；collision应invalid/极高代价而非普通小penalty；critic conflict；weight×raw cost实际贡献；scale/normalization；piecewise slow/hard-brake bands；footprint/state error/stopping distance/latency共同决定safety margin；critic attribution流程。
4. 深度：Cost decomposition L4；behavior attribution L5。
5. 工程连接：提前绕、绕后回拉、左右互搏、slow/hard brake。
6. 不展开：learned/social/risk-sensitive cost。
7. 考核：给critic raw/weight判断哪项主导行为。
8. 毕业考点：Critic trade-off、constraint、behavior attribution。

# Day88 — Tracking / Latency / Saturation / Model Mismatch
1. 今日目标：解释optimizer算对但real robot仍走差的原因。
2. 前置：Day80–87。
3. 必须教学：path vs trajectory；lateral/heading/velocity error；actuator dynamics；velocity/acceleration saturation；model mismatch；sensor→estimation→planning→controller→actuator latency chain；state age；delay-induced oscillation；feedback source和timestamp；controller frequency≠freshness；command smoothing trade-off；downstream safety override；prediction vs actual feedback；evidence链。
4. 深度：Latency/model mismatch L5。
5. 工程连接：LIO stale pose、chassis feedback、S形摆动。
6. 不展开：Smith predictor/robust control。
7. 考核：20Hz controller + 400ms pose age为何仍可能摆动。
8. 毕业考点：Tracking、latency、saturation、closed-loop reality。

# Day89 — Control Owner / MPPI Source Reading
1. 今日目标：形成 `State→Model→Prediction→Sampling/Optimization→Cost→Control→Actuator→Feedback→Behavior` 证据链。
2. 前置：Day80–88。
3. 必须教学：Planner vs Controller责任；state acquisition audit；pose/velocity source/frame/time/freshness；model/dt/limits audit；sampling/horizon/std/warm-start audit；critic/raw/weighted cost；clipping/smoothing/downstream limiter；actual feedback；behavior→candidate cause mapping；急停恢复；MPPI源码定位：state/noise/rollout/model/critic/cost/weight/update/shift/constraint/publish；算法本体 vs Nav2接口 vs 公司额外策略；回归指标。
4. 深度：Source mapping/failure attribution L5。
5. 工程连接：公司MPPI + Nav2官方MPPI。
6. 不展开：不新增控制算法。
7. 考核：给源码片段/日志定位其处于MPPI哪一阶段及行为根因。
8. 毕业考点：Control Owner evidence chain。

---

# M13 Graduation Exam
统一权重：**30%核心基础 / 50%综合系统场景 / 20% Source·Formula·Design**。

## 30% 核心基础
硬门槛：feedback、PID、stability/eigenvalue、controllability、LQR cost、MPC receding horizon/constraints、MPPI rollout-cost-weight-update、warm start、latency/saturation。

## 50% 综合系统场景
至少覆盖：
1. `A-BK`与eigenvalue stability；
2. MPC N/dt/horizon/constraint变更；
3. MPPI sample cost/weight/update简算；
4. 急停前command wz≠急停后actual wz的恢复设计；
5. LIO stale约400ms导致控制S形摆动；
6. 窄路critic conflict/std/warm-start/horizon综合分析。

## 20% Source / Formula / Design
必须在公司MPPI和Nav2官方MPPI中定位：state、motion model、sampling、rollout、critic、weight、update、warm start、clipping、feedback、publish。公司源码语义以实际读取为准。

## 通过标准
总分≥85%；必须明确 `Planner Path ≠ Predicted Trajectory ≠ Controller Command ≠ Actual Motion`；不能把旧command当真实state；必须能证明行为来自critic/latency/planner中的哪一层。

## Day80–Day89 索引
```text
Day80 Feedback / PID
Day81 Equilibrium / Stability / Eigenvalue
Day82 Controllability / Observability
Day83 Optimal Control / LQR
Day84 MPC / Horizon / Constraint / Receding Horizon
Day85 MPPI / Sampling / Rollout / Weight / Update
Day86 MPPI Noise / Temperature / Warm Start
Day87 Cost / Critic / Constraint / Behavior
Day88 Tracking / Latency / Saturation / Model Mismatch
Day89 Control Owner / MPPI Source
```
