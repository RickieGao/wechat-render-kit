# 生成新主题

本仓库内置 4 个主题 (`classic-humanist` / `classic-tech` / `modern-humanist` / `modern-tech`)。你可以按需新增主题——本文说明两条路径以及如何把新主题登记到渲染系统。

---

## 1. 意图: 两条路径

| 路径 | 何时选 | 输入 |
|---|---|---|
| **modern** (全新设计) | 想要与现有主题完全不同的视觉风格 | 填写 `theme-brief-template.md` |
| **classic** (继承旧号 DNA) | 想把自己公众号现有排版风格复现为可复用主题 | 提供 1–3 篇旧文 HTML + (可选) DNA 表 |

两条路径都以 `docs/wechat-render/references/themes/<name>.html` 为产出, 并需要登记到 3 处配置文件。

---

## 2. 用 skill (推荐): `/new-theme`

在 Claude Code 会话里说 `/new-theme`, skill 会引导你完成全流程。

### 步骤概览

**Step 1 — 说出路径和主题名**

```
/new-theme warm-ink modern humanist
```

含糊时 skill 会主动追问: 路径 (modern / classic)、名字 (kebab-case)、基调 (humanist / tech)。

**Step 2 — 给输入**

- **modern 路径**: skill 引导你逐项回答 brief (参考刊物 / 字体倾向 / 配色情绪 / 装饰语言 / 信息密度), 或直接粘贴填好的 `docs/wechat-render/references/theme-brief-template.md`。
- **classic 路径**: 提供 1–3 篇旧公众号文章 HTML (本地路径或直接粘贴), 也可附上按 `docs/wechat-render/references/style-dna-extracted.md` 提取好的 DNA 表。

**Step 3 — theme-designer 产 HTML**

skill 调度 `theme-designer` subagent 设计覆盖全元素 + 7 个 primitives 的主题 HTML, 写入 `docs/wechat-render/references/themes/<name>.html`。

**Step 4 — 自动登记**

skill 在主会话自动完成 3 处登记 (详见第 3 节)。

**Step 5 — 用新主题渲染示例预览**

```
用 <name> 渲染 examples/sample-article/article.md
```

示例文章覆盖全套 markdown 元素和所有 primitives, 是验证新主题效果的最快方法。

---

## 3. 全手动配方 (非 Claude Code / 其他 agent)

不使用 Claude Code skill 时, 按以下步骤手动完成。

### Step A — 准备设计输入

**modern 路径**: 按 `docs/wechat-render/references/theme-brief-template.md` 填写一份完整的主题设计 brief。字段包括: 主题名 (kebab-case)、基调 tone、参考刊物、字体倾向、配色情绪、装饰语言、信息密度。

**classic 路径**: 准备好 1–3 篇旧公众号文章 HTML (完整 inline-styled 版), 并按 `docs/wechat-render/references/style-dna-extracted.md` 提取 4 个维度的 DNA:
- 基础排版 (字号 / 行高 / 字距 / 对齐 / 边距)
- 招牌色 (主强调色 / 次要文字色 / 分割线 / 卡片底色)
- 标志性组件 (H2 样式 / 引用块 / 签名形式 / AI 声明卡 / 出处块)
- 段落习惯 (段首 emoji / 首字下沉 / 列表 marker / 章节编号)

### Step B — 产出主题 HTML

把以下材料一起喂给你的 AI:
1. 你在 Step A 准备的 brief (modern) 或 DNA + 旧文 (classic)
2. `docs/wechat-render/references/wechat-html-constraints.md` (公众号 HTML 硬约束)
3. 对应的 design brief 详尽版:
   - modern 路径: `docs/wechat-render/references/design-brief-modern.md`
   - classic 路径: `docs/wechat-render/references/design-brief-classic.md`

要求 AI 输出一段完整的 inline-styled HTML, 覆盖:
- 全套 markdown 元素: `<h1>~<h3>` `<p>` `<strong>` `<em>` `<a>` `<ul>` `<ol>` `<li>` `<blockquote>` `<table>` `<code>` `<pre>` `<hr>` `<img>`
- 7 个结构性 primitives: `section-title` / `inline-emoji` / `image-caption` / `signature` / `ai-disclosure` / `quote-block` / `sources`

把产出的 HTML 存到 `docs/wechat-render/references/themes/<name>.html`。

### Step C — 手动登记 (3 处)

**登记 1: `docs/wechat-render/system-prompt.md` 主题表**

在文件中找到以下锚点行:

```
<!-- THEME-TABLE-END: the new-theme skill inserts new theme rows immediately ABOVE this line -->
```

在锚点行**正上方**插入一行:

```
| `<name>` | <family> | <tone> | <一句话适用场景> |
```

示例 (假设新主题叫 `warm-ink`, modern 家族, humanist 基调):

```
| `warm-ink` | modern | humanist | 书信体散文 / 温暖叙事类长文 |
<!-- THEME-TABLE-END: the new-theme skill inserts new theme rows immediately ABOVE this line -->
```

---

**登记 2: `docs/wechat-render/system-prompt.md` Quick Reference**

在同一文件中找到以下锚点行:

```
<!-- THEME-QUICKREF-END: the new-theme skill inserts new theme Quick-Reference blocks immediately ABOVE this line -->
```

在锚点行**正上方**插入一个完整的 Quick-Reference 块 (空行分隔):

```
**<name>** (ref: `<name>.html`)
- Main font: <字体描述>
- <其他视觉特征 bullet, 参照现有 4 个主题的格式>
- <适用场景描述>

<!-- THEME-QUICKREF-END: the new-theme skill inserts new theme Quick-Reference blocks immediately ABOVE this line -->
```

示例:

```
**warm-ink** (ref: `warm-ink.html`)
- Main font: 衬线为主 (Source Han Serif SC + Georgia)
- 暖米白页面背景 rgb(252, 248, 242)
- H2 以手写风格短横线装饰
- 大号引号 "  " 作为 blockquote 背景装饰
- 书信体散文 / 温暖叙事类长文

<!-- THEME-QUICKREF-END: the new-theme skill inserts new theme Quick-Reference blocks immediately ABOVE this line -->
```

---

**登记 3: `.claude/agents/wechat-renderer.md` 映射表**

在文件中找到以下锚点行:

```
<!-- THEME-MAP-END: the new-theme skill inserts new theme→file rows immediately ABOVE this line -->
```

在锚点行**正上方**插入一行:

```
| <name> | `<name>.html` |
```

示例:

```
| warm-ink | `warm-ink.html` |
<!-- THEME-MAP-END: the new-theme skill inserts new theme→file rows immediately ABOVE this line -->
```

---

**登记原则**:
- 3 处插入顺序无关, 可以任意顺序完成
- 锚点行本身必须**完整保留** (下次再登记新主题时还需要它)
- 只插新行, **不修改**已有主题行

### Step D — 检查

渲染示例文章验证新主题覆盖全元素:

```
用 <name> 渲染 examples/sample-article/article.md
```

逐一确认渲染输出中出现: H1/H2/H3 标题、普通段落、粗体、斜体、链接、无序列表、表格、引用块、aside 块、图片 + 图注、分割线、代码、signature 块、sources 块、AI disclosure 块。

---

## 4. Worked example: classic 家族 DNA 路径

本仓库的 `classic-*` 两个主题是按 classic 路径设计的示例, 说明「DNA 路径长什么样」。

**输入**: 作者旧公众号文章的 inline-styled HTML (共 3 篇), 提取到的风格 DNA 包括:

| DNA 维度 | 提取值 (示例) |
|---|---|
| 招牌蓝 | `#0052FF` (粗标题 / H2 mono 前缀 / strong 强调色) |
| 正文字体 | PingFang SC 无衬线系统栈 |
| H2 装饰 | mono 数字前缀 `01 /` `02 /` |
| 签名形式 | 文末右对齐 + `■` 实心方块 Unicode 装饰 |
| 出处块底色 | `rgb(245, 245, 245)` 浅灰 |

**产出路径**: 两个主题按 humanist / tech 基调分开打磨, 分别存为 `classic-humanist.html` 和 `classic-tech.html`。

**查看**: 打开 `docs/wechat-render/references/themes/classic-tech.html` 可以看到完整的 HTML 样本; 对应的 DNA 提取方法见 `docs/wechat-render/references/style-dna-extracted.md` 的 Worked example 节。

---

## 5. 检查清单

新主题完成后, 确认以下几点:

- [ ] `docs/wechat-render/references/themes/<name>.html` 文件已存在
- [ ] `docs/wechat-render/system-prompt.md` 主题表已新增一行
- [ ] `docs/wechat-render/system-prompt.md` Quick Reference 已新增一个块
- [ ] `.claude/agents/wechat-renderer.md` 映射表已新增一行
- [ ] 用新主题渲染 `examples/sample-article/article.md` 输出正常, 覆盖全元素
