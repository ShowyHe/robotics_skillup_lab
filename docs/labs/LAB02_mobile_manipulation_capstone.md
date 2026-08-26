# LAB02 — Mobile Manipulation Capstone

## Goal
完成M18要求的跨模块闭环：`Task Command → Target Perception/Grounding → Navigation Goal → Nav2 → Base Pose Verification → Re-perception → Reachability → MoveIt Pick → Attach → Navigate While Carrying → Place → Detach → Task Verification / Recovery`。

优先Simulation first，再按硬件条件迁移Real Robot。该LAB同时作为M21最终Capstone的首选承载体。

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

语言/VLM/VLA可作为高层输入，但不是本LAB核心闭环的强制前提；VLA动作接口实践由LAB03专门覆盖。

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
5. **Attached object遗漏**：抓到长物体后transport collision risk；
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

---

# M21 Research Extension — Required for Final Capstone Defense
若LAB02作为最终Capstone，必须在系统闭环之外增加研究方法层：

## 1. Problem Statement
明确System / Scenario / Observed Problem / Expected Behavior / Current Limitation / Constraints。

## 2. Baseline
选择公平baseline，例如：
- 无base-reposition策略；
- 只一次perception、不re-perceive；
- 旧task-state恢复策略；
- 当前production/官方方案。

不得故意选择明显较差baseline。

## 3. Falsifiable Hypothesis
例如：
> “Navigation后强制re-perception + reachability-aware base placement将降低IK/planning failure和人工干预，而不显著增加总cycle time。”

## 4. Scenario Matrix
至少覆盖：nominal / boundary / failure / recovery；固定或记录start、goal、object pose、scene、robot/software/model/config/calibration版本。

## 5. Repeated Trials
同一关键scenario需要多次重复；不能用单次成功视频作为主要证据。

## 6. Ablation
至少拆一个关键设计，例如：
- full system；
- no re-perception；
- no reachability-aware base placement；
- no long-horizon recovery state。

## 7. Acceptance Criteria
实验前定义，例如task success、collision/intervention、grasp success、cycle time、retry等硬门槛/目标。

## 8. Failure Cases
必须主动展示失败case并给出root cause / evidence / limitation，不能只展示成功录像。

## 9. Reproducibility Package
至少保存：commit SHA、config、model、calibration、scenario、bags/logs、commands、metric script、experiment ID。

## 10. Final Defense
最终需要能从：
`Problem → Baseline → Hypothesis → Design → Implementation → Experiment → Ablation → Evidence → Failure → Limitation → Conclusion`
完整答辩。

---

## Acceptance Criteria
- 正常任务完整闭环可稳定重复；
- 能证明 `Navigation Success ≠ Manipulation Ready`；
- mandatory faults均能用Input/Output/Frame/Timestamp/Validity/Failure Reason定位；
- 至少一次task interruption后能基于long-horizon state安全恢复或明确进入safe failure；
- 报告包含版本、配置、场景、故障注入、指标、失败证据、修复与回归；
- 若作为M21最终Capstone，必须额外完成Research Extension中的Baseline/Hypothesis/Repeated Trials/Ablation/Reproducibility/Defense。
