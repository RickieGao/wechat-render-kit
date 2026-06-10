# wechat-render-kit — 给 AI 的引导

你被指向这个仓库, 是为了把它装进用户项目并教 ta 用。下面是给你 (agent) 的步骤。

---

## ⚙️ 执行总则（任何模型先读，本次 onboarding 严格遵守）

无论你是 **Claude Code、Codex 还是其他 agent**，执行本引导时遵守以下规范——目的是让不同模型产出**一致、精炼、完整**的引导，而不是各自发挥成详略不一的版本：

1. **按四步顺序执行**，不跳步、不合并、不提前剧透后面的步骤。
2. **每步只输出该步「📤 对用户输出」框里列的要点**，按给定顺序；标「原样」的代码块 / 表格照原文给。
3. **不扩写**：不解释"为什么"、不加背景科普、不把要点展开成段落（防输出膨胀）；**也不省略**任何标「必给」的要点（防输出过简）。
4. **每步末尾停下**，按该步指示等用户回应（确认 / 给信息 / 触发渲染），**不要一口气把四步全倒给用户**。
5. **篇幅基线 = 精炼完整**：要点式；核心操作（触发渲染 / 手动粘贴 / 署名配置）讲清楚，辅助信息（自我介绍 / 边界）一两句带过。
6. 不同 agent 只有**安装动作**不同（Claude Code 拷 skill / Codex 拷 docs + 注册）；**对用户说的话一律按本规范**，不因模型而异。

> 下面每一步的 `📤` 框，就是该步你对用户输出的**契约**：必须覆盖框里的要点，且不超出框定的篇幅。框外的文字是给你（agent）读的执行说明，**不要照念给用户**。

---

## 你是什么 / 这能干什么

> 📖 给你的背景（理解即可，不要整段念给用户）：这是一个把 **markdown → 微信公众号 inline-styled HTML** 的 agent 技能包。它不是代码库、也不是网页工具——而是一套装进用户项目的 skill + 知识文档：你读懂后就能帮用户把写好的文章一键排版成公众号可粘贴的 HTML。自带 4 套精选主题（感性书页 / 理性工程 各两个）；还带一个 `/new-theme` skill，既能从零 brief 造新风格，也能从用户旧公众号文章提取 DNA、延续已有品牌。

> 📤 **开场对用户说（≤2 句，必给）**：用一两句说明"我是帮你把 markdown 文章排成公众号可粘贴 HTML 的助手，现在先把工具装进你的项目"。**不要**在开场罗列全部功能 / 主题清单——后面步骤会讲。

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
| `docs/wechat-render/references/design-brief-modern.md` | `docs/wechat-render/references/design-brief-modern.md` |
| `docs/wechat-render/references/design-brief-classic.md` | `docs/wechat-render/references/design-brief-classic.md` |
| `docs/wechat-render/references/style-dna-extracted.md` | `docs/wechat-render/references/style-dna-extracted.md` |
| `docs/wechat-render/references/theme-brief-template.md` | `docs/wechat-render/references/theme-brief-template.md` |
| `docs/wechat-render/references/themes/modern-humanist.html` | `docs/wechat-render/references/themes/modern-humanist.html` |
| `docs/wechat-render/references/themes/modern-tech.html` | `docs/wechat-render/references/themes/modern-tech.html` |
| `docs/wechat-render/references/themes/classic-humanist.html` | `docs/wechat-render/references/themes/classic-humanist.html` |
| `docs/wechat-render/references/themes/classic-tech.html` | `docs/wechat-render/references/themes/classic-tech.html` |

### 执行原则（给你）：自主但透明 + 幂等

- **不覆盖用户已有内容**：
  - 若用户项目已有 `docs/wechat-render/config.md` → **停下来问用户**：「你已有 config.md, 要覆盖还是保留?」
  - 若 `docs/wechat-render/references/themes/` 下存在本仓库 4 个主题之外的 `.html`（用户自建主题）→ **停下来问用户**：「检测到自建主题 `<name>.html`, 不覆盖; 继续安装其余文件?」
  - 其余文件：自主拷贝，用户可通过 git 回滚。

> 📤 **本步对用户输出**：
> 1. 拷贝过程**逐项播报**（每项 1 行：拷了什么 → 到哪），不解释每个文件的作用。
> 2. 命中已有 `config.md` / 自建主题时，按上面原话停下问。
> 3. 装完：**1 行清单汇总**（共 N 个文件）+ 1 句过渡「装好了，先跑个示例验证一下？」
> 4. **停下等用户确认**，再进第二步。
> 篇幅：除逐项播报外 ≤3 句。

---

## 第二步: 验证安装

用内置示例文章跑一次完整渲染，验证安装是否正确。若 `examples/sample-article/article.md` 不存在（用户项目里没有），跳过本步，直接进第三步。

> 📤 **本步对用户输出**（固定 4 件，精炼）：
> 1. 1 句：「我来用 `modern-tech` 渲染示例文章，验证安装。」
> 2. 触发渲染（原样）：
>    ```
>    用 modern-tech 渲染 examples/sample-article/article.md
>    ```
> 3. 渲染完，给预览指引（原样）：
>    ```
>    示例渲染完成。用浏览器打开 examples/sample-article/article-formatted.html 预览效果。
>    ```
> 4. 1 句收尾：「预览没问题的话，我教你日常怎么用？」**停下等用户。**

---

## 第三步: 教日常用法

> 📤 **本步对用户输出**（只给以下 5 块，按顺序；勿展开"为什么"）：
> 1. **核心用法**（必给，代码块原样）：
>    ```
>    用 <主题名> 渲染 <你的文章.md>
>    ```
> 2. **4 个内置主题**（必给，原样给下方表格）；表后附 1 句：`classic-*` 是个性化范例，招牌色是示例色，可用 `/new-theme` 换成你自己的。
> 3. **三条关键提醒**（必给，各 1 句，勿展开）：
>    - 渲染前 `markdown-checklist-validator` 会自动严格校验，**失败会拦住渲染**并给 issue 清单，修好重发即可。
>    - **首次渲染你自己的文章前**，我会问你署名 + 招牌色，并代填 `config.md`（你不用手动改文件）。
>    - 最后**复制粘贴到公众号草稿箱是手动的**（浏览器打开 HTML → Ctrl+A / Ctrl+C → 贴进草稿箱），agent 无法替你粘贴。
> 4. **源格式**（1 句）：原稿在 Notion / 飞书 / Word 也行，告诉我在哪，我先归一化成 markdown 再渲染。
> 5. **收尾**（1 句，必给）：问「现在就渲染你的第一篇，还是先看看进阶功能？」**停下等用户。**
> 篇幅：除主题表外整步 ≤14 行。

### 4 个内置主题

| 主题 | 一句话 | 说明 |
|---|---|---|
| `classic-humanist` | 感性书页风, 衬线字体 + 暖橙首字下沉 | 个性化范例 (含示例 DNA), 散文 / 回忆 / 感性表达 |
| `classic-tech` | 理性分析风, 招牌蓝 + mono 章节编号 | 个性化范例 (含示例 DNA), 数据复盘 / 年度总结 |
| `modern-humanist` | 全新书页感, 暖赭主色 + 柔暖底色 | 品牌中性, 散文 + 想要全新视觉 |
| `modern-tech` | 全新工程博客风, Tailwind 配色 + 深色代码块 | 品牌中性, 技术文章 / 工程师博客 |

---

## 第四步 (得手后): 进阶

> 📤 **本步**：仅在用户完成首次渲染、或主动问"还能干什么"时讲；每项 1 句，勿展开：
> - **`/new-theme` 造专属主题**：两条路径——`modern`（从零 brief 设计全新风格）/ `classic`（从你旧号文章提取 DNA、延续已有品牌）。
> - **局部迭代**：渲染后哪不满意直接说（如「H2 标题颜色太深」），我用 feedback 模式只改局部、不重渲整篇。
> - **完整工作流参考**（可选，提一句即可）：想看作者本人的 Notion 写作 + 发布工作流，说「给我看工作流示例」，我指向 `docs/workflow-example/`。
> 篇幅：整步 ≤6 行。默认不主动长篇展开，用户问再细讲。

---

## 边界 / 反画像

> 📌 给你的背景，**onboarding 时不要主动倒给用户**；用户问"能不能 XX / 和 mdnice 啥区别 / 为什么要 agent"时，用下面的点回答即可。

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
