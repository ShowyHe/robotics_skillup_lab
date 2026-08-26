# M21 — Research Methodology & Capstone

## Module Goal
训练面对没有标准答案的新问题时，把“现象/经验”转化为可验证、可复现、可答辩的研究与工程结论。

主线：`Problem → Scope → Baseline → Literature → Hypothesis → Experiment → Implementation → Evaluation → Ablation → Failure Analysis → Conclusion → Technical Communication`。

本模块共 3 个理论 Day（Day127–Day129）。每个Day核心知识点≤20。

---

# Day127 — Problem Definition / Research Question / Hypothesis / Baseline / Literature
1. 今日目标：把模糊工程现象改写为可测量、可比较、可验证的问题。
2. 前置：M19 evaluation + M20 incident/owner。
3. 必须教学：①Problem Statement：system/scenario/observed problem/expected behavior/current limitation/metric/constraints；②symptom≠problem；③In/Out of Scope；④quantified requirement；⑤real-world constraints；⑥Research Question；⑦Hypothesis；⑧hypothesis必须可证伪；⑨Baseline；⑩strong/fair baseline；⑪existing system first；⑫literature reading目标：problem/method family/assumption/metric/failure；⑬three-pass reading + problem→idea→diagram→method→experiment→limitation→math；⑭contribution类别：algorithm/system/dataset/evaluation/integration；⑮Claim vs Evidence；⑯Related Work按method family/gap组织；⑰source hierarchy；⑱reproducibility questions与engineering gap。
4. 深度：Problem/Scope/Baseline L5；Literature L4。
5. 工程连接：把“窄路有人时机器人仍然会钻”先写成研究问题，不先改代码。
6. 不展开：citation格式/投稿流程。
7. 考核：给模糊现象完成Problem、Scope、Question、Hypothesis、Baseline、Metrics。
8. 毕业考点：Problem Definition、Hypothesis、Baseline、Claim/Evidence。

# Day128 — Experimental Design / Ablation / Statistics / Reproducibility
1. 今日目标：设计真正能检验Hypothesis的实验。
2. 前置：Day127 + M19 regression。
3. 必须教学：①experiment服务hypothesis；②independent/dependent variable；③controlled variable；④confounder；⑤control vs experimental group；⑥scenario matrix：nominal/boundary/failure；⑦repeated trials与random seed；⑧mean/variance/distribution/tail；⑨confidence interval直觉；⑩statistical vs engineering significance；⑪effect size；⑫ablation与核心claim；⑬parameter sweep且不能只报告best；⑭overfit to test scene；⑮development/validation/hold-out；⑯failure/negative result不能随意删；⑰reproducibility package：code/config/model/calibration/dataset/scenario/seed/hardware/commands/metric script；⑱experiment ID + automated metric；human rating仅在需要时定义标准/blind。
4. 深度：Experiment/Ablation/Reproducibility L5；Statistics L3-L4。
5. 工程连接：Current HPA/PathSwitch vs Improved版本测试矩阵。
6. 不展开：完整t-test/ANOVA/Bayesian statistics。
7. 考核：给同时加入TTL/persistence/cooldown/margin的新版本设计ablation与hold-out test。
8. 毕业考点：Controlled Experiment、Confounder、Ablation、Reproducibility。

# Day129 — Technical Report / Capstone Defense / Contribution / Research Owner
1. 今日目标：把复杂Robot项目讲清楚、证据守住、限制说清。
2. 前置：Day127–128 + M00–M20。
3. 必须教学：①technical story：Problem→Baseline→Failure→Design→Implementation→Experiment→Result→Limitation→Conclusion；②1–3句Contribution Statement；③architecture/algorithm/experiment diagram各自表达什么；④result table包含baseline/metric/trials/variation/failures；⑤qualitative video只做quantitative补充；⑥主动展示failure cases；⑦limitation：works under/does not handle/requires/fails when；⑧claim范围不能超过evidence；⑨correlation≠causation；⑩source contribution定位关键files/call path但不按代码量评分；⑪Owner解释在System→Algorithm→Math/Source→Evidence层切换；⑫Why-chain；⑬证据不足时明确“当前不能确定”并给出所需log/experiment；⑭technical debt；⑮reproducibility：build/run/reproduce/report；⑯README vs formal report；⑰defense question types与fair comparison；⑱research vs engineering + capstone评分看problem/theory/system/evidence/failure/owner；最终Robot chain与Research chain。
4. 深度：Contribution/Evidence/Defense/Limitation L5。
5. 工程连接：Navigation Owner、Mobile Manipulation、VLA Hybrid三类Capstone候选；优先把LAB02 Research Extension作为最终Capstone承载体。
6. 不展开：PPT视觉设计、英语写作、patent/publication strategy。
7. 考核：答辩“为什么不用更大VLA模型”“怎么证明改动有效”“什么场景会失败”。
8. 毕业考点：Contribution、Evidence、Limitation、Technical Defense。

---

# M21 Graduation Exam
统一权重：**30%核心基础 / 50%综合系统场景 / 20% Source·Formula·Design**。

## 30% 核心基础
硬门槛：Problem Statement、Research Question、Hypothesis、Baseline、Claim/Evidence、Confounder、Ablation、Reproducibility、Failure Analysis、Limitation、Contribution、Technical Defense。

## 50% 综合系统场景
至少覆盖：把“人多时走不好”改成完整研究问题；baseline/new但场景不公平识别confounder；多机制同时加入后的ablation；VLA offline好但robot差提出可证伪hypotheses；一段成功视频不能支持哪些claim；Capstone defense公平比较algorithm/compute/data/safety/evidence。

## 20% Source / Formula / Design
为真实机器人问题写Research Plan：Title/Problem/Baseline/Hypothesis/Method/Metrics/Scenarios/Ablation/Acceptance Criteria/Risks/Reproducibility，并完成Evidence Audit。

## 通过标准
总分≥85%；必须把模糊现象变成可证伪问题；改善必须相对公平baseline；实验必须有controlled variables、repeat、ablation和regression；证据不足时必须明确“当前不能确定”。
