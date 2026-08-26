# M11 — Planning & Navigation

## Module Goal
建立从图搜索、碰撞表示、非完整约束规划到 HPA / Nav2 / BT / Path Switching 的完整规划主线，并能从真实机器人现象反推 Planner / Costmap / BT / Controller 的责任边界。

主线：`World/Map → Discrete Representation → Collision/Feasibility → Search → Global Path → Validation/Switch/Replan → Local Controller → Motion`。

本模块共 8 个理论 Day（Day63–Day70）。源码顺序固定：`真实问题 → 公司实现 → 反推基础 → 数学/算法 → Nav2官方 → 必要最小实现 → 回真实系统`。

---

# Day63 — Graph / BFS / Dijkstra / Shortest Path
1. 今日目标：把地图规划抽象为图 `G=(V,E)` 和 shortest-path problem。
2. 前置：图、queue、priority queue、基本复杂度。
3. 必须教学：vertex/edge；path/path cost；BFS；Dijkstra；`g(n)`；priority queue；relaxation；parent reconstruction；open/visited；nonnegative edge cost；复杂度直觉；grid graph。
4. 深度：Dijkstra L4；手推小图 L4。
5. 工程连接：global planner、road graph、HPA abstract graph。
6. 不展开：negative edge/Bellman-Ford。
7. 考核：手算 Dijkstra，解释为何第一次弹出最小 g 节点可确定最短距离的条件。
8. 毕业考点：Graph、cost、relaxation、Dijkstra。

# Day64 — A* / Heuristic / Optimality / Search Efficiency
1. 今日目标：理解 `f(n)=g(n)+h(n)` 怎样用启发式减少搜索。
2. 前置：Day63。
3. 必须教学：g/h/f；admissible `h(n)≤h*(n)`；consistency；Euclidean/Manhattan；heuristic=0→Dijkstra；weak/aggressive heuristic；Weighted A*概念；open/closed；grid connectivity；corner cutting；resolution；optimality vs efficiency。
4. 深度：A* L4；admissibility/consistency L3-L4。
5. 工程连接：Nav2 global planning、JPS/HPA局部attach。
6. 不展开：D*/D* Lite；仅在未来真实需求出现时补。
7. 考核：手推 A* 并比较不同 heuristic。
8. 毕业考点：A*、heuristic、optimality。

# Day65 — Occupancy / Costmap / Footprint / Inflation / Collision
1. 今日目标：理解“地图有路”与“机器人可安全通过”不是同一件事。
2. 前置：M03/M07 world representation。
3. 必须教学：occupancy vs costmap；free/occupied/unknown，unknown≠free；robot≠point；footprint；inscribed/circumscribed radius；collision checking；inflation；distance field；layer composition；static/obstacle/inflation/keepout；narrow passage；resolution与几何误差；safety margin。
4. 深度：Costmap/footprint L4-L5。
5. 工程连接：Nav2 global/local costmap、窄道、人行障碍。
6. 不展开：具体Costmap2D源码实现细节。
7. 考核：给通道宽度、footprint、inflation判断可行性。
8. 毕业考点：Footprint、collision、inflation、unknown语义。

# Day66 — Hybrid A* / Motion Primitive / Nonholonomic Planning
1. 今日目标：理解仅搜索 `(x,y)` 为什么不足以规划受转向约束的机器人。
2. 前置：A* + M12移动运动学的直觉；正式运动学细节在 M12深化。
3. 必须教学：state `(x,y,θ)`；nonholonomic constraint；continuous propagation + discrete search；motion primitive；simple kinematic rollout；turning radius；forward/reverse；reverse/steering penalty；analytic expansion；Dubins/Reeds-Shepp概念；primitive整段collision check；state lattice概念。
4. 深度：Hybrid A* L4。
5. 工程连接：车辆/底盘可执行路径、Nav2 Smac类规划思想。
6. 不展开：完整Dubins/Reeds-Shepp推导。
7. 考核：解释为何2D A*路径可能物理不可执行。
8. 毕业考点：Nonholonomic、primitive、collision。

# Day67 — RRT / RRT* / Sampling-based Planning / OMPL
1. 今日目标：理解高维空间为什么常使用sampling-based planning。
2. 前置：C-space、collision。
3. 必须教学：sample→nearest→steer→collision→tree；goal bias；step size；probabilistic completeness；RRT vs RRT*；near/rewire；asymptotic optimality概念；sampling狭窄通道问题；OMPL角色；与Manipulation高维规划的桥接。
4. 深度：RRT L4；RRT* L3。
5. 工程连接：MoveIt/OMPL前置。
6. 不展开：高级sampling planner全集。
7. 考核：画一次RRT扩展并解释rewire。
8. 毕业考点：Sampling、RRT、RRT*、OMPL。

# Day68 — HPA / Hierarchical Planning / Dynamic Edge
1. 今日目标：理解大图规划为什么需要抽象层，以及动态失效如何回写抽象图。
2. 前置：Day63–67。
3. 必须教学：region/cluster；portal/gateway；abstract graph；high-level route；local refinement；precomputation；abstraction error；start/goal attach；K-nearest/LOS/JPS类attach思想；dynamic edge invalidation；edge block；TTL；local repair；fallback必须重新验证feasibility，禁止无条件Euclidean fallback。
4. 深度：HPA L4-L5；dynamic edge L5。
5. 工程连接：大地图预热、封边、PathSwitch前置。
6. 不展开：公司具体实现语义必须以实际源码为准。
7. 考核：动态障碍使抽象edge失效后如何传播和恢复。
8. 毕业考点：HPA、attach、dynamic edge、TTL。

# Day69 — Nav2 / BT / Replanning / Path Switching
1. 今日目标：建立 Nav2 Planner / Controller / BT / Recovery 的职责边界。
2. 前置：Day63–68。
3. 必须教学：Planner Server；Controller Server；BT Navigator；global path validity；periodic/event replanning；path switching；switch benefit/cost；hysteresis；persistence；cooldown；recovery；旧path保留条件；new path validation；planner failure vs controller failure；PathSwitchGuard类工程概念。
4. 深度：Nav2 boundary L5；switch reasoning L5。
5. 工程连接：keep/switch、行人阻塞、动态路径切换。
6. 不展开：不根据未读取公司源码臆测具体阈值语义。
7. 考核：给两条path与障碍变化判断 keep/switch/replan。
8. 毕业考点：BT、replan、path switching、responsibility boundary。

# Day70 — Dynamic Navigation / Path Quality / Planning Owner
1. 今日目标：把动态障碍、路径质量与真实系统证据链合起来。
2. 前置：Day63–69。
3. 必须教学：dynamic obstacle的时间属性；global vs local response；TTL/freshness；clearance；curvature；heading change；narrowness；reverse；costmap cost；smoothing与collision recheck；path vs trajectory；local controller限制；failure taxonomy；evidence collection；planner source mapping。
4. 深度：Owner attribution L5。
5. 工程连接：提前绕、钻窄路、绕后回拉、路径互搏。
6. 不展开：MPC/MPPI控制细节留M13。
7. 考核：从现象→costmap/path/switch/controller证据链定位责任。
8. 毕业考点：Dynamic planning、path quality、Owner debug。

---

# M11 Graduation Exam
统一权重：**30%核心基础 / 50%综合系统场景 / 20% Source·Formula·Design**。

## 30% 核心基础
硬门槛：Dijkstra/A*、heuristic、costmap/footprint/collision、Hybrid A*约束、HPA/dynamic edge、Nav2责任边界、path switching。

## 50% 综合系统场景
至少覆盖：
1. 小图 A* 手算 + heuristic解释；
2. 窄通道中 footprint/inflation/corner-cutting 的可行性分析；
3. 行人动态阻塞导致 keep/switch/replan 的完整链；
4. 大图HPA edge失效、TTL、fallback安全性；
5. Planner path正常但机器人行为异常时区分Planner/Controller责任。

## 20% Source / Formula / Design
必须能在公司planner/HPA与Nav2官方实现中定位：map/costmap输入、search入口、collision validity、path output、replan、BT、switch/guard相关调用链；公司仓库内容以真实读取为准，不臆测。

## 通过标准
总分≥85%；A*、collision/footprint、HPA动态edge、Nav2责任边界不得有基础错误；必须能从真实机器人证据判断“规划错”还是“控制执行错”。

## Day63–Day70 索引
```text
Day63 Graph / BFS / Dijkstra
Day64 A* / Heuristic
Day65 Occupancy / Costmap / Footprint / Inflation
Day66 Hybrid A* / Motion Primitive
Day67 RRT / RRT* / OMPL
Day68 HPA / Hierarchical Planning / Dynamic Edge
Day69 Nav2 / BT / Replanning / Path Switching
Day70 Dynamic Navigation / Path Quality / Planning Owner
```
