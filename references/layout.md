# 画面排版库

排版是最后一层。模型画字不稳定，能不下字就下字。必须下字时，优先英文短词，汉字只在用户强求时出现，并接受可能变形。

## 模式

- PLAIN — 无字，只有角色与场景。默认。
- MAGAZINE — 大刊名、一条短标题、条形码区可暗示但不求可读。
- TYPE_BG — 身后巨大字母（如 NOT TODAY），字母是图形不是段落。
- POSTER_WHITE — 大留白，人物偏一侧，像品牌海报。
- DUO_COVER — 双人同一刊面，两张脸层级清楚。
- DESK_FLAT — 俯视桌面：电脑、杯子、书脊、人坐在或靠在其中。

## 常用短文案（可选，英文优先）

- Same Girl Different Story.
- Same Guy Different Story.
- Not Today.
- Good Ideas Later.
- A Cooler Way To Work And Live.
- I see it. I do not care.

中文短句若用户指定，限制在 6 字内，例如「先喝咖啡」「看到了」。

## 写入句式

```
Layout: [mode]. On-image text limited to [exact words], clean editorial typography, large masthead, generous margins. Text is secondary to the face. No paragraphs. No watermark. No extra logos.
```

用户没有要求品牌名或刊名时，不要自动写水印、公众号或他人品牌字。那不是本技能默认输出。
