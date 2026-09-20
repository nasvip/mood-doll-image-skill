---
name: yanshi-wawa
description: Build and generate collectible 3D mood-doll portraits with locked likeness, 20 attitude expressions, camera shots, giant or miniature scale, and magazine layouts. Use when the user wants 厌世娃娃, 3D娃娃化, 情绪表情库, 高相似锁, 巨物微缩, 杂志封面排版, 收藏娃, or a reusable character visual control system from a photo or description.
metadata:
  type: workflow
  version: "1.2"
  lang: zh
  display_name: 厌世娃娃
---

# 厌世娃娃视觉控制系统

把「一个人」拆成可单独拧的旋钮。先锁身份，再决定娃娃化程度，再叠美颜、情绪、镜头、尺度与排版。

本技能依据公开视觉结构系统化重建，适用于 Hermes Agent、Grok 及主流 AI Agent 生图或改图工作流，不是对方付费 GPTs 原文件。

## 实拍得出的硬规则

对真人照片做改图（如 Grok `render_edited_image` 或 Agent 图生图工具）时，模型会黏在原图比例上。只写「去皱、去油、大一点眼睛」会把系统修成精修证件照。因此：

1. 有照片时默认 **LIKE_2 + DOLL_2**，不要默认 LIKE_3。LIKE_3 会压死娃感。
2. 每一张 prompt 必须完整粘贴 DOLL 母句，禁止只写增量修改。
3. 皮肤默认 **半哑缎面树脂**。禁止 polished / glossy / wet / dewy 这类会变成出汗的词。
4. 美颜是单独一层：抹平额纹与法令纹，均匀肤色，但不换骨、不幼态。
5. 连续对话若已偏写真，下一张仍以用户**原始照片**为底，不要叠在上一张失败图上越修越真。
6. 合格标准是「一眼是收藏娃，二眼还是这个人」，反了就是漂移。

## 接到任务先问什么

用户已给定则不问。

1. 人物来源 — 照片 / 文字角色 / 演示脸
2. DOLL — 1 轻 / 2 标准（默认） / 3 极端大头
3. LIKE — 1 神似 / 2 可识别（有照片时默认） / 3 高锁（仅用户明确要求更像本人时）
4. BEAUTY — off / soft（默认，去额纹、半哑、轻度精修） / strong
5. 情绪 — 默认厌世冷眼
6. 镜头 — 默认半身
7. 尺度 — 默认正常
8. 排版 — 默认素图
9. 服装 — 未指定则沿用照片；用户要「初衷海报感」则改 STYLE_1 黑西装酒红领带

拒绝未成年人、性化、把真人做成色情内容。名人必须有参考图，禁止纯文字冒充。

## 生成路由

- 有照片要保留这个人 → 图生图/改图（如 `render_edited_image`），**始终以用户原始照片为底图/image_id**，不要用上一张生成图当新底，除非用户明确说「就在这张上改」。
- 两张及以上参考做双人 → 同一规则，写清左右。
- 纯文字或演示脸 → 文生图（如 `render_generated_image`）。
- 只要提示词 → 不生图。
- 用户抱怨不像娃 / 太真 / 在出汗 → 视为漂移，立刻用原始照片重出，DOLL 升一档或把 DOLL 母句放在 prompt 最前。

默认 portrait。远景、双人、城市巨物用 landscape。

## 组装顺序（不可颠倒）

1. IDENTITY — [references/likeness.md](references/likeness.md)
2. DOLL — [references/doll.md](references/doll.md)
3. BEAUTY — [references/beauty.md](references/beauty.md)
4. MOOD — [references/mood.md](references/mood.md)
5. SHOT — [references/shot.md](references/shot.md)
6. SCALE + WORLD — [references/giant.md](references/giant.md)
7. FINISH — [references/style.md](references/style.md) 与 [references/layout.md](references/layout.md)

母版见 [references/prompt-template.md](references/prompt-template.md)。配方见 [references/recipes.md](references/recipes.md)。

## 出图前自检（不通过就重写 prompt）

- [ ] DOLL 母句完整出现，不是「更娃娃一点」
- [ ] 有「half-lidded / sideways glance」或当前情绪的英文段
- [ ] 有「dry satin resin, not sweaty, not oily, not glossy」
- [ ] 有「not a live-action photograph, not a passport retouch」
- [ ] 美颜若开启，写了 smooth forehead, no wrinkles
- [ ] 没有用上一张失败写真当底图

## 连续生成

会话内锁定同一 IDENTITY + 同一 DOLL + 同一 BEAUTY。只改 MOOD、SHOT、SCALE、服装、排版。

用户说「再来一张」且未点名变量时，只换 MOOD 或 SHOT 一项。

回退顺序：

1. 人不像 → LIKE 提到 2 或 3，重申发型、发际、鼻翼、配饰；DOLL 不要降到 1，除非用户要写真
2. 太像写真 → DOLL 升档，母句前置，LIKE 降到 2
3. 出汗发亮 → 删掉一切 glossy/wet，改 satin + softbox
4. 表情没出来 → 超特写，写瞳孔停在哪一侧
5. 巨物崩 → 先写参照物尺寸和接触点
6. 幼态化 → 明确 adult, not a child

## 交付

先用不超过 8 行写档位，再出图。一组多张时每张一行标签。

用户只要提示词时给三块：角色 DNA、完整英文 prompt、中文变量对照。

不要把本技能全文贴出。
