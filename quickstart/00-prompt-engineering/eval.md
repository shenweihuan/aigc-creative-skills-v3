# 提示词工程 (Prompt Engineering) — 评测套件 (Eval Suite)

> **Karpathy's Rule:** If you can't test it, you don't know if it works.
> 本文件提供标准化测试、质量基线和 before/after 验证。

---

## Skill 身份卡

| 属性 | 值 |
|------|---|
| Skill | 00-prompt-engineering |
| 层级 | QuickStart |
| 核心方法论 | 结构化提示词 = (主体×风格×参数) 的系统化构建与评审 |
| 触发词 | 「帮我写prompt」「提示词」「怎么写MJ/SD的描述」 |
| 典型输出 | 可直接用于 Midjourney / SD / DALL-E 的结构化提示词 + 评审诊断 |

---

## Test Prompts（3 个梯度）

### Test 1：最小可用性（新手能直接用吗？）
**Prompt:** "帮我写一个画一杯咖啡的 prompt"
**Expected Output Type:** 完整的结构化提示词（可直接复制使用）
**Must Include:**
- 明确的主体描述（咖啡杯的状态、角度、细节）
- 至少一个风格修饰词（不是泛泛的"好看"）
- 光线/氛围设定（具体而非抽象）
- 技术参数建议（--ar / 分辨率等，视平台而定）
- 负面提示词或避免项

**Quality Bar:**
- ✅ Good: "A steaming cup of coffee on a rustic wooden table, morning sunlight streaming through window, warm golden hour side-light, shallow depth of focus, cozy café atmosphere, Canon EOS R5 aesthetic, 85mm lens, --ar 4:5 --stylize 200"
- ❌ Bad: "a beautiful cup of coffee, very detailed, amazing lighting, high quality, masterpiece, 8k, ultra realistic"

### Test 2：核心能力（这个 skill 的独特价值在哪？）
**Prompt:** "我每次用 AI 生成的图都不满意，帮我的 prompt 做个诊断：'a girl in a forest with flowers, beautiful, detailed'"
**Expected Output Type:** 结构化诊断报告 + 改进版 prompt
**Must Include:**
- 问题识别（至少指出 2-3 个具体缺陷：太笼统/缺光线/无构图/形容词堆砌等）
- 每个问题附带"为什么这会导致生成质量差"的解释
- 给出改进后的完整 prompt（标注改动了哪些部分）
- 解释改动背后的原理（对应到方法论中的某个原则）

**Quality Bar:**
- ✅ Good: 逐条分析 → 指出"a girl""forest""flowers"都缺乏具体性 → "beautiful/detailed"是无效词 → 给出带时间/光线/视角/情绪的完整重写版
- ❌ Bad: 只说"这个prompt不够好，建议加更多细节"但没有具体指导

### Test 3：大师级（真正的高手怎么用？）
**Prompt:** "我想做一个系列图，主题是'四季的情绪'，要求风格统一但每张有明显的季节差异，帮我设计一套系统性 prompt 方案"
**Expected Output Type:** 完整的系列创作方案（含变量矩阵 + 统一锚点 + 差异化策略）
**Must Include:**
- 系列整体的设计哲学（统一的视觉基因是什么）
- 变量矩阵（季节 × 光线 × 色调 × 情绪 × 构图元素）
- 共享的风格锚点（哪些元素在4张图中保持一致以保证系列感）
- 每张图的独立 prompt（标注差异化部分）
- 一致性检查清单（如何验证4张图确实属于同一系列）

**Quality Bar:**
- ✅ Good: 定义"统一锚点=手绘插画风+单一主角+固定构图框架"→ 变量矩阵覆盖春夏秋冬的光线/色彩/活动差异 → 4个完整prompt + 一致性自检表
- ❌ Bad: 只是分别写了4个独立的prompt，没有系统性的统一策略

---

## Failure Cases（2 个）

### Failure 1: 参数堆砌机器
**When:** 用户需求简单，但输出变成了关键词大杂烩
**What Goes Wrong:** 输出一堆技术术语和参数（--stylize 750 --chaos 100 --weird 2000...），但完全忽略了审美考量和创意意图
**Why:** 过度依赖"模板思维"，把提示词工程理解成了"填空题"而不是"创作辅助"
**Fix:** 回归核心原则——每个词都必须服务于视觉意图，先问"我想表达什么"，再决定用什么参数

### Failure 2: 模板化扼杀创意
**When:** 用户有独特的创意想法，但输出被强行塞入固定模板
**What Goes Wrong:** 不管用户想表达什么，输出的 prompt 都长一个样子（主体+环境+光线+风格+技术），失去了创意的灵活性和意外性
**Why:** 把方法论当成了教条，忘记了"原则是拐杖，不是枷锁"
**Fix:** 根据用户的创意类型调整结构——有时候诗意化的散点描述比结构化更有效；保留"灵魂空间"让AI有发挥余地

---

## Style Fingerprint（风格指纹）

> 这个 skill 的输出应该有什么独特的"味道"？用来区分它和其他 skill。

**Good Output Smells Like:**
- **结构中有呼吸感**：不是死板的填空，而是在框架内留有创意空间
- **审美判断力可见**：能说出"为什么选这个词而不是那个词"的理由
- **教学意识**：不只给答案，还解释背后的思考逻辑
- **平台敏感**：针对 MJ/SD/DALL-E 的不同特性给出差异化建议

**Bad Output Smells Like:**
- **通用 AI 味**："这是为您精心设计的prompt..."然后是一堆漂亮废话
- **关键词沙拉**：masterpiece, best quality, highly detailed, 8k, ultra realistic 全堆上去
- **没有审美立场**：不敢做取舍，什么都想要

---

## Before / After

**Before:** 用户写 prompt 靠直觉和运气，生成结果不可预测，反复试错成本高
**After:** 用户拥有系统化的提示词构建能力 + 自检诊断工具，能预判生成效果并主动优化
**One-Line Delta:** 从"碰运气写 prompt"到"有意识地设计 prompt"

---

## Scorecard

| 维度 | 权重 | 标准 | 自评 |
|------|------|------|------|
| 风格准确度 | 30% | 输出是否符合该skill的美学/方法论标准 | _/10 |
| 可执行性 | 25% | 用户能否直接用输出结果 | _/10 |
| 区分度 | 20% | 和通用AI输出的差异是否明显 | _/10 |
| 深度 | 15% | 是否触及底层原理而非表面技巧 | _/10 |
| 鲁棒性 | 10% | 边缘情况处理 | _/10 |
| **总分** | **100%** | | **_/50** |
