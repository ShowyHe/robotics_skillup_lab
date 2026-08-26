# M14 — Manipulation

## Module Goal
把 `Object Pose → Grasp Pose → IK → Collision Checking → Motion Planning → Trajectory → Controller → Gripper/Contact → Verification/Recovery` 接成完整机械臂操作链。MoveIt2是主工程框架，但学习重点是责任边界和数学/系统语义。

本模块共 7 个理论 Day（Day90–Day96）+ 1 个独立 LAB（`docs/labs/LAB01_manipulation_pick_and_place.md`）。

---

# Day90 — Manipulation Architecture / Robot Model / MoveIt2
1. 今日目标：建立Manipulation全栈和MoveIt2职责边界。
2. 前置：M05/M07 + M12 + M13。
3. 必须教学：Manipulation定义；joint space vs task/Cartesian space；EEF target vs joint target；URDF vs SRDF；planning group；end-effector；MoveIt2整体角色；RobotModel；RobotState（当前joint configuration、FK/IK/collision查询载体）；kinematics plugin职责；Planning Pipeline；MoveGroup/接口概念；Manipulation与Navigation共同点/差异；Owner failure layers。
4. 深度：Architecture/RobotState L4；MoveIt boundary L4。
5. 工程连接：mobile manipulator、MoveIt2。
6. 不展开：MoveIt API逐函数。
7. 考核：解释MoveIt2不是单一IK/Planner，RobotState在链路中做什么。
8. 毕业考点：Joint/Task space、RobotModel/RobotState、MoveIt2 responsibility。

# Day91 — Planning Scene / Collision / Self-collision / Attached Object
1. 今日目标：理解Manipulation planning检查的是整个robot configuration是否合法。
2. 前置：Day90 + M11 collision。
3. 必须教学：Planning Scene；robot state + world objects；visual vs collision geometry；simplified collision geometry；environment collision objects；self-collision；self-collision matrix；Allowed Collision Matrix；attached object；attach/detach语义；C-space validity；trajectory中间state检查；discrete vs continuous collision概念；collision margin；dynamic scene refresh。
4. 深度：Planning Scene/self-collision/attach L4。
5. 工程连接：抓长物体、桌面、机械臂与body碰撞。
6. 不展开：FCL/BVH内部算法。
7. 考核：抓住物体后为何必须attach，为什么只检查EEF不够。
8. 毕业考点：Planning Scene、self-collision、attached object。

# Day92 — Manipulator Motion Planning / OMPL / Cartesian Motion / Trajectory Time Parameterization
1. 今日目标：理解高维C-space motion planning与Cartesian task motion，并补齐path→timed trajectory。
2. 前置：M11 RRT/RRT* + Day90–91。
3. 必须教学：n-DOF C-space；curse of dimensionality；OMPL角色；RRTConnect双树思想；feasible vs optimal；joint-space path；EEF路径由FK映射；Cartesian path/interpolation；逐步IK + collision；path fraction；orientation/position/joint constraints；planner与IK的关系；sampling path simplification/smoothing；**path vs trajectory**；joint velocity/acceleration limits；time parameterization把几何joint path赋时间；trajectory validity/execution前再次检查。
4. 深度：OMPL L3；joint/Cartesian distinction L4；time parameterization L3-L4。
5. 工程连接：pre-grasp free-space、approach/lift Cartesian、MoveIt trajectory execution。
6. 不展开：OMPL planner全集、trajectory optimization深入。
7. 考核：解释规划得到path后为何不能直接逐点发给motor。
8. 毕业考点：OMPL、Cartesian path、trajectory/time parameterization。

# Day93 — Object Pose / Grasp Pose / Candidate / Pre-grasp / Approach
1. 今日目标：从“知道物体在哪里”推进到“EEF应该以什么姿态抓”。
2. 前置：M05/M07 geometry + M12 IK。
3. 必须教学：`T_world^object`；grasp pose `T_world^grasp`；object-frame grasp `T_object^grasp`；`T_world^grasp=T_world^object T_object^grasp`；transform direction；grasp包含position/orientation/finger opening/approach/contact；pre-grasp；approach vector和所属frame；retreat/lift；multiple candidates；IK/joint limit/collision/approach/gripper-width filters；grasp quality概念；perception uncertainty；object symmetry；geometric vs task-aware grasp；learned grasp output仍需TF/IK/planning/control。
4. 深度：Grasp geometry L4。
5. 工程连接：RGB-D/6D pose/VLA high-level task。
6. 不展开：grasp wrench space完整推导。
7. 考核：手推object→grasp transform并分析2cm pose error。
8. 毕业考点：Object pose→Grasp pose、pre-grasp、candidate filtering。

# Day94 — Gripper / Contact / Friction / Force Closure / Grasp Verification
1. 今日目标：理解“close command sent”不等于稳定抓住物体。
2. 前置：M12 force/torque + Day93。
3. 必须教学：normal/tangential force；`|F_t|≤μF_n`；friction cone；no-slip；two-finger grasp；force closure intuition；form closure distinction；gripper force过小/过大；position-controlled vs force/current-controlled gripper；contact detection；grasp success verification；slip detection；payload/inertia/collision geometry变化；lift verification。
4. 深度：Contact/friction L3；verification L4。
5. 工程连接：parallel gripper、fragile object、current feedback。
6. 不展开：rigid contact complementarity、dexterous hand。
7. 考核：给μ/Fn判断slip风险并设计verification。
8. 毕业考点：Contact、friction、force closure、grasp verification。

# Day95 — Cartesian / Force / Impedance / Compliance Control
1. 今日目标：理解自由空间position control与接触环境后的force/compliance需求。
2. 前置：M12 Jacobian/dynamics + M13 feedback。
3. 必须教学：pure position control接触风险；force target；`τ=J^TF` frame/wrench语义；compliance；stiffness `F=K(x_d-x)`；damping；impedance `M_d e_ddot+D_d e_dot+K_d e=F_ext`的物理意义；admittance区别；hybrid position/force；Cartesian servo；singularity影响；high stiffness+delay的contact oscillation；force/torque/workspace safety limits。
4. 深度：Impedance L4；Cartesian/force mapping L4。
5. 工程连接：插拔、按键、擦拭、易碎物体。
6. 不展开：Operational Space Control完整推导。
7. 考核：解释为什么桌面高度误差会让纯position control产生大力。
8. 毕业考点：Impedance/compliance、force mapping、contact safety。

# Day96 — Pick-and-Place Full Chain / Recovery / Manipulation Owner
1. 今日目标：形成完整Pick/Place证据链并进行分层failure attribution。
2. 前置：Day90–95。
3. 必须教学：Pick pipeline（perception→pose→candidate→pre-grasp→approach→close→verify→attach→lift）；Place pipeline（target→plan→approach→release→detach→retreat→verify）；failure taxonomy：Perception/TF/Calibration/Grasp/IK/Collision/Planning/Trajectory/Controller/Gripper/Contact/Verification；IK failure原因；planning failure原因；execution failure；grasp failure；分层recovery；retry budget；scene refresh；mobile-base reposition桥接M18；logging字段；task/grasp/collision/retry/cycle metrics；VLA high-level vs low-level manipulation接口；Owner evidence chain。
4. 深度：Full chain/failure attribution L5。
5. 工程连接：真实MoveIt + gripper + VLA接口。
6. 不展开：不新增算法。
7. 考核：抓空、IK成功但planning fail、trajectory正常但掉物等跨层场景。
8. 毕业考点：Pick/Place、Recovery、Owner responsibility。

---

# M14 Graduation Exam
统一权重：**30%核心基础 / 50%综合系统场景 / 20% Source·Formula·Design**。

## 30% 核心基础
硬门槛：MoveIt2责任、RobotState/kinematics plugin、Planning Scene、self-collision/attached object、joint vs Cartesian path、time parameterization、object→grasp transform、pre-grasp、grasp verification、impedance/compliance。

## 50% 综合系统场景
至少覆盖：
1. `T_world^object T_object^grasp` transform推理；
2. IK有解但self/environment collision导致planning failure；
3. Cartesian approach中IK/singularity/collision连续检查；
4. 抓空：Depth/Calibration/Extrinsic/TF/Timestamp/Grasp Transform排查；
5. 抓住后掉落：contact/friction/force/current/payload；
6. 接触插入震荡：stiffness/damping/latency/force limit。

## 20% Source / Formula / Design
在MoveIt2官方实现或实际Manipulation package中定位：RobotModel/RobotState、kinematics plugin、Planning Scene、state validity、planner、Cartesian path、time parameterization、trajectory execution、attach/detach、gripper/recovery。

## 通过标准
总分≥85%；必须明确 `IK Success ≠ Planning Success ≠ Execution Success ≠ Grasp Success`；能完成object/grasp frame推理并做跨层failure attribution。

## Day90–Day96 索引
```text
Day90 Manipulation Architecture / RobotState / MoveIt2
Day91 Planning Scene / Collision / Attached Object
Day92 OMPL / Joint-Cartesian Motion / Time Parameterization
Day93 Object Pose / Grasp Pose / Pre-grasp
Day94 Contact / Friction / Force Closure / Verification
Day95 Cartesian / Force / Impedance Control
Day96 Pick-and-Place / Recovery / Owner
```
