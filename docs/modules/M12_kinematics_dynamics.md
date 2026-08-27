# M12 — Robot Kinematics / Dynamics / System Dynamics

## Module Goal
建立现代机器人运动学—动力学主线：

```text
Configuration / Rigid Body
→ Screw Axis / Twist / Wrench
→ FK / POE
→ Space & Body Jacobian / Adjoint
→ IK
→ Singularity / Manipulability
→ Mobile Robot Constraints
→ Newton-Euler / Lagrange Dynamics
→ ODE / State-space / Linearization / Action
```

本模块共 9 个理论 Day（Day71–Day79）。这是《Modern Robotics》在本课程中最核心的模块。

## 主要教材
- **Modern Robotics Chapter 2–8 + Chapter 13**：M12主要理论教材。
- **POE（Product of Exponentials）作为Forward Kinematics主线**；DH必须会读、会理解，但不再作为唯一/最高优先级运动学语言。
- Chapter 7 Closed-chain Kinematics只做边界认识；当前主线不深入闭链机构求解。
- 课程目标不是背教材公式，而是把 `frame / twist / Jacobian / wrench / dynamics` 映射到MoveIt、controller、mobile manipulation与VLA action interface。

---

# Day71 — Configuration Space / Joint / Screw Axis / Twist / Wrench
1. 今日目标：建立机械臂/移动机器人统一的configuration与刚体运动语言。
2. 前置：M02 matrix/coordinate + M08 SE(3)/Exp-Log。
3. 必须教学：rigid body；link/joint/DOF；configuration `q`与configuration space；base/link/EEF frame；kinematic chain/tree；joint limit；pose vs configuration；URDF tree；screw motion intuition；screw axis `S=[ω;v]` 的几何含义；pure rotation/pure translation；Twist `V=[ω;v]` 表示刚体瞬时运动且必须带frame语义；Wrench `F=[m;f]` 表示moment/force并依赖frame/reference point；twist/wrench是对偶量的功率直觉 `Power=F^T V`。
4. 深度：Configuration L4；Screw/Twist L3-L4；Wrench L3。
5. 工程连接：MoveIt RobotModel、joint_states、Cartesian servo、force sensor。
6. 不展开：Plücker coordinate严格几何证明、spatial algebra完整体系。
7. 考核：给revolute/prismatic joint写其screw-axis语义；解释同一个physical motion为什么换frame后6D数值会改变。
8. 毕业考点：Configuration、Screw Axis、Twist、Wrench、Frame Semantics。

# Day72 — Forward Kinematics / POE 主线 / DH 辅助
1. 今日目标：从joint configuration算EEF pose，并真正理解POE怎样把各joint screw motion组合成刚体变换。
2. 前置：Day71 + M08 Exp/SE(3)。
3. 必须教学：`T_base^ee=FK(q)`；2-link planar FK手算；home configuration `M`；single joint motion `e^[S]θ`；**Space-form POE** `T(θ)=e^[S1]θ1 ... e^[Sn]θn M`；每个screw axis表达在哪个frame；transform order；Body-form POE概念；DH `a,α,d,θ`与standard/modified convention；DH vs POE：DH是经典parameterization，POE用screw/SE(3)统一表达；FK source implementation如何核对frame/convention。
4. 深度：2-link FK L4；POE L4；DH L3。
5. 工程连接：MoveIt FK、robot model、EEF pose、calibration chain。
6. 不展开：复杂6轴整套数字手算、matrix exponential严格证明。
7. 考核：手算2-link FK；给2–3关节screw axes解释POE乘法顺序与`M`含义。
8. 毕业考点：FK、POE、DH、Transform Order。

# Day73 — Space/Body Jacobian / Adjoint / Velocity Kinematics / Statics
1. 今日目标：理解joint velocity如何映射到不同frame下的EEF twist，以及wrench如何映射回joint torque。
2. 前置：Day71–72 + M02 Jacobian。
3. 必须教学：`V_s=J_s(q) q_dot`；Space Jacobian columns的screw-axis传播；`V_b=J_b(q) q_dot`；Space vs Body representation；Adjoint `Ad_T` 的作用：同一个twist在不同frame之间变换；`V_s=Ad_T V_b` / 对应convention必须先声明T方向；wrench采用对偶变换并必须核对force-on-object/reference-point convention；2-link geometric Jacobian推导；inverse velocity；pseudoinverse；virtual work/power得到 `τ=J^T F`，要求J与wrench/twist在兼容frame和reference point表达。
4. 深度：Jacobian L4；Space/Body Jacobian L4；Adjoint L3-L4；`J^TF` L4。
5. 工程连接：Cartesian servo、numerical IK、force mapping、MoveIt Servo、manipulation controller。
6. 不展开：Adjoint群论证明、spatial vector完整递归算法。
7. 考核：给`J_s/J_b`解释二者为什么都对；完成一个简单Adjoint frame conversion；从virtual work解释`τ=J^TF`。
8. 毕业考点：Space/Body Jacobian、Adjoint、Twist/Wrench Frame、Statics。

# Day74 — Inverse Kinematics / Newton-Raphson / DLS
1. 今日目标：从目标EEF pose反求joint configuration，并理解数值IK本质是反复使用pose error和Jacobian。
2. 前置：Day72–73。
3. 必须教学：IK无解/多解；analytic vs numerical；pose error在明确frame中的表示；Newton-Raphson思想；`Δq=J†e`；`q_(k+1)=q_k+Δq`；Damped Least Squares；initial guess；joint limits；collision不由IK自动保证；redundancy；secondary objective/nullspace；Space/Body error convention必须与对应Jacobian一致。
4. 深度：Numerical IK L4；DLS L3-L4。
5. 工程连接：MoveIt kinematics plugin、grasp pose、Cartesian servo。
6. 不展开：工业臂解析IK全集。
7. 考核：给目标pose解释一次Newton迭代；解释IK success为何不等于可执行。
8. 毕业考点：IK、Pose Error、Jacobian Iteration、DLS、Limits。

# Day75 — Singularity / SVD / Manipulability / Redundancy
1. 今日目标：理解Jacobian在哪些configuration失去运动/力传递能力，以及怎样定量发现。
2. 前置：M02 SVD + Day73–74。
3. 必须教学：rank drop；square/non-square J；SVD；singular values；condition number；qdot amplification；velocity manipulability ellipsoid；`sqrt(det(JJ^T))`及适用条件；force/velocity duality intuition；redundancy；nullspace `Jz=0`；damping；singularity avoidance；base placement与arm posture。
4. 深度：Singularity L4；SVD/Manipulability L3-L4。
5. 工程连接：Cartesian control、mobile manipulation base placement。
6. 不展开：高级dynamic manipulability。
7. 考核：给singular values判断condition与控制风险；解释“理论可达但near-singular”为什么不理想。
8. 毕业考点：Rank、SVD、Singularity、Manipulability、Nullspace。

# Day76 — Wheeled Mobile Robot Kinematics / Nonholonomic Constraint
1. 今日目标：正式建立移动机器人运动学与轮约束，为M11 Hybrid A*、M13 MPPI和Mobile Manipulation统一底盘模型。
2. 前置：Day71 + M11 Day66。
3. 必须教学：state `[x,y,θ]`；body/world velocity；unicycle `x_dot=v cosθ`,`y_dot=v sinθ`,`θ_dot=ω`；differential drive `v=(vr+vl)/2`,`ω=(vr-vl)/L`；rolling/no-slip constraints；holonomic vs nonholonomic；差速车侧向约束 `-sinθ x_dot + cosθ y_dot = 0`；Pfaffian constraint `A(q)q_dot=0` intuition；wheel constraint→allowed instantaneous velocity；Euler integration；quadruped `[vx,vy,wz]` abstraction及其与真实gait/slip差距；command≠feedback；odometry drift/model error。
4. 深度：Mobile kinematics L4；Nonholonomic constraint L3-L4。
5. 工程连接：Nav2/Hybrid A*/MPPI rollout/chassis feedback。
6. 不展开：详细轮胎动力学、quadruped gait dynamics。
7. 考核：从v/ω积分pose；解释为什么diff-drive不能瞬间横移；把constraint和Hybrid A* primitive联系起来。
8. 毕业考点：Unicycle、Diff-drive、Nonholonomic、Body/World、Command/Feedback。

# Day77 — Force / Torque / Spatial Motion / Inertia / Newton-Euler
1. 今日目标：从运动学进入刚体动力学，理解force/torque怎样产生acceleration以及Newton-Euler如何沿机器人链递归传播。
2. 前置：Day71–73 + 基础力学。
3. 必须教学：`F=ma`；torque/moment；mass；CoM；inertia matrix；angular momentum；spatial velocity/wrench与Day71连接；Newton-Euler force/moment balance；gravity；friction；actuator/transmission；energy/power；frame/reference-point dependence；inverse dynamics递归思想；payload改变inertia与required torque。
4. 深度：Force/Torque/Inertia L3-L4；Newton-Euler structure L3。
5. 工程连接：arm payload、gripper、motor torque、force sensing。
6. 不展开：Recursive Newton-Euler全矩阵手推。
7. 考核：解释inertia/payload变化对同一desired acceleration的影响；指出wrench换reference point为何数值改变。
8. 毕业考点：Force、Wrench、CoM、Inertia、Newton-Euler。

# Day78 — Lagrangian / Manipulator Dynamics / M-C-g / Forward-Inverse Dynamics
1. 今日目标：理解机械臂标准动力学模型以及它如何连接model-based control。
2. 前置：Day77 + M02微积分。
3. 必须教学：generalized coordinate；kinetic/potential energy；`L=T-V`；Euler-Lagrange；`M(q)qdd+C(q,qdot)qdot+g(q)=τ`；friction扩展；`M`正定/惯性意义；C velocity coupling；gravity；**inverse dynamics** `(q,qdot,qdd)→τ`；**forward dynamics** `(q,qdot,τ)→qdd`；Newton-Euler与Lagrange是不同建模/计算视角但描述同一物理系统；model error；kinematic vs torque control；1DOF/2DOF简单推导。
4. 深度：M/C/g L4；Forward/Inverse Dynamics L4；简单推导 L3。
5. 工程连接：arm controller、payload compensation、computed torque前置。
6. 不展开：full contact dynamics、完整floating-base dynamics。
7. 考核：解释M/C/g与forward/inverse dynamics；分析payload模型错如何影响tracking。
8. 毕业考点：Manipulator Dynamics、Forward/Inverse Dynamics。

# Day79 — ODE / State-space / Linearization / Discretization / Action Interface
1. 今日目标：把robot dynamics转换成Control和Learned Policy可以消费的state model。
2. 前置：M02 Taylor/Jacobian + Day78。
3. 必须教学：ODE `x_dot=f(x,u,t)`；二阶→一阶 `x=[q;qdot]`；nonlinear state-space；linear `x_dot=Ax+Bu, y=Cx+Du`；equilibrium；linearization `δx_dot=Aδx+Bδu`；local validity；discrete model；Euler `A_d≈I+A dt`,`B_d≈Bdt`；integration/dt；state vs observation vs action；joint/EEF/base action；action≠controller command≠physical motion；VLA EEF delta→IK/trajectory/controller/actuator；M13 interface：reference/trajectory/control需要明确模型与state。
4. 深度：ODE/State-space/Linearization L4；Action Semantics L5。
5. 工程连接：LQR/MPC/MPPI/VLA。
6. 不展开：formal stability放M13；高级integrator。
7. 考核：二阶系统改写state-space；解释VLA action到physical motion责任链。
8. 毕业考点：State-space、Linearization、Discretization、Action Semantics。

---

# M12 Graduation Exam
统一权重：**30%核心基础 / 50%综合系统场景 / 20% Source·Formula·Design**。

## 30% 核心基础
硬门槛：Configuration；Screw/Twist/Wrench；POE FK；Space/Body Jacobian；Adjoint frame transformation；IK；Singularity；Mobile Nonholonomic Kinematics；`M/C/g`；Forward/Inverse Dynamics；ODE/state-space；Action Semantics。

## 50% 综合系统场景
至少覆盖：
1. 2-link FK + Jacobian手算，并用POE语言解释与传统几何解的关系；
2. 给screw axes/home pose解释POE和Space/Body frame；
3. Twist/Wrench换frame + `τ=J^TF` convention；
4. IK有解但joint limit/collision/near-singularity；
5. diff-drive约束与v/ω积分、slip/model mismatch；
6. payload变化对inverse dynamics/control影响；
7. Mobile Manipulator中Pose/Configuration/State/Observation/Action/Command/Motion责任链。

## 20% Source / Formula / Design
能从Modern Robotics Ch2–8/13、URDF/MoveIt/Controller源码中识别screw axis、FK/POE、Jacobian、frame、IK、state/action接口；必须能解释 `T=e^[S1]θ1...e^[Sn]θnM`、`V=Jqdot`、`τ=J^TF`、`M(q)qdd+Cqdot+g=τ` 的变量、frame、dimension和机器人意义。

## 通过标准
- 总分≥85%；
- 不得混淆pose/configuration/twist/state/action；
- POE、Space/Body Jacobian、Adjoint/frame、`J^TF`属于新的M12核心硬门槛；
- 必须能把教材公式映射到MoveIt/Controller/真实robot variable，而不是只会教材例题。

## Day71–Day79 索引
```text
Day71  Configuration / Screw Axis / Twist / Wrench
Day72  Forward Kinematics / POE Mainline / DH
Day73  Space-Body Jacobian / Adjoint / Velocity Kinematics / Statics
Day74  Inverse Kinematics / Newton / DLS
Day75  Singularity / SVD / Manipulability / Redundancy
Day76  Wheeled Mobile Kinematics / Nonholonomic Constraint
Day77  Force / Wrench / Inertia / Newton-Euler
Day78  Lagrangian / Manipulator Dynamics / Forward-Inverse Dynamics
Day79  ODE / State-space / Linearization / Discretization / Action
```
