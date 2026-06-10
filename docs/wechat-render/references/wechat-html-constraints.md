# 公众号 HTML 渲染约束

- ✅ 所有样式必须 inline（`style="..."`），不能用 `<style>` 或 `class`
- ✅ 不能用 webfont（`@font-face`、Google Fonts）；只能用系统字体栈
- ⚠️ `flex` 大多数情况可用，但极少数复杂嵌套会异常；`grid` 不可靠
- ⚠️ `:hover` 等伪类无效（公众号会清除）
- ✅ `<section>` 嵌套用得多，更稳定
- ✅ 图片用 markdown 原生 `![](url)`，由 `baoyu-post-to-wechat` 上传素材库换 `mediaId`
- ⚠️ 公众号编辑器粘贴时会清除 `<script>` 和大部分 `<style>` 标签 → 100% inline 没问题
- ✅ 颜色用 `rgb()` 或 `#hex`，避免 `var(--)`（公众号会清除 CSS 变量）
- ⚠️ 中文加粗用 `<strong>` 而不是 `<b>`，避免被微信编辑器额外处理
- ❌ **空 `<span>` + `display: inline-block` + `width/height` + `background-color`** 模拟的装饰条 / 方块，公众号 paste 会 strip 掉（实测 2026-05-25 modern-tech 版本的 H3 leader bar 和签名前蓝方块都丢失）。**正确做法：用 Unicode 字符 + `color`**：
  - **竖向短杠（首选 H3 leader / inline marker）** → `▌` (U+258C, 推荐) / `▎` (U+258E, 更细) / `┃` (U+2503) + `color: <accent>; font-size: 18px; vertical-align: -1px; margin-right: 8px`
  - 横向短杠（慎用 — `━━` 视觉"飘"像水平线，不像小标记）→ `━` / `━━` + `color: <accent>; font-weight: 700; letter-spacing: -0.15em`
  - **实心方块（签名 / 列表 marker）** → `■` (U+25A0, 推荐) / `▪` (U+25AA, 更小) + `color: <accent>; font-size: 16px; margin-right: 8px`
  - 圆点 → `●` (U+25CF) / `•` (U+2022) + `color: <accent>`
  - **完整 paste-safe 配方**：
    ```html
    <!-- H3 leader：竖向短杠 -->
    <span style="color: rgb(37, 99, 235); font-size: 18px; margin-right: 8px; vertical-align: -1px;">▌</span>背景

    <!-- 签名方块 -->
    <span style="color: rgb(37, 99, 235); margin-right: 8px; font-size: 16px;">■</span>作者
    ```
- ⚠️ `<div>` + `background-color` 装饰线（如签名下方的灰线 `<div style="width: 25%; height: 1px; background-color: #1D1D1D;">`）相对稳定，但仍有风险（classic-tech 用过此模式，未实测 paste 后是否保留）。如果非装饰必需，可改用 `<hr style="border: none; border-top: 1px solid #1D1D1D; width: 25%; margin-left: auto;">` 或 Unicode 字符
