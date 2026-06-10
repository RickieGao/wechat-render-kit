# Design Brief — modern 家族（Claude Design 输入 B）

> 把以下整段喂给你的 AI / 设计工具。本家族是全新设计，不需要参考你的旧排版。

---

## 共通的项目背景（两份 brief 都用）

**项目**：个人公众号文章排版设计系统

**作者**：<你>（填你的背景）

**定位**：<你的公众号定位 —— 例如「科技 + 人文」交汇点，按你自己的号填>。如果你的号也想同时承载两种气质，可以参考如下拆分：

- 数据复盘 / 理性分析 / 年度总结类长文 → **偏科技主题（tech）**
- 散文 / 回忆录 / 感性表达 → **偏人文主题（humanist）**

**通用技术约束（公众号渲染限制，两份 brief 都适用）**：

- ⛔ 所有样式必须 **inline**（写在 `style="..."` 里），**不能用 `<style>` 标签或 `class`**
- ⛔ **不能用 webfont**（不能引 Google Fonts、阿里云字体）——只能用系统字体栈
- ⛔ 不能用 `:hover` 等伪类
- ⛔ 不能用 CSS 变量 `var(--)`
- ⚠️ `flex` 简单布局 OK，复杂嵌套不稳；`grid` 不可靠
- ✅ `<section>` 嵌套常用且稳定（公众号编辑器原生输出就是这种）
- ✅ 内联 SVG OK
- ✅ 颜色用 `rgb()` 或 `#hex`

**通用产出格式（两份 brief 都按此格式产出）**：

为每个主题，输出**一段完整的 inline-styled HTML**，覆盖：

```
<section style="...">                  <!-- 根容器（line-height, color, font, padding） -->
  <h2 style="...">小节标题样例</h2>
  <p style="...">普通段落 <strong>含加粗</strong> 含 <em>斜体</em></p>
  <blockquote style="...">引用块样例</blockquote>
  <ul><li style="...">列表项</li></ul>
  <table style="..."><thead>...</thead><tbody>...</tbody></table>
  <!-- 7 个结构性 primitives 样例（见 brief 中清单）-->
</section>
```

**通用 primitives 清单（每个主题都需要给出 7 个 primitive 的对应版本）**：

1. `section-title` — 文章二级章节标题（H2）。视觉骨架，反复出现
2. `inline-emoji` — 段首装饰图标位置（emoji 也可改为线性图标）
3. `image-caption` — 配图说明，包含画名 · 作者 · 年份三段信息
4. `signature` — 文末个人签名/印章，建立公众号身份识别
5. `ai-disclosure` — 透明披露 AI 工具 + Prompt，存在感低
6. `quote-block` — 引用块
7. `sources` — 文末的「想法出处」块（**非正式参考文献**，谦逊语气）

**通用 markdown 元素清单**：`<p>` `<h1>~<h3>` `<strong>` `<em>` `<a>` `<ul>` `<ol>` `<li>` `<blockquote>` `<table>` 全套 `<code>` `<pre>` `<hr>` `<img>` —— 每个主题都需要全套样式覆盖。

---

## Design Brief — 「modern」家族

**家族定位**：**完全重设**。这是一次彻底的重新设计，**不需要、也不应该**参考你的旧排版习惯。

**你的任务**：

请基于业界最优秀的长文排版设计（参考但不限于：Stratechery、The New Yorker、Substack 优秀长文、Notion 出版物、Linear 博客、Ghost 优秀模板等）的核心思想，**自由发挥**设计两个主题。

**两个主题需要共享的元素**（modern 家族内部统一）：

- 同一套字体系统（建议中文衬线 + 中文无衬线 + 数字字体的组合）
- 同一组结构性 primitives（清单见 §10.A）
- 同一种文末签名（建立公众号身份识别）
- 节奏一致的行高 / 字距 / 段距基础

**两个主题需要差异化的部分**：

- **`modern-humanist`** —— 配色温暖有机，装饰倾向有机 / 书页感，留白多、节奏慢，标题字体可用衬线
- **`modern-tech`** —— 配色克制理性，装饰倾向几何 / 线性，信息密度高、节奏紧凑，标题字体倾向无衬线

设计理念上请大胆——可以引入装饰线、引言区块、首字下沉、章节编号、章节书签、阅读进度提示等任何能提升长文阅读体验的设计语汇。

请附上设计理念说明（为什么选这个配色 / 字体 / 节奏），便于后续工程化扩展。
