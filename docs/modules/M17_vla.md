# M17 — VLA

## Module Goal
正式进入 `Vision + Language + Robot State → Multimodal Policy → Action Representation / Sequence → Safety / Controller → Physical Robot`。

M15已经教授 BC、Action Chunking、Diffusion Policy 等学习基础；M17不重复从零教学，而是回答它们如何嵌入VLA、如何从预训练VLM扩展到Robot Policy、以及action最终怎样落到真实控制接口。

本模块共 6 个理论 Day（Day110–Day115）。

---

# Day110 — VLA Architecture / Proprioception / Action Representation
1. 今日目标：理解VLA与VLM根本区别以及“Action到底是什么”。
2. 前置：M12/M14/M15/M16。
3. 必须教学：VLA input：image/language/proprioception/history；robot state `q,qdot,EEF/gripper/base`；policy `a_t=π(o_t,l,s_t)`；joint absolute/delta/velocity/torque action；Cartesian EEF pose/delta action；base velocity vs waypoint/skill action；gripper action；action frequency；VLA rate vs controller/motor loop；EEF delta→IK/Servo→joint command；action representation对data/safety/portability的影响；VLA不等于end-to-end替换所有传统模块。
4. 深度：Action representation/interface L5。
5. 工程连接：EEF delta、base vx/wz、high-level skill三类接口比较。
6. 不展开：具体VLA论文架构。
7. 考核：比较joint torque、joint position、EEF delta三种action responsibility。
8. 毕业考点：Proprioception、Action Representation、Execution Boundary。

# Day111 — VLA Robot Dataset / Sequence / Temporal Context
1. 今日目标：把M15 Robot Dataset原则应用到Vision-Language-Action episode。
2. 前置：M15 Day100。
3. 必须教学：episode含instruction + image sequence + robot state + action + timestamps；multicamera/depth可选；observation/action alignment复用；teleop command vs feedback；history window；previous action作为context；success/failure/recovery episodes；language paraphrase与task metadata；action normalization；different embodiment dataset字段；padding/masking；episode-level split；data quality gate；多模态输入之间timestamp/calibration的一致性。
4. 深度：VLA dataset semantics/temporal context L5。
5. 工程连接：rosbag/teleop dataset→VLA training sample。
6. 不展开：特定RT-X/OXE schema细节。
7. 考核：给camera/joint/action三类timestamp设计training alignment。
8. 毕业考点：VLA Dataset、Temporal Context、Normalization。

# Day112 — Action Token / Autoregressive Action / Sequence Modeling
1. 今日目标：理解LLM式token generation如何扩展到robot action。
2. 前置：M16 autoregressive LM + M15 action semantics。
3. 必须教学：continuous action discretization；bin/token resolution；quantization error；action vocabulary vs independent action head；visual/language/state tokens→action token；autoregressive action dimensions/time sequence；teacher forcing；deployment exposure mismatch；within-action dependency；autoregressive latency；history context；de-tokenization/scale恢复；discrete token不代表robot物理动作离散；invalid token/action validation。
4. 深度：Action tokenization/autoregressive action L4。
5. 工程连接：token generation latency与10–20Hz robot policy。
6. 不展开：beam search、LLM decoding技巧全集。
7. 考核：给action range/bin数算resolution并分析量化误差。
8. 毕业考点：Action Token、Autoregressive Action、Latency。

# Day113 — Action Chunk in VLA / Closed-loop Execution / Temporal Aggregation
1. 今日目标：在VLA上下文中复用M15 Action Chunking，并重点理解闭环执行和Safety。
2. 前置：M15 Day101 + Day110–112。
3. 必须教学：VLA预测future action chunk；chunk horizon；full open-loop risk；receding execution；observation refresh；overlapping chunks；temporal aggregation；policy inference rate/action rate/chunk length区别；discrete gripper events不能盲目average；environment/object变化导致stale chunk；safety/e-stop/force monitor可中断；VLA chunk与MPC/MPPI future control sequence的相似闭环结构与算法差异；controller interpolation/tracking。
4. 深度：Receding chunk/freshness L5。
5. 工程连接：approach→close→lift期间物体移动的处理。
6. 不展开：重复讲Diffusion数学。
7. 考核：10Hz inference、50Hz action、20-step chunk的time horizon与刷新策略。
8. 毕业考点：Action Chunk、Receding Execution、Safety Interrupt。

# Day114 — VLA Training / Fine-tuning / Action Head / Cross-embodiment
1. 今日目标：理解一个预训练VLM怎样真正变成Robot Policy，以及不同robot embodiment如何进入训练设计。
2. 前置：M15 + M16 + Day110–113。
3. 必须教学：pretrained vision/VLM knowledge≠robot control knowledge；robot demonstration fine-tuning；multimodal backbone + proprioception encoder + action head；action head可为regression/token/chunk/diffusion（Diffusion机制复用M15，不重讲）；loss与action representation对应；freeze/backbone fine-tune/adapter trade-off；instruction tuning vs action supervision；embodiment定义；7DoF/6DoF/mobile manipulator差异；joint-space action跨robot迁移困难；EEF delta较统一但仍受reachability/dynamics/controller限制；action normalization/masking/embodiment-specific decoder；multi-robot dataset imbalance；language/object/scene generalization层级；fine-tuning后的catastrophic degradation与validation。
4. 深度：Training architecture/cross-embodiment L4-L5。
5. 工程连接：VLM backbone→EEF delta action head的真实部署思路。
6. 不展开：大规模distributed pretraining、具体foundation VLA recipe。
7. 考核：为什么“接一个Linear Head”不自动得到可靠VLA；比较joint vs EEF跨robot迁移。
8. 毕业考点：VLM→VLA bridge、Action Head、Fine-tuning、Embodiment。

# Day115 — VLA Evaluation / Safety / Generalization / System Owner
1. 今日目标：形成从raw observation到physical result的VLA证据链。
2. 前置：Day110–114。
3. 必须教学：open-loop action metric vs closed-loop task success；OOD/generalization分层：lighting/object instance/category/scene/robot state/embodiment；hallucination-to-action风险；raw action decode/de-normalization；range/rate/workspace/joint/collision/force safety validation；Safety Filter可reject/modify/stop；high-level VLA→skills/Nav2/MoveIt vs low-level VLA→continuous actions；hybrid architecture；controller/dynamics/action delay；failure recovery；Owner failure taxonomy：Instruction/Vision/Grounding/State/Dataset/Policy/Decode/Timestamp/Safety/Controller/Dynamics/Hardware；源码阅读：preprocess/vision/tokenizer/state encoder/fusion/action head/chunk/loss/inference/decode；evidence chain `Observation→Timestamp→Encoding→Policy→Raw Action→Decode→Safety→Controller→Feedback→Task Result`。
4. 深度：Evaluation/safety/interface/failure attribution L5。
5. 工程连接：mobile manipulator“走过去拿杯子”的high-level vs low-level设计。
6. 不展开：M19部署/Sim2Real细节。
7. 考核：offline MSE低、实机task success低的完整排查；设计VLA safety boundary。
8. 毕业考点：Open/Closed-loop Eval、Safety Filter、Classical Stack Boundary、Owner evidence。

---

# M17 Graduation Exam
统一权重：**30%核心基础 / 50%综合系统场景 / 20% Source·Formula·Design**。

## 30% 核心基础
硬门槛：VLM vs VLA、proprioception、joint/EEF/base action、absolute/delta/position/velocity/torque、temporal context、action token、action chunk/receding execution、VLM→VLA fine-tuning/action head、embodiment、open vs closed-loop eval、Safety Filter。

## 50% 综合系统场景
至少覆盖：
1. 三种action representation的下游controller/safety/portability；
2. multimodal VLA dataset timestamp alignment；
3. action token quantization/latency；
4. chunk中object移动后的refresh/replan；
5. 7DoF→6DoF cross-embodiment迁移；
6. offline action准确但real task success低；
7. high-level skill VLA vs direct base+arm continuous VLA系统设计。

## 20% Source / Formula / Design
选择1个代表性开源VLA官方实现，定位：observation preprocessing、vision/language/proprioception encoding、fusion、action head、normalization、chunk/token/diffusion接口、loss、inference、robot action decode；说明哪部分继承VLM，哪部分真正构成robot policy。

## 通过标准
总分≥85%；必须明确 `VLM Output ≠ VLA Action ≠ Controller Command ≠ Physical Motion`；temporal alignment、action representation、Safety Filter、closed-loop evaluation为硬门槛。

## Day110–Day115 索引
```text
Day110 VLA Architecture / Proprioception / Action Representation
Day111 VLA Dataset / Sequence / Temporal Context
Day112 Action Token / Autoregressive Action
Day113 Action Chunk / Closed-loop Execution
Day114 Training / Fine-tuning / Action Head / Embodiment
Day115 Evaluation / Safety / Generalization / Owner
```
