# 亚当斯·黑白风光 (Adams B&W Landscape) — 评测套件 (Eval Suite)

> **Karpathy's Rule:** If you can't test it, you don't know if it works.
> 本文件提供标准化测试、质量基线和 before/after 验证。

---

## Skill 身份卡

| 属性 | 值 |
|------|---|
| Skill | bw-landscape-adams |
| 层级 | Domain Master (视觉艺术·摄影) |
| 核心方法论 | 区域曝光系统(Zone System)：11级影调的精确控制 + 预可视化 + f/64极致清晰 + 三层构图法 |
| 触发词 | 「亚当斯」「黑白风光」「Ansel Adams」「区域曝光」「Zone System」 |
| 典型输出 | 亚当斯风格的摄影提示词/Zone System映射/ComfyUI工作流/影调分析 |

---

## Test Prompts（3 个梯度）

### Test 1：最小可用性（新手能直接用吗？）
**Prompt:** "模拟亚当斯分区曝光拍摄一座雪山"
**Expected Output Type:** Zone System 应用的基础提示词（体现亚当斯核心技术）
**Must Include:**
- Zone 分布规划（雪山的各个部分分别落在哪些Zone：雪顶=Zone VIII-IX/岩石阴影=Zone II-III/天空=Zone V-VII）
- 光线方向指定（亚当斯偏好侧光/侧逆光以增强立体感——雪山最适合的角度）
- 三层构图（前景：岩石或植被锚定 / 中景：山体主体 / 背景：天空或远处山峰）
- 大画幅相机模拟参数（f/64景深/8x10视角/胶片颗粒描述）
- 影调控制关键词（full tonal range / deep blacks with detail / bright highlights without clipping）
- 负面提示词（排除彩色/HDR/过度锐化/浅景深等反亚当斯元素）

**Quality Bar:**
- ✅ Good: "Ansel Adams Zone System photography of snow-capped mountain, [Zone plan: snow peaks VIII-IX with texture retained, granite face IV-VI showing rock detail, shadowed crevices II-III, sky V-VII with red filter effect], dramatic side-lighting from upper left creating hard-edged shadows across ridges, foreground: sharp boulders with frost texture, middle ground: towering monolith, background: stormy clouds with tonal depth, shot on 8x10 large format, f/64 everything sharp, full range from Zone 0 to Zone X, silver gelatin print aesthetic"
- ❌ Bad: "black and white mountain photo, Ansel Adams style, snow, dramatic, high contrast, detailed, 8k" —— 没有任何Zone System的应用意识

### Test 2：核心能力（这个 skill 的独特价值在哪？）
**Prompt:** "用亚当斯的视角拍摄城市建筑（不是自然风光）"
**Expected Output Type:** 技法跨题材迁移（将亚当斯的风光摄影方法论应用到城市/建筑摄影）
**Must Include:**
- 亚当斯技法在城市题材上的适用性分析（什么可以直接迁移/什么需要调整）
- 城市元素的 Zone 映射（玻璃幕墙=高光区域Zone VII-IX / 深色窗户=阴影Zone II-III / 混凝土纹理=中间调Zone IV-VI）
- 光线策略调整（城市峡谷的光线规律不同于自然风光——街道成为光隧道/建筑遮挡创造人造侧光/玻璃反射创造多重光源挑战）
- 构图适配（城市的前景选项：街道/车辆/行人/路灯；如何保持三层结构）
- 指出城市摄影特有的亚当斯式挑战（人工光源的色温复杂性/移动物体/符号干扰/法律限制）
- 至少1个"亚当斯会这样拍"的具体场景构想

**Quality Bar:**
- ✅ Good: "亚当斯如果拍城市会选择'城市峡谷'效应——摩天大楼之间的街道如同花岗岩缝隙，日出日落时的侧光如同自然光穿过山谷；玻璃幕墙的反射可以处理成类似水面的影调挑战；挑战在于夜间城市的多光源冲突——亚当斯会等待'黄金一小时'的城市侧光或者选择有统一天光的阴天"
- ❌ Bad: 只是在城市照片描述后面加了"Ansel Adams style"没有真正的技法迁移思考

### Test 3：大师级（真正的高手怎么用？）
**Prompt:** "设计一套 Stable Diffusion ComfyUI workflow 来实现亚当斯风格，要求可以从一张普通照片转换成亚当斯风格"
**Expected Output Type:** AI工具链整合（将亚当斯摄影理论映射到SD/ComfyUI的技术实现）
**Must Include:**
- 完整的节点流程图（从输入到输出的每个节点及参数设置）
- Checkpoint 模型推荐及理由（为什么选这个模型适合黑白摄影）
- LoRA 组合方案（胶片颗粒LoRA/对比度LoRA/如果有专门的Adam LoRA优先使用）
- ControlNet 应用策略（用什么预处理器来实现什么效果：Canny保持边缘/Depth保持构图/可能用IP-Adapter引入亚当斯风格参考）
- Zone System 的参数化映射（如何通过SD的参数模拟N-1/N/N+1显影控制：CFG/Steps/Sampler的影响）
- 后处理节点（影调曲线/颗粒添加/暗角/锐化——模拟暗房dodging and burning）
- 使用指南（用户如何操作这个workflow：输入什么/调整什么/输出什么）
- 已知局限（AI目前做不到的亚当斯技法：真正的光学景深/物理胶片响应/大画幅透视）

**Quality Bar:**
- ✅ Good: "Workflow: Input Image → Convert to Grayscale (if color) → ControlNet Canny (preserve edges, strength 0.85) → KSampler (Checkpoint: RealisticVision V5.1, Steps: 40, CFG: 8, Sampler: dpmpp_2m) → Detailer Fix (face/texture restoration) → Upscale (ESRGAN 4x) → Tone Curve Node (S-curve simulating Grade 2 paper) → Grain Node (Gaussian 1.5% monochrome) → Vignette (-15) → Output。LoRA: Add More Details (0.6) + Film Grain B&W (0.7)。Limitation: 无法真实模拟f/64的光学景深，只能近似"
- ❌ Bad: 只说"用SD生成黑白图然后调成高对比度"没有任何技术深度

---

## Failure Cases（2 个）

### Failure 1: "黑白对比"≠ Zone System
**When:** 用户要求亚当斯风格时
**What Goes Wrong:** 输出只是"高对比度黑白照片"，完全没有11级影调的概念，丢失了亚当斯最核心的贡献
**Why:** 把亚当斯等同于"黑白+对比度高"，忽略了Zone System的精髓——不是极端对比而是**有控制的完整影调范围**
**Fix:** 强制Zone分布检查——任何亚当斯风格输出必须包含至少7个可辨识的Zone层次（0-X的跨度），不能只有黑和白两级

### Failure 2: 曝光逻辑错误
**When:** 描述复杂光影场景时
**What Goes Wrong:** 影调分配违反物理常识（例如：阴影区域比受光区域更亮/本应是Zone II的地方描述成了Zone VII）
**Why:** 只关注"亚当斯风格"的视觉效果而忽略了亚当斯对光线物理的严谨理解——他是技术完美主义者
**Fix:** 在提示词中使用"统一光源方向+一致的影调逻辑"约束；生成后用Zone Checklist验证每个主要区域的影调合理性

---

## Style Fingerprint（风格指纹）

**Good Output Smells Like:**
- **技术宗教感**：亚当斯的方法论有一种近乎科学的虔诚——每个影调都有它的位置
- **崇高的寂静**：不是安静，是一种面对伟大事物时的肃穆
- **石头的质感**：你能"摸"到岩石表面的每一个纹理（f/64的极致清晰）
- **光的雕塑**：光线不是照亮场景，而是在雕刻场景

**Bad Output Smells Like:**
- **Instagram 黑白滤镜**：一键黑白+对比度+清晰度——没有灵魂
- **HDR黑白**：影调过度丰富反而失去了亚当斯的"选择性控制"
- **软焦浪漫**：亚当斯是最不浪漫的摄影师——他的清晰度是对柔焦美学的宣战

---

## Before / After

**Before:** 用户拍/生成黑白照片只知道"转黑白+拉对比度"，结果要么灰蒙蒙要么死黑死白
**After:** 用户拥有Zone System的思维工具，能在按下快门前预可视化最终影调，精确控制从纯黑到纯白的每个层次
**One-Line Delta:** 从"黑白照片"到"11级影调的精确编排"

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
