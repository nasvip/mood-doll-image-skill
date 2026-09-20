---
name: yanshi-wawa
description: Build and generate collectible 3D mood-doll portraits with locked likeness, 20 attitude expressions, camera shots, giant or miniature scale, and magazine layouts. Use when the user wants 厌世娃娃, 3D娃娃化, 情绪表情库, 高相似锁, 巨物微缩, 杂志封面排版, 收藏娃, or a reusable character visual control system from a photo or description.
metadata:
  type: workflow
  version: "1.3"
  lang: zh
  display_name: 厌世娃娃
---

# 厌世娃娃视觉控制系统

把「一个人」拆成 7 个独立可调的物理与视觉旋钮：先锁身份，再定娃娃化骨架，再叠美颜、20种情绪、镜头焦段、空间尺度、材质布光与排版。

本技能已深度适配 **`gpt-image-2.5-sunburst`**（物理光学/半哑树脂旗舰）、**`nano-banana-2`**（中英文杂志封面排版）及 **`gemini-3.1-flash-image`**（自然语言流）。

## 实拍得出的硬规则

对真人照片做改图时，模型会黏在原图比例上。只写「去皱、去油、大一点眼睛」会修成精修证件照：

1. 有照片时默认 **LIKE_2 + DOLL_2**，切忌默认 LIKE_3（会彻底压死娃感）。
2. 每一张 prompt 必须完整粘贴 DOLL 母句，严禁只写增量修改。
3. 皮肤默认 **半哑缎面树脂（dry satin resin）**，严禁使用 polished / glossy / wet / dewy。
4. 美颜为独立层：抹平额纹与法令纹，匀净肤色，**绝不换骨、绝不幼态**。
5. 连续对话若偏写真漂移，下一张**必须以用户原始照片为底**，严禁叠在上一张失败图上越修越偏。
6. 合格标准是「一眼是收藏娃，二眼还是这个人」。

## 接到任务先问什么（用户已给定则不问）

1. 人物来源 — 照片 / 文字角色 / 演示脸
2. DOLL — 1 轻 / 2 标准（默认） / 3 极端大头
3. LIKE — 1 神似 / 2 可识别（有照片时默认） / 3 高锁（仅当明确要求更像本人时）
4. BEAUTY — off / soft（默认，去额纹、半哑树脂） / strong
5. 情绪 — 默认 01 厌世冷眼（共 20 种）
6. 镜头 — 默认半身三点柔光
7. 尺度 — 默认 NORMAL
8. 排版 — 默认 PLAIN（素图）；若要封面大字则指定文字
9. 服装 — 未指定沿用照片；要初衷海报感则指定 STYLE_1 黑西装酒红领带

## 多模型适配与执行路由

- **旗舰物理渲染（默认底座，如 `gpt-image-2.5-sunburst`）**：
  - 强制注入光学摄影参数（Hasselblad 100mm f/2.2, 120cm 八角柔光箱）与微表面材质（半哑树脂微表面粗糙度 0.35, SSS 散射 1.0mm）；
  - 强制锁定 `quality: max/xhigh`，4K 超高分；纯正向描述，杜绝反向词聚焦。
- **杂志海报与中英大字（如 `nano-banana-2`）**：
  - 路由至 `references/layout.md`，使用引号结构化注入中英文标题、杂志刊头与极简版式。
- **多模态自然语言流（如 `gemini-3.1-flash-image`）**：
  - 采用连贯叙事段落，锚定参考图人物特征与空间比例关系。
- **图生图底图铁律**：有照片保留人物时，**始终以用户原始照片为底图**，严禁链式叠图。

## 组装顺序（不可颠倒）

1. IDENTITY — [references/likeness.md](references/likeness.md)
2. DOLL — [references/doll.md](references/doll.md)
3. BEAUTY — [references/beauty.md](references/beauty.md)
4. MOOD — [references/mood.md](references/mood.md)
5. SHOT — [references/shot.md](references/shot.md)
6. SCALE — [references/giant.md](references/giant.md)
7. FINISH — [references/style.md](references/style.md) 与 [references/layout.md](references/layout.md)
8. ADAPTER — [references/adapters.md](references/adapters.md)（针对目标模型注入特定光学/排版参数）

母版见 [references/prompt-template.md](references/prompt-template.md)，配方见 [references/recipes.md](references/recipes.md)。

## 出图前自检（不通过则重写 prompt）

- [ ] DOLL 母句完整出现，包含大杏仁眼、厚眼睑、短面中
- [ ] 包含目标情绪的精准英文段落（如 half-lidded, indifferent side-glance）
- [ ] 皮肤质感写明 dry satin resin、微表面散射 SSS 与柔光箱布光
- [ ] 高光仅限内眼角与下唇微小点，额头脸颊全哑光
- [ ] 若开启美颜，写明 smooth forehead, adult bone structure, not a child
- [ ] 严格以用户原始照片为底，没有叠用上一次失败写真

## 连续生成与崩坏回退

会话内固定同一 DNA（IDENTITY + DOLL + BEAUTY）。连续生成默认只变动 MOOD 或 SHOT。
- **人不像本人** ➔ 提至 LIKE_2/3，重申发型发际与鼻唇；不要降 DOLL。
- **太像真人写真** ➔ DOLL 升档，DOLL 母句前置，LIKE 降至 2。
- **满脸反光出汗** ➔ 彻底清理 glossy/wet，改注入 dry satin resin 与 120cm 八角柔光箱。
- **幼态婴儿化** ➔ 强调 adult bone structure, not a child。

## 交付规范

先用不超过 8 行输出当前配置档位，再执行渲染出图。用户只要提示词时，交付角色 DNA、完整英文 Prompt、中文变量对照表。
