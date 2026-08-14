---
name: figure-ultra
description: 统一图表生成技能（Draw.io + Excalidraw + Matplotlib 三引擎）：生成流程图、系统架构图、UML 时序图、ER 图、状态机图、思维导图、数据流图、泳道图、类图、深度学习模型架构图、学科示意图（数理化生史地语）、手绘白板草图，以及论文级数据图（折线图、柱状图、散点图、雷达图，dpi=300）。支持风格迁移、多图文章配图、PNG/SVG/PDF 导出。当用户要求画图、画架构图/流程图/时序图/ER 图/状态图/思维导图、可视化模型结构、论文或技术文章配图、draw.io/.drawio、Excalidraw/手绘/白板，或提供数据要求画折线图/柱状图/散点图/雷达图、导出图片格式时使用本技能；引擎由路由自动选择（结构图 → Draw.io，手绘/白板 → Excalidraw，数据图 → Matplotlib）。
---

# figure-ultra：统一图表技能

本技能整合了三套引擎——Draw.io（专业结构化图表）、Excalidraw（手绘白板图）、Matplotlib（论文级数据图）——用「路由 + reference」统一管理：主文件只做路由决策和核心规则，详细规范按需读取 `references/`。

整合来源：`drawio-diagram`（模型架构/学术图/学科图/风格迁移）+ `drawio-chart`（业务架构/技术文章配图/导出）+ `excalidraw-diagram`（手绘图）+ `excalidraw-diagram-generator`（图类型模板/图标库）+ `paper-plot-skills/plot-from-data`（8 种论文数据图风格）。

## Step 0：引擎路由（Draw.io vs Excalidraw vs Matplotlib）

| 用户需求特征 | 引擎 | 后续 |
| --- | --- | --- |
| 论文/技术文档配图、审稿级严谨、精确坐标对齐、UML 规范 | **Draw.io** | Draw.io Step 1 |
| 需要导出 PNG/SVG/PDF、`.drawio` 格式 | **Draw.io** | Draw.io Step 1 |
| 用户明确说「draw.io」「diagrams.net」`.drawio` | **Draw.io** | Draw.io Step 1 |
| 手绘风、白板讨论、草图发散、头脑风暴 | **Excalidraw** | 读 `references/excalidraw-basics.md` |
| 用户明确说「Excalidraw」`.excalidraw`「手绘」「sketch」 | **Excalidraw** | 同上 |
| 用户提供数据（数字/数组/CSV）要折线图、柱状图、散点图、雷达图 | **Matplotlib** | 读 `references/plot-catalog.md` |
| 「把数据画出来」「训练曲线」「消融对比柱状图」「t-SNE 可视化」 | **Matplotlib** | 同上 |

边界判断：节点连线类结构图（架构、流程、时序）→ Draw.io；坐标轴数据类图（有 x/y 数值、对比系列）→ Matplotlib；手绘/白板 → Excalidraw。「画个架构图」无风格限定时默认 Draw.io。Excalidraw 导出的 SVG/PNG 可再导入 Draw.io，反向不适用。

---

## Draw.io 引擎路由

### Step 1：任务模式识别

| 模式 | 处理 |
| --- | --- |
| 单张图生成 | 继续 Step 2 |
| 一篇文章生成多张图 | 读 `references/use-cases.md` 多图文章模式（2-6 张、每图一个 diagram page） |
| 用户提供参考图要求「按这个风格画」 | 读 `references/style-migration.md`，跳过 Step 2 |
| 修改已有 `.drawio` | 读 `references/xml-basics.md` 后按需改动 |

### Step 2：风格预设选择

| 场景 | 风格 | 读取 |
| --- | --- | --- |
| 论文、深度学习模型架构、感受野/注意力等学术示意图 | **学术风** | `references/style-academic.md` |
| 业务系统架构、微服务、技术文章配图、产品文档 | **产品风**（floracat 语义配色） | `references/style-product.md` |
| 学科示意图（几何、坐标系、实验装置、地图等） | 学科图规范 | `references/edu/` 对应学科文件 |
| 用户给了参考图 | 以参考图为准 | `references/style-migration.md` |

判断线索：「论文/模型/Transformer/CNN 架构」→ 学术风；「微服务/网关/数据库/文章配图」→ 产品风。

### Step 3：图表类型路由

| 图表类型 | 读取 |
| --- | --- |
| 流程图 / 业务架构图 / ER 图 / 状态机图 / 思维导图 | `references/diagram-generic.md`（+ 所选风格文件） |
| UML 时序图（参与者、生命线、激活条、消息） | `references/diagram-sequence.md` |
| 深度学习模型架构（Transformer/CNN/检测网络/感受野/注意力） | `references/diagram-ml-architecture.md`（+ `style-academic.md`） |
| 学科图 | 数学 `references/edu/edu-math.md` · 物理 `edu-physics.md` · 化学 `edu-chemistry.md` · 生物 `edu-biology.md` · 地理 `edu-geography.md` · 历史 `edu-history.md` · 语文 `edu-chinese.md` |

需要导出或命名时读 `references/export-and-files.md`。

---

## Excalidraw 引擎路由

1. 读 `references/excalidraw-basics.md`（文件结构、元素约定、默认配色、输出格式）
2. 按图类型读 `references/excalidraw-advanced.md`（9 种图类型路由 + 8 个现成模板 + 元素数量控制 + 图标库脚本工作流）
3. 同类型图优先从 `templates/*.excalidraw` 起步替换，不从零拼 JSON
4. 需要字段级定义时读 `references/excalidraw-schema.md` / `references/excalidraw-element-types.md`
5. AWS/GCP/K8s 等专业图标需求：优先用 `scripts/add-icon-to-diagram.py` 和 `scripts/add-arrow.py`（图标 JSON 不进上下文），见 `excalidraw-advanced.md` 图标库章节

---

## Matplotlib 引擎路由（数据图）

1. 读 `references/plot-catalog.md`（8 种论文图风格目录 + 数据替换规则）
2. 按图类型选定风格后读 `references/plot/<style_name>.md`（精确 rcParams、配色、字号）
3. 复制 `scripts/plot/<script>.py` 替换数据区，运行生成 dpi=300 PNG
4. 训练曲线 → `line_*` 系列；消融/对比 → `bar_*`；降维聚类 → `scatter_*`；多维对比 → `radar_dual_series`

---

## 核心规则（三引擎共用）

**先规划，再生成**：生成前明确图表类型、单页还是多页、布局方向、哪些连线需要标签。不要边写边想结构。

**信息完整优先于视觉**：结构清晰 > 视觉统一 > 好看。连线标签短小；长说明放节点或旁注节点。同类节点同尺寸、同字号、同配色。

**最小必要输入**：主题、图类型、关键节点/实体/步骤、关系、是否导出及格式。信息不完整但可合理推断时直接推断并在结果中说明假设；仅缺关键结构信息时才追问。

**Draw.io XML 严格性**（最常见失败点）：

- 标签正确闭合：`<mxCell>` 对应 `</mxCell>`；节点 `vertex="1"`、连线 `edge="1"`；id 唯一递增
- 连线 `source`/`target` 指向存在节点；特殊字符转义 `&amp;` `&lt;` `&gt;`
- **value 纯文本规则**：`value` 默认纯文本，换行用 `&#xa;`，禁止嵌入 HTML 标签；样式串中 `html=1` 仅服务布局行为；交付前搜索残留 `<b>`、`<br>`

**Excalidraw JSON 严格性**：id 唯一、坐标不重叠、text 元素全部 `fontFamily: 5`、字号 ≥16、单图元素 ≤20。

## 工作流

1. Step 0 完成引擎路由，收集最小必要输入
2. 按对应引擎的 Step 1-3 完成 reference 路由
3. 生成顺序：标题 → 容器/分组 → 核心节点 → 连线 → 标签与旁注
4. 执行核心规则自查清单
5. 导出仅按用户要求执行（本机无 `drawio` CLI 时保留源文件并给手动命令，见 `references/export-and-files.md`）

## 验证清单

- [ ] 引擎选择正确（专业/导出 → Draw.io，手绘/白板 → Excalidraw）
- [ ] 图表类型与任务匹配，主要节点齐全、关系方向正确
- [ ] XML 标签闭合 / JSON 语法正确，id 唯一
- [ ] Draw.io value 无 HTML 残留；Excalidraw 文字全部 fontFamily=5
- [ ] 同类节点样式一致，连线不穿过关键文字
- [ ] 多页文件页面名清晰（英文小写中划线）
- [ ] 导出文件命名符合规范（`{name}-flow.drawio` 等）

## 失败处理

- 需求模糊：先给推断的图表结构，再说明假设
- 导出失败：保留源文件，报告原因和可手动执行的命令
- 内容更适合 Mermaid（用户只要代码不要文件）或位图插画：明确说明并建议替代方式

## 输出要求

- 默认返回生成文件路径；执行了导出则同时返回导出路径
- 存在假设、删减或结构调整时，用一句话说明
