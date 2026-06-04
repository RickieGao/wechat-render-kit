# 公众号写作发布 SOP

Last updated: 2026-05-27 (Notion-centric overhaul)

## 目标

把公众号文章从 Notion 起稿稳定推进到本地渲染、公众号发布、Notion 回填和经验沉淀。

## 设计原则

- **Notion 是写作主战场** — 选题 / 初稿 / AI 协作迭代 / 留痕全在 Notion 内完成
- 本地仓库是**渲染发布工序** — 终稿才导入, 不再有 "本地 → Notion → 本地" 双向 sync
- **数据库版本号** = 留痕机制 — 同标题多版本通过后缀区分:
  - **人改大版本**: `v2` / `v3` / `v4`
  - **AI 改小版本**: `v2.1` / `v2.2` / `v2.3` (基于 v2 的 AI 迭代)
- **markdown-checklist 自动校验** — 渲染前 skill 自动跑严格校验, 不通过阻塞渲染
- **Notion 回填自动化** — `/publish-notion-update` skill 一键回填 + 引导经验沉淀
- 单篇文章配一份 **per-article SOP checklist** (`_process/sop-checklist.md`), 实例化跟踪进度
- 大文件、PSD/PSB、视频、凭据、本地会话 不入 git
- 渲染主路: Claude Code skill `.claude/skills/wechat-render/` (in-session 调度 subagent); fallback Claude.ai Project (见 `docs/X-path/setup.md`)

## 标准流程 (10 步, 分 7 个阶段)

### ① Notion 起稿 + 数据库登记

1. 在 `公众号文章` 数据库新建条目 (v0 初稿)
   - `标题` = 文章标题 (后续不变, 版本号通过后缀区分而非改标题)
   - `状态` = `草稿`
   - `分类` / `关键词` 等按需

### ② Notion 内写作 + AI 协作 (循环)

2. 在该 Notion 页正文里写作 + AI 协作。**全程在 Notion 里完成**, 不来回切换:
   - **大改** (人手大幅修改 / AI 大幅重写): 用 Notion 的 duplicate page 复制当前页, 标题加版本后缀
     - 人改: `<标题> v2` / `<标题> v3`
     - AI 改: `<标题> v2.1` / `<标题> v2.2` (基于 v2 的 AI 迭代)
     - 数据库列表里能看到完整版本序列
   - **小改** (微调措辞 / 错字 / 标点): 让 AI 在原页加 comment 列**修改清单**, 作者照清单改
3. 状态推进: `草稿` → `写作中` → `待审核` → `审核通过` (终稿确认)

### ③ 本地 bootstrap

4. 在 Claude Code 会话里说 **`/start-article <slug>`** 一键 bootstrap (详见 `.claude/skills/start-article/SKILL.md`):
   - 在 Notion 数据库搜同 slug / 标题最新一条 (按版本号大小, 取最新)
   - 在 `02_文章/<series>/<YYYY-MM>_<slug>/` 建文章目录 + `assets/` + `_process/{research,sop-checklist.md,notes.md}`
   - 把 Notion 终稿正文拉到 `article.md` (含 frontmatter 骨架)
   - 复制 `00_SOP/sop-checklist-template.md` 到 `_process/sop-checklist.md`

### ④ 终稿 frontmatter + 资产到位

5. 补全 `article.md` frontmatter (`title` / `theme` / `signature` / `sources` / `ai_disclosure` 视情况, 按 [markdown-checklist.md](../docs/wechat-render/markdown-checklist.md))
6. 准备图片资产到 `assets/0N-<desc>.png` (封面 + 文中图; 没有图的位置用 image placeholder `![](待补图 "<title>")`, **不要用纯文字 "(待补)"** — subagent 会 drop)

### ⑤ 渲染 HTML

7. 在 Claude Code 会话里说 **"用 `<theme>` 渲染 article.md"** (详见 `.claude/skills/wechat-render/SKILL.md`):
   - skill 自动 dispatch `markdown-checklist-validator` subagent 严格校验
   - **校验通过** → 自动 dispatch `wechat-renderer` subagent 渲染 → `article-formatted.html` (gitignored)
   - **校验失败** → 报错并列出 issue 清单, 不渲染。修复 issue 后重试
   - 浏览器双击预览; 不满意在同会话说 "改/调" 触发 feedback 迭代

### ⑥ 公众号发布

8. paste 公众号草稿箱 (**手动 Ctrl+A/C/V**):
   1. 浏览器打开 `article-formatted.html` → Ctrl+A 全选 → Ctrl+C
   2. mp.weixin.qq.com → 草稿箱 → 新建文章
   3. **鼠标点正文区** (不是标题输入框!) → Ctrl+V
   4. 手动填标题 / 摘要 / 封面 / 作者
   5. 保存草稿 → 手机端预览 → 手动发布

### ⑦ 回填 Notion + 经验沉淀

9. 在 Claude Code 会话里说 **`/publish-notion-update <notion-url> <published-url>`** 一键完成 (详见 `.claude/skills/publish-notion-update/SKILL.md`):
   - 自动回填 Notion: `状态` = `已发布`, `发布日期` = 今天, `文章链接` = 公众号 URL
   - 半自动引导更新:
     - `_process/notes.md` 发布后复盘段 (读者反馈 / 数据观察 / 下次改进)
     - `docs/wechat-render/lessons.md` 跨文章新教训 (本次发现的新规则 / paste 验证情况)
     - `progress.md` 时间线节
10. 发布 1-2 天后, 手动补阅读量 / 点赞数到 Notion `公众号文章` 数据库 (skill 不自动做这步, 数据要等)

---

## per-article SOP checklist

每篇文章 `/start-article` bootstrap 后会自动生成 `_process/sop-checklist.md`, 含本 SOP 全部步骤的复选框 + 引用链接, 一个个打勾推进。模板见 [`sop-checklist-template.md`](sop-checklist-template.md)。

## 状态映射 (本地阶段 ↔ Notion)

| 本地阶段 | Notion `状态` |
|---|---|
| Notion 起稿 (v0) | 草稿 |
| Notion 写作中 (v1/v2/v2.1/... 迭代) | 写作中 |
| AI 大改后等 review | 待审核 |
| 终稿确定 | 审核通过 |
| 本地 bootstrap + 渲染 + 发布 | 审核通过 (本地阶段 Notion 状态不变) |
| 已发布 + 回填 | 已发布 |
| 归档 | 已归档 |

## 文章目录结构

每篇文章目录 (`/start-article` 一键生成):

```text
02_文章/<series>/<YYYY-MM>_<slug>/
├── article.md                          # 终稿 (single source of truth, 从 Notion 拉来)
├── article-formatted.html              # wechat-render skill 渲染输出 (gitignored)
├── assets/                             # 发到公众号的图 (按出场序号化: 01-cover.png 02-...png)
└── _process/
    ├── sop-checklist.md                # 本篇 SOP 工作流程表 (一键生成, 复选框)
    ├── research/                       # 资料原文 (各券商复盘 / 第三方分析, 不进发布)
    ├── ai-sessions/                    # (可选) AI 协作特殊产物 (大段 prompt-engineering 等)
    ├── drafts/                         # (可选) 关键里程碑 snapshot (vN-<阶段>.md)
    └── notes.md                        # (可选) 修改清单 / 想法 / log / 发布后复盘
```

> `_process/{ai-sessions,drafts}` 全部 optional, 按需建。`/start-article` 默认只建 `assets/` + `_process/{research/,sop-checklist.md,notes.md}`。

> 老约定 `00_-06_` 编号文件名 / `_process/raw/notion-export.zip` 2026-05-19 起整段废弃; 历史文章 `02_文章/archive/` 保留原结构不动。

## 渲染工具

- `.claude/skills/wechat-render/` (主路) — Claude Code skill, 输入 `article.md`, 输出 `article-formatted.html`. 含 markdown-checklist 自动校验. 详见 SKILL.md.
- `.claude/skills/start-article/` — 文章 bootstrap skill, 从 Notion 拉终稿到本地
- `.claude/skills/publish-notion-update/` — 发布后回填 Notion + 引导经验沉淀
- `docs/X-path/setup.md` (fallback) — Claude.ai Project 端到端渲染, 仅在主路输出不可接受时用

发布 (markdown → 公众号草稿) 是手动 Ctrl+A/C/V (见 Step 8), baoyu 自动化 2026-05-19 已删。

## 发布风险控制

- 涉及财务 / 投资 / 医疗 / 法律 / 政策 / 时事信息时, 发布前必须核对事实和日期
- 外部引用要保留来源, 微信正文不适合直接展示的链接整理为文末 "想法出处" (`sources` frontmatter)
- 不提交 `_local_large_files/`、`.env`、cookies、浏览器 session、PSD/PSB、视频原始文件
- 不直接自动发布; 自动化最多到保存草稿箱 (即 Step 8 之前)

## 完成定义

一篇文章完成发布流程时, 应至少具备:

- `article.md` (含完整 frontmatter, 通过 markdown-checklist 严格校验)
- `assets/` 下所有发布图已压缩到位
- `_process/sop-checklist.md` 全部步骤打勾
- Notion `状态` = `已发布`, `文章链接` / `发布日期` 已回填
- (可选) `_process/notes.md` 末尾有发布后复盘段
- (可选) `docs/wechat-render/lessons.md` 末尾有跨文章新教训段

---

## 反模式 (已废)

### ❌ 本地 ↔ Notion 双向 sync (2026-05-27 废弃)

**老工作流**: 在 Notion 写初稿 → 拉到本地 article.md → 修改 → 复制回 Notion 修改 → 再拉回本地 AI 改 → ... 反复几轮成终稿。

**为什么废弃**: 作者 hold 不住 "哪个版本最新", 反复 "复制到 Notion → 拉回本地" 是手工 diff/merge, 一旦 Notion 改了一段但本地也改了同一段就要 manually reconcile。实际执行别扭, 切换成本高。

**替代方案**: 本 SOP。写作主战场固定在 Notion (含 AI 协作), 终稿后才本地 bootstrap, 单向流动。

### ❌ `_process/raw/notion-export.zip` 导入 (2026-05-19 已废 + 本次 SOP 整理时再次确认)

**老工作流**: 手动 Notion 导出 zip → 解压 → 复制 markdown 到 `_process/raw/`。

**替代方案**: `/start-article` 通过 Notion MCP 直接拉, 无需导出 zip。

### ❌ `_process/ai-sessions/<model>-<topic>.md` 强制留痕

**老工作流**: AI 协作每次都存 `.md` 中间产物到 `_process/ai-sessions/`。

**为什么废弃**: Claude Code 会话 transcript 已留过程, frontmatter `ai_disclosure.prompts` 已记关键 prompts, 重复留痕 ROI 低。

**替代方案**: 改为 optional。只有大段 prompt-engineering 或特殊产物才存 (例如调试某个装饰元素时反复迭代的对话)。
