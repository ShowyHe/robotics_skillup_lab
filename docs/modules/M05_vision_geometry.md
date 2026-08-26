# M05 — Vision Geometry

## Module Goal
真正理解 pixel 是怎样通过相机模型、深度和坐标变换变成机器人世界中的 3D 点，为后续 Deep Perception、VIO、Manipulation、VLM/VLA grounding 建立几何底座。

---

## Day22 — Pinhole Camera Model 与图像形成

### 今日目标
理解 3D 世界为什么会映射成 2D 图像，并建立 Camera Geometry 的基础模型。

### 前置知识
M02：vector、matrix、coordinate、projection。

### 必须教学内容
1. Camera Coordinate Frame：理解 `X_c, Y_c, Z_c` 及光轴方向。
2. Pinhole Model：3D 点通过光心投影到 image plane。
3. Similar Triangle：必须推到 `x=fX/Z`、`y=fY/Z`，并解释 X/Y/Z 变化怎样影响图像位置。
4. Focal Length：决定投影尺度。
5. Image Plane：区分 physical image plane、normalized image plane、pixel grid。
6. Normalized Coordinates：`x_n=X/Z`、`y_n=Y/Z`。
7. Principal Point：`(c_x,c_y)`，不一定等于图像几何中心。
8. Pixel Coordinate：camera ray → normalized coordinate → pixel coordinate。
9. FOV：理解 focal length 与 FOV 的关系，不做复杂光学。
10. Perspective Effect：远处物体看起来更小是透视结果。

### 深度要求
Pinhole derivation L3；focal/principal point L3；perspective geometry L3。

### 工程连接
Camera detection、monocular image、object bounding box、visual navigation。

### 明确不展开
Lens optics、distortion、stereo、epipolar geometry、CNN。

### 本日考核点
1. 为什么 3D 可以投到 2D？
2. `x=fX/Z` 每项含义？
3. Z 增大时图像中的物体怎么变？
4. focal length 变大有什么视觉效果？
5. principal point 是什么？
6. normalized coordinate 为什么要除 Z？
7. pixel coordinate 与 camera coordinate 是否同一概念？

### M05毕业考试核心考点
Pinhole projection、normalized coordinate、focal/principal point。

---

## Day23 — Intrinsic、Extrinsic 与完整 3D→Pixel 投影链

### 今日目标
能够从 world/base frame 中的 3D 点，一直计算/解释到 image pixel。

### 前置知识
Day22 + M02 Day8–10。

### 必须教学内容
1. 多坐标系：严格区分 world、map、base_link、camera、normalized image、pixel。
2. Rigid Transform：`p_c = R_cw p_w + t_cw`；解释 R 与 t 的作用。
3. Homogeneous Coordinates 初步：认识 `[[R,t],[0,1]]` 如何统一 rotation + translation；完整 SE(3) 留 M08。
4. Extrinsic：Camera 相对另一个 frame 的位置与方向。
5. Intrinsic Matrix：必须掌握并逐项解释
   `K=[[f_x,0,c_x],[0,f_y,c_y],[0,0,1]]`。
6. Full Projection：理解 `p_pixel ~ K[R|t]P_world`，并拆成 world → camera → divide by Z → normalized → pixel。
7. Transform Direction：严格理解 `T_camera_base` 与 `T_base_camera` 不是同一个变换。
8. Source / Target Frame：与 TF2 语义对应。
9. Matrix Dimension Reasoning：逐层检查矩阵维度和乘法顺序。

### 深度要求
Intrinsic L3；Extrinsic L3-L4；Full Projection L3；Transform Direction L4。

### 工程连接
TF2、Camera frame、base_link、object detection 坐标转换。

### 明确不展开
Distortion、stereo、PnP、hand-eye。

### 本日考核点
1. Intrinsic 与 extrinsic 区别？
2. K 矩阵每个参数含义？
3. world point 如何变成 camera point？
4. 为什么 camera point 还不能直接叫 pixel？
5. 为什么要除 Z？
6. `T_base_camera` 与 `T_camera_base` 如何转换？
7. 给一组矩阵判断乘法顺序。
8. 画完整 3D→2D 链。

### M05毕业考试核心考点
Coordinate chain、K、extrinsic、transform direction。

---

## Day24 — Lens Distortion 与 Camera Calibration

### 今日目标
理解真实 Camera 为什么偏离理想 pinhole，以及 Calibration 究竟在估计什么、怎样判断标定质量。

### 前置知识
Day22–23。

### 必须教学内容
1. Ideal vs Real Camera：pinhole 是理想模型。
2. Radial Distortion：barrel / pincushion；离图像中心越远通常越明显。
3. Tangential Distortion：镜头与传感器安装不完美导致的偏移。
4. Distortion Coefficients：认识 `k1 k2 k3 p1 p2` 的类别和作用，不要求背高阶公式。
5. Undistortion：distorted pixel → calibration model → corrected pixel。
6. Intrinsic Calibration：估计 fx/fy、cx/cy、distortion coefficients。
7. Calibration Board：已知 geometry 为什么能约束 camera 参数。
8. Reprojection Error：known 3D / board point → estimated camera model → predicted pixel，与 observed pixel 的残差。
9. Calibration Error ≠ Calibration Success：工具跑通不代表参数质量足够；关注 pose coverage、reprojection error、data diversity。
10. Extrinsic Calibration：理解 Camera 与 LiDAR/base 的外参需求，但本日不做 SE(3) 优化推导。

### 深度要求
Distortion L2-L3；calibration objective L3；reprojection error L3-L4。

### 工程连接
物体定位偏移、camera-base 外参、robot grasp、perception overlay。

### 明确不展开
Zhang calibration 完整推导、nonlinear calibration optimizer、hand-eye 完整算法。

### 本日考核点
1. 为什么真实 camera 不完全符合 pinhole？
2. radial/tangential 区别？
3. intrinsic calibration 在求什么？
4. reprojection error 是什么？
5. reprojection error 小是否一定保证所有机器人任务准确？
6. 为什么 calibration 需要多个不同姿态？
7. camera-base extrinsic 错误如何表现？

### M05毕业考试核心考点
Distortion、calibration parameters、reprojection error、calibration quality。

---

## Day25 — Stereo、Depth 与 RGB-D Geometry

### 今日目标
理解深度从哪里来，以及如何把 `pixel + depth` 变成 Camera frame 中的 3D 点。

### 前置知识
Day22–24。

### 必须教学内容
1. Monocular Ambiguity：单张普通 RGB 图像中的一个 pixel 只定义 camera ray，不能唯一确定 3D depth。
2. Camera Ray：由 pixel 和 intrinsic 反投影得到方向。
3. Stereo Cameras：left/right camera、baseline、同一 3D point 的像素差。
4. Disparity：`d=u_L-u_R`。
5. Stereo Depth：`Z=fB/d`；分析 disparity 越小距离越远、远距离 depth 误差为何放大。
6. Baseline：增大 baseline 对远距离 depth 能力与工程安装/matching 的 trade-off。
7. RGB-D：区分 Stereo、Structured Light、ToF 的基本测距思路。
8. Pixel + Depth → 3D：必须会
   `X=(u-c_x)Z/f_x`，
   `Y=(v-c_y)Z/f_y`，
   `Z=Z`。
9. Depth vs Range：optical-axis depth 与 Euclidean range 不同。
10. Missing / Invalid Depth：hole、reflectivity failure、max range、sunlight interference 等真实问题。

### 深度要求
Stereo geometry L3；`Z=fB/d` L3；back-projection L4；RGB-D 原理 L2-L3。

### 工程连接
RGB-D grasp、obstacle location、3D perception、VLM grounding 到机器人坐标。

### 明确不展开
Stereo matching 算法、epipolar geometry 严格推导、learned depth。

### 本日考核点
1. 单 pixel 为什么不能确定 3D 位置？
2. Stereo 为什么能恢复 depth？
3. disparity 越小为什么越远？
4. `Z=fB/d` 中每项是什么？
5. baseline 变大有什么影响？
6. depth 与 range 是否相同？
7. 给 `u,v,Z,K` 计算 camera-frame XYZ。
8. 远距离 stereo 为什么更不稳定？

### M05毕业考试核心考点
Camera ray、disparity、stereo depth、back-projection。

---

## Day26 — Pixel → Camera → Base → World 完整机器人视觉几何链

### 今日目标
M05 最重要的一天。必须能够独立解释并计算：pixel → ray/depth → camera 3D → extrinsic → base_link 3D → robot pose / TF → world 3D。

### 前置知识
Day22–25 全部核心内容。

### 必须教学内容
1. Pixel Observation：object center `(u,v)` 仍不是机器人可执行目标位置。
2. Undistortion：先把 real camera measurement 映射回理想模型。
3. Back Projection：利用 intrinsic + depth 得到 `P_c=[X_c,Y_c,Z_c]^T`。
4. Camera → Base：严格使用 `P_b=T_(b←c)P_c`；强调 transform direction。
5. Base → World：利用 robot 当前 pose / TF，`P_w=T_(w←b)P_b`。
6. Complete Transform Chain：`P_w=T_(w←b)T_(b←c)P_c`；理解矩阵右侧先执行。
7. Timestamp Alignment：Camera measurement 属于 `t_camera`，应使用对应时刻的 robot pose / TF；运动机器人中时间错位会直接转为空间误差。
8. Error Propagation 直觉：pixel error + depth error + intrinsic error + extrinsic error + robot pose error + time error → world position error；暂不做 covariance propagation。
9. PnP 基本思想：已知 3D–2D correspondence，可反求 camera pose；不展开算法。
10. Hand-Eye Calibration 基本思想：Manipulator camera 需要求 camera ↔ robot / EEF extrinsic；不展开 `AX=XB`。
11. Navigation 连接：视觉 obstacle/object 变到 base/world 后，才能进入 world model / costmap / navigation logic。
12. Manipulation 连接：Object pose 必须进入 robot base / manipulator reference frame，IK / planning 才能使用。
13. VLM/VLA 连接：VLM 的语义 grounding 不是 action；仍需 geometry / state / action 接口把语义结果落到机器人执行空间。

### 深度要求
Complete coordinate chain L4；Transform Direction L4；Timestamp effect L3-L4；Error source reasoning L3-L4；PnP/hand-eye L1-L2。

### 工程连接
TF、Camera、robot pose、perception、navigation、manipulation、VLA。

### 明确不展开
PnP 求解算法、hand-eye `AX=XB` 推导、uncertainty propagation、VIO。

### 本日考核点
1. YOLO 给出的 pixel 为什么不能直接发给 robot？
2. `u,v,depth` 如何得到 camera-frame point？
3. 如何从 camera 变到 base？
4. 如何从 base 变到 world？
5. transform 连乘顺序怎么判断？
6. Camera timestamp 为什么影响 world coordinate？
7. 外参误差如何影响远距离目标？
8. robot pose 误差会不会影响 object world pose？
9. PnP 解决什么问题？
10. hand-eye 解决什么问题？
11. Navigation 如何消费视觉 3D 结果？
12. Manipulator 为何需要目标转换到指定 reference frame？

### M05毕业考试核心考点
Pixel→3D、Camera→Base→World、timestamp alignment、error chain。

---

# M05 Graduation Exam Specification

## A. 核心基础专项 — 30%
必须考：pinhole、projection、intrinsic、extrinsic、transform direction、distortion、calibration、stereo、back-projection。

## B. 综合视觉几何场景 — 50%
至少 3 组：

### 场景1：3D→Pixel
给 world point、robot pose、camera extrinsic、camera intrinsic，要求写完整投影链、矩阵维度和物理意义。

### 场景2：Pixel→3D→World
给 detection `(u,v)`、depth、K、`T_base_camera`、`T_world_base`，要求求/写目标 world coordinate。重点不是复杂数字，而是链路与 transform direction 正确。

### 场景3：视觉定位偏差
例如：图像检测框准确，但 robot 得到的目标位置总向右偏 20cm，且距离越远偏差越大。要求分析 intrinsic、distortion、extrinsic、depth、timestamp、robot pose 中哪些更可能，并说明验证方法。

## C. 机器人系统题 — 20%
给 `Camera → Detection → Depth → TF → World Model → Navigation / Manipulation`，要求判断每层数据含义、坐标系、时间语义和错误传播。

## Knowledge Coverage Matrix
- Pinhole Model：**核心必考**
- Normalized Coordinate：必考
- Intrinsic K：**核心必考**
- Extrinsic：**核心必考**
- Transform Direction：**硬门槛**
- 3D→2D Projection：**核心必考**
- Distortion：必考
- Calibration：必考
- Reprojection Error：必考
- Stereo / Disparity：必考
- Depth Geometry：必考
- Pixel+Depth→3D：**硬门槛**
- Camera→Base→World：**硬门槛**
- Timestamp Alignment：必考
- Error Propagation 直觉：必考
- PnP：理解题
- Hand-Eye：理解题

## 通过标准
- 总分 ≥85%；
- Transform Direction、Pixel→3D、Camera→Base→World 不得出现根本错误；
- 必须能够独立画出完整视觉几何链；
- 不接受“会调用 OpenCV 函数但不知道坐标怎样变化”；
- 核心单点错误采用定向补课 + 复测，不机械重学整个 M05。
