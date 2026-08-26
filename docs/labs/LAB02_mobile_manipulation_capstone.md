# LAB02 — Mobile Manipulation Capstone

## Goal
完成M18要求的跨模块闭环：`Task Command → Target Perception/Grounding → Navigation Goal → Nav2 → Base Pose Verification → Re-perception → Reachability → MoveIt Pick → Attach → Navigate While Carrying → Place → Detach → Task Verification / Recovery`。

优先先在Simulation完成，再按硬件条件迁移Real Robot。

## Minimum Architecture
- Task/Skill state machine（BT/FSM均可）；
- target identity + long-horizon task state；
- navigation goal generator；
- Nav2；
- base pose/freshness verification；
- re-perception / object pose update；
- reachability + base reposition；
- MoveIt2 pick/place；
- gripper verification；
- attach/detach/carry state；
- safety stop / retry / recovery；
- end-to-end logging。

## Required Task
至少完成一种：
> “找到目标物 → 导航到可操作区域 → 重新观察 → 抓取 → 搬运到第二位置 → 放置 → 验证。”

语言/VLM/VLA可作为高层输入，但不是本LAB的强制前提；核心是Mobile Manipulation闭环。

## Required State Contract
任务运行时必须显式保存：
- current skill/state；
- target ID；
- last valid target pose + frame + timestamp；
- current base pose validity；
- object attached state；
- carry mode；
- retry count；
- latest failure reason；
- recovery/resume point。

## Mandatory Fault Injection
至少制造并诊断6类：
1. **Base pose/yaw error**：导航到达但grasp geometry失效；
2. **Stale object pose**：导航后不re-perceive直接抓；
3. **IK unreachable / bad base placement**：base停太远或姿态差；
4. **Planning Scene error**：漏桌面/障碍导致错误规划；
5. **Attached object遗漏**：抓到长物体后transport时发生collision risk；
6. **Grasp verification failure**：close command成功但object未真正抓住。

建议增加：navigation failure、human/object movement、controller timeout、task interruption/recovery。

## Required Recovery
至少实现或明确验证：
- target不可见→search/re-perceive；
- target可见但不可达→reposition；
- grasp failed→refresh pose→next candidate→retry budget；
- navigation interrupted→重新验证base/world/task state；
- object lost while carrying→safe stop + recovery state；
- task restart不得盲目复用旧trajectory/action chunk。

## Metrics
至少记录：
- overall task success；
- navigation success；
- grasp success；
- placement success；
- base reposition count；
- retry count；
- intervention count；
- collision/near-collision；
- total cycle time；
- perception→action freshness/latency（如可测）。

## Acceptance Criteria
- 正常任务完整闭环至少稳定重复多次；
- 能证明 `Navigation Success ≠ Manipulation Ready`；
- mandatory faults都能用Input/Output/Frame/Timestamp/Validity/Failure Reason定位；
- 至少一次task interruption后能基于long-horizon state安全恢复或明确进入safe failure；
- 报告包含版本、配置、场景、故障注入、指标、失败证据、修复与回归。
