# M12 — Robot Kinematics / Dynamics / System Dynamics

## Module Goal
建立 `Rigid Body Pose → FK → Jacobian → IK → Singularity → Mobile Kinematics → Force/Torque/Inertia → Manipulator Dynamics → ODE/State-space → Linearization/Discretization → Action Interface` 完整主线。

本模块共 9 个理论 Day（Day71–Day79）。

---

# Day71 — Rigid Body / Joint / DOF / Kinematic Chain
1. 今日目标：建立机械臂和移动底盘的configuration语言。
2. 前置：M02矩阵/坐标变换，M08 SE(3)基础。
3. 必须教学：rigid body；link；revolute/prismatic/fixed joint；DOF；`q=[q1...qn]`；configuration space；base/link/EEF frame；kinematic chain/tree；joint limit；`T_0n=T_01...T_(n-1)n`；pose vs configuration；URDF tree语义。
4. 深度：DOF/configuration/transform chain L4。
5. 工程连接：MoveIt robot model、joint_states、EEF。
6. 不展开：full multibody dynamics。
7. 考核：给URDF式链判断DOF、frame、configuration。
8. 毕业考点：Pose、Configuration、DOF、transform chain。

# Day72 — Forward Kinematics / DH / POE
1. 今日目标：从joint configuration算EEF pose。
2. 前置：Day71。
3. 必须教学：`T_base^ee=FK(q)`；完整2-link planar FK：`x=l1 cosθ1+l2 cos(θ1+θ2)`、`y=l1 sinθ1+l2 sin(θ1+θ2)`；homogeneous transforms；DH参数 `a,α,d,θ`；standard vs modified convention；POE `T=e^(ξ1^ q1)...e^(ξn^ qn)M` 概念；DH vs POE边界。
4. 深度：2-link FK L4；DH L3；POE L2-L3。
5. 工程连接：EEF pose、MoveIt FK。
6. 不展开：完整screw theory证明、复杂6轴手算。
7. 考核：手算2-link FK并解释transform order。
8. 毕业考点：FK、DH、transform order。

# Day73 — Manipulator Jacobian / Velocity Kinematics
1. 今日目标：理解joint velocity如何映射到EEF twist。
2. 前置：M02 Jacobian + Day72。
3. 必须教学：`x_dot=J(q) q_dot`；Jacobian row/column意义；2-link Jacobian推导；linear/angular velocity；twist；frame约定；inverse velocity；pseudoinverse；`τ=J^T F`的virtual-work直觉，并明确wrench/twist必须在兼容frame表达。
4. 深度：Jacobian L4。
5. 工程连接：Cartesian servo、IK、force mapping。
6. 不展开：spatial algebra完整体系。
7. 考核：从2-link FK求J并算EEF velocity。
8. 毕业考点：Jacobian、velocity mapping、`J^T F`。

# Day74 — Inverse Kinematics
1. 今日目标：从目标EEF pose反求joint configuration。
2. 前置：Day72–73。
3. 必须教学：IK无解/多解；analytic vs numerical；`min ||e(q)||²`；Jacobian iteration `Δq=J†Δx`；Damped Least Squares `Δq=J^T(JJ^T+λ²I)^-1Δx`；initial guess；joint limits；collision不由IK自动保证；redundancy；secondary objective/nullspace概念。
4. 深度：IK L4；DLS L3-L4。
5. 工程连接：MoveIt kinematics plugin、grasp pose。
6. 不展开：复杂工业臂解析IK全集。
7. 考核：解释IK成功为何不等于可执行。
8. 毕业考点：IK、多解、DLS、limits。

# Day75 — Singularity / SVD / Manipulability / Redundancy
1. 今日目标：理解为什么某些姿态附近EEF运动会变得困难。
2. 前置：M02 SVD + Day73。
3. 必须教学：rank drop；det仅适用于square J；SVD；singular values；condition number；qdot explosion；manipulability `sqrt(det(JJ^T))` 的适用条件；velocity ellipsoid；redundancy；nullspace `Jz=0`；damping；avoid singularity。
4. 深度：Singularity L4；SVD reasoning L3-L4。
5. 工程连接：base placement、Cartesian control。
6. 不展开：高级manipulability metrics。
7. 考核：给singular values判断condition与控制风险。
8. 毕业考点：Rank、SVD、singularity、manipulability。

# Day76 — Mobile Robot Kinematics
1. 今日目标：正式建立移动机器人运动学，而非只为Hybrid A*临时使用。
2. 前置：Day71 + M11 Day66复用。
3. 必须教学：state `[x,y,θ]`；body/world velocity；unicycle `x_dot=v cosθ`,`y_dot=v sinθ`,`θ_dot=ω`；differential drive `v=(vr+vl)/2`,`ω=(vr-vl)/L`；holonomic/nonholonomic；quadruped导航抽象 `[vx,vy,wz]`；Euler integration；slip/gait/delay/model error；command≠feedback；odometry drift。
4. 深度：Mobile kinematics L4。
5. 工程连接：Nav2/MPPI rollout、chassis feedback。
6. 不展开：详细步态动力学。
7. 考核：从v/ω积分一步pose，并解释model mismatch。
8. 毕业考点：Unicycle、diff-drive、body/world、command/feedback。

# Day77 — Force / Torque / Mass / Inertia / Newton-Euler
1. 今日目标：从运动学进入“为什么会这样运动”。
2. 前置：基础力学。
3. 必须教学：`F=ma`；torque；mass；CoM；inertia matrix；angular momentum直觉；Newton-Euler直觉；gravity torque；Coulomb/viscous friction；actuator/transmission；energy；frame/reference-point依赖。
4. 深度：Force/torque/inertia L3。
5. 工程连接：payload、motor torque、gripper/arm。
6. 不展开：Recursive Newton-Euler算法完整推导。
7. 考核：解释同样角加速度下inertia变化意味着什么。
8. 毕业考点：Force、torque、CoM、inertia、friction。

# Day78 — Lagrangian / Manipulator Dynamics / M-C-g
1. 今日目标：理解机械臂标准动力学方程。
2. 前置：Day77 + M02微积分。
3. 必须教学：generalized coordinate；kinetic/potential energy；`L=T-V`；Euler-Lagrange `d/dt(∂L/∂qdot_i)-∂L/∂q_i=τ_i`；`M(q)qdd+C(q,qdot)qdot+g(q)=τ`；friction扩展；M的物理意义/正定直觉；C与velocity coupling；g；forward/inverse dynamics；model error；kinematic vs torque control；1DOF/pendulum级推导。
4. 深度：M/C/g interpretation L4；简单推导 L3。
5. 工程连接：arm controller、payload、model-based control。
6. 不展开：full multibody/contact dynamics。
7. 考核：解释M/C/g每项及模型误差传播。
8. 毕业考点：Lagrange、Manipulator dynamics。

# Day79 — ODE / State-space / Linearization / Discretization / Action
1. 今日目标：把Dynamics转换成Control/Policy可以消费的state model。
2. 前置：M02 Taylor/Jacobian + Day78。
3. 必须教学：ODE `x_dot=f(x,u,t)`；二阶→一阶 `x=[q;qdot]`；nonlinear state-space `x_dot=f(x,u), y=h(x,u)`；linear model `x_dot=Ax+Bu, y=Cx+Du`；equilibrium；linearization `δx_dot=Aδx+Bδu`，A/B为Jacobian；local validity；discrete `x_(k+1)=A_d x_k+B_d u_k`；Euler `A_d≈I+A dt`,`B_d≈Bdt`；integration方法概念；dt；state vs observation vs action；base/joint/EEF action；action≠physical motion；VLA EEF delta→IK/trajectory/controller/actuator。
4. 深度：ODE/state-space/linearization L4；action semantics L5。
5. 工程连接：LQR/MPC/MPPI/VLA。
6. 不展开：stability正式放M13；高级numerical integrator。
7. 考核：二阶系统改写state-space；解释action链。
8. 毕业考点：State-space、linearization、discretization、action semantics。

---

# M12 Graduation Exam
统一权重：**30%核心基础 / 50%综合系统场景 / 20% Source·Formula·Design**。

## 30% 核心基础
硬门槛：transform chain、2-link FK、Jacobian、IK、singularity、mobile kinematics、`M/C/g`、ODE/state-space、linearization、action semantics。

## 50% 综合系统场景
至少覆盖：
1. 2-link FK + Jacobian手算；
2. IK有解但collision/joint limit/near-singularity的判断；
3. mobile robot v/ω到pose的离散积分与slip/model mismatch；
4. payload变化对dynamics/control的影响；
5. mobile manipulator中Pose/Configuration/State/Observation/Action/Command/Physical Motion责任链。

## 20% Source / Formula / Design
能从URDF/MoveIt/Controller或机器人源码中识别joint/frame/FK/IK/Jacobian/state/action接口，并说明 `τ=J^TF` 的frame约定和 `M(q)qdd+Cqdot+g=τ` 各项。

## 通过标准
总分≥85%；不得混淆pose/configuration/state/action；必须能把公式映射到实际robot variable并判断dimension。

## Day71–Day79 索引
```text
Day71 Rigid Body / Joint / DOF / Kinematic Chain
Day72 Forward Kinematics / DH / POE
Day73 Jacobian / Velocity Kinematics
Day74 Inverse Kinematics
Day75 Singularity / SVD / Manipulability
Day76 Mobile Robot Kinematics
Day77 Force / Torque / Mass / Inertia / Newton-Euler
Day78 Lagrangian / Manipulator Dynamics
Day79 ODE / State-space / Linearization / Discretization / Action
```
