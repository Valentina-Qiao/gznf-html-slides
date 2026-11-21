# gznf-html-slides

**广州南方学院深红学术答辩模板**的表现层 Skill：把逐页内容蓝图做成单个自包含、16:9 固定舞台、键盘左右翻页的 HTML 演讲页，确认无误后导出同版式 PDF。

- 高度复刻：主色 `#780000`、顶部深红导航条、左侧深红竖栏章节页、右下角红色方块页码、红字重点、微软雅黑/等线 + Georgia 英文。
- 内容拆页先用 [`ppt-content-blueprint`](https://github.com/Valentina-Qiao/ppt-content-blueprint) 出蓝图，本 Skill 只做忠实视觉化，不擅改文案。
- 内置已修复的章节页显隐规则、防溢出工艺与 PDF 脚本，沉淀了完整踩坑复盘。

## 目录

- `SKILL.md`：工作流、品牌 Token 摘要、7 条必守坑、PDF 指引。
- `references/`：`design-tokens.md`（色板/字体/组件 class）、`pitfalls.md`（踩坑与避坑）、`pdf-export.md`。
- `assets/boilerplate.html`：可直接改造的单文件骨架。
- `scripts/export_pdf.py`：Playwright 逐页截图 + PyMuPDF 合成 16:9 PDF。

## 配套 Skill

- 内容层（通用）：[`ppt-content-blueprint`](https://github.com/Valentina-Qiao/ppt-content-blueprint)
- 表现层·任意 PPTX 模板的通用工艺：[`pptx-stage-slides`](https://github.com/Valentina-Qiao/pptx-stage-slides)
- 导航入口与总览：[`presentation-slides-skills`](https://github.com/Valentina-Qiao/presentation-slides-skills)
