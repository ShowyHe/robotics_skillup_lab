# M20 — Safety / Reliability / Owner

## Module Goal
把分散在感知、定位、规划、控制、VLA和部署中的安全概念收束为 `Hazard → Risk → Safety Requirement → Interlock/Monitoring → Degraded/Safe State → Recovery → Incident Evidence → Regression → Release Gate`。

必须始终区分：`Safety ≠ Reliability ≠ Correctness`。

本模块共 4 个理论 Day（Day123–Day126）。

---

# Day123 — Hazard / Risk / Safety Requirement / Fail-safe
1. 今日目标：从hazard出发，而不是从“加急停”倒推安全。
2. 前置：M00/M03/M13/M19。
3. 必须教学：hazard；harm；hazardous situation；risk与severity/exposure/probability；Safety Requirement；Safety Constraint；fail-safe；safe state不一定断电；fail-safe vs fail-operational；safety by design；safety margin；stopping distance包含detection/decision/command/braking delay；speed-distance-force-workspace safety envelope；Task correctness vs physical safety；learned/VLA system仍需hard safety；human presence场景；保护措施层级。
4. 深度：Hazard/Risk/Fail-safe L4-L5。
5. 工程连接：定位失效、行人靠近、VLA异常action。
6. 不展开：具体安全标准认证条文/SIL/PL计算。
7. 考核：从hazard写成可验证requirement与safe response。
8. 毕业考点：Hazard、Risk、Safety Requirement、Fail-safe、Safe State。

# Day124 — Fault / Failure / Watchdog / Timeout / Interlock / E-stop
1. 今日目标：系统必须主动判断“数据/模块是否仍可信”。
2. 前置：Day123 + M01/M03/M09/M19。
3. 必须教学：fault/error/failure；hard vs soft fault；liveness/heartbeat；heartbeat limitation；watchdog；timeout/freshness；frequency≠freshness；stale-but-alive；range/rate/plausibility checks；cross-sensor consistency；innovation/residual health；Safety Interlock/permission gate；latched fault；auto-recovery risk；command watchdog；protective stop/software stop/E-stop区别；E-stop高优先级和明确reset语义；**human takeover**：operator intervention channel、manual control authority、takeover后旧automation state失效、handover/handback必须重新验证pose/world/task/controller state。
4. 深度：Watchdog/interlock/recovery/human takeover L5。
5. 工程连接：GPS/LIO freshness、cmd timeout、手把急停、人工介入。
6. 不展开：safety PLC/硬件冗余电路。
7. 考核：alive但500ms stale pose是否可用；E-stop/human takeover后为何不能自动resume old trajectory。
8. 毕业考点：Fault/Failure、Watchdog、Interlock、E-stop、Human Takeover。

# Day125 — Reliability / Redundancy / Degraded Mode / FMEA / Fault Tree
1. 今日目标：判断哪些fault可降级运行、哪些必须停止，并系统分析failure propagation。
2. 前置：Day123–124。
3. 必须教学：reliability；availability概念；redundancy；redundancy≠两个相同sensor；independent/common-cause failure；voting/consistency概念；graceful degradation；Normal→Degraded→Safe Stop；degradation budget：TTL/distance/uncertainty；fault containment；single point of failure；FMEA：function→failure mode→effect→detect→mitigation；FTA：top event→AND/OR causes；common-cause如CPU overload同时拖慢perception/control/watchdog；diverse safety channel；monitoring vs control separation；recovery重新validate；定位RTK/LIO/GNSS degraded chain设计。
4. 深度：Degraded/FMEA/FTA/common cause L4-L5。
5. 工程连接：RTK FIX→LIO-only reduced speed→stop。
6. 不展开：MTBF定量可靠性、硬件投票电路。
7. 考核：给Mobile Robot做FMEA和collision top-event fault tree。
8. 毕业考点：Redundancy、Common Cause、Degraded Mode、FMEA/FTA。

# Day126 — Incident / Safety Case / Fault Injection / Release Gate / Owner
1. 今日目标：危险行为发生后不仅“改参数”，而是证明root cause、fix与长期防复发。
2. 前置：Day123–125 + M19 regression。
3. 必须教学：incident/near miss；symptom vs root cause；root cause≠first bug；timeline `sensor→estimate→planner→controller→command→feedback→intervention`；evidence hierarchy：bag/log/timestamp/feedback/video/config/commit；counterfactual safety question；defense in depth；Safety Case `Claim→Argument→Evidence`；requirement必须可验证；fault injection：kill/freeze/stale/dropout；incident→minimal reproducer→regression；hard safety gate不能被平均指标抵消；unknown failure需containment与增强logging；Owner responsibility boundary；cross-module ownership；Task Intelligence vs Safety Authority；最终否决权；**version risk**：code/model/config/calibration/runtime升级可能破坏已验证安全case，变更后必须重跑对应safety regression；release gate；incident report固定结构。
4. 深度：Root cause/fault injection/release/safety case L5。
5. 工程连接：已看到行人但仍高速靠近、最后人工急停的完整链。
6. 不展开：正式认证/法规法律结论。
7. 考核：从near miss构造timeline/fault tree/safety requirement/fault injection/regression/release decision。
8. 毕业考点：Incident Analysis、Safety Case、Fault Injection、Safety Authority、Version Risk。

---

# M20 Graduation Exam
统一权重：**30%核心基础 / 50%综合系统场景 / 20% Source·Formula·Design**。

## 30% 核心基础
硬门槛：Hazard/Risk/Safety Requirement/Fail-safe；Fault/Failure；Watchdog/Freshness/Plausibility；Interlock/E-stop/Human Takeover；Redundancy/Common Cause/Degraded Mode；FMEA/FTA；Defense in Depth/Fault Injection；Release Gate/Version Risk。

## 50% 综合系统场景
至少覆盖：
1. LIO 10Hz但age=500ms：alive vs fresh、degraded/stop；
2. GNSS/RTK loss设计Normal→Degraded→Stop和recovery；
3. VLA异常EEF delta的range/rate/workspace/collision rejection；
4. E-stop或human takeover后重新接管automation；
5. 行人near miss的Perception/World/Planner/Control/Safety fault tree；
6. CPU overload造成多模块common cause；
7. 新model/config版本是否破坏既有Safety Case。

## 20% Source / Formula / Design
设计Safety State Machine（INIT/NORMAL/DEGRADED/PROTECTIVE_STOP/E_STOP/RECOVERY/FAULT）、LIO/Camera/Controller/cmd watchdog、Mobile Manipulator FMEA、可验证Safety Requirement与fault-injection test。

## 通过标准
总分≥85%；必须明确 `Safety≠Reliability`、`Heartbeat≠Correctness`、`Frequency≠Freshness`、`Fault disappears≠Auto Resume`；VLA/AI不得成为唯一Safety Authority；每个危险incident必须能进入regression。

## Day123–Day126 索引
```text
Day123 Hazard / Risk / Safety Requirement / Fail-safe
Day124 Fault / Failure / Watchdog / Interlock / E-stop / Human Takeover
Day125 Reliability / Redundancy / Degraded Mode / FMEA / Fault Tree
Day126 Incident / Safety Case / Fault Injection / Release Gate / Owner
```
