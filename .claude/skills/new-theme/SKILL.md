---
name: new-theme
description: Use when the user wants to create their own 微信公众号 style theme — either a fresh design from a brief, or one that inherits/continues their existing 公众号 look (DNA extraction). Triggers: "造个新主题", "我想要自己的风格", "/new-theme", "把我现有号的风格做成模板". Produces a new theme HTML + auto-registers it.
---

# new-theme

为用户生成一套全新的公众号主题 HTML reference，并自动登记到渲染系统（system-prompt + renderer 映射表）。设计工作完全委托给 `theme-designer` subagent，skill 本身只负责意图解析、流程调度、登记写入。

---

## 1. 解析意图（先问清楚，不瞎猜）

从用户消息中提取以下变量：

| 变量 | 说明 |
|---|---|
| `path`（路径） | `fresh`（全新设计）或 `inherited`（继承旧号风格） |
| `name`（主题名） | kebab-case 小写短横线，例 `mono-editorial`、`warm-ink` |
| `family` | `fresh` 或 `inherited`（通常与 `path` 一致） |
| `tone` | `humanist`（感性书页）或 `tech`（理性几何） |

**含糊时主动问，不猜**。典型需要追问的情形：

- 用户只说"造个主题"，没给名字 → 问：「主题名是什么（kebab-case）？基调是感性（humanist）还是理性（tech）？」
- 用户没说 fresh / inherited → 问：「你是想从零设计一个全新风格，还是想继承你现有公众号的视觉风格？」
- 名字含空格或中文 → 提示转换：「主题名需要是 kebab-case 格式，例如 `warm-ink`，你看用哪个？」

---

## 2. fresh 路径：收集 brief

用户选择 fresh 路径时，引导填写 `docs/wechat-render/references/theme-brief-template.md`。

有两种方式，用户选哪种都行：

- **方式 A（对话逐项问）**：按 brief 模板的字段顺序依次提问（参考刊物 → 字体倾向 → 配色情绪 → 装饰语言 → 信息密度），收集完整后整理成 brief 摘要，传给 subagent。
- **方式 B（用户直接给）**：告诉用户「你也可以直接按 `docs/wechat-render/references/theme-brief-template.md` 填好后粘贴给我」，用户给了就直接用。

**「必须覆盖」清单（全套 markdown 元素 + 7 个 primitives）已预填在模板里**，无需再问——直接告诉 subagent「按 design-brief-fresh.md 的清单覆盖」即可。

---

## 3. inherited 路径：收集旧文

用户选择 inherited 路径时：

1. 请用户提供 **1–3 篇旧公众号文章**，形式之一：
   - 本地 HTML 文件路径（例如 `旧文/article1.html`）
   - 直接粘贴 HTML 片段到对话里
2. （可选）请用户提供已填好的 `docs/wechat-render/references/style-dna-extracted.md` 内容作为 DNA 锚点

**如果用户说没有旧文章 HTML**：建议改走 fresh 路径，说明「inherited 路径需要你现有文章的 HTML 作为视觉锚点，没有的话 fresh 路径会更顺畅——你只需要描述你想要的感觉」。不要强行继续。

---

## 4. dispatch theme-designer subagent

收齐输入后，dispatch `theme-designer` subagent（Agent 工具，subagent_type=`theme-designer`）。

**prompt 模板（fresh 路径）**：

```
path: fresh
name: <name>
family: fresh
tone: <humanist|tech>
output_file: docs/wechat-render/references/themes/<name>.html

Brief 摘要：
<从对话收集的 brief 各字段，或用户粘贴的完整 brief>

设计时参考 docs/wechat-render/references/design-brief-fresh.md 的业界范式和 primitives 清单。
覆盖全套 markdown 元素 + 7 个 primitives（详见 brief）。
返回 Quick-Reference 摘要 + 写入确认。不要输出完整 HTML。
```

**prompt 模板（inherited 路径）**：

```
path: inherited
name: <name>
family: inherited
tone: <humanist|tech>
output_file: docs/wechat-render/references/themes/<name>.html

旧文输入：
<文件路径列表，或"用户粘贴的 HTML 见下方：<内容>">

<如有 style-dna：「用户提供的 DNA 摘要：<内容>」>

设计时按 docs/wechat-render/references/style-dna-extracted.md 的方法提取 DNA，
再参考 docs/wechat-render/references/design-brief-inherited.md 精化。
覆盖全套 markdown 元素 + 7 个 primitives（详见 design-brief-inherited.md）。
返回 Quick-Reference 摘要 + 写入确认。不要输出完整 HTML。
```

**等待 subagent 返回**，获取：

- Quick-Reference 摘要（多行 bullet 块，格式见 theme-designer）
- 写入确认（文件路径 + 行数）

---

## 5. 自动登记（3 处确定性插入，用锚点）

subagent 返回后，skill 在**主会话**用 Edit 工具完成 3 处插入。锚点已内置在目标文件中，不要破坏。

### 5.1 system-prompt：主题表新行

**文件**：`docs/wechat-render/system-prompt.md`

**锚点**：`<!-- THEME-TABLE-END: the new-theme skill inserts new theme rows immediately ABOVE this line -->`

**插入方式**：把锚点行替换成「新行 + 换行 + 锚点行」：

```
| `<name>` | <family> | <tone> | <一句话适用场景，例 "工程博客 / 理性数据复盘类长文"> |
<!-- THEME-TABLE-END: the new-theme skill inserts new theme rows immediately ABOVE this line -->
```

适用场景从 Quick-Reference 摘要的最后一条 bullet 提炼（或自行概括，保持与表格其他行风格一致）。

### 5.2 system-prompt：Quick-Reference 块

**文件**：`docs/wechat-render/system-prompt.md`

**锚点**：`<!-- THEME-QUICKREF-END: the new-theme skill inserts new theme Quick-Reference blocks immediately ABOVE this line -->`

**插入方式**：把锚点行替换成「Quick-Reference 块 + 空行 + 锚点行」：

```
**<name>** (ref: `<name>.html`)
- Main font: ...
- ...（subagent 返回的完整 bullet 列表）

<!-- THEME-QUICKREF-END: the new-theme skill inserts new theme Quick-Reference blocks immediately ABOVE this line -->
```

注意：Quick-Reference 块末尾到锚点之间**必须保留一个空行**（如模板所示），以维持各主题块之间的视觉间隔；而 5.1 的表格行和 5.3 的映射行插入时**不加空行**（直接「新行 + 换行 + 锚点行」），这三处规则不同，不要用同一模板套用。

### 5.3 wechat-renderer 映射表新行

**文件**：`.claude/agents/wechat-renderer.md`

**锚点**：`<!-- THEME-MAP-END: the new-theme skill inserts new theme→file rows immediately ABOVE this line -->`

**插入方式**：把锚点行替换成「新行 + 换行 + 锚点行」：

```
| <name> | `<name>.html` |
<!-- THEME-MAP-END: the new-theme skill inserts new theme→file rows immediately ABOVE this line -->
```

**插入原则**：

- 每次用 Edit 工具，将锚点行作为 `old_string`，替换为「新内容行 + `\n` + 锚点行」
- **锚点行必须完整保留**（下次再登记新主题时还要用）
- **既有行不得破坏**（不要替换整个表格，只插单行）
- 3 处插入可以连续执行，无依赖顺序

---

## 6. 收尾提示

3 处登记完成后，告诉用户：

```
已登记主题 <name>。

新主题已写入 docs/wechat-render/references/themes/<name>.html，
并登记到 system-prompt（主题表 + Quick Reference）和 wechat-renderer 映射表。

用它渲染一篇示例文章试试效果：
  『用 <name> 渲染 examples/sample-article/article.md』
```

---

## 不要做的事

- **不要在主会话自己设计 HTML** — 设计永远委托 `theme-designer` subagent
- **不要手动修改既有 theme 文件的内容** — 你只负责新主题的登记
- **插入登记行必须严格用锚点（`<!-- THEME-TABLE-END -->` / `<!-- THEME-QUICKREF-END -->` / `<!-- THEME-MAP-END -->`），不破坏既有行** — 不要替换整个表格或整段文字
- **不要在 subagent 返回前就开始插入登记** — 必须等拿到 Quick-Reference 摘要才知道该填什么
- **不要在 FAIL / 含糊时继续流程** — 先确认意图和输入，再调度 subagent
