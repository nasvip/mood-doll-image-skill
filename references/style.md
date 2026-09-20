# 材质微表面、造型预设与光学系统 (Style, Materials & Optics)

本模块定义厌世娃娃（Mood Doll）的物理微观材质、服装质感、造型预设与高级影棚布光系统，特别深度适配 **`gpt-image-2.5-sunburst`** 的物理渲染引擎。

---

## 1. 物理核心材质规范 (Physical Material & Micro-Surface)

厌世娃娃的核心精髓是**高级半哑缎面树脂（Dry Satin Resin）**，具有微观触感与次表面散射，彻底告别廉价塑料与出汗蜡像：

| 属性 | 物理参数 / 规范描述 | 目的与效果 |
|---|---|---|
| **面部基底** | `dry satin resin, tactile matte finish, micro-surface roughness 0.35` | 触感细腻的哑光树脂肌理，杜绝全脸大面积高光反光 |
| **次表面散射** | `subsurface scattering (SSS depth 1.0mm), soft translucent porcelain warmth` | 光线在树脂表层产生微透散射，提供有温度的人偶骨相而非僵硬死白 |
| **微观特征** | `delicate micro-freckles across nose bridge, soft pore texture` | 增加微距下的人工手作收藏娃高级细节 |
| **高光约束** | `strictly isolated specular highlights: tiny pin-point on lower lip center and inner eye tear ducts only` | 限制高光在眼角与下唇微小点，额头、鼻翼、脸颊全哑光 |
| **头发质感** | `distinct individual hair strands, natural hairline flyaways, fine hair fibers` | 自然分束发丝与微小碎发，杜绝塑料头盔感 |
| **服装材质** | `tactile natural fabrics: heavy wool, fine-rib knit, soft washed leather, crisp cotton poplin` | 织物表面拥有真实物理纹理与自然吸光特性 |

---

## 2. 造型预设库 (Style Presets)

- **`STYLE_1 办公室厌世` (初衷经典款，默认)**：
  - **服装**：合体深色西装（Black or charcoal tailored suit）、浅色挺括衬衫、酒红色窄版领带（Wine-red skinny tie）；
  - **配饰与道具**：外带咖啡纸杯（Minimalist paper coffee cup）；
  - **光线**：大面积北向天光/百叶窗侧光；
  - **适用**：最符合原版海报的高冷职场厌世氛围。用户抱怨「不像初衷」时优先切回此款。

- **`STYLE_2 家居慵懒摆烂`**：
  - **服装**：粗针织毛衣（Chunky oversized knit sweater）、圆框金属细眼镜；
  - **环境**：深色布艺沙发、慵懒半卧、散落的书本与马克杯。

- **`STYLE_3 重工机车侧颜`**：
  - **服装**：做旧质感重磅黑皮衣（Distressed black leather biker jacket）、银质极简耳钉；
  - **光线**：深色背景、经典的伦勃朗侧逆光、强烈的角色轮廓分界。

- **`STYLE_4 高定极简大刊`**：
  - **服装**：极简剪裁单色廓形风衣或高领羊绒衫；
  - **环境**：纯净高级灰平涂背景或微弱夜景光斑虚化。

- **`STYLE_PHOTO 沿用参考图`**：
  - 严格提取并沿用用户参考照片中的服装款式、颜色及随身配饰（如红T恤、特定图案或头戴式耳机）。

---

## 3. 光学摄影与布光系统 (Studio Lighting & Camera)

专为 `gpt-image-2.5` 渲染引擎设计的光学参数：

```text
[Camera & Optical Parameters]
- Sensor & Lens: Hasselblad H6D-100c medium format camera, HC 100mm f/2.2 portrait prime lens.
- Depth of Field: shallow depth of field, optical precision focus on the iris, creamy smooth bokeh roll-off.
- Key Light: 120cm octabox soft light positioned at 45 degrees, highly diffused soft wrap-around illumination.
- Fill Light: subtle negative fill on shadow side to preserve sculptural bone structure without harsh shadows.
- Separation: faint warm rim light along hair edges to separate character from background.
```

---

## 4. 结尾材质定调母句 (Ending Quality Anchor)

提示词末尾必须包含以下纯正向定调短语，强化材质并切断廉价塑料感：

```text
High-end 3D art-toy collectible render, tactile fabrics, dry satin resin skin with subtle SSS, controlled softbox fashion lighting, gallery quality finish.
```
