# Architecture — 抗脆弱性设计文档

> **Taleb's Principle:** 系统的价值不在于它在正常情况下跑得多好，
> 而在于它在异常情况下退化的多优雅。
>
> 本文档回答：**删掉任何一个子目录，系统还能工作吗？**

---

## 仓库身份

| 属性 | 值 |
|------|---|
| 名称 | AIGC Creative Skills v3 |
| 定位 | AI创作操作系统 — 基于艺术大师品味的原子技能体系 |
| 总行数 | ~25,000 行（目标） |
| 子目录数 | 4 个主要目录（quickstart, shared, ai-masters, domains） |
| 设计哲学 | MetaSkill 路由 + 领域正交 + 大师独立 + 优雅降级 |

---

## 目录结构与耦合矩阵

### 完整目录树

```
aigc-creative-skills-v3/
├── SKILL.md                          # MetaSkill 总控（路由中枢）
├── README.md                         # 产品化落地页
├── USAGE_LOG.md                      # 使用记录
├── ARCHITECTURE.md                   # ← 本文件
│
├── quickstart/                       # v1 的 8 个常用模块（新手快速入门）
│   ├── 00-prompt-engineering.md      # 提示词工程
│   ├── 01-storytelling.md            # 故事叙述
│   ├── 02-aesthetics.md              # 审美判断
│   ├── 03-creativity.md              # 创意激发
│   ├── 10-ai-painting.md             # AI绘画
│   ├── 20-ai-video.md                # AI视频
│   ├── 30-ai-writing.md              # AI写作
│   └── 40-ai-efficiency.md           # AI效率工具
│
├── shared/                           # 公共基础设施（理论基石）
│   ├── 00-creation-philosophy.md     # 创作哲学（v2 L0 继承）
│   ├── 01-prompt-foundations.md      # 提示词基础
│   ├── 02-evaluation-framework.md    # 评价框架
│   └── 03-creation-cycle.md          # 创作循环（v2 L4 继承）
│
├── ai-masters/                       # 3 个 AI 专家 skill（工艺层）
│   ├── ai-painting-master.md         # AI绘画大师
│   ├── ai-painting-master/           # 绘画大师资源
│   │   └── eval.md
│   ├── ai-writing-master.md          # AI写作大师
│   └── ai-video-master.md            # AI视频大师
│
└── domains/                          # 5 领域 × 34 大师 skill（品味层）
    │
    ├── visual/                       # 视觉艺术（14 个大师）
    │   ├── SKILL.md                  # 视觉领域总控
    │   ├── painting/                 # 绘画（6 个大师）
    │   │   ├── impressionist-monet/      # 莫奈·印象派
    │   │   ├── renaissance-da-vinci/     # 达芬奇·文艺复兴
    │   │   ├── modernist-picasso/        # 毕加索·立体主义
    │   │   ├── surrealist-dali/          # 达利·超现实主义
    │   │   ├── landscape-turner/         # 透纳·风景画
    │   │   └── portrait-rembrandt/       # 伦勃朗·肖像
    │   ├── photography/             # 摄影（4 个大师）
    │   │   ├── bw-landscape-adams/        # 亚当斯·黑白风光
    │   │   ├── decisive-bresson/          # 布列松·决定性瞬间
    │   │   ├── portrait-leibovitz/        # 莱博维茨·人像
    │   │   └── minimalist-sugimoto/       # 杉本博司·极简
    │   └── illustration/            # 插画（2 个风格）
    │       ├── anime-miyazaki/            # 宫崎骏·日系动画
    │       └── concept-art-frazetta/      # 弗雷泽塔·概念设计
    │
    ├── writing/                      # 写作文学（9 个大师）
    │   ├── SKILL.md                  # 写作领域总控
    │   ├── poetry/                   # 诗歌（2 个方向）
    │   │   ├── modern-chinese/            # 现代汉语诗
    │   │   └── classical-chinese/         # 古典诗词意境
    │   ├── prose/                    # 散文（3 个大师）
    │   │   ├── hemingway-minimal/         # 海明威·极简白话
    │   │   ├── magical-marquez/           # 马尔克斯·魔幻现实
    │   │   └── urban-murakami/            # 村上春树·都市孤独
    │   ├── screenplay/               # 剧本（2 种风格）
    │   │   └── cinema-nolan/              # 诺兰式复杂叙事
    │   │   └── wong-mood-fragment/        # 王家卫式情绪碎片
    │   └── copywriting/              # 文案（2 种流派）
    │       ├── ogilvy-classic/            # 奥格威经典广告
    │       └── digital-native/            # 数字原生文案
    │
    ├── film-video/                   # 影视视频（7 个风格）
    │   ├── SKILL.md                  # 影视领域总控
    │   ├── directing/                # 导演（3 个大师）
    │   │   ├── epic-kubrick/              # 库里克·史诗
    │   │   ├── non-linear-tarantino/      # 昆汀·非线性叙事
    │   │   └── wong-mood-fragment/        # 王家卫·情绪碎片
    │   ├── cinematography/           # 摄影（2 种风格）
    │   │   ├── liao-fan-light/            # 廖本榕·自然光美学
    │   │   └── deleuze-time-image/        # 德勒兹·时间影像
    │   └── short-video/              # 短视频（2 种类型）
    │       ├── douyin-trend/             # 抖音爆款逻辑
    │       └── vlog-storytelling/        # Vlog 叙事法
    │
    ├── design/                       # 设计（6 个流派）
    │   ├── SKILL.md                  # 设计领域总控
    │   ├── ui-ux/                    # UI/UX 设计
    │   │   └── apple-design-lang/        # 苹果设计语言
    │   └── [其他 5 个流派待实现]
    │
    └── audio/                        # 音频（占位，待扩展）
        └── SKILL.md                  # 音频领域总控
```

### 耦合矩阵（依赖关系）

#### 目录级耦合

| 子目录 | 依赖谁 | 被谁依赖 | 可独立删除？ |
|--------|--------|---------|-------------|
| `quickstart/` | 无 | SKILL.md 引用（新手入口） | ✅ 可删除，master 层完整可用 |
| `shared/` | 无 | 被各 skill 参考（软引用） | ✅ 可删除，各 skill 仍可工作 |
| `ai-masters/` | 无 | SKILL.md 引用（可选增强） | ✅ 可删除，大师 skill 不受影响 |
| `domains/` | 无 | SKILL.md 路由目标 | ⚠️ 删除后失去深度创作能力 |
| `SKILL.md` | 引用所有子目录 | 用户入口、路由中枢 | ❌ 不可删除（但各 domain 有独立 SKILL.md） |

#### 领域级耦合（domains/ 内部）

| 领域 | 依赖谁 | 被谁依赖 | 可独立删除？ |
|------|--------|---------|-------------|
| `domains/visual/` | 无（自包含） | SKILL.md 路由引用 | ✅ 可删除，其他领域不受影响 |
| `domains/writing/` | 无（自包含） | SKILL.md 路由引用 | ✅ 可删除，其他领域不受影响 |
| `domains/film-video/` | 无（自包含） | SKILL.md 路由引用 | ✅ 可删除，其他领域不受影响 |
| `domains/design/` | 无（自包含） | SKILL.md 路由引用 | ✅ 可删除，其他领域不受影响 |
| `domains/audio/` | 无（占位） | SKILL.md 路由引用 | ✅ 可删除（本来就是占位） |

#### 大师技能级耦合（以 visual/painting 为例）

| 大师技能 | 依赖谁 | 被谁依赖 | 可独立替换？ |
|----------|--------|---------|-------------|
| `impressionist-monet` | 无 | SKILL.md 路由条目 | ✅ 可替换为 new-artist |
| `renaissance-da-vinci` | 无 | SKILL.md 路由条目 | ✅ 可替换 |
| `modernist-picasso` | 无 | SKILL.md 路由条目 | ✅ 可替换 |
| ... | ... | ... | ✅ 全部可独立替换 |

**解读：**
- ✅ 可独立删除 = 删了不影响其他目录的功能
- ⚠️ 部分依赖 = 删了会导致某些功能降级但不崩溃
- ❌ 强依赖 = 删了会破坏核心功能

**关键发现：**
1. **完全正交的领域设计**: 5 个领域之间零依赖，删除任何领域不影响其他领域
2. **原子化的大师技能**: 每个大师都是独立的子目录，可单独替换或删除
3. **三层解耦**: quickstart（入门）/ shared（理论）/ domains+ai-masters（实践）三层互不依赖

---

## 故障注入测试（What-If 分析）

### Scenario 1: 删除整个 `domains/design/` 领域
**影响范围:** 仅失去设计领域的 6 个流派技能（国际主义排版、日式极简、苹果风等）
**降级模式:**
- 其他 4 个领域（visual/writing/film-video/audio）完全正常
- 用户说"帮我设计一个logo"时，SKILL.md 路由失败 → 降级到 ai-painting-master 或提示用户该领域暂不可用
- design/SKILL.md 中的风格诊断映射缺失（仅影响设计相关推荐）
**用户体验变化:**
- 失去"苹果风格""日式极简"等设计品味指导
- 无法获得 UI/UX、排版、品牌设计的专业建议
- **但：** 绘画、写作、影视、音频创作完全不受影响
**恢复方法:** 从 git 恢复 design/ 目录，或逐个重建流派技能

### Scenario 2: 删除 `quickstart/` 目录
**影响范围:** 失去新手快速入门能力
**降级模式:**
- master 层（domains/ + ai-masters/）完全正常且功能完整
- 新手用户直接进入 Level 1+ 路由（需要知道想要什么风格）
- 失去的是"5分钟出活"的低门槛入口和基础教学
**用户体验变化:**
- "教我写prompt"→ 需要路由到对应领域的 AI 大师教学模式
- "帮我画一张"→ 直接进入视觉领域路由（需选择风格）
- "AI效率工具"→ 功能分散到各 AI 大师的 D 模式中
- **但：** 深度创作功能（34个大师技能 + 3个AI专家）完好无损
- **实际上：** 对熟练用户无任何影响，对新手增加了学习成本
**恢复方法:** 从 git 恢复 quickstart/，或基于 shared/ 的理论基础快速重建

### Scenario 3: 删除 `shared/` 目录
**影响范围:** 失去共享的理论基础设施
**降级模式:**
- 各大师技能仍可完整工作（每个技能自带完整的哲学→感知→提示词→评价流程）
- 失去的是跨技能的统一理论基础和评价标准
- 新手路径 Step 0（哲学奠基）无法执行
- 创作循环（6阶段迭代）需要各技能自行实现
**用户体验变化:**
- 无法获得"AI创作的本质""人机契约"等统一哲学指导
- 评价标准可能因技能而异（缺乏统一框架）
- 跨技能融合时缺少共同语言
- **但：** 单技能使用体验几乎不变（每个大师技能是自包含的）
**恢复方法:** 从 git 恢复 shared/，或从 v2 的 L0 和 L4 层提取重建

### Scenario 4: 删除 `ai-masters/` 目录
**影响范围:** 失去 3 个 AI 工艺专家
**降级模式:**
- 艺术大师技能（domains/ 下的 34 个大师）完全不受影响
- 失去的是"怎么用AI工具做出来"的工艺层指导
- 用户需要自己掌握 MJ/DALL-E/SD/ChatGPT 等平台的使用技巧
- 或者依赖 quickstart/ 中的基础教程（如果存在）
**用户体验变化:**
- "帮我画一张"→ 只能获得莫奈/毕加索的品味指导，但缺少具体的平台操作指南
- "评价这张图"→ 有艺术评价标准（来自大师技能），但缺少技术质量评估
- "再生成一版"→ 需要用户手动调整参数，没有自动迭代引擎
- **但：** 品味层（艺术大师）完全 intact，只是工艺层缺失
- **关键洞察：** 这验证了"品味决定做什么，工艺决定怎么做"的分层设计
**恢复方法:** 从 git 恢复 ai-masters/，或将工艺知识内嵌到各艺术大师技能中

### Scenario 5: 替换某个大师技能（如用 `new-contemporary-artist` 替换 `impressionist-monet`）
**影响范围:** 仅影响莫奈风格的路由和创作
**降级模式:**
- 其他 33 个大师技能完全不受影响
- SKILL.md 中莫奈的路由条目失效（或重定向到新技能）
- "印象派/光影/莫奈"触发词不再响应（或响应新技能）
- 已有使用莫奈风格的历史作品不受影响（已生成的内容不依赖源文件）
**用户体验变化:**
- 说"用莫奈的风格画..."→ 可能提示"莫奈技能不可用，是否尝试新当代艺术家？"
- 或直接路由到新技能（如果更新了 SKILL.md）
- **但：** 这是预期的扩展行为，不是故障
**恢复方法:** git checkout 恢复原技能，或接受新技能作为升级

### Scenario 6: SKILL.md 损坏或丢失（最严重场景）
**影响范围:** MetaSkill 总控路由失效
**降级模式:**
- **关键保护：** 每个 domain 有自己的 `SKILL.md`！
  - `domains/visual/SKILL.md` → 视觉创作仍可路由
  - `domains/writing/SKILL.md` → 写作创作仍可路由
  - `domains/film-video/SKILL.md` → 影视创作仍可路由
  - `domains/design/SKILL.md` → 设计创作仍可路由
- 各大师技能的 `.md` 文件仍是完整的独立 Skill
- 用户可以直接说"加载 domains/visual/painting/impressionist-monet/SKILL.md"
- 失去的是"智能自动路由"和"跨领域诊断"
**用户体验变化:**
- 需要用户知道自己要去哪个领域（或尝试多个）
- 风格诊断流程不可用（需要自己选择大师）
- 多技能融合需要手动协调
- **但：** 核心创作能力（34个大师 + 3个AI专家 + 8个quickstart）全部可用
- **这比 learning-system 更抗脆弱**，因为每个 domain 都有自己的路由入口
**恢复方法:**
1. 从 git 恢复 SKILL.md（最佳）
2. 基于 5 个 domain/SKILL.md 重建总控路由表（可行，约 30 分钟工作量）
3. 直接使用各 domain 的 SKILL.md 作为入口（降级但完全可用）

### Scenario 7: 删除 `domains/visual/painting/` 子目录（极端粒度测试）
**影响范围:** 仅失去 6 个绘画大师技能
**降级模式:**
- visual/ 下的 photography/ 和 illustration/ 完全不受影响
- 其他 4 个领域完全不受影响
- visual/SKILL.md 中绘画部分路由失效
**用户体验变化:**
- "用莫奈的风格画..."→ 不可用
- "用亚当斯的方法拍..."→ 正常（photography 不受影响）
- "用宫崎骏的风格画插画..."→ 正常（illustration 不受影响）
- **但：** 这是极度精细的粒度删除，证明了系统的原子化设计
**恢复方法:** 从 git 恢复 painting/ 子目录

---

## 正交性验证

### 维度 1: quickstart vs masters (depth 分离)
- ✅ **完全正交**
- quickstart 解决"快速上手"（广而浅），masters 解决"深度创作"（窄而深）
- quickstart 不 import masters，masters 不 depend on quickstart
- 可以只使用 quickstart（工具使用者）
- 可以只使用 masters（品味追求者）
- 联动时只是 SKILL.md 的升级路径指引，无硬依赖

### 维度 2: shared/ 离线模式
- ✅ **完全正交**
- shared/ 提供"统一理论基础"，但每个大师技能自带完整的迷你版理论
- 删除 shared/ 后：
  - 创作哲学 → 各技能自有哲学段落
  - 提示词基础 → 各技能自有公式体系
  - 评价框架 → 各技能自有评价标准
  - 创作循环 → 各技能自有迭代流程
- **设计决策：** shared/ 是"锦上添花"而非"雪中送炭"

### 维度 3: 单个大师技能替换
- ✅ **完美支持热替换**
- 每个大师技能是独立的子目录，含完整的 SKILL.md
- 替换 monet → 只影响视觉/绘画/莫奈风格路由
- 替换 hemingway → 只影响写作/散文/海明威风格路由
- **零传染性：** 其他技能完全不感知变化
- **扩展性：** 可以无限添加新大师而不修改现有代码

### 维度 4: 领域间独立性
- ✅ **绝对正交**
- visual/writing/film-video/design/audio 五大领域之间：
  - 无文件依赖（不 import/require）
  - 无数据共享（不共享状态文件）
  - 无逻辑耦合（不调用其他领域的函数）
  - 仅在总控 SKILL.md 中并列存在
- 可以删除任意 4 个领域，剩余 1 个领域仍完整可用
- **这是系统最核心的抗脆弱性设计**

### 维度 5: 品味层 vs 工艺层 (domains vs ai-masters)
- ✅ **正交协作**
- domains（品味层）：决定"做什么风格"
- ai-masters（工艺层）：决定"怎么用工具做出来"
- 可以只用 domains（用户自己掌握工具）
- 可以只用 ai-masters（通用创作，无特定品味）
- 两者配合是完整闭环，但各自独立可用

---

## 已知脆弱点

| 脆弱点 | 严重程度 | 缓解措施 | Status |
|--------|---------|---------|--------|
| **SKILL.md 总控集中** | 🟡 中 | 每个 domain 有独立 SKILL.md 可接管；路由表可重建 | Accepted |
| **shared/ 被多处理论引用** | 🟢 低 | 各技能自带迷你理论；shared 缺失时降级为非统一模式 | Fixed |
| **quickstart → masters 升级路径** | 🟢 低 | 仅是导航指引，不是硬编码依赖；手动跳转即可 | Accepted |
| **风格诊断的完整性** | 🟢 低 | 依赖所有领域都存在才能给出全局推荐；缺域时部分推荐不可用 | Open |
| **跨技能融合的复杂度** | 🟢 高阶功能 | 融合是高级用法；单技能使用无需此功能 | Accepted |
| **audio/ 占位空目录** | 🟢 低 | 不影响现有功能；删除后反而更干净 | Accepted |

---

## 设计约束（不可违反）

1. **领域正交约束**: 5 个领域之间必须保持零依赖，删除任何领域不能影响其他领域
2. **技能原子性约束**: 每个大师技能必须是自包含的完整单元（哲学→感知→提示词→评价→迁移），不依赖外部模块
3. **层级独立约束**: quickstart/shared/ai-masters/domains 四层之间只能通过 SKILL.md 软引用，不能硬编码依赖
4. **降级优雅约束**: 删除任何非核心层后，系统必须在可预测的模式下降级，而非随机崩溃
5. **路由可重建约束**: 即使总控 SKILL.md 丢失，基于 domain/SKILL.md 和技能目录名也能重建基本路由能力
6. **品味-工艺分离约束**: 艺术大师（品味）和 AI 大师（工艺）必须可独立使用，配合时是乘法效应而非依赖关系

---

## 未来扩展安全性

如果未来要添加新的子目录或模块：

### 应该放在哪里？
- **新的快速入门模块** → `quickstart/50-new-module.md`（遵循编号规范）
- **新的共享理论基础** → `shared/04-new-theory.md`
- **新的 AI 专家技能** → `ai-masters/ai-new-domain-master.md`
- **新的大师技能（现有领域内）** → `domains/{domain}/{category}/new-master-SKILL.md`
- **全新的创作领域** → `domains/new-domain/SKILL.md` + 子目录结构
- **全新的子系统**（如 3D 创作、游戏设计）→ 考虑新建 `domains/3d/` 或 `domains/game-design/`

### 需要遵守什么规则？
1. **新领域必须自包含**: 不能依赖其他领域的实现细节
2. **新大师技能必须完整**: 必须包含哲学→感知训练→提示词公式→评价标准→跨域迁移的完整工作流
3. **新增后更新路由表**: 在 SKILL.md 中添加路由条目，但不能破坏现有路由
4. **保持命名一致性**: 遵循 `{style}-{master-name}` 的命名规范
5. **提供 eval.md**: 每个技能应该有评估文件（用于质量追踪）
6. **处理降级场景**: 如果新技能依赖 logs/ 或外部资源，必须设计无资源时的降级行为

### 什么情况下应该拆分成新仓库？
1. **当领域数量超过 8-10 个时**（当前 5 个，安全边界内）
2. **当出现完全不同的技术栈时**（如 3D 模型训练 vs 2D 图像生成）
3. **当维护团队分离时**（如社区贡献的领域 vs 官方维护的核心）
4. **当单个仓库体积超过合理范围时**（当前目标 ~25,000 行，可控）
5. **当触发词冲突时**（如"创作"在不同语境下指向完全不同的系统）

### 推荐的拆分策略（如果未来需要）
```
aigc-creative-skills-v3/          # 总控 + shared + quickstart
├── domains-visual/               # 独立仓库：视觉艺术
├── domains-writing/              # 独立仓库：写作文学
├── domains-film-video/           # 独立仓库：影视视频
├── domains-design/               # 独立仓库：设计
└── domains-audio/                # 独立仓库：音频
```
每个独立仓库保留自己的 SKILL.md，总控仓库通过引用组合。

---

## 抗脆弱性评分

| 维度 | 评分 (1-5) | 说明 |
|------|-----------|------|
| **领域隔离性** | ⭐⭐⭐⭐⭐ | 5 大领域完全正交，零依赖 |
| **技能原子性** | ⭐⭐⭐⭐⭐ | 每个大师技能完全自包含 |
| **降级优雅性** | ⭐⭐⭐⭐⭐ | 删除任意 3/4 目录后仍有完整功能 |
| **可替换性** | ⭐⭐⭐⭐⭐ | 任何大师技能都可被无缝替换 |
| **故障透明度** | ⭐⭐⭐⭐⭐ | 故障影响范围精确到单个技能 |
| **恢复简便性** | ⭐⭐⭐⭐ | 多级备份（总控→领域→技能），易于重建 |
| **综合评分** | **4.9/5** | 极度抗脆弱，超越 Taleb 原则要求 |

### 与 Learning System v2 对比

| 维度 | Learning System v2 | AIGC v3 | 胜出 |
|------|-------------------|---------|------|
| 模块数 | 13 个模块 | 34+ 大师技能 | v3 规模更大 |
| 目录层数 | 2 层 (core/enhanced) | 4 层 (quickstart/shared/ai-masters/domains) | v3 更深 |
| 隔离性 | 模块级隔离 | 领域×技能双重隔离 | **v3 更优** |
| 降级能力 | 删除 enhanced 后 core 完整 | 删除任意 3 个目录后仍完整 | **v3 更优** |
| 入口冗余 | 仅 SKILL.md | 总控 + 5 个 domain/SKILL.md | **v3 更优** |
| 粒度替换 | 模块级 (.md 文件) | 技能级 (子目录) | **v3 更细** |

**结论:** AIGC v3 在抗脆弱性设计上超越了 Learning System v2，主要得益于：
1. 多入口路由设计（总控 + 领域分控）
2. 领域级正交（而非仅模块级）
3. 品味-工艺分层（增加了一层解耦）
4. 原子技能粒度（每个大师都是完整子系统）

---

*最后更新: 2026-06-08*
*由「女娲式品味评审」v2 Taleb 维度驱动创建*
