# ultra-skills

Ultra 系列 Agent Skills，用于 Qoder / Claude Code / 兼容 SKILL.md 规范的 Agent 平台。两个技能都遵循「主文件做路由，细节按需加载」的渐进式披露设计，避免把全部规范塞进上下文。

## 技能列表

| 技能 | 用途 | 输出 |
| --- | --- | --- |
| [`learn-ultra`](skills/learn-ultra) | 把 GitHub 仓库、论文或技术文章转成逐模块、逐观点的深度解析报告 | 单文件离线 HTML（TOC 侧栏 + Mermaid + 代码/大白话双栏） |
| [`figure-ultra`](skills/figure-ultra) | 统一图表生成，三引擎自动路由 | `.drawio` / `.excalidraw` / dpi=300 PNG |

---

## learn-ultra

把任意 **GitHub 仓库**、**研究论文**（arXiv / PDF / Markdown）或 **技术文章/博客**（网页 URL），转换成一篇单文件、离线可读的中文交互式 HTML 解析报告。面向「深度研究」而非「快速浏览」。

**固定 9 章节结构**，保证跨主题可对比阅读：一句话理解 → 项目/论文卡片 → 为什么存在 → 架构/模块拆解 → 核心观点逐条解析 → 关键代码/公式 + 大白话 → 端到端流程 → 心智模型 → 延伸阅读。

特征：

- 三种输入分支自动识别（仓库 / 论文 / 文章），各有对应的五步分析流程
- 章节 03/04/05 强制逐条展开，不允许一段话笼统概括
- 章节 04 与 07 必须含 Mermaid 图（架构图 + 端到端调用链）
- 章节 06 至少 3 组「代码或公式 + 中文大白话」双栏对照
- 文章分支会额外用 WebSearch 补全背景资料，让报告比原文更系统

触发词：`learn-ultra`、深入解析、深入解读、学习报告、研究报告、解读论文、解读这个仓库、解读文章，或直接给出 GitHub / arXiv / 文章 URL。

---

## figure-ultra

统一图表生成技能，**Draw.io + Excalidraw + Matplotlib 三引擎**，由路由按需求特征自动选择。

| 需求特征 | 引擎 | 输出 |
| --- | --- | --- |
| 论文/技术文档配图、架构/流程/时序/ER/状态/思维导图、需导出 PNG/SVG/PDF | Draw.io | `.drawio` + 可导出 |
| 手绘风、白板讨论、草图发散、头脑风暴 | Excalidraw | `.excalidraw` |
| 提供数据要折线图/柱状图/散点图/雷达图（论文级） | Matplotlib | dpi=300 PNG |

**Draw.io 引擎**：双风格预设（学术风 / 产品风语义配色）· UML 时序图精确坐标规则 · 深度学习模型架构专用工作流（Transformer / CNN / 检测网络 / 感受野 / 注意力）· 7 科学科示意图 · 风格迁移（参考图 + 内容 → 新图）· 多图文章模式（一文 2-6 图，同文件多 page）

**Excalidraw 引擎**：9 种图类型路由（流程 / 关系 / 思维导图 / 架构 / DFD / 泳道 / 类图 / 时序 / ER）· 8 个起步模板 · AWS/GCP/K8s 图标库经 Python 脚本注入，图标 JSON 不进上下文

**Matplotlib 引擎**：8 种论文图风格（配对柱状 + 增益箭头、分组消融斜线柱、置信带折线、断点训练曲线、L 形 spine + inset、t-SNE 聚类、折断轴散点、双系列雷达），「选模板 + 替换数据区」工作流

触发词：画图、架构图、流程图、时序图、ER 图、状态图、思维导图、模型结构可视化、论文配图、draw.io、Excalidraw、手绘/白板、把数据画出来、折线/柱状/散点/雷达图。

---

## 安装

复制到 Agent 的技能目录即可：

```bash
git clone https://github.com/TobyChain/ultra-skills.git
cp -r ultra-skills/skills/learn-ultra  ~/.qoder/skills/
cp -r ultra-skills/skills/figure-ultra ~/.qoder/skills/
```

其他平台把目标路径换成对应的 skills 目录（如 `~/.claude/skills/`）。

**可选依赖**：

- Draw.io 导出 PNG/SVG/PDF 需本机 `drawio` CLI（未安装时保留源文件并给出手动命令）
- 数据图需 Python + matplotlib + numpy
- learn-ultra 的 Mermaid 与代码高亮走 CDN，断网时样式仍在、仅图表不渲染

## 设计原则

1. **渐进式披露**：SKILL.md 只放路由决策与核心规则，详细规范落在 `references/`，按任务需要读取
2. **模板优先于从零生成**：Excalidraw 从 `templates/` 起步，数据图从 `scripts/plot/` 复制改数据，降低出错率
3. **脚本承担确定性工作**：坐标变换、ID 生成、图标注入交给 Python 脚本，不消耗上下文也不会算错
4. **冲突显式裁决**：合并多来源技能时，规范冲突（如 XML value 是否允许 HTML）在主文件里给出唯一裁决，不留两套并行规则

## 来源与致谢

`figure-ultra` 整合自以下技能，冲突项已统一裁决：

| 来源 | 贡献 |
| --- | --- |
| 本地 `drawio-diagram` | 模型架构 / 学术图 / 学科图 ×7 / 风格迁移 / UML 时序精确坐标 |
| [Snailclimb/AIGuide](https://github.com/Snailclimb/AIGuide) `drawio-chart` | 产品风语义配色 / XML 纯文本规则 / 导出命令 / 多图文章模式 |
| 本地 `excalidraw-diagram` | 手绘图文件结构与默认配色 |
| [github/awesome-copilot](https://github.com/github/awesome-copilot) `excalidraw-diagram-generator` | 9 图类型路由 / 8 模板 / 图标库脚本 |
| [Trae1ounG/paper-plot-skills](https://github.com/Trae1ounG/paper-plot-skills) `plot-from-data` | 8 种论文数据图风格与复现脚本 |
