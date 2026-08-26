# M07 — Deep Vision & 3D Perception

## Module Goal
把 M05 的视觉几何与 M06 的深度学习接到机器人 world representation：

```text
Camera / Depth / LiDAR
↓
Deep Feature
↓
Detection / Segmentation / Depth / 3D Detection
↓
2D / 3D Geometry
↓
PointCloud / Voxel / BEV / Occupancy
↓
Tracking / World Model
↓
Navigation / Manipulation / VLA
```

最终不能停留在“模型检测到了目标”，而必须能继续回答：目标在什么坐标系、几何位置是什么、如何进入机器人 world model、置信度和时效性如何解释、错误会在哪一层传播。

本模块共 6 个理论 Day（Day34–Day39），默认不安排强制实验。

---

# Day34 — Classification、Detection 与 YOLO 基础

## 1. 今日目标
严格区分 classification、localization、object detection、confidence、class score、bounding box、IoU、NMS，以及 anchor / anchor-free 基本思想。

## 2. 前置知识
M05 + M06，尤其是 Camera image、CNN feature、classification、loss、tensor shape。

## 3. 必须教学内容
1. Classification vs Detection：`image→class` 与 `image→multiple objects→class+box+score`。
2. Bounding Box：`(x,y,w,h)`、`(x1,y1,x2,y2)`，pixel 与 normalized coordinate。
3. Box Regression：连续位置参数回归。
4. Detection Head：`Backbone→Feature→Detection Head→Box+Class+Confidence`。
5. Multi-scale Detection：小目标/大目标需要不同 spatial scale feature。
6. Confidence：区分 objectness、class confidence、final score；不同 YOLO 版本定义可能不同，不能把所有 confidence 当成同一个量。
7. IoU：`IoU=|A∩B|/|A∪B|`，必须会手算简单例子。
8. NMS：保留高分 box，抑制与其高度重叠的低分候选。
9. Anchor-based vs Anchor-free：理解设计思想，不讲版本历史。
10. Detection Loss：box loss + classification loss + objectness/quality related loss 的结构概念。
11. TP / FP / FN、Precision / Recall 初步，为 Day39 做准备。

## 4. 深度要求
Detection pipeline L3；IoU L3；NMS L3；YOLO architecture L2-L3；具体版本细节 L1-L2。

## 5. 工程连接
Pedestrian detection、obstacle detection、traffic sign、object finder、grasp object detection。

## 6. 明确不展开
某一 YOLO 版本完整源码、全部 loss 系数、TensorRT 部署、3D Detection 细节。

## 7. 本日考核点
1. Classification 和 Detection 区别？
2. Bounding box 有哪些表示方式？
3. 为什么 box prediction 是 regression？
4. IoU 表示什么并手算简单例子。
5. NMS 解决什么问题？
6. 高 confidence 是否一定是真物体？
7. Precision / Recall 区别？
8. 小目标为什么通常更难？
9. 给 YOLO 输出，指出机器人真正还缺哪些空间信息。

### M07毕业考试核心考点
Detection pipeline、IoU、NMS、confidence interpretation。

---

# Day35 — Semantic Segmentation、Instance Segmentation 与 Traversability

## 1. 今日目标
理解 bounding box 与 pixel-level perception 的差异，并能把 segmentation 与机器人 traversability / semantic world model 连接起来。

## 2. 前置知识
Day34 + M05 pixel geometry。

## 3. 必须教学内容
1. Semantic Segmentation：每个 pixel → semantic class。
2. Instance Segmentation：同类不同实例必须分开，例如 person #1 / #2。
3. Detection vs Segmentation：box 粗位置 vs pixel 级形状，精度/计算/标注 trade-off。
4. Segmentation Tensor：常见 `C×H×W`，每 pixel 对各类别有 score。
5. Per-pixel Classification 与空间结构建模。
6. Encoder / Decoder：`image→encoder→compressed feature→decoder/upsampling→pixel map`。
7. Upsampling 与 spatial resolution。
8. Boundary Problem：物体边缘最容易出错。
9. Free Space / Traversability：`road ≠ 一定可通行`，`grass ≠ 一定不可通行`；必须区分 object class、semantic class 与 traversability。
10. Semantic Map：segmentation 经过 geometry 可形成 semantic point cloud / BEV / occupancy。

## 4. 深度要求
Semantic/instance L3；segmentation architecture L2-L3；traversability reasoning L3-L4。

## 5. 工程连接
Sidewalk、grass、blind path、road boundary、semantic obstacle、local costmap。

## 6. 明确不展开
Mask R-CNN源码、SAM完整架构、panoptic segmentation深入、BEV完整算法。

## 7. 本日考核点
1. Semantic 和 instance segmentation 区别？
2. 为什么 box 不能准确表示人形障碍？
3. segmentation tensor 各维是什么？
4. encoder/decoder 各负责什么？
5. road class 是否等于 traversable？
6. segmentation 如何进入 costmap/world model？
7. pixel class 错误会怎样影响 navigation？

### M07毕业考试核心考点
Semantic vs instance、pixel prediction、traversability、robot integration。

---

# Day36 — Learned Depth：Monocular Depth、Metric / Relative Depth 与 Distribution Shift

## 1. 今日目标
把 M05 已学的 Stereo/RGB-D 几何作为快速复习对照，真正新增学习 learned depth：metric / relative depth、monocular ambiguity、scale ambiguity、distribution shift 与 temporal consistency。

## 2. 前置知识
M05 Day25–26 + M06。

## 3. 必须教学内容
1. **M05快速复习，不重复完整教学**：Stereo / RGB-D 作为 geometry-driven depth 对照。
2. Metric Depth：输出具有真实物理尺度，例如 2.3 m。
3. Relative Depth：可靠表达前后关系，但不保证绝对米制尺度。
4. Monocular Ambiguity：单 RGB 无几何 baseline，绝对深度天然存在歧义。
5. Learned Monocular Depth：依赖 training data 中的视觉统计规律和 prior，不是单帧严格三角测量。
6. Stereo / RGB-D vs Learned Depth：geometry/sensor driven vs data-prior driven。
7. Scale Ambiguity：几何形状可能对，但整体尺度错误。
8. Depth Error vs Distance：远距离通常更困难。
9. Depth Map → PointCloud：`(u,v,Z)→K^-1/back projection→(X,Y,Z)`，复用 M05，不重新从头推。
10. Invalid Depth / Confidence：missing、reflective、transparent、sky、far range 等。
11. Temporal Consistency：连续帧 depth 抖动会直接污染 world model。
12. Distribution Shift：昼夜、隧道、雨天、场景变化可能产生系统性 learned depth 错误。

## 4. 深度要求
Metric vs relative L3；monocular ambiguity L3；depth→3D L4（复用）；learned depth limitations L3-L4。

## 5. 工程连接
Obstacle distance、visual navigation、BEV、grasp distance、pure-vision navigation。

## 6. 明确不展开
Stereo完整推导（已在 M05）、self-supervised depth loss、NeRF、foundation depth模型细节。

## 7. 本日考核点
1. Metric 与 relative depth 区别？
2. 单目为什么有 scale ambiguity？
3. Learned monocular depth 是否是几何三角测量？
4. Stereo/RGB-D 与 learned depth 本质差异？
5. scale 错 20% 如何影响机器人？
6. temporal depth 抖动如何影响 navigation？
7. 为什么室内训练模型到室外可能失效？

### M07毕业考试核心考点
Depth source、metric/relative、scale ambiguity、depth→3D、distribution shift。

---

# Day37 — PointCloud、Filtering、Voxel Downsampling、KD-tree 与 3D Detection Basics

## 1. 今日目标
理解 point cloud 作为 3D geometry 数据结构，以及基础预处理、空间索引、surface normal 与 3D detection 输出。

## 2. 前置知识
M05 + Day36。

## 3. 必须教学内容
1. Point 可以包含 xyz、intensity、RGB、normal、semantic label、timestamp。
2. Organized vs Unorganized PointCloud。
3. Frame：PointCloud 中的 XYZ 永远必须属于明确 coordinate frame。
4. Range Filtering。
5. Statistical / Radius Outlier 基本思想。
6. **Voxel Grid Downsampling**：把点云按 voxel cell 压缩以减少点数；这是“降采样方法”，不是 Day38 的“voxel world representation”。
7. Density：LiDAR 点云密度与 distance、scan pattern、sensor geometry 有关。
8. Nearest Neighbor 与 KD-tree：KD-tree 是加速空间查询的索引结构，不展开构造证明。
9. Surface Normal：利用局部邻域估计 plane / normal。
10. Point-to-point / Point-to-plane 只建立直觉，完整 ICP 在 M10。
11. Semantic PointCloud：`2D semantic + depth/LiDAR + calibration → 3D semantic points`。
12. **3D Detection Basics**：3D box 常表示 `x/y/z + l/w/h + yaw + class/score`；2D box 与 3D box 的空间含义不同；Camera/LiDAR 3D Detection 只讲概念；所有 3D box 必须属于明确 frame。

## 4. 深度要求
PointCloud frame L4；filtering/downsampling L3；KD-tree L2-L3；normal L3；3D Detection L2-L3。

## 5. 工程连接
Livox、local obstacle、FAST-LIO、3D mapping、semantic navigation、3D object world model。

## 6. 明确不展开
PCL API、ICP完整推导、octree详细实现、PointPillars/CenterPoint网络细节、point cloud neural network。

## 7. 本日考核点
1. point 除 XYZ 还可以包含什么？
2. PointCloud 为什么必须有 frame？
3. voxel downsampling 的 voxel size 增大会怎样？
4. KD-tree为什么有用？
5. normal 的几何意义？
6. semantic point cloud 如何生成？
7. 2D box 与 3D box 的信息差异？
8. 一个 3D box 若没有 frame 为什么不可直接给 planner？

### M07毕业考试核心考点
PointCloud representation/frame、voxel downsampling、nearest neighbor、semantic 3D、3D box basics。

---

# Day38 — Voxel Representation、Occupancy、BEV 与 Robot World Representation

## 1. 今日目标
理解机器人为什么要把 raw image / point cloud 转换成更适合规划、控制与决策的空间 world representation。本日核心只锁定：**Occupancy + BEV + BEV/Costmap责任边界**。

## 2. 前置知识
Day34–37。

## 3. 必须教学内容
1. Representation Problem：image、point cloud、voxel、occupancy grid、BEV、object list、semantic map 服务不同下游任务。
2. **Voxel Representation**：3D 空间离散 cell，可存 occupied/free、density、semantic feature、learned embedding；与 Day37 voxel downsampling 明确区分。
3. Occupancy：occupied / free / unknown；**unknown ≠ free** 为硬门槛。
4. 2D Occupancy Grid：世界映射到 XY grid 的 free/occupied/cost 表达。
5. BEV：top-down spatial representation，适合 ground-plane mobile robotics。
6. Image → BEV：`image feature + camera geometry / learned lifting → 3D/pseudo-3D feature → ground-plane aggregation → BEV`，只讲结构。
7. LiDAR → BEV：按 XY 空间聚合 point cloud。
8. Semantic BEV：cell 可表达 road、person、vehicle、grass、blind path、traversability 等。
9. YOLO vs BEV：YOLO 是 image object detection；BEV 是 spatial world representation，两者不是替代关系。
10. BEV vs Costmap：BEV 属于 perception/world representation；Costmap 是 planning-oriented cost representation；可能存在 `Semantic BEV→rule/fusion→Costmap`。
11. Occupancy Prediction：网络可直接预测空间 occupancy，L2-L3理解。
12. Resolution / Range Trade-off：cell越细，空间精度高但计算/内存成本高。
13. Temporal Fusion：连续帧融合需要考虑 ego-motion，L2-L3。
14. Coordinate Alignment：Camera/LiDAR/BEV/world frame 必须一致。

## 4. 深度要求
Occupancy L3-L4；BEV L4；BEV vs Costmap L4；representation trade-off L3-L4；temporal fusion/occupancy prediction L2-L3。

## 5. 工程连接
Pure vision navigation、Semantic BEV、local costmap、obstacle layer、traversability、Milo类视觉导航架构。

## 6. 明确不展开
BEVFormer、Lift-Splat-Shoot详细网络、3D Gaussian、Occupancy Network具体论文、Nav2 costmap具体插件算法。

## 7. 本日考核点
1. 为什么需要 world representation？
2. Day37 voxel downsampling 与本日 voxel representation 有什么区别？
3. unknown 为什么不能当 free？
4. BEV 为什么适合 mobile robot？
5. YOLO 和 BEV 最大区别？
6. BEV 是否等于 costmap？
7. Semantic BEV 如何进入 costmap？
8. resolution越细有什么代价？
9. image / LiDAR 如何概念上变成 BEV？
10. extrinsic错会如何影响 BEV？

### M07毕业考试核心考点
Voxel representation、occupancy、unknown/free、BEV、BEV→costmap/world model。

---

# Day39 — Tracking、Metrics 与 Perception→Robot System Integration

## 1. 今日目标
把 M07 串成完整机器人感知系统。核心是 **Metrics + Tracking + Perception→Robot Failure Chain**，不深入 tracking filter 数学。

## 2. 前置知识
Day34–38全部内容。

## 3. 必须教学内容
1. TP / FP / FN、Precision、Recall：`Precision=TP/(TP+FP)`，`Recall=TP/(TP+FN)`。
2. IoU Threshold 与 TP/FN 判断。
3. Precision-Recall Trade-off：threshold 改变如何影响二者。
4. AP / mAP：理解汇总意义，不做复杂积分手算。
5. Segmentation Metrics：pixel accuracy、IoU、mIoU。
6. Depth Metrics：absolute error、relative error、RMSE。
7. Tracking：单帧检测不能回答连续帧是否为同一对象。
8. Data Association：geometry、appearance、motion prediction等用于 observation↔track 匹配。
9. Track State：ID、position、velocity、age、confidence。
10. Temporal Filtering：smoothing、persistence、timeout；严格状态估计数学留 M09。
11. Stale Perception：当时正确但过旧的数据仍可能危险。
12. Detection → Robot Geometry：`Image Detection→Depth/Geometry→Camera3D→TF→Base/World→Track/World Model→Planner/Manipulation`。
13. Model Score vs Robot Decision Threshold：是否停车/绕行/抓取是系统决策，不等同模型score。
14. End-to-end Metric：mAP提高不自动等于 collision rate、clearance、task success更好。
15. Failure Attribution：Detection、Depth、Calibration、TF、Tracking、World Model、Costmap、Planner 的责任边界。

## 4. 深度要求
Precision/Recall L3；mAP L2-L3；tracking architecture L3；perception→robot integration L4；failure attribution L4。

## 5. 工程连接
Pedestrian avoidance、YOLO→costmap、object finder、visual navigation、VLA perception input。

## 6. 明确不展开
Kalman tracking数学、DeepSORT源码、MOT benchmark深入、安全认证。

## 7. 本日考核点
1. Precision / Recall公式？
2. FP和FN哪个更危险是否固定？
3. threshold提高通常如何影响precision/recall？
4. mAP能否直接代表机器人安全？
5. 为什么需要tracking和timeout？
6. stale perception是什么？
7. detector score与robot decision threshold是否相同？
8. box正确但机器人绕错位置可能有哪些跨模块原因？
9. 如何设计最终机器人层metric？

---

# M07 Graduation Exam Specification

## A. 核心理论专项 — 30%
必须覆盖：Classification vs Detection、Box、IoU（硬门槛）、NMS、Confidence、Semantic/Instance、Traversability、Metric/Relative Depth、Depth→3D（硬门槛）、PointCloud Frame（硬门槛）、Filtering/Voxel Downsampling、3D Detection basics、Occupancy、Unknown vs Free（硬门槛）、BEV、BEV vs Costmap、Precision/Recall、mAP、Tracking/Data Association、Stale Perception。

## B. 综合场景 — 50%
至少3组：
1. 行人避障：Detection→Depth→TF→World Position→Tracking→Costmap→Planner→Controller，明确“Detector看到了”不能证明整条链正确。
2. 纯视觉导航：RGB→Segmentation/Depth→Semantic BEV→Traversability→Costmap→Planner，解释数据、frame和failure mode。
3. 指标冲突：比较 mAP / Recall 不同模型，结合 FP/FN成本、system threshold、downstream behavior和real robot metric选择。

## C. Robot Perception Architecture — 20%
独立画：`Camera/LiDAR→Detection/Segmentation/Depth/3D Detection→3D Geometry→PointCloud/BEV→Tracking→World Model→Navigation/Manipulation/VLA`，并说明 data type、frame、uncertainty、timestamp、consumer。

## M07通过标准
- 总分 ≥85%；
- IoU、Depth→3D、PointCloud Frame、Unknown≠Free 为硬门槛；
- 必须理解 YOLO、Semantic BEV、Costmap 三者责任边界；
- 必须能从2D/3D网络输出一路推到robot world representation；
- 不接受“检测准，所以机器人就应该避得好”。

## Day34–Day39索引
```text
Day34  Classification / Detection / YOLO / IoU / NMS
Day35  Semantic / Instance Segmentation / Traversability
Day36  Learned Depth / Metric-Relative / Scale / Distribution Shift
Day37  PointCloud / Filtering / Voxel Downsampling / KD-tree / 3D Detection
Day38  Voxel Representation / Occupancy / BEV / Costmap Boundary
Day39  Tracking / Metrics / Perception→Robot Integration
```
