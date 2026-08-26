# M22 — Foundation Cleanup

## Module Goal
M22不是固定新知识模块，而是根据M00–M21 Quiz/Graduation Exam/Source Reading暴露的真实Foundation Debt，定点补齐会阻断后续Owner能力的基础缺口。

流程：`Exam/Quiz → Knowledge Debt Matrix → classify debt → minimal prerequisite path → targeted teaching/calculation → robot transfer → retest → CLOSED`。

初始预留 Day130–Day135 共6个动态Day，但允许少于6天、扩展或跨学习时段；**禁止为了凑Day制造内容。**

---

# 1. Foundation Debt Types

## Type 1 — Definition Debt
概念边界混淆，如 State vs Observation、Covariance vs Error、Path vs Trajectory、Fault vs Failure。

## Type 2 — Calculation Debt
概念懂但计算/方向/维度反复错，如transform chain、Jacobian、Bayes、小矩阵。

## Type 3 — Derivation Debt
只背公式不知道为何成立，如 `P^-=FPF^T+GQG^T`、`x_dot=Jq_dot`。

## Type 4 — Transfer Debt
理论会但迁移到机器人语义错误，如把ROS covariance开方直接当当前真实定位误差。

## Type 5 — Source-reading Debt
理论懂，但换成代码变量/function就识别不出 prediction/residual/Jacobian/update。

## Type 6 — System Reasoning Debt
单模块会，但跨模块错误归因，如看到机器人离行人近就直接判“Planner问题”。

---

# 2. Priority
- **P0 Hard Gate**：不关闭不能毕业，如TF direction、Jacobian、covariance语义、KF predict/update、feedback、timestamp freshness、VLA action semantics。
- **P1 Core Foundation**：持续影响多个模块，如Chain Rule、Taylor、LS/GN、SE(3)、Eigenvalue、State-space。
- **P2 Module-specific Weakness**：需要时补，如RRT* rewiring、CLIP matrix、force closure。
- **P3 Detail**：API/算法变体等，不占整天。

---

# 3. Knowledge Debt Matrix
必须维护：

| Knowledge | Exposed At | Debt Type | Priority | Target Level | Status |
|---|---|---|---|---|---|
| example | Mxx/Dayxx | Calculation | P0 | L3/L4 | OPEN |

状态统一：`OPEN → LEARNING → RETEST → CLOSED`。

---

# 4. Dynamic Day Generation Rule
每天只选 **1个主要foundation chain + 最多1个次要debt**。

每个动态Day必须使用：
1. 今日修复目标：写成可验证能力；
2. Debt来源：Module/Day/Exam/Source；
3. 当前错误模型；
4. 最小前置知识；
5. 必须教学内容；
6. 必须推导/计算；
7. Robot迁移题；
8. Retest：换数值/场景/frame；
9. Closure Criteria：Definition + Calculation + Transfer + Boundary。

---

# 5. Candidate Foundation Pools
以下仅是候选池，不提前绑定Day130–135。

## A. Mathematics / Numerical Optimization
Vector/Matrix/Basis/Transform/Rank/Eigen/SVD/Condition/Quadratic Form；Derivative/Partial/Gradient/Chain Rule/Jacobian/Hessian/Taylor/Linearization/Numerical Integration；Residual/LS/WLS/Newton/GN/LM；**Convexity**；**Constrained Optimization**；**Lagrange multiplier / KKT intuition**；conditioning、scaling、numerical stability。

## B. Probability / Statistics
Random Variable/Conditional Probability/Bayes/Expectation/Variance/Covariance/Gaussian/Likelihood/MLE/MAP/Information/Mahalanobis；sampling uncertainty、confidence interval和tail risk按真实Debt补。

硬边界：`σ=sqrt(variance)` 不等于“当前真实误差”；covariance是uncertainty model，需要consistency/assumption验证。

## C. Robot Geometry / Estimation
Rotation/Quaternion/SO(3)/SE(3)/Exp-Log/Perturbation/Transform Direction；Filter prediction→innovation→gain→correction；Optimization state→residual→Jacobian→H/g→Δx→update。

## D. Dynamics / Control
ODE/State-space/Equilibrium/Eigenvalue/Feedback/Controllability/Observability/Quadratic Cost/LQR/MPC/MPPI。

## E. Deep Learning / Robot Learning
Tensor Shape/Computational Graph/Chain Rule/Backprop/Cross Entropy/Attention QKV/Autoregressive/Bellman/Actor-Critic/BC/Distribution Shift；ACT latent/chunk与Diffusion公式只在实际薄弱时补。

## F. Robot System Semantics
Frame/Timestamp/Freshness/Command/Feedback/State/Observation/Goal/Path/Trajectory/Action/Physical Motion。

硬边界：`Planner Path ≠ Predicted Trajectory ≠ cmd_vel ≠ Chassis Feedback ≠ Physical Motion`。

## G. C++ / ROS2 / Build / Runtime
Lifetime/Ownership/Pointer/Reference/Lambda/Thread/Mutex/Executor/Callback Group/Future/QoS/TF/Linux Scheduling/Latency；**CMake / package dependency / target-link concepts / colcon build-workspace-overlay basics**；只补真正阻塞源码构建与阅读的部分。

## H. Algorithms / CS Foundations
Complexity `O(.)`；BFS/DFS；priority queue；recursion/iteration；dynamic programming basics；graph relaxation；heuristic guarantees；sampling planner probabilistic completeness/asymptotic optimality；algorithm invariants与correctness reasoning。只在M11/源码阅读暴露Debt时进入。

## I. Python / NumPy for Robotics & AI
Python object/reference基础、list/dict、function/class、NumPy array/shape/broadcast/indexing/matrix operation、PyTorch↔NumPy data boundary。默认入口诊断，不单独占固定Day；仅在AI/数据处理实践真正受阻时补。

---

# 6. M22 Training Methods
1. **Blank-paper Derivation**：无资料写核心公式和变量dimension/meaning。
2. **Dimension Check**：用shape发现错误。
3. **Physical Meaning**：每个公式回答机器人世界里代表什么。
4. **Source Mapping**：理论符号映射到变量/function/data flow。
5. **Counterexample**：主动构造“covariance小但estimate错”等边界反例。
6. **Cross-module Transfer**：同一基础迁移到LIO/VLA/Manipulation/Control等不同模块。

---

# 7. Prohibited Patterns
- 禁止因一个Module考差就整模块重学，除非大面积基础崩溃；
- 禁止只看讲义，必须推导/计算/解释/迁移；
- 禁止原题重考造成背答案；
- 禁止用工程经验绕过数学；
- 禁止把课程范围外高级细节强行列为P0；
- 禁止为了补“能力表”而把未暴露问题机械塞进M22。

---

# 8. Daily Quiz / Retest
M22每日Quiz建议：Foundation 2–4题 + Transfer 2–3题 + Adversarial 1–2题。

Debt CLOSED必须同时满足：
1. Definition准确；
2. Calculation/Derivation无提示完成；
3. Transfer到新Robot模块；
4. Boundary：知道该概念不能证明什么。

---

# 9. M22 Graduation Exam — Foundation Debt Defense
权重继续遵循统一结构：
- **30% Debt Closure Audit**：原错误→正确理解→为什么→如何验证；
- **50% Cross-module Transfer**：3–5个未知场景跨Geometry/Time/Control/Data/Safety建立证据链；
- **20% Blank-paper Fundamentals**：只抽实际暴露过Debt的核心公式。

## 通过标准
- 所有P0 debt必须CLOSED；
- P1不得残留会阻断Owner能力的基础错误；
- 至少3次 `Math/Theory → New Robot Scenario` 迁移；
- 不得再出现“公式记得但变量/物理意义不知道”；
- 不得再出现“会调参数但解释不了为什么”；
- 核心链达到 `Definition → Math → Physical Meaning → Source → Robot Behavior`。

---

# Day130–Day135
只锁定动态槽位，不锁定学科：
```text
Day130 Dynamic Foundation Debt #1
Day131 Dynamic Foundation Debt #2
Day132 Dynamic Foundation Debt #3
Day133 Dynamic Foundation Debt #4
Day134 Dynamic Foundation Debt #5
Day135 Dynamic Foundation Debt #6
```

实际内容必须由 `PROGRESS.md` 中累计Foundation Debt和前面Module Graduation Exam结果动态生成。
