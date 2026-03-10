# 02｜ROS2 运行机制与系统行为深潜

## 定位

本目录虽然沿用了 `ros2_linux_fundamentals` 这个名字，但实际内容不再是基础命令复习，而是针对 ROS2 系统运行机制进行深潜。

目标不是再练一遍 `ros2 topic list`、`grep` 或 `find` 这种命令，而是回答下面这些更深层的问题：

- ROS2 graph 为什么是观察和调试系统的入口
- topic / service / action 在运行时的差异到底是什么
- QoS 为什么会直接影响系统行为与正确性
- lifecycle、parameter、launch 在系统装配与运行控制中的作用是什么
- executor、callback group、timer、spin 为什么会限制开发上限
- TF、clock、sim_time、bag 在复杂系统中为什么会互相影响，甚至导致实验失真

## 为什么这个模块必须存在

当前已经完成过较系统的 ROS2 / Linux 命令训练，并且已经在工程实践中真实使用过：

- topic / action / param / lifecycle
- launch / bag / TF
- shell / logs / scripts / results / docs

说明表层工具已经不是当前短板。  
真正的短板在于：**这些工具背后的系统机制尚未被系统化语言化**。

如果这一步不补齐，后面会出现这些问题：

- 现象会看，但不会解释行为背后的机制
- 知道哪里报错，但不知道为什么这个机制会导致这种错误
- 看到 QoS / sim_time / callback 这些词仍然容易停留在表面
- 进入源码层时，只能看函数名，不能理解运行语义

## 模块目标

完成本模块后，应逐步具备以下能力：

- 能从 graph 视角看系统，而不是只看单一命令输出
- 能解释 topic、service、action 的运行时差异
- 能解释 QoS 与系统功能异常之间的因果关系
- 能初步理解 executor、callback group、timer、spin 的关系
- 能解释 lifecycle、parameter、launch 在系统装配与运行控制中的意义
- 能说明 use_sim_time、clock、TF、bag 之间的耦合关系与实验风险

## 模块主要产物

本模块后续应重点沉淀：

- ROS2 运行机制总图
- topic / service / action / QoS / lifecycle 对照表
- executor / callback / spin / timer 关系图
- TF + time + bag + launch + param 系统关系图
- runtime failure pattern 清单
- 模块总结
- 模块总验收
- 证据索引

## 学习与执行方式

本模块采用 Sprint 机制推进，每个 Sprint 按以下节奏执行：

1. 问题定义与图谱建立  
2. 运行时证据采集  
3. 文档 / 源码 / 设计资料深读  
4. 对照、trace、profiling 或实验动作  
5. 总结、闭卷与复盘  

每天必须保留：

- 今天的核心机制问题
- 今天抓到的系统证据
- 今天读到的设计或实现要点
- 今天最容易混淆的概念
- 当天闭卷问答
- 明日衔接点

## 验收标准

完成本模块时，至少应满足以下条件：

- 能用运行机制而不是表面现象解释一类系统问题
- 能说清 QoS、lifecycle、graph、sim_time、TF 等机制的作用与边界
- 能初步解释 executor / callback group / spin 的开发意义
- 能把系统错误从“经验问题”上升为“机制问题”
- 能通过闭卷问答形成稳定表达

## 与后续模块的关系

本模块是很多后续模块的基础：

- 为 `04｜C++ / rclcpp / pluginlib 语言门槛突破` 提供运行时语义背景
- 为 `05｜Nav2 源码入口、改造与扩展点` 提供系统行为理解基础
- 为 `06｜评测科学、性能分析与研究实验纪律` 提供 trace、timing、runtime analysis 的机制前提