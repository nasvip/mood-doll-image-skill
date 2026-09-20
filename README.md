# 厌世娃娃 · 人物视觉控制系统

版本：1.2  
中文名：厌世娃娃  
技能目录名：`yanshi-wawa`  
语言：中文工作流 + 英文生图提示词  
适用环境：Grok 自定义 Skill（将本目录放到用户技能路径后，由描述字段触发）

---

## 1. 这是什么

这是一套给 Grok 用的人物视觉控制技能，目标不是「套一个厌世滤镜」，而是把一个人拆成可单独拧的旋钮：

1. 先锁住「这个人是谁」  
2. 再决定娃娃化程度  
3. 再叠加美颜、情绪、镜头、尺度、材质与排版  

同一角色连续出图时，身份与娃娃骨架保持不变，只改你指定的那一层。

典型触发说法：

- 厌世娃娃 / 3D 娃娃化 / 收藏娃  
- 情绪表情库 / 高相似锁  
- 巨物微缩 / 杂志封面排版  
- 把这张成人照片做成收藏娃  

---

## 2. 与原版付费产品的关系

公开网络上有过同类「厌世娃娃 / 情绪收藏娃」营销海报与功能拆解。那些材料里的作者名与品牌名 **不沿用**。

本仓库是独立撰写的 Grok 技能，用于生图 / 改图，中文名就叫「厌世娃娃」。

本仓库 **不是**：

- 任何付费 GPTs 原文件的复制  
- 可安装到 ChatGPT 的第三方包  
- 带他人水印、公众号或品牌授权的发行版  

生成图默认不要自动写他人品牌或公众号字样，除非用户明确要求入画文字。

---

## 3. 目录结构

```text
yanshi-wawa/
├── README.md                 # 本说明（给人看）
├── SKILL.md                  # 主指令（给模型看，含 YAML frontmatter）
├── assets/
│   └── mood-index.txt        # 20 种情绪编号速查
└── references/
    ├── likeness.md           # 人物相似度锁 LIKE_1–3
    ├── doll.md               # 娃娃化骨架锁 DOLL_1–3
    ├── beauty.md             # 美颜层 BEAUTY_OFF / SOFT / STRONG
    ├── mood.md               # 20 种情绪表情器
    ├── shot.md               # 镜头与构图
    ├── giant.md              # 巨物 / 微缩 / 超比例
    ├── style.md              # 材质、造型预设、光线
    ├── layout.md             # 素图 / 杂志 / 大字 / 双人
    ├── recipes.md            # 现成视觉配方 R01–R11
    └── prompt-template.md    # 改图 / 文生图母版与会话 DNA
```

`SKILL.md` 控制流程；`references/` 在需要时按层加载；不要把参考文档一次性全文贴给用户。

---

## 4. 安装

1. 将整个 `yanshi-wawa` 文件夹放到 Grok 用户技能目录：  
   `/home/workdir/.grok/skills/yanshi-wawa/`  
   （其他环境则放到该产品文档指定的 skills 目录，**文件夹名必须等于** frontmatter 里的 `name`。）
2. 确认 `SKILL.md` 顶部 YAML 合法：  
   - `name` 只能含小写字母、数字、单连字符  
   - `description` 不使用冒号+空格、引号、尖括号  
3. 新开一轮对话，用触发词或直接说「按厌世娃娃来」。

无需安装 ChatGPT、无需对方 GPTs 账号。实际出图走 Grok 的 `render_generated_image` / `render_edited_image`。

---

## 5. 核心原则

### 5.1 组装顺序不可颠倒

| 顺序 | 层 | 文件 | 作用 |
|---|---|---|---|
| 1 | IDENTITY | `likeness.md` | 性别、年龄感、脸型、眉眼距、鼻嘴、发际、发型、眼镜、配饰 |
| 2 | DOLL | `doll.md` | 头身比、大眼、厚眼睑、短面中、小鼻、半哑树脂 |
| 3 | BEAUTY | `beauty.md` | 去额纹、匀肤；不换骨、不幼态 |
| 4 | MOOD | `mood.md` | 只改眼、眉、嘴、下颌、肩颈 |
| 5 | SHOT | `shot.md` | 景别、焦距、机位 |
| 6 | SCALE | `giant.md` | 正常 / 巨物 / 微缩 / 超比例 |
| 7 | FINISH | `style.md` + `layout.md` | 材质灯光与是否封面 |

后一层不得改写前一层已经锁死的骨头。

### 5.2 合格标准

一眼是收藏娃，二眼还是这个人。

不合格：

- 真人比例 + 磨皮 = 精修证件照  
- 满脸高光 = 出汗蜡像  
- 头很大但五官换了 = 另一个人  
- 身体幼态化 = 禁止，即使是极端娃娃档

### 5.3 有照片时的硬规则（v1.1 实拍结论）

对真人照片做改图时，模型会黏在原图比例上。只写「去皱、去油、眼睛大一点」会把系统修成证件照。

1. 有照片默认 **LIKE_2 + DOLL_2**，不要默认 LIKE_3。  
2. 每一张都必须完整粘贴 DOLL 母句，禁止只写增量。  
3. 皮肤默认 **半哑缎面树脂**。禁止 `polished / glossy / wet / dewy` 用在脸上。  
4. 美颜是独立层：抹平额纹，但不换骨。  
5. 连续对话若已偏写真，下一张仍以用户**原始照片**为底，不要叠在上一张失败图上。  
6. 用户说「不像初衷」时，优先切回 `STYLE_1`（黑西装 + 酒红领带 + 窗光）。

---

## 6. 默认档位

用户未指定时使用：

| 项 | 默认 |
|---|---|
| LIKE | 有照片：2；纯文字角色：1 |
| DOLL | 2 |
| BEAUTY | SOFT |
| MOOD | 01 厌世冷眼 |
| SHOT | 半身三点 |
| SCALE | 正常人体 |
| LAYOUT | 素图（无字） |
| 皮肤 | 半哑缎面，高光只在唇与眼角 |
| 画幅 | 竖图；远景 / 双人 / 城市巨物改横图 |

服装：未指定则沿用参考图；用户要海报初衷则改 STYLE_1。

---

## 7. 档位速查

### LIKE 相似度

- LIKE_1 神似：性别、年龄段、发色、大致脸型  
- LIKE_2 可识别：再锁眉形、眼距、鼻梁、嘴宽、标志发型（有照片时的默认）  
- LIKE_3 高锁：再锁发际、眼镜、耳饰、痣。仅当用户明确要求「更像本人、可以牺牲部分娃感」时使用  

### DOLL 娃娃化

- DOLL_1：轻 stylize，接近杂志人物  
- DOLL_2：标准收藏娃（默认）  
- DOLL_3：极端大头树脂娃，接近设计师玩具  

### BEAUTY 美颜

- OFF：保留年龄纹，仍用半哑树脂  
- SOFT：去额纹与明显法令纹，匀肤（默认）  
- STRONG：更光洁，下颌略收，仍必须是同一成人  

---

## 8. 20 种情绪

编号与名称见 `assets/mood-index.txt`。写入生图 prompt 时用 `references/mood.md` 里的英文段，不要把中文名直接丢进英文提示词。

| 编号 | 名称 | 适用备注 |
|---|---|---|
| 01 | 厌世冷眼 | 母表情，默认 |
| 02 | 不可一世 | 适合仰拍 |
| 03 | 嚣张挑衅 | |
| 04 | 轻蔑嫌弃 | |
| 05 | 搞怪斜眼 | |
| 06 | 疯感轻笑 | |
| 07 | 狂气失控 | 适合鱼眼 |
| 08 | 无语凝视 | |
| 09 | 高冷审判 | |
| 10 | 坏笑恶作剧 | 可与 01 拼：眼用 01、嘴用 10 |
| 11 | 疯批气质 | |
| 12 | 摆烂到底 | 适合俯拍 |
| 13 | 拽王模式 | |
| 14 | 阴阳怪气 | |
| 15 | 黑化前夜 | |
| 16 | 慵懒上头 | 适合巨型咖啡杯 |
| 17 | 目中无人 | |
| 18 | 戏精附体 | 适合鱼眼 |
| 19 | 反骨模式 | |
| 20 | 宇宙级无所谓 | |

一次只用一种主情绪。若要「厌世但在笑」，写明眼睛跟 01、嘴巴跟 06 或 10。

---

## 9. 镜头、尺度、造型、配方

### 镜头

脸部超特写 / 眼部超特写 / 半身 / 全身 / 仰拍 / 俯拍 / 鱼眼 / 怼脸 / 电影远景 / 双人

表情要成立用超特写或半身；故事感用半身加道具；世界观用远景或巨物。

### 尺度

- NORMAL：日常比例  
- GIANT_PERSON：人像都市巨人  
- GIANT_PROP：杯子、键盘、气球变成建筑尺度  
- MINI_PERSON：人坐在键帽或杯沿上  
- SURREAL_MIX：超现实混比例  

必须先写参照物真实尺寸，再写接触点（下巴抵杯沿、脚踩空格键、胸口齐楼顶）。

微缩的是尺度，不是年龄。人物仍是同一成人身份。

### 造型预设

- STYLE_1 办公室厌世（最接近原海报）  
- STYLE_2 家居摆烂  
- STYLE_3 皮衣侧面  
- STYLE_4 时装封面  
- STYLE_PHOTO 沿用参考图服装与道具  

### 现成配方

见 `references/recipes.md`。可直接点名：

- R01 办公室厌世半身  
- R02 眼部标本  
- R03 鱼眼失控  
- R04 城市巨人  
- R05 巨型咖啡杯  
- R06 键盘微缩  
- R07 家居男人摆烂  
- R08 双人办公桌  
- R09 皮衣侧颜  
- R10 单色时装刊  
- R11 宫廷厌世  

---

## 10. 对用户怎么说

### 有成人照片

「DOLL_2，厌世冷眼，半身。」  
下一张只改一项：「同一张脸，改鱼眼 + 狂气失控。」

### 只要提示词、不要出图

「只出 prompt。」模型应返回三块：

1. 角色 DNA  
2. 完整英文 prompt  
3. 中文变量对照  

### 一组对照测试

同一 DNA，分别出：半身厌世 / 眼部超特写 / 巨型道具 / 鱼眼换情绪。用来判断身份锁是否成立。

---

## 11. 提示词母版（摘要）

完整可复制版本以 `references/prompt-template.md` 为准。有照片时的骨架如下：

```text
Use this photo only as identity reference for an adult [man/woman].
Keep hair family, brow angle, nose character, mouth width, and signature accessories.

Collectible 3D fashion-doll character, not a retouched photo:
slightly oversized head, very large horizontal almond eyes,
heavy soft eyelids covering the top of the iris, shortened midface,
small refined nose, compact mouth, dry satin resin skin with
subsurface scatter and tiny freckles.
Highlight only on the lower lip and inner eye corner.
Not glossy, not sweaty, not oily, not a live-action photograph,
not anime lineart, not a baby face, not a passport portrait.

Beautify softly: smooth forehead with no wrinkles, even satin complexion.
Keep adult bone structure.

Mood: [英文情绪段]
Camera: [镜头]
Scale: [尺度]
Wardrobe and setting: [服装 / STYLE]

High-end 3D character render, tactile fabrics, soft fashion lighting.
Same person. Dry satin resin face.
```

会话 DNA 建议固定为：

```text
DNA
- LIKE:
- DOLL:
- BEAUTY:
- Face / hair / signature:
- Default wardrobe:
- Skin finish: dry satin resin
```

---

## 12. 崩坏回退

按顺序改，不要整段重写身份。

1. 人不像 → LIKE 提到 2 或 3，重申发型、发际、鼻翼、配饰；不要一上来把 DOLL 降到 1  
2. 太像写真 → DOLL 升档，母句放到 prompt 最前，LIKE 降到 2  
3. 出汗发亮 → 删掉脸上的 glossy / wet，改 satin + 大面积柔箱  
4. 表情没有 → 改超特写，写明瞳孔停在眼角哪一侧  
5. 巨物比例崩 → 先写参照物尺寸和接触点  
6. 幼态化 → 明确 adult, not a child  
7. 封面字糊 → 改素图，或只留 1–3 个英文词  
8. 用户说不像初衷 → 切 STYLE_1，仍用原始照片当底  

---

## 13. 安全边界

本技能 **拒绝**：

- 未成年人照片的改图、娃娃化、换装、复原脸  
- 把真人做成色情内容  
- 无参考图、纯文字捏造可辨认名人脸（应用参考图锚定）  
- 把儿童身体写成「微缩」「萌化」来绕过年龄限制  

机车、海报、家庭纪念等题材不能改变上述边界。儿童照片请用户自行留存，不要进入「改脸出图」流程。

成人照片可以二创。只要车、不要特定人脸时，可以只出车辆或场景。

---

## 14. 已知限制

1. Grok 按真人照片改图时会强黏原图。技能用默认档位和完整母句对抗，但不能保证每张都达到原海报那种设计师玩具强度。  
2. 入画汉字不稳定，封面文案优先英文短词。  
3. 品牌名、车贴、杯身字可能被模型写错，不要把可读文字当验收标准。  
4. 双人图需要两套 IDENTITY，并写清左右位置。  
5. 本技能不包含 ChatGPT 账号、第三方模型订阅或原版 12 份知识库文件。

---

## 15. 给移植者的说明

若把本技能迁到 Hermes、OpenClaw 或其他 Agent：

- 保留 `SKILL.md` frontmatter 的 `name` 与目录名一致  
- 把文中的 `render_generated_image` / `render_edited_image` 换成该环境的生图工具名  
- `description` 仍避免 `: `、引号和 `<>`，否则部分加载器会解析失败  
- 不要把 `README.md` 当作模型主指令；模型只应加载 `SKILL.md` 与按需读取的 `references/`  

校验（在原 Grok 环境）：

```bash
bash /root/.grok/skills/skill-creator/scripts/validate-skill.sh \
  /home/workdir/.grok/skills/yanshi-wawa
```

---

## 16. 版本记录

### 1.2

- 技能更名为「厌世娃娃」，目录改为 `yanshi-wawa`  
- 不再使用原文章作者的品牌名作为技能标识  

### 1.1

- 有照片默认 LIKE_2，避免 LIKE_3 压死娃感  
- 新增 BEAUTY 层与 `references/beauty.md`  
- 皮肤改为半哑缎面，禁止脸上 glossy  
- 规定始终以用户原始照片为改图底，不叠失败中间图  
- 增加出图前 6 条自检与「防漂移」纪律  
- 增加 STYLE_PHOTO；用户抱怨不像初衷时切回 STYLE_1  

### 1.0

- 初版：IDENTITY / DOLL / MOOD / SHOT / SCALE / LAYOUT 六层  
- 20 种情绪、镜头库、巨物库、配方库、提示词母版  

---

## 17. 使用许可与免责

本技能用于个人学习与自己的出图工作流。  
视觉控制结构参考了公开营销材料，实现与提示词为独立撰写。  
实际生成效果受模型版本、原图质量、输入完整度影响，不保证与任何商业海报一致。  
请勿将输出伪称为他人付费包或官方交付物。
