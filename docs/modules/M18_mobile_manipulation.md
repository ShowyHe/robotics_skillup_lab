# M18 — Mobile Manipulation

## Module Goal
把 Navigation、Perception、Manipulation、VLA 与 long-horizon task state 合流，解决“目标不在机械臂当前可达范围内时，机器人如何移动底盘、重新感知、重新定位、重新规划并完成抓取/放置”。

主线：`Task → Find Target → Estimate Pose → Check Reachability → Base Reposition → Re-localize/Re-perceive → Manipulate → Carry → Navigate → Place → Verify/Recover`。

本模块共 3 个理论 Day（Day116–Day118）+ 1 个独立 Capstone LAB（`docs/labs/LAB02_mobile_manipulation_capstone.md`）。

---

# Day116 — Base + Arm Geometry / Reachability / Base Placement
1. 今日目标：理解base pose本身也是Manipulation feasibility的一部分。
2. 前置：M11 + M12 + M14。
3. 必须教学：mobile manipulator configuration `[x_b,y_b,θ_b,q_arm]`；base pose移动arm workspace；`T_base^target=(T_world^base)^-1 T_world^target`；geometric/kinematic/collision-free/executable reachability；reachability map概念；base placement optimization；candidate base pose；navigation feasibility + arm IK + collision + manipulability + visibility；base orientation；navigation goal≠manipulation-ready pose；final base error传播；fine alignment；reachability vs manipulability；singularity margin；camera visibility/occlusion。
4. 深度：Base-arm geometry/base placement L5。
5. 工程连接：导航到“1m内”为什么不等于能抓。
6. 不展开：whole-body nonlinear optimization。
7. 考核：候选base pose综合navigation/IK/collision/manipulability选择。
8. 毕业考点：Transform、Reachability、Base Placement、Manipulability。

# Day117 — Navigation→Manipulation Handoff / Re-perception / Whole-body / Long-horizon State
1. 今日目标：理解Navigation结束不代表Manipulation可直接使用旧感知；同时建立long-horizon task state和混合编排。
2. 前置：Day116 + M09/M10/M14。
3. 必须教学：task phases Search→Navigate→Align→Re-perceive→Manipulate→Verify；handoff只是到达候选操作区域；为什么必须re-perceive；stale object pose；base/TF/joint state freshness；Planning Scene refresh；fine alignment；sequential base-stop→arm vs simultaneous whole-body；whole-body configuration/collision；sequential vs whole-body trade-off；handoff contract：base pose/frame/time/target ID/pose/freshness/reachability/scene validity；not-reachable→reposition→reobserve→retry；candidate scoring/hysteresis/retry budget；BT/FSM/Skill/VLA hybrid orchestration；**long-horizon task state**：current skill、target identity、object attached state、retry count、last valid observation、failure reason、resume/recovery point；任务中断后不能只靠语言上下文恢复物理状态。
4. 深度：Handoff/freshness/long-horizon state L5。
5. 工程连接：Nav2→MoveIt→VLA skill orchestration。
6. 不展开：whole-body optimal control。
7. 考核：导航8秒后为什么old object pose不能直接抓；设计FSM/BT state contract。
8. 毕业考点：Handoff、Re-perception、Long-horizon State、Hybrid Orchestration。

# Day118 — End-to-End Mobile Manipulation / VLA Integration / Owner
1. 今日目标：从自然语言任务一路追到motor/feedback并能跨模块归因。
2. 前置：M00–M17核心，重点M11–M17。
3. 必须教学：task decomposition；semantic target vs metric navigation/manipulation goal；object附近可操作base pose generation；`T_world^object→T_world^grasp`；pick→attach→safe carry configuration；carry时payload/clearance/arm posture/speed变化；navigate while carrying；placement support/release/stability；pick/place verification；high-level VLA skill sequence vs low-level continuous action；hierarchical architecture；skill library；skill preconditions/postconditions；recovery tree；long-horizon state persistence；cross-module error propagation；system logging/evidence chain；system metrics：task/nav/grasp/place/intervention/collision/retry/cycle time；每个接口必须定义Input/Output/Frame/Timestamp/Validity/Failure Reason。
4. 深度：Full-chain/Owner L5。
5. 工程连接：Nav2 + MoveIt2 + VLM/VLA + hardware。
6. 不展开：不新增算法。
7. 考核：“去厨房拿红瓶放会议桌”完整architecture + failure tree。
8. 毕业考点：Mobile Manipulation full chain、VLA integration、Recovery/Owner。

---

# M18 Graduation Exam
统一权重：**30%核心基础 / 50%综合系统场景 / 20% Source·Formula·Design**。

## 30% 核心基础
硬门槛：base-arm transform、reachability/base placement、navigation→manipulation handoff、re-perception/freshness、long-horizon state、attached/carry state、skill pre/postconditions、recovery、high/low-level VLA boundary。

## 50% 综合系统场景
至少覆盖：
1. `T_base^object=(T_world^base)^-1 T_world^object`；
2. 导航成功但arm不可达的base placement排查；
3. stale perception与re-observation；
4. attached长物体后navigation碰墙；
5. long-horizon任务中断后如何恢复target/attachment/retry/world state；
6. high-level VLA→Nav2/MoveIt vs direct base+arm action的data/safety/debug/latency比较。

## 20% Source / Formula / Design
设计并能在实际系统中定位：task state/BT-FSM、navigation goal、base pose verification、re-perception、reachability/IK、MoveIt planning、attach/detach、carry mode、verification、recovery/logging接口。

## 通过标准
总分≥85%；必须明确 `Navigation Success ≠ Manipulation Ready`；freshness和base placement为硬门槛；能对完整long-horizon mobile manipulation失败进行跨模块归因。

## Day116–Day118 索引
```text
Day116 Base + Arm Geometry / Reachability / Base Placement
Day117 Nav→Manipulation Handoff / Re-perception / Whole-body / Long-horizon State
Day118 End-to-End Mobile Manipulation / VLA Integration / Owner
```
