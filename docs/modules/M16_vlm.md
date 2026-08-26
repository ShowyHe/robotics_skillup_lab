# M16 — VLM

## Module Goal
建立 `Text→Token→Embedding→Language Transformer` 与 `Image→Vision Encoder→Visual Tokens`，再通过 Alignment / Projector / Cross-Attention / Fusion 形成多模态语义理解，并明确 VLM 与机器人metric geometry、grounding、VLA action的边界。

本模块共 5 个理论 Day（Day105–Day109）。

---

# Day105 — Token / Embedding / Autoregressive Language Model
1. 今日目标：补齐理解VLM所需LLM基础。
2. 前置：M06 Transformer。
3. 必须教学：token≠word；vocabulary/token ID；embedding；position information/RoPE概念；causal LM；causal mask；next-token `p(x_t|x_<t)`；sequence probability；logits/softmax；cross-entropy/teacher forcing；autoregressive inference；greedy/sampling；temperature/top-k/top-p概念；context window；LLM不是数据库。
4. 深度：Autoregressive objective L4；token/embedding L3-L4。
5. 工程连接：robot instruction首先也是token sequence。
6. 不展开：tokenizer训练、KV cache、MoE、RLHF。
7. 考核：解释training和inference为何不同、temperature改变什么。
8. 毕业考点：Token、Embedding、Causal LM、Next-token objective。

# Day106 — Vision Encoder / Patch Token / Image Representation
1. 今日目标：理解图像如何变成Transformer可处理的visual tokens。
2. 前置：M06 CNN/Transformer + M07 vision。
3. 必须教学：image tensor shape；CNN feature map；ViT patching；`H/P × W/P` token count；patch flatten + linear projection；`N×d` visual tokens；spatial positional info；CLS概念；local vs global feature；resolution/token count/attention compute；multi-scale概念；vision encoder output是feature而非object list；pretrained encoder；spatial information对grounding的重要性。
4. 深度：Patch/token dimension L4。
5. 工程连接：RGB camera→visual tokens。
6. 不展开：Swin/DINO/SAM/DETR细节。
7. 考核：给resolution/patch size算token count并分析compute。
8. 毕业考点：Vision Encoder、Patch Token、Spatial Representation。

# Day107 — CLIP / Contrastive Learning / Vision-Language Alignment
1. 今日目标：理解image与text如何进入共享语义空间。
2. 前置：Day105–106 + M06 loss。
3. 必须教学：image/text encoders；shared embedding；cosine similarity；N-pair batch similarity matrix `S∈R^(N×N)`；diagonal positive/others negative；contrastive objective；temperature；zero-shot classification；alignment≠generation；representation transfer；global semantic alignment limitation；semantic similarity≠pixel/3D geometry；CLIP类模型与聊天VLM边界。
4. 深度：Alignment/similarity matrix L4。
5. 工程连接：text query“red cup”与image semantic match。
6. 不展开：InfoNCE严格证明、distributed CLIP training。
7. 考核：画4×4 similarity matrix并解释positive/negative。
8. 毕业考点：Vision-language alignment、contrastive learning、semantic-vs-geometry。

# Day108 — Projector / Cross Attention / Multimodal Fusion / Instruction Tuning
1. 今日目标：理解visual tokens怎样真正进入LLM。
2. 前置：Day105–107。
3. 必须教学：vision dimension `d_v` vs LLM `d_l`；linear/MLP projector；visual tokens as prefix；self-attention vs cross-attention；`Q`来自language、`K/V`来自vision的shape reasoning；query/resampler概念；early/late fusion；alignment vs fusion；multimodal context；pretrained vision + pretrained LLM + bridge training；freeze/fine-tune trade-off；multimodal instruction tuning；output text/structured token但不天然是metric pose/action；源码阅读层次。
4. 深度：Projector/Cross Attention/Fusion L4。
5. 工程连接：camera + instruction→semantic result。
6. 不展开：某单一VLM完整架构。
7. 考核：给Q/K/V shape推 `QK^T` 维度并解释。
8. 毕业考点：Projector、Cross Attention、Fusion、Instruction Tuning。

# Day109 — Grounding / Spatial Reasoning / Hallucination / Robot Interface
1. 今日目标：判断VLM语义正确后离机器人可执行还缺哪些信息。
2. 前置：Day105–108 + M05/M07 geometry。
3. 必须教学：grounding；referring expression；bbox/mask/point/object ID；open-vocabulary perception；semantic spatial relation vs metric robot geometry；2D grounding + depth + intrinsics + extrinsics/TF→3D；hallucination与language prior；language certainty≠sensor uncertainty；verification gate；multiple-frame consistency；timestamp/freshness/tracking；VLM作为high-level semantic module/planner；structured output contract；input/output/frame/time/confidence/validity；VLM≠VLA；failure attribution：vision/alignment/grounding/reasoning/geometry/TF/time/world model/downstream；robot-centric metrics。
4. 深度：Grounding/geometry boundary/interface L5。
5. 工程连接：VLM说“前方有人”为什么不能直接代替实时collision avoidance。
6. 不展开：action model留M17。
7. 考核：从“红杯子在左边”设计grounding→3D→Manipulation Target链。
8. 毕业考点：Grounding、Hallucination、Semantic vs Metric Geometry、VLM/VLA boundary。

---

# M16 Graduation Exam
统一权重：**30%核心基础 / 50%综合系统场景 / 20% Source·Formula·Design**。

## 30% 核心基础
硬门槛：Autoregressive LM、Vision Encoder/Patch Token、Alignment、Projector/Cross Attention、Grounding、Semantic vs Metric Geometry、Hallucination、VLM/VLA boundary。

## 50% 综合系统场景
至少覆盖：
1. patch token数量/shape；
2. CLIP similarity matrix；
3. projector与cross-attention tensor dimension；
4. “找红杯子”从VLM语义到bbox/mask/depth/TF/3D/grasp target；
5. hallucinated exit的verification/safety；
6. dynamic person stale result为何不适合硬实时避障。

## 20% Source / Formula / Design
选择一个主流开源VLM官方实现 + 一个CLIP类实现，能定位 tokenizer、vision encoder、visual feature selection、projector/adapter、fusion、LLM forward、output/generation，并说明pretrained backbone与multimodal bridge边界。

## 通过标准
总分≥85%；必须明确 `CLIP semantic alignment ≠ metric 3D perception`、`VLM Output ≠ Robot Action`，并能把VLM result接回Grounding→Geometry→TF→World Model→Nav2/MoveIt。

## Day105–Day109 索引
```text
Day105 Token / Embedding / Autoregressive LM
Day106 Vision Encoder / Patch Token
Day107 CLIP / Contrastive Alignment
Day108 Projector / Cross Attention / Fusion
Day109 Grounding / Spatial Reasoning / Hallucination / Robot Interface
```
