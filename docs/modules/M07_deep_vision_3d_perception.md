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
1. 今日目标：把 image-space output 连接到 metric 3D。
2. 前置：M05 camera geometry。
3. 必须教学：metric vs relative depth；monocular scale ambiguity；stereo disparity-depth关系复用；RGB-D；depth invalid/noise；depth edge；learned depth distribution shift；scale calibration；pixel+depth→camera 3D；depth uncertainty随距离变化；2D box center depth的风险；mask/point sampling；timestamp alignment。
4. 深度：Depth→3D L4；learned-depth limits L3-L4。
5. 工程连接：object 3D、obstacle projection、manipulation target。
6. 不展开：depth network训练细节。
7. 考核：给pixel/depth写back-projection；判断relative depth能否直接给Nav2米制障碍。
8. 毕业考点：Metric/Relative Depth、Depth→3D、scale/time。

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
1. 今日目标：把单帧模型指标接到动态机器人闭环。
2. 前置：Day34–38。
3. 必须教学：Precision/Recall；IoU threshold；AP/mAP；segmentation/depth metrics；tracking必要性；data association；track ID/position/velocity/age/confidence；persistence/timeout；stale perception；Detection→Depth→TF→World/Track→Planner/Manipulation；model score vs robot decision threshold；component metric vs end-to-end metric；failure attribution：Detection/Depth/Calibration/TF/Tracking/World Model/Costmap/Planner。
4. 深度：Metrics L3；tracking/integration/failure attribution L4。
5. 工程连接：pedestrian avoidance、YOLO→costmap、VLA perception input。
6. 不展开：Kalman tracking数学、MOT benchmark深入。
7. 考核：mAP提升但robot更危险时如何查；box正确但world位置错有哪些层。
8. 毕业考点：Metrics、Tracking、Freshness、System Integration。

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
