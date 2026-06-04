# wechat-render-kit

**专注思考与写作, 排版交给你的 AI。**

内容写好, 让你的 AI agent 完成公众号排版——渲染、配样式、适配公众号, 你只需一键复制到草稿箱即可发布。自带 4 套精选主题, 还能**创造**全新风格、或**延续**你已有的风格。

---

## 怎么用 (复制这句话给你的 agent)

```
读 <repo-url> , 按里面给 AI 的引导, 把这套公众号排版 skill 装进我当前项目, 然后手把手教我用。
```

剩下的交给你的 agent——它会自我安装并手把手教你。

---

## 👇 以下主要给 AI 看

agents 请读 [`AGENTS.md`](AGENTS.md)。

---

## 4 套主题

| 主题 | 风格一句话 | 适用场景 |
|---|---|---|
| `fresh-humanist` | 全新书页感, 暖赭主色 + 柔暖米底 | 散文 / 随笔 / 感性表达, 想要全新视觉风格 |
| `fresh-tech` | 全新工程博客风, Tailwind 配色 + 深色代码块 | 技术文章 / 工程师博客 / 数据分析 |
| `inherited-humanist` | 感性书页风, 衬线字体 + 暖橙首字下沉 | 散文 / 回忆录 / 感性表达 (个性化范例, 含示例 DNA) |
| `inherited-tech` | 理性分析风, 招牌蓝 + mono 章节编号 | 数据复盘 / 年度总结 / 理性分析 (个性化范例, 含示例 DNA) |

`inherited-*` 两个主题是**个性化范例**: 它们展示了如何把某个公众号的品牌色和视觉习惯注入主题。招牌色是示例色——用 `/new-theme` (inherited 路径) 换成你自己的 DNA。

---

## 造你自己的风格

不满足于 4 个内置主题? 有两条路:

- **fresh 路径**: 从零 brief 设计全新风格——你描述感觉 (参考刊物 / 配色 / 字体倾向), agent 出设计。
- **inherited 路径**: 把你旧公众号的视觉 DNA 提炼成可复用主题, 延续既有品牌。

详见 [GENERATING-THEMES.md](GENERATING-THEMES.md) 和 `/new-theme` skill。

---

## 对 AI 说这些就行 (prompt 速查)

```
用 fresh-tech 渲染 my-article/article.md
```
```
用 fresh-humanist 和 inherited-tech 渲染同一篇, 我要对比两个主题效果
```
```
H2 标题颜色太深, 稍微浅一点
```
```
造个新主题, 我想要类似 The New Yorker 的书卷气
```
```
署名改成「张三」, 招牌色用 #E05C2A
```

---

## 边界

- **需要 AI agent + token**: 渲染由 agent 在运行时完成, 不是静态脚本, 没有 agent 环境无法使用。
- **和 mdnice 不抢场景**: 只想随手排个版、不用 agent? [mdnice](https://mdnice.com) 开网页就能用, 更省事。本工具的定位是「写好内容, 剩下全给 agent」。
- **粘贴是手动的**: HTML 生成后, 复制粘贴到公众号草稿箱这一步是人工完成的——agent 无法操控浏览器替你粘贴。

---

## English

`wechat-render-kit` is an AI-agent skill pack that converts markdown articles into inline-styled HTML ready for WeChat Official Accounts. It ships with 4 curated themes (humanist / tech × fresh / inherited), a `/new-theme` skill to generate custom styles, and a strict pre-render markdown validator. No code to run — your AI agent installs the skills into your project and guides you through every step.

**One sentence to get started (give this to your agent):**

```
Read <repo-url> , follow the AI guide inside to install this WeChat article rendering skill into my project, then walk me through using it.
```

---

## License

MIT
