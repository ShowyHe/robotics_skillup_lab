# M02 — Mathematical Foundations I

## Module Goal
建立后续 Vision、EKF、SLAM、Control、Manipulation、Deep Learning 都会反复使用的数学语言。重点不是刷数学题，而是理解变量维度、几何意义、局部变化关系和机器人算法中的统一表达。

---

## Day8 — Vector、Matrix 与机器人状态表示

### 今日目标
理解为什么机器人算法大量使用向量和矩阵表示状态、观测和变换，并能判断矩阵运算是否合法。

### 必须教学内容
1. Scalar / Vector / Matrix：标量、状态向量、线性关系结构；结合position `[x,y]`、pose `[x,y,θ]`、velocity `[vx,vy,wz]`。
2. Vector维度：`x∈R^n`、`A∈R^(m×n)`、`Ax∈R^m`；看到公式先检查维度。
3. Row / Column Vector：理解机器人算法为何通常采用column vector。
4. Matrix Addition / Scalar Multiplication。
5. Matrix Multiplication：不是逐元素乘；`A(m×n)B(n×p)`；输出是输入变量的加权组合。
6. Transpose。
7. Identity Matrix：`Ix=x`。
8. Matrix Inverse：`A^-1A=I`，但并非所有矩阵可逆。
9. Matrix作为函数：`y=Ax` 表示输入状态经过线性映射得到输出。

### 深度要求
Vector/Matrix L3；matrix multiplication L3；inverse L2-L3。

### 工程连接
Robot state、TF rotation、EKF state vector、neural network layer、state-space control。

### 明确不展开
determinant完整理论、eigenvalue、SVD、tensor高维运算。

### 本日考核点
1. 为什么 `[x,y,θ]` 可以表示成vector？
2. `3×4` 与 `4×2` 相乘结果维度？
3. 为什么 `3×4` 与 `3×2` 不能直接相乘？
4. `y=Ax` 的机器人含义？
5. Identity matrix意义？
6. 什么情况下inverse不存在？
7. 给状态变换公式判断变量维度。

### M02毕业考试核心考点
矩阵维度、matrix multiplication、state representation、linear mapping。

---

## Day9 — Basis、Coordinate 与 Linear Transformation

### 今日目标
真正理解同一个物理向量在不同坐标系下为什么数字不同。

### 必须教学内容
1. Vector几何意义：区分向量本身与其在basis下的坐标。
2. Basis：坐标系提供一组basis；坐标是向量在basis上的表示。
3. Coordinate Transformation：same physical vector, different coordinate representation。
4. Linear Transformation：`T(ax+by)=aT(x)+bT(y)`；rotation/scaling/projection可由matrix描述。
5. Matrix Columns：一列可理解为一个basis vector变换后去了哪里。
6. Rank：真正独立的信息维度。
7. Null Space：`Ax=0` 中系统“看不见”的方向。
8. Invertibility 与 Information Loss：rank loss意味着信息丢失。

### 深度要求
Basis/coordinate L3；linear transformation L3；rank/null space L2-L3。

### 工程连接
TF、Camera frame/base_link、observability直觉、Jacobian singularity直觉。

### 明确不展开
vector space公理证明、column space严格理论、rank-nullity theorem证明。

### 本日考核点
1. 物理向量与坐标值是否同一回事？
2. basis是什么？
3. 为什么换坐标后数字变但物理对象不变？
4. matrix为什么代表线性变换？
5. matrix column的几何含义？
6. rank低说明什么？
7. null space的机器人含义？

### M02毕业考试核心考点
Coordinate representation、transformation、rank、information loss。

---

## Day10 — Dot、Cross、Norm、Projection 与几何关系

### 今日目标
掌握距离、方向、夹角、投影、法向量这些机器人算法高频几何工具。

### 必须教学内容
1. Vector Norm：`||x||`，重点2-norm。
2. Distance：`||p1-p2||` 为欧氏距离。
3. Dot Product：`a^Tb=||a||||b||cosθ`；方向关系。
4. Angle：利用dot判断夹角。
5. Projection：向量在某方向上的分量。
6. Orthogonality：rotation matrix与正交性的联系。
7. Cross Product：3D中产生垂直方向。
8. Normal Vector：surface normal与point-to-plane ICP的前置直觉。
9. Cost中的距离、heading、projection error。

### 深度要求
Norm/dot/projection L3；cross L3；几何解释L3。

### 工程连接
Path distance、heading error、obstacle clearance、PointCloud normal、ICP、camera ray。

### 明确不展开
general p-norm、functional analysis、complex vector space。

### 本日考核点
1. `||p1-p2||`为何是距离？
2. dot=0说明什么？
3. 如何求夹角？
4. projection物理意义？
5. cross方向如何判断？
6. surface normal有什么作用？
7. clearance cost与norm有什么关系？

### M02毕业考试核心考点
Distance、angle、projection、normal/vector geometry。

---

## Day11 — Eigenvalue、Eigenvector 与 Quadratic Form

### 今日目标
建立状态空间稳定性、协方差、PCA、LQR都会用到的“特征方向”直觉。

### 必须教学内容
1. Eigen Equation：`Av=λv`；特殊方向只缩放不改变方向。
2. Eigenvector：系统特殊方向。
3. Eigenvalue：该方向被放大/缩小的程度。
4. 2×2简单计算：`det(A-λI)=0`。
5. Dynamics直觉：`x_(k+1)=Ax_k`，`|λ|>1`放大，`|λ|<1`衰减。
6. Symmetric Matrix：协方差、Hessian、LQR矩阵为何常对称。
7. Quadratic Form：`x^TQx` 为各方向误差加权平方代价。
8. Positive Definite：`x^TQx>0` 为何适合作为cost。

### 深度要求
Eigenvalue几何意义L3；简单计算L3；quadratic form L3；positive definite L2-L3。

### 工程连接
System stability、covariance、LQR cost、optimization Hessian、PCA概念。

### 明确不展开
Jordan form、spectral theorem严格证明、complex eigenvalue深入。

### 本日考核点
1. `Av=λv`表达什么？
2. eigenvector为什么特殊？
3. `|λ|>1`在离散系统意味着什么？
4. `x^TQx`为何适合作为cost？
5. Q不同方向权重意味着什么？
6. positive definite为何重要？
7. 手算简单2×2 eigenvalue。

### M02毕业考试核心考点
Eigen方向、stability初步直觉、quadratic cost。

---

## Day12 — SVD、Conditioning 与矩阵中的信息强弱

### 今日目标
理解SVD为何在视觉、点云、最小二乘、数据降维中常见。

### 必须教学内容
1. SVD：`A=UΣV^T`。
2. 几何解释：rotate → scale → rotate。
3. Singular Value：不同输入方向经A后被放大/缩小多少。
4. Rank与Singular Value：零或近零意味着方向信息缺失、rank降低、求解不稳定。
5. Conditioning：输入误差是否被巨大放大。
6. Ill-conditioned Problem：sensor noise如何被数学求解放大。
7. Pseudoinverse：不可逆时寻找least-squares意义的解。
8. 场景：point fitting、ICP、least squares、PCA、calibration。

### 深度要求
SVD几何意义L3；singular value/rank L3；pseudoinverse L2；conditioning L3。

### 工程连接
SLAM退化、point cloud fitting、calibration、least squares。

### 明确不展开
完整SVD数值算法、Golub-Kahan、numerical linear algebra深入。

### 本日考核点
1. SVD三个矩阵的几何作用？
2. singular value接近0意味着什么？
3. rank与SVD关系？
4. ill-conditioned含义？
5. sensor noise为何可能被求解放大？
6. pseudoinverse解决什么？
7. SLAM退化为何与方向信息缺失有关？

### M02毕业考试核心考点
Rank、SVD、conditioning、information degeneracy。

---

## Day13 — Derivative、Differential 与机器人状态变化

### 今日目标
把导数理解为“变量微小变化时输出如何响应”，而不是考试公式。

### 必须教学内容
1. Function：`y=f(x)`。
2. Derivative：`dy/dx` 为局部变化率。
3. Limit直觉：局部行为，不做严格ε-δ证明。
4. Geometry：导数=切线斜率。
5. Physics：position → derivative → velocity → derivative → acceleration。
6. Differential：`dy≈f'(x)dx`。
7. Numerical Derivative：`[f(x+h)-f(x)]/h`，h太大/太小的问题。
8. Integration：导数描述瞬时变化，积分描述累计变化。
9. Discrete Robot System：`x_(k+1)≈x_k+v_kΔt`。

### 深度要求
Derivative/differential L3；numerical derivative L2-L3；integration直觉L3。

### 工程连接
Odometry integration、IMU integration、controller dt、MPPI rollout、finite difference。

### 明确不展开
极限严格证明、积分技巧大全、微分方程完整课程。

### 本日考核点
1. 导数物理意义？
2. velocity为何是position导数？
3. differential与derivative关系？
4. 为什么`v*dt`能近似位移？
5. dt为何影响数值积分？
6. numerical derivative为何怕noise？
7. 根据离散velocity计算position变化。

### M02毕业考试核心考点
Derivative、differential、numerical integration、discrete dynamics直觉。

---

## Day14 — Partial Derivative、Gradient 与 Chain Rule

### 今日目标
进入机器人和AI真正使用的多变量函数。

### 必须教学内容
1. Multivariable Function，例如`J(x,y)`。
2. Partial Derivative：固定其他变量看单一变量影响。
3. Gradient：`∇J` 收集所有方向的一阶变化信息。
4. Gradient Direction：指向局部增长最快方向，`-∇J`用于下降。
5. Gradient Descent：`x←x-α∇J` 的几何意义。
6. Chain Rule：从`y=f(g(x))`到多级计算链，理解下游变化如何传回上游。
7. Computational Graph：`x→z→y→loss` 的gradient传播。
8. Robot Cost：trajectory → position error → cost的依赖关系。

### 深度要求
Partial derivative L3；gradient L3；chain rule L3-L4；gradient descent L3。

### 工程连接
Optimization、backprop、trajectory cost、MPC、SLAM residual。

### 明确不展开
Backprop完整网络、constrained optimization、Hessian优化算法。

### 本日考核点
1. partial derivative与普通derivative区别？
2. gradient每个元素是什么？
3. gradient为何是最快上升方向？
4. gradient descent为何取负gradient？
5. learning rate作用？
6. chain rule解决什么？
7. 手算两层函数chain rule。
8. 根据简单computational graph追gradient。

### M02毕业考试核心考点
Gradient、chain rule、multivariable dependency、optimization intuition。

---

## Day15 — Jacobian、Hessian、Taylor 与局部线性化

### 今日目标
理解为什么机器人非线性问题常在当前状态附近用线性模型近似。

### 必须教学内容
1. Vector-valued Function：`f:R^n→R^m`。
2. Jacobian定义：`J_ij=∂f_i/∂x_j`；row对应output，column对应input，维度`m×n`。
3. Jacobian核心意义：`Δy≈JΔx`，输入小变化如何映射到输出小变化。
4. 至少手算一个`R²→R²`或`R²→R³` Jacobian。
5. Single-variable Taylor：`f(x+Δx)≈f(x)+f'(x)Δx`。
6. Multivariable Taylor：`f(x+Δx)≈f(x)+JΔx`。
7. Linearization：只是当前工作点附近的局部近似。
8. Operating Point：状态变化后Jacobian可能变化。
9. Hessian：gradient的一阶变化、二阶曲率；scalar output下`n×n`。
10. EKF连接：nonlinear model → Jacobian → local linear approximation。
11. Manipulation连接：joint Δq → Jacobian → end-effector Δx。
12. SLAM连接：state change → residual change。
13. Control连接：非线性dynamics局部线性化。

### 深度要求
Jacobian definition L3；计算L3；`Δy≈JΔx` L4级理解；Taylor L3；Hessian L2-L3；linearization L3-L4。

### 工程连接
Jacobian → EKF / SLAM / Manipulation / Control；统一理解为局部变化关系。

### 明确不展开
Lie Jacobian、EKF covariance完整推导、Manipulator Jacobian完整推导、Gauss-Newton、Newton optimization。

### 本日考核点
1. 什么函数需要Jacobian？
2. Jacobian行列分别代表什么？
3. `R³→R²` Jacobian维度？
4. 手算一个Jacobian。
5. `Δy≈JΔx`含义？
6. Taylor为何可局部近似？
7. linearization是否永久把系统变线性？
8. operating point改变后Jacobian会否变？
9. Hessian代表什么？
10. EKF为何需要Jacobian？
11. IK为何需要Jacobian？
12. SLAM optimization为何需要Jacobian？

---

# M02 Graduation Exam Specification

M02不是考背公式，而是检查是否能用数学语言描述机器人问题。

## A. 基础专项 — 30%
必须覆盖matrix dimensions、matrix multiplication、coordinate/basis、rank、dot/projection、eigenvalue、quadratic form、SVD、derivative、gradient、chain rule、Jacobian、Taylor。核心计算必须本人完成。

## B. 数学 + 机器人综合题 — 50%
设计3–4组场景题：
1. 坐标与矩阵：给robot frame point与变换关系，判断维度、乘法顺序、物理意义。
2. 系统动态：给`x_(k+1)=Ax_k`，分析eigenvalue与误差增长/衰减。
3. Optimization：给简单`J(x,y)`，求partial derivative、gradient、下降方向、chain rule。
4. 非线性机器人模型：给`y=f(x)`，求Jacobian、写局部Taylor、给Δx估算Δy并联系EKF/IK/SLAM。

## C. 推理 / 公式理解 — 20%
看到`x^TQx`、`A=UΣV^T`、`Δy≈JΔx`、`x←x-α∇J`，能逐项解释变量、维度、数学作用和机器人意义。

## Knowledge Coverage Matrix
- Vector / Matrix：必考
- Dimension reasoning：必考
- Linear transformation：必考
- Basis / coordinate：必考
- Rank / information loss：必考
- Norm / Dot / Projection：必考
- Eigenvalue / Eigenvector：必考
- Quadratic Form：必考
- SVD / Conditioning：必考
- Derivative / Differential：必考
- Numerical integration直觉：必考
- Partial derivative：必考
- Gradient：必考
- Chain Rule：核心必考
- Jacobian：核心必考
- Taylor / Linearization：核心必考
- Hessian：理解题

## 通过标准
- 总分 ≥85%；
- Matrix dimension、Chain Rule、Jacobian、Taylor/Linearization为硬门槛；
- 核心知识出现根本错误即定向补课和复测；
- 不要求数学专业证明能力，但不能只会套公式而不知道变量、维度和机器人意义。