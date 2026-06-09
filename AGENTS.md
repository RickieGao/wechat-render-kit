# wechat-render-kit — 给 AI 的引导

你被指向这个仓库, 是为了把它装进用户项目并教 ta 用。下面是给你 (agent) 的步骤。

---

## 你是什么 / 这能干什么

这是一个把 **markdown → 微信公众号 inline-styled HTML** 的 agent 技能包。它不是代码库, 也不是网页工具——它是一套装进用户项目的 skill + 知识文档: 你 (agent) 读懂这些 skill 后, 就能帮用户把写好的文章一键排版成公众号可粘贴的 HTML。技能包自带 4 套精选主题 (感性书页 / 理性工程 各两个); 还带一个 `/new-theme` skill, 既能从零 brief 造全新风格, 也能从用户旧公众号文章里提取 DNA、延续已有品牌。

---

## 第一步: 自我安装到用户项目

### 检测运行环境

**你是 Claude Code (有 `.claude/skills/` 体系)**:

- 目标目录: 用户项目的 `.claude/skills/`、`.claude/agents/`、`docs/wechat-render/`
- 按下方拷贝清单逐一复制文件

**你是 Codex / 其他无 skill 体系的 agent**:

- 把 `docs/wechat-render/` 整目录拷进用户项目
- 在用户项目的 `AGENTS.md` (或 `CLAUDE.md` / 项目说明文件) 追加一条注册声明:
  ```
  渲染公众号文章时, 遵循 docs/wechat-render/system-prompt.md 里的规则和主题定义。
  ```
- `.claude/skills/` 和 `.claude/agents/` 对你无效, 无需拷贝; 但 `docs/wechat-render/` 里的知识文档 (system-prompt / markdown-checklist / config / references) 对任何 agent 都适用。

### 取文件

若你已在本仓库内 (本文件就在仓库根目录), 直接用当前目录的文件, 无需再 clone。若你是从外部链接来的, 先把仓库 clone 到临时目录:

```bash
git clone https://github.com/RickieGao/wechat-render-kit.git /tmp/wechat-render-kit
```

然后从 `/tmp/wechat-render-kit/` 按下方清单拷贝。

### 拷贝清单 (Claude Code 路径, 路径要准确)

以下路径均相对于**本仓库根目录** → 拷贝到**用户项目对应路径**:

| 本仓库路径 | 拷贝到用户项目 |
|---|---|
| `.claude/skills/wechat-render/` | `.claude/skills/wechat-render/` |
| `.claude/skills/new-theme/` | `.claude/skills/new-theme/` |
| `.claude/agents/wechat-renderer.md` | `.claude/agents/wechat-renderer.md` |
| `.claude/agents/markdown-checklist-validator.md` | `.claude/agents/markdown-checklist-validator.md` |
| `.claude/agents/theme-designer.md` | `.claude/agents/theme-designer.md` |
| `docs/wechat-render/system-prompt.md` | `docs/wechat-render/system-prompt.md` |
| `docs/wechat-render/markdown-checklist.md` | `docs/wechat-render/markdown-checklist.md` |
| `docs/wechat-render/config.md` | `docs/wechat-render/config.md` |
| `docs/wechat-render/references/wechat-html-constraints.md` | `docs/wechat-render/references/wechat-html-constraints.md` |
| `docs/wechat-render/references/design-brief-fresh.md` | `docs/wechat-render/references/design-brief-fresh.md` |
| `docs/wechat-render/references/design-brief-inherited.md` | `docs/wechat-render/references/design-brief-inherited.md` |
| `docs/wechat-render/references/style-dna-extracted.md` | `docs/wechat-render/references/style-dna-extracted.md` |
| `docs/wechat-render/references/theme-brief-template.md` | `docs/wechat-render/references/theme-brief-template.md` |
| `docs/wechat-render/references/themes/fresh-humanist.html` | `docs/wechat-render/references/themes/fresh-humanist.html` |
| `docs/wechat-render/references/themes/fresh-tech.html` | `docs/wechat-render/references/themes/fresh-tech.html` |
| `docs/wechat-render/references/themes/inherited-humanist.html` | `docs/wechat-render/references/themes/inherited-humanist.html` |
| `docs/wechat-render/references/themes/inherited-tech.html` | `docs/wechat-render/references/themes/inherited-tech.html` |

### 自主但透明 + 幂等

- **逐步播报**: 每拷完一个目录/文件, 告诉用户拷了什么到哪 (一行一条即可)。
- **不覆盖用户已有内容**:
  - 若用户项目已有 `docs/wechat-render/config.md` → **停下来问用户**: 「你已有 config.md, 要覆盖还是保留?」
  - 若 `docs/wechat-render/references/themes/` 下存在本仓库 4 个主题之外的 `.html` 文件 (用户自建主题) → **停下来问用户**: 「检测到自建主题 `<name>.html`, 不覆盖; 继续安装其余文件?」
  - 其余文件: 自主拷贝, 用户可通过 git 回滚。
- 装完后汇报清单 (文件数量 + 各路径), 然后说: 「装好了, 我们先跑个示例验证一下。」

---

## 第二步: 验证安装

用内置示例文章跑一次完整渲染, 验证安装是否正确:

对用户说: 「我来用 `fresh-tech` 主题渲染示例文章, 验证安装。」

然后触发渲染:

```
用 fresh-tech 渲染 examples/sample-article/article.md
```

渲染完成后, 告诉用户:

```
示例渲染完成。用浏览器打开 examples/sample-article/article-formatted.html 预览效果。
（双击文件, 或在终端: open examples/sample-article/article-formatted.html）
```

如果 `examples/sample-article/article.md` 不存在 (用户项目里没有这个文件), 跳过验证, 直接进第三步。

---

## 第三步: 教日常用法

### 核心用法: 一句话触发渲染

```
用 <主题名> 渲染 <你的文章.md>
```

例如: `用 fresh-tech 渲染 2026-06_my-article/article.md`

### 4 个内置主题

| 主题 | 一句话 | 说明 |
|---|---|---|
| `inherited-humanist` | 感性书页风, 衬线字体 + 暖橙首字下沉 | 个性化范例 (含示例 DNA), 散文 / 回忆 / 感性表达 |
| `inherited-tech` | 理性分析风, 招牌蓝 + mono 章节编号 | 个性化范例 (含示例 DNA), 数据复盘 / 年度总结 |
| `fresh-humanist` | 全新书页感, 暖赭主色 + 柔暖底色 | 品牌中性, 散文 + 想要全新视觉 |
| `fresh-tech` | 全新工程博客风, Tailwind 配色 + 深色代码块 | 品牌中性, 技术文章 / 工程师博客 |

`inherited-*` 两个主题是个性化范例: 它们展示了如何把某个公众号的品牌色和字体习惯注入主题, 但**招牌色是示例色**, 你自己的公众号应该换成自己的配色 (见下方配置)。

### 渲染前的严格校验

每次渲染前, `markdown-checklist-validator` 子 agent 会先检查你的 markdown 是否符合规范 (frontmatter 完整 / 无占位文字 / 中文标点全角化等)。**校验失败会阻塞渲染**, 并给出 issue 清单——你修好后重新触发渲染即可。

### 首次渲染你自己的文章前: 先告诉我你的署名和招牌色

**在你渲染第一篇自己的文章之前**, 我会主动问你:

- 「文末署名用什么名字?」
- 「有没有招牌色? (hex 或 rgb 都行, 留空就用主题自带的强调色)」

你只用自然语言回答, 我会帮你填进 `docs/wechat-render/config.md`——**你不用手动编辑这个文件**。

### 原稿在 Notion / 飞书 / Word?

没关系——告诉我原稿在哪, 我会先把它归一化成 markdown, 再渲染。你只管说「渲染这篇文章」, 源格式不用担心。

### 最后一步是手动的

渲染完成后, 你会看到一个 HTML 文件。用浏览器打开预览; 满意后:

**Ctrl+A (全选) → Ctrl+C (复制) → 粘到公众号草稿箱正文区**

这一步是手动的——agent 无法替你粘贴到公众号编辑器。粘贴后填标题 / 封面 / 摘要, 手机预览, 再发布。

---

## 第四步 (得手后): 进阶

### 造专属主题: `/new-theme`

对我说「造个新主题」就能触发。有两条路径:

- **fresh 路径**: 从零 brief 设计全新风格——你描述感觉 (参考刊物 / 配色情绪 / 字体倾向), 我来出设计。
- **inherited 路径**: 从你旧公众号的文章 HTML 里提取 DNA, 把你已有的视觉语言精化成可复用的主题。

新主题会自动登记到渲染系统, 之后直接 `用 <新主题名> 渲染 article.md` 即可使用。

### 局部迭代

渲染出来后觉得某处不满意? 直接说「H2 标题颜色太深」「代码块字号小一点」——我会用 feedback 模式局部修改 HTML, 不重新渲染整篇。

### 完整工作流参考 (可选)

想看作者本人的完整工作流做参考 (含 Notion 写作自动化、发布回填等)? 说「给我看工作流示例」, 我会指向 `docs/workflow-example/`。

---

## 边界 / 反画像

- **需要 AI agent + token**: 渲染是 agent 在运行时完成的, 不是静态脚本。没有 Claude Code / Codex 等 agent 环境就无法使用。
- **只想随手排个版?**: 如果你不用 agent、只想手动排版, [mdnice](https://mdnice.com) 这类网页工具更省事——开网页就能用, 不需要配环境。本工具的定位是: 写好内容, 剩下的全交给 agent。
- **粘贴是手动的**: 排版 HTML 生成后, 复制粘贴到公众号草稿箱这一步是人工完成的, agent 无法操控浏览器替你粘贴。

---

## 手动安装备选 (clone 失败 / 无网时)

如果你无法 clone 仓库, 或者 agent 拿不到仓库文件, 用户可以手动安装:

1. 下载 / 解压仓库到本地任意位置 (称为 `<kit>/`)
2. 在用户自己的项目根目录下, 手动创建以下目录:
   ```
   mkdir -p .claude/skills .claude/agents docs/wechat-render/references/themes
   ```
3. 复制文件 (按上方拷贝清单, `<kit>/` → 用户项目对应路径):
   - `.claude/skills/wechat-render/` → 整目录复制
   - `.claude/skills/new-theme/` → 整目录复制
   - `.claude/agents/wechat-renderer.md`
   - `.claude/agents/markdown-checklist-validator.md`
   - `.claude/agents/theme-designer.md`
   - `docs/wechat-render/` → 整目录复制 (含 config.md / system-prompt.md / markdown-checklist.md / references/ 全部)
4. 打开新的 Claude Code 会话, 说「帮我设置一下署名和招牌色」, 开始使用。

Codex / 其他 agent 的手动安装: 只需第 1 步 + 第 3 步中 `docs/wechat-render/` 整目录, 并在项目 `AGENTS.md` 里加一行注册声明 (见第一步的 Codex 路径说明)。
