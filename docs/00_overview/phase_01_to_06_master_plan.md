# 01–06 总计划（当前能力基线 → 系统核心开发 → 研究级评测底盘）

## 1. 文档定位

本文档定义 `robotics_skillup_lab` 仓库中 `01–06` 六个模块的长期主线计划。

这不是基础扫盲计划，也不是求职速成计划。  
它面向的目标是：

- 基于当前已经具备的 ROS2 / Nav2 / Linux / 脚本化 / 评测 / 失败案例沉淀能力继续上探
- 把“能跑、能排障、能记录、能做最小评估”的能力，提升到系统架构、运行机制、实验基础设施、源码改造、插件扩展与研究级评测层
- 为后续 `07–12` 的大代码库架构理解、高级插件、论文复现、原创方法与长期知识系统打下硬底盘

本计划默认已经具备以下基础：

- 已经做过系统化 ROS2 / Linux 命令训练，并具备闭卷、文档化、日志与结果落盘意识
- 已经做过 Nav2 评估、最小 A/B 对照、三地图泛化、bag 回放、失败案例整理与 ready packet 沉淀
- 已经不是“不会跑 demo”的阶段，而是正从工程层走向系统核心层

因此，本文档只关注“从现有工程能力继续向上”的路径，不重复基础入门内容。

---

## 2. 总体目标

`01–06` 六个模块的总体目标，不是“学更多知识点”，而是完成以下能力升级：

### 目标 1：系统架构理解能力
能从运行链、模块职责、配置入口、扩展点和执行时序角度解释 Nav2 / ROS2 系统，而不只是会运行。

### 目标 2：运行机制理解能力
能从 graph、QoS、executor、callback、lifecycle、time、TF、bag、launch 等机制角度解释系统行为与故障模式。

### 目标 3：实验基础设施能力
能把脚本、配置、日志、结果和图表组织成统一实验基础设施，而不是零散的一次性工具。

### 目标 4：语言与核心代码进入能力
通过 C++ / rclcpp / pluginlib 的系统学习，拿到进入 ROS2 / Nav2 核心开发层的语言门票。

### 目标 5：源码改造与扩展能力
能理解关键包、关键 server、关键插件的源码结构与扩展方式，并完成最小改造或最小插件接入。

### 目标 6：研究级评测与实验纪律
能设计 benchmark、指标、回归、鲁棒性、性能分析和可复现实验流程，而不是只会“跑一次看看”。

---

## 3. 当前能力基线（作为本计划的起点）

当前并不是从零起步。  
现有能力基线可以概括为：

### 已具备
- Linux / ROS2 基础命令训练与闭卷机制
- logs / results / docs / scripts / workspace 的证据链意识
- Nav2 基线评估、A/B 对照、三地图泛化、bag 回放
- failure case 记录与 ready packet 沉淀
- 面向工程验证的文档化能力

### 仍待补齐
- 运行证据 → 架构语言
- 现象理解 → 运行机制语言
- 脚本集合 → 实验基础设施
- 会看一点代码 → 真正进入核心源码层
- 工程评估 → 研究级评测纪律
- 最小修改 → 扩展点与插件层理解

因此，`01–06` 的主线必须是平滑过渡，而不是重头补基础，也不是直接跳顶刊层。

---

## 4. 总周期与投入规模

## 4.1 学习日总量

- `01_nav2_foundation_7d`：30 学习日
- `02_ros2_linux_fundamentals`：30 学习日
- `03_python_for_robotics`：25 学习日
- `04_cpp_for_ros2`：50 学习日
- `05_nav2_engineering_drills`：55 学习日
- `06_tooling_and_automation`：80 学习日

合计：

- **270 个学习日**

## 4.2 时间估算

按照每天 2–4 小时计算：

- 最低投入约：540 小时
- 最高投入约：1080 小时

按现实推进节奏估计：

- 高强度推进：约 12–14 个月
- 常规推进：约 18–24 个月
- 若中间有工作任务插队、项目返工、模块重做：拉长到 2–3 年完全正常

说明：

这里的时间不是为了压缩，而是为了保证深度和能力上限。  
如果某个 Sprint 难度过高，可以拆开重做；优先保证能力过关，不优先追求速度。

---

## 5. 统一执行机制

六个模块全部采用同一套高强度、证据链导向的执行框架。

## 5.1 Sprint 机制

每个模块由若干个 **5 天 Sprint** 组成。  
每个 Sprint 的固定节奏如下：

### Day 1：问题定义 + 图谱建立
- 定义本 Sprint 要解决的深层问题
- 明确该问题位于系统哪一层：架构层、运行层、源码层、插件层或实验层
- 列出本 Sprint 需要看的资料、源码、配置、日志、图表或实验对象

### Day 2：运行时证据采集
- 从 graph、topic、action、lifecycle、logs、bag、timing、trace、状态输出等角度抓运行证据
- 建立“概念 ↔ 现象 ↔ 证据”的对应关系

### Day 3：源码 / 实现深读
- 读包入口、类、接口、调用链、配置挂载路径
- 或完成一个很小但关键的实现/伪实现

### Day 4：修改 / 实验 / 对照
- 做小修改、小实验、参数对照、实现对照、profiling、trace 或 replay 验证
- 把理解转成证据

### Day 5：总结 + 闭卷 + 复盘
- 写 Sprint 总结
- 做闭卷问答
- 写“最容易混淆的概念”
- 明确下一 Sprint 的衔接点

---

## 5.2 每日固定产物

每天都必须留下文档。  
建议最少包含以下内容：

- 今天的核心问题
- 今天看的资料
- 今天抓到的运行证据
- 今天读到的源码入口或实现要点
- 今天做的修改 / 实验 / 对照
- 今天最容易混淆的概念
- 今天的闭卷问答
- 明天要衔接什么

---

## 5.3 闭卷问答规则（长期固定）

所有模块、所有 Sprint、所有模块总验收都必须保留闭卷机制。

### 规则 1：每天闭卷问答至少 12 题
- 先独立回答
- 不会就写“不确定”
- 不允许边查边答

### 规则 2：模块结束必须做总闭卷
- 总闭卷必须覆盖该模块全部核心问题
- 题目数量可以高于日常闭卷

### 规则 3：最终 md 必须同时保留两套答案
- 你的原始回答
- 我的修正后的标准回答

### 规则 4：闭卷问题必须逐步升级
不再停留在基础定义，重点转向：
- 为什么这样设计
- 为什么这个机制会影响系统行为
- 为什么这个改动合理或不合理
- 如何用证据支持一个结论
- 如何区分类似概念
- 如何从源码和运行时证明自己的判断

---

## 5.4 模块结束固定交付物

每个模块结束时必须至少交付以下文件：

### 1）模块总结
建议文件名：
- `module_summary.md`

必须包含：
- 本模块最核心的理解升级了什么
- 本模块补齐了哪些能力缺口
- 本模块还存在哪些半懂不懂的地方
- 本模块如何支撑下一个模块

### 2）模块总验收
建议文件名：
- `module_final_exam.md`

必须包含：
- 总闭卷题纲
- 你的原始回答
- 我的修正回答
- 模块最终结论与通过情况

### 3）证据索引
建议文件名：
- `evidence_index.md`

建议包含：
- 文档列表
- 图表列表
- 脚本列表
- 代码片段
- 实验记录
- 日志位置
- 配置文件
- 调用链记录
- 插件接入或改造记录

### 4）模块衔接判断
必须写明：
- 本模块结束后，现在能做什么
- 现在还不能做什么
- 下一模块最该往哪里钻

---

## 6. 模块 01：`01_nav2_foundation_7d`

## 6.1 模块重新定义

虽然目录名仍为 `foundation_7d`，但本模块内容按高阶版本执行。  
它不再是基础概念复习，而是：

**Nav2 架构抽象与执行链重建**

## 6.2 总时长
- 30 学习日
- 6 个 Sprint

## 6.3 模块核心目标

本模块要解决的不是“planner 是干什么的”，而是：

- 一个导航任务从 `NavigateToPose` 发出到机器人产生控制输出，执行链到底如何运行
- `bt_navigator`、`planner_server`、`controller_server`、`behavior_server` 在运行时如何配合
- action、BT、lifecycle、server、plugin、costmap、localization、TF 如何互相咬合
- 配置、参数、插件装载、默认 BT、运行角色之间如何一一对应
- Nav2 为什么是这种架构设计，而不是另一种

## 6.4 重点资料范围

- Nav2 官方文档
- Nav2 配置文档
- BT Navigator 相关资料
- 默认 BT XML
- 默认参数文件
- `navigation2` 源码仓库
- 运行系统输出
- 必要时查阅规划/控制相关论文或设计说明

## 6.5 模块成果

- `Nav2 执行链总图`
- `核心模块职责与交互图`
- `server / plugin / BT / action / lifecycle 边界表`
- `概念 → 包 → 节点 → 配置 → 运行时角色` 映射表
- `module_summary.md`
- `module_final_exam.md`
- `evidence_index.md`

## 6.6 模块验收条件

- 能从 `NavigateToPose` 一路讲到控制输出
- 能说清 `bt_navigator`、`planner_server`、`controller_server`、`behavior_server` 的协作逻辑
- 能区分 server 与 plugin 的职责
- 能说明 costmap、footprint、TF、map、localization 的运行角色
- 能给出概念到源码包的清晰映射
- 模块总闭卷通过

## 6.7 Sprint 主题

### Sprint 1：从 goal 到 motion 的总执行链
### Sprint 2：BT、Action、Lifecycle 的调度骨架
### Sprint 3：Planner / Controller / Behavior 的契约边界
### Sprint 4：Costmap / Footprint / Localization / TF 的耦合关系
### Sprint 5：参数、插件、配置与架构内核
### Sprint 6：源码入口整合与模块总复盘

---

## 7. 模块 02：`02_ros2_linux_fundamentals`

## 7.1 模块重新定义

虽然目录名仍为 `fundamentals`，但本模块内容按高阶版本执行。  
它不再是 Linux 命令复习，而是：

**ROS2 运行机制与系统行为深潜**

## 7.2 总时长
- 30 学习日
- 6 个 Sprint

## 7.3 模块核心目标

本模块要补的是系统运行机制，而不是命令表面：

- ROS2 graph 为什么是观察系统和调试系统的入口
- topic / service / action 在运行时的差异到底是什么
- QoS 为什么足以改变系统行为与正确性
- DDS / RMW 在系统中扮演什么角色
- executor / callback group / spin / timer 为什么会影响开发上限
- lifecycle、time、TF、bag、launch、parameter 是如何形成系统级耦合的

## 7.4 重点资料范围

- ROS2 design 文档
- QoS / lifecycle / executor / callback group / launch / parameters 相关文档
- DDS / RMW 资料
- ros2_tracing、profiling、运行时机制资料
- 运行系统证据
- 必要的源码入口

## 7.5 模块成果

- `ROS2 运行机制总图`
- `QoS 场景表`
- `executor / callback / spin` 关系图
- `TF + time + bag + launch + param` 系统关系图
- `runtime failure pattern` 清单
- `module_summary.md`
- `module_final_exam.md`
- `evidence_index.md`

## 7.6 模块验收条件

- 能解释 topic / service / action 的运行时差异
- 能解释 QoS 失配与行为异常之间的因果关系
- 能初步解释 executor / callback / timer / spin 的关系
- 能说明 use_sim_time、clock、TF、bag 的时序耦合
- 能用 graph 视角而不是“感觉”视角看系统问题
- 模块总闭卷通过

## 7.7 Sprint 主题

### Sprint 1：ROS2 graph 与系统观测论
### Sprint 2：QoS 与 DDS / RMW 深潜
### Sprint 3：Executor / Callback Group / Spin / Timer
### Sprint 4：Lifecycle / Parameters / Launch 的系统装配逻辑
### Sprint 5：TF、Clock、Bag、Determinism
### Sprint 6：Tracing / Profiling / Runtime Failure Pattern

---

## 8. 模块 03：`03_python_for_robotics`

## 8.1 模块重新定义

虽然目录名仍为 `python_for_robotics`，但本模块内容按高阶版本执行。  
它不再是 Python 语法训练，而是：

**实验基础设施与工程脚本体系**

## 8.2 总时长
- 25 学习日
- 5 个 Sprint

## 8.3 模块核心目标

本模块把 Python 从“工具语言”提升为：

- 实验 orchestration（编排）
- 配置管理
- 结果汇总
- 日志解析
- 参数 sweep
- 图表生成
- 研究/工程公共基础设施

要学的是：
- 工程脚本设计
- 错误处理
- 输入输出接口
- 目录规范
- 可复用而不是一次性脚本

## 8.4 重点资料范围

- Python packaging / 项目组织
- argparse / subprocess / pathlib / logging
- YAML / JSON / CSV
- pandas / numpy / matplotlib
- pytest（最少理解基本结构）
- 你已有的脚本、日志、结果目录、实验记录

## 8.5 模块成果

- 一套脚本设计模板
- 一套配置与结果目录规范
- 至少 2–3 个工程级工具
- 一份“Python 如何服务研究与工程”的说明
- `module_summary.md`
- `module_final_exam.md`
- `evidence_index.md`

## 8.6 模块验收条件

- Python 脚本具备清晰输入输出、日志、异常处理与目录结构
- 能处理配置、批量运行、结果归档、日志汇总
- 能支撑后面 05、06 模块
- 模块总闭卷通过

## 8.7 Sprint 主题

### Sprint 1：工程脚本设计学
### Sprint 2：配置、数据模式与目录规范
### Sprint 3：Subprocess / Logging / Batch Orchestration
### Sprint 4：结果解析、统计汇总、图表自动化
### Sprint 5：模块整合与总验收

---

## 9. 模块 04：`04_cpp_for_ros2`

## 9.1 模块重新定义

虽然目录名仍为 `cpp_for_ros2`，但本模块内容按高阶版本执行。  
它不再是普通 C++ 学习，而是：

**C++ / rclcpp / pluginlib 语言门槛突破**

## 9.2 总时长
- 50 学习日
- 10 个 Sprint

## 9.3 模块核心目标

本模块是进入高阶开发层的语言门票。  
要解决的问题包括：

- 现代 C++ 的对象模型、资源管理、所有权模型
- 接口、继承、多态、虚函数在插件体系中的意义
- 模板、类型系统、现代风格如何影响可读性与扩展性
- rclcpp 节点、executor、callback group 等 C++ 实现视角
- pluginlib 需要哪些语言前置能力

## 9.4 重点资料范围

- Effective Modern C++
- C++ Core Guidelines
- cppreference
- rclcpp 示例和相关源码
- pluginlib 相关资料
- ROS2 / Nav2 C++ 代码片段

## 9.5 模块成果

- C++ 深潜笔记
- 基础到中等复杂度的 C++ / ROS2 小程序
- 最小 `rclcpp` 节点
- plugin 风格接口骨架理解
- `module_summary.md`
- `module_final_exam.md`
- `evidence_index.md`

## 9.6 模块验收条件

- 能读懂基础到中等难度 ROS2 / Nav2 C++ 代码
- 能解释 shared_ptr / unique_ptr / 生命周期 / 所有权
- 能解释接口类、继承、多态、虚函数与插件机制的关系
- 能实现基础级 `rclcpp` 节点
- 模块总闭卷通过

## 9.7 Sprint 主题

### Sprint 1：现代 C++ 基础压缩回顾
### Sprint 2：类、对象、构造、析构与对象模型
### Sprint 3：头文件/源文件/编译组织/链接思维
### Sprint 4：引用、指针、const、RAII、所有权
### Sprint 5：STL、算法、lambda、现代风格
### Sprint 6：模板、类型系统、编译期思维
### Sprint 7：继承、虚函数、接口、多态
### Sprint 8：并发、线程、锁、callback 语义
### Sprint 9：rclcpp 节点、executor、callback group 实战
### Sprint 10：pluginlib 前置 + 模块总验收

---

## 10. 模块 05：`05_nav2_engineering_drills`

## 10.1 模块重新定义

虽然目录名仍为 `engineering_drills`，但本模块内容按高阶版本执行。  
它不再只是工程练习，而是：

**Nav2 源码入口、改造与扩展点**

## 10.2 总时长
- 55 学习日
- 11 个 Sprint

## 10.3 模块核心目标

本模块开始真正进入核心开发层：

- 全景理解 `navigation2` 仓库
- 读懂关键包的职责、入口、类、接口、插件装载点
- 解释 BT / server / plugin / config / action 在源码层如何接起来
- 完成小范围源码改造或最小插件扩展
- 把改动和实验验证绑定起来

## 10.4 重点资料范围

- `navigation2` 源码仓库
- build from source 流程
- plugin tutorials
- 默认 planner / controller / BT plugin
- costmap layers
- 关键 server 代码
- 必要的规划/控制资料

## 10.5 模块成果

- Nav2 核心包关系图
- 多个关键包阅读笔记
- 最小插件骨架理解或最小插件接入
- 一次小范围源码改造记录
- 一份“扩展点总览”
- `module_summary.md`
- `module_final_exam.md`
- `evidence_index.md`

## 10.6 模块验收条件

- 能说清 8–10 个核心包的职责与关系
- 能解释 BT / server / plugin / config / action 在源码层如何接起来
- 能完成一个小修改或最小插件接入
- 能用实验或运行证据验证修改影响
- 模块总闭卷通过

## 10.7 Sprint 主题

### Sprint 1：仓库全景与 build from source
### Sprint 2：bringup 与系统装配入口
### Sprint 3：bt_navigator 与 BT 层源码
### Sprint 4：planner_server 与 planner plugins
### Sprint 5：controller_server 与默认 controller plugins
### Sprint 6：costmap_2d、layers、footprint、observation
### Sprint 7：behavior_server / recovery 行为
### Sprint 8：map_server / amcl / localization 边界
### Sprint 9：pluginlib 装载链与配置挂接
### Sprint 10：做一个小修改或最小插件接入
### Sprint 11：源码层总整合 + 模块总验收

---

## 11. 模块 06：`06_tooling_and_automation`

## 11.1 模块重新定义

虽然目录名仍为 `tooling_and_automation`，但本模块内容按高阶版本执行。  
它不再是工具脚本集合，而是：

**评测科学、性能分析与研究实验纪律**

## 11.2 总时长
- 80 学习日
- 16 个 Sprint

## 11.3 模块核心目标

本模块是从高阶开发走向研究导向工作的关键桥梁。  
它要解决的是：

- benchmark 设计
- 指标设计
- regression / robustness / stress / fault injection
- profiling / tracing / timing / resource metrics
- reproducibility
- ablation / negative results
- claim-evidence map 前置能力
- 自动化实验执行骨架

## 11.4 重点资料范围

- benchmark / metric / reproducibility 相关资料
- profiling / tracing / resource analysis 资料
- fault injection 资料
- 你在前五个模块中沉淀的系统、代码和实验结果
- bag / replay / regression / baseline 相关记录

## 11.5 模块成果

- benchmark / metric 体系
- regression / robustness / stress / fault injection 模板
- profiling / tracing / timing / resource metrics 基础框架
- 自动化实验骨架
- claim-evidence map 前置模板
- `module_summary.md`
- `module_final_exam.md`
- `evidence_index.md`

## 11.6 模块验收条件

- 能自己定义一类 benchmark 和指标集
- 能设计对照、回归、鲁棒性、压力测试
- 能解释“为什么这个实验可信”
- 能做 profiling / tracing 基础分析
- 能把 failure case 升级成系统性 failure science
- 模块总闭卷通过

## 11.7 Sprint 主题

### Sprint 1：实验不是跑一次，是可复现系统
### Sprint 2：指标体系
### Sprint 3：对照实验与变量控制
### Sprint 4：Regression 与变更可信度
### Sprint 5：Robustness / Stress / Boundary Conditions
### Sprint 6：Failure Taxonomy 与 Fault Injection
### Sprint 7：Bag / Replay / Deterministic Validation
### Sprint 8：Profiling / Tracing / Timing
### Sprint 9：Resource Metrics
### Sprint 10：Benchmark 设计
### Sprint 11：Ablation 与 Negative Results
### Sprint 12：Claim-Evidence Map 前置
### Sprint 13：自动化实验执行骨架
### Sprint 14：结果图表与研究汇报骨架
### Sprint 15：模块整合与研究过渡
### Sprint 16：模块总整合 + 总验收

---

## 12. 模块之间的衔接逻辑

这份 01–06 计划的衔接逻辑如下：

### 01
先把已有的运行和评估经验，提升成架构语言

### 02
再把已有的现象观察，提升成运行机制语言

### 03
把已有的脚本和结果目录，升级成实验基础设施

### 04
拿下进入核心代码层的语言门票

### 05
进入 Nav2 源码、改造与扩展点层

### 06
把工程评测正式抬升为研究评测与实验纪律

这条线不是“从零学起”，而是“把现有能力继续向上顶”，因此比直接跳源码、直接跳论文更平滑，也更符合当前能力基线。

---

## 13. 模块结束后的通过判断

所有模块结束时，都不只问“学完没有”，而要问以下问题：

### 1）我现在能解释什么
是否能清晰解释本模块的核心机制、设计原因和边界。

### 2）我现在能做什么
是否能基于本模块完成实现、改造、实验、验证或分析动作。

### 3）我现在能证明什么
是否有图、表、日志、配置、代码、脚本、实验结果支撑自己的理解。

### 4）我现在还不能做什么
是否还能诚实指出自己尚未跨过去的门槛。

### 5）下个模块为什么成立
是否能明确说出下个模块是在补哪一块缺失能力，而不是随便进入新主题。

---

## 14. 执行建议

## 14.1 计划窗口职责
本总计划文档所在窗口只负责：
- 定义模块顺序
- 调整周期
- 调整目标
- 定义总验收
- 决定下一模块方向

## 14.2 执行窗口职责
执行窗口负责：
- 当天具体任务
- 当天闭卷题
- 当天 md 填写
- 当天代码、实验、图表或批改

## 14.3 使用原则
- 不在执行窗口临时改总路线
- 不在总计划窗口写当天杂事
- 每个模块结束后回到这里复盘和升级计划

---

## 15. 最终说明

这份计划不是为了“学快一点”，而是为了把能力上限真正往上推。  
它的关键词不是：

- 多记几个命令
- 多会几个包名
- 多做几个脚本

而是：

- 架构
- 机制
- 基础设施
- 源码
- 扩展
- 评测
- 证据
- 研究纪律

如果 `01–06` 全部做扎实，那么得到的不会只是“更熟练的工程能力”，而是：

- 更强的系统解释能力
- 更深的运行机制理解
- 更可靠的实验基础设施
- 更实际的源码进入能力
- 更明确的扩展点理解
- 更严谨的研究级评测底盘

这不是顶刊终点，但它是从工程开发真正走向高水平研究工作的硬底盘。