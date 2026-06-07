# 共享模块：提示词基础 (Prompt Foundations)

> 跨平台、跨领域的通用提示词工程原理。
> 无论绘画/写作/视频，底层逻辑相通。
> 各AI大师技能会在此基础上叠加平台特性。

## 通用提示词结构

### 标准五段式

```
[主体] + [环境/背景] + [光线/氛围] + [风格/参考] + [技术参数]

示例（图像）：
"A lone cypress tree"                                    # 主体
"on a cliff overlooking the Amalfi Coast"                  # 环境
"golden hour side-light with long shadows"                 # 光线
"Impressionist oil painting style, Claude Monet inspired"   # 风格
"--ar 16:9 --stylize 250 --v 6.0"                          # 技术

示例（写作）：
"You are an experienced environmental journalist"          # 角色(=主体的另一种形式)
"writing for National Geographic Chinese edition"           # 环境(=受众)
"narrative-driven tone, emotionally resonant"              # 氛围
"Hemingway minimal style — short sentences, strong verbs"  # 风格
"800 words, start with a specific scene, use data sparingly" # 技术
```

### 关键原则

**原则1：约束产生质量**

```
约束越多 → AI的选择空间越小 → 输出越可控 → 质量越稳定

好的约束：
  ✅ "用海明威的风格，每句不超过15个字"
  ✅ "印象派风格，只使用蓝紫橙三个色系"
  ✅ "30秒视频，只有一个运动主体"

坏的约束：
  ❌ "写得好一点"（什么算好？）
  ❌ "画一张好看的图"（主观且无方向）
  ❌ "做个视频"（完全没有边界）
```

**原则2：具体 > 抽象**

| 抽象（效果差） | 具体（效果好） |
|---------------|--------------|
| 好看的光 | golden hour rim light from upper left at 35° angle |
| 美丽的颜色 | palette of burnt sienna, ochre, and muted violet |
| 流畅的文字 | 平均12字/句，短-长-短节奏交替 |
| 好看的运镜 | slow dolly-in from 3m to 1m distance over 8 seconds |

**原则3：范例 > 解释**

```
❌ "写得更有画面感"
   → AI的理解 ≈ 你的理解？大概率不等。

✅ "当前：'这是一个美丽的城市'
    期望效果类似：
    '这座城市的美是不请自来的——它藏在清晨雾气未散的
     梧桐叶尖上，藏在老弄堂里飘出的豆浆香里'"
    请按这个方向修改全文"
```

**原则4：负面约束同等重要**

```
每个提示词都应该有"不要什么"的部分：

图像：--no blurry, watermark, distorted anatomy, oversaturated
写作：avoid clichés, no passive voice, don't use "very/really/extremely"
视频：no sudden movements, avoid style inconsistency, no abrupt cuts
```

## 跨媒介提示词映射

同一创作意图在不同媒介中的表达方式：

| 创作意图 | 图像提示词 | 写作提示词 | 视频提示词 |
|---------|-----------|-----------|-----------|
| "温暖" | warm color palette, golden hour, soft lighting | gentle tone, sensory details of comfort, slow pacing | warm color grading, slow camera movement, golden hour lighting |
| "孤独" | single small figure in vast empty space, muted colors | short declarative sentences, internal monologue, isolation imagery | long static shot, wide framing, minimal movement, silence |
| "力量" | dynamic composition, strong contrast, diagonal lines | active verbs, short punchy sentences, rhythmic repetition | fast cuts, low camera angle, dramatic lighting, powerful music cue |
| "神秘" | partial visibility, shadows, fog/mist, ambiguity | withheld information, unreliable narrator, open-ended questions | shallow depth of field, reveal through movement, ambient sound only |

## 提示词版本管理

每次迭代都应该记录提示词变化：

```markdown
## Prompt 版本历史

V1 (初始): [原始提示词]
  结果：[简述]
  问题：[主要问题]

V2 (修改): [改动点标注]
  结果：[改善/退化]
  问题：[遗留问题]

V3 (再改): [改动点标注]
  结果：[最终评分]
  结论：[什么策略有效]
```

---

> 本模块被所有 AI 大师技能引用
> 各平台详细参数见对应 AI 大师技能的教学模式章节
