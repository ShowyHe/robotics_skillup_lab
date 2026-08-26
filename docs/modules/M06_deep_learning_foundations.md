# M06 — Deep Learning Foundations

## Module Goal
建立后续 Deep Vision、Robot Learning、VLM、VLA 都依赖的统一深度学习理论框架。重点不是“会调用 PyTorch / 会跑模型”，而是理解数据如何进入模型、参数如何通过梯度更新、CNN 与 Transformer 为什么有效，以及训练为什么会失败。

完整主线：

```text
数据 / Dataset
↓
Tensor / DataLoader
↓
模型 fθ(x)
↓
预测
↓
Loss
↓
Gradient
↓
Backpropagation / Autograd
↓
Optimizer 更新 θ
↓
新的模型
```

本模块共 7 个理论 Day（Day27–Day33）。默认不安排强制实验；PyTorch 以代码阅读、tensor 维度推理和训练链理解为主。

---

# Day27 — Neural Network、Tensor、Dataset 与 Loss：模型到底在学什么

## 1. 今日目标
学完后必须能够解释一个神经网络训练过程本质上在优化什么，并严格区分 input、feature、parameter、prediction、target、loss、objective、tensor、dataset 与 batch。

## 2. 前置知识
M02：vector/matrix、function、derivative、gradient、chain rule。

## 3. 必须教学内容

### ① Supervised Learning基本形式
建立 `(x_i, y_i)`，其中 `x_i` 是 input，`y_i` 是 target。模型写为：

`ŷ = f_θ(x)`

必须理解 `θ` 是模型通过训练学习的参数集合。

### ② Parameter vs Hyperparameter
必须区分：
- Parameter：weight、bias，由训练更新；
- Hyperparameter：learning rate、batch size、network depth等，由人为设定或搜索。

### ③ Tensor
必须理解 Tensor 是 DL 中承载批量多维数据的基本结构，不等于“神秘的高维矩阵”。必须会读：
- shape；
- dtype；
- device；
- batch dimension。

必须建立“看到网络代码先看 tensor shape”的习惯。

### ④ Linear Layer
必须理解：

`y = Wx + b`

若 `x∈R^n`、`W∈R^(m×n)`、`b∈R^m`，则 `y∈R^m`。必须进行维度推理。

### ⑤ 为什么需要Nonlinearity
如果所有 layer 都只是线性/仿射变换，多层叠加仍可等价成一个仿射变换。因此需要 ReLU 等 activation。Sigmoid/Tanh仅建立概念。

### ⑥ Loss Function
必须理解 Loss 是对当前 prediction 与 target 不一致程度的数值描述。典型 MSE：

`L = (ŷ - y)^2`

Cross Entropy先建立概念，Day30展开。

### ⑦ Empirical Risk / Dataset Objective
训练不是只优化单个 sample，而是优化 dataset 上的整体目标：

`J(θ) = (1/N) Σ_i L(f_θ(x_i), y_i)`

### ⑧ Dataset
必须理解 Dataset 的职责是定义：
- 一个样本是什么；
- input/target如何组成；
- 数据索引与预处理在哪里发生。

### ⑨ DataLoader / Batch
必须理解 DataLoader 负责把 Dataset 按 batch 取出，并可处理 shuffle、batching、并行读取等工程问题。重点理解数据流，不要求背 API。

必须建立：

```text
Dataset
→ sample
→ DataLoader
→ mini-batch Tensor
→ Model
```

### ⑩ Forward Pass
必须理解：

```text
input tensor
→ layer1
→ activation
→ layer2
→ prediction
→ loss
```

这是 forward computation。

### ⑪ Training vs Inference
必须区分：
- Training：data → forward → loss → backward → parameter update；
- Inference：通常只有 data preprocessing + forward + postprocess。

### ⑫ Model Capacity
建立：模型过简单可能 underfitting；相对数据过复杂可能增加 overfitting 风险。暂不讲 VC theory。

## 4. 深度要求
Tensor/shape L3；`y=Wx+b` L3；parameter/loss L3；Dataset/DataLoader L2-L3；training loop总体结构 L3；model capacity L2。

## 5. 机器人连接
必须联系：
- YOLO：image → detections；
- Depth model：image → depth；
- Policy：observation → action；
- VLA：vision/language/state → action。

重点建立：不同任务输出不同，但训练基本框架高度一致。

## 6. 明确不展开
Backprop完整推导、CNN、Transformer、Dataset工程优化、distributed data loading、optimizer具体算法。

## 7. 本日考核点
1. Parameter和hyperparameter区别？
2. Tensor的shape/dtype/device分别表示什么？
3. `W(m×n)x(n×1)`输出维度？
4. 多层纯linear layer为什么仍是linear/affine？
5. activation解决什么？
6. Dataset和DataLoader职责区别？
7. sample和batch区别？
8. prediction和target区别？
9. loss在训练中承担什么角色？
10. training与inference最大区别？
11. 给一个robot policy指出input/output/parameter/loss分别可能是什么。

### M06毕业考试核心考点
Tensor维度、Dataset/DataLoader、模型、parameter、loss、forward、training objective。

---

# Day28 — Backpropagation 与 Autograd：Gradient 如何穿过网络

## 1. 今日目标
真正理解 Loss 为什么能够告诉网络中大量 parameter 应分别往哪个方向变化，并理解 PyTorch Autograd 实际替我们自动完成了什么。

## 2. 前置知识
Day27 + M02 Chain Rule。

## 3. 必须教学内容

### ① Computational Graph
例如：

`z = wx + b`

`ŷ = g(z)`

`L = L(ŷ, y)`

建立：

```text
w
↓
z
↓
ŷ
↓
L
```

### ② Local Derivative
每一个计算节点只需要知道自己的输出对输入如何变化。

### ③ Chain Rule
必须完整计算：

`∂L/∂w = (∂L/∂ŷ)(∂ŷ/∂z)(∂z/∂w)`

要求真正手算一个简单网络。

### ④ Backward Pass
必须理解 Backpropagation 不是新的数学定理，本质是高效组织 Chain Rule。

### ⑤ Gradient的意义
必须反复强调：

`∂L/∂w`

表示 parameter `w` 微小变化时 loss 如何变化。

### ⑥ Vector / Tensor Gradient
必须理解一个 parameter tensor 对应同 shape 的 gradient tensor；gradient不是单个神秘数字。

### ⑦ Gradient Accumulation
一个 parameter 可通过多条 computational path 影响 loss，因此各路径 gradient 贡献需要相加。

### ⑧ Reverse-mode Automatic Differentiation
工程级理解：网络 parameter 很多而最终 loss 通常为 scalar，因此 reverse-mode 很适合 DL。

### ⑨ PyTorch Autograd
必须理解：

```text
forward构建计算关系
↓
loss.backward()
↓
Autograd按Chain Rule计算各parameter.grad
```

必须认识 `requires_grad`、`.grad` 的角色，但不要求背 API。

### ⑩ Gradient Detach / no_grad概念
建立：有些计算不希望进入梯度图，inference通常也不需要构建完整 backward graph。

## 4. 深度要求
Chain Rule L4；Backprop L4；Computational Graph L3-L4；Autograd L3。

## 5. 机器人连接
必须联系：

```text
image/state
↓
policy network
↓
predicted action
↓
action loss
↓
gradient
↓
policy parameters
```

以后 BC、ACT、VLA 都复用这套机制。

## 6. 明确不展开
CNN backward细节、Transformer backward、second-order autodiff、distributed gradient。

## 7. 本日考核点
1. Backprop本质是什么？
2. 为什么需要Chain Rule？
3. `∂L/∂w`物理意义？
4. 手算一个两层简单computational graph。
5. 为什么parameter tensor与gradient tensor通常shape相同？
6. 一个变量通过两条路径影响loss时gradient怎么办？
7. forward和backward分别计算什么？
8. Autograd自动完成了什么？
9. `requires_grad`解决什么？
10. inference为什么通常不需要gradient graph？
11. Robot policy的action error如何影响早期layer参数？

### M06毕业考试核心考点
Chain Rule、computational graph、backprop、Autograd、gradient interpretation。

---

# Day29 — Training Loop、Gradient Descent、SGD、Momentum 与 Adam

## 1. 今日目标
理解有了 gradient 以后 parameter 如何被更新，并能完整读懂一个最小 PyTorch training loop 的执行顺序。

## 2. 前置知识
Day27–28。

## 3. 必须教学内容

### ① Basic Gradient Descent
必须掌握：

`θ_(k+1) = θ_k - α∇_θ L`

逐项解释 θ、α、gradient。

### ② Learning Rate
必须理解：
- 太大：overshoot、oscillation、divergence；
- 太小：convergence慢、长期停滞。

### ③ Full Batch vs Mini-batch
必须区分完整dataset gradient与mini-batch gradient estimate。

### ④ SGD中的Noise
Mini-batch gradient不是完整gradient，因此具有随机性；这种noise不完全是坏事。

### ⑤ Epoch / Iteration / Batch
严格区分三个概念。

### ⑥ 完整Training Loop顺序
必须理解如下逻辑顺序：

```text
for batch in DataLoader
  ↓
清理上一轮gradient
  ↓
forward
  ↓
compute loss
  ↓
backward
  ↓
optimizer step
  ↓
记录metric
```

必须理解为什么不能把 `optimizer.step()` 放在 `backward()` 前面，以及为什么通常需要清理旧gradient。

### ⑦ `train()` / `eval()`模式概念
只建立：某些 layer 在training与inference行为不同，因此框架需要显式切换模型模式。BatchNorm/Dropout具体原因留Day32。

### ⑧ Momentum
必须建立：不仅看当前gradient，还累计过去更新方向。比喻只能辅助，不能代替数学理解。

### ⑨ Adam
工程级理解：Adam利用一阶与二阶moment估计，对不同parameter形成不同有效step。无需完整收敛证明。

### ⑩ Learning Rate Schedule
理解 warmup、decay 为什么用于训练不同阶段。

### ⑪ Local Minimum / Saddle Point
建立non-convex optimization基本认知，不形成“DL一定能找到global optimum”的错误观点。

### ⑫ Gradient Clipping
理解通过限制gradient magnitude避免异常大更新，为Transformer/Robot Learning做准备。

## 4. 深度要求
Gradient descent L3-L4；Training loop L3-L4；SGD L3；Adam L2-L3；LR reasoning L3-L4。

## 5. 机器人连接
必须联系：Detection training loss不下降、policy training震荡、VLA fine-tuning、batch size受GPU显存限制。

## 6. 明确不展开
Adam严格收敛理论、convex optimization完整理论、distributed training、AMP细节、L-BFGS等其他optimizer。

## 7. 本日考核点
1. 为什么沿负gradient更新？
2. Learning rate太大可能发生什么？
3. Epoch/iteration/batch区别？
4. Mini-batch gradient和full gradient是否完全一样？
5. SGD里的noise从哪里来？
6. 最小training loop正确顺序是什么？
7. 为什么backward前通常要处理上一轮gradient？
8. 为什么step必须在backward之后？
9. `train/eval`为什么需要区分？
10. Momentum解决什么？
11. Adam为什么对不同parameter可有不同有效step？
12. Gradient clipping解决什么？

### M06毕业考试核心考点
Training loop、Gradient Descent、LR、mini-batch、SGD、Adam、training dynamics。

---

# Day30 — Classification、Logit、Softmax 与 Cross Entropy

## 1. 今日目标
真正理解classification network为什么通常输出logits，而不是直接输出“这是某类别”。

## 2. 前置知识
Day27–29。概率严格体系放M08；本日只学习DL所需部分。

## 3. 必须教学内容

### ① Regression vs Classification
必须区分continuous output与discrete category。

### ② Logit
必须理解网络最后输出的raw score。Logit不要求和为1，也不要求非负。

### ③ Softmax
必须理解：

`p_i = exp(z_i) / Σ_j exp(z_j)`

作用：转为正数、normalize为和1，并保留相对score关系。

### ④ Probability Interpretation
Softmax输出可作为模型类别分布表达，但高softmax值不等于现实世界中同等程度的真实可信度。

### ⑤ Cross Entropy
必须理解：正确类别得到的概率越低，loss越大。核心形式：

`L = -log p_y`

### ⑥ One-hot Target
理解classification target编码。

### ⑦ Why Log
必须建立：高confidence正确 → loss小；对正确类别给极低概率 → 惩罚很大。不展开information theory。

### ⑧ Binary Classification
认识sigmoid + BCE，与multiclass softmax的典型区别。

### ⑨ Class Imbalance
理解不同类别样本数量差异如何影响学习。

### ⑩ Threshold
理解classification/detection后处理threshold是系统decision规则，不应把它和训练概率语义混为一谈。

## 4. 深度要求
Logits/softmax L3；Cross Entropy L3；classification probability interpretation L3。

## 5. 机器人连接
必须联系YOLO class confidence、pedestrian detection、traffic light recognition、VLM token probability。必须明确：网络confidence与机器人安全confidence不能简单等价。

## 6. 明确不展开
Entropy/KL完整理论、Bayesian DL、confidence calibration完整方法、detection loss完整结构。

## 7. 本日考核点
1. Logit是不是probability？
2. Softmax做了什么？
3. 为什么输出和为1？
4. Cross Entropy惩罚什么？
5. 正确类别概率从0.9降到0.01时loss如何变化？
6. One-hot是什么？
7. Sigmoid和Softmax典型使用区别？
8. Class imbalance为什么有问题？
9. 0.95 confidence是否意味着现实中95%一定正确？
10. Detector threshold为什么属于系统设计的一部分？

### M06毕业考试核心考点
Logit、Softmax、Cross Entropy、probability interpretation。

---

# Day31 — CNN Foundations：为什么图像网络不直接使用普通全连接层

## 1. 今日目标
理解CNN核心不是 `Conv2d` API，而是利用图像局部结构、parameter sharing和层级feature提取。

## 2. 前置知识
Day27–30 + M05 Camera/Image概念。

## 3. 必须教学内容

### ① Image Tensor
必须理解典型 `C×H×W`，batch后通常为 `N×C×H×W`，并知道每一维的物理/数据含义。

### ② Convolution核心思想
局部kernel在图像上滑动，对局部区域做加权组合。必须理解 convolution layer 本质仍是parameterized linear operation，再配合activation形成非线性网络。

### ③ Kernel / Filter
理解kernel size、input channel、output channel。

### ④ Parameter Sharing
同一组kernel在不同image location复用。必须理解为什么视觉任务适合这种inductive bias。

### ⑤ Receptive Field
必须理解一个高层feature最终“看到了”原始图像多大区域。

### ⑥ Stride
理解对spatial resolution的影响。

### ⑦ Padding
理解为什么需要控制feature map尺寸与边界信息。

### ⑧ Channel
避免把channel理解成“永远是RGB三通道”；中间feature channel可几十或几百。

### ⑨ Pooling / Downsampling
理解spatial compression、receptive field扩大、information loss trade-off。

### ⑩ Hierarchical Features
建立概念：edge/texture → local pattern → object part → semantic feature，但不把这一层级描述绝对化。

### ⑪ Translation Equivariance直觉
理解输入平移后feature响应也随位置移动的基本性质。

### ⑫ CNN Output进入Task Head
建立：

```text
Backbone
↓
Feature
↓
Classification / Detection / Segmentation Head
```

为M07做准备。

## 4. 深度要求
Tensor dimensions L3-L4；Convolution L3；Receptive Field L3；CNN architecture reasoning L3。

## 5. 机器人连接
联系YOLO backbone、segmentation、depth network、vision encoder。

## 6. 明确不展开
YOLO具体head、ResNet完整源码、convolution backward手推、3D CNN。

## 7. 本日考核点
1. `N,C,H,W`分别是什么？
2. 为什么不用纯fully-connected处理高分辨率image？
3. Parameter sharing是什么意思？
4. Kernel为什么可以检测局部pattern？
5. Stride增大会发生什么？
6. Padding为什么存在？
7. Receptive field是什么？
8. Feature channel代表什么？
9. Pooling有哪些代价？
10. Backbone和task head区别？
11. 给出简单Conv配置判断input/output tensor shape。

### M06毕业考试核心考点
CNN结构、tensor dimension、parameter sharing、receptive field。

---

# Day32 — Generalization：Overfitting、Normalization、Regularization 与 Distribution Shift

## 1. 今日目标
理解 Training loss很低为什么不代表机器人真正学会任务。

## 2. 前置知识
Day27–31。

## 3. 必须教学内容

### ① Train / Validation / Test
必须严格区分三个dataset角色。

### ② Generalization
定义：对未见数据保持性能。

### ③ Overfitting
典型表现：training performance继续提高，而validation performance恶化或停止提升。

### ④ Underfitting
模型甚至无法把training data拟合好。

### ⑤ Data Leakage
必须认识training阶段间接看到了validation/test信息，会造成虚假performance。

### ⑥ Input Normalization
理解输入尺度归一化为何有利于optimization与稳定训练。

### ⑦ Batch Normalization
工程级理解：对activation做统计归一化；training与inference行为存在差异。不推完整gradient。

### ⑧ Layer Normalization
建立与Transformer联系：主要在feature维度归一化，不依赖与BatchNorm相同的batch统计机制。

### ⑨ Regularization
理解weight decay、dropout、augmentation等主要作用。

### ⑩ Dropout与train/eval
必须解释为什么Dropout在training与inference行为不同，并与Day29的模型模式切换对应。

### ⑪ Data Augmentation
必须理解：改变输入但尽量保持task label语义不变。机器人视觉augmentation必须符合任务与物理合理性。

### ⑫ Distribution Shift
这是Robot Learning/VLA的重要前置。训练分布 `p_train(x)` 与实机分布 `p_real(x)` 不同，模型可能失败。

### ⑬ Metric ≠ Real Robot Success
mAP或classification accuracy提高不自动意味着navigation更安全、manipulation success更高。

## 4. 深度要求
Generalization L3；Overfit/underfit L3；Normalization L2-L3；Distribution Shift L3-L4。

## 5. 机器人连接
必须联系室内/室外、sunlight/night、camera位置变化、Sim2Real、robot policy和VLA跨平台迁移。

## 6. 明确不展开
Statistical learning theory、domain adaptation完整算法、Sim2Real详细方法。

## 7. 本日考核点
1. Train/val/test分别做什么？
2. Validation为何不能代替最终test？
3. Overfitting和underfitting表现？
4. Data leakage为什么危险？
5. Input normalization作用？
6. BatchNorm training/inference为什么不同？
7. LayerNorm与BatchNorm关注的维度有何主要不同？
8. Dropout为什么需要train/eval模式？
9. Augmentation为什么不能乱做？
10. Distribution shift对机器人为何特别严重？
11. mAP提高为什么不一定让导航更安全？

### M06毕业考试核心考点
Generalization、data split、overfit、normalization、regularization、distribution shift。

---

# Day33 — Attention 与 Transformer Foundations

## 1. 今日目标
作为从传统DL进入VLM/VLA的关键入口，必须真正理解：Attention解决的是“一个token应该从其他token读取哪些信息，以及读取多少”。

## 2. 前置知识
Day27–32，尤其matrix multiplication、softmax、learned parameter、feature vector。

## 3. 必须教学内容

### ① Sequence / Token
必须理解token不只可以是文字，还可以是word/subword token、image patch token、state token、action token。

### ② Embedding
必须理解将离散或原始输入映射为连续feature vector。

### ③ Q / K / V
必须分别解释：
- Query：当前token用于查询关系的表示；
- Key：用于与Query匹配的表示；
- Value：最终被加权读取的信息表示。

比喻只能辅助，最终必须回到matrix定义。

### ④ Scaled Dot-product Attention
必须理解：

`Attention(Q,K,V) = softmax(QK^T / sqrt(d_k)) V`

逐步拆成：

```text
QK^T
↓
similarity scores
↓
scale
↓
softmax
↓
attention weights
↓
weighted sum of V
```

### ⑤ Matrix Dimensions
这是硬门槛。例如 `Q∈R^(n×d)`、`K∈R^(m×d)`，则 `QK^T∈R^(n×m)`。必须理解attention matrix每个元素表达“第i个query与第j个key的匹配分数”。

### ⑥ 为什么除以 `sqrt(d_k)`
工程级理解：dimension增大时dot-product幅值可能增大，使softmax过于尖锐、梯度行为变差。

### ⑦ Self-Attention
Q/K/V来自同一sequence。必须理解每个token可读取同一sequence中其他token的信息。

### ⑧ Cross-Attention
Q来自一个sequence，K/V来自另一个sequence。为VLM建立：language representation 与 visual representation之间可以通过cross-attention交互。

### ⑨ Multi-head Attention
理解多组不同projection允许模型在不同representation subspace学习不同关系；不能把“每个head一定负责一种固定语义”当事实。

### ⑩ Positional Information
Self-attention本身不天然编码sequence顺序，因此需要positional encoding/embedding等位置信息。

### ⑪ Transformer Block
必须认识典型组成：Attention、Residual、Normalization、FFN。具体Pre-LN/Post-LN不展开。

### ⑫ FFN
理解Attention负责token间信息交换，FFN负责每个token内部feature的非线性变换。

### ⑬ Causal Mask
为language/action autoregressive模型建立：当前token不能读取未来token的信息。

### ⑭ Transformer 与 CNN 区别
必须从inductive bias理解：CNN强调locality和parameter sharing；Transformer强调token relation与更灵活的全局交互。不能简化为“Transformer一定比CNN高级”。

### ⑮ Robotics Connection
建立：

```text
Image patches
+
Language tokens
+
Robot state
↓
Transformer
↓
Action representation
```

只建立统一表示框架，不提前教授VLA训练。

## 4. 深度要求
Q/K/V L3-L4；Attention公式 L3-L4；Dimensions L4；Self/Cross Attention L3；Transformer Block L3。

## 5. 工程连接
必须联系ViT、VLM、ACT、Diffusion Policy中的feature representation、VLA，但不提前教授这些系统。

## 6. 明确不展开
LLM训练、tokenizer算法、FlashAttention、KV Cache、RoPE深入、ViT完整架构、VLM、VLA。

## 7. 本日考核点
1. Token只能是文字吗？
2. Embedding解决什么？
3. Q/K/V分别承担什么角色？
4. `QK^T`中某个元素表示什么？
5. Attention weight如何产生？
6. 为什么最后乘V？
7. Q为`10×64`、K为`20×64`时score矩阵维度？
8. 为什么除`sqrt(d_k)`？
9. Self-Attention和Cross-Attention区别？
10. Multi-head解决什么？
11. 为什么需要position信息？
12. Causal mask为什么存在？
13. Attention和FFN分别负责什么？
14. CNN与Transformer各有什么inductive bias？
15. Image+Language+Robot State如何进入Transformer框架？

### M06毕业考试核心考点
Q/K/V、Attention matrix dimension、Self/Cross Attention、Transformer Block。

---

# M06 Graduation Exam Specification

## A. 核心理论专项 — 30%

必须覆盖：

| 能力 | 要求 |
|---|---|
| Tensor / Shape / Device / Dtype | 必考 |
| Dataset / DataLoader / Batch | 必考 |
| Parameter / Hyperparameter | 必考 |
| Linear Layer / Dimension | 必考 |
| Loss / Objective | 必考 |
| Chain Rule | **硬门槛** |
| Backpropagation / Autograd | **硬门槛** |
| Gradient | **硬门槛** |
| Training Loop | **硬门槛** |
| Gradient Descent / Learning Rate | 必考 |
| Mini-batch / Epoch | 必考 |
| Adam | 理解题 |
| Logit / Softmax | 必考 |
| Cross Entropy | 必考 |
| CNN Dimension | 必考 |
| Convolution / Parameter Sharing | 必考 |
| Generalization | **核心必考** |
| Overfit / Distribution Shift | **核心必考** |
| Q / K / V | **硬门槛** |
| Attention Matrix Dimension | **硬门槛** |
| Self / Cross Attention | 必考 |
| Transformer Block | 必考 |

## B. 综合理论场景 — 50%

至少设计3组综合题。

### 场景1：训练失败诊断
例如：一个机器人视觉模型training loss快速下降，但validation accuracy下降；换到室外实机后性能进一步恶化。

要求分析：overfitting、data split、augmentation、distribution shift、metric、model capacity，并明确需要查看什么证据。

### 场景2：Backprop / Optimization
给一个小型网络：

```text
x
→ Linear
→ ReLU
→ Linear
→ prediction
→ loss
```

要求写变量维度、根据简单数值算forward、用chain rule求部分gradient，并解释learning rate改变后的训练行为。

### 场景3：Attention
给4个image tokens、3个language tokens、embedding dim=8，要求判断self-attention与cross-attention的Q/K/V和score matrix shape、每行attention weight含义、为什么weighted sum V，以及language如何读取visual information。

## C. PyTorch源码 / Robot AI架构题 — 20%

给一个小型PyTorch训练代码片段，要求能够：
- 找出Dataset/DataLoader的数据流；
- 判断tensor shape；
- 找出model parameters；
- 指出forward、loss、backward、optimizer step；
- 判断training loop顺序是否正确；
- 判断`train/eval`使用是否合理；
- 修改一个小型CNN或Transformer的输入/output维度或head，并解释修改理由。

同时给：

```text
Camera
↓
Vision Encoder
↓
Feature
+
Language
+
Robot State
↓
Transformer / Policy
↓
Action
```

要求说明input、parameter、training target、loss、gradient propagation、inference阶段消失的步骤，以及distribution shift可能发生的位置。

---

# M06 Knowledge Coverage Matrix

| 能力 | 要求 |
|---|---|
| Tensor / Shape | **核心必考** |
| Dataset / DataLoader | 必考 |
| Training Loop | **硬门槛** |
| Parameter / Hyperparameter | 必考 |
| Loss / Objective | 必考 |
| Chain Rule / Backprop | **硬门槛** |
| Autograd | 必考 |
| Gradient / LR | **硬门槛** |
| SGD / Adam | 必考/理解 |
| Softmax / Cross Entropy | 必考 |
| CNN / Conv / Receptive Field | 必考 |
| Generalization / Overfit | **核心必考** |
| Normalization / Regularization | 必考 |
| Distribution Shift | **核心必考** |
| Q / K / V | **硬门槛** |
| Attention Dimension | **硬门槛** |
| Self / Cross Attention | 必考 |
| Transformer Block | 必考 |
| 小型PyTorch代码阅读与修改 | **核心必考** |

# M06最终通过标准

- 总分 ≥85%；
- Chain Rule / Backprop、Gradient、Training Loop、Attention维度推理属于硬门槛；
- 不允许只会说“backward就是求导”“Attention就是关注重要信息”；
- 必须能从具体变量、tensor shape、computational graph和matrix维度解释实际计算过程；
- 必须能读懂并做小修改于小型CNN/Transformer训练代码，但本模块不要求每天训练模型或安排强制实验；
- M06结束后，应具备继续进入 Detection / Segmentation / Depth / Transformer / Robot Learning / VLM / VLA 的理论底座。

---

# Day27–Day33 Index

```text
Day27  Neural Network / Tensor / Dataset / DataLoader / Loss
Day28  Backpropagation / Computational Graph / Autograd
Day29  Training Loop / Gradient Descent / SGD / Adam
Day30  Softmax / Cross Entropy / Classification
Day31  CNN Foundations
Day32  Generalization / Normalization / Distribution Shift
Day33  Attention / Transformer Foundations
```
