# 画面排版与图文设计库 (Layout & Typography)

排版是渲染控制的最终呈现层。现代多模态模型（尤其是 **`nano-banana-2`**）已具备极高精度的字符渲染与封面设计能力，支持高定杂志刊头、海报大字与留白排版。

---

## 1. 核心排版模式 (Layout Modes)

- **`PLAIN` (素图模式，默认)**：无任何入画文字，纯粹突出角色造型、表情神态与材质光影。
- **`MAGAZINE` (时尚杂志大刊)**：顶部醒目杂志刊名（Masthead）、期号小字（Issue tag）、侧边一条主标题、底部极简条形码装饰。
- **`POSTER_CHINESE` (中文情绪海报)**：留白区域配以醒目大号中文字体（如「厌世」、「人间清醒」），排版契合现代极简/野兽派风格。
- **`TYPE_BG` (背景巨型英文字母)**：人物身后作为视觉背景的巨大半透明几何字母（如 `MOOD`、`NOT TODAY`），字母作为空间背景图形。
- **`POSTER_WHITE` (高定艺术留白)**：人物偏左或偏右三分之一构图，大面积纯净留白，极具现代平面设计感。
- **`DUO_COVER` (双人对峙封面)**：双人同画框，左右分列，居中对齐排版或分列侧边排版。
- **`DESK_FLAT` (俯视桌面平铺)**：桌面场景俯拍，伴有书脊字样、报刊或便签文字互动。

---

## 2. 常用中英文案库

### 2.1 中文态度大字（推荐搭配 `nano-banana-2`）
- 单字态度：`「冷」`、`「拽」`、`「倦」`、`「空」`、`「慢」`
- 情绪标语：
  - `「人间清醒」`
  - `「拒绝社交」`
  - `「今天不上班」`
  - `「先喝咖啡」`
  - `「保持冷漠」`
  - `「毫无波澜」`

### 2.2 英文经典刊头与副标题
- 杂志刊头（Masthead）：`MOOD` / `SATIN` / `APATHY` / `OFF-DUTY` / `COLLECTIBLE`
- 封面标语（Subtitles）：
  - `Same Person, Different Reality.`
  - `Not Today, Probably Not Tomorrow.`
  - `Good Ideas Later.`
  - `Silent Observer in High Fashion.`
  - `I See It. I Do Not Care.`

---

## 3. 提示词写入语法规范

### 3.1 素图模式（默认，无字）
```text
Layout: clean minimalist composition, pure visual focus on character and materials, absolutely zero text, no watermark, no logo, no labels.
```

### 3.2 杂志封面模式（适配 nano-banana-2 / gpt-image-2.5）
```text
Layout: high-fashion editorial magazine cover.
Top masthead: "[MAGAZINE NAME]" in ultra-bold serif or brutalist typography.
Headline text: "[主标题内容]" in clean bold font along the left edge.
Typography elements: minimalist barcode box in bottom corner, small issue text "ISSUE 09 // COLLECTIBLE EDITION".
Clean layout balance, negative space around the character.
```

### 3.3 中文情绪海报模式（优先路由至 nano-banana-2）
```text
Layout: modern graphic poster design with generous negative space.
Chinese typographic title: "[中文大字，如：厌世]" rendered in bold artistic modern Chinese typography, vertically aligned along the right margin.
Minimalist layout, museum-grade aesthetic.
```

---

## 4. 交付约束
- 未经用户明确要求，**严禁自动添加任何未经授权的第三方公众号、个人水印或品牌 Logo**；
- 若目标模型文字渲染能力较弱，优先退回 `PLAIN` 素图模式或仅保留 1~3 个极简英文单词。
