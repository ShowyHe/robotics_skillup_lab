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
Residual / LS / WLS
↓
Nonlinear Optimization / GN / LM
↓
Rotation / Quaternion
↓
SO(3) / SE(3) / Exp-Log / Pose Perturbation
↓
M09/M10 Estimation & SLAM + M12 Robot Kinematics
```

本模块共 9 个理论 Day（Day40–Day48），默认不安排强制实验。Day47 为重理论日，若 2–3h 内未完全消化，可跨下一学习时段继续，但课程编号仍保持一个 Day。

## 主要教材
- **Modern Robotics — Kevin M. Lynch & Frank C. Park，Chapter 3: Rigid-Body Motions**：Day46–Day47 的主要教材参考。
- 本模块只建立 `SO(3) / SE(3) / Lie algebra / Exp-Log / perturbation` 基础；**Screw Axis / Twist / Wrench / Adjoint 的正式计算和机器人运动学使用放在 M12**，避免提前堆叠数学。
- 教材章节用于补强理论骨架，不改变本课程原有 Probability / Optimization / SLAM 数学主线。

---

# Day40 — Probability、Conditional Probability 与 Bayes
## 1. 今日目标
理解机器人是在有限、带噪观测下不断更新对世界状态的 belief，而不是直接知道真实世界。
## 2. 前置知识
M02 function/vector/basic integration intuition。
## 3. 必须教学内容
1. Random Variable：physical quantity、random variable、observed value。
2. Probability Distribution `p(x)`；discrete vs continuous。
3. Joint / Marginal Probability。
4. Conditional Probability：`p(x|y)=p(x,y)/p(y)`。
5. Independence：只有独立时 `p(x,y)=p(x)p(y)`。
6. Bayes：`p(x|z)=p(z|x)p(x)/p(z)`；prior/likelihood/posterior/evidence。
7. Robot interpretation：prior + sensor likelihood → posterior。
8. Sequential update：posterior → next prior。
## 4. 深度要求
Conditional probability L3；Bayes L3-L4；robot interpretation L4。
## 5. 工程连接
GPS/LIO fusion、localization、sensor fusion、object existence。
## 6. 明确不展开
Kalman Filter、particle filter、measure theory。
## 7. 本日考核点
Random variable vs measurement；joint/conditional；Bayes四项；likelihood≠posterior；GPS定位Bayes更新。
### M08毕业考试核心考点
Probability、conditional probability、Bayes。

---

# Day41 — Expectation、Variance、Covariance 与 Gaussian
## 1. 今日目标
从 Day16 的传感器不确定性基础，推进到多维 state uncertainty 与 Gaussian geometry。
## 2. 前置知识
Day40 + M03 Day16；M02 eigen geometry。
## 3. 复用回顾
Variance、Standard Deviation、Covariance 基本含义与单位；**Covariance ≠ Actual Error**。已稳定通过则不重新从头教学。
## 4. 新增教学内容
1. Expectation `E[X]`。
2. Covariance Matrix：diagonal variance / cross-covariance。
3. Gaussian `N(μ,σ²)` 与 Multivariate Gaussian `N(μ,Σ)`。
4. Covariance ellipse：eigenvector方向、eigenvalue spread。
5. Correlation 与 covariance 的区别。
6. systematic bias 与 covariance 的边界继续用于多维场景。
## 5. 知识连接
把 Day16 的“单个 sensor measurement uncertainty”连接到 `state vector + covariance matrix`：后续 KF / EKF 不只维护一个估计值，还维护各 state dimension 及其相关性的 uncertainty structure。
## 6. 深度要求
Expectation L3；Covariance Matrix L4；Gaussian geometry L3-L4。
## 7. 工程连接
GNSS covariance、EKF covariance、localization uncertainty。
## 8. 明确不展开
Chi-square、严格confidence interval、Gaussian mixture。
## 9. 本日考核点
重点考 expectation、covariance matrix/cross-covariance、Gaussian geometry、correlation；variance/std 只做必要迁移检查，不机械重复 Day16 基础题。
### M08毕业考试核心考点
Variance、Covariance Matrix、Gaussian。

# Day42 — Likelihood、MLE、MAP
## 1. 今日目标
理解根据observations寻找最能解释数据的state/parameter。
## 2. 前置知识
Day40–41。
## 3. 必须教学内容
1. Probability vs Likelihood。
2. `L(θ)=p(D|θ)`。
3. 条件独立measurement的product likelihood。
4. MLE `argmax p(D|θ)`。
5. Log likelihood / NLL。
6. MAP `argmax p(D|θ)p(θ)`。
7. MLE vs MAP。
8. Gaussian noise → squared residual / least squares。
## 4. 深度要求
Likelihood L3；MLE/MAP L3-L4；Gaussian→LS L3。
## 5. 工程连接
SLAM、calibration、sensor fusion、neural-network loss。
## 6. 明确不展开
Bayesian model selection、conjugate prior。
## 7. 本日考核点
Probability/likelihood；MLE/MAP；NLL；Gaussian为何导出LS。
### M08毕业考试核心考点
Likelihood、MLE/MAP、Gaussian→LS。

---

# Day43 — Residual、Least Squares 与 Weighted Least Squares
## 1. 今日目标
理解机器人估计问题常见本质：寻找state，使多个measurement residual按可信度整体最小。
## 2. 前置知识
M02 Jacobian + Day42。
## 3. 必须教学内容
1. Measurement model `z=h(x)+noise`。
2. Residual `r=z-h(x)`。
3. Linear LS：`min ||Ax-b||²`。
4. Normal Equation：`A^TAx=A^Tb`，强调工程上不习惯性显式求inverse。
5. Overdetermined system与projection intuition。
6. WLS：`min r^TWr`。
7. Gaussian场景 `W=Σ^-1`。
8. Information Matrix `Ω=Σ^-1`。
9. Correlated measurement与非对角covariance。
## 4. 深度要求
Residual L4；LS L3-L4；WLS/covariance weighting L4。
## 5. 工程连接
GPS+LIO、ICP、calibration、factor graph。
## 6. 明确不展开
Sparse solver、QR/Cholesky、GN。
## 7. 本日考核点
Residual；LS/WLS；A^TA维度；covariance与weight。
### M08毕业考试核心考点
Residual、LS、WLS、Information。

---

# Day44 — Nonlinear Least Squares、Gradient 与 Newton
## 1. 今日目标
在复用 M02 / M06 已掌握微分与 Gradient Descent 的基础上，理解 nonlinear residual 为什么需要 iterative optimization，以及 Newton update 如何产生。
## 2. 前置知识
M02 Day14–15 + M06 Day29 + Day43。
## 3. 复用回顾
Gradient、First/Second-order Taylor、Hessian、Gradient Descent；已稳定掌握则只恢复公式和角色，不重新完整教学。
## 4. 新增教学内容
1. `r(x)=z-h(x)`；`min ||r(x)||²`。
2. Nonlinear Least Squares（非线性最小二乘）。
3. Iteration：initial guess→residual→derivative→Δx→update。
4. Newton：`Δx=-H^-1g`。
5. GD vs Newton 在 update information 上的区别。
6. Initial guess / local minimum / divergence。
7. Convergence indicators。
## 5. 知识连接
把以前“导数 / Taylor / Hessian 是什么”连接成真正 estimator update：
`Residual → Derivative → Local Model → Δx → State Update → Recompute`。
## 6. 深度要求
Nonlinear LS L3-L4；Newton L3；Taylor→optimization transfer L4。
## 7. 工程连接
SLAM、ICP、calibration。
## 8. 明确不展开
完整convex optimization、line search、trust region；不重新机械训练 Gradient Descent 基础。
## 9. 本日考核点
为何迭代；Newton step；initial guess；local/global；convergence；重点考旧数学如何进入 nonlinear estimator。
### M08毕业考试核心考点
Nonlinear Optimization、Newton、Initialization。

# Day45 — Gauss-Newton、LM 与 Robust Optimization
## 1. 今日目标
理解SLAM源码中的residual、Jacobian、`J^TJ`、GN、LM和degeneracy。
## 2. 前置知识
Day43–44。
## 3. 必须教学内容
1. `r(x+Δx)≈r(x)+JΔx`。
2. Local LS `min ||r+JΔx||²`。
3. GN：`J^TJΔx=-J^Tr`。
4. Approximate Hessian `H≈J^TJ`。
5. State update；pose后续需manifold update。
6. LM：`(J^TJ+λI)Δx=-J^Tr`。
7. Outlier与Robust Loss（Huber/Cauchy概念）。
8. Degeneracy / Conditioning与SVD/eigen联系。
## 4. 深度要求
GN L4；LM L3；robust L2-L3；degeneracy L3-L4。
## 5. 工程连接
ICP、LiDAR SLAM、VIO、calibration、factor graph。
## 6. 明确不展开
Ceres/G2O源码、sparse Schur complement。
## 7. 本日考核点
Residual→linearization→normal equation→update；LM；outlier；degenerate direction。
### M08毕业考试核心考点
Gauss-Newton、LM、Robust、Degeneracy。

---

# Day46 — 3D Rotation、Rotation Matrix、Euler 与 Quaternion
## 1. 今日目标
理解robot orientation不能像普通XYZ位置一样随便加减，并为Modern Robotics Ch3做准备。
## 2. 前置知识
M02 matrix/basis。
## 3. 必须教学内容
1. 3D rotation与frame orientation。
2. `R∈SO(3)`：`R^TR=I`、`det(R)=1`。
3. Columns of R与basis含义。
4. Composition `R_ac=R_abR_bc` 与右侧先作用。
5. Inverse `R^-1=R^T`。
6. Euler angles与gimbal lock。
7. Quaternion convention、normalization、composition。
8. `q`与`-q`同rotation；SLERP概念。
## 4. 深度要求
Rotation matrix/frame composition L4；Euler/quaternion L3。
## 5. 工程连接
TF、IMU orientation、LIO、manipulator pose。
## 6. 明确不展开
Quaternion代数证明、dual quaternion、SO(3) Exp/Log。
## 7. 本日考核点
Rotation约束；composition/inverse；Euler问题；quaternion convention。
### M08毕业考试核心考点
Rotation Matrix、Composition、Quaternion。

---

# Day47 — SO(3)、SE(3)、Lie Algebra、Exp/Log 与 Pose Perturbation
## 1. 今日目标
理解robot pose位于manifold，而optimization需要局部vector increment；建立进入M12 Screw/POE体系所需的刚体运动数学底座。
## 2. 前置知识
Day46 + M02 Jacobian/Taylor。
## 3. 必须教学内容
1. SO(3)与rotation manifold。
2. Rotation不是普通Euclidean vector。
3. Lie Group / Lie Algebra工程概念。
4. `so(3)`、hat operator、Exp/Log。
5. SE(3)：`T=[[R,t],[0,1]]`与6 DoF。
6. `se(3)` local pose increment `ξ∈R^6`。
7. Pose Perturbation与valid manifold update。
8. Left vs Right Perturbation：`Exp(δξ^)T` vs `TExp(δξ^)`，读源码先确认convention。
9. **M12 bridge**：同一个刚体运动/速度在不同frame下表达会不同；M12将正式学习 Screw Axis、Twist、Wrench、Adjoint、POE 和 Space/Body Jacobian。
10. Why SLAM/Manipulation use Lie geometry：optimization使用local vector，真实pose保持在SE(3)。
## 4. 深度要求
SO(3)/SE(3) L4；Exp/Log/perturbation L3-L4；M12 bridge L2。
## 5. 工程连接
FAST-LIO、ICP、pose graph、VIO、TF、manipulation pose。
## 6. 明确不展开
**本模块不要求** Adjoint计算、Screw Theory、BCH、SO(3)/SE(3) left/right Jacobian完整推导；Adjoint/Screw正式放M12。
## 7. 本日考核点
SO(3)/SE(3)；Exp/Log；6D increment；left/right perturbation；为什么不同frame下的6D运动表示需要专门变换。
### M08毕业考试核心考点
SO(3)、SE(3)、Exp/Log、Pose Perturbation。

---

# Day48 — Probability + Optimization + SE(3) 综合：SLAM数学主链
## 1. 今日目标
**Integration Day（综合连接日）**：不重新逐项教学 Day40–47，而是把已学数学串成完整 estimator / SLAM update chain。
## 2. 前置知识
Day40–47。
## 3. 复用方式
Probability、Covariance、Likelihood、Residual/WLS、Jacobian、GN/LM、SO(3)/SE(3) 均视为已学组件；若无暴露遗忘，不重新讲定义与单独基础题。
## 4. 综合主链
1. State / Measurement / Prediction / Residual。
2. Measurement covariance `Σ`。
3. Weighted residual `r^TΣ^-1r`。
4. Multi-sensor objective `Σ r_i^TΩ_ir_i`。
5. Residual Jacobian。
6. GN structure `HΔx=-g`，`H=ΣJ_i^TΩ_iJ_i`。
7. Euclidean vs SO(3)/SE(3) update。
8. Iterate：linearize→solve→update→recompute。
9. Unobservable/degenerate direction。
10. Outlier：gating/robust/rejection。
11. Estimation是推断目标，optimization是实现手段之一。
## 5. 知识连接
强制解释为什么一条真实 SLAM 数学链会同时出现：
`Sensor → Probability/Covariance → Residual → Weight → Jacobian → GN/LM → SE(3) Update → New Pose`。
## 6. 深度要求
完整数学链 L4；weighted residual/Jacobian→GN L4；SE(3) update L3-L4。
## 7. 工程连接
GPS/LIO、FAST-LIO、ICP、factor graph、manipulation transforms。
## 8. 明确不展开
KF完整推导、factor graph完整理论、IMU preintegration、Screw Theory；不把 Day40–47 重讲一遍。
## 9. 本日考核点
以综合题为主：完整 SLAM 数学链；为什么乘 `Σ^-1`；pose update；degeneracy；组件之间的因果关系。

---

# M08 Graduation Exam
统一权重：**30%核心基础 / 50%综合系统场景 / 20% Source·Formula·Design**。

## 30% 核心基础
硬门槛：Bayes、Covariance、Residual/WLS、Gauss-Newton、Rotation/Transform direction、SO(3)/SE(3)、Exp/Log、Pose Perturbation。

## 50% 综合系统场景
至少覆盖：Bayesian sensor fusion；不同covariance的WLS；nonlinear residual→GN→update；SE(3) pose composition/inverse/perturbation；说明为什么M12不能把Twist/POE当普通XYZ加法问题。

## 20% Source / Formula / Design
独立解释：`Sensor→z→h(x)→r→Σ→Weighted Cost→J→GN/LM→Δx→SE(3) Update`；并能在Modern Robotics Ch3或SLAM源码中识别rotation/transform/Exp-Log/perturbation的数学对象与frame convention。

## 通过标准
- 总分 ≥85%；
- Bayes、Covariance、Residual、WLS、GN、Pose Perturbation为硬门槛；
- 不允许 `covariance小 = 真实误差一定小`；
- 不允许只背GN公式；
- 必须理解M08只完成SE(3)底座，Screw/Twist/Adjoint/POE的正式机器人使用进入M12。

## Day40–Day48索引
```text
Day40  Probability / Conditional Probability / Bayes
Day41  Expectation / Variance / Covariance / Gaussian
Day42  Likelihood / MLE / MAP
Day43  Residual / Least Squares / Weighted LS
Day44  Nonlinear LS / Gradient / Newton
Day45  Gauss-Newton / LM / Robust Optimization
Day46  Rotation Matrix / Euler / Quaternion
Day47  SO(3) / SE(3) / Exp-Log / Perturbation / M12 Bridge
Day48  Probability + Optimization + SE(3) 综合
```
