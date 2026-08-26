# LAB01 — Manipulation Pick-and-Place Full Chain

## Goal
验证 M14 中理论无法完全替代的 Manipulation 全链路：`Object Pose → TF → Grasp Pose → IK → Collision → Planning → Timed Trajectory → Gripper → Attach → Lift → Place → Detach → Verification`。

本LAB不追求大量代码，重点验证frame、state、collision、execution、contact与recovery。

## Recommended Order
1. Simulation first；
2. 完成可重复故障注入；
3. 条件允许再迁移Real Robot。

## Minimum System
- URDF/SRDF + MoveIt2 RobotModel/RobotState；
- Planning Scene；
- IK/kinematics plugin；
- OMPL planner；
- trajectory time parameterization + execution；
- object pose（真实感知或已知ground truth均可）；
- gripper interface；
- attach/detach；
- logging与task result。

## Required Procedure
1. 获得 `T_world^object`；
2. 定义 `T_object^grasp`；
3. 计算 `T_world^grasp=T_world^object T_object^grasp`；
4. 生成pre-grasp；
5. 检查IK、joint limits、self/environment collision；
6. free-space规划到pre-grasp；
7. Cartesian approach；
8. close gripper并verification；
9. attach object；
10. lift/retreat；
11. plan to place pose；
12. release、detach、retreat；
13. verify final object state。

## Mandatory Fault Injection
至少故意制造并诊断以下5类：
1. **TF direction / calibration error**：导致grasp pose偏移；
2. **IK unreachable**：目标超workspace或orientation过严；
3. **Collision planning failure**：table/self-collision/scene object；
4. **Gripper/grasp failure**：close command已发但object未稳定抓住；
5. **Stale object pose**：移动object后仍使用旧pose。

建议增加：time parameterization/velocity limit异常、attached object遗漏、Planning Scene未刷新。

## Logging
至少记录：
- object pose/frame/timestamp；
- selected grasp/pre-grasp；
- RobotState/joint state；
- IK result/failure reason；
- collision object/Planning Scene version；
- planned path + timed trajectory；
- joint command/feedback；
- gripper command/state/current/force（如有）；
- attach/detach state；
- success/failure/retry reason。

## Acceptance Criteria
- 正常case完整Pick-and-Place成功；
- 能清楚解释 `IK Success ≠ Planning Success ≠ Execution Success ≠ Grasp Success`；
- 五类mandatory fault均能从log/scene/state中定位；
- 每次failure都有明确layer：Perception/TF/IK/Collision/Planning/Trajectory/Controller/Gripper/Contact/Verification；
- 生成一份简短实验报告：版本、配置、场景、结果、failure evidence、fix、regression。
