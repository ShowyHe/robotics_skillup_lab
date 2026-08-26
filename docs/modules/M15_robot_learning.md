# M15 — Robot Learning

## Module Goal
把已有 RL 经验重新系统化，但主线明确放在“机器人动作如何从 demonstrations / dataset 学出来”，而不是重新刷完整 RL 算法全集。

核心链：`MDP / Policy → RL复盘 → BC / IL / DAgger → Robot Dataset / Action → ACT / Action Chunking → Diffusion Policy → Offline RL / OOD → Closed-loop Evaluation / Sim2Real`。

本模块共 8 个理论 Day（Day97–Day104）。

---

# Day97 — MDP / Return / V-Q-A / Bellman
1. 今日目标：建立序列决策统一语言。
2. 前置：M06 + M08 + M12 action语义。
3. 必须教学：MDP `(S,A,P,R,γ)`；Markov property；state vs observation；deterministic/stochastic policy；trajectory；reward vs return；discount；`V^π`、`Q^π`、`A=Q-V`；Bellman expectation/optimality；reward是task proxy，可能reward hacking；terminal vs truncation对bootstrap的基本区别。
4. 深度：MDP/V-Q-A/Bellman L4。
5. 工程连接：navigation/continuous action/task reward。
6. 不展开：完整DP算法。
7. 考核：return手算、V/Q/A区别、terminal/truncation判断。
8. 毕业考点：MDP、Return、V/Q/A、Bellman。

# Day98 — PPO / TD3 / On-policy vs Off-policy Review
1. 今日目标：把已有PPO/TD3经验放进统一Actor-Critic和data-reuse框架。
2. 前置：Day97 + M06 backprop。
3. 必须教学：value vs policy-based；policy gradient intuition；Actor/Critic；on/off-policy；PPO ratio `π_new/π_old`、clip、advantage/GAE概念；Replay Buffer；DDPG结构；TD3 twin critics/min target、delayed actor update、target policy smoothing；continuous action；sample efficiency vs exploration risk；SAC maximum-entropy仅作扩展概念。
4. 深度：PPO/TD3 Review L3-L4；On/Off-policy L4。
5. 工程连接：已有DQN/PPO/TD3经验校准。
6. 不展开：DQN全集、SAC完整推导、TRPO。
7. 考核：比较PPO/TD3的数据复用、探索和continuous action。
8. 毕业考点：On/Off-policy、Actor-Critic、PPO/TD3核心机制。

# Day99 — Imitation Learning / BC / DAgger / Covariate Shift
1. 今日目标：理解有expert demonstrations时如何直接学习policy，以及BC为何closed-loop逐步偏离。
2. 前置：M06 supervised learning + Day97。
3. 必须教学：IL；dataset `D={(o,a)}`；BC；continuous action loss；covariate shift `d_expert≠d_π`；compounding error；single-step accuracy≠trajectory success；DAgger流程；expert来源；recovery data；dataset quality；action representation决定learning target。
4. 深度：BC/Covariate Shift L4；DAgger L3-L4。
5. 工程连接：teleop→record→BC→deploy。
6. 不展开：GAIL/inverse RL。
7. 考核：validation MSE低但closed-loop越走越偏的原因。
8. 毕业考点：BC、Covariate Shift、DAgger。

# Day100 — Robot Dataset / Episode / Temporal Alignment / Action Semantics
1. 今日目标：理解机器人学习数据是带时间的episode，而不是独立图片。
2. 前置：M03 timestamp + M12 action semantics + Day99。
3. 必须教学：episode `(o_t,s_t,a_t)` sequence；instruction/metadata；camera/joint/action/gripper timestamp；measurement vs arrival time；teleop command vs actual motion；observation-action delay；nearest/interpolation alignment；joint pos/vel/torque、EEF delta、base velocity；absolute vs delta；normalization/clipping；success/failure/recovery data；episode-level split；same-trajectory leakage；dataset diversity/coverage。
4. 深度：Temporal Alignment/Action Semantics L5。
5. 工程连接：rosbag→episode/data pipeline。
6. 不展开：特定RT-X/OXE schema。
7. 考核：给image/action延迟判断training mapping错误。
8. 毕业考点：Robot Dataset、Timestamp Alignment、Action Representation。

# Day101 — ACT / Action Chunking / CVAE / Temporal Aggregation
1. 今日目标：理解ACT作为代表性action-chunking transformer为什么不仅是“预测多步action”。
2. 前置：Day99–100 + M06 Transformer。
3. 必须教学：single-step vs chunk `A_t=[a_t...a_(t+H-1)]`；chunk horizon；Transformer根据observation/state context一次预测action sequence；ACT中的CVAE思想：training时用future action chunk编码latent `z`，inference时从prior/约定latent生成chunk；latent用于表达demonstration中的多样性/模式；重建/action loss与KL regularization只要求结构理解；full open-loop risk；receding chunk；overlapping chunks；temporal ensemble/aggregation；inference rate vs action rate vs chunk length；gripper discrete event；stale chunk；safety interrupt；history/context。
4. 深度：Action Chunk/Receding L4-L5；ACT/CVAE structure L3-L4。
5. 工程连接：robot manipulation demonstration policy。
6. 不展开：ACT完整论文源码、VAE严格概率推导。
7. 考核：解释training/inference latent差异；给10Hz inference、50Hz action、20-step chunk算physical horizon并设计refresh。
8. 毕业考点：ACT、CVAE Latent、Action Chunk、Temporal Aggregation、Freshness。

# Day102 — Diffusion Policy / Conditional Denoising / Multimodal Action
1. 今日目标：理解多模态连续动作为什么不能简单MSE平均，并掌握conditional diffusion的最小数学主链。
2. 前置：M06 + Day99–101。
3. 必须教学：multimodal `p(a|o)`；MSE平均陷阱；forward noising示意 `a_t = sqrt(alpha_bar_t) a_0 + sqrt(1-alpha_bar_t) ε, ε~N(0,I)`；condition可以来自vision/state/language/history；noise-prediction training loss `L=E||ε-ε_θ(a_t,t,c)||²` 的结构意义；reverse denoising从noise逐步得到action/chunk；预测noise/clean-action等parameterization只做概念；action-sequence diffusion；优势：multimodality/smooth continuous sequence；代价：multiple denoise steps/latency；Diffusion Policy不是RL；Safety独立。
4. 深度：Multimodality/Diffusion Chain L4；公式 L3。
5. 工程连接：抓取、绕障存在多种正确trajectory。
6. 不展开：DDPM/score matching严格证明、flow matching。
7. 考核：`a=-1/+1`双mode为何MSE预测0；解释forward/noise-loss/reverse三步。
8. 毕业考点：Diffusion Policy、Conditional Denoising、Multimodal Action。

# Day103 — Offline RL / Dataset Support / OOD Action
1. 今日目标：理解固定robot dataset上的RL为什么最危险的问题是dataset分布外Q估计。
2. 前置：Day97–100。
3. 必须教学：offline dataset `(s,a,r,s')`；Offline RL vs BC；dataset support；OOD state/action；critic extrapolation error；policy exploiting Q error；conservative learning概念；coverage≠transition数量；mixed-quality data；reward/label provenance；behavior policy；offline→real safety；CQL/IQL代表概念；dataset未覆盖动作不能默认可靠。
4. 深度：OOD/Support L4。
5. 工程连接：历史bag/log利用。
6. 不展开：CQL/IQL完整公式。
7. 考核：dataset只含`vx≤0.3`但policy输出0.6的风险链。
8. 毕业考点：Offline RL、Dataset Support、OOD。

# Day104 — Robot Policy Evaluation / Sim2Real / Owner
1. 今日目标：解释training/offline指标好为何real robot仍失败，并形成policy evidence chain。
2. 前置：Day97–103。
3. 必须教学：open-loop vs closed-loop；action metric vs task success；task/collision/intervention/recovery/time metrics；visual/dynamics/task/state shift；observation/action latency；calibration/controller mismatch；simulation domain gap；policy safety layer；hybrid learned/classical architecture；state/observation/policy/action/controller command/physical motion区分；failure taxonomy；Owner evidence chain。
4. 深度：Evaluation/Failure Attribution L5。
5. 工程连接：robot rollout与release判断。
6. 不展开：M19部署细节。
7. 考核：offline action准确但task success低的完整归因。
8. 毕业考点：Closed-loop Eval、Distribution Shift、Hybrid Architecture。

---

# M15 Graduation Exam
统一权重：**30%核心基础 / 50%综合系统场景 / 20% Source·Formula·Design**。

## 30% 核心基础
硬门槛：MDP/Return/V-Q-A/Bellman；On/Off-policy；BC/Covariate Shift；temporal alignment；action semantics；ACT/CVAE/action chunk；Diffusion forward-loss-reverse主链；Offline RL/OOD；closed-loop evaluation。

## 50% 综合系统场景
至少覆盖：return/Bellman；PPO vs TD3 data reuse；BC drift；teleop timestamp错误；ACT chunk stale/receding/latent；Diffusion vs MSE多模态动作；Offline RL OOD；Sim→Real policy failure。

## 20% Source / Formula / Design
能读一个高质量PyTorch robot policy实现，定位episode loader、state/action preprocessing、BC/ACT/diffusion policy head、chunk generation、latent或denoising、loss、optimizer、evaluation。

## 通过标准
总分≥85%；必须明确 `Reward≠Return`、`Open-loop Accuracy≠Closed-loop Success`、`Action≠Physical Motion`；Robot Learning主线必须回到dataset/action/closed-loop，而不是算法名词罗列。
