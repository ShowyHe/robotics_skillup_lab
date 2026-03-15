# s01_overview.md

# Sprint 1｜从 goal 到 motion 的总执行链

## 1. Sprint 核心问题

本 Sprint 要解决的核心问题不是“planner 是干什么的”“controller 是干什么的”，而是：

> 一个导航任务从 `NavigateToPose` 发出，到机器人最终产生控制输出，这条执行链到底如何运行？

这个问题必须被回答成一条完整链路，而不是拆成若干孤立概念。

本 Sprint 的任务，是先建立 Nav2 的第一张总执行链地图，回答：

- goal 由谁接收
- 流程由谁组织
- 规划由谁提供
- 控制由谁输出
- 恢复行为在什么位置介入
- costmap、TF、map、localization 等支撑层在链路里处于什么位置

本 Sprint 暂时不追求把所有细节一次性吃透，也不提前展开后续 Sprint 的重点内容。这里只做一件事：

> 先把 `goal → motion` 主链讲清、画清、证据化。

---

## 2. Sprint 与模块目标的关系

模块 01 的总目标，是把已有的 Nav2 运行、评测、bag 回放、failure case 经验，提升成以下几类系统语言：

- 执行链语言
- 模块协作语言
- 分层边界语言
- 配置挂载语言
- 架构设计语言

而 Sprint 1 是整个模块的第一块地基。

### 2.1 为什么 Sprint 1 必须先做总执行链

如果连 `NavigateToPose` 到控制输出的主链都没有一张稳定总图，后面去讲：

- BT、Action、Lifecycle 的调度骨架
- Planner / Controller / Behavior 的契约边界
- Costmap / Footprint / Localization / TF 的耦合关系
- 参数、插件、配置与架构内核
- 概念到源码入口的映射

这些内容都会失去抓手。

### 2.2 Sprint 1 在整个模块里的角色

Sprint 1 的定位不是把 01 模块一次做完，而是先拿到下面三样东西：

1. 一条能口述的主执行链  
2. 一张能复用的主链总图  
3. 一组“概念 ↔ 运行现象 ↔ 证据”的初步对应关系

这三样东西，会成为后面 5 个 Sprint 继续往下拆的共同底图。

### 2.3 本 Sprint 明确不解决什么

为了避免跑偏，本 Sprint 不提前深挖以下内容：

- BT / Action / Lifecycle 的调度骨架细节  
  这些是 Sprint 2 的主任务
- Planner / Controller / Behavior 的契约边界细节  
  这些是 Sprint 3 的主任务
- Costmap / Footprint / Localization / TF 的系统级耦合细节  
  这些是 Sprint 4 的主任务
- 参数、插件装载、默认 BT、运行角色的一一对应细节  
  这些是 Sprint 5 的主任务
- 概念到源码包的系统整合与模块级收口  
  这些是 Sprint 6 的主任务

也就是说，Sprint 1 只负责先把主链立起来，不抢后面 Sprint 的活。

---

## 3. Sprint 的 5 天任务安排

### Day 1｜问题定义 + 执行链骨架图 v0

**当天核心问题：**  
从 `NavigateToPose` 发出开始，到机器人产生运动控制，中间到底经过了哪些核心层和核心对象？

**当天目标：**

- 建立 `goal → motion` 第一版骨架图
- 明确本 Sprint 的问题边界
- 先把核心概念放到正确层级，而不是急着背局部定义

**当天重点任务：**

1. 重读模块 01 的总目标与 Sprint 1 的主题
2. 列出主链中的一级对象  
   建议至少包括：
   - `NavigateToPose`
   - `bt_navigator`
   - `planner_server`
   - `controller_server`
   - `behavior_server`
   - `cmd_vel`
   - costmap
   - TF
   - map
   - localization
3. 画出第一版主链草图
4. 列出当前最容易混淆的边界  
   例如：
   - action 和 BT 的关系
   - server 和 plugin 的关系
   - planner 输出和 controller 输出的关系
   - 恢复行为处在链路中的哪个位置

**当天输出：**

- `fig_01_goal_to_motion_chain_v0.png`
- `tbl_01_core_objects_and_layers.csv`
- `s01_d01_problem_map.md`

---

### Day 2｜运行时证据采集

**当天核心问题：**  
昨天画出的主链，能否从真实运行系统中找到证据支撑？

**当天目标：**

- 从运行时现象中抓证据，而不是只靠文档理解
- 建立“概念 ↔ 运行现象 ↔ 证据”的第一版对应关系
- 至少抓到一条成功链路证据

**当天重点任务：**

1. 在可运行的 Nav2 场景中发送一次 `NavigateToPose`
2. 记录从发出 goal 到机器人开始运动期间的关键观察点
3. 收集能够支撑主链的运行系统输出  
   例如：
   - action 相关现象
   - 关键节点存在性
   - 控制输出出现
   - 运行日志中的关键阶段提示
4. 尝试补一条轻量级对照现象  
   例如：
   - goal 发出但无法正常推进
   - 恢复行为被触发
   - 控制输出未按预期产生

**当天输出：**

- `tbl_02_runtime_evidence_chain.csv`
- `fig_02_runtime_signal_timeline.png`
- `s01_d02_runtime_evidence.md`

---

### Day 3｜文档与实现入口对齐

**当天核心问题：**  
运行时看到的主链，在官方文档、默认 BT XML、默认参数文件和必要源码入口中，能否对得上？

**当天目标：**

- 把运行现象和文档结构对齐
- 初步建立“概念 ↔ 包 / 节点 / 配置入口”的第一层连接
- 为后续 Sprint 的边界拆解做准备

**当天重点任务：**

1. 阅读与 Sprint 1 直接相关的 Nav2 官方资料
2. 查看默认 BT XML，观察主链中的关键节点如何被组织
3. 查看默认参数文件，识别主链中关键模块的配置入口
4. 必要时查看 `navigation2` 仓库中的最小入口代码或包结构
5. 回填 Day 1 的主链图，让图和文档结构初步对齐

**当天输出：**

- `tbl_03_concept_to_package_node_config_v0.csv`
- `fig_03_goal_to_motion_chain_doc_aligned.png`
- `s01_d03_source_reading.md`

---

### Day 4｜最小修改 / 对照验证

**当天核心问题：**  
这条主链不仅能“看懂”，还能不能通过一个最小修改或最小对照被验证？

**当天目标：**

- 用一个最小实验验证主链中的关键判断
- 让主链从“理解图”变成“被证据钉住的图”
- 避免做大改动，把实验范围控制在 Sprint 1 边界内

**当天重点任务：**

从下面两个方向选一个即可：

**方向 A：默认 BT 路径验证**
- 观察默认 BT 结构中的关键执行节点
- 结合运行现象，验证主链中“流程组织者”的位置是否判断正确

**方向 B：主链相关配置轻量验证**
- 对主链相关的一个配置入口做最小改动
- 观察现象是否与主链判断一致
- 记录“改动落点 → 现象变化 → 对主链判断的支撑”

**当天输出：**

- `exp_01_goal_to_motion_chain_probe.md`
- `tbl_04_change_to_effect_map.csv`
- `fig_04_goal_to_motion_chain_verified.png`
- `s01_d04_experiment_or_modification.md`

---

### Day 5｜Sprint 收口 + 闭卷 + 向 Sprint 2 交接

**当天核心问题：**  
经过前 4 天，是否已经真正拿到了 `goal → motion` 的第一版系统解释能力？

**当天目标：**

- 收口 Sprint 1 的图、表、证据和结论
- 完成 Sprint 1 闭卷
- 明确哪些问题已经解决，哪些问题留给 Sprint 2

**当天重点任务：**

1. 合并并整理前 4 天产出
2. 输出 Sprint 1 总结
3. 做至少 12 题闭卷
4. 保留“我的原始回答”和“GPT 修正后的标准回答”
5. 列出仍然不稳的点，作为 Sprint 2 的输入

**当天输出：**

- `s01_summary.md`
- `s01_exam.md`
- `tbl_05_sprint1_confusion_list.csv`
- `s01_d05_summary_and_exam.md`

---

## 4. Sprint 输入

本 Sprint 的输入，必须严格服从模块 01 的资料范围，并只围绕 Sprint 1 的主问题展开。

### 4.1 必看输入

- Nav2 官方文档
- Nav2 配置文档
- BT Navigator 相关资料
- 默认 BT XML
- 默认参数文件
- `navigation2` 源码仓库中的必要入口
- 可运行 Nav2 系统的真实运行输出

### 4.2 运行输入

本 Sprint 至少需要一个可运行的 Nav2 场景，作为运行时证据来源。  
证据形式可以包括：

- action 现象
- 关键节点与日志
- 控制输出表现
- 运行过程截图或时间线记录
- 必要的终端输出或 RViz 观察结果

### 4.3 可选输入

只有在主链判断需要补强时，才查阅下列内容：

- 规划相关论文或设计说明
- 控制相关论文或设计说明
- 官方实现说明中的扩展材料

注意：  
这些内容只允许作为补充，不允许把 Sprint 1 做成“论文阅读 Sprint”。

---

## 5. Sprint 输出

Sprint 1 的输出必须直接服务于模块 01 的总成果，尤其是为后续 Sprint 提供一张稳定底图。

### 5.1 图类输出

- `fig_01_goal_to_motion_chain_v0.png`
- `fig_02_runtime_signal_timeline.png`
- `fig_03_goal_to_motion_chain_doc_aligned.png`
- `fig_04_goal_to_motion_chain_verified.png`

### 5.2 表类输出

- `tbl_01_core_objects_and_layers.csv`
- `tbl_02_runtime_evidence_chain.csv`
- `tbl_03_concept_to_package_node_config_v0.csv`
- `tbl_04_change_to_effect_map.csv`
- `tbl_05_sprint1_confusion_list.csv`

### 5.3 文档输出

- `s01_d01_problem_map.md`
- `s01_d02_runtime_evidence.md`
- `s01_d03_source_reading.md`
- `s01_d04_experiment_or_modification.md`
- `s01_d05_summary_and_exam.md`
- `s01_summary.md`
- `s01_exam.md`

### 5.4 Sprint 1 必须拿到的阶段成果

Sprint 1 结束时，至少要拿到以下阶段成果：

1. 一版可口述的 `NavigateToPose → 控制输出` 主执行链  
2. 一张可复用的主链图  
3. 一组“概念 ↔ 运行现象 ↔ 证据”的初步对应关系  
4. 一版“概念 → 包 → 节点 → 配置”初步骨架  
5. 一份 Sprint 1 易混概念清单

---

## 6. Sprint 验收条件

Sprint 1 是否通过，不看“看了多少文档”，而看是否真正拿到了主链理解。

### 6.1 主链口述验收

必须能够从 `NavigateToPose` 一路讲到控制输出，并说明：

- 关键执行节点
- 状态流转
- 中间协作关系
- 恢复行为大致处于什么位置
- 支撑层在主链中的作用位置

如果仍然只能说“planner 规划路径、controller 跟踪路径”，则 Sprint 1 不通过。

### 6.2 主链图验收

必须形成一张完整度足够的主链图，图中至少应包含：

- goal 入口
- `bt_navigator`
- `planner_server`
- `controller_server`
- `behavior_server`
- 控制输出
- 支撑层位置

这张图不要求在 Sprint 1 就达到模块最终版，但必须已经能作为后续 Sprint 的共同底图。

### 6.3 证据验收

必须给出至少一条被运行时现象支撑的成功链路证据。  
最好再补一条轻量对照证据，用来说明主链不是拍脑袋画出来的。

### 6.4 结构骨架验收

必须建立第一版结构骨架，至少能说出一部分：

- 这个概念大致落在哪个包
- 哪个节点承担它的运行角色
- 哪类配置与它相关

这里不要求在 Sprint 1 一次完成模块最终的总映射表，但必须已经有第一层雏形。

### 6.5 边界意识验收

必须能指出至少 3 组当前容易混淆的边界，并说明混淆点在哪里。  
例如：

- action 和 BT
- server 和 plugin
- planner 输出和 controller 输出
- 主执行层和支撑层

Sprint 1 不要求完全吃透这些边界，但必须知道“哪里容易混”。

---

## 7. Day 5 的总结与闭卷要求

根据总控规则，Sprint 第 5 天必须完成阶段收口，而不能只留下零散动作。

### 7.1 Sprint 总结必须回答的问题

Sprint 1 总结中，至少要回答下面几个问题：

1. 我现在能否从 `NavigateToPose` 讲到控制输出？  
2. 我已经能说清哪些关键执行节点？  
3. 我的主链图中，哪些部分已有证据支撑？  
4. 哪些部分还只是初步判断？  
5. 哪些边界仍然模糊，必须交给 Sprint 2 继续解决？

### 7.2 闭卷要求

- 至少 12 题
- 必须先写“我的原始回答”
- 再写“GPT 修正后的标准回答”
- 题目不能全是名词解释，必须覆盖：
  - 主链口述
  - 关键节点职责
  - 中间协作关系
  - 支撑层位置
  - 易混边界
  - 证据支撑点

### 7.3 Sprint 2 的交接要求

Sprint 1 收尾时，必须明确写出以下结论：

> 我现在已经拿到了 `goal → motion` 的第一版主链，但还没有真正拆开“流程是谁在调度、状态是谁在管理、BT / Action / Lifecycle 是如何组织这条链”的问题。  
> 这些问题将进入 Sprint 2 继续解决。

这句交接很关键。  
没有它，Sprint 1 很容易做成“看起来学了不少，实际上没有把问题传给下一阶段”的散装笔记。

---