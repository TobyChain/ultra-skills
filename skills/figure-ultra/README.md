# figure-ultra

统一图表生成技能：**Draw.io（结构图）+ Excalidraw（手绘图）+ Matplotlib（数据图）三引擎**，用「路由 + reference」统一管理。主文件只做路由决策与核心规则，详细规范按需加载。

## 引擎路由

| 需求特征 | 引擎 | 输出 |
| --- | --- | --- |
| 论文/技术文档配图、架构/流程/时序/ER/状态/思维导图、需导出 PNG/SVG/PDF | Draw.io | `.drawio` + 可导出 |
| 手绘风、白板讨论、草图发散、头脑风暴 | Excalidraw | `.excalidraw` |
| 提供数据要折线图/柱状图/散点图/雷达图（论文级） | Matplotlib | dpi=300 PNG |

## 能力总览

**Draw.io 引擎**（合并自 drawio-diagram + drawio-chart）：

- 双风格预设：学术风（论文/模型架构，浅色底粗描边）与产品风（业务架构/技术文章，floracat 语义配色）
- 图类型：流程图、系统架构、UML 时序图（生命线/激活条精确坐标）、ER、状态机、思维导图
- 深度学习模型架构专用工作流：Transformer/CNN/检测网络/感受野/注意力模板
- 学科示意图 7 科：数学几何/坐标系、物理、化学、生物、地理、历史、语文
- 风格迁移模式：参考图 + 内容 → 按参考图风格生成新图
- 多图文章模式：一篇文章 2-6 张图，同一 `.drawio` 文件多个 diagram page
- 统一裁决的 value 纯文本规则（`&#xa;` 换行，禁 HTML 标签）

**Excalidraw 引擎**（合并自 excalidraw-diagram + excalidraw-diagram-generator）：

- 9 种图类型路由：流程/关系/思维导图/架构/DFD/泳道/类图/时序/ER
- 8 个现成模板起步，不从零拼 JSON
- 图标库工作流：AWS/GCP/K8s 图标经 Python 脚本注入，图标 JSON 不进上下文
- 全部 text 元素 `fontFamily: 5`（Excalifont）

**Matplotlib 引擎**（来自 paper-plot-skills/plot-from-data）：

- 8 种论文图风格：配对柱状+增益箭头、分组消融斜线柱、置信带折线、断点训练曲线、L形 spine+inset、t-SNE 聚类、折断轴散点、双系列雷达
- 「选模板 + 替换数据区」工作流，全部 dpi=300 PNG

## 目录结构

```text
figure-ultra/
├── SKILL.md                    # 三引擎路由 + 核心规则
├── references/
│   ├── xml-basics.md           # Draw.io XML 规则（合并两套，统一纯文本 value）
│   ├── style-academic.md       # 学术风版式与配色
│   ├── style-product.md        # 产品风（floracat 语义配色）
│   ├── style-migration.md      # 风格迁移模式
│   ├── diagram-generic.md      # 流程/架构/ER/状态/思维导图布局
│   ├── diagram-sequence.md     # UML 时序图精确坐标规则
│   ├── diagram-ml-architecture.md  # 深度学习模型架构工作流
│   ├── edu/                    # 学科图 × 7
│   ├── export-and-files.md     # 导出命令与命名规范
│   ├── use-cases.md            # 提示词示例与多图文章模式
│   ├── excalidraw-basics.md    # Excalidraw 文件结构与元素约定
│   ├── excalidraw-advanced.md  # 图类型路由/模板/图标库
│   ├── excalidraw-schema.md    # 完整 JSON schema
│   ├── excalidraw-element-types.md  # 元素类型规格
│   ├── plot-catalog.md         # 8 种数据图风格目录
│   └── plot/                   # 8 种数据图精确参数 × 各风格
├── templates/                  # 8 个 Excalidraw 起步模板
└── scripts/
    ├── add-icon-to-diagram.py  # 图标注入（不进上下文）
    ├── add-arrow.py            # 箭头注入
    ├── split-excalidraw-library.py  # .excalidrawlib 拆分
    ├── README.md
    └── plot/                   # 8 个 matplotlib 复现脚本
```

## 使用

将本目录放入技能目录（如 `~/.qoder/skills/figure-ultra/` 或对应平台的 skills 路径）。触发词：画图、架构图、流程图、时序图、ER 图、状态图、思维导图、模型结构、论文配图、draw.io、Excalidraw、手绘/白板、把数据画出来、折线/柱状/散点/雷达图。

导出依赖：Draw.io 导出需本机 `drawio` CLI（未安装时保留源文件并给手动命令）；数据图需 Python + matplotlib + numpy。

## 整合来源

| 来源 | 贡献 |
| --- | --- |
| 本地 `drawio-diagram` | 模型架构/学术图/学科图×7/风格迁移/UML 时序精确坐标 |
| `drawio-chart`（Snailclimb/AIGuide） | 产品风语义配色/XML 纯文本规则/导出命令/多图文章模式 |
| 本地 `excalidraw-diagram` | 手绘图文件结构与默认配色 |
| `excalidraw-diagram-generator`（github/awesome-copilot） | 9 图类型路由/8 模板/图标库脚本 |
| `plot-from-data`（Trae1ounG/paper-plot-skills） | 8 种论文数据图风格与复现脚本 |

冲突裁决：value 内 HTML → 统一默认纯文本 + `&#xa;`；两套 Draw.io 配色 → 保留为双风格预设按场景路由；时序图 → 采用精确坐标版规范。
