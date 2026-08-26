# M01 — C++ / Linux / ROS2 Systems

## Module Goal
从“会写 ROS2 代码”提升到能够理解对象生命周期、callback、并发、Linux 运行时、DDS/QoS、Executor / Callback Group、TF、Lifecycle、Plugin 与 ros2_control 的系统执行逻辑。

本模块按每天 2–3h 理论学习设计。Day7 内容较多，因此明确以 **Executor / Callback Group / Async / TF** 为主线；Lifecycle / Component / ros2_control 只要求架构级理解，不在本模块深入实现细节。

---

## Day2 — C++对象、内存、生命周期与 Ownership

### 今日目标
真正理解一个机器人 C++ 程序中：对象在哪里、什么时候存在、谁拥有它、什么时候释放。

### 前置知识
基础 C++：class、function、pointer 基本语法。

### 必须教学内容
1. Stack / Heap：生命周期差异、自动对象与动态对象；不能简化成“stack 快、heap 慢”。
2. Pointer / Reference：地址、别名、nullable 差异；ownership 与 pointer 不是同一概念。
3. Constructor / Destructor：construction → use → destruction。
4. RAII：资源与对象生命周期绑定；联系 memory、file、mutex、ROS 资源。
5. Ownership：Who owns? Who observes? Who determines lifetime?
6. `unique_ptr/shared_ptr/weak_ptr`：重点是所有权语义，不是 API。
7. Copy / Move：理解复制和转移资源的意义，大型消息/容器为何在意复制。
8. Callback 生命周期：callback 可能活得比创建它的局部变量更久。

### 深度要求
Lifetime / ownership L3；智能指针 L3；Move L2。

### 工程连接
ROS2 Node、Publisher、Subscription、Timer、Callback、shared_ptr。

### 明确不展开
allocator、placement new、custom memory pool、复杂模板元编程。

### 本日考核点
1. stack 对象什么时候销毁？
2. heap 对象由什么决定生命周期？
3. pointer 和 ownership 为什么不是一回事？
4. RAII 解决什么问题？
5. unique/shared/weak 分别解决什么问题？
6. 什么是 dangling pointer？
7. callback 捕获局部变量为什么危险？
8. 给一段 ROS2 代码画 ownership 关系。

### M01毕业考试核心考点
Object lifetime、ownership、callback 生命周期、shared_ptr 误用导致的系统问题。

---

## Day3 — STL、Lambda、Callback 与现代 C++ 数据流

### 今日目标
能够顺着陌生机器人 C++ 源码追踪：数据在哪里存 → 函数在哪里注册 → callback 什么时候执行 → 数据最终流向哪里。

### 前置知识
Day2。

### 必须教学内容
1. `vector/deque/map/unordered_map` 的结构、适用场景与基本复杂度直觉。
2. value / reference / const reference 参数传递。
3. callable：普通函数 → function object → lambda → `std::function`。
4. lambda capture：`[] [&] [=] [this]`，以及 capture 与生命周期关系。
5. callback 模型：保存一个未来由事件触发的函数，而非立即调用。
6. template 基础：一份逻辑针对不同类型生成代码。
7. `auto`、range-for、move 与容器的工程意义。

### 深度要求
Callback / lambda L3；STL 容器选择 L2-L3；Template L2。

### 工程连接
`create_subscription`、Timer callback、Action callback、Service callback、async callback。

### 明确不展开
SFINAE、Concepts、模板元编程、STL 源码。

### 本日考核点
1. vector 与 map 适用场景区别？
2. `const T&` 解决什么问题？
3. `std::function` 是什么？
4. `[&]` 和 `[=]` 本质区别？
5. `[this]` 捕获了什么？
6. callback 为什么会产生生命周期问题？
7. 从 `create_subscription()` 追到真正业务函数。
8. 判断 lambda 捕获是否安全。

### M01毕业考试核心考点
Callback 注册链、lambda capture、数据流追踪、参数传递与生命周期。

---

## Day4 — Process、Thread 与并发安全

### 今日目标
理解机器人程序为什么会出现偶发错误、卡死、数据错乱和时序问题。

### 前置知识
Day2–3。

### 必须教学内容
1. Process vs Thread：地址空间、共享资源、thread 自身 stack。
2. Concurrency vs Parallelism。
3. Shared State：并发问题核心通常是共享可变状态。
4. Race Condition：通过执行顺序理解。
5. Critical Section。
6. Mutex / Lock：互斥保护的对象是什么。
7. `lock_guard`：RAII 在锁上的应用。
8. Condition Variable：等待条件 vs busy wait。
9. Atomic：适合简单原子状态，不是万能替代 mutex。
10. Deadlock：形成条件与锁顺序。
11. Producer / Consumer：Sensor → Processing 典型并发模型。
12. Callback 并发：为 Day7 Executor 做准备。

### 深度要求
Race / mutex / deadlock L3；condition_variable L2-L3；atomic L2。

### 工程连接
多个 ROS callback 共享当前速度、path、robot state、emergency 状态。

### 明确不展开
lock-free、C++ memory order、kernel scheduler 实现。

### 本日考核点
1. process 和 thread 关键区别？
2. concurrency 和 parallelism 是否相同？
3. 什么是 race condition？
4. 为什么只读通常安全而同时写可能有问题？
5. mutex 保护的到底是什么？
6. condition variable 解决什么？
7. atomic 为什么不能替代所有 mutex？
8. deadlock 怎么形成？
9. 给两个 callback 判断 race。
10. 给出修复方案并说明代价。

### M01毕业考试核心考点
Shared state、race、lock、deadlock、callback 并发。

---

## Day5 — Linux Runtime 与机器人实时行为

### 今日目标
从 Linux 角度解释为什么配置 10Hz 的 ROS 节点真实系统可能不能稳定 10Hz 运行。

### 前置知识
Day4。

### 必须教学内容
1. Process virtual address space。
2. Virtual memory 基础模型。
3. User space / kernel space 基本概念。
4. System call。
5. File descriptor。
6. File / pipe / socket 统一 IO 抽象。
7. Blocking / non-blocking IO。
8. IPC。
9. Socket。
10. Runnable / sleeping。
11. Scheduler。
12. Context switch。
13. CPU-bound / IO-bound。
14. CPU / memory / IO 竞争。
15. Latency vs throughput。
16. Signal 基础。
17. 必须解释：Timer 配置周期 ≠ callback 真正执行周期。

### 深度要求
Linux 运行时模型 L2-L3；performance reasoning L3。

### 工程连接
network read 阻塞 callback、CPU 100%、LIO 计算耗时、message 积压、ROS 节点频率下降。

### 明确不展开
Linux kernel 源码、页表细节、TCP 拥塞控制。

### 本日考核点
1. 什么是 virtual memory？
2. file descriptor 是什么？
3. blocking IO 为什么拖慢节点？
4. CPU-bound 和 IO-bound 如何区别？
5. scheduler 为什么影响延迟？
6. throughput 高是否意味着 latency 低？
7. 10Hz timer 为什么可能只有 7Hz 输出？
8. 给一个 ROS 延迟现象列 Linux 层排查树。

### M01毕业考试核心考点
Blocking、scheduling、CPU/IO、timing、performance diagnosis。

---

## Day6 — ROS2通信、DDS 与 QoS

### 今日目标
理解 ROS2 消息为什么能传，以及为什么 publisher 存在时 subscriber 仍可能拿不到正确数据。

### 前置知识
Day2–5。

### 必须教学内容
1. Topic / Service / Action：从通信语义区分，而非只看 API。
2. DDS 作用：ROS2 API → RMW → DDS → Network / Shared Memory，工程级理解。
3. Discovery：节点如何发现 endpoint。
4. QoS：Reliability、History、Depth、Durability。
5. QoS Compatibility：判断 publisher/subscriber 是否匹配。
6. Queue：production rate > consumption rate 时发生什么。
7. Reliable 代价：更可靠不等于更适合实时传感器。
8. Sensor Data：Camera/LiDAR 为何常用 best effort。

### 深度要求
Topic / Service / Action L3；QoS L3；DDS 内部 L1-L2。

### 工程连接
LiDAR、Camera、odometry、command、Navigation Action。

### 明确不展开
RTPS packet、FastDDS/CycloneDDS 源码。

### 本日考核点
1. Topic/Service/Action 各适合什么？
2. DDS 在 ROS2 中负责什么？
3. Reliability 是什么？
4. History 和 Depth 什么关系？
5. Durability 解决什么？
6. 什么是 QoS compatibility？
7. reliable 一定更好吗？
8. subscriber 慢于 publisher 会怎样？
9. 高频 LiDAR 为什么可能选 best effort？
10. 给两个 QoS 配置判断是否合理。

### M01毕业考试核心考点
ROS 通信模式、DDS 基本作用、QoS 配置、queue/backpressure、latency/reliability trade-off。

---

## Day7 — Executor、Callback Group、TF、Lifecycle、Plugin 与 ROS2 系统架构

### 今日目标
把 Day2–Day6 串起来，理解 callback 什么时候执行、在哪个线程执行、为什么可能互相阻塞，以及大型机器人系统如何组织。

### 前置知识
Day2–Day6 全部核心内容。

### 必须教学内容
1. **Executor（重点）**：等待 ready entities 并调度 callback。
2. **`spin()`（重点）**：不是简单死循环；理解等待与调度角色。
3. **Ready Entity（重点）**：subscription 消息、timer 到期、service request、future ready 与 callback 执行的关系。
4. **SingleThreadedExecutor（重点）**：串行 callback。
5. **MultiThreadedExecutor（重点）**：具备并发能力，但不自动等于线程安全，也不保证所有 callback 都并行。
6. **Callback Group（重点）**：MutuallyExclusive / Reentrant，与 Executor 线程组合关系。
7. **Async / Future（重点）**：request → server callback → response → future ready → client callback。
8. **TF2（重点）**：Transform、Buffer、TransformListener、source/target frame、timestamp、`TimePointZero`。
9. Lifecycle：主要状态、生命周期管理的工程价值；只要求架构级理解。
10. Component / Composition：减少进程边界的目的、性能收益与隔离性 trade-off；只要求架构级理解。
11. Pluginlib：Interface → Implementation → Runtime Loading；要求能读懂插件式结构。
12. ros2_control：Controller Manager → Controller → Hardware Interface → Actuator；只要求系统分层，不深入 controller 数学和硬件实现。
13. Parameters / Launch：配置层与运行时关系。
14. 系统分层：Sensor → Localization → Costmap → Planner → BT → Controller → Chassis，分别通过 Topic / Action / TF / Plugin 等接口连接。

### 深度要求
- Executor / Callback Group：L3-L4，Day7 主重点。
- TF：L3，Day7 主重点。
- Action async / Future：L3，Day7 主重点。
- Pluginlib：L3。
- Lifecycle / Component / ros2_control：**L2，仅要求架构与责任边界，不在本日深入实现细节。**

### 时间优先级
若 2–3h 内无法覆盖全部内容，必须优先保证：

```text
Executor
→ Callback Group
→ Async / Future
→ TF2
→ Pluginlib
```

Lifecycle / Component / ros2_control 只做结构性理解，不因“课程点数”压缩前述核心内容。

### 工程连接
大量联系真实 Nav2 / ROS2 源码结构，重点练习 callback 注册 → ready → Executor 调度 → shared state / TF / Action 的系统链路。

### 明确不展开
DDS 内部 thread、Nav2 具体规划控制算法、ros2_control 控制数学、realtime kernel、Lifecycle/Component 底层实现。

### 本日考核点
1. Executor 为什么存在？
2. callback 什么时候 ready？
3. Single/MultiThreaded 区别？
4. MultiThreaded 为什么仍可能没有并行？
5. MutuallyExclusive 与 Reentrant 区别？
6. future 什么时候 ready？
7. server callback 与 client response callback 区别？
8. TF Buffer 存什么？
9. `TimePointZero` 是什么意思？
10. Lifecycle 为什么适合机器人？（架构级）
11. Pluginlib 为什么不是简单函数调用？
12. ros2_control 三层分别负责什么？（架构级）
13. 给一个 ROS 系统判断 Node / Action / Topic / TF / Plugin 关系。

---

# M01 Graduation Exam Specification

## A. 核心基础专项 — 30%
至少覆盖：lifetime/ownership、lambda/callback、race/mutex/deadlock、process/thread、blocking/scheduling、QoS、Executor/Callback Group、TF、Action async、Plugin。核心概念不能靠总分弥补。

## B. 系统综合场景 — 50%
设置 2–4 个真实机器人场景。每题要求同时调用多个知识点。例如：LiDAR 10Hz；Localization 偶尔 400ms 无输出；系统使用 MultiThreadedExecutor；两个 callback 访问同一状态；NavigateToPose 仍 running；同时出现 TF timeout。

要求从 Linux runtime、callback scheduling、shared state、QoS、TF freshness、Action state 等角度构造：

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
给陌生 ROS2 源码片段，要求找 callback 注册、追执行链、判断对象 lifetime、并发风险、QoS、plugin 结构并画数据流。

## Knowledge Coverage Matrix
- C++ Lifetime / Ownership：必考
- Smart Pointer：必考
- Lambda / Callback：必考
- STL 数据流：必考
- Thread / Race：必考
- Mutex / Deadlock：必考
- Linux Blocking / Scheduling：必考
- CPU / IO / Latency：必考
- Topic / Service / Action：必考
- DDS / QoS：必考
- Executor：**核心必考**
- Callback Group：**核心必考**
- Async Future：必考
- TF：**核心必考**
- Lifecycle：至少理解题
- Component：至少理解题
- Pluginlib：必考
- ros2_control 架构：至少理解题

## 通过标准
- 总分 ≥85%；
- 所有核心必考项不得出现根本性概念错误；
- 综合场景必须形成证据链，不能只罗列可能原因；
- 总分够但单个核心基础错误时：只补该知识点并定向复测，不要求整个 M01 重学；
- 已明显掌握的内容允许在正式学习时通过入门测试跳过。
