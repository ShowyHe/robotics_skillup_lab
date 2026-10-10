# M11 — Planning & Navigation

## Module Goal
建立从图搜索、configuration-space碰撞表示、运动约束规划到 HPA / Nav2 / BT / Path Switching 的完整规划主线，并能从真实机器人现象反推 Planner / Costmap / BT / Controller 的责任边界。

主线：`World/Map → Representation → Collision/Feasibility → Search → Global Path → Validate/Switch/Replan → Local Controller → Motion`。

本模块共 8 个理论 Day（Day63–Day70）。当前将 Navigation 作为主攻强项，目标不是“会用 Planner”，而是达到 **Graduate-level Planning Theory + Production Navigation Owner，整体 L4→L5**。源码顺序固定：`真实问题 → 公司实现 → 反推基础 → 数学/算法 → Nav2官方 → 必要最小实现 → 回真实系统`。

## 主要教材
- **Modern Robotics Chapter 2 / Chapter 10**：仅作为 Configuration Space 与 Motion Planning 的辅助理论参考。
- M11主教材仍然是：真实Navigation问题、公司Planner/HPA、Nav2官方资料，以及A*/Hybrid A*/RRT等规划算法本体。
- Modern Robotics不会替代Costmap、HPA、Nav2/BT、动态路径切换等本课程工程主线。

## Foundation Coverage Contract（系统基础覆盖合同）

M11 不因已有 Navigation 工程经验而跳过尚未系统学习过的基础。每个 Day 必须在同一天完成：

```text
Foundation
→ Core Theory
→ Advanced / Owner
```

Day Goal 与完整覆盖优先：每个 Day 最多 **35 个编号教学知识单元**。允许在同一单元内整合相关定义、公式、最小例题与边界；不得删除 Foundation / Core / Owner 或 Hard Gate，不拆 Day。

各 Day 的 Foundation 最低覆盖：

- **Day63**：Graph / Vertex / Edge；Directed / Undirected；Weighted / Unweighted；Neighbor / Degree；Walk / Path / Cycle；Reachability / Connectivity；Adjacency List / Matrix；Queue / FIFO；BFS；Visited / Parent。
- **Day64**：Search State；Search Tree vs Graph Search；Frontier / Open；Closed；Goal Test；Path Cost；Uninformed vs Informed Search；Heuristic；Manhattan / Euclidean / Octile distance。
- **Day65**：Occupancy Grid；resolution / origin；cell↔world coordinate；free / occupied / unknown；robot geometry；circle / polygon footprint；point robot vs finite-size robot；obstacle distance。
- **Day66**：configuration / state；heading；continuous vs discrete state；holonomic / nonholonomic；curvature；turning radius；kinematic model；motion primitive。
- **Day67**：Workspace vs Configuration Space；state validity；edge validity；deterministic vs sampling search；random sampling；nearest neighbor；steer / local connection；collision checking。
- **Day68**：abstraction；hierarchy；cluster / region；portal / gateway；abstract graph；precomputation；graph reuse；dynamic edge。
- **Day69**：ROS2 Action 最小语义；Planner Server；Controller Server；BT Navigator；Behavior Tree node；Sequence / Fallback；Recovery；Lifecycle；path / goal / feedback。
- **Day70**：Path；Trajectory；Reference；velocity；acceleration；curvature；clearance；smoothing；global vs local response。

已在本课程正式学习并 PASS 的知识才允许按 Knowledge Reuse Rule 简短恢复；“工作中用过”只能加快速度，不能删除基础覆盖。

---

# Day63 — Graph / BFS / Dijkstra
1. 今日目标：从零建立 Graph Search（图搜索）基础，并把地图规划严格抽象为 shortest-path problem（最短路径问题）。
2. 前置：基本编程与数组/容器直觉；queue、priority queue、graph representation、复杂度直觉均在本 Day 内系统补齐，不作为默认已掌握前置。
3. 必须教学：Graph `G=(V,E)`；vertex / edge；directed / undirected；weighted / unweighted；neighbor / degree；walk / path / cycle；reachability / connectivity；adjacency list / adjacency matrix；queue / FIFO；BFS；visited / parent；BFS shortest-path 的成立条件；path/path cost；weighted shortest path；Dijkstra；`g(n)`；priority queue / min-heap；relaxation；stale queue entry；parent reconstruction；open/visited；nonnegative edge cost；shortest-path optimality intuition；grid graph / road graph；复杂度直觉。
4. 深度：Dijkstra L4。
5. 工程连接：global planner、road graph、HPA abstract graph。
6. 不展开：negative edge/Bellman-Ford。
7. 考核：能判断 graph 类型与表示方式；手推 BFS / Dijkstra；解释 relaxation、priority queue、nonnegative edge 与 optimality。
8. 毕业考点：Graph、Cost、Dijkstra。

# Day64 — A* / Heuristic / Optimality / Anytime Search
1. 今日目标：理解 `f=g+h` 如何减少搜索，并理解 real-time robot 为什么有时需要“先给可行次优解，再逐步改进”。
2. 前置：Day63。
3. 必须教学：g/h/f；admissible；consistency；Euclidean/Manhattan；h=0→Dijkstra；heuristic strength；Weighted A*；open/closed；grid connectivity；corner cutting；resolution；optimality vs efficiency；suboptimality bound 直觉；Anytime Search；ARA* 核心思想：inflated heuristic / epsilon、先快速求解、时间允许时降低 epsilon 并改进解。
4. 深度：A* L4-L5；admissibility/consistency L3-L4；Anytime Search L3。
5. 工程连接：Nav2 global planning、JPS/HPA local attach、有限规划周期中的 path quality vs planning latency。
6. 不展开：ARA*完整证明；Incremental Search / D* Lite 正式放 Day68。
7. 考核：手推A*并比较heuristic；解释 Anytime planner 为什么不等于“随便返回一条差路径”。
8. 毕业考点：A*、Heuristic、Optimality、Weighted / Anytime trade-off。

# Day65 — Occupancy / Costmap / Footprint / Inflation
1. 今日目标：在复用 M07 world representation 的基础上，理解“环境哪里有障碍”如何进一步变成“考虑机器人几何后，哪些 configuration 可安全通过”。
2. 前置：M07 Day38 world representation；M03 robot geometry基础。
3. 复用回顾（已通过则简短恢复）：occupancy vs costmap；free/occupied/unknown；**unknown≠free**；world representation 到 costmap 的基本边界。
4. 新增教学：robot≠point；footprint；inscribed/circumscribed radius；collision checking；inflation；distance field概念；static/obstacle/inflation/keepout layers；narrow passage；resolution误差；safety margin。
5. 知识连接：从 Day38 的“环境单元是什么状态”推进到 `Occupancy / Cost → Robot Footprint → Collision / Inflation → Configuration Feasibility`；重点理解同一张地图对不同尺寸 robot 的可通行区域不同。
6. 深度：Costmap/Footprint/Collision L4-L5。
7. 工程连接：Nav2 costmap、窄道、人行障碍、近场 254 collision。
8. 不展开：Costmap2D内部源码；不重新完整教学 Occupancy / Unknown 的基础语义。
9. 考核：给通道宽度/footprint/inflation判断可行性；解释环境 free space 为什么不等于 robot configuration free space。
10. 毕业考点：Footprint、Collision、Inflation、Unknown语义。

# Day66 — Hybrid A* / Motion Primitive / Nonholonomic Planning
1. 今日目标：理解只在 `(x,y)` 搜索为什么不足以保证底盘可执行。
2. 前置：Day64–65；本日内先补 `state=(x,y,θ)`、turning radius、holonomic/nonholonomic 的最小运动约束直觉，M12 Day76再正式系统化移动机器人运动学。
3. 必须教学：state `(x,y,θ)`；nonholonomic intuition；continuous propagation + discrete search；motion primitive；kinematic rollout；turning radius；forward/reverse；reverse/steering penalty；analytic expansion；Dubins/Reeds-Shepp概念；primitive整段collision check；state lattice概念；**Kinodynamic Planning boundary**：当仅有 `(x,y,θ)` 不能表达 velocity / acceleration / dynamic feasibility 时，为什么需要把 `v,ω,a` 等进入 state / propagation。
4. 深度：Hybrid A* L4-L5；Kinodynamic Planning boundary L2-L3。
5. 工程连接：车辆/底盘可执行路径、Nav2 Smac思想。
6. 不展开：完整Dubins/Reeds-Shepp推导、正式mobile kinematics留M12。
7. 考核：解释2D A*路径为什么可能物理不可执行。
8. 毕业考点：Nonholonomic、Primitive、Collision。

# Day67 — Configuration Space / RRT / RRT* / Sampling / OMPL
1. 今日目标：理解高维configuration space为什么常使用sampling-based planning，以及robot geometry怎样转化为configuration validity问题。
2. 前置：Day65 collision；Modern Robotics Ch2/10可辅助阅读，M12/M14后续深化机械臂configuration。
3. 必须教学：Configuration `q`与C-space `𝒞`；`𝒞_obs`表示导致robot与障碍碰撞的configurations；`𝒞_free=𝒞\𝒞_obs`；workspace obstacle不等于C-space obstacle；state validity / edge validity；sample→nearest→steer→collision→tree；goal bias；step size；probabilistic completeness；RRT vs RRT*；near/rewire；asymptotic optimality概念；narrow passage sampling难题；OMPL角色；**PRM / RRT-Connect 只做概念和适用场景比较**；与Manipulation高维规划桥接。
4. 深度：C-space/Collision L4；RRT L4；RRT* L3。
5. 工程连接：MoveIt/OMPL、robot footprint到state validity的统一理解。
6. 不展开：C-obstacle解析几何推导、sampling planner全集；PRM / RRT-Connect 的正式算法深化留到 Manipulation / OMPL。
7. 考核：解释workspace中的障碍怎样变成C-space中的不可行configuration；画一次RRT扩展并解释rewire。
8. 毕业考点：C-space、`C_free/C_obs`、RRT、RRT*、OMPL。

# Day68 — HPA / Hierarchical Planning / Dynamic Edge / Incremental Search
1. 今日目标：理解大图抽象、local refinement与动态edge失效，并回答“局部地图变化后为什么不一定需要从头搜索”。
2. 前置：Day63–67。
3. 必须教学：region/cluster；portal/gateway；abstract graph；high-level route；local refinement；precomputation；abstraction error；start/goal attach；K-nearest/LOS/JPS类attach思想；dynamic edge invalidation；edge block；TTL；local repair；fallback必须重新验证feasibility，禁止无条件Euclidean fallback；Incremental Search 动机；LPA* 的 g/rhs consistency 直觉；D* Lite 与 LPA* 的关系、反向搜索 / 移动 start 的工程直觉；复用旧搜索结果 vs full replan。
4. 深度：HPA L4-L5；Dynamic Edge L5；LPA* / D* Lite L3-L4。
5. 工程连接：大地图预热、封边、PathSwitch前置、局部 cost / edge 变化后的快速 repair。
6. 不展开：公司具体实现语义必须以实际源码为准；不要求完整证明 LPA* / D* Lite。
7. 考核：动态障碍使 abstract edge 失效后如何传播与恢复；说明 incremental repair 和从头 A* 的信息复用差异。
8. 毕业考点：HPA、Attach、Dynamic Edge、TTL、Incremental Search。

# Day69 — Nav2 / BT / Replanning / Path Switching
1. 今日目标：建立 Planner / Controller / BT / Recovery / Switch 的职责边界。
2. 前置：Day63–68。
3. 必须教学：Planner Server；Controller Server；BT Navigator；global path validity；periodic/event replanning；path switching；switch benefit/cost；hysteresis；persistence；cooldown；recovery；旧path保留条件；new path validation；planner failure vs controller failure；PathSwitchGuard类工程概念。
4. 深度：Nav2 boundary/Switch reasoning L5。
5. 工程连接：keep/switch、行人阻塞、动态路径切换。
6. 不展开：不根据未读取公司源码臆测阈值语义。
7. 考核：给两条path与障碍变化判断keep/switch/replan。
8. 毕业考点：BT、Replan、Path Switching、Responsibility Boundary。

# Day70 — Dynamic Navigation / Path Quality / Trajectory Bridge / Planning Owner
1. 今日目标：把动态障碍、路径质量和系统证据链合起来，并建立 Planning 输出如何进入 Trajectory / Control 的正式边界。
2. 前置：Day63–69。
3. 必须教学：dynamic obstacle time property；global vs local response；TTL/freshness；clearance；curvature；heading change；narrowness；reverse；costmap cost；smoothing后collision recheck；**Path ≠ Trajectory**；path parameter vs time parameter；trajectory 至少包含随时间变化的 pose / velocity / acceleration；Trajectory Optimization 基本形式：cost + dynamics + boundary + state/input constraints；local controller限制；failure taxonomy；evidence collection；planner source mapping；Planning Under Uncertainty 的最小边界：pose / obstacle uncertainty 会改变 feasibility / safety margin，不能把 covariance 当作零。
4. 深度：Owner Attribution L5；Path→Trajectory boundary L4；Trajectory Optimization bridge L2-L3；Planning Under Uncertainty boundary L2。
5. 工程连接：提前绕、钻窄路、绕后回拉、路径互搏、global path→MPPI reference。
6. 不展开：MPPI控制细节留M13；trajectory optimization 数值求解留 M13；Belief-space Planning / Chance Constraint / POMDP 在 M09/M10 后再深化。
7. 考核：从现象→costmap/path/switch/controller证据链定位责任；解释一条 collision-free path 为什么仍可能不是可执行 trajectory。
8. 毕业考点：Dynamic Planning、Path Quality、Path/Trajectory Boundary、Owner Debug。

---

# M11 Graduation Exam
统一权重：**30%核心基础 / 50%综合系统场景 / 20% Source·Formula·Design**。

## 30% 核心基础
硬门槛：Dijkstra/A*、heuristic、costmap/footprint/collision、Hybrid A*约束、`C_free/C_obs`与configuration validity、HPA/dynamic edge、Nav2责任边界、path switching；Anytime / Incremental Search 至少能解释其问题定义、复用信息与 trade-off。

## 50% 综合系统场景
至少覆盖：A*手算；Anytime 搜索质量—时间权衡；窄通道可行性；workspace obstacle→C-space validity；Hybrid A* vs kinodynamic boundary；行人动态阻塞 keep/switch/replan；HPA edge失效/TTL/fallback；局部变化时 incremental repair vs full replan；collision-free path 但 trajectory/control 不可执行；Planner path正常但robot异常时区分Planner/Controller。

## 20% Source / Formula / Design
在公司planner/HPA与Nav2官方实现中定位map/costmap输入、search、collision validity、path output、replan、BT、switch/guard调用链；能够用Modern Robotics Ch2/10的C-space语言解释MoveIt/OMPL前置思想，但不以教材替代真实Navigation源码。

## 通过标准
总分≥85%；A*、collision/footprint、C-space validity、HPA dynamic edge、Nav2 boundary不得有基础错误；必须能判断“规划错”还是“控制执行错”。
