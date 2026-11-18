# Fullfold.md（quiet.md）文档排版

目标应用是 Fullfold（原名 quiet.md），本地 Markdown 知识库应用。它的 `.md` 文件既要在应用里正常显示，也要在普通 Markdown 阅读器里可读。本文件只写平台差异；通用判断按 [内容与样式](reading-design.md)，通用写法按 [原 Markdown 能力](markdown.md)。

## 文件与标题

- 每篇文档是一个普通 `.md` 文件；YAML frontmatter 和未知语法原样保留，不为排版重写未改动区域。
- **文档标题取自文件名**，正文不再重复写一个同名 H1。正文可以直接从一级章节开始，标题层级仍逐级下探，不跳级。
- 这些标准 GFM 写法直接用普通 Markdown，不要包 HTML：段落、引用（`>`）、无序/有序/任务列表（可嵌套）、GFM 表格（不支持合并单元格）、围栏代码、分隔线、图片、链接、粗体/斜体/删除线/行内代码、行内公式 `$...$`、行间公式 `$$`、文档互链 `[[笔记名]]`。
- 不要写 Obsidian 提示块 `> [!note]`：应用只当普通引用显示，不会变成提示块。

## 提示块（callout）

提示块、分栏、自动编号标题、彩色文字在文件里存成带 `data-quiet-` 属性的 HTML 块（版本 1）。应用按严格结构回读：属性拼错、层级简化或自创属性，整块会退化成纯文本，不再渲染。**需要这些样式时整套照抄下面的模板，只改文字和颜色名；每块写在一行内，不要换行缩进。**

```html
<div data-quiet-version="1" data-quiet-block="callout" data-quiet-align="left" style="text-align:left" data-quiet-emoji="💡" data-quiet-callout-bg="light-blue" data-quiet-border="blue"><div data-quiet-children="1"><div data-quiet-version="1" data-quiet-block="paragraph" data-quiet-align="left" style="text-align:left"><p data-quiet-text="1" style="white-space:pre-wrap"><span><strong>说明：</strong></span><span>正文。</span></p></div></div></div>
```

- `data-quiet-emoji` 必填；正文放在 `data-quiet-children` 里的子块中，可以有多个子块，每个子块同样从模板复制。
- 颜色成对使用：背景 `light-blue` 配边框 `blue`，还有 `light-green`/`green`、`light-yellow`/`yellow`、`light-red`/`red`、`light-gray`/`gray`、`light-orange`/`orange`、`light-purple`/`purple`。背景也可用更深的 `medium-*`。
- 语义按阅读设计：蓝=说明，绿=推荐/已确认，黄=注意/待确认，红=风险/禁止，灰=中性补充。每篇少量使用，块内用“说明：”“注意：”等文字写明含义，颜色不作为唯一信息。

## 自动编号标题

普通 Markdown 标题承载不了自动编号，整篇二选一，不要混用：

1. **默认（可移植）**：在标题文字里手打编号，如 `## 1. 标题`、`### 1.1 小节`。任何 Markdown 工具都能读，但增删章节后要自己改号。
2. **自动编号（只在本应用里读）**：用 HTML 标题块，增删章节后应用自动顺延：

```html
<div data-quiet-version="1" data-quiet-block="heading" data-quiet-align="left" style="text-align:left" data-quiet-level="2" data-quiet-auto-number="true"><h2 data-quiet-text="1" style="white-space:pre-wrap"><span>标题</span></h2></div>
```

编号规则：同一容器（页面、提示块、分栏、表格单元格）单独计数；按实际用到的标题级别依次编号，最外层显示 `1.`，下一级显示 `1.1`。把模板中的 `2`/`h2` 换成需要的级别。

## 分栏

只用于并排阅读更清楚的短内容；各栏 `data-quiet-ratio` 之和为 1（如 0.5/0.5、0.3/0.7）。手机窄屏效果未验证时不要声称已确认。

```html
<div data-quiet-version="1" data-quiet-block="columns" data-quiet-align="left" style="text-align:left"><div data-quiet-children="1"><div data-quiet-version="1" data-quiet-block="column" data-quiet-align="left" style="text-align:left" data-quiet-ratio="0.5"><div data-quiet-children="1"><div data-quiet-version="1" data-quiet-block="paragraph" data-quiet-align="left" style="text-align:left"><p data-quiet-text="1" style="white-space:pre-wrap"><span>左栏。</span></p></div></div></div><div data-quiet-version="1" data-quiet-block="column" data-quiet-align="left" style="text-align:left" data-quiet-ratio="0.5"><div data-quiet-children="1"><div data-quiet-version="1" data-quiet-block="paragraph" data-quiet-align="left" style="text-align:left"><p data-quiet-text="1" style="white-space:pre-wrap"><span>右栏。</span></p></div></div></div></div></div>
```

## 文字颜色与底色

**普通 Markdown 段落里的行内 HTML 会被忽略**，彩色文字必须把整段写成 HTML 段落块。短词句才上色，不整段染色，旁边保留文字含义。

```html
<div data-quiet-version="1" data-quiet-block="paragraph" data-quiet-align="left" style="text-align:left"><p data-quiet-text="1" style="white-space:pre-wrap"><span data-quiet-color="red">红色短语</span><span>普通文字</span><span data-quiet-bg="light-yellow">黄色底色</span><span data-quiet-color="green" data-quiet-bg="light-green">绿字绿底</span></p></div>
```

- 文字色：`data-quiet-color`；底色：`data-quiet-bg`；可叠加。
- 颜色名：文字用 `gray`、`red`、`orange`、`yellow`、`green`、`blue`、`purple`；底色用 `light-*` 或 `medium-*`。
- 段落里的每个文字片段各自包一个 `<span>`，不留裸文字。

## 不要手写的部分

合并单元格、表格列头、单元格底色、附件卡片、按钮、倒计时、子页面列表等结构更复杂，默认在应用里插入。确需手写时，从应用真实生成的文件里整块复制再改文字，不凭印象拼属性。

## 写作习惯

- 长文默认使用编号（按上节二选一）；短文可以不编号。
- 步骤有先后用有序列表，并列用无序列表，任务用任务列表；术语和并列要点不要挤成一整段。
- 强调克制：关键结论、容易误解的前提、操作风险才用提示块或加粗。
- 图片和附件沿用文档已有的相对路径约定，不擅自搬动或改写已有资源路径。

## 校验

- 结构自查：`python3 scripts/check_md_structure.py <文件>`。
- 无法在应用里打开回读时，明确说明“未验证渲染效果”；不要仅凭写入成功宣布样式已生效。
