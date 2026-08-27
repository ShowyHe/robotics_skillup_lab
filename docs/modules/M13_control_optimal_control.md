# M13 — Control & Optimal Control

## Module Goal
建立：

```text
Geometric Path
→ Time-parameterized Reference / Trajectory
→ Error / Feedback
→ Controller
→ Control Input
→ Robot Dynamics
→ New State
→ Feedback
```

再进入：

```text
Model + Horizon + Cost + Constraints
→ Optimize Future Controls
→ Execute First
→ Re-observe
```

本模块共 10 个理论 Day（Day80–Day89）。Navigation/Control源码主线坚持：公司真实实现 + 数学本体 + Nav2官方实现。

## 主要教材
- **Modern Robotics Chapter 9 — Trajectory Generation**：作为reference/time-scaling与机械臂trajectory基础。
- **Modern Robotics Chapter 11 — Robot Control**：作为robot feedback、model-based control的主要理论参考之一。
- 本课程在教材基础上继续系统学习 **Stability / Controllability / Observability / LQR / MPC / MPPI**，因此不会按教材章节顺序机械推进。

---

# Day80 — Trajectory / Time Scaling / Feedback / PID
1. 今日目标：先回答“controller到底在跟踪什么”，再建立PID闭环。
2. 前置：M12 state-space/dynamics/kinematics。
3. 必须教学：Path vs Geometric Trajectory vs Time-parameterized Trajectory；path parameter `s` 与time scaling `s(t)`；reference `q_d(t), qdot_d(t), qddot_d(t)`；boundary condition；cubic/quintic time-scaling概念；velocity/acceleration limit对trajectory duration的约束；trajectory≠逐点立即command；open/closed loop；`e=r-y`；P/I/D；steady-state error；overshoot/oscillation；discrete PID/sample time；saturation/rate limit；integral windup/anti-windup；derivative noise；command vs actual actuator input。
4. 深度：Trajectory/Time Scaling L3-L4；Feedback/PID L4。
5. 工程连接：arm joint trajectory、yaw/velocity control、MoveIt trajectory execution。
6. 不展开：jerk-optimal trajectory、full trajectory optimization、formal stability/LQR/MPC。
7. 考核：给path说明为何还缺time parameterization；解释P/I/D、windup、sample time、saturation。
8. 毕业考点：Path→Timed Reference、Feedback、PID、Command vs Actual Behavior。

# Day81 — Equilibrium / Stability / Eigenvalue / Closed-loop Response
1. 今日目标：理解“能运行”不等于“稳定”。
2. 前置：M12 state-space/eigenvalue。
3. 必须教学：equilibrium `f(x*,u*)=0`；对LTI或非线性系统在equilibrium附近local linearization，continuous `x_dot=Aδx` 中 `Re(λ)<0` 表示局部指数衰减，discrete `δx_(k+1)=A_dδx_k` 中 `|λ|<1` 表示局部离散稳定；不能脱离线性/局部线性语境泛化；pole；complex mode/oscillation；rise/overshoot/settling/steady-state error；state feedback `u=-Kx`；closed-loop `A-BK`；stability≠performance；delay对stability影响；Lyapunov quadratic intuition。
4. 深度：Stability/Eigenvalue L4。
5. 工程连接：S形摆动、arm overshoot。
6. 不展开：root locus/Bode、完整Lyapunov证明。
7. 考核：给continuous/discrete eigenvalues判断local linear stability并说明适用前提。
8. 毕业考点：Equilibrium、Stability、Closed-loop Dynamics。

# Day82 — Controllability / Observability / State Feedback
1. 今日目标：回答“输入能不能驱动需要的状态”和“measurement能不能恢复需要的state”。
2. 前置：M09 observability + M12 state-space。
3. 必须教学：`x_dot=Ax+Bu, y=Cx+Du`；controllability；`𝒞=[B,AB,...,A^(n-1)B]`；`rank(𝒞)=n`；physical coupling；uncontrollable unstable mode；observability；`𝒪=[C;CA;...;CA^(n-1)]`；`rank(𝒪)=n`；unobservable mode；actuator/sensor数量≠rank；state feedback/pole placement概念；LQR/MPC前提关系。
4. 深度：Controllability/Observability L3-L4。
5. 工程连接：mobile robot constraints、sensor feedback缺失。
6. 不展开：Hautus、nonlinear Lie tests。
7. 考核：小矩阵计算`rank(𝒞)` / `rank(𝒪)`并解释物理意义。
8. 毕业考点：Controllability、Observability、Structural Limits。

# Day83 — Optimal Control / LQR
1. 今日目标：理解state error与control effort trade-off。
2. 前置：M02 quadratic form + Day81。
3. 必须教学：linear discrete dynamics；LQR objective `J=Σ(x^TQx+u^TRu)`；Q/R物理意义；`u=-Kx`；Riccati/DARE概念；finite/infinite horizon；regulation/reference tracking；error state；LQR vs PID；model dependence；classic LQR不天然处理hard constraints。
4. 深度：Q/R/Cost L4；Riccati L2-L3。
5. 工程连接：trajectory tracking、arm/balance。
6. 不展开：Riccati严格推导、LQG/H∞。
7. 考核：Q/R变化如何影响行为。
8. 毕业考点：Optimal Control、LQR、Quadratic Cost。

# Day84 — MPC / Horizon / Constraint / Receding Horizon
1. 今日目标：理解有限时域受约束最优控制如何反复在线求解。
2. 前置：Day83。
3. 必须教学：prediction sequence；horizon N与`T=NΔt`；stage/terminal cost；state/input constraints；speed/accel/steering limits；receding horizon；execute first action；re-optimize；horizon/dt trade-off；feasibility；warm start；MPC vs LQR；reference trajectory如何进入cost。
4. 深度：Receding Horizon/Constraints L4。
5. 工程连接：local navigation/manipulation trajectory。
6. 不展开：QP/NMPC solver细节。
7. 考核：N、dt、T、reference与constraint推理。
8. 毕业考点：MPC、Horizon、Constraints。

# Day85 — MPPI / Sampling / Rollout / Weight / Update
1. 今日目标：理解MPPI优化future control sequence而不是global path。
2. 前置：M12 dynamics + Day84 + M08 probability。
3. 必须教学：nominal sequence `Ū`；`ε~N(0,Σ)`；candidate control；rollout `x_(t+1)=f(x_t,u_t)`；K samples；horizon；trajectory cost `S_k`；weight `w_k∝exp(-(S_k-ρ)/λ)`；`ρ=min S`数值稳定；normalized weights；update `ū_t←ū_t+Σ_k w_k ε_(k,t)`；不是只选best sample；execute first；shift/warm start；compute trade-off。
4. 深度：MPPI chain L4。
5. 工程连接：samples/dt/horizon/vx-wz noise/critics。
6. 不展开：path-integral随机控制严格证明。
7. 考核：简化sample cost/noise计算weight/update。
8. 毕业考点：Rollout、Weight、Update、Warm Start。

# Day86 — MPPI Noise / Temperature / Warm Start / Feedback Base
1. 今日目标：理解sampling distribution和上一周期解如何塑造搜索区域。
2. 前置：Day85。
3. 必须教学：`σ_v/σ_ω`；std太小/太大；control limit/clipping；temperature λ与weight尖锐度；effective samples；nominal sequence；shift；warm-start temporal consistency与stale风险；E-stop后old command、actual velocity、new nominal区别；initial rollout state来自明确state/feedback source；zero/nominal sample只是implementation detail；sample count与std是不同维度。
4. 深度：Sampling/Warm Start L4；Command-State Distinction L5。
5. 工程连接：急停恢复、odom feedback、MPPI internal state。
6. 不展开：adaptive covariance/CEM。
7. 考核：解释old command为什么不能代替急停后的actual wz。
8. 毕业考点：Sampling Distribution、Temperature、Warm Start、Feedback Semantics。

# Day87 — Cost / Critic / Constraint / Robot Behavior
1. 今日目标：理解robot behavior如何由多个cost/critic trade-off塑造。
2. 前置：Day85–86。
3. 必须教学：`S=Σw_iC_i`；path distance/alignment；goal；obstacle；velocity preference；smoothness/acceleration/jerk；soft vs hard constraint；collision不能只是普通小penalty；critic conflict；weight×raw cost实际贡献；scale/normalization；slow/hard-brake bands；footprint/state error/stopping distance/latency共同决定safety margin；critic attribution。
4. 深度：Cost L4；Behavior Attribution L5。
5. 工程连接：提前绕、绕后回拉、左右互搏。
6. 不展开：learned/social/risk-sensitive cost。
7. 考核：给critic raw/weight判断主导项。
8. 毕业考点：Critic Trade-off、Constraint、Behavior Attribution。

# Day88 — Tracking / Latency / Saturation / Model Mismatch
1. 今日目标：解释optimizer算对但real robot仍走差。
2. 前置：Day80–87。
3. 必须教学：path vs timed trajectory；lateral/heading/velocity error；actuator dynamics；velocity/acceleration saturation；model mismatch；sensor→estimate→planning→control→actuator latency；state age；delay-induced oscillation；feedback source/timestamp；controller frequency≠freshness；smoothing trade-off；safety override；prediction vs actual feedback；evidence chain。
4. 深度：Latency/Model Mismatch L5。
5. 工程连接：LIO stale、chassis feedback、S形摆动。
6. 不展开：Smith predictor/robust control。
7. 考核：20Hz controller + 400ms pose age为什么仍可能摆动。
8. 毕业考点：Tracking、Latency、Saturation、Closed-loop Reality。

# Day89 — Control Owner / MPPI Source Reading
1. 今日目标：形成 `Reference→State→Model→Prediction→Optimization→Control→Actuator→Feedback→Behavior` 证据链。
2. 前置：Day80–88。
3. 必须教学：Planner vs Trajectory Generator vs Controller；reference/state source/frame/time/freshness；model/dt/limits；sampling/horizon/std/warm-start；critic raw/weighted cost；clipping/smoothing/downstream limiter；actual feedback；behavior→candidate cause；E-stop recovery；MPPI源码定位 state/noise/rollout/model/critic/cost/weight/update/shift/constraint/publish；算法本体 vs Nav2接口 vs 公司额外策略；regression metrics；Modern Robotics Ch9/11与实际控制代码的映射方式。
4. 深度：Source Mapping/Failure Attribution L5。
5. 工程连接：公司MPPI + Nav2官方MPPI + arm trajectory/control reference。
6. 不展开：新增控制算法。
7. 考核：给源码/log定位控制链阶段和behavior root cause。
8. 毕业考点：Control Owner Evidence Chain。

---

# M13 Graduation Exam
统一权重：**30%核心基础 / 50%综合系统场景 / 20% Source·Formula·Design**。

## 30% 核心基础
硬门槛：Path/Timed Trajectory/Reference区别；Time Scaling基本语义；feedback/PID；local linear stability/eigenvalue；controllability/observability；LQR；MPC receding horizon/constraints；MPPI rollout-cost-weight-update；warm start；latency/saturation。

## 50% 综合系统场景
至少覆盖：path→timed trajectory→controller；`A-BK` stability；MPC horizon/constraint；MPPI weight/update；急停command≠actual state；stale LIO导致S形摆动；窄路critic/std/warm-start/horizon综合分析。

## 20% Source / Formula / Design
能够把Modern Robotics Ch9/11的trajectory/reference/control概念与MoveIt trajectory、公司MPPI、Nav2官方MPPI映射；在实际源码定位state、reference/path、motion model、sampling、rollout、critic、weight、update、warm start、clipping、feedback、publish。

## 通过标准
- 总分≥85%；
- 必须明确 `Planner Path ≠ Time-parameterized Reference ≠ Predicted Trajectory ≠ Controller Command ≠ Actual Motion`；
- 不能把旧command当真实state；
- 稳定性结论必须声明线性/LTI或局部线性化适用语境。
