# 00｜总览与长期主线定义

## 1. 目录职责

`00_overview` 是整个 `robotics_skillup_lab` 的总控目录。

这个目录不负责记录某一天学了什么，也不负责存放临时实验笔记。  
它只负责六类事情：

1. 定义长期主线  
2. 管理总计划  
3. 规定“计划如何生成”  
4. 规定各层级文档应该写什么  
5. 维护命名与归档规范  
6. 负责阶段切换与知识收敛  

一句话说清：

> 这里是仓库的总控台，不是执行现场。

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

补充说明：

本仓库中，01–12 表示 12 个成长阶段；每个阶段在执行与归档时各自对应一个独立模块。  
因此，阶段编号与模块编号保持一一对应，但“阶段”强调成长主线位置，“模块”强调该阶段对应的具体学习与产出体系。

---

## 3. 本目录里应该放什么

本目录只放“总控型文档”和“计划型文档”。

建议长期保留的内容包括：

- 本 README
- `phase_01_to_06_master_plan.md`
- `phase_07_to_12_master_plan.md`
- 年度路线图
- 阶段切换判断文档
- 命名与归档规范
- 长期知识树总图
- 阶段归纳总入口
- 各 phase 的计划归档目录，例如：
  - `01_phase_plan_and_sprint_overview/`
  - `02_phase_plan_and_sprint_overview/`
  - ...
  - `12_phase_plan_and_sprint_overview/`

其中，每个 `xx_phase_plan_and_sprint_overview/` 目录专门用于集中存放该 phase 的：

- `XX_plan.md`
- `sXX_overview.md`

一句话说清：

> `00_overview` 不只是总控台，也是所有“计划层文档”的集中归档区。

本目录不建议放：

- 单日学习记录
- 某次临时实验结果
- 临时命令草稿
- 一次性杂项笔记
- 某个模块的具体执行细节

这些内容应当放在对应模块目录中。

---

## 4. 计划系统的完整层级

整个仓库必须严格按以下层级生成和执行计划：

### 第 0 层：仓库首页导航
文件：
- 根目录 `README.md`

作用：
- 说明仓库定位
- 提供 01–12 模块总览
- 引导进入 `docs/00_overview/`

---

### 第 1 层：总控规则页
文件：
- `docs/00_overview/README.md`

作用：
- 定义计划生成机制
- 规定各层级职责
- 规定命名与归档规范
- 规定总结、验收和证据链规则

---

### 第 2 层：总计划（Master Plan）
文件：
- `phase_01_to_06_master_plan.md`
- `phase_07_to_12_master_plan.md`

作用：
- 定义 01–06 / 07–12 模块顺序
- 说明每个模块的高层目标
- 说明模块间衔接关系
- 说明每个模块的大纲、成果和验收条件

---

### 第 3 层：模块计划（Module Plan）
文件：
- `01_plan.md`
- `02_plan.md`
- ...
- `12_plan.md`

放置位置：
- `docs/00_overview/xx_phase_plan_and_sprint_overview/`
- 例如：
  - `docs/00_overview/01_phase_plan_and_sprint_overview/01_plan.md`
  - `docs/00_overview/02_phase_plan_and_sprint_overview/02_plan.md`

作用：
- 把总计划中的“模块大纲”，变成模块级作战地图

---

### 第 4 层：Sprint 计划（Sprint Overview）
文件：
- `s01_overview.md`
- `s02_overview.md`
- ...

放置位置：
- 对应 phase 的计划目录中，即：
  - `docs/00_overview/xx_phase_plan_and_sprint_overview/`

例如：
- `docs/00_overview/01_phase_plan_and_sprint_overview/s01_overview.md`
- `docs/00_overview/01_phase_plan_and_sprint_overview/s02_overview.md`

作用：
- 把模块计划中的某个阶段目标，变成一个 5 天 Sprint 的执行安排

---

### 第 5 层：每日任务（Daily Task）
文件：
- `s01_d01_problem_map.md`
- `s01_d02_runtime_evidence.md`
- `s01_d03_source_reading.md`
- `s01_d04_experiment_or_modification.md`
- `s01_d05_summary_and_exam.md`

放置位置：
- 对应模块目录中，而不是 `00_overview`
- 例如：
  - `docs/01_nav2_foundation/`
  - `docs/02_ros2_linux_fundamentals/`

作用：
- 真正执行当天任务
- 记录输入、输出、闭卷、原始答案、GPT 修正答案、复盘

---

### 第 6 层：模块收尾与总验收
文件：
- `module_summary.md`
- `module_final_exam.md`
- `evidence_index.md`

放置位置：
- 对应模块目录中

作用：
- 模块总结
- 模块总闭卷与总问答
- 证据索引与收口

---

## 5. 计划生成规则

所有模块都必须按以下顺序生成计划：

### 第一步：先看总计划
先看本模块所在阶段对应的 master plan，确认：
- 为什么现在轮到这个模块
- 它在 01–12 主线中的位置
- 它补的是哪一类能力缺口

### 第二步：写模块计划
先产出 `XX_plan.md`，不允许直接进入 Sprint 和每天任务。

模块计划统一存放到：
- `docs/00_overview/xx_phase_plan_and_sprint_overview/`

### 第三步：拆 Sprint
模块计划写完后，再拆成多个 `sXX_overview.md`。

Sprint 计划统一存放到：
- `docs/00_overview/xx_phase_plan_and_sprint_overview/`

### 第四步：拆每天任务
Sprint 计划写完后，再拆成每天任务文档。

每日任务统一存放到：
- 对应模块目录中

### 第五步：Sprint 收口
每个 Sprint 第 5 天必须做总结和闭卷。

### 第六步：模块收口
模块结束时必须做模块总结、模块总验收和证据索引。

一句话说清：

> 总计划不直接生成每日任务，中间必须经过模块计划和 Sprint 计划；其中计划层文档统一归档到 `00_overview`，执行层文档保留在各模块目录。

---

## 6. 每一层计划“根据什么”来制定

这是最关键的规则。  
每一层不是凭感觉写，而是必须有明确依据。

### 6.1 总计划根据什么制定
总计划必须根据：

1. 长期职业主线  
2. 当前能力基线  
3. 01–12 的能力爬坡逻辑  
4. 每个模块对前后模块的支撑关系  

也就是说，总计划决定的是“先学什么、后学什么、为什么这样排”。

---

### 6.2 模块计划根据什么制定
每个 `XX_plan.md` 必须根据：

1. 对应阶段的 master plan  
2. 当前模块所在位置  
3. 前一个模块的输出与不足  
4. 当前模块要补的能力缺口  
5. 当前模块的总时长与难度  

也就是说，模块计划决定的是“这个模块几周 / 几个月到底在补什么”。

---

### 6.3 Sprint 计划根据什么制定
每个 `sXX_overview.md` 必须根据：

1. 模块计划中的 Sprint 主题  
2. 当前 Sprint 与模块总目标的关系  
3. 上一个 Sprint 的输出与遗留问题  
4. 本 Sprint 计划解决的单一核心问题  

也就是说，Sprint 计划决定的是“这 5 天到底集中解决什么”。

---

### 6.4 每日任务根据什么制定
每个 `sXX_dYY_<topic>.md` 必须根据：

1. 所属 Sprint 的 `sXX_overview.md`  
2. 当前天在 5 天节奏中的位置  
3. 前一天输出的结果  
4. 本天要完成的单一推进任务  

也就是说，每日任务决定的是“今天具体做什么、输入什么、输出什么”。

---

## 7. 每个模块计划必须包含的内容

以后任何 `XX_plan.md` 都必须至少有这几块：

1. 模块定位  
2. 当前能力基线  
3. 本模块要补的缺口  
4. 为什么现在学这个模块  
5. 模块总目标  
6. 模块总时长  
7. 模块包含多少个 Sprint  
8. 每个 Sprint 的主题  
9. 每个 Sprint 的阶段产出  
10. 模块最终成果  
11. 模块验收标准  
12. 与下个模块的衔接关系  

这条规则是为了防止：

- 任务做着做着变成“随便多看点资料”
- Sprint 内容开始偏离主题
- 某个模块持续几个月后，已经忘了自己为什么在学这个

---

## 8. 每个 Sprint 计划必须包含的内容

以后任何 `sXX_overview.md` 都必须至少有这几块：

1. Sprint 核心问题  
2. Sprint 与模块目标的关系  
3. Sprint 的 5 天任务安排  
4. Sprint 输入  
5. Sprint 输出  
6. Sprint 验收条件  
7. Day 5 的总结与闭卷要求  

这条规则是为了防止：

- Sprint 主题太散
- 每天任务互相脱节
- 5 天下来只做了零散动作，没有阶段收敛

---

## 9. 每日任务文件必须包含的内容

以后任何 `sXX_dYY_<topic>.md` 都必须至少有这几块：

1. 当天计划  
2. 当天输入  
3. 当天输出  
4. 当天闭卷测试  
5. 我的原始回答  
6. GPT 修正后的标准回答  
7. 当天复盘  

其中：

### 当天计划
必须写清：
- 今天的主题
- 今天的核心问题
- 今天在当前 Sprint 中的位置

### 当天输入
必须写清：
- 今天看的文档 / 源码 / 论文 / 配置 / 日志 / 数据

### 当天输出
必须写清：
- 今天必须完成的图、表、笔记、代码、实验结果、总结片段

### 当天闭卷测试
必须满足：
- 至少 12 题
- 先写我的原始回答
- 再写 GPT 修正后的标准回答

### 当天复盘
必须写清：
- 今天最容易混淆的概念
- 今天还不稳的点
- 明天怎么衔接

这一层不能偷懒。  
因为长期执行几年之后，**不统一骨架就一定会乱。**

---

## 10. 每个 Sprint 和模块如何收尾

### 10.1 每个 Sprint 的收尾
每个 Sprint 第 5 天必须完成：

- Sprint 总结
- 至少 12 题闭卷
- 我的原始回答
- GPT 修正后的标准回答
- 易混概念修正
- 下一 Sprint 衔接说明

### 10.2 每个模块的收尾
每个模块结束时必须完成：

- `module_summary.md`
- `module_final_exam.md`
- `evidence_index.md`

模块总验收必须回答：

1. 我现在能解释什么  
2. 我现在能做什么  
3. 我现在能证明什么  
4. 我现在还不能做什么  
5. 下个模块为什么成立  

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
- `01_nav2_foundation`
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

### 11.3 phase 计划目录命名

为了集中管理各模块的计划层文档，`docs/00_overview/` 下统一建立 phase 计划目录。

命名统一采用：

- `01_phase_plan_and_sprint_overview`
- `02_phase_plan_and_sprint_overview`
- `03_phase_plan_and_sprint_overview`
- ...
- `12_phase_plan_and_sprint_overview`

说明：

- `01`–`12` 对应模块编号
- `phase_plan_and_sprint_overview` 表示该目录只存放该模块的计划层文档
- 目录名统一使用小写字母、数字和下划线，不使用大写字母

### 11.4 模块内固定文件命名

每个模块根目录固定保留以下文件：

- `README.md`
- `module_summary.md`
- `module_final_exam.md`
- `evidence_index.md`

说明：

- `XX_plan.md` 和 `sXX_overview.md` 不再放在模块目录
- 这两类文件统一放在 `docs/00_overview/xx_phase_plan_and_sprint_overview/`

### 11.5 phase 计划目录内固定文件命名

每个 `xx_phase_plan_and_sprint_overview/` 目录固定保留：

- 与目录编号一致的模块计划文件，例如：
  - `01_phase_plan_and_sprint_overview/` 内固定为 `01_plan.md`
  - `02_phase_plan_and_sprint_overview/` 内固定为 `02_plan.md`
- 该模块下的 Sprint 计划文件：
  - `s01_overview.md`
  - `s02_overview.md`
  - `s03_overview.md`
  - ...

不允许在 phase 计划目录内混放：
- 每日任务文档
- 模块总结文档
- 临时实验记录
- 一次性草稿

### 11.6 实验文档命名

统一采用：

- `exp_01_rpp_vs_dwb_map1.md`
- `exp_02_replay_vs_live_map2.md`
- `exp_03_costmap_layer_stress_test.md`

### 11.7 图表文件命名

统一采用：

- `fig_01_execution_chain.png`
- `fig_02_qos_dependency_map.png`
- `fig_03_ablation_results.png`

- `tbl_01_metric_definition.csv`
- `tbl_02_baseline_comparison.csv`
- `tbl_03_failure_taxonomy.csv`

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

### 11.10 模块目录与计划目录的链接规则

为了保证“计划层”和“执行层”分离后仍然可快速跳转，每个模块目录下的 `README.md` 必须提供对应计划入口。

统一包含：

- 对应模块计划 `XX_plan.md` 的相对路径
- 对应 Sprint 计划目录的相对路径

例如，在 `docs/01_nav2_foundation/README.md` 中应提供：

- `../00_overview/01_phase_plan_and_sprint_overview/01_plan.md`
- `../00_overview/01_phase_plan_and_sprint_overview/`

---

## 12. 维护原则

这个目录需要尽量保持稳定，不要频繁改。

只有在以下情况才建议修改本目录内容：

- 长期主线发生调整
- 模块顺序或边界发生变化
- 计划生成机制需要升级
- 命名规范需要调整
- 阶段切换方式需要变化
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