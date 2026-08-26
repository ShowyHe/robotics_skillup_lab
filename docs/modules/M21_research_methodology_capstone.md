# M21 — Research Methodology & Capstone

## Module Goal
训练面对没有标准答案的新问题时，将“现象/经验”转化为可验证、可复现、可答辩的研究与工程结论。

主线：`Problem → Scope → Baseline → Literature → Hypothesis → Experiment → Implementation → Evaluation → Ablation → Failure Analysis → Conclusion → Technical Communication`。

本模块共 3 个理论 Day（Day127–Day129）。

---

# Day127 — Problem Definition / Research Question / Hypothesis / Baseline / Literature
1. 今日目标：把模糊工程现象改写为可测量、可比较、可验证的问题。
2. 前置：M19 evaluation + M20 incident/owner。
3. 必须教学：Problem Statement包含system/scenario/observed problem/expected behavior/current limitation/metric/constraints；symptom≠problem definition；In/Out of Scope；quantified requirement；real-world constraints；Research Question；Hypothesis；hypothesis必须可证伪；Baseline；strong/fair baseline；existing system first；literature reading目标：problem/method family/assumption/metric/failure；three-pass reading；paper reading order；contribution类别：algorithm/system/dataset/evaluation/integration；Claim vs Evidence；Related Work按method family/gap组织；source hierarchy；reproducibility questions；engineering gap。
4. 深度：Problem/Scope/Baseline L5；literature L4。
5. 工程连接：将“窄路有人时机器人仍然会钻”先写成研究问题，不先改代码。
6. 不展开：citation格式/投稿流程。
7. 考核：给模糊现象完成Problem、Scope、Question、Hypothesis、Baseline、Metrics。
8. 毕业考点：Problem Definition、Hypothesis、Baseline、Claim/Evidence。

# Day128 — Experimental Design / Ablation / Statistics / Reproducibility
1. 今日目标：设计真正能够检验Hypothesis的实验。
2. 前置：Day127 + M19 regression。
3. 必须教学：experiment服务hypothesis；independent/dependent variable；controlled variables；confounder；control vs experimental group；scenario matrix；nominal/boundary/failure cases；repeated trials；random seed；mean/variance/distribution/tail；confidence interval直觉；statistical vs engineering significance；effect size；ablation；parameter sweep；不能只报告best setting；overfit to test scene；development/validation/hold-out；failure case不能随意删；reproducibility package：code/config/model/calibration/dataset/scenario/seed/hardware/commands/metric script；experiment ID；automated metrics优先；human rating需标准/blind；negative result也必须报告。
4. 深度：Experiment/Ablation/Reproducibility L5；statistics L3-L4。
5. 工程连接：Current HPA/PathSwitch vs Improved版本测试矩阵。
6. 不展开：完整统计学课程。
7. 考核：给同时加入TTL/persistence/cooldown/margin的新版本设计ablation。
8. 毕业考点：Controlled Experiment、Confounder、Ablation、Reproducibility。

# Day129 — Technical Report / Capstone Defense / Contribution / Research Owner
1. 今日目标：把复杂Robot项目讲清楚、证据守住、限制说清。
2. 前置：Day127–128 + M00–M20。
3. 必须教学：technical story Problem→Baseline→Failure Analysis→Design→Implementation→Experiment→Result→Limitation→Conclusion；1–3句Contribution Statement；architecture diagram表达module/data flow/boundary；algorithm diagram；experiment diagram；result table应有baseline/metric/trials/variation/failures；qualitative video/trajectory是定量补充；主动展示failure cases；limitation `works under / does not handle / requires / fails when`；claim范围不能超过evidence；correlation≠causation；source contribution定位关键files/call path但不按代码量评分；Owner解释需在System/Algorithm/Math-Source-Evidence三层切换；“Why chain”；证据不足时明确不能确定并指出所需log/experiment；technical debt；reproducibility run/build/report；README vs formal report；defense question types；fair comparison；research vs engineering；capstone评分看problem/theory/system/evidence/failure/owner；final Robot full-stack chain与research chain。
4. 深度：Contribution/Evidence/Defense/Limitations L5。
5. 工程连接：Navigation Owner、Mobile Manipulation、VLA Hybrid 三类Capstone候选。
6. 不展开：PPT视觉设计、英语写作、patent/publication strategy。
7. 考核：现场答辩“为什么不用更大VLA模型”“你怎么证明改动有效”“什么场景会失败”。
8. 毕业考点：Contribution、Evidence、Limitation、Technical Defense。

---

# M21 Graduation Exam
统一权重：**30%核心基础 / 50%综合系统场景 / 20% Source·Formula·Design**。

## 30% 核心基础
硬门槛：Problem Statement、Research Question、Hypothesis、Baseline、Claim/Evidence、Confounder、Ablation、Reproducibility、Failure Analysis、Limitation、Contribution、Technical Defense。

## 50% 综合系统场景
至少覆盖：
1. 把“人多时走不好”改写为完整研究问题；
2. baseline 80/100 vs new 88/100但测试场景更宽，识别confounder；
3. 四功能同时加入后的ablation；
4. VLA offline96%但robot58%：提出多个可证伪hypotheses与实验；
5. 一段成功视频不能支持哪些claim；
6. Capstone defense时对algorithm/compute/data/safety/evidence做公平比较。

## 20% Source / Formula / Design
为真实机器人问题写Research Plan：Title/Problem/Baseline/Hypothesis/Method/Metrics/Scenarios/Ablation/Acceptance Criteria/Risks/Reproducibility，并完成Evidence Audit。

## 通过标准
总分≥85%；必须能够把模糊现象变成可证伪问题；改善必须相对公平baseline；实验必须有controlled variables、repeat、ablation和regression；证据不足时必须明确“当前不能确定”。

## Day127–Day129 索引
```text
Day127 Problem Definition / Hypothesis / Baseline / Literature
Day128 Experimental Design / Ablation / Statistics / Reproducibility
Day129 Technical Report / Capstone Defense / Contribution / Research Owner
```
