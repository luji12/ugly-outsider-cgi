---
name: ugly-outsider-cgi
description: 将用户提供的个人照片转换成结构完整、主体清晰但故意丑怪笨拙的 early 3D render 角色画面。用于照片风格转换、naive 3D character、业余 3D 动画、primitive CGI、early computer graphics、outsider animation、坏掉的低分辨率材质和反精致视觉实验；默认保留主体身份、物种、姿势、关键道具、颜色关系和原始构图，以完全没有美术基础的外行初学者做出的低面数曲面形体、僵硬表情、错误比例、破损拉伸贴图、粗糙颗粒材质、flat/Gouraud shading 和低配背景重构照片，不把丑感做成碎裂模型或随机肢体故障。
---

# 故意丑怪 CGI 照片转换

把真实照片重构成一个完全没有美术基础的外行初学者用早期 3D 软件做出的完整角色画面。目标是：**主体和动作认得出来，模型笨拙粗糙，材质像坏掉的低配贴图，画面像早期电脑生成的动画截图**。丑感来自设计能力不足和渲染质量有限，不来自模型被炸碎。

## 核心平衡

1. **先识别，再丑化。** 保留主体数量、物种/身份、姿势、关键配件、颜色关系和主要动作；比例和造型可以明显偏离原图。
2. **曲面低面数，不是方块。** 使用稀疏多边形近似圆头、躯干、四肢和尾巴，保留弧面轮廓；不要做成立方体、体素或积木。
3. **模型完整，材质不可靠。** 可以有贴图拉伸、错位、颜色溢出、粗糙颗粒和低质量渲染，但默认不缺肢、不碎裂、不出现大洞。
4. **故意像外行。** 不追求准确解剖、漂亮 low-poly、干净拓扑或现代游戏资产；要像初学者凭感觉拼出的第一版角色。

## 工作流程

1. 使用 `view_image` 查看输入照片，记录主体数量、物种/身份、动作、姿势、关键配件、主色和背景关系。
2. 将输入照片作为 edit target，而不是只当作氛围参考；先锁定主体识别和构图，再做角色化重建。
3. 为主体选择 2–4 个识别锚点。动物优先保留物种轮廓、耳朵/角/尾巴、毛色块、眼睛、嘴部和关键姿态；人物优先保留脸型、发型、服装和动作。
4. 将主体拆成少量大形：头部/主体块、躯干块、四肢或支撑件、附属件和简单脸部部件。
5. 使用 `image_gen` 的编辑流程生成位图。对本地附件先用 `view_image` 使其进入视觉上下文；如有风格参考图，只借鉴设计语言，不复制参考角色身份。
6. 按“角色设计 → 曲面低面数 → 坏掉的材质 → early 3D 渲染 → 简化背景”顺序组织提示词。
7. 生成后检查：主体是否可识别，模型是否像外行做的，材质是否明显粗糙/拉伸，画面是否有早期 3D 感，以及是否避免了过度碎裂和现代精致感。

## 角色设计语言

### 大形与比例

- 使用少量宽大、圆钝、块状但仍有弧面的几何体，让头部、躯干、四肢和附属件关系清楚。
- 允许头部过大、躯干过短或过厚、四肢粗细不一致、耳朵/角度略不对称、爪子或蹄子笨重。
- 使用 `simple primitive forms`, `large intentional shapes`, `crude character blockout`, `unfinished amateur model`；不要只写 `low-poly`，避免生成漂亮的专业游戏模型。
- 轮廓要像一个人做出的选择，不要变成随机碎片、融化物或无法判断的抽象块。

### 新手造型的硬判定

- 新手感必须来自建模规划不足，而不是只给成熟模型降面数：部件像分别做完后粗略拼接，接缝、转折和体块关系笨拙，局部面密度突然变多或变少，邻近部件的面大小明显不一致。
- 必须有至少两处明显的大比例错误和两处建模能力不足证据：例如头部过大、躯干过短、四肢粗细不一、爪子过重、耳朵长度不同、部件连接处塌陷或被压扁、局部 edge flow 没有规划。
- 不接受“漂亮的专业 low-poly”：如果轮廓、面密度、部件连接和明暗都过于统一，结果应判为失败，即使它有 flat shading 或像素化。
- 抽象夸张指保留物种和动作识别前提下的形体选择失控，不指碎裂、缺肢、随机漂浮部件或纯抽象纹理。

### 物种与身份识别

- 在写丑化之前先明确物种/身份和 2–4 个识别锚点。
- 狗仍要有犬类头部、鼻子、耳朵、四肢和犬类姿态；猫仍要有猫耳、尾巴和猫脸；人物仍要保留服装、发型和动作关系。
- 可以把毛发、肌肉、皮肤或衣物简化成颜色块和雕塑形体，但不能删除关键结构。

### 脸部与表情

- 使用少量独立、明显、略显笨拙的脸部部件：简单眼睛、粗眉、钝鼻子、独立嘴部/口鼻块和简单耳朵。
- 让眼睛、鼻子、嘴部比例不够自然，轻微不对称，表情死板、迟钝、困惑或没有灵魂，但仍符合主体物种。
- 脸部像被粗略安装到头部表面，不要做真实解剖、真实眼球高光、复杂口腔或电影级表情。
- 五官位置关系必须保持正常可读：双眼大致处于同一水平线，鼻子位于口鼻块中央，嘴部位于鼻子下方；丑感来自简化、夸张尺寸、笨拙部件形状和僵硬表情，不来自眼睛、鼻子或嘴巴的错位。

## 坏掉的材质

- 使用低分辨率 bitmap/cutout 贴图，像几块破损图像被硬贴在模型上：贴图岛、脏色块、颜色溢出、局部接缝和错误比例的图案。
- 必须让局部 UV 有被拉伸、压扁或错位的感觉：条状纹理被拖长，图案在曲面上变形，局部 texel density 不一致。
- 表面呈现粗糙多孔的火山石、浮石或失败 bump/displacement 感：可见的粗大颗粒、凹坑、结块和不均匀起伏。颗粒要能在中景读出，但不要把整张图都变成麻点。
- 保持主体 1–3 个主色和清晰的颜色关系；让材质反应失控但不要覆盖掉物种识别。
- 必须把三种效果分开：polygon facets 是几何和面光，bitmap/cutout patches 是贴图故障，large coarse bumps 是粗糙度或 bump/displacement；不能用统一的多边形噪点同时冒充三者。
- 贴图故障要能被直接看见：至少出现局部贴图岛、错误比例的图案、被拖长或压扁的纹理条、不同 texel density、接缝或颜色溢出。低分辨率本身不等于坏贴图。
- 大颗粒粗糙度只集中在主体和服装上，使用少量但尺度很大的凹坑、结块和多孔颗粒；背景、眼睛和关键识别点保持相对干净。
- 不要变成手绘黏土、灰泥、细腻毛发、真实皮肤、光滑塑料或现代 PBR 材质。
- 材质故障和模型结构故障分开：允许贴图坏掉，默认禁止缺面、断肢、碎片化身体和随机多余器官。

## Early 3D render 语言

- 使用 `very low polygon count` 和稀疏的大三角面/四边面近似弧面形体；明确保留 `curved organic silhouettes`，不是 cubes、voxels 或 block-only geometry。
- 面密度可以不均匀、边线笨拙、局部突然多面或少面，没有 subdivision 和拓扑清理；但整体仍是完整角色。
- 使用 `flat shading`, `crude Gouraud shading`, `vertex-lit polygons`, `faceted curved surfaces`，保留可见面片明暗。
- 画面像早期工作站或家用电脑输出：低分辨率 raster、明显锯齿、有限色深、色阶断裂、简单 bitmap filtering、弱景深和基础硬阴影。
- 渲染质量明显低于现代游戏：低动态范围、粗糙颜色采样、糊成块的阴影和不干净的边缘；不要只是添加复古滤镜。
- 不使用现代 PBR、实时引擎级反射、细腻全局光、电影级体积光、光滑 subdivision、干净边缘或高级材质分层。

## 生成与交付纪律

- 普通单图编辑只调用一次 `image_gen` 并交付一张最终图；不要先生成造型版、再生成材质版，因为第二次调用会重新生成整张图，无法可靠锁住第一版几何。
- 如果结果仍像专业 low-poly，应优先重写造型约束并重新生成，不要继续叠加像素滤镜或全画面颗粒。

## 背景、灯光与镜头

- 将背景简化为低信息的 3D 场景：纯色墙面、简单地面、粗糙木桌、几何树木、低细节栅栏或少量重复块面。
- 背景可以低配，但不要抢过主体，也不要重新引入完整写实环境。
- 使用平直、正面或轻微侧面的基础光；保留简单硬阴影和低动态范围，不使用高级轮廓光、复杂反射或电影级景深。
- 尽量保留输入照片的主要构图、动作和观看关系，允许轻微笨拙的镜头和比例。

## 结构保护

以下项目默认必须完整，除非用户明确要求模型破损或故障变形：

- 主体数量、物种/身份、头部或主轮廓、关键肢体、眼睛/耳朵/嘴部、主要道具和原始动作。
- 不制造大面积空洞、断肢、碎片化身体、漂浮肢体或随机多余器官。
- 不把丑怪做成血腥、恐怖、怪物化或纯抽象纹理。

判断标准：**看起来像一个笨拙的人设计并完成了一个角色，而不是程序把模型炸碎了。**

## 默认编辑提示词模板

将下面内容改写成适合输入照片的自然英文描述，并替换方括号内容：

```text
Use the source photo as the edit target. Transform it into a deliberately ugly, abstract, exaggerated and awkward low-budget early 3D render made by a first-time amateur with no formal art training. Keep [subject], [2–4 recognition anchors], [pose/action], [key prop], the main color relationships and the original composition recognizable, but do not accurately preserve the real proportions or polished appearance.

Rebuild the subject from a few large intentional primitive forms: [main head/body], [simple torso], [limbs or supports], [attached parts] and a few crude facial components. Use very low polygon count with sparse broad facets and curved organic silhouettes, not cubes, voxels or block-only geometry. Make the head much too large or heavy, the torso too short or thick, the limbs chunky and mismatched, and the hands/paws/feet crude. Make adjacent parts look separately made and awkwardly joined, with visibly uneven polygon density, inconsistent facet sizes, pinching, flattening and poorly planned edge flow. Keep the model structurally complete, but make it clearly look like an unfinished first character blockout rather than a polished low-poly asset.

Use a broken low-resolution bitmap material: visible damaged bitmap/cutout patches, stretched and squashed UVs, dragged texture strips, mismatched texture islands, dirty color leakage, wrong texel density, wrong texture scale and visible seams. Keep these texture failures visibly distinct from the polygon facets. Give the subject an uneven coarse porous surface with large volcanic-rock-like bumps, pits and chunky relief, restrained to the subject and readable as a bad early-3D roughness/bump material rather than fine noise, clay or plaster. Damage the material, not the anatomy.

Keep the facial features stylized but coherently placed: simple eyes at roughly the same horizontal level, a centered nose on the muzzle, and a simple mouth below the nose. Make them crude, oversized or underspecified, but do not displace or randomly misalign the facial layout.

Render with flat or crude Gouraud shading, visible faceted curved surfaces, low-resolution raster output, jagged aliased edges, limited color depth, hard color steps, simple bitmap filtering and muddy hard shadows. Simplify the background into a low-detail primitive 3D scene with [background elements]. The result should feel naive, ugly, stiff, cheap and strangely memorable, while remaining clearly recognizable as the source subject.

Avoid polished professional low-poly art, accurate anatomy, clean topology, smooth subdivision, modern PBR, photorealistic fur or skin, cinematic lighting, glossy toy rendering, excessive fragmentation, missing limbs, large holes, horror, gore, text and watermark.
```

## 迭代规则

- 如果太像专业 low-poly：增加 `first-time amateur`, `no formal art training`, `poorly planned edge flow`, `unfinished character blockout` 和错误比例；不要继续增加破面。
- 如果太像方块或体素：增加 `curved organic silhouettes`, `faceted cylindrical limbs`, `sparse polygon approximation of rounded forms`，并明确不是 cubes/voxels。
- 如果材质不够坏：增加 `stretched UV strips`, `dragged bitmap smear`, `mismatched texture islands`, `visible seams` 和 `coarse porous relief`；不要把背景一起做成麻点。
- 如果粗糙过头：减少 bump/displacement 的覆盖面积和密度，只保留主体上的大颗粒，保持眼睛、脸部识别点和背景相对干净。
- 如果结果太精致：增加低分辨率 raster、锯齿、有限色深、粗糙 bitmap filtering 和 muddy hard shadows，而不是只增加复古色调。
- 如果不像原主体：先恢复物种识别锚点、原始动作和关键配件，再调整丑感。
- 如果模型像坏掉的故障艺术：删除 broken topology、fragmented body、missing limbs 等方向，改回 `unfinished but structurally complete`。
- 每次迭代只改变一个主要轴：角色比例、曲面面数、脸部组件、材质故障、early 3D 渲染或背景简化。

## 交付前检查

- 一眼能认出原照片主体、物种/身份和主要动作。
- 主体由少量明确的大块曲面构成，而不是随机碎片、方块或体素。
- 角色确实像外行做的，但结构完整；丑感来自比例、表情、材质和渲染质量。
- 能看见局部低质量贴图、拉伸/错位纹理、接缝和主体上的粗大颗粒凹凸。
- 背景没有被错误地全部处理成麻点；眼睛和关键识别点没有凹陷、腐烂或恶心化。
- 能看见 flat/Gouraud shading、面片明暗、低分辨率、锯齿、色阶断裂和简单阴影。
- 没有现代 PBR、漂亮游戏资产、潮玩感、电影级灯光、血腥恐怖或文字水印。
