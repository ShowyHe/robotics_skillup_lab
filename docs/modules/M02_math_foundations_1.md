# M02 — Mathematical Foundations I

## Module Goal
建立后续 Vision、EKF、SLAM、Control、Manipulation、Deep Learning 都会反复使用的数学语言。重点不是刷数学题，而是理解变量维度、几何意义、局部变化关系和机器人算法中的统一表达。

---

## Day8 — Vector、Matrix 与机器人状态表示

### 今日目标
理解为什么机器人算法大量使用向量和矩阵表示状态、观测和变换，并能判断矩阵运算是否合法。

### 必须教学内容
1. Scalar / Vector / Matrix：标量、状态向量、线性关系结构；结合 position `[x,y]`、pose `[x,y,θ]`、velocity `[vx,vy,wz]`。
2. Vector 维度：`x∈R^n`、`A∈R^(m×n)`、`Ax∈R^m`；看到公式先检查维度。
3. Row / Column Vector：理解机器人算法为何通常采用 column vector。
4. Matrix Addition / Scalar Multiplication。
5. Matrix Multiplication：不是逐元素乘；`A(m×n)B(n×p)`；输出是输入变量的加权组合。
6. Transpose。
7. Identity Matrix：`Ix=x`。
8. Matrix Inverse：`A^-1A=I`，但并非所有矩阵可逆。
9. Matrix 作为函数：`y=Ax` 表示输入状态经过线性映射得到输出。

### 深度要求
Vector / Matrix L3；matrix multiplication L3；inverse L2-L3。

### 工程连接
Robot state、TF rotation、EKF state vector、neural network layer、state-space control。

### 明确不展开
determinant 完整理论、eigenvalue、SVD、tensor 高维运算。

### 本日考核点
1. 为什么 `[x,y,θ]` 可以表示成 vector？
2. `3×4` 与 `4×2` 相乘结果维度？
3. 为什么 `3×4` 与 `3×2` 不能直接相乘？
4. `y=Ax` 的机器人含义？
5. Identity matrix 意义？
6. 什么情况下 inverse 不存在？
7. 给状态变换公式判断变量维度。

### M02毕业考试核心考点
矩阵维度、matrix multiplication、state representation、linear mapping。

---

## Day9 — Basis、Coordinate 与 Linear Transformation

### 今日目标
真正理解同一个物理向量在不同坐标系下为什么数字不同。

### 必须教学内容
1. Vector 几何意义：区分向量本身与其在 basis 下的坐标。
2. Basis：坐标系提供一组 basis；坐标是向量在 basis 上的表示。
3. Coordinate Transformation：same physical vector, different coordinate representation。
4. Linear Transformation：`T(ax+by)=aT(x)+bT(y)`；rotation/scaling/projection 可由 matrix 描述。
5. Matrix Columns：一列可理解为一个 basis vector 变换后去了哪里。
6. Rank：真正独立的信息维度。
7. Null Space：`Ax=0` 中系统“看不见”的方向。
8. Invertibility 与 Information Loss：rank loss 意味着信息丢失。

### 深度要求
Basis / coordinate L3；linear transformation L3；rank/null space L2-L3。

### 工程连接
TF、Camera frame/base_link、observability 直觉、Jacobian singularity 直觉。

### 明确不展开
vector space 公理证明、column space 严格理论、rank-nullity theorem 证明。

### 本日考核点
1. 物理向量与坐标值是否同一回事？
2. basis 是什么？
3. 为什么换坐标后数字变但物理对象不变？
4. matrix 为什么代表线性变换？
5. matrix column 的几何含义？
6. rank 低说明什么？
7. null space 的机器人含义？

### M02毕业考试核心考点
Coordinate representation、transformation、rank、information loss。

---

## Day10 — Dot、Cross、Norm、Projection 与几何关系

### 今日目标
掌握距离、方向、夹角、投影、法向量这些机器人算法高频几何工具。

### 必须教学内容
1. Vector Norm：`||x||`，重点 2-norm。
2. Distance：`||p1-p2||` 为欧氏距离。
3. Dot Product：`a^Tb=||a||||b||cosθ`；方向关系。
4. Angle：利用 dot 判断夹角。
5. Projection：向量在某方向上的分量。
6. Orthogonality：rotation matrix 与正交性的联系。
7. Cross Product：3D 中产生垂直方向。
8. Normal Vector：surface normal 与 point-to-plane ICP 的前置直觉。
9. Cost 中的 distance、heading、projection error。

### 深度要求
Norm/dot/projection L3；cross L3；几何解释 L3。

### 工程连接
Path distance、heading error、obstacle clearance、PointCloud normal、ICP、camera ray。

### 明确不展开
general p-norm、functional analysis、complex vector space。

### 本日考核点
1. `||p1-p2||` 为何是距离？
2. dot=0 说明什么？
3. 如何求夹角？
4. projection 物理意义？
5. cross 方向如何判断？
6. surface normal 有什么作用？
7. clearance cost 与 norm 有什么关系？

### M02毕业考试核心考点
Distance、angle、projection、normal/vector geometry。

---

## Day11 — Eigenvalue、Eigenvector 与 Quadratic Form

### 今日目标
建立状态空间稳定性、协方差、PCA、LQR 都会用到的“特征方向”直觉。

### 必须教学内容
1. Eigen Equation：`Av=λv`；特殊方向只缩放不改变方向。
2. Eigenvector：系统特殊方向。
3. Eigenvalue：该方向被放大/缩小的程度。
4. 2×2 简单计算：`det(A-λI)=0`。
5. Dynamics 直觉：`x_(k+1)=Ax_k`，`|λ|>1` 放大，`|λ|<1` 衰减。
6. Symmetric Matrix：协方差、Hessian、LQR 矩阵为何常对称。
7. Quadratic Form：`x^TQx` 为各方向误差加权平方代价。
8. Positive Definite：`x^TQx>0` 为何适合作为 cost。

### 深度要求
Eigenvalue 几何意义 L3；简单计算 L3；quadratic form L3；positive definite L2-L3。

### 工程连接
System stability、covariance、LQR cost、optimization Hessian、PCA 概念。

### 明确不展开
Jordan form、spectral theorem 严格证明、complex eigenvalue 深入。

### 本日考核点
1. `Av=λv` 表达什么？
2. eigenvector 为什么特殊？
3. `|λ|>1` 在离散系统意味着什么？
4. `x^TQx` 为何适合作为 cost？
5. Q 不同方向权重意味着什么？
6. positive definite 为何重要？
7. 手算简单 2×2 eigenvalue。

### M02毕业考试核心考点
Eigen 方向、stability 初步直觉、quadratic cost。

---

## Day12 — SVD、Conditioning 与矩阵中的信息强弱

### 今日目标
理解 SVD 为何在视觉、点云、最小二乘、数据降维中常见。**本日重点是几何直觉与信息强弱，不把时间耗在复杂 SVD 手算。**

### 必须教学内容
1. SVD：`A=UΣV^T`。
2. 几何解释：rotate → scale → rotate。
3. Singular Value：不同输入方向经 A 后被放大/缩小多少。
4. Rank 与 Singular Value：零或近零意味着方向信息缺失、rank 降低、求解不稳定。
5. Conditioning：输入误差是否被巨大放大。
6. Ill-conditioned Problem：sensor noise 如何被数学求解放大。
7. Pseudoinverse：不可逆时寻找 least-squares 意义的解。
8. 场景：point fitting、ICP、least squares、PCA、calibration。

### 深度要求
- SVD 几何意义：L3。
- singular value / rank：L3。
- conditioning / degeneracy：L3。
- pseudoinverse：L2。
- **只允许极简单矩阵用于辅助理解，不要求复杂 SVD 分解手算或数值算法。**

### 工程连接
SLAM 退化、point cloud fitting、calibration、least squares。

### 明确不展开
复杂 SVD 手算、完整 SVD 数值算法、Golub-Kahan、numerical linear algebra 深入。

### 本日考核点
1. SVD 三个矩阵的几何作用？
2. singular value 接近 0 意味着什么？
3. rank 与 SVD 关系？
4. ill-conditioned 含义？
5. sensor noise 为何可能被求解放大？
6. pseudoinverse 解决什么？
7. SLAM 退化为何与方向信息缺失有关？

### M02毕业考试核心考点
Rank、SVD、conditioning、information degeneracy；不考复杂 SVD 手算。

---

## Day13 — Derivative、Differential、Discrete Integration 与机器人状态变化

### 今日目标
把导数理解为“变量微小变化时输出如何响应”，并建立机器人离散时间积分的正确直觉。

### 必须教学内容
1. Function：`y=f(x)`。
2. Derivative：`dy/dx` 为局部变化率。
3. Limit 直觉：局部行为，不做严格 ε-δ 证明。
4. Geometry：导数 = 切线斜率。
5. Physics：position → derivative → velocity → derivative → acceleration。
6. Differential：`dy≈f'(x)dx`。
7. Numerical Derivative：`[f(x+h)-f(x)]/h`；h 太大带来近似误差，h 太小会受数值误差与噪声影响。
8. Integration：导数描述瞬时变化，积分描述累计变化。
9. **Sampling 与 `dt`**：机器人算法只在离散时刻拿到 measurement / state，因此连续积分通常需要数值近似。
10. **Forward Euler Integration**：`x_(k+1)=x_k+v_kΔt`，理解它是一个离散近似而不是精确物理定律。
11. **Accumulated Error**：每一步的小误差会在连续积分中累计；IMU/Wheel Odom 尤其敏感。
12. **Frequency ≠ Accuracy**：更高频率可以减小部分离散化误差、提高时间分辨率，但如果 measurement 有 bias/noise/model error，高频本身并不会自动让积分更准确。
13. **Variable `dt`**：真实系统中 callback / sensor 时间间隔可能波动；不能总是假定固定周期。

### 深度要求
Derivative / differential L3；numerical derivative L2-L3；Euler integration / `dt` / accumulated error L3。

### 工程连接
Odometry integration、IMU integration、controller dt、MPPI rollout、finite difference、ROS timestamp。

### 明确不展开
极限严格证明、积分技巧大全、高阶数值积分方法、微分方程完整课程。

### 本日考核点
1. 导数物理意义？
2. velocity 为何是 position 导数？
3. differential 与 derivative 关系？
4. 为什么 `v*dt` 能近似位移？
5. Forward Euler 为什么只是近似？
6. `dt` 太大可能带来什么问题？
7. variable `dt` 为什么不能简单用固定周期替代？
8. numerical derivative 为何怕 noise？
9. 高频 IMU 是否天然意味着积分后位置更准？为什么？
10. 根据离散 velocity 与各自 timestamp 计算 position 变化。

### M02毕业考试核心考点
Derivative、differential、sampling / `dt`、Euler integration、accumulated error、discrete dynamics 直觉。

---

## Day14 — Partial Derivative、Gradient 与 Chain Rule

### 今日目标
进入机器人和 AI 真正使用的多变量函数。

### 必须教学内容
1. Multivariable Function，例如 `J(x,y)`。
2. Partial Derivative：固定其他变量看单一变量影响。
3. Gradient：`∇J` 收集所有方向的一阶变化信息。
4. Gradient Direction：指向局部增长最快方向，`-∇J` 用于下降。
5. Gradient Descent：`x←x-α∇J` 的几何意义。
6. Chain Rule：从 `y=f(g(x))` 到多级计算链，理解下游变化如何传回上游。
7. Computational Graph：`x→z→y→loss` 的 gradient 传播。
8. Robot Cost：trajectory → position error → cost 的依赖关系。

### 深度要求
Partial derivative L3；gradient L3；chain rule L3-L4；gradient descent L3。

### 工程连接
Optimization、backprop、trajectory cost、MPC、SLAM residual。

### 明确不展开
Backprop 完整网络、constrained optimization、Hessian 优化算法。

### 本日考核点
1. partial derivative 与普通 derivative 区别？
2. gradient 每个元素是什么？
3. gradient 为何是最快上升方向？
4. gradient descent 为何取负 gradient？
5. learning rate 作用？
6. chain rule 解决什么？
7. 手算两层函数 chain rule。
8. 根据简单 computational graph 追 gradient。

### M02毕业考试核心考点
Gradient、chain rule、multivariable dependency、optimization intuition。

---

## Day15 — Jacobian、Hessian、Taylor 与局部线性化

### 今日目标
理解为什么机器人非线性问题常在当前状态附近用线性模型近似。

### 必须教学内容
1. Vector-valued Function：`f:R^n→R^m`。
2. Jacobian 定义：`J_ij=∂f_i/∂x_j`；row 对应 output，column 对应 input，维度 `m×n`。
3. Jacobian 核心意义：`Δy≈JΔx`，输入小变化如何映射到输出小变化。
4. 至少手算一个 `R²→R²` 或 `R²→R³` Jacobian。
5. Single-variable Taylor：`f(x+Δx)≈f(x)+f'(x)Δx`。
6. Multivariable Taylor：`f(x+Δx)≈f(x)+JΔx`。
7. Linearization：只是当前工作点附近的局部近似。
8. Operating Point：状态变化后 Jacobian 可能变化。
9. Hessian：gradient 的一阶变化、二阶曲率；scalar output 下 `n×n`。
10. EKF 连接：nonlinear model → Jacobian → local linear approximation。
11. Manipulation 连接：joint `Δq` → Jacobian → end-effector `Δx`。
12. SLAM 连接：state change → residual change；Jacobian 表示 residual 对 state 的敏感度。
13. Control 连接：非线性 dynamics 为什么经常需要 linearization。

### 深度要求
Jacobian definition L3；Jacobian calculation L3；`Δy≈JΔx` 与 linearization L4 级理解；Taylor L3；Hessian L2-L3。

### 工程连接
必须建立：Jacobian → EKF、SLAM、Manipulation、Control 四条未来主线，并理解本质都是局部变化关系。

### 明确不展开
Lie Jacobian、EKF covariance 完整推导、Manipulator Jacobian 完整推导、Gauss-Newton、Newton optimization。

### 本日考核点
1. 什么类型函数需要 Jacobian？
2. Jacobian 行和列各代表什么？
3. `R³→R²` 的 Jacobian 维度？
4. 独立手算一个 Jacobian。
5. `Δy≈JΔx` 是什么意思？
6. Taylor 为什么能做局部近似？
7. linearization 是否把系统永久变成线性？
8. operating point 改变后 Jacobian 会不会改变？
9. Hessian 代表什么？
10. EKF 为什么需要 Jacobian？
11. IK 为什么需要 Jacobian？
12. SLAM optimization 为什么需要 Jacobian？

### M02毕业考试核心考点
Jacobian 计算、dimensional reasoning、Taylor、local linearization、跨 EKF/SLAM/IK/Control 统一理解。

---

# M02 Graduation Exam Specification

## A. 基础专项 — 30%
必须覆盖：matrix dimensions、matrix multiplication、coordinate/basis、rank、dot/projection、eigenvalue、quadratic form、SVD/conditioning、derivative、sampling/`dt`、Euler integration、gradient、chain rule、Jacobian、Taylor。

核心计算必须本人完成；SVD 不考复杂手算。

## B. 数学 + 机器人综合题 — 50%
设计 3–4 组场景：

### 场景1：坐标与矩阵
给 robot frame 中的 point 与 rotation/translation 关系，要求判断 vector/matrix 维度、乘法顺序和物理意义。

### 场景2：系统动态与离散积分
给 `x_(k+1)=Ax_k` 或离散 velocity / timestamp 序列，要求分析 eigenvalue、误差增长/衰减，并判断 `dt`、sampling、integration error 对状态估计的影响。

### 场景3：Optimization
给简单 `J(x,y)`，要求 partial derivative、gradient、gradient descent 方向、chain rule。

### 场景4：非线性机器人模型
给 `y=f(x)`，要求求 Jacobian、写局部 Taylor、给定 `Δx` 估算 `Δy`，解释这和 EKF/IK/SLAM 的关系。

## C. 推理 / 公式理解 — 20%
看到 `x^TQx`、`A=UΣV^T`、`Δy≈JΔx`、`x←x-α∇J`、`x_(k+1)=x_k+v_kΔt`，能够逐项解释变量、维度、数学作用和机器人意义。

## Knowledge Coverage Matrix
- Vector / Matrix：必考
- Dimension reasoning：必考
- Linear transformation：必考
- Basis / coordinate：必考
- Rank / information loss：必考
- Norm / Dot / Projection：必考
- Eigenvalue / Eigenvector：必考
- Quadratic Form：必考
- SVD / Conditioning：必考（不考复杂手算）
- Derivative / Differential：必考
- Sampling / `dt` / Euler Integration：**核心必考**
- Accumulated integration error：必考
- Partial derivative：必考
- Gradient：必考
- Chain Rule：**核心必考**
- Jacobian：**核心必考**
- Taylor / Linearization：**核心必考**
- Hessian：理解题

## 通过标准
- 总分 ≥85%；
- Matrix dimension、Sampling/`dt`、Chain Rule、Jacobian、Taylor/Linearization 属于硬门槛；
- 核心知识出现根本性错误，即使总分够，也进行定向补课和复测；
- 不要求达到数学专业证明深度；
- 不接受“会套公式，但不知道变量、维度和机器人意义”。
