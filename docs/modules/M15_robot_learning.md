# M15 — Robot Learning

## Module Goal
把已有 RL 经验重新系统化，但主线明确放在“机器人动作如何从 demonstrations / dataset 学出来”，而不是重新刷完整 RL 算法全集。

核心链：`MDP / Policy → RL基础复盘 → Behavior Cloning / IL / DAgger → Robot Dataset / Action → ACT / Action Chunking → Diffusion Policy → Offline RL / OOD → Closed-loop Evaluation / Sim2Real`。

本模块共 8 个理论 Day（Day97–Day104）。

---

# Day97 — MDP / Return / V-Q-A / Bellman
1. 今日目标：建立序列决策统一语言。
2. 前置：M06 + M08 + M12 action语义。
3. 必须教学：MDP `(S,A,P,R,γ)`；Markov property；state vs observation；deterministic/stochastic policy `π(a|s)`；trajectory；reward vs return `G_t=Σγ^k r_(t+k)`；discount；`V^π(s)`；`Q^π(s,a)`；advantage `A=Q-V`；Bellman expectation；Bellman optimality；reward是task proxy，可能reward hacking。
4. 深度：MDP/V/Q/A/Bellman L4。
5. 工程连接：navigation continuous action/reward。
6. 不展开：完整DP算法。
7. 考核：return手算、V/Q/A区别、reward设计反例。
8. 毕业考点：MDP、Return、V/Q/A、Bellman。

# Day98 — PPO / TD3 / On-policy vs Off-policy 复盘
1. 今日目标：把已有DQN/PPO/TD3经验放进统一Actor-Critic和data-reuse框架，为后续IL/Offline RL服务。
2. 前置：Day97 + M06 backprop。
3. 必须教学：value-based vs policy-based；policy gradient直觉；Actor/Critic；on-policy vs off-policy；PPO ratio `π_new/π_old`、clip、advantage/GAE概念；Replay Buffer；DDPG架构；TD3 twin critics/min target、delayed actor update、target policy smoothing；continuous action；为什么real robot昂贵数据更关心sample efficiency；SAC仅作为maximum-entropy扩展概念，不单独占Day。
4. 深度：PPO/TD3复盘 L3-L4；on/off-policy L4。
5. 工程连接：已有PPO/TD3经验校准。
6. 不展开：DQN全集、SAC完整推导、TRPO。
7. 考核：比较PPO/TD3的数据复用、探索、连续action。
8. 毕业考点：On/Off-policy、Actor-Critic、PPO/TD3核心机制。

# Day99 — Imitation Learning / Behavior Cloning / DAgger / Covariate Shift
1. 今日目标：理解有expert demonstration时如何直接学习policy，以及BC为何closed-loop会逐步偏离。
2. 前置：M06 supervised learning + Day97。
3. 必须教学：IL；dataset `D={(o,a)}`；BC `πθ(o)≈a_expert`；continuous action loss；BC优点；covariate shift `d_expert≠d_π`；compounding error；单步accuracy≠trajectory success；DAgger：run current policy→visit own states→expert label→aggregate→retrain；expert来源；recovery data；dataset quality；action representation决定学习目标。
4. 深度：BC/Covariate shift L4；DAgger L3-L4。
5. 工程连接：teleop→record→BC→deploy。
6. 不展开：GAIL/inverse RL。
7. 考核：解释validation MSE低但闭环越走越偏。
8. 毕业考点：BC、Covariate Shift、DAgger。

# Day100 — Robot Dataset / Episode / Temporal Alignment / Action Semantics
1. 今日目标：理解机器人学习数据不是独立图片，而是带时间的episode。
2. 前置：M03 timestamp + M17前置接口概念。
3. 必须教学：episode `(o_t,s_t,a_t)` sequence；instruction/metadata；camera/joint/action/gripper timestamps；measurement time vs arrival time；teleop command vs actual motion；observation-action delay；exact/nearest/interpolation对齐；action representation：joint pos/vel/torque、EEF delta、base velocity；absolute vs delta；normalization；success/failure/recovery data；episode-level train/val/test split；same-trajectory leakage；dataset diversity/coverage。
4. 深度：Temporal alignment/action semantics L5。
5. 工程连接：rosbag→episode/data pipeline。
6. 不展开：具体RT-X schema。
7. 考核：给image/action延迟判断训练mapping错误。
8. 毕业考点：Robot Dataset、Timestamp Alignment、Action Representation。

# Day101 — ACT / Action Chunking / Temporal Aggregation
1. 今日目标：理解为什么policy一次预测一段future actions，以及chunk如何在closed loop中执行。
2. 前置：Day99–100 + Transformer基础。
3. 必须教学：single-step vs action chunk `A_t=[a_t...a_(t+H-1)]`；chunk horizon；temporal consistency；降低inference frequency；full open-loop执行风险；receding chunk；overlapping chunks；temporal aggregation/ensemble概念；policy inference rate vs action execution rate vs chunk length；gripper discrete event；stale chunk；safety interrupt；history/context；ACT作为action-chunking transformer代表思想，不要求绑定单一论文细节。
4. 深度：Action chunk/receding execution L4-L5。
5. 工程连接：robot manipulation demonstration policy。
6. 不展开：ACT完整源码/API。
7. 考核：给10Hz inference、50Hz action、20-step chunk算physical horizon并判断刷新策略。
8. 毕业考点：Action Chunk、Temporal Aggregation、Freshness。

# Day102 — Diffusion Policy / Multimodal Continuous Action
1. 今日目标：理解为什么多模态连续动作不能简单用MSE平均，并掌握conditional diffusion作为action generator的思想。
2. 前置：M06 DL + Day99–101。
3. 必须教学：multimodal `p(a|o)`；MSE平均陷阱；generative policy；forward noise；reverse denoising；conditioning on vision/state/language/history；noise/clean-action等parameterization只做概念；action-sequence diffusion；适合continuous high-dimensional chunk；优势：multimodality/smooth sequence；代价：multiple denoise steps/latency；Diffusion Policy不是RL；Safety仍独立。
4. 深度：Multimodality/Diffusion intuition L4。
5. 工程连接：抓取/绕障存在多种正确trajectory。
6. 不展开：DDPM/score matching完整证明、flow matching。
7. 考核：`a=-1/+1`双mode为何MSE可能预测0。
8. 毕业考点：Diffusion Policy、Multimodal Action。

# Day103 — Offline RL / Dataset Support / OOD Action
1. 今日目标：理解固定robot dataset上的RL为什么最危险的问题是dataset分布外的Q估计。
2. 前置：Day97–100。
3. 必须教学：offline dataset `(s,a,r,s')`；Offline RL vs BC；dataset support；OOD state/action；critic extrapolation error；policy exploiting Q error；conservative learning概念；coverage≠transition数量；mixed-quality data；reward/label provenance；behavior policy；offline→real safety；CQL/IQL仅作代表概念；不能默认dataset未覆盖动作可靠。
4. 深度：OOD/support L4。
5. 工程连接：真实robot历史bag/log利用。
6. 不展开：CQL/IQL完整公式。
7. 考核：dataset只含vx≤0.3但policy输出0.6的风险链。
8. 毕业考点：Offline RL、Dataset Support、OOD。

# Day104 — Robot Policy Evaluation / Sim2Real / Owner
1. 今日目标：解释training/offline指标好为何real robot仍失败，并形成policy evidence chain。
2. 前置：Day97–103。
3. 必须教学：open-loop vs closed-loop evaluation；action MSE/token accuracy vs task success；task/collision/intervention/recovery/time metrics；distribution shift：visual/dynamics/task/state；observation/action latency；calibration/controller mismatch；simulation parallel training与domain gap；policy safety layer；hybrid learned/classical architecture；state/observation/policy/action/controller command/physical motion区分；failure attribution：Dataset/Label/Reward/Observation/Policy/Action/Latency/Controller/Dynamics/Safety/Hardware；Owner evidence chain。
4. 深度：Evaluation/failure attribution L5。
5. 工程连接：robot rollout与release判断。
6. 不展开：M19部署细节。
7. 考核：offline action准确率高但task success低的完整归因。
8. 毕业考点：Closed-loop Eval、Distribution Shift、Hybrid Architecture。

---

# M15 Graduation Exam
统一权重：**30%核心基础 / 50%综合系统场景 / 20% Source·Formula·Design**。

## 30% 核心基础
硬门槛：MDP/Return/V-Q-A/Bellman；On vs Off-policy；BC/Covariate Shift；temporal alignment；action representation；ACT/action chunk；Diffusion multimodality；Offline RL/OOD；closed-loop evaluation。

## 50% 综合系统场景
至少覆盖：
1. return/Bellman简算；
2. PPO vs TD3 data reuse选择；
3. BC单步accuracy高但closed-loop drift；
4. teleop dataset timestamp/action delay错误；
5. action chunk stale/receding execution；
6. Diffusion vs MSE多模态动作；
7. Offline RL OOD action；
8. Sim→Real policy失败的视觉/动力学/延迟/控制归因。

## 20% Source / Formula / Design
能读懂一个官方/高质量PyTorch robot policy实现，定位：episode loader、state/action preprocessing、policy/critic或BC head、chunk/diffusion generation、loss、optimizer、evaluation；源码数量不是目标。

## 通过标准
总分≥85%；必须明确 `Reward≠Return`、`Open-loop Accuracy≠Closed-loop Success`、`Action≠Physical Motion`；Robot Learning主线必须回到dataset/action/closed-loop而不是算法名词罗列。

## Day97–Day104 索引
```text
Day97 MDP / Return / V-Q-A / Bellman
Day98 PPO / TD3 / On-policy vs Off-policy Review
Day99 IL / BC / DAgger / Covariate Shift
Day100 Robot Dataset / Temporal Alignment / Action Semantics
Day101 ACT / Action Chunking / Temporal Aggregation
Day102 Diffusion Policy / Multimodal Action
Day103 Offline RL / Dataset Support / OOD
Day104 Policy Evaluation / Sim2Real / Owner
```
