# M01 — C++ / Linux / ROS2 Systems

## Module Goal
从“会写ROS2代码”提升到能够理解对象生命周期、callback、并发、Linux运行时、DDS/QoS、Executor/Callback Group、TF、Lifecycle、Plugin 与 ros2_control 的系统执行逻辑。

---

## Day2 — C++对象、内存、生命周期与 Ownership

### 今日目标
真正理解一个机器人C++程序中：对象在哪里、什么时候存在、谁拥有它、什么时候释放。

### 前置知识
基础C++：class、function、pointer基本语法。

### 必须教学内容
1. Stack / Heap：生命周期差异、自动对象与动态对象；不能简化成“stack快、heap慢”。
2. Pointer / Reference：地址、别名、nullable差异；ownership与pointer不是同一概念。
3. Constructor / Destructor：construction → use → destruction。
4. RAII：资源与对象生命周期绑定；联系memory、file、mutex、ROS资源。
5. Ownership：Who owns? Who observes? Who determines lifetime?
6. `unique_ptr/shared_ptr/weak_ptr`：重点是所有权语义，不是API。
7. Copy / Move：理解复制和转移资源的意义，大型消息/容器为何在意复制。
8. Callback生命周期：callback可能活得比创建它的局部变量更久。

### 深度要求
Lifetime/ownership L3；智能指针L3；Move L2。

### 工程连接
ROS2 Node、Publisher、Subscription、Timer、Callback、shared_ptr。

### 明确不展开
allocator、placement new、custom memory pool、复杂模板元编程。

### 本日考核点
1. stack对象什么时候销毁？
2. heap对象由什么决定生命周期？
3. pointer和ownership为什么不是一回事？
4. RAII解决什么问题？
5. unique/shared/weak分别解决什么问题？
6. 什么是dangling pointer？
7. callback捕获局部变量为什么危险？
8. 给一段ROS2代码画ownership关系。

### M01毕业考试核心考点
Object lifetime、ownership、callback生命周期、shared_ptr误用导致的系统问题。

---

## Day3 — STL、Lambda、Callback与现代C++数据流

### 今日目标
能够顺着陌生机器人C++源码追踪：数据在哪里存 → 函数在哪里注册 → callback什么时候执行 → 数据最终流向哪里。

### 前置知识
Day2。

### 必须教学内容
1. `vector/deque/map/unordered_map` 的结构、适用场景与基本复杂度直觉。
2. value / reference / const reference 参数传递。
3. callable：普通函数 → function object → lambda → `std::function`。
4. lambda capture：`[] [&] [=] [this]`，以及capture与生命周期关系。
5. callback模型：保存一个未来由事件触发的函数，而非立即调用。
6. template基础：一份逻辑针对不同类型生成代码。
7. `auto`、range-for、move与容器的工程意义。

### 深度要求
Callback/lambda L3；STL容器选择 L2-L3；Template L2。

### 工程连接
`create_subscription`、Timer callback、Action callback、Service callback、async callback。

### 明确不展开
SFINAE、Concepts、模板元编程、STL源码。

### 本日考核点
1. vector与map适用场景区别？
2. `const T&`解决什么问题？
3. `std::function`是什么？
4. `[&]`和`[=]`本质区别？
5. `[this]`捕获了什么？
6. callback为什么会产生生命周期问题？
7. 从`create_subscription()`追到真正业务函数。
8. 判断lambda捕获是否安全。

### M01毕业考试核心考点
Callback注册链、lambda capture、数据流追踪、参数传递与生命周期。

---

## Day4 — Process、Thread与并发安全

### 今日目标
理解机器人程序为什么会出现偶发错误、卡死、数据错乱和时序问题。

### 前置知识
Day2–3。

### 必须教学内容
1. Process vs Thread：地址空间、共享资源、thread自身stack。
2. Concurrency vs Parallelism。
3. Shared State：并发问题核心通常是共享可变状态。
4. Race Condition：通过执行顺序理解。
5. Critical Section。
6. Mutex / Lock：互斥保护的对象是什么。
7. `lock_guard`：RAII在锁上的应用。
8. Condition Variable：等待条件 vs busy wait。
9. Atomic：适合简单原子状态，不是万能替代mutex。
10. Deadlock：形成条件与锁顺序。
11. Producer / Consumer：Sensor→Processing典型并发模型。
12. Callback并发：为Day7 Executor做准备。

### 深度要求
Race/mutex/deadlock L3；condition_variable L2-L3；atomic L2。

### 工程连接
多个ROS callback共享当前速度、path、robot state、emergency状态。

### 明确不展开
lock-free、C++ memory order、kernel scheduler实现。

### 本日考核点
1. process和thread关键区别？
2. concurrency和parallelism是否相同？
3. 什么是race condition？
4. 为什么只读通常安全而同时写可能有问题？
5. mutex保护的到底是什么？
6. condition variable解决什么？
7. atomic为什么不能替代所有mutex？
8. deadlock怎么形成？
9. 给两个callback判断race。
10. 给出修复方案并说明代价。

### M01毕业考试核心考点
Shared state、race、lock、deadlock、callback并发。

---

## Day5 — Linux Runtime与机器人实时行为

### 今日目标
从Linux角度解释为什么配置10Hz的ROS节点真实系统可能不能稳定10Hz运行。

### 前置知识
Day4。

### 必须教学内容
1. Process virtual address space。
2. Virtual memory基础模型。
3. User space / kernel space基本概念。
4. System call。
5. File descriptor。
6. File / pipe / socket统一IO抽象。
7. Blocking / non-blocking IO。
8. IPC。
9. Socket。
10. Runnable / sleeping。
11. Scheduler。
12. Context switch。
13. CPU-bound / IO-bound。
14. CPU / memory / IO竞争。
15. Latency vs throughput。
16. Signal基础。
17. 必须解释：Timer配置周期 ≠ callback真正执行周期。

### 深度要求
Linux运行时模型L2-L3；performance reasoning L3。

### 工程连接
network read阻塞callback、CPU 100%、LIO计算耗时、message积压、ROS节点频率下降。

### 明确不展开
Linux kernel源码、页表细节、TCP拥塞控制。

### 本日考核点
1. 什么是virtual memory？
2. file descriptor是什么？
3. blocking IO为什么拖慢节点？
4. CPU-bound和IO-bound如何区别？
5. scheduler为什么影响延迟？
6. throughput高是否意味着latency低？
7. 10Hz timer为什么可能只有7Hz输出？
8. 给一个ROS延迟现象列Linux层排查树。

### M01毕业考试核心考点
Blocking、scheduling、CPU/IO、timing、performance diagnosis。

---

## Day6 — ROS2通信、DDS与QoS

### 今日目标
理解ROS2消息为什么能传，以及为什么publisher存在时subscriber仍可能拿不到正确数据。

### 前置知识
Day2–5。

### 必须教学内容
1. Topic / Service / Action：从通信语义区分，而非只看API。
2. DDS作用：ROS2 API → RMW → DDS → Network / Shared Memory，工程级理解。
3. Discovery：节点如何发现endpoint。
4. QoS：Reliability、History、Depth、Durability。
5. QoS Compatibility：判断publisher/subscriber是否匹配。
6. Queue：production rate > consumption rate 时发生什么。
7. Reliable代价：更可靠不等于更适合实时传感器。
8. Sensor Data：Camera/LiDAR为何常用best effort。

### 深度要求
Topic/Service/Action L3；QoS L3；DDS内部 L1-L2。

### 工程连接
LiDAR、Camera、odometry、command、Navigation Action。

### 明确不展开
RTPS packet、FastDDS/CycloneDDS源码。

### 本日考核点
1. Topic/Service/Action各适合什么？
2. DDS在ROS2中负责什么？
3. Reliability是什么？
4. History和Depth什么关系？
5. Durability解决什么？
6. 什么是QoS compatibility？
7. reliable一定更好吗？
8. subscriber慢于publisher会怎样？
9. 高频LiDAR为什么可能选best effort？
10. 给两个QoS配置判断是否合理。

### M01毕业考试核心考点
ROS通信模式、DDS基本作用、QoS配置、queue/backpressure、latency/reliability trade-off。

---

## Day7 — Executor、Callback Group、TF、Lifecycle、Plugin与ROS2系统架构

### 今日目标
把Day2–Day6串起来，理解callback什么时候执行、在哪个线程执行、为什么可能互相阻塞，以及大型机器人系统如何组织。

### 前置知识
Day2–Day6全部核心内容。

### 必须教学内容
1. Executor：等待ready entities并调度callback。
2. `spin()`：不是简单死循环。
3. Ready Entity：subscription消息、timer到期、service request、future ready与callback执行的关系。
4. SingleThreadedExecutor：串行callback。
5. MultiThreadedExecutor：并发能力不自动等于线程安全。
6. Callback Group：MutuallyExclusive / Reentrant，与Executor线程组合关系。
7. Async / Future：request → server callback → response → future ready → client callback。
8. TF2：Transform、Buffer、TransformListener、source/target frame、timestamp、`TimePointZero`。
9. Lifecycle：主要状态与机器人系统为何需要生命周期管理。
10. Component / Composition：减少进程边界的目的与trade-off。
11. Pluginlib：Interface → Implementation → Runtime Loading。
12. ros2_control：Controller Manager → Controller → Hardware Interface → Actuator。
13. Parameters / Launch：配置层与运行时关系。
14. 系统分层：Sensor → Localization → Costmap → Planner → BT → Controller → Chassis，分别通过什么接口连接。

### 深度要求
Executor/Callback Group L3-L4；TF L3；Action async L3；Pluginlib L3；Lifecycle/Component/ros2_control L2-L3。

### 工程连接
大量联系真实Nav2/ROS2源码结构。

### 明确不展开
DDS内部thread、Nav2具体算法、ros2_control控制数学、realtime kernel。

### 本日考核点
1. Executor为什么存在？
2. callback什么时候ready？
3. Single/MultiThreaded区别？
4. MultiThreaded为什么仍可能没有并行？
5. MutuallyExclusive与Reentrant区别？
6. future什么时候ready？
7. server callback与client response callback区别？
8. TF Buffer存什么？
9. `TimePointZero`是什么意思？
10. Lifecycle为什么适合机器人？
11. Pluginlib为什么不是简单函数调用？
12. ros2_control三层分别负责什么？
13. 给一个ROS系统判断Node/Action/Topic/TF/Plugin关系。

---

# M01 Graduation Exam Specification

## A. 核心基础专项 — 30%
至少覆盖：lifetime/ownership、lambda/callback、race/mutex/deadlock、process/thread、blocking/scheduling、QoS、Executor/Callback Group、TF、Action async、Plugin。核心概念不能靠总分弥补。

## B. 系统综合场景 — 50%
设置2–4个真实机器人场景。每题要求同时调用多个知识点，例如：LiDAR 10Hz；Localization偶尔400ms无输出；MultiThreadedExecutor；两个callback访问同一状态；NavigateToPose仍running；TF timeout。要求从Linux runtime、callback scheduling、shared state、QoS、TF freshness、Action state等角度构造：

```text
现象
↓
假设
↓
需要什么证据
↓
如何验证
↓
最可能根因
↓
修改方案
```

## C. 源码 / 架构题 — 20%
给陌生ROS2源码片段，要求找callback注册、追执行链、判断对象lifetime、并发风险、QoS、plugin结构并画数据流。

## Knowledge Coverage Matrix
- C++ Lifetime / Ownership：必考
- Smart Pointer：必考
- Lambda / Callback：必考
- STL数据流：必考
- Thread / Race：必考
- Mutex / Deadlock：必考
- Linux Blocking / Scheduling：必考
- CPU / IO / Latency：必考
- Topic / Service / Action：必考
- DDS / QoS：必考
- Executor：必考
- Callback Group：必考
- Async Future：必考
- TF：必考
- Lifecycle：至少理解题
- Component：至少理解题
- Pluginlib：必考
- ros2_control架构：至少理解题

## 通过标准
- 总分 ≥85%；
- 所有核心必考项不得出现根本性概念错误；
- 综合场景必须形成证据链，不能只列可能原因；
- 总分够但单项核心错误：定向补课 + 复测，不重学整个M01。