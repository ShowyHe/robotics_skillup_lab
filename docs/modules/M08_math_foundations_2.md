# M08 — Mathematical Foundations II

## Module Goal
建立 State Estimation、SLAM/LIO/VIO、Optimization、Control、Manipulation 共用的第二层数学底座：

```text
Probability / Bayes
↓
Gaussian / Covariance
↓
Likelihood / MLE / MAP
↓
Residual
↓
Least Squares / Weighted LS
↓
Nonlinear Optimization
↓
Newton / Gauss-Newton / LM
↓
Rotation / Quaternion
↓
SO(3) / SE(3)
↓
Robot State Estimation / SLAM
```

本模块共 9 个理论 Day（Day40–Day48），默认不安排强制实验。Day47 为重理论日，若 2–3h 内未完全消化，可跨下一学习时段继续，但课程编号仍保持一个 Day，不为赶进度压缩核心理解。

---

# Day40 — Probability、Conditional Probability 与 Bayes

## 1. 今日目标
理解机器人是在有限、带噪观测下不断更新对世界状态的 belief，而不是直接知道真实世界。

## 2. 前置知识
M02 function/vector/basic integration intuition。

## 3. 必须教学内容
1. Random Variable：区分 physical quantity、random variable、observed value。
2. Probability Distribution `p(x)`：描述不同可能状态的概率/密度。
3. Discrete vs Continuous：工程级理解。
4. Joint Probability `p(x,y)`。
5. Marginal Probability：通过求和/积分消去不关心变量。
6. Conditional Probability：`p(x|y)=p(x,y)/p(y)`，理解已知 y 后对 x 的 belief 改变。
7. Independence：`p(x,y)=p(x)p(y)` 仅在独立时成立。
8. Bayes Rule：`p(x|z)=p(z|x)p(x)/p(z)`；prior、likelihood、posterior、evidence。
9. Robot Bayes Interpretation：prior + sensor likelihood → posterior。
10. Sequential Update：posterior 可成为下一时刻 prior。

## 4. 深度要求
Conditional probability L3；Bayes L3-L4；robot interpretation L4。

## 5. 工程连接
GPS/LIO fusion、localization、sensor fusion、object existence。

## 6. 明确不展开
Kalman Filter、particle filter、measure theory、Bayesian network。

## 7. 本日考核点
Random variable vs measurement；joint/conditional；independence；Bayes四项；prior/posterior；likelihood≠posterior；GPS定位Bayes更新解释。

### M08毕业考试核心考点
Probability、conditional probability、Bayes、prior/likelihood/posterior。

---

# Day41 — Expectation、Variance、Covariance 与 Gaussian

## 1. 今日目标
理解“估计值 + 不确定性”与单纯一个数值的根本区别。

## 2. 前置知识
Day40。

## 3. 必须教学内容
1. Expectation `E[X]`：随机变量平均中心，不是下一次测量必然值。
2. Variance `Var(X)=E[(X-μ)^2]`。
3. Standard Deviation `σ=sqrt(Var)`，注意单位与variance区别。
4. Covariance `Cov(X,Y)`：两个变量共同变化关系。
5. Covariance Matrix：对角为variance，非对角为cross-covariance。
6. **Covariance ≠ Actual Error**：它是模型/算法对不确定性的表达，不保证真实误差；systematic bias可在covariance很小时仍造成严重错误。
7. Gaussian：`X~N(μ,σ²)`，理解mean/variance/shape。
8. Multivariate Gaussian：`x~N(μ,Σ)`；Σ决定 uncertainty geometry。
9. Covariance Ellipse：eigenvector决定方向，eigenvalue决定spread，复用M02。
10. Correlation：归一化covariance概念。

## 4. 深度要求
Expectation/variance L3；covariance L4；Gaussian geometry L3-L4。

## 5. 工程连接
GNSS covariance、EKF pose covariance、localization uncertainty、sensor confidence。

## 6. 明确不展开
Chi-square、confidence interval严格统计、Gaussian mixture。

## 7. 本日考核点
Expectation；variance/std；covariance正负；matrix diagonal/off-diagonal；covariance小是否一定真实准确；Gaussian μ/σ；covariance ellipse；position covariance不能机械等同“误差多少米”。

### M08毕业考试核心考点
Variance、covariance、Gaussian、covariance geometry。

---

# Day42 — Likelihood、MLE、MAP 与从数据反推参数

## 1. 今日目标
理解拿到一组 observations 后如何寻找最能解释这些数据的 state / parameter。

## 2. 前置知识
Day40–41。

## 3. 必须教学内容
1. Probability vs Likelihood：parameter给定看data概率 vs data给定把概率看作parameter函数。
2. Likelihood：`L(θ)=p(D|θ)`。
3. 条件独立measurement：`p(D|θ)=∏p(z_i|θ)`。
4. MLE：`θ_MLE=argmax p(D|θ)`。
5. Log Likelihood：乘积转求和，便于数值/优化。
6. Negative Log Likelihood：最大likelihood转minimize NLL。
7. MAP：`θ_MAP=argmax p(θ|D)=argmax p(D|θ)p(θ)`。
8. MLE vs MAP：data only vs data + prior。
9. Gaussian Noise → Least Squares：`r_i=z_i-h_i(x)`，Gaussian likelihood 与平方残差目标的关系。

## 4. 深度要求
Likelihood L3；MLE/MAP L3-L4；Gaussian→LS L3。

## 5. 工程连接
SLAM pose optimization、calibration、sensor fusion、neural-network loss。

## 6. 明确不展开
Bayesian model selection、conjugate prior、information theory深入。

## 7. 本日考核点
Probability/likelihood；MLE/MAP；log likelihood；NLL；Gaussian noise为何导出LS；prior太强对MAP的影响。

### M08毕业考试核心考点
Likelihood、MLE/MAP、Gaussian likelihood→least squares。

---

# Day43 — Residual、Least Squares 与 Weighted Least Squares

## 1. 今日目标
理解机器人估计问题常见本质：寻找一个 state，使多个 measurement residual 按其可信度整体最小。

## 2. 前置知识
M02 Jacobian + Day42。

## 3. 必须教学内容
1. Measurement Model：`z=h(x)+noise`。
2. Residual：`r=z-h(x)`；区分measurement、prediction、residual。
3. Linear LS：`Ax≈b`，目标 `min ||Ax-b||²`。
4. Normal Equation：`A^T A x=A^T b`，形式解概念；强调工程上不应习惯性显式求 inverse。
5. Overdetermined System：方程多于unknowns，寻找best fit。
6. Geometric Projection：LS可理解为把 b 投影到 A 的 column space。
7. WLS：`min r^TWr`，不同 measurement 权重不同。
8. Weight与Covariance：Gaussian场景 `W=Σ^-1`；uncertainty大→weight小。
9. Information Matrix：`Ω=Σ^-1`。
10. Correlated Measurement：非对角covariance代表residual维度不独立。

## 4. 深度要求
Residual L4；LS L3-L4；WLS L4；covariance weighting L4。

## 5. 工程连接
GPS+LIO、ICP、calibration、factor graph、pose fitting。

## 6. 明确不展开
Sparse solver、QR/Cholesky详细算法、Gauss-Newton。

## 7. 本日考核点
Residual；为何不能exact fit；LS目标；A^TA维度；inverse问题；WLS；covariance与weight；information matrix；两种精度不同sensor的weight判断。

### M08毕业考试核心考点
Residual、LS、WLS、covariance/information weighting。

---

# Day44 — Nonlinear Least Squares、Gradient 与 Newton Method

## 1. 今日目标
理解为什么SLAM、Calibration、Pose Estimation通常需要 iterative optimization。

## 2. 前置知识
M02 Taylor/Jacobian/Hessian + Day43。

## 3. 必须教学内容
1. Nonlinear residual：`r(x)=z-h(x)`；`min ||r(x)||²`。
2. Iterative optimization：initial guess→residual→derivative→Δx→update→repeat。
3. Gradient Descent回顾：`x_{k+1}=x_k-α∇f(x_k)`。
4. First-order Taylor回顾。
5. Second-order Taylor：`f(x+Δx)≈f(x)+g^TΔx+1/2 Δx^T H Δx`。
6. Newton Method：`Δx=-H^-1 g`；利用curvature求step。
7. GD vs Newton：一阶 vs 二阶信息、step与计算成本。
8. Initial Guess：可能converge、wrong local solution、diverge。
9. Local Minimum：非线性问题不保证唯一global optimum。
10. Convergence：cost下降、update变小、gradient变小等停止指标。

## 4. 深度要求
Nonlinear LS L3；Newton L3；Taylor→optimization L4；initialization L4。

## 5. 工程连接
SLAM、ICP、calibration、pose estimation。

## 6. 明确不展开
完整convex optimization、line search细节、trust-region理论。

## 7. 本日考核点
Linear vs nonlinear LS；为何迭代；GD/Newton信息差异；Hessian；initial guess；local/global；convergence。

### M08毕业考试核心考点
Nonlinear optimization、Taylor、Hessian、Newton、initialization。

---

# Day45 — Gauss-Newton、Levenberg-Marquardt 与 Robust Optimization

## 1. 今日目标
理解SLAM/Calibration源码里常见的 residual、Jacobian、J^TJ、GN、LM 和 degeneracy。

## 2. 前置知识
Day43–44。

## 3. 必须教学内容
1. Residual linearization：`r(x+Δx)≈r(x)+JΔx`。
2. Local LS：`min_Δx ||r+JΔx||²`。
3. GN normal equation：`J^TJ Δx=-J^T r`。
4. Approximate Hessian：`H≈J^TJ`。
5. State update：`x←x+Δx`；后续SO(3)/SE(3) pose不总能普通加法。
6. LM：`(J^TJ+λI)Δx=-J^Tr`；λ小更像GN，λ大更保守。
7. Damping帮助bad initialization / poor conditioning。
8. Outlier：平方残差会放大大残差影响。
9. Robust Loss：Huber/Cauchy概念，降低异常 residual 支配。
10. Degeneracy / Conditioning：观测不足时 `J^TJ` 接近singular，联系SVD/eigen。

## 4. 深度要求
GN推导逻辑 L4；normal equation L4；LM L3；robust loss L2-L3；degeneracy L3-L4。

## 5. 工程连接
ICP、LiDAR SLAM、VIO、camera calibration、factor graph。

## 6. 明确不展开
Ceres/G2O源码、sparse Schur complement、trust-region严格理论。

## 7. 本日考核点
GN为何需要Jacobian；J^TJ/J^Tr维度与含义；GN vs Newton；LM与λ；outlier；robust loss；degenerate direction。

### M08毕业考试核心考点
Gauss-Newton、LM、residual/Jacobian/update、degeneracy。

---

# Day46 — 3D Rotation、Rotation Matrix、Euler 与 Quaternion

## 1. 今日目标
理解robot orientation不能像普通XYZ位置一样随便加减。

## 2. 前置知识
M02 matrix/basis。

## 3. 必须教学内容
1. 3D rotation与coordinate frame orientation。
2. Rotation Matrix：`R∈R^(3×3)`，满足 `R^TR=I`、`det(R)=1`。
3. Columns of R：某frame basis在另一个frame中的表示。
4. Composition：`R_ac=R_ab R_bc`，右侧先作用。
5. Inverse：`R^-1=R^T`。
6. Euler Angles：roll/pitch/yaw；只是parameterization，不是rotation本身。
7. Gimbal Lock：Euler representation singularity概念。
8. Quaternion：常见 `q=[w,x,y,z]` 或其他convention；必须核对库的顺序和方向定义。
9. Unit Quaternion：rotation quaternion需normalize。
10. Quaternion composition：multiplication表示rotation composition。
11. Sign Ambiguity：q与-q表示同一rotation。
12. SLERP：适合rotation interpolation的概念。

## 4. 深度要求
Rotation matrix L4；frame composition L4；Euler L3；quaternion L3。

## 5. 工程连接
TF、IMU orientation、LIO pose、manipulator pose。

## 6. 明确不展开
Quaternion代数证明、dual quaternion、SO(3) exp/log。

## 7. 本日考核点
Rotation matrix约束；column含义；composition/inverse；Euler问题；quaternion normalization/sign/convention。

### M08毕业考试核心考点
Rotation matrix、composition、Euler limitation、quaternion。

---

# Day47 — SO(3)、SE(3)、Lie Algebra 与 Exp/Log

## 1. 今日目标
理解 robot pose 属于非欧空间，而 optimization 需要可加的小增量；Lie algebra 用局部向量连接二者。

## 2. 前置知识
Day46 + M02 Jacobian/Taylor。

## 3. 必须教学内容
1. SO(3)：所有合法3D rotation组成的空间：`R^TR=I, det(R)=1`。
2. Rotation不是普通Euclidean vector：matrix元素直接相加通常不再合法。
3. Lie Group：连续空间 + group composition 的工程概念，不做严格群论证明。
4. so(3)：rotation附近的小变化可用3D vector表达。
5. Hat Operator：`ω∈R^3 → ω^` skew-symmetric matrix。
6. Exp Map：`R=exp(ω^)`，把局部rotation increment映射回SO(3)。
7. Log Map：把rotation difference映射回局部vector。
8. SE(3)：`T=[[R,t],[0,1]]` 表示rigid pose。
9. SE(3) 6 DoF：3 translation + 3 rotation。
10. se(3)：local pose increment `ξ∈R^6`。
11. Pose Perturbation：current pose + local increment → Exp(ξ) → valid pose update。
12. Left vs Right Perturbation：只要求理解 `Exp(δξ^)T` 与 `TExp(δξ^)` convention不同，读源码必须先确认。
13. Why SLAM uses Lie Algebra：optimization喜欢vector increment，而pose位于manifold。

## 4. 深度要求
SO(3)/SE(3) L4；Exp/Log物理意义 L3-L4；perturbation L4。

## 5. 工程连接
FAST-LIO、ICP、pose graph、VIO、manipulation pose、TF。

## 6. 明确不展开
**不要求** Adjoint、BCH、SO(3)/SE(3) left/right Jacobian完整推导、manifold严格证明。若本日2–3h不足，允许跨下一学习时段继续，不压缩核心理解。

## 7. 本日考核点
SO(3)；rotation为何不能普通相加；Lie algebra；Exp/Log；SE(3) 6DoF；6D increment；left/right perturbation convention；FAST-LIO为何不能随便加rotation matrix。

### M08毕业考试核心考点
SO(3)、SE(3)、Exp/Log、pose perturbation。

---

# Day48 — Probability + Optimization + SE(3) 综合：SLAM数学主链

## 1. 今日目标
把M08全部连成：`Sensor→Probability/Covariance→Residual→Likelihood→WLS→Jacobian→GN/LM→SE(3) Update→New Pose`。

## 2. 前置知识
Day40–47全部内容。

## 3. 必须教学内容
1. State：待估计变量集合，如position/orientation/bias等。
2. Measurement `z`。
3. Prediction `z_hat=h(x)`。
4. Residual `r=z-h(x)`。
5. Measurement covariance `Σ`。
6. Weighted residual `r^TΣ^-1r`。
7. Multi-sensor objective：`J(x)=Σ_i r_i^T Ω_i r_i`。
8. Residual Jacobian：`J_i=∂r_i/∂x`。
9. GN structure：`HΔx=-g`，常见 `H=ΣJ_i^TΩ_iJ_i`、`g=ΣJ_i^TΩ_i r_i`。
10. Euclidean vs SO(3)/SE(3) state update。
11. Iterate：linearize→solve→update→recompute residual。
12. Unobservable/Degenerate Direction：缺constraint时normal matrix condition变差，uncertainty/drift增大。
13. Outlier：需要gating/robust loss/rejection。
14. Estimation vs Optimization：state estimation是推断目标，optimization是实现手段之一。
15. M09/M10接口：M09学predict/update/covariance propagation；M10学ICP/VIO/factor graph residual与pose optimization。

## 4. 深度要求
全数学链 L4；weighted residual L4；Jacobian→GN L4；SE(3) update L3-L4。

## 5. 工程连接
GPS/LIO、FAST-LIO、ICP、factor graph、VIO。

## 6. 明确不展开
KF完整推导、factor graph完整理论、IMU preintegration、FAST-LIO具体公式。

## 7. 本日考核点
State/measurement/prediction/residual；为何乘Σ^-1；multi-sensor objective；Jacobian；J^TΩJ；pose update；缺constraint；outlier；estimation vs optimization；完整SLAM数学主链。

---

# M08 Graduation Exam Specification

## A. 核心数学专项 — 30%
必须覆盖：Conditional Probability、Bayes（硬门槛）、Expectation/Variance、Covariance（硬门槛）、Gaussian、Likelihood、MLE/MAP、Residual（硬门槛）、LS（硬门槛）、WLS（硬门槛）、Information Matrix、Nonlinear LS、Newton、Gauss-Newton（硬门槛）、LM、Robust Loss、Rotation Matrix、Quaternion、SO(3)、SE(3)、Exp/Log、Pose Perturbation（硬门槛）。

## B. 综合数学场景 — 50%
至少4组：Bayesian Sensor Fusion；Weighted LS（比较不同σ/variance/information/weight）；Nonlinear Optimization（linearize→GN→Δx→update）；SE(3) Robot Pose（composition/inverse/transform direction/6D perturbation）。

## C. SLAM数学架构题 — 20%
独立解释：`Sensor→z→h(x)→r→Σ→Weighted Cost→J→GN/LM→Δx→SE(3) Update`，说明输入、输出、数学意义和错误传播。

## M08通过标准
- 总分 ≥85%；
- Bayes、Covariance、Residual、WLS、Gauss-Newton、Pose Perturbation为硬门槛；
- 不允许把 covariance 小直接等同真实误差一定小；
- 不允许只背“GN就是求J”，必须解释 residual→linearization→normal equation→update；
- 必须理解为何SLAM rotation需要SO(3)/SE(3)。

## Day40–Day48索引
```text
Day40  Probability / Conditional Probability / Bayes
Day41  Expectation / Variance / Covariance / Gaussian
Day42  Likelihood / MLE / MAP
Day43  Residual / Least Squares / Weighted LS
Day44  Nonlinear LS / Gradient / Newton
Day45  Gauss-Newton / LM / Robust Optimization
Day46  Rotation Matrix / Euler / Quaternion
Day47  SO(3) / SE(3) / Exp-Log / Perturbation
Day48  Probability + Optimization + SE(3) 综合
```
