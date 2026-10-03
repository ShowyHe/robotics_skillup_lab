# M07 — Deep Vision & 3D Perception

## Module Goal
建立 `Image / LiDAR → Detection / Segmentation / Depth / 3D Geometry → PointCloud / Occupancy / BEV → Tracking / World Model → Navigation / Manipulation` 主线，并能区分模型指标与机器人系统指标。

本模块共 6 个理论 Day（Day34–Day39）。

---

# Day34 — Classification / Detection / YOLO / IoU / NMS
1. 今日目标：理解 object detection 的输出、训练/推理与后处理语义。
2. 前置：M06 CNN/softmax/generalization。
3. 必须教学：classification vs detection；bbox；class/score；IoU；TP/FP/FN；confidence threshold；NMS；YOLO-style backbone/neck/head 概念；anchor/anchor-free 只做概念；multi-scale detection；small object问题；score≠现实可信度；2D detection 只有 image geometry。
4. 深度：IoU/NMS/box L4；YOLO architecture L3。
5. 工程连接：pedestrian/object finder、visual navigation。
6. 不展开：YOLO各版本源码、loss全集。
7. 考核：手算IoU；解释NMS和threshold如何影响FP/FN。
8. 毕业考点：Detection、IoU、NMS、score semantics。

# Day35 — Semantic / Instance Segmentation / Traversability
1. 今日目标：理解像素级语义如何转成可通行区域与机器人world representation。
2. 前置：Day34。
3. 必须教学：semantic vs instance segmentation；mask/logit；pixel class；class imbalance；boundary error；traversability定义；road/grass/blind-path等语义；semantic≠geometry；unknown/uncertain region；mask→depth/BEV/world 的接口；distribution shift；temporal consistency。
4. 深度：Segmentation/Traversability L4。
5. 工程连接：纯视觉BEV导航、可通行区域。
6. 不展开：特定segmentation网络源码。
7. 考核：解释“语义是路”为什么仍不等于机器人一定能走。
8. 毕业考点：Semantic/Instance、Traversability、semantic-vs-geometry。

# Day36 — Monocular Depth / Stereo / RGB-D / Learned Depth
1. 今日目标：在复用 M05 深度几何的基础上，重点理解 learned depth 的尺度、可靠性与机器人 3D 接口。
2. 前置：M05 camera geometry + M03 timestamp/time。
3. 复用回顾（已通过则简短恢复，不重新完整教学）：stereo disparity-depth；RGB-D；pixel+depth→camera 3D；depth invalid/noise 基础；timestamp alignment。
4. 新增教学：metric vs relative depth；monocular scale ambiguity；learned monocular depth；learned-depth distribution shift；scale calibration；depth edge；depth uncertainty随距离变化；2D box center depth风险；mask/point sampling。
5. 知识连接：Stereo / RGB-D / Learned Depth 是不同 **Depth Source（深度来源）**，后端统一进入 `Pixel + Depth → Camera 3D → TF → base/world`；重点理解“换了深度来源，几何链不变，但 scale / validity / uncertainty 语义会变”。
6. 深度：learned-depth limits L3-L4；Depth Source→Metric 3D interface L4。
7. 工程连接：object 3D、obstacle projection、manipulation target、纯视觉导航。
8. 不展开：depth network训练细节；已稳定掌握的 Stereo / RGB-D 几何不重复长推导。
9. 考核：重点考 Metric vs Relative、Scale Calibration、Learned Depth failure、BBox/Mask depth sampling；Depth→3D 只在迁移场景检查，不机械重复旧题。
10. 毕业考点：Metric/Relative Depth、Depth→3D、scale/validity/time。

# Day37 — PointCloud / Filtering / KD-tree / Clustering / 3D Detection
1. 今日目标：理解3D点集合如何被过滤、组织、聚类并形成机器人可消费的几何对象。
2. 前置：M05/M03 + Day36。
3. 必须教学：point fields与frame；organized/unorganized cloud概念；crop/range/outlier filter；voxel downsampling；voxel size trade-off；nearest neighbor；KD-tree作用；local normal；Euclidean clustering；DBSCAN概念；cluster参数与过分割/欠分割；semantic point cloud；2D semantic+depth/LiDAR→3D；3D box `x/y/z+l/w/h+yaw+class/score`；2D vs 3D box；3D result必须有frame/timestamp。
4. 深度：PointCloud frame L4；filter/clustering L3；KD-tree/3D detection L2-L3。
5. 工程连接：Livox、local obstacle、3D object world model。
6. 不展开：PCL API、PointPillars/CenterPoint、point neural network。
7. 考核：解释 voxel size、clustering tolerance 改变的行为；无frame 3D box为何不可用。
8. 毕业考点：PointCloud、Voxel、Nearest Neighbor、Clustering、3D Box。

# Day38 — Voxel Representation / Occupancy / BEV / Costmap Boundary
1. 今日目标：理解raw perception如何变成规划/控制需要的spatial representation。
2. 前置：Day34–37。
3. 必须教学：voxel representation vs voxel downsampling；occupied/free/unknown；**unknown≠free**；2D occupancy grid；BEV；image→BEV概念；LiDAR→BEV；semantic BEV；occupancy prediction概念；YOLO vs BEV；BEV vs costmap；`Semantic BEV→rule/fusion→Costmap`；resolution/range/memory trade-off；temporal fusion与ego-motion；coordinate alignment。
4. 深度：Occupancy/BEV/Costmap boundary L4。
5. 工程连接：pure vision navigation、semantic traversability、local costmap。
6. 不展开：BEVFormer/LSS具体网络。
7. 考核：解释YOLO、BEV、Costmap三者责任；extrinsic错如何污染BEV。
8. 毕业考点：Occupancy、Unknown/Free、BEV、World Representation。

# Day39 — Tracking / Metrics / Perception→Robot Integration
1. 今日目标：从 Day34 的单帧检测指标推进到时间连续的 robot world model 与 closed-loop integration。
2. 前置：Day34–38。
3. 复用回顾（已通过则简短恢复）：TP/FP/FN；IoU 基础；confidence threshold 与 FP/FN 的关系。
4. 新增教学：Precision/Recall；IoU threshold在评估中的作用；AP/mAP；segmentation/depth metrics；tracking必要性；data association；track ID/position/velocity/age/confidence；persistence/timeout；stale perception；model score vs robot decision threshold；component metric vs end-to-end metric。
5. 知识连接：`Single-frame Detection → Data Association / Tracking → World Model → Planner / Manipulation`；把“这一帧看到了什么”升级成“机器人在时间上相信世界里有什么、在哪里、是否仍然新鲜”。
6. 系统归因：Detection→Depth→Calibration/TF→World/Track→Costmap→Planner/Manipulation，区分 component metric 与真实 robot behavior。
7. 深度：Metrics L3；tracking/integration/failure attribution L4。
8. 工程连接：pedestrian avoidance、YOLO→costmap、VLA perception input。
9. 不展开：Kalman tracking数学、MOT benchmark深入；不重新机械考 Day34 已稳定的 TP/FP/FN 定义。
10. 考核：mAP提升但robot更危险时如何查；box正确但world位置错有哪些层；stale track为何危险。
11. 毕业考点：Metrics、Tracking、Freshness、System Integration。

---

# M07 Graduation Exam
统一权重：**30%核心基础 / 50%综合系统场景 / 20% Source·Formula·Design**。

## 30% 核心基础
硬门槛：IoU、Depth→3D、PointCloud frame、Unknown≠Free；必须覆盖 Detection/NMS、Segmentation/Traversability、PointCloud filtering/clustering、3D box、BEV/Costmap boundary、Precision/Recall、Tracking/Freshness。

## 50% 综合系统场景
至少覆盖：
1. 行人避障：Detection→Depth→TF→Tracking→Costmap→Planner；
2. 纯视觉导航：RGB→Seg/Depth→Semantic BEV→Traversability→Costmap；
3. point cloud clustering参数导致障碍合并/分裂；
4. mAP/Recall更高但closed-loop安全指标下降；
5. stale perception或extrinsic错误导致world obstacle位置错误。

## 20% Source / Formula / Design
能读一个视觉/3D perception pipeline，定位 preprocess、model output、threshold/NMS、depth/3D projection、point filtering/clustering、BEV/world output、tracking/timeout，并说明每层 frame/timestamp/consumer。

## 通过标准
总分≥85%；必须理解 `Detector saw it ≠ Robot world model is correct`，并能从2D/3D输出追到真实机器人consumer。
