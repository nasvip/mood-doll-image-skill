# 多模型适配器指南 (Model Adapters)

本模块针对主流多模态生图引擎（`gpt-image-2.5-sunburst`、`nano-banana-2`、`gemini-3.1-flash-image`）的底层生成机理，提供专属参数映射、提示词工程与渲染控制规范。

---

## 1. 核心模型适配矩阵

| 模型名称 | 核心优势 | 专长场景 | 提示词语法偏好 | 典型配置 |
|---|---|---|---|---|
| **`gpt-image-2.5`**<br>*(含 sunburst)* | **极致物理渲染**<br>微表面散射、高级树脂肌理 | 人物特写、标准半身收藏娃、微距眼部、极端质感大片 | **物理光学参数 + 正向材质描述**<br>（严禁负面词反向聚焦） | 4K UHD<br>`quality: max/xhigh`<br>9:16 / 4:5 / 1:1 |
| **`nano-banana-2`** | **中英文排版天花板**<br>版式排版、字符精准不糊 | 杂志封面（MAGAZINE）、海报大字、中英文情绪标语、双人对页 | **严格文字语法引用块**<br>`Headline: "[文字]"` + 排版方位指定 | 竖版海报<br>高质量插画/海报模式 |
| **`gemini-3.1-flash-image`** | **多模态语义连贯**<br>超快推理、指令遵循严谨 | 复杂空间故事、多道具微缩/巨物、自然语言改图 | **结构化自然语言连贯段落**<br>（拒绝零散 Comma-tag 乱炖） | 标准高分<br>图文混合上下文 |

---

## 2. GPT-Image-2.5 (Sunburst) 专属优化规范

### 2.1 物理材质与微表面光学注入
`gpt-image-2.5` 对物理光学参数（SSS 次表面散射、粗糙度、镜头光圈）极其敏感。要避免生成油腻蜡像或廉价塑料，必须使用以下物理级正向参数：

```text
[Material & Finish]
- Surface: dry satin resin with tactile velvety matte finish.
- Subsurface Scattering: subtle subcutaneous resin translucency (SSS depth: 1.0mm) creating warm soft ambient shadows.
- Micro-texture: micro-porcelain surface roughness (0.35), delicate faint freckles across nose bridge.
- Specular Control: strictly zero specular shine or glare on forehead, nose tip, and cheeks. Specular reflections restricted exclusively to the tear duct and center of lower lip.
```

### 2.2 摄影棚光学系统
```text
[Optics & Lighting]
- Camera: Hasselblad H6D-100c medium format, HC 100mm f/2.2 portrait lens.
- Depth of Field: shallow depth of field, razor-sharp focus on the nearest iris, creamy cinematic roll-off.
- Lighting: key light from a large 120cm octabox at 45 degrees, ultra-diffused, subtle negative fill on shadow side, faint rim light separating hair from the dark background.
```

### 2.3 摒弃否定词陷阱（Positive Reframing）
- ❌ **避免使用**：`not sweaty, not glossy, not oily, not plastic, not real human photo`（模型易反向捕捉 sweaty/glossy 特征）。
-  **正向强化**：`ultra-dry porcelain-resin surface, uniform satin diffusion, strictly matte finish, tactile designer art-toy collectible sculpture`.

---

## 3. Nano-Banana-2 专属排版与文字规范

`nano-banana-2` 彻底解决了以往生图模型“画字必糊”的痛点，支持高精度**中文情绪大字**与**时尚杂志封面排版**。

### 3.1 文字控制语法格式
在提示词末尾添加排版控制块，使用英文双引号严格包裹文字内容：

```text
[Typography & Poster Layout]
- Masthead Title: "[杂志名或英文大词]" in ultra-bold high-fashion sans-serif typography, horizontally centered at the top third.
- Chinese Headline: "[中文大字，如：厌世 / 摆烂 / 人间清醒]" in artistic heavy gothic or brutalist Chinese font, vertical layout along the left margin.
- Editorial Sub-elements: minimal volume issue "NO. 09 / AUTUMN EDITION", clean barcode block at bottom right.
- Composition: generous editorial negative space, typography interacting behind the character's head.
```

### 3.2 常用高表现力中文文案预设
- 态度单字：`「冷」`、`「拽」`、`「空」`、`「休」`
- 情绪短语：`「人间清醒」`、`「拒绝社交」`、`「先喝咖啡」`、`「今天不上班」`、`「关我屁事」`
- 英文杂志名：`MOOD`、`SATIN`、`APATHY`、`OFF-DUTY`、`DOLL CORE`

---

## 4. Gemini-3.1-Flash-Image 专属语义流规范

Gemini 擅长理解长句逻辑与图文因果关联，反感断续的逗号词堆。采用**自然叙事流（Narrative Stream）**表达：

```text
A high-end collectible 3D designer doll portrait adapted from the reference image. The character represents the same adult person, preserving their distinctive facial bone structure, eyebrows, and signature hair, but re-imagined as a resin art-toy with a slightly oversized head, wide horizontal almond eyes with heavy drooping eyelids, and a shortened midface. 

The facial skin has an exquisite dry satin resin texture with soft diffuse lighting, showing zero oily gloss. The character wears [outfit description], giving a side-glance with an indifferent, detached mood (Mood 01). Photographed in medium close-up with soft studio rim lighting and shallow focus.
```

---

## 5. 自动路由与切换逻辑 (Agent Auto-Routing)

当 Agent 处理出图请求时，依据用户指令特征选择最优渲染模板：

1. **若包含杂志封面、中文标题、海报排版、字样入画** ➔ 注入 **Nano-Banana-2** 排版语法；
2. **若强调极致树脂微质感、4K 细节、眼部特写、逼真微表面** ➔ 注入 **GPT-Image-2.5** 物理光学参数；
3. **若处于多轮对话复杂道具互动（如人坐在机械键盘或咖啡杯沿）** ➔ 注入 **Gemini** 空间因果自然语言模板。
