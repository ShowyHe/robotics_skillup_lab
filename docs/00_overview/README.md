# 00｜总览与长期主线定义

## 1. 目录职责

`00_overview` 是整个 `robotics_skillup_lab` 的总控目录。

这个目录不负责记录某一天学了什么，也不负责存放临时实验笔记。  
它只负责四类事情：

1. 定义长期主线  
2. 管理总计划  
3. 维护规则与命名规范  
4. 负责阶段切换与知识收敛  

一句话说清：

> 这里是仓库的“总控台”，不是“施工现场”。

---

## 2. 长期主线

整个仓库服务的长期主线如下：

> 成为机器人领域兼具系统开发、架构设计、实验验证和研究创新能力的核心技术人员，并最终产出具有工程价值和学术影响力的高水平成果。

这条主线意味着：

- 不能只停留在调试和交付支持层
- 不能只停留在跑 demo 和改配置层
- 不能只停留在会复现而不会抽象问题层
- 也不能脱离真实系统，只做空泛研究

后续所有计划、模块和阶段切换，都必须围绕这条主线判断是否成立。

---

## 3. 本目录里应该放什么

本目录只放“总控型文档”。

建议长期保留的内容包括：

- 总览 README
- `phase_01_to_06_master_plan.md`
- `phase_07_to_12_master_plan.md`
- 年度路线图
- 阶段切换判断文档
- 统一命名与归档规范
- 长期知识树总图
- 阶段归纳总入口

本目录不建议放：

- 单日学习记录
- 某次临时实验结果
- 临时命令草稿
- 一次性杂项笔记
- 某个模块的具体执行细节

这些内容应当放在对应模块目录中。

---

## 4. 两份 master plan 的职责

本目录中的两份总计划文件分别承担不同阶段的职责。

### `phase_01_to_06_master_plan.md`
负责前半程路线，解决的是：

- 如何从当前能力基线继续上探
- 如何把现有工程能力提升成系统架构与运行机制理解
- 如何建立实验基础设施
- 如何拿到 C++ / rclcpp / pluginlib 门票
- 如何进入 Nav2 源码、扩展点与研究级评测底盘

它对应的关键词是：

- 架构
- 机制
- 基础设施
- 源码入口
- 扩展点
- 评测纪律

### `phase_07_to_12_master_plan.md`
负责后半程路线，解决的是：

- 如何从系统核心开发继续上探
- 如何建立大代码库架构理解
- 如何进入高级插件与系统定制设计
- 如何构建文献矩阵、baseline 地图与高质量复现
- 如何形成原创方法、论文叙事与长期知识系统

它对应的关键词是：

- 架构重建
- 高级扩展
- baseline
- 复现
- 原创方法
- 论文管线
- 总归纳

---

## 5. 计划窗口与执行窗口分工

为了避免整个仓库越做越乱，必须区分两个层级。

### 5.1 计划窗口
计划窗口负责：

- 定义模块顺序
- 调整周期与目标
- 定义阶段边界
- 调整总验收标准
- 决定下一模块方向

计划窗口不负责：

- 写当天执行细节
- 记临时命令
- 存中间实验草稿

### 5.2 执行窗口
执行窗口负责：

- 当天任务拆解
- 当天闭卷题
- 当天 md
- 当天代码 / 实验 / 图表
- 当天批改与修正

执行窗口不负责：

- 临时推翻总路线
- 私自改模块顺序
- 把日常草稿写进总计划文件

---

## 6. 统一命名与归档规范

为了保证后续 01–12 全部模块都能长期维护、快速检索、便于脚本处理，仓库统一采用以下命名规则。

### 6.1 总原则

所有文件统一遵循以下规则：

- 文件名只使用：
  - 小写字母 `a-z`
  - 数字 `0-9`
  - 下划线 `_`
- 不使用：
  - 中文文件名
  - 空格
  - 括号
  - 特殊符号
- 文件名应满足：
  - 排序稳定
  - 终端易输入
  - 便于 `find`、`grep`、脚本批处理
  - 便于长期归档

### 6.2 模块目录命名

模块目录统一采用：

- `00_overview`
- `01_nav2_foundation_7d`
- `02_ros2_linux_fundamentals`
- `03_python_for_robotics`
- `04_cpp_for_ros2`
- `05_nav2_engineering_drills`
- `06_tooling_and_automation`
- `07_testing_evaluation_and_debugging`
- `08_source_code_reading`
- `09_plugins_and_customization`
- `10_research_reproduction`
- `11_paper_writing_and_experiments`
- `12_portfolio_and_interview`

说明：

- 前缀两位编号 `00–12` 保证排序稳定
- 目录名保持历史连续性
- README 标题与总计划标题可以使用更正式、更高阶的中文表述

### 6.3 模块内固定文件命名

每个模块根目录固定保留以下文件：

- `README.md`
- `module_summary.md`
- `module_final_exam.md`
- `evidence_index.md`

说明：

- `README.md`：模块定位、目标、成果、验收
- `module_summary.md`：模块总结
- `module_final_exam.md`：模块总闭卷与总问答
- `evidence_index.md`：证据索引

### 6.4 Sprint / Day 文档命名

统一推荐：

- `s01_overview.md`
- `s01_d01_problem_map.md`
- `s01_d02_runtime_evidence.md`
- `s01_d03_source_reading.md`
- `s01_d04_experiment_or_modification.md`
- `s01_d05_summary_and_exam.md`

一般公式为：

- `sXX_dYY_<topic>.md`

其中：

- `sXX`：Sprint 编号，两位数
- `dYY`：Day 编号，两位数
- `<topic>`：主题关键词

如果是 Sprint 总览或 Sprint 总结，则用：

- `sXX_overview.md`
- `sXX_summary.md`

### 6.5 主题文档命名

如果某个模块内需要长期保留主题文档，而不是按 Sprint / Day 组织，统一采用：

- `01_<topic>.md`
- `02_<topic>.md`
- `03_<topic>.md`

例如：

- `01_execution_chain_map.md`
- `02_qos_scenarios.md`
- `03_plugin_loading_path.md`

一般公式为：

- `NN_<topic>.md`

### 6.6 实验文档命名

统一采用：

- `exp_01_rpp_vs_dwb_map1.md`
- `exp_02_replay_vs_live_map2.md`
- `exp_03_costmap_layer_stress_test.md`

一般公式为：

- `exp_<id>_<method_or_target>_<scene>.md`

### 6.7 图表文件命名

统一采用：

- `fig_01_execution_chain.png`
- `fig_02_qos_dependency_map.png`
- `fig_03_ablation_results.png`

- `tbl_01_metric_definition.csv`
- `tbl_02_baseline_comparison.csv`
- `tbl_03_failure_taxonomy.csv`

一般公式为：

- `fig_<id>_<topic>.<ext>`
- `tbl_<id>_<topic>.<ext>`

### 6.8 脚本文件命名

统一按动作命名：

- `run_<topic>.sh`
- `check_<topic>.sh`
- `analyze_<topic>.py`
- `plot_<topic>.py`
- `summarize_<topic>.py`
- `replay_<topic>.sh`

例如：

- `run_nav2_regression.sh`
- `check_qos_state.sh`
- `analyze_benchmark_results.py`
- `plot_ablation_curves.py`
- `summarize_failure_cases.py`

### 6.9 结果文件命名

结果文件应体现：

- 实验编号
- 方法 / 插件 / 参数组
- 场景 / 地图
- 日期（必要时）

推荐格式：

- `result_exp_01_rpp_map1.csv`
- `result_exp_02_dwb_map2.csv`
- `result_exp_03_plugin_variant_a.json`

如需时间戳，统一追加到末尾：

- `result_exp_03_plugin_variant_a_2026_03_10.csv`

---

## 7. 维护原则

这个目录需要尽量保持稳定，不要频繁改。

只有在以下情况才建议修改本目录内容：

- 长期主线发生调整
- 模块顺序或边界发生变化
- 命名规范需要升级
- 阶段切换方式需要调整
- 新增长期总控型文档

不建议因为某次临时任务、某个局部 bug、某次短期实验，就来改动 `00_overview` 的核心内容。

---

## 8. 这个目录最重要的作用

`00_overview` 的存在意义，不是让仓库看起来更正式，而是防止整个 01–12 路线在推进过程中发生下面这些问题：

- 学了很多，但没有主线
- 写了很多，但无法长期调用
- 做了很多实验，但没有收口
- 计划和执行混在一起，后面全乱
- 模块之间没有衔接，知识越来越碎

一句话说清：

> `00_overview` 负责定义主线、收口路线、约束命名、维护知识树，是整个 `robotics_skillup_lab` 的总控层。