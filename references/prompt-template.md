# 提示词母版与多模型架构 (Prompt Templates)

构建提示词时**必须整段完整输出**，严禁只发送增量修改词。以下按目标模型提供专属母版：

---

## 1. GPT-Image-2.5 (Sunburst) 物理旗舰母版 (有参考图改图)

针对 `gpt-image-2.5-sunburst` 的顶级物理光学与材质渲染引擎：

```text
Use this photo strictly as identity reference for an adult [man/woman].
Retain facial bone architecture, brow shape and distance, eye spacing, nose bridge profile, mouth contour, and distinctive hair style or accessories.

High-end collectible 3D designer doll character:
Slightly oversized head, wide horizontal almond eyes with heavy drooping upper eyelids partially obscuring the top of the iris, shortened midface, refined small nose, compact calm mouth.
Dry satin resin skin with subtle subsurface scattering (SSS depth 1.0mm) and delicate micro-freckles across the nose.
Matte finish across forehead and cheeks, with strictly isolated pinpoint highlights only on the inner tear duct and lower lip center.
Velvety tactile finish, adult bone structure, smooth forehead with no wrinkles.

Mood: [选自 references/mood.md 的 20 种情绪之一，如 Mood 01 厌世冷眼]
Camera & Optics: Hasselblad 100mm f/2.2 portrait lens, shallow depth of field, sharp focus on the iris with creamy background blur.
Lighting: Diffused 120cm octabox soft light at 45 degrees, subtle negative fill, warm faint rim light along hair edges.
Wardrobe & Scene: [STYLE_1 黑西装或指定服装预设], [道具].
Layout: Clean minimalist composition, zero text, pure visual focus on the character.

High-end 3D art-toy collectible render, tactile natural fabrics, dry satin resin skin, gallery quality finish.
```

---

## 2. Nano-Banana-2 封面排版与大字海报母版

针对 `nano-banana-2` 的高精度图文排版与中英文字渲染能力：

```text
A high-end collectible 3D designer doll based on the reference photo:
Slightly enlarged head, expressive large horizontal almond eyes with sleepy heavy eyelids, shortened midface, dry satin resin porcelain-matte finish with zero oily gloss.
Preserve the adult subject's distinctive hair, facial proportions, and identity.

Mood: [情绪英文段落]
Wardrobe: [STYLE_n 造型预设]

Layout & Typography:
- Format: High-fashion editorial magazine cover.
- Top Masthead: "[如：MOOD / SATIN]" in heavy bold brutalist sans-serif typography across the top.
- Headline: "[中文大字，如：厌世 / 人间清醒 / 先喝咖啡]" vertically aligned along the margin in modern bold typography.
- Issue Details: "VOL. 09 // COLLECTIBLE ART TOY EDITION" in fine minimalist tracking.
- Visual Balance: Generous negative space, magazine-grade layout harmony.
```

---

## 3. Gemini-3.1-Flash-Image 自然叙事流母版

针对 `gemini-3.1-flash-image` 的多模态连贯逻辑理解：

```text
Create a collectible 3D mood-doll portrait reimagining the adult person in the reference image. Maintain their core identity—specifically their facial structure, signature hairstyle, and brow shape—while transforming their proportions into a stylized designer art-toy: slightly larger head, wide almond eyes with heavy, half-lidded sleepy eyelids covering the upper pupil, a shortened midface, and a petite mouth.

The doll's skin is crafted from premium dry satin resin, showing a soft, non-reflective velvety texture with faint natural warmth and tiny freckles, completely free of oily shine. 

The character expresses [Mood 01: cold, indifferent side-glance, chin slightly resting down, utterly unfazed]. Dressed in [STYLE_1: tailored black suit with a skinny wine-red tie]. Captured in a cinematic portrait shot with soft studio lighting and smooth depth of field.
```

---

## 4. 文生图通用母版 (Text-to-Image without photo)

```text
A collectible 3D mood-doll character: an adult [man/woman], [hair description], [distinguishing facial traits].
Slightly oversized head, wide horizontal almond eyes with heavy drooping eyelids covering top iris, shortened midface, compact lips, dry satin resin skin with subtle SSS.
Matte velvety texture, smooth forehead, adult facial structure.

Mood: [Mood selection].
Camera: [Shot & lens].
Wardrobe: [STYLE_n].
Lighting: Soft studio octabox, warm rim light.

High-end 3D art-toy render, dry satin resin finish, gallery print quality.
```

---

## 5. 会话级角色 DNA 锁定卡

在连续多轮生成时，保持以下 DNA 参数固定不变，只调整 `MOOD`、`SHOT` 或 `LAYOUT`：

```text
[CHARACTER DNA]
- IDENTITY: [性别 / 年龄感 / 标志发型 / 关键五官特征]
- LIKE: 2 (可识别)
- DOLL: 2 (标准收藏娃)
- BEAUTY: SOFT (平额纹 / 匀肤)
- SKIN FINISH: Dry satin resin (半哑树脂 / SSS / 零出汗)
- DEFAULT WARDROBE: STYLE_1 (黑西装酒红领带)
```
