# 02_ros2_linux_fundamentals

## 定位

本目录用于夯实 ROS2 与 Linux 基础，使后续 Nav2、自动化、源码阅读、插件开发不再建立在松软地基上。

很多看起来像 Nav2 的问题，实际根源往往在 ROS2 机制理解不稳，或者 Linux 命令、路径、环境、进程、日志处理不熟。

## 核心目标

建立以下基础能力：

- 能稳定使用 ROS2 CLI
- 能理解 node、topic、service、action、param、lifecycle、TF 等基本机制
- 能理解 launch、workspace、colcon、环境变量等工作流
- 能处理 Linux 文件、进程、权限、管道、重定向、日志与脚本
- 能用 git 完成基本版本管理与提交

## 为什么这一层必须单独存在

如果这层不稳，后面会反复出现以下情况：

- 命令会敲，但不知道为什么这样敲
- 看到 topic、action、param 输出，不知道它在说明什么
- 启动失败时，只会重跑，不会判断问题在哪
- 看不懂终端输出、找不到日志、不会组织排查顺序
- 学到后面一堆概念，最后全是半懂不懂

所以这层不是“基础补课”，而是整个技术路径的底盘。

## 学习范围

建议逐步覆盖：

- ROS2 CLI
- Node / Topic / Service / Action / Param
- Lifecycle
- TF
- Launch
- Bag
- Colcon Workspace
- Linux 命令行
- Shell 基础
- Git 基础工作流

## 产出形式

本目录后续应以“主题 + 命令 + 解释 + 证据 + 常见坑 + 修正理解”的方式组织内容，而不是只堆教程链接。

## 完成标准

完成本阶段时，应至少做到：

- 能解释最常用 ROS2 CLI 的目的和输出意义
- 能独立排查一类基础启动或通信问题
- 能把 Linux / ROS2 基础命令写成自己可复用的检查流程
- 能在不查资料的情况下说清几个核心概念的区别

## 与后续衔接

本阶段支撑：

- `03_python_for_robotics`
- `04_cpp_for_ros2`
- `05_nav2_engineering_drills`
- `06_tooling_and_automation`

没有这层，后面脚本和工程能力都会虚。