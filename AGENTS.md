# wechat-render-kit — 给 AI 的引导

你被指向这个仓库, 是为了把它**一次性装进用户项目**, 并**用一条简介教 ta 用**。下面是给你 (agent) 的步骤。

---

## ⚙️ 执行总则（任何模型先读，本次 onboarding 严格遵守）

无论你是 **Claude Code、Codex 还是其他 agent**，执行本引导时遵守以下规范——目的是让不同模型产出**一致、精炼、完整**的引导，而不是各自发挥成详略不一、或拖沓多轮的版本：

1. **一次性装完，不要每步停下等用户。** 安装是你的自主动作；**唯一**允许停下的情况是"命中覆盖风险"（见第一步），其余一气呵成。不要把"装一点→问一句→再装一点"拆成多轮——那正是慢的根源。
2. **装完只输出一条「三段简介」**：安装内容 / 日常用法 / 进阶用法（见第二步），末尾附一句可选验证。**不要**把安装确认、用法、进阶拆成多轮一问一答。
3. **不扩写**：要点式，不解释"为什么"、不加背景科普、不把要点展开成长段落（防输出膨胀）；**也不省略**任何标「必给」的要点（防输出过简）。
4. **`📖` = 给你（agent）读的背景，不要照念给用户；`📤` = 对用户输出的内容。** 受众分离——别把背景展开成科普倒给用户。
5. **篇幅基线 = 精炼完整**：核心操作（触发渲染 / 手动粘贴 / 署名配置）讲清楚，辅助信息（自我介绍 / 边界）一两句带过。
6. 不同 agent 只有**安装动作**不同（Claude Code 拷 skill / Codex 拷 docs + 注册）；**对用户说的话一律按本规范**，不因模型而异。

> 下面每个 `📤` 框，就是该处你对用户输出的**契约**：必须覆盖框里的要点，且不超出框定的篇幅。框外的文字是给你（agent）读的执行说明，**不要照念给用户**。

---

## 你是什么 / 这能干什么

> 📖 给你的背景（理解即可，不要整段念给用户）：这是一个把 **markdown → 微信公众号 inline-styled HTML** 的 agent 技能包。它不是代码库、也不是网页工具——而是一套装进用户项目的 skill + 知识文档：你读懂后就能帮用户把写好的文章一键排版成公众号可粘贴的 HTML。自带 4 套精选主题（感性书页 / 理性工程 各两个）；还带一个 `/new-theme` skill，既能从零 brief 造新风格，也能从用户旧公众号文章提取 DNA、延续已有品牌。

> 📤 **开场对用户说（≤2 句，必给）**：用一两句说明"我是帮你把 markdown 文章排成公众号可粘贴 HTML 的助手，现在先把工具一次性装进你的项目"。**不要**在开场罗列全部功能 / 主题清单——第二步会讲。说完**直接开始装，不要问"可以吗"**。

---

## 第一步: 一次性自我安装（不停顿）

### 检测运行环境

**你是 Claude Code (有 `.claude/skills/` 体系)**:

- 目标目录: 用户项目的 `.claude/skills/`、`.claude/agents/`、`docs/wechat-render/`
- 按下方"批量拷贝"复制

**你是 Codex / 其他无 skill 体系的 agent**:

- 把 `docs/wechat-render/` 整目录拷进用户项目
- 在用户项目的 `AGENTS.md` (或 `CLAUDE.md` / 项目说明文件) 追加一条注册声明:
  ```
  渲染公众号文章时, 遵循 docs/wechat-render/system-prompt.md 里的规则和主题定义。
  ```
- `.claude/skills/` 和 `.claude/agents/` 对你无效, 无需拷贝; 但 `docs/wechat-render/` 里的知识文档对任何 agent 都适用。

### 取文件

若你已在本仓库内 (本文件就在仓库根目录), 直接用当前目录的文件, 无需再 clone。若你是从外部链接来的, **用浅克隆** (省时, 不要历史):

```bash
git clone --depth 1 https://github.com/RickieGao/wechat-render-kit.git /tmp/wechat-render-kit
```

### 批量拷贝 (Claude Code 路径)

**用尽量少的命令一次性拷这三处** (例如一条 `cp -r` 带多个源)，**不要逐个文件拷贝、更不要逐项播报**——这是本次提速的关键:

1. `.claude/skills/wechat-render/` + `.claude/skills/new-theme/`
2. `.claude/agents/` 下 3 个 `.md`: `wechat-renderer` / `markdown-checklist-validator` / `theme-designer`
3. `docs/wechat-render/` **整目录** (含 `system-prompt.md` / `markdown-checklist.md` / `config.md` / `references/` 全部主题与文档)

> **完整文件清单 (仅供你核对覆盖面，不必逐个拷)**:
>
> | 本仓库路径 | 拷贝到用户项目 |
> |---|---|
> | `.claude/skills/wechat-render/` | `.claude/skills/wechat-render/` |
> | `.claude/skills/new-theme/` | `.claude/skills/new-theme/` |
> | `.claude/agents/wechat-renderer.md` | `.claude/agents/wechat-renderer.md` |
> | `.claude/agents/markdown-checklist-validator.md` | `.claude/agents/markdown-checklist-validator.md` |
> | `.claude/agents/theme-designer.md` | `.claude/agents/theme-designer.md` |
> | `docs/wechat-render/system-prompt.md` | `docs/wechat-render/system-prompt.md` |
> | `docs/wechat-render/markdown-checklist.md` | `docs/wechat-render/markdown-checklist.md` |
> | `docs/wechat-render/config.md` | `docs/wechat-render/config.md` |
> | `docs/wechat-render/references/wechat-html-constraints.md` | `docs/wechat-render/references/wechat-html-constraints.md` |
> | `docs/wechat-render/references/design-brief-modern.md` | `docs/wechat-render/references/design-brief-modern.md` |
> | `docs/wechat-render/references/design-brief-classic.md` | `docs/wechat-render/references/design-brief-classic.md` |
> | `docs/wechat-render/references/style-dna-extracted.md` | `docs/wechat-render/references/style-dna-extracted.md` |
> | `docs/wechat-render/references/theme-brief-template.md` | `docs/wechat-render/references/theme-brief-template.md` |
> | `docs/wechat-render/references/themes/modern-humanist.html` | （同路径） |
> | `docs/wechat-render/references/themes/modern-tech.html` | （同路径） |
> | `docs/wechat-render/references/themes/classic-humanist.html` | （同路径） |
> | `docs/wechat-render/references/themes/classic-tech.html` | （同路径） |

### 唯一允许的停顿：防覆盖（幂等）

- 若用户项目已有 `docs/wechat-render/config.md` → **停下来问**：「你已有 config.md, 要覆盖还是保留?」
- 若 `docs/wechat-render/references/themes/` 下存在本仓库 4 个主题之外的 `.html`（用户自建主题）→ **停下来问**：「检测到自建主题 `<name>.html`, 不覆盖; 继续安装其余文件?」
- 其余文件：**自主拷贝，不要逐个问、不要逐个播报**，用户可通过 git 回滚。

> 📤 **本步对用户输出**：装完给**一行汇总**（如「✅ 已装好 N 个文件：2 个 skill + 3 个 agent + `docs/wechat-render/` 知识库（含 4 套主题））。**绝不逐文件播报。** 仅在命中上面"防覆盖"时才停下问。装完**不要停顿等确认，直接进第二步输出简介**。

---

## 第二步: 一条三段简介（装完一次性输出，别拆成多轮）

> 📤 **按顺序输出以下三段 + 一句收尾**，要点式、勿展开"为什么"：
>
> **【① 安装内容】**（1-2 行）：装了什么——**2 个 skill**（`wechat-render` 渲染 / `new-theme` 造主题）+ **3 个 agent**（渲染器 / 严格校验器 / 主题设计）+ `docs/wechat-render/` **知识库**（system-prompt / checklist / config + 4 套主题）。都在你项目里，可 git 回滚。
>
> **【② 日常用法】**：
> 1. 核心用法（代码块**原样**给）：
>    ```
>    用 <主题名> 渲染 <你的文章.md>
>    ```
> 2. **4 个内置主题**（**原样**给下方表格）；表后附 1 句：`classic-*` 是个性化范例，招牌色是示例色，可用 `/new-theme` 换成你自己的。
> 3. **三条关键提醒**（各 1 句，勿展开）：
>    - 渲染前 `markdown-checklist-validator` 会自动严格校验，**失败会拦住渲染**并给 issue 清单，修好重发即可。
>    - **首次渲染你自己的文章前**，我会问你署名 + 招牌色，并代填 `config.md`（你不用手动改文件）。
>    - 最后**复制粘贴到公众号草稿箱是手动的**（浏览器开 HTML → Ctrl+A / Ctrl+C → 贴进草稿箱），agent 无法替你粘贴。
> 4. **源格式**（1 句）：原稿在 Notion / 飞书 / Word 也行，告诉我在哪，我先归一化成 markdown 再渲染。
>
> **【③ 进阶用法】**（每项 1 句，勿展开）：
> - **`/new-theme` 造专属主题**：`modern`（从零 brief 设计全新风格）/ `classic`（从你旧号文章提取 DNA、延续已有品牌）。
> - **局部迭代**：渲染后哪不满意直接说（如「H2 标题颜色太深」），我用 feedback 模式只改局部、不重渲整篇。
> - **完整工作流参考**（提一句即可）：想看作者本人的 Notion 写作 + 发布工作流，说「给我看工作流示例」，我指向 `docs/workflow-example/`。
>
> **【收尾 · 可选验证】**（1 句，必给）：「要不要我先用**内置示例**跑一次渲染、顺便验证安装？想直接上手你自己的文章也行。」 → **用户说要**才渲染 `examples/sample-article/article.md` 并给浏览器预览指引；**用户没要求就不渲染、不阻塞**（示例渲染是当前最慢的一步，默认不跑）。
>
> 篇幅：除主题表外，整条简介控制在 ~20 行内。

### 4 个内置主题

| 主题 | 一句话 | 说明 |
|---|---|---|
| `classic-humanist` | 感性书页风, 衬线字体 + 暖橙首字下沉 | 个性化范例 (含示例 DNA), 散文 / 回忆 / 感性表达 |
| `classic-tech` | 理性分析风, 招牌蓝 + mono 章节编号 | 个性化范例 (含示例 DNA), 数据复盘 / 年度总结 |
| `modern-humanist` | 全新书页感, 暖赭主色 + 柔暖底色 | 品牌中性, 散文 + 想要全新视觉 |
| `modern-tech` | 全新工程博客风, Tailwind 配色 + 深色代码块 | 品牌中性, 技术文章 / 工程师博客 |

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
3. 复制文件 (按上方完整清单, `<kit>/` → 用户项目对应路径):
   - `.claude/skills/wechat-render/` → 整目录复制
   - `.claude/skills/new-theme/` → 整目录复制
   - `.claude/agents/wechat-renderer.md`
   - `.claude/agents/markdown-checklist-validator.md`
   - `.claude/agents/theme-designer.md`
   - `docs/wechat-render/` → 整目录复制 (含 config.md / system-prompt.md / markdown-checklist.md / references/ 全部)
4. 打开新的 Claude Code 会话, 说「帮我设置一下署名和招牌色」, 开始使用。

Codex / 其他 agent 的手动安装: 只需第 1 步 + 第 3 步中 `docs/wechat-render/` 整目录, 并在项目 `AGENTS.md` 里加一行注册声明 (见第一步的 Codex 路径说明)。
