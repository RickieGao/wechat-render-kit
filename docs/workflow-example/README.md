# 工作流参考

这是作者本人的工作流参考。本仓库只覆盖其中「渲染」一段; 其余是作者的个性化扩展, 供你设计自己的流程时参考。

---

## 通用流程骨架

以下骨架不绑定任何具体工具, 适用于任何人:

```
① 写作
   在你熟悉的写作环境完成初稿 + 修改 + 终稿确认。
   可以是本地 Markdown 编辑器、Notion、Obsidian、语雀……任何工具皆可。

② 终稿自检 (markdown-checklist)
   对照 docs/wechat-render/markdown-checklist.md 逐项确认:
   - frontmatter 完整 (title / theme / signature / sources / ai_disclosure)
   - 图片用正确语法 (缺图用占位, 不要纯文字 "(待补)")
   - 无破坏渲染的特殊语法

③ 渲染 (本仓库 wechat-render skill)
   对 agent 说: "用 <theme> 渲染 <article.md 路径>"
   → markdown-checklist-validator 严格校验 (不通过阻塞, 修复后重试)
   → 通过 → wechat-renderer 写出 article-formatted.html

④ 浏览器预览
   双击打开 article-formatted.html, 确认视觉效果。
   不满意可在同会话继续说 "调整……" 触发反馈迭代。

⑤ 一键复制进草稿箱
   浏览器打开 HTML → Ctrl+A 全选 → Ctrl+C
   → 公众号编辑器 → 新建文章 → 鼠标点正文区 → Ctrl+V

⑥ 手动发布
   填标题 / 摘要 / 封面 / 作者 → 保存草稿 → 手机端预览 → 手动发布

⑦ (可选) 经验沉淀
   发布后记录本次渲染/发布中发现的新规则或教训, 追加到你自己的笔记里。
```

这 7 步里, 本仓库直接支持的是 **② 终稿自检 + ③ 渲染**。其余步骤由你自行安排工具链。

---

## 一个真实实例 (作者 SOP, 含 Notion 自动化)

[`作者公众号工作流SOP.md`](作者公众号工作流SOP.md) 是本仓库作者自己使用的完整工作流, 供你参考。

**关键说明**:

- 作者以 Notion 为写作主战场, 用 Notion MCP 实现「从 Notion 拉终稿到本地」和「发布后回填 Notion」的自动化。这是**作者的个性化扩展**, 不是本仓库的必要部分。
- SOP 中提到的 `start-article`、`publish-notion-update` 等 skill 是**作者私有 skill**, 不随本仓库发布。你可以把它们当作灵感, 参考其设计自己的类似自动化。
- SOP 中提到的 `markdown-checklist-validator` 和 `wechat-renderer` subagent 是本仓库内置的, 通过 `wechat-render` skill 自动调度, **随本仓库发布**。
- SOP 中的 `02_文章/<series>/` 目录结构是作者的个人约定, 你可以用任意目录结构存放文章。

如果你也想构建类似的 Notion-centric 工作流, SOP 里的「完整工作流」和「设计原则」两节是最值得参考的部分。
