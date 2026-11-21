# gznf-html-slides

> 表现层 Skill（品牌特化）：高度复刻「广州南方学院」深红学术答辩模板，把逐页内容蓝图做成**单个自包含、16:9 固定舞台、键盘左右翻页的 HTML 演讲页**，确认无误后导出同版式 PDF。品牌规格已内置，开箱即用。

- 导航入口：[presentation-slides-skills](https://github.com/Valentina-Qiao/presentation-slides-skills)
- 内容拆页先用 [ppt-content-blueprint](https://github.com/Valentina-Qiao/ppt-content-blueprint)；其他品牌模板改用 [pptx-stage-slides](https://github.com/Valentina-Qiao/pptx-stage-slides)

## 它的特长

- **像素级品牌复刻**：主色 `#780000` 深红体系、顶部深红导航条、左侧深红竖栏章节页、右下角红色方块页码、红字重点、微软雅黑/等线 + Georgia 英文，全部沉淀为设计 token 与可复用 class。
- **现成骨架即改即用**：`assets/boilerplate.html` 已内置修复后的章节页显隐规则、舞台等比缩放、翻页脚本与方块页码，不从零写、不重犯老 bug。
- **确定性防溢出**：固定 1280×720 舞台、内容安全高约 580px；复杂图用固定高容器 + `object-fit:contain` 锁高等比，数学上保证不超框。
- **可靠 PDF 工艺**：Playwright 调系统 Chromium 逐页 `show(i)` 截 `#stage`（2 倍清晰度），PyMuPDF 合成 1280×720 PDF，分页天然正确、与屏幕一致。

## 负责解决什么问题

解决“按南院模板手工套版慢、易翻车”的实际问题：章节页/目录多页叠加重影、技术路线图反复溢出舞台、封面校徽裂图、并列卡被错加箭头、内容被擅自删减、浏览器直接打印只导出当前页、LibreOffice 排版走样等。本 Skill 把这些真实返工都写成了避坑纪律与脚本。

## 什么场景适合用

- 明确要求按广州南方学院模板/该 `.pptx` 复刻，做深红学术风答辩或汇报 HTML。
- 已经有逐页蓝图（来自 ppt-content-blueprint），需要忠实视觉化并导出 PDF。
- 要把已做好的同模板 HTML 转成 16:9 PDF。

**不适用**：其他品牌/学校模板请用 C；内容怎么拆页请先用 A，本 Skill 不改文案、不替你梳理逻辑。

## 工作流（7 步）

1. 解压模板确认色板/字体/比例/校徽校名，图片导出本地后上传换不过期 URL。
2. 基于 `assets/boilerplate.html` 搭骨架，不另起炉灶。
3. 按蓝图选页型组件逐页填内容，用语义化 class。
4. 控高与对齐：栅格 + 紧凑组件按安全高度预算，不靠目测缩字号。
5. 自检页码顺移、总页数同步、section 开闭配平。
6. 只交付最终一个 `.html`。
7. 用户确认版式后再运行脚本导 PDF。

## PDF 导出

```bash
pip install pymupdf playwright            # 另需系统 chromium 与中文字体
python3 scripts/export_pdf.py \
  --html 开题答辩.html --out 开题答辩.pdf
# 图床失效/离线渲染时把在线 URL 换本地文件（可重复 --map，不改交付 HTML）
python3 scripts/export_pdf.py --html a.html --out a.pdf \
  --map "https://host/logo.png|/abs/assets/logo.png"
```
禁用浏览器直接打印（其余页 `display:none` 只会出当前页）与 LibreOffice（flex/transform 复刻差）。

## 常见问题与解决方案

- **多个章节页/目录文字叠加重影？** 根因是 `.chapter{display:grid}` 压过了 `.slide{display:none}`。骨架已内置三条修复（`.slide.chapter{display:none}`、`.slide.chapter.active{display:grid}`、`.toc .chapter{display:grid}`），勿删；新增任何特殊 display 的页型都要补其非激活 `display:none`。
- **内容/流程图超出 16:9 舞台？** 不要心算高度或缩字号。按约 580px 安全高做预算并留 ≥30px；复杂技术路线优先用高清原图，容器写死高度 + `object-fit:contain`；超长就拆页。禁止用固定死高裁切文字。
- **封面校徽/图片裂图不显示？** 在线图床 URL 会过期。云电脑一律 `lark-cli drive +html-image-upload --file <path>` 取不过期 `data.url` 写入 `src`，**严禁 FileBatchUpload**；排查先看 src 是否旧链，重传换新；图片只用 `src` 引用。
- **中文乱码？** 先排查是不是“显隐叠加”被误看花屏，再谈编码；文件统一 UTF-8（可带 BOM），保留 `<meta charset>`。
- **并列模块被加了因果箭头？** 同层级维度卡间禁箭头；箭头只用于时间/步骤流程或卡内“风险→对策”，拿不准按并列并确认。
- **空间不够能不能删文案？** 不能。只重组不阉割，用重构布局/收紧间距/拆页解决，删字必须先问；要求回退就原样回退再只改指定点。
- **合并/删页后页码错乱？** 顺移右下角方块页码、同步 counter 的 `1 / N`、检查 section 配平与导航高亮；总页数由 `slides.length` 自动算。
- **PDF 只导出一页或排版错位？** 别用打印/LibreOffice，用 `scripts/export_pdf.py`；中文出方块检查中文字体，裂图用 `--map` 离线替换，画面没截稳调大 `--wait`。

## 目录结构

```
gznf-html-slides/
├─ SKILL.md
├─ assets/boilerplate.html        # 可直接改造的单文件骨架（已修显隐/缩放/翻页/页码）
├─ scripts/export_pdf.py          # 逐页截图合成 16:9 PDF（支持 --map 离线换图）
└─ references/
   ├─ design-tokens.md            # 完整色板/字体/版式/组件与 class 命名
   ├─ pitfalls.md                 # 10 条真实踩坑复盘 + 交付前检查清单
   └─ pdf-export.md               # HTML→PDF 工艺、依赖与故障排查
```

## 边界

单文件自包含、CSS/JS 全内联、原生 JS 无框架；只交付一个最终 `.html`；用户确认 HTML 后才导 PDF；内容以蓝图为准，不擅改。
