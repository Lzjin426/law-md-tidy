# law-md-tidy

按内容和阅读目的设计、整理笔记与文档的结构和排版。先理解文档，再决定标题、列表、表格、提示块和留白；只改结构和写法，不改内容事实。

## 支持的目标

- 通用 Markdown / Obsidian
- 飞书云文档（原生编号、分隔线、空段落、高亮块）
- Fullfold.md（quiet.md）：普通 GFM 直接写；提示块、分栏、自动编号标题、颜色按 `references/fullfold.md` 的 HTML 模板写

## 结构

```
SKILL.md                    工作流、路由、版本探针
VERSION                     当前版本（0.1）
references/reading-design.md  内容与样式的判断和交付前检查
references/markdown.md      Markdown 写作与迁移规范
references/feishu.md        飞书排版约定
references/fullfold.md      Fullfold.md 平台差异
references/independent-review.md  独立只读复核
scripts/check_md_structure.py     Markdown 结构扫描
scripts/check_update.py     远端版本探针（只读）
```

## 安装

把本目录放到 skills 目录并软链到 agent 的技能目录，例如：

```sh
ln -s /path/to/law-md-tidy ~/.skillsbase/skills/law-md-tidy
```

## 版本与发布

- 当前版本写在 `VERSION`。
- 每次发布：修改内容 → 把 `VERSION` 加一（如 `0.1` → `0.2`）→ 提交 → 打对应 tag（如 `v0.2`）→ 推送。
- 使用本技能时，agent 会运行 `python3 scripts/check_update.py` 对比远端 `VERSION`；发现新版本只提示，不自动更新。离线或超时静默跳过。

## 许可

个人使用方法与约定，按现状提供。
