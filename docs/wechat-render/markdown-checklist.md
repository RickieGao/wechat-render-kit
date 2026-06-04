# Markdown 终稿 Checklist

> 在把 markdown 喂给 X 路 Claude.ai Project 之前，按本清单过一遍。目标：让最终稿包含所有应该出现的模块，避免输出 HTML 后才发现缺东西。

## A. Frontmatter 完整性

终稿应该有完整 frontmatter（即使部分字段为空）：

```yaml
---
title: 总结2025-财务篇（理财部分）        # 必填
theme: inherited-tech                    # 必填（4 选 1，见 §C）
signature: true                          # 可省，默认 true
sources:                                 # 可选（见 §B-2）
  - 《2001 太空漫游》｜阿瑟·克拉克
  - 与 R 的一次餐桌讨论, 2026.02
sources_note: "*注：这些不是正式的参考文献"
ai_disclosure:                           # 可选（见 §B-1）
  model: Claude Opus 4.7
  prompts:
    - "把这份理财日志按季度聚合，画 KPI 趋势图"
---
```

### 字段说明

| 字段 | 必填 | 触发 | 备注 |
|---|---|---|---|
| `title` | ✅ | 仅用于本地归档（公众号编辑器另填） | |
| `theme` | ✅ | 见 §C 选型 | 也可在 Claude 对话里口头指定 |
| `signature` | 否（默认 true） | true → 文末渲染右对齐签名灰线 | 不需要签名时设 `false` |
| `sources` | 否 | 文末渲染"想法出处"块 | 数组，每项一条出处 |
| `sources_note` | 否 | sources 块的尾注小字 | 默认 `*注：` 谦逊语气 |
| `ai_disclosure` | 否 | 文末渲染 AI 协作声明卡 | 见 §B-1 触发条件 |

## B. 必备模块检查

### B-1. AI 创作声明（如果用了 AI）

**触发条件**：本篇文章在写作过程中**实质性**使用了 AI（不限于 Claude）协助 — 如改写、润色、数据分析、图表生成、翻译、配图选择等。

仅"AI 帮我查个资料"不算实质性使用，可不声明。

如果触发，frontmatter 加：

```yaml
ai_disclosure:
  model: Claude Opus 4.7                    # 实际用的模型
  prompts:                                  # 关键 prompt（不要全贴）
    - "把这份理财日志按季度聚合，画 KPI 趋势图"
    - "帮我润色这段，保持原意但更冷静一点"
```

**自检**：本篇有没有任何段落或图表是 AI 生成的？如果有，必须声明。

### B-2. 想法出处（可选但鼓励）

**触发条件**：文章主要观点 / 关键论据有**具体来源**（书、文章、对话、个人经验、特定数据集）。

不是正式参考文献，谦逊语气，承认借鉴：

```yaml
sources:
  - "《2025 投资十论》| Charlie Munger"
  - "与 R 的一次餐桌讨论, 2026.02"
  - "Notion 数据库：公众号文章 / 财务篇下"
sources_note: "*注：这些不是正式参考文献，是这一周读到 / 听到 / 想到的几个出处。"
```

**自检**：
- 文中"我读到 / 我听说 / 据说" 之类句式有没有具体来源？
- 有引用书 / 文章 / 数据时来源是否准确？
- 不需要严格学术格式，但读者可以追溯

### B-3. 签名

默认 `signature: true` 即在文末渲染。除非：
- 文章是某系列的中间章，不需要每节都签名（→ `signature: false`）
- 临时草稿测试

**自检**：本篇是独立完整文，还是系列的一部分？

## C. Theme 选型

每篇文章在终稿前选定 1 个 theme。**犹豫时按"内容主导"原则选**：

| 内容类型 | 推荐 theme | 关键特征 |
|---|---|---|
| 数据复盘 / 财务总结 / KPI 复盘 / 投资分析 / 健康数据 | `inherited-tech` | 招牌蓝 + 暖陶人文加入 + mono 编号 H2 + 可选数据卡 |
| 散文 / 回忆录 / 感性反思 / 文化引用 / 季节性写作 | `inherited-humanist` | 全衬线 + 罗马数字 H2 + asterism hr + 首字下沉 |
| 工程师博客风 / 想完全跳出原风格 / Substack 感 | `fresh-tech` | Tailwind 色板 + 重 mono + 暗底代码 + amber 高亮 |
| 想完全跳出原风格 + 散文气质 / 文学感 | `fresh-humanist` | 暖陶主色（无招牌蓝）+ 暖米底 + 大引号 blockquote |

**主题选错的修复成本很低** — 在 Claude.ai 对话里改一句"换 inherited-humanist 重做"就行。但默认建议**先选 inherited-tech**（你的主力场景）。

## D. 内容自检

按这个清单过一遍正文：

### D-1. 结构

- [ ] 至少有 2-4 个 `## H2` 小节（一篇 3000 字以上文章通常需要）
- [ ] H2 标题足够明确 — 读者扫一遍 H2 能大致 grasp 文章脉络
- [ ] 段落不会过长（>10 行的段落考虑拆分或加 H3）
- [ ] 没有 placeholder（如 `xxx` / `TODO` / `待补`）

### D-2. 装饰元素（可选）

如果适用，markdown 里主动加这些 X 路特殊语法：

- [ ] **旁注块（aside）**：作者插话 / 反思 / 自嘲，用 `> [!aside]` 触发暖米斜体段
  ```markdown
  > [!aside]
  > 说实话，"非线性变化"是数据上的说法。实际感受是身体在 9 月那次感冒之后……
  ```
- [ ] **段首 emoji**（inherited 主题习惯）：每段开头放一个 emoji 作视觉锚点
- [ ] **图片 caption**：用 markdown title 语法 `![alt](url "画名 · 作者 · 年份")` — X 路会渲染为正式 caption

### D-3. 数据展示（仅 tech 主题文章）

如果文章包含数据 / KPI，让 X 路 LLM 选择是用普通表格还是富装饰元素：
- 简单二维数据 → 标准 markdown 表格即可，X 路会按 theme 样式渲染
- 关键 KPI（3-4 个核心指标）→ markdown 里写出来就行，X 路 LLM 会判断要不要做成数据卡片
- 时间序列趋势 → 写 markdown 表格 + 注释说"如果数据点少建议改 SVG 趋势图"，X 路会判断

不要自己手画 SVG / 数据卡 — 让 X 路 LLM 决定形态。

### D-4. 链接

- [ ] 所有 `[文字](url)` 链接的 url 是真实可访问的（不是 `#` 或 `xxx`）
- [ ] 链接太多时考虑改为文末"想法出处"块（`sources`）

### D-5. 图片

- [ ] 所有 `![](url)` 的 url 是真实的图片地址或本地路径（公众号编辑器会要求重新上传到素材库）
- [ ] 文章 cover 图准备好（公众号编辑器另填，不在 markdown 里）
- [ ] **图尚未就位时**：用 image placeholder 语法 `![<alt>](待补图 "<title>")` 占位，subagent 会渲染为 broken-image + caption 保留位置。**不要用纯文字描述**如"项目架构图：（待补，可见 _process/research/...）"— subagent 会把这种行当作元数据 drop 掉（2026-05-27 教训，下篇首次渲染丢了 6 处占位）

## E. 终稿前最后一道

- [ ] frontmatter YAML 语法有效（特别注意 `:` 后空格、字符串引号、缩进）
- [ ] markdown 文件保存为 UTF-8（避免中文乱码）
- [ ] 删除调试/临时段落（如"@@待补"、"FIXME 这里"）
- [ ] 全篇通读一遍，确保无前后矛盾 / 表达不清
- [ ] **中文段落标点全角化**：`,` → `，` / `;` → `；` / `?` → `？` / `!` → `！` / `:` → `：` / `.` → `。`（**英文术语 / 文件名 / 模型名 / URL / 版本号 / 代码 周围的 ASCII 标点保留**，如 `Opus 4.7` / `daily_run.py` / `(FastAPI · 本地进程)`）。subagent 现在会自动转（system-prompt §Vocabulary Conventions 已落规则），但 source-of-truth 的 article.md 自己写时也应该用中文标点，避免依赖渲染时转换

## F. 推到 X 路

完成上面所有 checklist 后：

1. 打开 Claude.ai Project `wechat-style-apply-X`
2. 新对话粘 markdown 全文
3. 一句话指定 theme：例如"用 inherited-tech 渲染"
4. 拿 HTML artifact 进浏览器预览
5. 不满意就让 Claude 改（"H2 用罗马数字不要 mono"、"数据卡不要"等）
6. 满意后按 [SOP Step 9-10](../../00_SOP/公众号写作发布SOP.md) 推草稿箱

---

> 这个 checklist 应该随着实际写作经验迭代。发现常被遗漏的项 → 加进来；发现某项很少触发 → 移到附录或删除。
