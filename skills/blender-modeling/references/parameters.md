# Blender 参数与技巧详解

配套 `SKILL.md` 使用。收录具体数值、参数组合与进阶技巧。

---

## 一、面数预算参考

| 用途 | 主要物体面数 | 场景总面数 | 备注 |
|---|---|---|---|
| 手机游戏 | 500 - 3,000 | 5 万以内 | 需 LOD，贴图合图 |
| PC 游戏主角 | 1 万 - 3 万 | — | 4 套 UV，需法线烘焙 |
| PC 游戏场景道具 | 1,000 - 8,000 | 50 万 - 200 万 | 用实例化重复物件 |
| 影视特写 | 5 万 - 50 万 | 视镜头而定 | 只在镜头内的部分加密 |
| 产品静帧 | 2 万 - 20 万 | — | 可拆件，边缘做圆角 |
| 3D 打印 | 不限，但需封闭流形 | — | 壁厚 ≥ 1mm，检查 non-manifold |

细分曲面会按倍数增面：1 级约 ×4，2 级 ×16，3 级 ×64。开细分前先确认基础网格够不够干净。

---

## 二、修改器详解

### 修改器顺序原则

修改器**从下往上算**，越靠上越晚生效。常见正确顺序（从上到下）：

```
Mirror → Subdivision → Bevel → Solidify → Boolean → Weighted Normal
```

实际规则：**生成几何的放在前面，切割的在中间，修正法线的在最后。**

### Boolean

| 参数 | 建议 | 说明 |
|---|---|---|
| 运算类型 | Difference（差集）最常用 | Union 合并，Intersect 取交 |
| Solver | Exact | 精确但慢；Fast 易出错 |
| Self | 勾选（复杂网格必开） | 处理自相交 |
| 物体位置 | 切割体略偏移 0.1-0.5mm | 避免面重合导致失败 |

失败排查顺序：

1. 两物体法线是否朝外（`Shift+N`）
2. 是否都存在非流形边（`Select > All by Trait > Non Manifold`）
3. 是否有重合面
4. 尝试先应用（Apply）其中一个修改器再运算

### Subdivision Surface

| 参数 | 说明 |
|---|---|
| Levels Viewport | 视口显示级别，1-2 即可，太高卡顿 |
| Render | 渲染级别，通常 2-3 |
| Use Limit Surface | 默认开，边缘更准确 |
| Quality | 3 足够 |

### Bevel

| 参数 | 建议 |
|---|---|
| Amount | 0.2mm - 2mm（按真实尺寸，硬表面小倒角更真实） |
| Segments | 2-3 段即可，配合细分可更少 |
| Limit Method | Angle，角度阈值 30° |
| Harden Normals | 开，避免倒角区域着色发脏 |

**真实世界几乎不存在绝对锐边**，硬表面物体加 0.2-0.5mm 倒角后渲染质感会明显提升。

### Solidify

| 参数 | 说明 |
|---|---|
| Thickness | 布料 1-2mm，塑料壳 1-3mm，金属板 1-2mm |
| Offset | -1 表示向内加厚，1 向外 |
| Use Even Thickness | 开，避免尖角处厚度塌陷 |
| Rim Fill | 开，边缘封口 |

### Mirror

- 用「镜像中心空物体」而不是物体原点，方便整体移动
- 开 Clipping，防止中缝处的点被拉过中线
- Merge 阈值 0.001，中缝会焊接

### Array

- 相对偏移（Relative Offset）适合等距复制
- 物体偏移（Object Offset）配合空物体可做渐变缩放阵列（做栅栏、地砖常用）

---

## 三、UV 展开参数

### Smart UV Project

| 参数 | 建议 |
|---|---|
| Angle Limit | 66°（默认），结构复杂的模型可降到 45° |
| Island Margin | 0.02（约 2% 间距，防贴图溢色） |
| Area Weight | 0，除非有特定区域需要更高密度 |

缺点：接缝位置不可控，适合快速打样和贴图简单的物体。

### 手动 Unwrap 流程

1. `Ctrl+E > Mark Seam` 标记切开的边（选在视觉不明显的区域：背面、接缝处、被遮挡处）
2. 全选面，`U > Unwrap`
3. 在 UV 编辑器里看是否有拉伸：打开 `Overlay > Stretch`
4. `UV > Average Island Scale` 统一纹理密度
5. `UV > Pack Islands`，Margin 设 0.01-0.02

### UV 检查清单

- [ ] 没有重叠的 UV 岛
- [ ] 没有明显拉伸（棋盘格贴图检查）
- [ ] 所有岛在 0-1 空间内
- [ ] 岛之间有足够间距（防溢色）
- [ ] 纹理密度接近（同一物体上不该有明显分辨率差异）

---

## 四、材质与 PBR 参数

### 常见材质的 Principled BSDF 数值

| 材质 | Base Color | Roughness | Metallic | 备注 |
|---|---|---|---|---|
| 哑光塑料 | 任意 | 0.4 - 0.6 | 0 | IOR 1.45 |
| 光泽塑料 | 任意 | 0.15 - 0.3 | 0 | 加 Clearcoat 1.0 |
| 拉丝金属 | 灰白 | 0.3 - 0.5 | 1.0 | 用各向异性，切线方向沿拉丝 |
| 抛光金属 | 灰白 | 0.05 - 0.15 | 1.0 | 需要环境光反射才好看 |
| 磨砂玻璃 | 白 | 0.3 | 0 | Transmission 1.0，需开 Roughness 模糊 |
| 皮肤 | 暖色 | 0.4 左右 | 0 | 加 Subsurface，Radius 设 0.5-2mm |
| 木材 | 棕 | 0.5 - 0.7 | 0 | 靠纹理做细节 |
| 布料 | 深色 | 0.8 - 0.95 | 0 | 加 Sheen 0.3 |

**金属必须 Roughness 不为 0 才真实**，纯镜面金属在现实中不存在。

### 贴图类型对应

| 贴图 | 连接目标 | 说明 |
|---|---|---|
| Base Color / Albedo | Base Color | sRGB 色彩空间 |
| Roughness | Roughness | Non-Color |
| Metallic | Metallic | Non-Color |
| Normal | Normal（经 Normal Map 节点） | Non-Color |
| AO | 与 Base Color 相乘 | Non-Color |

**切记：除 Base Color 外，所有贴图都要设为 Non-Color**，否则渲染会偏色。

---

## 五、灯光数值参考

### 三点布光（产品/人像）

| 位置 | 灯型 | 功率 | 尺寸 | 距离 |
|---|---|---|---|---|
| Key（主光） | Area | 100-300W | 1m×1m | 主体高度的 1.5 倍 |
| Fill（补光） | Area | 主光的 1/4 - 1/8 | 1.5m×1.5m | 与主光同侧或对侧 |
| Rim（轮廓） | Area / Spot | 主光的 1/2 | 0.5m | 背后 45° |

### 环境光强度

| 场景 | World Strength |
|---|---|
| 室内柔光 | 0.3 - 0.8 |
| 室外晴天 | 1.0 - 2.0（配 HDRI） |
| 影棚黑背景 | 0.01 - 0.05 |
| 夜景 | 0.05 - 0.2 |

### 色温速查

| 光源 | 开尔文 |
|---|---|
| 烛光 | 1900K |
| 白炽灯 | 2700K |
| 暖白 LED | 3000K |
| 中性白 | 4000K |
| 正午阳光 | 5500K |
| 阴天 | 6500K |
| 蓝天 | 10000K+ |

---

## 六、渲染设置

### Cycles

| 参数 | 预览 | 出图 | 说明 |
|---|---|---|---|
| Samples | 32-64 | 128-512 | 配合 Denoise 可大幅降低 |
| Denoiser | OpenImageDenoise | 同 | 不牺牲太多细节 |
| Max Bounces | 4-6 | 8-12 | 玻璃多的场景需提高 Transmission |
| Caustics | 关 | 按需 | 开了极慢，一般用假焦散 |
| Light Tree | 开 | 开 | 多灯光源加速明显 |

### EEVEE

| 参数 | 建议 | 说明 |
|---|---|---|
| TAA Samples | 16-32 | 控锯齿 |
| Ambient Occlusion | 开，距离 0.1-0.5 | 增加层次 |
| Screen Space Reflections | 开 | 金属和玻璃必需 |
| Bloom | 按需 | 发光物体需要 |
| Shadow | Soft Shadows 开，分辨率 1024+ | |

### 输出格式

| 用途 | 格式 | 说明 |
|---|---|---|
| 后期合成 | OpenEXR（Multi-layer） | 保留全部信息，32 位 |
| 直接交付 | PNG 16bit | 无损 |
| 网页/预览 | JPEG 90% | 体积小 |

---

## 七、常见报错与排查

### 「Non-manifold」几何

3D 打印和 Boolean 的大敌。检查方法：

- `Select > All by Trait > Non Manifold`
- 常见成因：内部面、孤立顶点、重合边、开口

处理：删除内部面，`M > By Distance` 合并重合点，补洞 `F`。

### 法线翻转

表现：表面发黑、渲染时内部可见。

处理：编辑模式全选 `Shift+N`；如仍异常，检查是否有重复面重叠。

### 细分后出现尖刺

成因：网格存在非平面 n-gon 或重合点。

处理：细分前三角化可疑区域（`Ctrl+T`），或合并重复点。

### 布尔后布线混乱

这是正常现象。解决方案：

- 布尔只用于**最终形态**，不要中途布尔后再改形
- 需要继续编辑时，用「重拓扑」（Quad Remesher 插件或手动）
- 或者改用「硬表面套件」类插件的非破坏性方案

### 渲染出现黑斑（Shadow Terminator）

成因：低面数曲面配合平滑着色。

处理：加细分，或提高 Light Path 的 Diffuse Bounces。

### 内存不足

- 减少细分级别
- 用 `ALT+D` 关联复制而不是 `Shift+D` 独立复制
- 大场景用 Collection Instance
- 检查贴图分辨率是否过高（4K 贴图 20 张就是 1.3GB）

---

## 八、效率技巧

### 快捷键进阶

| 操作 | 快捷键 |
|---|---|
| 沿法线挤出 | `Alt+E` |
| 循环选择边 | `Alt+Click` |
| 循环选择面 | `Ctrl+Alt+Click`（需插件） |
| 加选循环到边界 | `Alt+Click` 后 `Shift` |
| 边缘环移 | `Ctrl+Alt+滚轮` |
| 快速切换透视/正交 | `Numpad 5` |
| 局部视图 | `/` |
| 隐藏未选中 | `Shift+H` |
| 显示全部 | `Alt+H` |
| 对齐视图到选中面 | `Shift+Numpad 7`（顶视图到所选） |
| 重复上一操作 | `Shift+R` |
| 采样取色 | `I`（材质吸管） |

### 建模习惯

1. **每 15 分钟存一次**，用 `Ctrl+Shift+S` 递增版本号
2. 大改动前 `Ctrl+S` + 新建 Collection 备份
3. 用 Collection 管理场景，避免所有物体堆在一个层级
4. 命名规范：`SM_`（静态网格）/ `SK_`（骨骼网格）/ `M_`（材质）/ `T_`（贴图）
5. 用 Asset Browser 保存常用组件（螺丝、铆钉、接口）

### 检查清单（交付前）

- [ ] 法线全部朝外
- [ ] 无重合面、无内部面
- [ ] 无孤立顶点（`Select > All by Trait > Loose Geometry`）
- [ ] UV 无重叠、无越界
- [ ] 材质贴图色彩空间正确
- [ ] 修改器状态符合交付要求（保留还是应用）
- [ ] 物体变换已应用（`Ctrl+A > All Transforms`）
- [ ] 单位与比例正确
