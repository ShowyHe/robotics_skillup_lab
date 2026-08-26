# robotics_skillup_lab

面向 **机器人全栈工程 + 具身智能（Embodied AI）+ VLA** 的长期学习与能力升级仓库。

最终目标：

> **研究生级机器人理论基础 + 真实机器人全栈工程能力 + VLA / Mobile Manipulation具身智能能力 + 系统 Owner 能力。**

当前课程大纲已完成二次审计并进入 **Curriculum v1.0冻结**：后续优先正式学习、考试和真实Foundation Debt修复，不再因为“还能再加知识”持续扩Module或Day。

## 1. 总体能力链
```text
Sensors / Actuators
        ↓
C++ / Linux / ROS2 Runtime
        ↓
Perception / World Representation
        ↓
State Estimation / SLAM
        ↓
Planning / Navigation
        ↓
Kinematics / Dynamics / Control
        ↓
Manipulation
        ↓
Robot Learning
        ↓
VLM / VLA
        ↓
Mobile Manipulation
        ↓
Deployment / Data / Evaluation / Safety
        ↓
Research / Capstone / Foundation Cleanup
```

## 2. 学习方式
采用 **“专业模块反推基础”**：
```text
真实问题 / 专业模块
→ 反推真正需要的数学 / CS / 物理基础
→ 先补前置
→ 学专业理论
→ 连接真实源码 / 官方实现
→ 必要时最小自实现或独立 LAB
→ Quiz / Module Graduation Exam
→ Foundation Debt 复测
```

正常学习日按 **2–3小时**设计。理论、数学、公式、算法、源码理解和真实案例映射是主线；**不要求每天代码，不要求每天实验**。

## 3. 源码学习原则
已有真实工程对应模块（尤其 Navigation / Control）：
```text
真实问题
→ 公司真实实现
→ 反推基础
→ 数学 / 算法本体
→ 官方标准实现
→ 必要最小自实现
→ 回真实系统验证
```

Robot Learning / VLM / VLA 等方向若没有公司实现，则以论文、官方项目、官方实现为主。源码数量不是目标。

## 4. 主课程与Day范围
```text
M00  Day1
M01  Day2–7
M02  Day8–15
M03  Day16–19
M04  Day20–21
M05  Day22–26
M06  Day27–33
M07  Day34–39
M08  Day40–48
M09  Day49–54
M10  Day55–62
M11  Day63–70
M12  Day71–79
M13  Day80–89
M14  Day90–96
M15  Day97–104
M16  Day105–109
M17  Day110–115
M18  Day116–118
M19  Day119–122
M20  Day123–126
M21  Day127–129
M22  Day130–135（动态 Foundation Cleanup）
```

固定主课程 Day1–Day129；M22预留动态槽位，具体内容必须根据真实Foundation Debt生成。

详见 [`docs/03_MASTER_PLAN.md`](docs/03_MASTER_PLAN.md)。

## 5. 文档结构
```text
docs/
├── 00_GOALS.md
├── 01_COMPETENCY_MAP.md
├── 02_DEPENDENCIES.md
├── 03_MASTER_PLAN.md
├── 04_MODULE_SPECS.md
├── LEARNING_RULES.md
├── PROGRESS.md
├── modules/               # M00–M22详细Day教学规格
├── lessons/               # 真正学习时逐日生成详细讲义
└── labs/                  # 少量必要LAB / Capstone
```

职责：
- `03_MASTER_PLAN.md`：总Day数、Module范围、总索引；
- `04_MODULE_SPECS.md`：Module知识边界和毕业能力；
- `modules/`：每个Day的Teaching Contract，不是当天长篇讲义；
- `lessons/`：真正学习到对应Day时生成；
- `labs/`：只有不可被理论替代的实验；
- `PROGRESS.md`：当前学习节点、薄弱点、复测与Foundation Debt。

## 6. 掌握等级
- **L1：见过**
- **L2：能解释**
- **L3：能计算 / 推导**
- **L4：能实现 / Debug**
- **L5：能迁移 / 修改 / 设计**

核心数学至少L3；核心机器人专业理论L3→L4；Navigation / Control / System Owner / VLA关键能力逐步向L5。

## 7. 统一考试结构
普通 Module Graduation Exam 默认：
- **30% 核心基础**
- **50% 综合系统场景**
- **20% Source / Formula / Design**

默认总分 **≥85%**；Hard Gate基础概念不能靠其他题得分抵消。M00为1-Day总纲，保留轻量Owner场景考试例外。

## 8. 正式LAB
当前锁定3个：
- `docs/labs/LAB01_manipulation_pick_and_place.md`：Manipulation全链；
- `docs/labs/LAB02_mobile_manipulation_capstone.md`：Mobile Manipulation端到端，并可扩展为M21最终Research Capstone；
- `docs/labs/LAB03_robot_policy_vla_action_interface.md`：Robot Learning/VLA的Dataset→Policy→Action→Safety→Controller闭环。

LAB用于验证真实闭环、frame、collision、execution、contact、learned action、recovery和跨模块Owner能力，不为了课程形式凑数量。

## 9. v1.0审计重点
本轮收口已明确：
- M04/M09/M10考试统一30/50/20；
- 修复M11/M15前置倒挂；
- M13稳定性语境与controllability/observability符号；
- M14 force/impedance convention；
- M15 ACT/CVAE、Diffusion最小数学链；
- M07 point-cloud clustering；
- M19 Orin power/thermal/memory bandwidth；
- M21单日≤20核心点并与Capstone闭环；
- M22增加Constrained Optimization、Algorithm Theory、CMake/colcon、Python/NumPy动态候选池；
- 增加LAB03 learned policy/VLA action-interface实践。

## 10. 最终验收
课程完成不以“看完多少Day”为标准，而以是否能够：
1. 从数学/物理解释核心机器人算法；
2. 从sensor一路追到真实action/physical motion；
3. 判断跨模块故障责任；
4. 读懂并修改关键真实/官方实现；
5. 建立Perception→Estimation→Planning→Control→Manipulation→VLA完整联系；
6. 完成Mobile Manipulation / Embodied AI Capstone；
7. 真正跑通至少一次learned policy/VLA action→Safety→Controller闭环；
8. 建立数据、部署、评测、失败回流、Safety、Regression闭环；
9. 对证据不足的问题明确说“当前不能确定”，并指出下一步需要的证据。
