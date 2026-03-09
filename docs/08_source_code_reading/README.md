# 08_source_code_reading

## 定位

本目录用于逐步建立 Nav2 / ROS2 相关源码阅读能力。

目标不是一口气读完整个仓库，而是学会从概念、包名、类名、接口和调用关系中定位关键实现。

## 核心目标

逐步建立以下能力：

- 从文档概念映射到源码包
- 看懂包之间的大致职责关系
- 定位关键入口类、节点和接口
- 看懂 README、头文件、实现文件之间的分工
- 对局部调用链建立初步理解
- 为后续插件开发和系统修改打基础

## 学习原则

源码阅读不等于通读源代码。  
这里更强调：

- 先知道为什么读
- 先知道读哪个包
- 先知道要找什么问题
- 从目录、README、头文件开始，而不是一头扎进实现细节

## 建议范围

- nav2_bringup
- nav2_controller
- nav2_planner
- nav2_bt_navigator
- nav2_behavior_tree
- nav2_costmap_2d
- nav2_map_server
- nav2_amcl
- 典型 controller / planner 插件包

## 产出要求

每次阅读，至少留下这些内容：

- 这次看的是哪个包
- 这个包大概负责什么
- 我关心的问题是什么
- 我找到的入口在哪里
- 还有哪些地方没看懂

## 完成标准

完成本阶段时，应至少做到：

- 能说出几个核心包分别负责什么
- 能根据一个概念定位到大概的源码目录
- 能读懂基础级别的头文件与类接口
- 不再把 server、plugin、node、package 混成一锅

## 与后续衔接

本阶段直接支撑：

- `09_plugins_and_customization`
- `11_paper_writing_and_experiments`

源码阅读是从“会用系统”走向“敢碰系统”的分水岭。