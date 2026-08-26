# LAB03 — Robot Policy / VLA Action Interface Closed-loop Lab

## Goal
补齐M15/M17理论与真实执行之间的最小闭环：

`Episode / Dataset → Observation-Action Alignment → BC / ACT / Diffusion-style Policy → Raw Action → Decode / De-normalize → Safety Filter → Controller → Robot/Simulation → New Observation → Closed-loop Evaluation`

本LAB不追求训练大模型，也不要求真实VLA foundation model。重点是亲手验证“learned action如何进入机器人闭环”，并看清 Dataset / Policy / Action / Safety / Controller / Physical Motion 的责任边界。

优先Simulation first；若真实robot条件允许，再迁移低速、安全场景。

---

## Minimum Scope
只需选择一个简单连续控制任务，例如：
- 2D point navigation / reaching；
- 简单EEF reaching；
- pick pre-grasp approach；
- simulator中的base velocity或EEF delta control。

Policy可以选择一种：
1. **BC baseline**；
2. **ACT/action-chunk policy**；
3. **Diffusion-style action policy**。

至少完成BC；ACT或Diffusion二选一用于比较即可。

---

## Required Data Contract
每个episode至少包含：
- observation；
- robot state / proprioception；
- action command；
- timestamp；
- task/goal；
- success/failure；
- episode ID。

如果有image：必须记录camera measurement timestamp，而不是只依赖arrival/file time。

必须明确：
`Expert Command ≠ Controller Output ≠ Actual Robot Motion`。

---

## Required Pipeline
1. 收集或生成demonstration episodes；
2. 检查observation/action temporal alignment；
3. 划分train/validation/test，按episode划分避免相邻frame leakage；
4. 定义action representation：joint / EEF delta / base velocity 中一种；
5. 定义normalization、range、rate；
6. 训练BC baseline；
7. 可选训练ACT或Diffusion-style policy；
8. 做offline action metric；
9. 接入action decode / de-normalization；
10. 接入independent safety filter；
11. 接入controller / simulator；
12. closed-loop rollout；
13. 记录actual feedback与task result；
14. 比较offline与closed-loop结果。

---

## Mandatory Safety Filter
至少实现或验证：
- action range limit；
- rate / delta limit；
- workspace或state boundary；
- stale observation timeout；
- policy output NaN/invalid rejection；
- stop/override path。

Safety Filter必须能 **reject / clamp / stop** policy action，不能只记录warning。

---

## Mandatory Fault / Stress Cases
至少覆盖以下5类：
1. **Temporal misalignment**：故意错开observation/action timestamp，观察offline/closed-loop变化；
2. **Distribution shift**：改变start pose、lighting/texture或object位置中的一种；
3. **Action scale error**：normalization/de-normalization错误；
4. **Stale observation**：冻结或延迟输入；
5. **Unsafe raw action**：注入超range/rate action，验证Safety Filter阻止执行。

建议增加：controller saturation、recovery state、action chunk stale。

---

## Evaluation
必须同时报告：

### Offline
- action MSE / token accuracy / likelihood中适用的一种；
- validation/test split；
- action distribution或error histogram。

### Closed-loop
- task success；
- collision / boundary violation；
- intervention / safety-filter trigger；
- recovery；
- completion time；
- observation age / action latency；
- actual feedback vs commanded action。

硬结论：
> **Offline Action Accuracy ≠ Closed-loop Robot Success.**

---

## ACT / Diffusion Optional Comparison
若选择ACT：必须说明 chunk horizon、inference rate、action rate、receding execution、stale chunk handling。

若选择Diffusion：必须说明 conditioning、denoising steps、inference latency、multimodal action advantage。

不要求训练大规模vision-language backbone。

---

## Acceptance Criteria
- BC baseline能完成至少一个closed-loop task；
- 至少一种ACT或Diffusion-style policy完成对比，或在资源不足时完成源码级替代实验并说明限制；
- temporal misalignment、stale observation、unsafe action均能被明确诊断；
- Safety Filter真实阻止至少一次非法action；
- 能从 `Observation → Policy → Raw Action → Decode → Safety → Controller → Feedback → Result` 逐层说明数据、timestamp和failure；
- 报告必须同时包含offline metric和closed-loop metric，不得只用loss证明policy可用。
