# 00｜总览与长期主线定义

## 1. 目录职责

`00_overview` 是整个 `robotics_skillup_lab` 的总控目录。

这个目录不负责记录某一天学了什么，也不负责存放临时实验笔记。  
它只负责五类事情：

1. 定义长期主线  
2. 管理总计划  
3. 管理“计划如何生成”的规则  
4. 维护命名与归档规范  
5. 负责阶段切换与知识收敛  

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

## 5. 计划生成机制（新增的核心规则）

这是整个仓库最关键的一条规则：

> **所有模块都必须按“四层计划结构”执行：**  
> **总计划 → 模块计划 → Sprint 计划 → 每日任务**

不能直接从总计划跳到当天任务。  
必须先把每个模块的计划定清，再进入 Sprint，再进入每天任务。

如果没有这层结构，长期执行几个月到几年之后，几乎必然会跑偏。

---

## 6. 四层计划结构

### 第 1 层：总计划（Master Plan）
总计划在 `docs/00_overview/` 中维护，包括：

- `phase_01_to_06_master_plan.md`
- `phase_07_to_12_master_plan.md`

这一层负责定义：

- 长期主线
- 01–12 模块顺序
- 每个模块的高阶目标
- 每个模块的总时长
- 每个模块的大纲、成果和验收条件

这一层不能写得太细，不负责每天怎么做。

---

### 第 2 层：模块计划（Module Plan）
每个模块正式开始前，必须先写模块计划文件。

推荐统一文件名：

- `01_plan.md`
- `02_plan.md`
- `03_plan.md`
- `04_plan.md`
- `05_plan.md`
- `06_plan.md`
- `07_plan.md`
- `08_plan.md`
- `09_plan.md`
- `10_plan.md`
- `11_plan.md`
- `12_plan.md`

放在对应模块目录根部，例如：

- `docs/01_nav2_foundation_7d/01_plan.md`
- `docs/02_ros2_linux_fundamentals/02_plan.md`

模块计划文件必须回答：

1. 这个模块要补什么能力缺口  
2. 为什么它排在当前这个阶段  
3. 这个模块的总目标是什么  
4. 模块计划包含多少个 Sprint  
5. 每个 Sprint 的主题是什么  
6. 每个 Sprint 的阶段产物是什么  
7. 模块最终成果是什么  
8. 模块总验收标准是什么  

一句话说清：

> 模块计划负责把 master plan 里的“模块大纲”，变成这个模块自己的可执行地图。

---

### 第 3 层：Sprint 计划（Sprint Plan）
有了模块计划之后，才能拆 Sprint。

Sprint 计划推荐文件名：

- `s01_overview.md`
- `s02_overview.md`
- `s03_overview.md`

每个 Sprint 计划文件必须说明：

1. 本 Sprint 的核心问题  
2. 本 Sprint 和模块总目标的关系  
3. 本 Sprint 的 5 天任务主题  
4. 本 Sprint 的关键输入（文档、源码、论文、日志、实验对象）  
5. 本 Sprint 的关键输出（图、表、代码、文档、实验结果）  
6. 本 Sprint 的验收条件  
7. 本 Sprint 第 5 天的总结与闭卷要求  

一句话说清：

> Sprint 计划负责把模块计划里的“阶段目标”，变成连续 5 天的作战安排。

---

### 第 4 层：每日任务（Daily Task）
有了 Sprint 计划，才能拆成每天任务。

每日任务推荐文件名：

- `s01_d01_problem_map.md`
- `s01_d02_runtime_evidence.md`
- `s01_d03_source_reading.md`
- `s01_d04_experiment_or_modification.md`
- `s01_d05_summary_and_exam.md`

每日任务文件必须固定包含以下内容：

1. **当天计划**  
   - 今天要解决什么问题  
   - 这一天在当前 Sprint 中的位置是什么  

2. **当天输入**  
   - 今天要看哪些文档 / 源码 / 论文 / 日志 / 配置 / 数据  

3. **当天输出**  
   - 今天必须产出什么  
   - 例如图、表、笔记、代码、实验结果、总结文档  

4. **当天闭卷测试**  
   - 至少 12 题  
   - 先写你的原始回答  
   - 再补 GPT 修正后的标准回答  

5. **当天复盘**  
   - 今天最容易混淆的概念  
   - 今天还不稳的点  
   - 明天怎么衔接  

一句话说清：

> 每日任务是执行层，但必须严格服从上面的 Sprint 计划和模块计划。

---

## 7. 每个模块的标准执行顺序

以后所有模块统一按下面顺序执行：

### 第一步：先看总计划
明确当前模块在 01–12 主线中的位置。

### 第二步：写模块计划
先产出 `XX_plan.md`，不允许直接开干。

### 第三步：拆 Sprint
先把这个模块拆成若干个 `sXX_overview.md`。

### 第四步：拆每天任务
每天按 Sprint 计划落成 `sXX_dYY_<topic>.md`。

### 第五步：每个 Sprint 结束必须总结
Sprint 第 5 天必须有：

- Sprint 总结
- Sprint 闭卷
- 原始答案
- GPT 修正答案
- 易混概念修正
- 下一 Sprint 衔接点

### 第六步：模块结束必须总验收
模块结束时必须有：

- `module_summary.md`
- `module_final_exam.md`
- `evidence_index.md`

---

## 8. 每个模块计划必须包含的内容

以后任何 `XX_plan.md` 都必须至少有这几块：

1. 模块定位  
2. 模块目标  
3. 当前能力基线与本模块的缺口  
4. 为什么现在学这个模块  
5. 模块总时长  
6. 模块包含多少个 Sprint  
7. 每个 Sprint 的主题  
8. 每个 Sprint 的产出  
9. 模块最终成果  
10. 模块验收标准  
11. 与下个模块的衔接关系  

这条规则是为了防止：

- 任务做着做着变成“随便多看点资料”
- Sprint 内容开始偏离主题
- 某个模块持续几个月后，已经忘了自己为什么在学这个

---

## 9. 每个 Sprint 必须包含的内容

以后任何 `sXX_overview.md` 都必须至少有这几块：

1. Sprint 核心问题  
2. Sprint 与模块目标的关系  
3. Sprint 的 5 天任务安排  
4. Sprint 输入  
5. Sprint 输出  
6. Sprint 验收条件  
7. Sprint 第 5 天的总结和闭卷要求  

这条规则是为了防止：

- Sprint 主题太散
- 每天任务互相脱节
- 5 天下来只做了零散动作，没有阶段收敛

---

## 10. 每日任务文件的固定骨架

以后所有 `sXX_dYY_<topic>.md` 必须统一骨架，至少包含：

### 1）当天计划
- 今天的主题
- 今天的核心问题
- 今天在当前 Sprint 里的位置

### 2）当天输入
- 文档
- 源码
- 论文
- 配置
- 日志
- 实验对象

### 3）当天输出
- 图
- 表
- 代码
- 笔记
- 实验结果
- 局部总结

### 4）闭卷测试
- 至少 12 题
- 你的原始回答
- GPT 修正后的标准回答

### 5）当天复盘
- 最容易混淆的概念
- 今天还不稳的点
- 明天怎么衔接

这一层不能偷懒。  
因为长期执行几年之后，**不统一骨架就一定会乱。**

---

## 11. 统一命名与归档规范

为了保证后续 01–12 全部模块都能长期维护、快速检索、便于脚本处理，仓库统一采用以下命名规则。

### 11.1 总原则

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

### 11.2 模块目录命名

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

### 11.3 模块内固定文件命名

每个模块根目录固定保留以下文件：

- `README.md`
- `01_plan.md` / `02_plan.md` / … / `12_plan.md`
- `module_summary.md`
- `module_final_exam.md`
- `evidence_index.md`

说明：

- `README.md`：模块定位、目标、成果、验收
- `XX_plan.md`：模块计划
- `module_summary.md`：模块总结
- `module_final_exam.md`：模块总闭卷与总问答
- `evidence_index.md`：证据索引

### 11.4 Sprint / Day 文档命名

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

### 11.5 主题文档命名

如果某个模块内需要长期保留主题文档，而不是按 Sprint / Day 组织，统一采用：

- `01_<topic>.md`
- `02_<topic>.md`
- `03_<topic>.md`

例如：

- `01_execution_chain_map.md`
- `02_qos_scenarios.md`
- `03_plugin_loading_path.md`

### 11.6 实验文档命名

统一采用：

- `exp_01_rpp_vs_dwb_map1.md`
- `exp_02_replay_vs_live_map2.md`
- `exp_03_costmap_layer_stress_test.md`

一般公式为：

- `exp_<id>_<method_or_target>_<scene>.md`

### 11.7 图表文件命名

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

### 11.8 脚本文件命名

统一按动作命名：

- `run_<topic>.sh`
- `check_<topic>.sh`
- `analyze_<topic>.py`
- `plot_<topic>.py`
- `summarize_<topic>.py`
- `replay_<topic>.sh`

### 11.9 结果文件命名

结果文件应体现：

- 实验编号
- 方法 / 插件 / 参数组
- 场景 / 地图
- 日期（必要时）

推荐格式：

- `result_exp_01_rpp_map1.csv`
- `result_exp_02_dwb_map2.csv`
- `result_exp_03_plugin_variant_a.json`

---

## 12. 维护原则

这个目录需要尽量保持稳定，不要频繁改。

只有在以下情况才建议修改本目录内容：

- 长期主线发生调整
- 模块顺序或边界发生变化
- 命名规范需要升级
- 计划生成机制需要调整
- 阶段切换方式需要调整
- 新增长期总控型文档

不建议因为某次临时任务、某个局部 bug、某次短期实验，就来改动 `00_overview` 的核心内容。

---

## 13. 这个目录最重要的作用

`00_overview` 的存在意义，不是让仓库看起来更正式，而是防止整个 01–12 路线在推进过程中发生下面这些问题：

- 学了很多，但没有主线
- 写了很多，但无法长期调用
- 做了很多实验，但没有收口
- 总计划、模块计划、Sprint、每日任务混成一锅
- 模块之间没有衔接，知识越来越碎

一句话说清：

> `00_overview` 负责定义主线、控制计划生成、约束命名、维护知识树，是整个 `robotics_skillup_lab` 的总控层。