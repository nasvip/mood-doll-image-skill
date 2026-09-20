# 厌世娃娃 · Mood Doll Image Skill (`yanshi-wawa`)

<p align="center">
  <img src="https://img.shields.io/badge/Skill_Name-yanshi--wawa-ff4d4f?style=flat-square" alt="Skill Name" />
  <img src="https://img.shields.io/badge/Version-1.2-blue?style=flat-square" alt="Version" />
  <img src="https://img.shields.io/badge/Type-Visual_Workflow-8a2be2?style=flat-square" alt="Type" />
  <img src="https://img.shields.io/badge/License-MIT-green?style=flat-square" alt="License" />
  <img src="https://img.shields.io/badge/Platform-Hermes%20%7C%20Grok%20%7C%20Multi--Agent-orange?style=flat-square" alt="Platform" />
</p>

---

## 📌 核心速览

| 项目属性 | 规范说明 |
|---|---|
| **GitHub 仓库名** | `nasvip/mood-doll-image-skill` |
| **技能目录名 (Skill Name)** | `yanshi-wawa`（**必须与 `SKILL.md` 中 `name` 完全一致**） |
| **中文名称** | 厌世娃娃 · 3D人物视觉控制系统 |
| **工作流语言** | 中文控制指令 + 纯正英文精准生图提示词 |
| **适用环境** | Hermes Agent、Grok 自定义技能、OpenClaw 及主流多 Agent 框架 |

> ⚠️ **关键注意**：本技能标准定义名称与目录名为 **`yanshi-wawa`**。通过 Git 克隆或手动解压时，请确保目标文件夹名称为 **`yanshi-wawa`**，以保证各 Agent 框架的 YAML 前置元数据解析与技能加载器正常识别。

---

## 1. 这是什么

这是一套专为 AI Agent（Hermes、Grok 等）设计的高维人物视觉控制技能。

它的核心理念不是粗暴地「套一个厌世滤镜」，而是把「一个人」解构成 7 个彼此正交、可独立旋拧的物理与视觉旋钮：

```text
[原始输入] ──> 1. IDENTITY ──> 2. DOLL ──> 3. BEAUTY ──> 4. MOOD ──> 5. SHOT ──> 6. SCALE ──> 7. FINISH ──> [最终成图]
               (锁住身份)     (娃娃化骨架)   (肤质精修)    (20种情绪)   (构图机位)    (巨物/微缩)   (排版材质)
```

1. **先锁住「这个人是谁」**：固定骨骼五官、发型配饰与核心特征，拒绝换脸与漂移；
2. **再决定娃娃化程度**：从轻微 Stylize 到标准收藏娃，再到极端大头树脂设计师玩具；
3. **分层叠加视觉属性**：独立控制美颜、20种精微态度情绪、镜头机位、空间尺度、材质质感与杂志封面排版。

同一角色在连续多轮生成时，**身份与娃娃骨架保持恒定**，只修改你指定的那一层旋钮，彻底解决 AI 绘图「一改提示词就换了个人」的顽疾。

### 典型触发词
- 厌世娃娃 / 3D 娃娃化 / 收藏娃 / 情绪表情库
- 高相似锁 / 巨物微缩 / 杂志封面排版
- 把这张成人照片做成收藏娃 / 情绪娃娃写真

---

## 2. 目录结构

```text
yanshi-wawa/
├── README.md                 # 完整工程文档与使用指南
├── SKILL.md                  # 主调度指令（给 Agent 模型读取，含 YAML frontmatter）
├── LICENSE                   # 开源许可证（MIT License）
├── .gitignore                # 忽略文件配置
├── assets/
│   └── mood-index.txt        # 20 种态度情绪编号与速查表
└── references/
    ├── likeness.md           # 人物相似度锁 LIKE_1–3
    ├── doll.md               # 娃娃化骨架锁 DOLL_1–3
    ├── beauty.md             # 美颜精修层 BEAUTY_OFF / SOFT / STRONG
    ├── mood.md               # 20 种精微情绪表情器（英文提示词段）
    ├── shot.md               # 镜头、构图与机位控制
    ├── giant.md              # 巨物 / 微缩 / 超空间比例
    ├── style.md              # 材质、造型预设与布光
    ├── layout.md             # 素图 / 杂志排版 / 中英大字 / 双人
    ├── recipes.md            # 现成爆款视觉配方 R01–R11
    └── prompt-template.md    # 改图 / 文生图提示词母版与会话 DNA
```

- `SKILL.md` 掌控主流程控制逻辑与自检纪律；
- `references/` 为模块化分层知识库，Agent 仅在调度具体层时按需读取，避免上下文污染。

---

## 3. 安装与配置

由于 GitHub 仓库名为 `mood-doll-image-skill`，而技能内定义标识名为 `yanshi-wawa`，克隆时**请务必直接重命名目标目录为 `yanshi-wawa`**。

### 方式 A：Hermes Agent 环境（推荐）
将本技能克隆或软链至技能目录：
```bash
cd ~/.hermes/skills
git clone https://github.com/nasvip/mood-doll-image-skill.git yanshi-wawa
```
重载或新起会话即可自动识别技能 `yanshi-wawa`。

### 方式 B：Grok 环境
将文件夹放置在 Grok 的用户技能路径下：
```bash
cd /home/workdir/.grok/skills
git clone https://github.com/nasvip/mood-doll-image-skill.git yanshi-wawa
```
验证技能合法性：
```bash
bash /root/.grok/skills/skill-creator/scripts/validate-skill.sh /home/workdir/.grok/skills/yanshi-wawa
```

### 方式 C：其他 Agent / 手动加载
直接将整个 `yanshi-wawa` 文件夹放入对应系统的 `skills/` 目录，确保 `SKILL.md` 位于 `yanshi-wawa/SKILL.md`。

---

## 4. 核心组装原则

### 4.1 组装顺序（绝对不可颠倒）

| 顺序 | 控制层 | 核心参考文件 | 作用说明 |
|:---:|:---|:---|:---|
| **1** | **IDENTITY** | `references/likeness.md` | 锁定性别、年龄段、脸型骨相、眉眼距、鼻翼嘴形、发型发际与标志配饰 |
| **2** | **DOLL** | `references/doll.md` | 确立娃娃骨架：头身比、杏仁大眼、厚眼睑覆瞳、短面中、半哑树脂肌 |
| **3** | **BEAUTY** | `references/beauty.md` | 平额纹、淡法令纹、均匀缎面质感；**绝不换骨、绝不幼态** |
| **4** | **MOOD** | `references/mood.md` | 精细调节眼皮开度、视线方向、眉形微挑、嘴角与肩颈姿态 |
| **5** | **SHOT** | `references/shot.md` | 确定景别（超特写/半身/全景）、焦段（85mm/鱼眼）、俯仰机位 |
| **6** | **SCALE** | `references/giant.md` | 物理比例控制：日常正常 / 城市巨人 / 巨型静物 / 微缩键帽 |
| **7** | **FINISH** | `references/style.md` + `layout.md` | 材质预设（西装/皮衣）、柔光箱布光与封面排版设计 |

> **铁律**：后执行的层**不得改写**前一层已经锁死的骨骼架构。

### 4.2 验收合格标准
- **核心及格线**：「一眼是收藏娃娃，二眼还是这个人」。
- **严重不及格（判定为生成漂移）**：
  - ❌ 真人五官比例 + 强磨皮 = 精修证件照
  - ❌ 满面反光高光油腻 = 出汗蜡像
  - ❌ 头虽很大但面容完全换了 = 另一个陌生人
  - ❌ 身体幼态婴儿化 = 违背成人收藏娃准则

### 4.3 实拍总结出的对抗性硬规则
当输入为真人照片改图时，模型极易黏滞在原图比例上：
1. **默认档位**：有照片时默认使用 **`LIKE_2 + DOLL_2`**，切忌一开始就上 `LIKE_3`（高锁会彻底压死娃娃感）。
2. **母句完整**：每张提示词必须完整粘贴 DOLL 母句，严禁仅写增量描述。
3. **材质锁死**：皮肤默认使用 **半哑缎面树脂（dry satin resin）**，严禁使用 `polished / glossy / wet / dewy`，防止生成出汗油面。
4. **拒绝叠图修补**：如果连续对话出现漂移变写真，下一张**必须退回以用户原始照片为基底**，绝不叠在失败图上越修越偏。

---

## 5. 档位配置速查

### 5.1 默认档位

| 参数项 | 默认值 | 详细说明 |
|---|---|---|
| **LIKE** | 2（有照片）/ 1（纯文字） | 2 档锁眉眼鼻嘴与特征发型，保留娃娃重塑空间 |
| **DOLL** | 2 | 标准 3D 收藏娃，杏仁大眼、厚眼睑、短面中 |
| **BEAUTY** | SOFT | 抹平额纹与法令纹，匀净肤色，不改变成年人骨相 |
| **MOOD** | 01 厌世冷眼 | 核心基底表情，半阖眼睑微斜视 |
| **SHOT** | 半身三点柔光 | 兼顾人物表情与服装材质质感 |
| **SCALE** | NORMAL | 正常人体与环境比例 |
| **LAYOUT** | 素图（CLEAN） | 纯粹高质量大片，无杂乱入画文字 |
| **画幅** | 竖构图（Portrait） | 远景、双人互动、城市巨物自动切为横构图（Landscape） |

### 5.2 20 种态度情绪表（速查）

完整英文提示词段落请查阅 `references/mood.md` 与 `assets/mood-index.txt`：

| 编号 | 情绪名称 | 核心特征与推荐搭配 |
|:---:|---|---|
| **01** | **厌世冷眼** *(默认)* | 母表情，眼皮半垂、目光斜睨、嘴角放平，极致冷感 |
| **02** | **不可一世** | 适合仰拍低机位，下颌微抬，视线下压 |
| **03** | **嚣张挑衅** | 单侧挑眉，眼神直刺镜头 |
| **04** | **轻蔑嫌弃** | 鼻翼微皱，上唇极微上提，视线偏离中心 |
| **05** | **搞怪斜眼** | 眼球极致偏侧，下眼睑微紧 |
| **06** | **疯感轻笑** | 眼神冰冷冷酷，单侧嘴角带冷笑弧度 |
| **07** | **狂气失控** | 瞳孔扩张，适合搭配鱼眼镜头（Fisheye） |
| **08** | **无语凝视** | 瞳孔居中空洞，双肩下沉，完全无互动感 |
| **09** | **高冷审判** | 眉间微平整，居高临下的冷酷气场 |
| **10** | **坏笑恶作剧** | 可与 01 混合（01 的冷眼 + 10 的微翘嘴角） |
| **11** | **疯批气质** | 似笑非笑，眼神锐利捉摸不定 |
| **12** | **摆烂到底** | 适合俯拍视角，身体重心瘫软，眼神无焦点 |
| **13** | **拽王模式** | 下颌微收，眼神向上翻视冷视 |
| **14** | **阴阳怪气** | 眉尾不对称挑起，嘴角似笑非笑 |
| **15** | **黑化前夜** | 侧逆光，眼窝加深，阴郁冰冷 |
| **16** | **慵懒上头** | 微张嘴唇，眼睑重度下垂，适合巨型咖啡杯道具 |
| **17** | **目中无人** | 完全越过镜头视线，仿佛面前空无一物 |
| **18** | **戏精附体** | 夸张的面部微表情戏剧感，适合微距超特写 |
| **19** | **反骨模式** | 侧头、斜视、紧咬后槽牙线条 |
| **20** | **宇宙级无所谓** | 彻底出世的虚无感，一切与我无关 |

---

## 6. 现成经典视觉配方

详见 `references/recipes.md`，使用时可直接指定配方编号：

- **R01 办公室厌世半身**：经典黑西装 + 酒红领带 + 窗边侧光 + 01 冷眼
- **R02 眼部标本特写**：微距切入眼眶，半垂眼睑、虹膜纹理与微小雀斑
- **R03 鱼眼失控失重**：180° 超宽鱼眼畸变 + 07 狂气 + 怼脸近距
- **R04 城市微缩巨人**：人物置身摩天楼群之间，晨雾丁达尔光
- **R05 巨型咖啡杯慵懒**：下巴抵在巨大马克杯沿，16 慵懒情绪
- **R06 机械键盘微缩**：人偶盘腿坐于机械键盘空格键上
- **R07 居家摆烂沙发**：棉质卫衣 + 凌乱沙发 + 12 摆烂俯视
- **R08 职场双人冷战**：双人办公桌左右对峙，两套独立身份锁
- **R09 机车皮衣侧颜**：重工皮革质感 + 冷调轮廓光 + 19 反骨侧视
- **R10 极简时装大刊**：纯单色高级灰背景 + 杂志排版 + 09 高冷审判
- **R11 华丽宫廷厌世**：复古繁复丝绒刺绣礼服 + 厌世面容反差

---

## 7. 提示词生成母版（示例）

有参考照片时，Agent 构建的英文提示词架构如下（完整版以 `references/prompt-template.md` 为准）：

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

Mood: [选填 20 种情绪之一的英文控制段落]
Camera: [镜头与景别描述]
Scale: [尺度与环境参考物]
Wardrobe and setting: [服装预设与光影布光]

High-end 3D character render, tactile fabrics, soft fashion lighting.
Same person. Dry satin resin face.
```

---

## 8. 常见问题与崩坏自救

| 异常现象 | 核心根因 | 修正方案 |
|---|---|---|
| **完全不像本人** | 身份骨相未锚定 | 提升至 `LIKE_2` 或 `LIKE_3`，强化发型、鼻唇、眼镜特征；**不要**把 DOLL 降回 1 |
| **太像真人写真 / 像证件照** | 娃感不足，原图权重过大 | 提高 DOLL 档位至 2 或 3，将 DOLL 母句置于提示词开头，降 `LIKE` 至 2 |
| **满脸油光 / 像出汗** | 光学词污染 | 彻底剔除 `glossy / wet / dewy`，强制 `dry satin resin` 与柔光箱布光 |
| **缺乏厌世情绪** | 眼神未定型 | 改用眼部超特写，在提示词中明确瞳孔偏侧方向与眼睑压盖比例 |
| **身材比例幼态化** | 触发了娃娃的婴儿关联 | 在提示词中强行声明 `adult bone structure, not a child` |

---

## 9. 安全边界与合规

本技能严格遵守以下原则：
- 🚫 **严禁未成年人**：拒绝未成年人照片的娃娃化、改图或微缩；
- 🚫 **严禁成人色情**：禁止生成任何性化、色情或低俗向内容；
- 🚫 **尊重肖像合法性**：知名公众人物必须具备公开合法参考源，严禁捏造与恶意冒充。

---

## 10. 开源许可与维护

- **许可证**：本项目采用 [MIT License](LICENSE) 开源。
- **整理与维护**：由 **Hermes Agent** 维护与重构优化。
- **说明**：本技能视觉控制架构源于对公开视觉设计结构的系统化重构，所有提示词工程、控制分层与参数解耦均为原创整理，旨在为多智能体生态提供工业级可靠的 3D 角色视觉控制工作流。
