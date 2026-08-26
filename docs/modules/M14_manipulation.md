# M14 — Manipulation

## Module Goal
把 `Object Pose → Grasp Pose → IK → Collision → Motion Planning → Timed Trajectory → Controller → Gripper/Contact → Verification/Recovery` 接成完整Manipulation链。MoveIt2是主工程框架，重点是责任边界、几何与执行语义。

本模块共 7 个理论 Day（Day90–Day96）+ 独立 `LAB01_manipulation_pick_and_place.md`。

---

# Day90 — Manipulation Architecture / RobotState / MoveIt2
1. 今日目标：建立Manipulation全栈和MoveIt2职责边界。
2. 前置：M05/M07 + M12 + M13。
3. 必须教学：joint vs task/Cartesian space；EEF/joint target；URDF/SRDF；planning group；EEF；RobotModel；RobotState；FK/IK/collision query；kinematics plugin；Planning Pipeline；MoveGroup interface；Navigation/Manipulation共同点和差异；Owner failure layers。
4. 深度：Architecture/RobotState L4。
5. 工程连接：MoveIt2、mobile manipulator。
6. 不展开：MoveIt API逐函数。
7. 考核：解释MoveIt2不是单一IK/Planner，RobotState做什么。
8. 毕业考点：Joint/Task Space、RobotModel/RobotState、MoveIt2 Boundary。

# Day91 — Planning Scene / Collision / Attached Object
1. 今日目标：理解Manipulation planning检查整个robot configuration是否合法。
2. 前置：Day90 + M11 collision。
3. 必须教学：Planning Scene；robot state + world objects；visual vs collision geometry；self-collision；self-collision matrix；Allowed Collision Matrix；attached object；attach/detach语义；C-space validity；trajectory intermediate state；discrete/continuous collision概念；collision margin；dynamic scene refresh。
4. 深度：Planning Scene/Attach L4。
5. 工程连接：桌面、长物体、body collision。
6. 不展开：FCL/BVH内部。
7. 考核：为什么抓住物体后必须attach，为什么只检查EEF不够。
8. 毕业考点：Planning Scene、Self-collision、Attached Object。

# Day92 — OMPL / Joint-Cartesian Motion / Time Parameterization
1. 今日目标：理解高维C-space planning、Cartesian approach与path→trajectory。
2. 前置：M11 RRT/OMPL + Day90–91。
3. 必须教学：n-DOF C-space；RRTConnect概念；feasible vs optimal；joint-space path；EEF path由FK映射；Cartesian interpolation；逐步IK+collision；path fraction；constraints；planner vs IK；path simplification/smoothing；path vs trajectory；joint velocity/acceleration limits；time parameterization；execution前validity recheck。
4. 深度：Joint/Cartesian L4；Time Parameterization L3-L4。
5. 工程连接：pre-grasp、approach、lift、trajectory execution。
6. 不展开：planner全集、trajectory optimization深入。
7. 考核：为什么path不能直接逐点发motor。
8. 毕业考点：OMPL、Cartesian Path、Timed Trajectory。

# Day93 — Object Pose / Grasp Pose / Pre-grasp
1. 今日目标：从“物体在哪”推进到“EEF怎样抓”。
2. 前置：M05/M07 + M12 IK。
3. 必须教学：`T_world^object`；`T_object^grasp`；`T_world^grasp=T_world^object T_object^grasp`；transform direction；grasp position/orientation/opening/approach；pre-grasp；approach frame；retreat/lift；candidate filtering：IK/joint/collision/approach/gripper-width；grasp quality概念；perception uncertainty；object symmetry；learned grasp仍需TF/IK/planning/control。
4. 深度：Grasp Geometry L4。
5. 工程连接：RGB-D/6D pose/VLA high-level task。
6. 不展开：grasp wrench space完整推导。
7. 考核：手推object→grasp transform并分析pose error。
8. 毕业考点：Object→Grasp、Pre-grasp、Candidate Filtering。

# Day94 — Contact / Friction / Force Closure / Verification
1. 今日目标：理解close command不等于稳定抓住物体。
2. 前置：M12 force/torque + Day93。
3. 必须教学：normal/tangential force；`|F_t|≤μF_n`；friction cone；no-slip；force closure intuition；form closure区别；gripper force过小/过大；position vs force/current-controlled gripper；contact detection；grasp verification；slip detection；payload/inertia/collision geometry变化；lift verification。
4. 深度：Contact/Friction L3；Verification L4。
5. 工程连接：parallel gripper、fragile object、current feedback。
6. 不展开：contact complementarity、dexterous hand。
7. 考核：给μ/Fn判断slip风险并设计verification。
8. 毕业考点：Contact、Friction、Force Closure、Verification。

# Day95 — Cartesian / Force / Impedance / Compliance Control
1. 今日目标：理解自由空间position control与接触后的force/compliance需求。
2. 前置：M12 Jacobian/Dynamics + M13 Feedback。
3. 必须教学：position control接触风险；force target；`τ=J^T F` 只有在joint torque与wrench/twist使用兼容frame、reference point和sign convention时成立；compliance；stiffness；damping；在本日固定误差定义 `e=x_d-x`，并明确 `F_ext` 的正方向后再使用示意式 `M_d e_ddot + D_d e_dot + K_d e = F_ext`，若实现采用其他error/wrench convention公式符号会相应改变；admittance区别；hybrid position/force；Cartesian servo；singularity；high stiffness+delay contact oscillation；force/torque/workspace limits。
4. 深度：Impedance/Force Mapping L4。
5. 工程连接：插拔、按键、擦拭、易碎物体。
6. 不展开：Operational Space Control完整推导。
7. 考核：先声明frame/error/sign，再解释桌面高度误差为什么会产生大接触力。
8. 毕业考点：Impedance/Compliance、Force Mapping、Convention、Contact Safety。

# Day96 — Pick-and-Place / Recovery / Owner
1. 今日目标：形成Pick/Place全链和分层failure attribution。
2. 前置：Day90–95。
3. 必须教学：Pick pipeline；Place pipeline；failure taxonomy：Perception/TF/Calibration/Grasp/IK/Collision/Planning/Trajectory/Controller/Gripper/Contact/Verification；IK/planning/execution/grasp failure区别；分层recovery；retry budget；scene refresh；mobile-base reposition桥接M18；logging；task/grasp/collision/retry/cycle metrics；VLA high-level vs low-level接口；Owner evidence chain。
4. 深度：Full Chain/Failure Attribution L5。
5. 工程连接：MoveIt + gripper + VLA接口。
6. 不展开：不新增算法。
7. 考核：抓空、IK成功但planning fail、trajectory正常但掉物等跨层场景。
8. 毕业考点：Pick/Place、Recovery、Owner Responsibility。

---

# M14 Graduation Exam
统一权重：**30%核心基础 / 50%综合系统场景 / 20% Source·Formula·Design**。

## 30% 核心基础
硬门槛：MoveIt2责任、RobotState/kinematics plugin、Planning Scene、self-collision/attached object、joint vs Cartesian path、time parameterization、object→grasp transform、pre-grasp、grasp verification、impedance/compliance与frame/sign convention。

## 50% 综合系统场景
至少覆盖：object→grasp transform；IK有解但collision导致planning failure；Cartesian approach连续检查；抓空的Depth/Calibration/Extrinsic/TF/Timestamp排查；抓住后掉落；接触插入震荡。

## 20% Source / Formula / Design
在MoveIt2官方或实际Manipulation package定位 RobotModel/RobotState、kinematics plugin、Planning Scene、state validity、planner、Cartesian path、time parameterization、trajectory execution、attach/detach、gripper/recovery。

## 通过标准
总分≥85%；必须明确 `IK Success ≠ Planning Success ≠ Execution Success ≠ Grasp Success`；能完成object/grasp frame推理并在使用force/impedance公式前声明frame、reference point、error与sign convention。
