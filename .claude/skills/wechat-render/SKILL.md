---
name: wechat-render
description: Use when the user wants to render a markdown article into 微信公众号 inline-styled HTML, with explicit theme choice (classic-humanist/classic-tech/modern-humanist/modern-tech). Triggers on phrases like "渲染 article.md", "用 classic-tech 转微信", "把这篇出 HTML". Dispatches markdown-checklist-validator (strict pre-check) + wechat-renderer subagent to keep the main session context clean.
---

# wechat-render

把 markdown 文章渲染为公众号可用的 inline-styled HTML。**渲染前先严格校验 markdown-checklist, 校验失败阻塞渲染**。校验和渲染都委托给 subagent, 主会话只负责解析意图 + 调度 + 把结果转告用户。

## 触发流程

0. **判断输入源 (front door, 源无关)**: 核心只渲染 markdown, 但用户的"原始文本"未必是 markdown。
   - 已是本地 `.md` 文件 → 直接进下一步
   - 在 Notion / 飞书 / Word / 一段富文本粘贴里 → **先归一化成本地 markdown**: 用你手上的工具 (Notion MCP / 导出 / 直接转写) 把正文转成一个 `.md` 文件存到合适目录, 再继续。**不要**内置任何特定源的拉取逻辑——用你自己 agent 已有的能力。

1. **解析用户意图**, 提取 3 个变量:
   - `markdown_path` — 待渲染的 .md 文件绝对路径
   - `theme` — 已登记的主题之一 (自带 classic-{humanist,tech} / modern-{humanist,tech}; 加上用户用 new-theme 造的任意主题。登记主题 = `docs/wechat-render/references/themes/` 下存在的 `<theme>.html`)
   - `mode` — `initial` (首轮渲染) or `feedback` (基于已有 HTML 的迭代)

2. **theme 推断顺序**:
   a. 用户对话里明说 → 用这个
   b. markdown frontmatter 有 `theme:` → 用这个
   c. 都没有 → 列 4 个 theme 让用户选, 不要默认

3. **mode 判断** (从用户对话语义判断, 不查文件 mtime):
   - 没有同目录 `<stem>-formatted.html` → `initial`
   - 用户说 "重新渲染" / 换 theme / 语义上是整篇重做 → `initial` (覆盖已有 HTML)
   - 有 `<stem>-formatted.html` + 用户说 "改/调/迭代/不对/再试试" 等局部反馈词 → `feedback`
   - 含糊时主动问用户 "initial 重渲 还是 feedback 改局部?", 不要瞎猜

4. **Initial 模式: Step 0 严格校验** (feedback 模式跳过, 因 HTML 已存在校验意义不大):
   - dispatch `markdown-checklist-validator` subagent (Agent 工具, subagent_type=`markdown-checklist-validator`)
   - prompt: `"Validate <markdown_path> against markdown-checklist.md. Strict mode."`
   - 等待返回报告
   - **校验通过 (PASS)**: 进入 Step 5 渲染
   - **校验失败 (FAIL)**: 不渲染, 把 issue 清单原样返回用户, 请用户修复后重试。**不要自动修** — 让作者决定怎么改, 否则隐藏 source-of-truth 问题

5. **dispatch wechat-renderer subagent** (Agent 工具, subagent_type=`wechat-renderer`):
   - initial 模式 prompt: `"Render <markdown_path> with theme <theme>. Write to <output_path>."`
   - feedback 模式 prompt: `"Apply this feedback to existing HTML at <output_path>: <feedback 原文>. Re-write the file."`

6. **dispatch 完后**告诉用户:
   - 输出文件路径
   - 一句话提示 "浏览器双击打开 HTML 预览; 满意后全选复制 (Ctrl+A / Ctrl+C), 粘到公众号草稿箱正文区即可。这一步是手动的——agent 无法替你粘贴。"
   - 不在主会话里读 / 展示 HTML 内容 (太长污染上下文)

## output_path 计算

```
output_path = <markdown_path 所在目录>/<markdown 文件名去掉 .md 后缀>-formatted.html
```

例: `02_文章/2026-05_断更两月-上/article.md`
    → `02_文章/2026-05_断更两月-上/article-formatted.html`

`*-formatted.html` 已在 `.gitignore`, 不会误入 git。

## 不要做的事

- 不要在主会话里自己渲染 HTML — 永远委托 `wechat-renderer` subagent
- 不要在主会话里自己跑 markdown-checklist 校验 — 永远委托 `markdown-checklist-validator` subagent
- **不要在 validator FAIL 时自动修 article.md** — issue 清单返回用户, 让作者决定怎么改
- 不要主动展示 HTML 文件内容 — 用户要看就让他自己打开
- 不要修改 `docs/wechat-render/system-prompt.md` 或 references — 那是 source of truth
- 不要尝试自动发布到公众号 — 职责边界是 markdown→HTML, 发布由用户手动完成
