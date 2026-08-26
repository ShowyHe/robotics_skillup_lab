# M11 — Planning & Navigation

## Module Goal
建立从图搜索、碰撞表示、运动约束规划到 HPA / Nav2 / BT / Path Switching 的完整规划主线，并能从真实机器人现象反推 Planner / Costmap / BT / Controller 的责任边界。

主线：`World/Map → Representation → Collision/Feasibility → Search → Global Path → Validate/Switch/Replan → Local Controller → Motion`。

本模块共 8 个理论 Day（Day63–Day70）。源码顺序固定：`真实问题 → 公司实现 → 反推基础 → 数学/算法 → Nav2官方 → 必要最小实现 → 回真实系统`。

---

# Day63 — Graph / BFS / Dijkstra
1. 今日目标：把地图规划抽象为graph shortest-path problem。
2. 前置：queue、priority queue、基本复杂度。
3. 必须教学：`G=(V,E)`；path/path cost；BFS；Dijkstra；`g(n)`；priority queue；relaxation；parent reconstruction；open/visited；nonnegative edge cost；grid graph；复杂度直觉。
4. 深度：Dijkstra L4。
5. 工程连接：global planner、road graph、HPA abstract graph。
6. 不展开：negative edge/Bellman-Ford。
7. 考核：手推Dijkstra并解释relaxation。
8. 毕业考点：Graph、Cost、Dijkstra。

# Day64 — A* / Heuristic / Optimality
1. 今日目标：理解 `f=g+h` 如何减少搜索。
2. 前置：Day63。
3. 必须教学：g/h/f；admissible；consistency；Euclidean/Manhattan；h=0→Dijkstra；heuristic strength；Weighted A*概念；open/closed；grid connectivity；corner cutting；resolution；optimality vs efficiency。
4. 深度：A* L4；admissibility/consistency L3-L4。
5. 工程连接：Nav2 global planning、JPS/HPA local attach。
6. 不展开：D*/D* Lite；未来出现动态增量重规划需求再补。
7. 考核：手推A*并比较heuristic。
8. 毕业考点：A*、Heuristic、Optimality。

# Day65 — Occupancy / Costmap / Footprint / Inflation
1. 今日目标：理解“地图有空隙”与“机器人可安全通过”不是一回事。
2. 前置：M03/M07 world representation。
3. 必须教学：occupancy vs costmap；free/occupied/unknown且unknown≠free；robot≠point；footprint；inscribed/circumscribed radius；collision checking；inflation；distance field概念；static/obstacle/inflation/keepout；narrow passage；resolution误差；safety margin。
4. 深度：Costmap/Footprint L4-L5。
5. 工程连接：Nav2 costmap、窄道、人行障碍。
6. 不展开：Costmap2D内部源码。
7. 考核：给通道宽度/footprint/inflation判断可行性。
8. 毕业考点：Footprint、Collision、Inflation、Unknown语义。

# Day66 — Hybrid A* / Motion Primitive / Nonholonomic Planning
1. 今日目标：理解只在 `(x,y)` 搜索为什么不足以保证底盘可执行。
2. 前置：Day64–65；本日内先补 `state=(x,y,θ)`、turning radius、holonomic/nonholonomic 的最小运动约束直觉，M12 Day76再正式系统化移动机器人运动学。
3. 必须教学：state `(x,y,θ)`；nonholonomic intuition；continuous propagation + discrete search；motion primitive；kinematic rollout；turning radius；forward/reverse；reverse/steering penalty；analytic expansion；Dubins/Reeds-Shepp概念；primitive整段collision check；state lattice概念。
4. 深度：Hybrid A* L4。
5. 工程连接：车辆/底盘可执行路径、Nav2 Smac思想。
6. 不展开：完整Dubins/Reeds-Shepp推导、正式mobile kinematics留M12。
7. 考核：解释2D A*路径为什么可能物理不可执行。
8. 毕业考点：Nonholonomic、Primitive、Collision。

# Day67 — RRT / RRT* / Sampling / OMPL
1. 今日目标：理解高维configuration space为什么常使用sampling-based planning。
2. 前置：Day65 collision；本日内先建立 C-space = “robot configuration 的搜索空间”及state validity概念，M12/M14再深化机械臂configuration。
3. 必须教学：C-space/state validity；sample→nearest→steer→collision→tree；goal bias；step size；probabilistic completeness；RRT vs RRT*；near/rewire；asymptotic optimality概念；narrow passage sampling难题；OMPL角色；与Manipulation高维规划桥接。
4. 深度：RRT L4；RRT* L3。
5. 工程连接：MoveIt/OMPL前置。
6. 不展开：sampling planner全集。
7. 考核：画一次RRT扩展并解释rewire。
8. 毕业考点：C-space、RRT、RRT*、OMPL。

# Day68 — HPA / Hierarchical Planning / Dynamic Edge
1. 今日目标：理解大图抽象、local refinement与动态edge失效。
2. 前置：Day63–67。
3. 必须教学：region/cluster；portal/gateway；abstract graph；high-level route；local refinement；precomputation；abstraction error；start/goal attach；K-nearest/LOS/JPS类attach思想；dynamic edge invalidation；edge block；TTL；local repair；fallback必须重新验证feasibility，禁止无条件Euclidean fallback。
4. 深度：HPA L4-L5；Dynamic Edge L5。
5. 工程连接：大地图预热、封边、PathSwitch前置。
6. 不展开：公司具体实现语义必须以实际源码为准。
7. 考核：动态障碍使abstract edge失效后如何传播与恢复。
8. 毕业考点：HPA、Attach、Dynamic Edge、TTL。

# Day69 — Nav2 / BT / Replanning / Path Switching
1. 今日目标：建立 Planner / Controller / BT / Recovery / Switch 的职责边界。
2. 前置：Day63–68。
3. 必须教学：Planner Server；Controller Server；BT Navigator；global path validity；periodic/event replanning；path switching；switch benefit/cost；hysteresis；persistence；cooldown；recovery；旧path保留条件；new path validation；planner failure vs controller failure；PathSwitchGuard类工程概念。
4. 深度：Nav2 boundary/Switch reasoning L5。
5. 工程连接：keep/switch、行人阻塞、动态路径切换。
6. 不展开：不根据未读取公司源码臆测阈值语义。
7. 考核：给两条path与障碍变化判断keep/switch/replan。
8. 毕业考点：BT、Replan、Path Switching、Responsibility Boundary。

# Day70 — Dynamic Navigation / Path Quality / Planning Owner
1. 今日目标：把动态障碍、路径质量和系统证据链合起来。
2. 前置：Day63–69。
3. 必须教学：dynamic obstacle time property；global vs local response；TTL/freshness；clearance；curvature；heading change；narrowness；reverse；costmap cost；smoothing后collision recheck；path vs trajectory；local controller限制；failure taxonomy；evidence collection；planner source mapping。
4. 深度：Owner Attribution L5。
5. 工程连接：提前绕、钻窄路、绕后回拉、路径互搏。
6. 不展开：MPPI控制细节留M13。
7. 考核：从现象→costmap/path/switch/controller证据链定位责任。
8. 毕业考点：Dynamic Planning、Path Quality、Owner Debug。

---

# M11 Graduation Exam
统一权重：**30%核心基础 / 50%综合系统场景 / 20% Source·Formula·Design**。

## 30% 核心基础
硬门槛：Dijkstra/A*、heuristic、costmap/footprint/collision、Hybrid A*约束、C-space、HPA/dynamic edge、Nav2责任边界、path switching。

## 50% 综合系统场景
至少覆盖：A*手算；窄通道可行性；行人动态阻塞keep/switch/replan；HPA edge失效/TTL/fallback；Planner path正常但robot异常时区分Planner/Controller。

## 20% Source / Formula / Design
在公司planner/HPA与Nav2官方实现中定位map/costmap输入、search、collision validity、path output、replan、BT、switch/guard调用链；公司源码内容以真实读取为准。

## 通过标准
总分≥85%；A*、collision/footprint、HPA dynamic edge、Nav2 boundary不得有基础错误；必须能判断“规划错”还是“控制执行错”。
