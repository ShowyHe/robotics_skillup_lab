# M19 — Deployment / Data / Evaluation / Sim2Real

## Module Goal
把“算法能跑”推进到“真实机器人长期、稳定、可测量、可回归地运行”。主线：`Training/Algorithm → Artifact → Orin Runtime → CPU/GPU/Memory/I-O → Latency/Freshness → Data Pipeline → Evaluation/Regression → Sim→Real → Release/Monitoring/Data Reflow`。

本模块共 4 个理论 Day（Day119–Day122）。

---

# Day119 — Orin / CUDA-TensorRT Runtime / Latency / Resource Budget
1. 今日目标：理解机器人部署性能必须看完整runtime chain，而非只看model forward。
2. 前置：M01 runtime + M03 timing + M06 DL + M13 latency。
3. 必须教学：training code/weights/export graph/runtime engine/ROS node区别；inference vs training；ONNX/TensorRT类artifact；export operator/dynamic-shape/数值等价验证；FP32/FP16/INT8与量化校准概念；NVIDIA Orin CPU/GPU/DRAM/功耗共享资源的工程视角；CUDA execution model基础：host/device、kernel、stream/asynchronous execution概念；H2D/D2H copy；batching/zero-copy概念；TensorRT engine optimization定位；profiling：CPU、GPU utilization、VRAM/RAM、copy、pre/post/inference stage；latency vs throughput；frequency vs freshness；queue backlog与drop-old/latest-only；resource contention（LIO+YOLO+VLA+RViz）；cold warm-up vs steady-state；watchdog；Docker/systemd在部署中的职责：环境固定、进程启动/重启/日志/资源；end-to-end timestamp instrumentation。
4. 深度：Latency/throughput/freshness/profiling L5；CUDA/TensorRT L2-L3。
5. 工程连接：Orin多模块集成、LIO 10Hz但pose age 0.4s。
6. 不展开：CUDA kernel编程、TensorRT API细节。
7. 考核：30FPS为何可能200ms latency；设计sensor→command latency测量。
8. 毕业考点：Orin Runtime、Profiling、Latency、Freshness、Resource Budget。

# Day120 — Robot Data Pipeline / Logging / Dataset Version / Data Reflow
1. 今日目标：把rosbag/log变成可追溯、可训练、可评测的数据产品，并建立failure mining→数据回流。
2. 前置：M03/M10 timing + M15/M17 datasets。
3. 必须教学：raw sensor/processed estimate/command/feedback/event区别；minimal reproducible bag；必须同时记录能重建decision的input/output；measurement/header/arrival/experiment time；clock domains；multi-sensor sync/nearest/interpolation；missing/dropout/TF缺失；metadata：robot ID/software SHA/model/config/calibration/environment/task/operator/outcome；dataset version/changelog；label provenance；success/failure/intervention/collision labels；episode/scene/object/robot/day split与data leakage；hard/near-failure/recovery/OOD cases；data quality gate；privacy/access基本意识；**failure mining**：从incident/regression中自动/人工筛选高价值episode；**data reflow**：failure→curation/relabel→train/validate→new model→regression，避免只收数据不闭环。
4. 深度：Timestamp/versioning/data quality/reflow L5。
5. 工程连接：bag缺 `/gps/fix` 就不能验证GPS恢复。
6. 不展开：云端data lake/annotation平台工程。
7. 考核：设计Mobile Manipulation最小可复现实验记录集合及version metadata。
8. 毕业考点：Logging、Sync、Versioning、Failure Mining、Data Reflow。

# Day121 — Evaluation / Benchmark / Regression / Statistical Reasoning
1. 今日目标：用可重复证据替代“视频看起来更顺”。
2. 前置：M04 + M15/M17 evaluation。
3. 必须教学：metric与system goal；component vs system metric；offline vs online/closed-loop；bag deterministic replay的边界；controlled variables；randomness/repeated trials；mean/variance/std/min-max；success rate与sample count；confidence直觉；tail risk/P50/P95/P99；latency distribution；benchmark scenario matrix；regression/golden cases；ablation；confounder；acceptance criteria预定义；failure taxonomy；release decision；统计显著性 vs engineering significance；effect size。
4. 深度：Metric/regression/release reasoning L5。
5. 工程连接：baseline vs new navigation/controller/VLA版本。
6. 不展开：完整假设检验课程。
7. 考核：3/3 vs 9/10为何不能直接判前者更好；设计A/B regression矩阵。
8. 毕业考点：System Metric、Regression、Ablation、Tail Risk。

# Day122 — Sim2Real / Progressive Deployment / Monitoring / Release Owner
1. 今日目标：解释Simulation/Offline正常但real失败，并形成可rollback的上线闭环。
2. 前置：M04 + Day119–121。
3. 必须教学：`P_sim≠P_real`；visual/sensor/dynamics/contact/timing/calibration domain gap；sensor noise/dropout/jitter；actuator delay/friction/slip/payload；domain randomization及合理range；system identification；measure what can be measured + randomize residual variation；real data fine-tune/adaptation概念；progressive deployment：offline→sim→bench→low-speed→controlled→hard cases→limited/full release；shadow mode；safety hard gates；runtime monitoring：latency/resource/dropout/success/intervention/anomaly；Docker/systemd/engine/model/config版本与rollback；canary fleet概念；real incident→minimal reproducer→regression；**version risk**：code/model/config/calibration/runtime任一组合变化均可能改变系统；release manifest；data feedback闭环；Owner release checklist。
4. 深度：Sim2Real taxonomy/deployment strategy L5。
5. 工程连接：MPPI sim好、real左右摆必须具体拆timing/dynamics/feedback等gap。
6. 不展开：advanced domain adaptation/meta learning。
7. 考核：VLA sim95% real45%构造visual/timing/dynamics/controller证据树；设计release/rollback计划。
8. 毕业考点：Sim2Real、Progressive Deployment、Monitoring、Rollback、Version Risk。

---

# M19 Graduation Exam
统一权重：**30%核心基础 / 50%综合系统场景 / 20% Source·Formula·Design**。

## 30% 核心基础
硬门槛：Orin resource model、latency/throughput/freshness、queueing、profiling、timestamp/sync/versioning、failure mining/data reflow、system metric/regression/ablation、Sim2Real timing/dynamics/calibration gap、progressive deployment、rollback/version risk。

## 50% 综合系统场景
至少覆盖：
1. detector inference 25ms但robot 450ms才反应的end-to-end latency；
2. 单模块正常、集成后GPU/CPU contention；
3. bag证据不足与重新录制设计；
4. baseline/new version小样本与confounder判断；
5. VLA/Perception Sim→Real失败树；
6. model更准但更慢导致closed-loop task下降；
7. release manifest/rollback/canary设计。

## 20% Source / Formula / Design
设计并解释stage timestamp `t_sensor,t_receive,t_pre,t_infer,t_publish,t_consume,t_cmd`；能读部署脚本/launch/systemd/Docker/model-config mapping，明确artifact/version；设计dataset/reflow和release pipeline。

## 通过标准
总分≥85%；必须明确 `Frequency≠Freshness`、`Throughput≠Latency`、`Offline Metric≠Closed-loop System Metric`；能够做有证据的Release/No-release判断并保证rollback。

## Day119–Day122 索引
```text
Day119 Orin / CUDA-TensorRT / Runtime / Profiling / Latency
Day120 Robot Data Pipeline / Logging / Version / Failure Mining / Data Reflow
Day121 Evaluation / Benchmark / Regression / Ablation
Day122 Sim2Real / Progressive Deployment / Monitoring / Rollback / Owner
```
