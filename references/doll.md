# 娃娃化骨架锁

娃娃化解决的是「高级真人写真」问题。作者自述里翻车点是皮肤和衣服都对，但没有娃感。娃感来自比例与材料，不是来自滤镜名。

## 档位

### DOLL_1 轻 stylize
头身比接近真人。眼睛略放大，皮肤像精修广告，仍偏写真。适合要「像杂志人物」多于「像收藏娃」。

### DOLL_2 标准收藏娃（默认）
略大的头，明显放大的横向杏眼，厚而柔软的上眼睑，略短的面中，小而挺的鼻子，饱满但窄的嘴。皮肤像打磨过的半哑树脂或瓷器，细雀斑，高光只留在唇与眼角，不是磨皮网红，也不是满脸出汗。身体仍能穿时装，手可以偏小、指节简化。

### DOLL_3 极端收藏娃
头明显偏大，眼睛占脸的比例很高，眼白少、瞳孔大，眼睑厚重，面中更短，下巴尖而小。树脂感更强，接近设计师玩具。全身时四肢略缩短。巨物/微缩场景常用这一档，因为比例本来就不写实。

有照片时把母句放在 IDENTITY 之后。用户说「眼睛再大」只能追加，不能取代母句。

## 必须出现的娃感句（DOLL_2/3）

```
Collectible 3D fashion-doll character, not a retouched photo: slightly oversized head, very large horizontal almond eyes, heavy soft eyelids covering the top of the iris, shortened midface, small refined nose, compact mouth, dry satin resin skin with subsurface scatter and tiny freckles. Highlight only on the lower lip and inner eye corner. Not glossy, not sweaty, not oily, not a live-action photograph, not anime lineart, not a baby face, not a passport portrait.
```

## 明确排除

- 真人皮肤毛孔纪录片
- 二次元线稿或平涂
- 幼态婴儿脸、合法萝莉体
- 过度瘦脸瘦到换骨
- 蜡像或恐怖谷湿润过度

## 头身比参考（只在全身/全景时写）

- DOLL_1：约 7.5–8 头身
- DOLL_2：约 6–6.5 头身
- DOLL_3：约 5–5.5 头身

特写不要写头身比，以免模型去编造一个错误身体。
