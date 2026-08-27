# M14 — Manipulation

## Module Goal
把：

`Object Pose → Grasp Pose → IK → Collision → Motion Planning → Timed Trajectory → Controller → Contact/Gripper → Verification/Recovery`

接成完整Manipulation链。MoveIt2是主工程框架；Modern Robotics补强Grasp/Contact的理论骨架。重点始终是责任边界、几何、接触与真实执行语义。

本模块共 7 个理论 Day（Day90–Day96）+ 独立 `LAB01_manipulation_pick_and_place.md`。

## 主要教材
- **Modern Robotics Chapter 9**：Trajectory Generation辅助参考。
- **Modern Robotics Chapter 12 — Grasping and Manipulation**：Day94–Day95的主要理论参考之一。
- MoveIt2、Planning Scene、OMPL、RobotState、Trajectory Execution等工程部分仍以MoveIt2官方资料与真实系统为主。
- 目标不是做完整Grasp Mechanics数学专门课，而是必须理解 `contact → friction → contact wrench → grasp map → object wrench → closure/stability`。

---

# Day90 — Manipulation Architecture / RobotState / MoveIt2
1. 今日目标：建立Manipulation全栈和MoveIt2职责边界。
2. 前置：M05/M07 + M12 + M13。
3. 必须教学：joint vs task/Cartesian space；EEF/joint target；URDF/SRDF；planning group；EEF；RobotModel；RobotState；FK/IK/collision query；kinematics plugin；Planning Pipeline；MoveGroup interface；Navigation/Manipulation共同点与差异；Owner failure layers。
4. 深度：Architecture/RobotState L4。
5. 工程连接：MoveIt2、mobile manipulator。
6. 不展开：MoveIt API逐函数。
7. 考核：解释MoveIt2不是单一IK/Planner，RobotState做什么。
8. 毕业考点：Joint/Task Space、RobotModel/RobotState、MoveIt2 Boundary。

# Day91 — Planning Scene / Collision / Attached Object
1. 今日目标：理解Manipulation planning检查整个robot configuration是否合法。
2. 前置：Day90 + M11 C-space/collision。
3. 必须教学：Planning Scene；robot state + world objects；visual vs collision geometry；self-collision；self-collision matrix；Allowed Collision Matrix；attached object；attach/detach语义；C-space validity；trajectory intermediate state；discrete/continuous collision概念；collision margin；dynamic scene refresh。
4. 深度：Planning Scene/Attach L4。
5. 工程连接：桌面、长物体、body collision。
6. 不展开：FCL/BVH内部。
7. 考核：为什么抓住物体后必须attach，为什么只检查EEF不够。
8. 毕业考点：Planning Scene、Self-collision、Attached Object。

# Day92 — OMPL / Joint-Cartesian Motion / Trajectory / Time Parameterization
1. 今日目标：理解高维C-space planning、Cartesian approach与path→timed trajectory。
2. 前置：M11 RRT/OMPL + M12/M13 trajectory bridge + Day90–91。
3. 必须教学：n-DOF C-space；RRTConnect概念；feasible vs optimal；joint-space path；EEF path由FK映射；Cartesian interpolation；逐步IK+collision；path fraction；constraints；planner vs IK；path simplification/smoothing；path vs trajectory；joint velocity/acceleration limits；time parameterization；execution前validity recheck；Modern Robotics Ch9与MoveIt trajectory semantics对照。
4. 深度：Joint/Cartesian L4；Time Parameterization L3-L4。
5. 工程连接：pre-grasp、approach、lift、trajectory execution。
6. 不展开：planner全集、trajectory optimization深入。
7. 考核：为什么path不能直接逐点发motor；time scaling改变什么、不改变什么。
8. 毕业考点：OMPL、Cartesian Path、Timed Trajectory。

# Day93 — Object Pose / Grasp Pose / Pre-grasp
1. 今日目标：从“物体在哪”推进到“EEF怎样抓”。
2. 前置：M05/M07 + M12 IK/SE(3)。
3. 必须教学：`T_world^object`；`T_object^grasp`；`T_world^grasp=T_world^object T_object^grasp`；transform direction；grasp position/orientation/opening/approach；pre-grasp；approach frame；retreat/lift；candidate filtering：IK/joint/collision/approach/gripper-width；grasp quality概念；perception uncertainty；object symmetry；learned grasp仍需TF/IK/planning/control。
4. 深度：Grasp Geometry L4。
5. 工程连接：RGB-D/6D pose/VLA high-level task。
6. 不展开：grasp wrench space完整推导留Day94只学最小需要部分。
7. 考核：手推object→grasp transform并分析pose error。
8. 毕业考点：Object→Grasp、Pre-grasp、Candidate Filtering。

# Day94 — Contact / Friction Cone / Wrench / Grasp Map / Closure / Verification
1. 今日目标：理解“gripper close”为什么不等于物体被稳定约束，并建立Modern Robotics Ch12的最小抓取力学主线。
2. 前置：M12 Twist/Wrench/Statics + Day93。
3. 必须教学：contact model的基本类型（frictionless point / frictional point概念）；normal/tangential force；Coulomb friction `||f_t||≤μf_n`；2D/3D friction cone与polyhedral approximation概念；contact force/wrench必须声明作用对象、frame、reference point；多个contact force组合；**Grasp Map `G`**：把contact-force/wrench coordinates映射为object net wrench，写作 `F_o=Gf` 时必须先声明sign convention；internal force概念；form closure vs force closure；force closure直觉：允许的contact forces能否抵抗任意小external wrench；gripper force过小/过大；position vs force/current-controlled gripper；contact/slip detection；grasp verification；payload/inertia/collision geometry变化；lift verification。
4. 深度：Friction/Contact L3-L4；Grasp Map/Closure L3；Verification L4。
5. 工程连接：parallel gripper、fragile object、force/current feedback、learned grasp candidate验证。
6. 不展开：grasp wrench space凸包完整计算、contact complementarity、dexterous hand优化。
7. 考核：给μ/Fn判断slip风险；解释`G`输入输出；为什么两个contact位置/方向改变会改变可抵抗的object wrench；设计grasp verification。
8. 毕业考点：Contact、Friction Cone、Wrench、Grasp Map、Form/Force Closure、Verification。

# Day95 — Cartesian / Force / Impedance / Compliance Control
1. 今日目标：理解自由空间position control与接触后的force/compliance需求，并把M12的Jacobian/Wrench真正用于contact control。
2. 前置：M12 Jacobian/Dynamics/Wrench + M13 Feedback。
3. 必须教学：position control接触风险；force target；`τ=J^T F`必须保证joint torque与wrench/twist使用兼容frame/reference point/sign convention；Grasp/Contact wrench与joint effort接口；compliance；stiffness；damping；固定误差定义 `e=x_d-x` 并明确 `F_ext` 正方向后再使用示意式 `M_d e_ddot + D_d e_dot + K_d e = F_ext`；admittance区别；hybrid position/force；Cartesian servo；singularity；high stiffness + delay引起contact oscillation；force/torque/workspace limits。
4. 深度：Impedance/Force Mapping L4。
5. 工程连接：插拔、按键、擦拭、易碎物体、VLA low-level EEF action safety。
6. 不展开：Operational Space Control完整推导。
7. 考核：先声明frame/error/sign，再解释桌面高度误差为什么会产生大接触力；把object/contact wrench追到joint torque。
8. 毕业考点：Impedance/Compliance、Force Mapping、Convention、Contact Safety。

# Day96 — Pick-and-Place / Recovery / Owner
1. 今日目标：形成Pick/Place全链和分层failure attribution。
2. 前置：Day90–95。
3. 必须教学：Pick pipeline；Place pipeline；failure taxonomy：Perception/TF/Calibration/Grasp Geometry/Grasp Mechanics/IK/Collision/Planning/Trajectory/Controller/Gripper/Contact/Verification；IK/planning/execution/grasp failure区别；分层recovery；retry budget；scene refresh；mobile-base reposition桥接M18；logging；task/grasp/collision/retry/cycle metrics；VLA high-level vs low-level接口；Owner evidence chain。
4. 深度：Full Chain/Failure Attribution L5。
5. 工程连接：MoveIt + gripper + force/contact + VLA接口。
6. 不展开：不新增算法。
7. 考核：抓空、IK成功但planning fail、trajectory正常但force closure不足/掉物、接触震荡等跨层场景。
8. 毕业考点：Pick/Place、Recovery、Owner Responsibility。

---

# M14 Graduation Exam
统一权重：**30%核心基础 / 50%综合系统场景 / 20% Source·Formula·Design**。

## 30% 核心基础
硬门槛：MoveIt2责任、RobotState/kinematics plugin、Planning Scene、self-collision/attached object、joint vs Cartesian path、time parameterization、object→grasp transform、pre-grasp、friction cone、wrench/grasp map、form vs force closure、grasp verification、impedance/compliance与frame/sign convention。

## 50% 综合系统场景
至少覆盖：object→grasp transform；IK有解但collision导致planning failure；Cartesian approach连续检查；抓空的Depth/Calibration/Extrinsic/TF/Timestamp排查；contact位置/摩擦不足导致grasp不稳定；抓住后掉落；接触插入震荡。

## 20% Source / Formula / Design
在MoveIt2官方/实际Manipulation package定位RobotModel/RobotState、kinematics plugin、Planning Scene、state validity、planner、Cartesian path、time parameterization、trajectory execution、attach/detach、gripper/recovery；并能用Modern Robotics Ch12解释contact wrench、grasp map和closure的物理意义。

## 通过标准
- 总分≥85%；
- 必须明确 `IK Success ≠ Planning Success ≠ Execution Success ≠ Grasp Success`；
- 必须明确 `Gripper Close ≠ Force Closure ≠ Physical Grasp Verified`；
- 能完成object/grasp frame推理，并在使用grasp/force/impedance公式前声明frame、reference point、作用对象与sign convention。
