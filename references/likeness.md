# 人物相似度锁

目标是「先认出是同一个人，再允许娃娃化」。没有参考图时，IDENTITY 要写得具体到可以复用，而不是「一个好看的女生」。

## 档位

- LIKE_1 神似：保留性别、年龄段、发色、大致脸型。允许大幅度风格化。
- LIKE_2 可识别：再锁眉形、眼距、鼻梁高低、嘴宽、标志发型。
- LIKE_3 高锁：再锁发际线、眼镜、耳饰、痣/酒窝/疤。仅当用户明确要求更像本人、可以牺牲部分娃感时使用。有照片时默认 LIKE_2，否则改图会黏在真人比例上。

## 必须写入 IDENTITY 的字段

按这个清单从参考图或用户描述提取，缺项就写「not specified, keep generic」而不是编造：

1. 性别呈现与年龄感（teen 禁止用于真人未成年；成人写 early-20s / late-20s / 30s）
2. 脸型 — oval / short heart / long rectangle / round / sharp jaw
3. 眉 — 粗细、角度、间距
4. 眼 — 大小只在 DOLL 层放大；这里只锁眼距、眼角走向、双眼皮形态
5. 鼻 — 梁高、翼宽、尖圆钝
6. 嘴 — 唇厚、嘴宽、嘴角默认方向
7. 面中 — 实际长短；DOLL 层可以压短，但鼻唇相对关系要还在
8. 发际线与发型 — 中分/偏分、碎发、长度、是否遮眉
9. 眼镜与配饰 — 圆框、耳钉、十字架耳坠、手表、领结颜色
10. 皮肤标记 — 雀斑密度、痣的位置

## 写入句式

```
Identity lock (do not change across shots): an adult [woman/man], [age feel], [face shape] face, [brow], [eye spacing and corner], [nose], [mouth], [hairline and hairstyle], [glasses/jewelry if any], [freckles or moles]. Keep the same person. Do not beautify into a different face.
```

## 改图时的额外一句

有参考照片时，在 prompt 最前面加：

```
Keep this exact person's facial identity, bone structure, hairstyle family, and signature accessories. Stylize toward a collectible 3D mood doll, but do not replace them with a generic model.
```

## 常见翻车

- 把「短面中」写进 IDENTITY 而不是 DOLL，真人长面中会被改成另一个人
- 忘掉眼镜，下一张人就不像
- 把染发、换装写成身份，导致换场景时头发锁死。发色造型可列为「可替换层」，发际线才是身份层
- 有照片时把 LIKE 开到 3，再加「keep exact face」——结果一定偏写真。要娃，先降 LIKE
