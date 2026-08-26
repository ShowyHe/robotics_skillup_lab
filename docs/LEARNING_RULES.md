# LEARNING_RULES — 教学、考试、实验与进度规则

## 1. 核心原则
本课程主要目标是补齐系统理论，而不是重复已有实操经验。正常学习日默认 **2–3小时**。

优先级：
```text
理论理解
> 数学 / 公式
> 算法机制
> 源码理解
> 真实工程案例映射
> 闭卷题 / 复测
> 必要实现
> 必要实验
```

实验不是每日必需，代码也不是每日必需。

---

## 2. 学习主线
对已有真实工程实现的模块，尤其Navigation / Control：
```text
真实问题
→ 公司真实实现
→ 反推缺失基础
→ 数学 / 算法本体
→ 官方标准实现
→ 必要时最小自实现
→ 回真实系统验证
```

Robot Learning / VLM / VLA 等没有公司现成实现时：
```text
论文 / 官方项目
→ 理论
→ 官方实现
→ 必要修改 / 复现
→ 训练或推理验证
```

源码数量不是目标。

---

## 3. Module Day 是Teaching Contract，不是当天讲义
`docs/modules/Mxx_*.md`中的每个Day固定定义：
1. 今日目标；
2. 前置知识；
3. 必须教学内容；
4. 必须达到的深度；
5. 工程 / 机器人连接；
6. 明确不展开内容；
7. 本日考核点；
8. 进入Module Graduation Exam的核心考点。

未来GPT：
- 必须覆盖Teaching Contract中的全部required content；
- 可以增加例子、图解、额外练习；
- 不可删除核心内容；
- 不可大规模提前拉入未来Module内容；
- 不可擅自改变课程主线或Day职责。

真正详细讲义只在学习到当天时写入 `docs/lessons/dayXXX.md`。

---

## 4. 普通理论Day要求
每个普通Day：
- 核心知识点 **不超过20个**；
- 单个知识点可以详细推导；
- 数学必须说明variable dimension、physical/geometric meaning和robot mapping；
- 关键公式不能只背结论；
- 已有工程案例优先用于映射新理论；
- 允许连续5–10个理论Day没有LAB；
- 不为了“今天有产出”强行写代码。

---

## 5. Daily Quiz
Daily Quiz目标是逐个验证关键知识点，而不是追求固定题数。

通常5–10题；知识点多时可以更多。可包含：
- concept / judgment；
- reasoning；
- small calculation；
- source-reading；
- robot transfer。

Hard Gate concept必须独立通过，不能靠其它题得分抵消。

---

## 6. Module Graduation Exam
默认统一权重：
- **30% 核心基础**
- **50% 综合系统场景**
- **20% Source / Formula / Design**

默认通过条件：
- 总分 **≥85%**；
- Hard Gate不得出现基础性错误；
- 总分通过但单个critical concept失败：只做targeted remediation + targeted retest；
- 不机械重考已经稳定掌握内容。

Module Exam优先使用2–4个真实系统场景综合多个知识点，并通过Knowledge Coverage Matrix防止关键内容漏考。

---

## 7. 掌握等级
```text
L1 见过
L2 能解释
L3 能计算 / 推导
L4 能实现 / Debug
L5 能迁移 / 修改 / 设计
```

目标：
- 基础数学关键知识至少L3；
- 核心机器人理论L3→L4；
- System / Navigation / Debug L4→L5；
- Control关键部分L4→L5；
- VLA长期主方向逐步L4→L5。

---

## 8. 入门诊断与跳级
每个Module开始前允许入口测试：
- **≥85% 且Hard Gate正确**：跳过已掌握部分；
- **70–85%**：只补薄弱知识；
- **<70% 或核心概念错误**：完整学习相关部分。

工程操作熟练不代表数学可跳；数学理论陌生也不降低毕业标准。

---

## 9. 实验规则
只有实验对能力形成明显增益时才安排。

应安排LAB/PROJECT：
- 理论无法替代真实运行直觉；
- 必须亲手实现才能理解关键机制；
- 新方向此前缺乏实践；
- 必须验证跨模块闭环；
- Capstone需要。

不应安排：
- 已有真实经验足以建立联系；
- 只是证明“今天学过”；
- 环境准备成本远大于知识收益；
- 仅运行Demo不增加理解。

LAB独立放 `docs/labs/LABxx_*.md`，复杂LAB可占多个学习时段，不塞进普通理论Day。

当前正式LAB：
- LAB01 Manipulation Pick-and-Place；
- LAB02 Mobile Manipulation Capstone。

---

## 10. 最小自实现规则
建议在明显帮助理论理解时自实现：
- A*；
- simplified KF/EKF；
- PID/LQR；
- simplified MPC/MPPI；
- Attention；
- small BC/policy。

已有源码阅读 + 推导足够时，不为了完成任务强行写实现。

---

## 11. 已有经验使用规则
已有ROS2、Nav2、Sensors、LIO、Planning、MPPI、真实机器人测试开发经验是学习资产。

优先：
```text
新理论
→ 映射到过去真实问题
→ 用理论重新解释现象
```

如果工程经验与公式/算法定义/官方实现/真实数据冲突，以可验证证据重新校准。

---

## 12. 源码边界
Navigation / Control源码：公司实现 + 官方实现足够，不追求仓库广度。

任何未实际读取的公司源码：
- 不得臆测函数逻辑；
- 不得把讨论中的假设写成源码事实；
- 只能写成“工程概念/待源码验证”。

---

## 13. PROGRESS更新规则
每次学习结束后按需要记录：
```text
Current Module / Day
Mastered
Weak
Wrong Understanding
Corrected
Retest
Source Reading Progress
LAB / Project Progress
Foundation Debt
Next
```

`PROGRESS.md`是跨聊天、跨GPT教学连续性的当前状态源。

Foundation Debt统一状态：
```text
OPEN → LEARNING → RETEST → CLOSED
```

优先级：P0 Hard Gate / P1 Core / P2 Module-specific / P3 Detail。

---

## 14. M22 Foundation Cleanup规则
M22不是预先固定的数学复习周。

Day130–Day135只是动态槽位，具体内容由M00–M21实际考试/源码阅读暴露的Foundation Debt生成。

每个M22 Day只处理：
- 1个主要foundation chain；
- 最多1个次要debt。

Debt CLOSED必须同时满足：
1. Definition；
2. Calculation / Derivation；
3. Transfer到新Robot场景；
4. Boundary：知道该知识不能证明什么。

禁止原题背答案、整模块无差别重学、把课程范围外高级细节强行列为P0。

---

## 15. Day / Module / Lesson / LAB职责
- `03_MASTER_PLAN.md`：总课程与Day索引；
- `04_MODULE_SPECS.md`：Module知识范围/毕业标准；
- `modules/`：每个Day的Teaching Contract；
- `lessons/`：真正当天使用的详细讲义；
- `labs/`：少量必要实验；
- `PROGRESS.md`：当前学习状态与Foundation Debt。

---

## 16. Future GPT接手规则
未来GPT接手前必须先读：
1. `00_GOALS.md`
2. `03_MASTER_PLAN.md`
3. `04_MODULE_SPECS.md`
4. `LEARNING_RULES.md`
5. `PROGRESS.md`
6. 当前 `docs/modules/Mxx_*.md`

不得重新询问仓库中已经明确的目标/规则；不得擅自增加每日实验；不得因用户工程经验强就默认数学已掌握；不得因理论陌生降低毕业标准。
