# AGENTS.md

Static HTML/CSS coursework portfolio ("高级网页设计" class practice). No build, test, lint, package manager, or CI — the `.html` files ARE the product. There is no git repo here.

## 项目规则（必须遵守 / mandatory）
- **`zyt_` 前缀**：你写的所有 CSS 类名、id 名、JS 变量名必须以 `zyt_` 开头（如 `.zyt_card`、`#zyt_box`、`var zyt_count`）。旧页面已有命名不追溯，但向其中新增的名称同样必须带前缀。
- **提示词日志**：收到任何用户提示词后，**执行之前**先以追加方式写入 `zyt_prompt.md`（append only，绝不覆盖/清空已有条目）。每条格式：
  ```
  ## yyyy-mm-dd-hh-mm
  模型: <model id，如 opencode/mimo-v2.6-flash-free>
  提示词: <提示词原文>
  ```
  时间精确到分；模型取当前会话的模型 ID。该文件同样是 UTF-8 无 BOM——不要用 PowerShell 默认编码追加（会写成 GBK/ANSI 损坏中文）。
- **新页面必须挂到首页**：新建的 HTML 文件必须在 `index.html` 的 `.links-grid` 里添加一个 `.link-card` 链接（`<a>` + `<h3>` 标题 + `<p>` 一句说明，参考相邻卡片的写法）。页面改名或删除时同步更新链接，避免死链。

## How to run / verify
- Open pages directly in a browser (`file://` works for almost everything; no dev server needed).
- Verification = open the edited page and check it renders. No command to run.
- Exception: `new-element2.html` uses root-absolute image paths (`/img/19.jpg`…) that break under `file://`. Either fix them to relative paths (`img/19.jpg`) or serve the folder with a local server rooted here.

## Conventions that are easy to get wrong
- **`index.html` is the required gallery page.** Its embedded prompt (line ~142) is the assignment spec: native code only, **no CDN / no external libraries**, new pages must be linked from `index.html` (see mandatory rule above; several legacy pages like `10-1*.html` and `new-element*.html` are still missing there — add them if you touch those).
- **Filenames contain spaces and Chinese characters** (`7-1 上图下文.html`, `3-2 css.html`). Always quote paths and use `-LiteralPath` in PowerShell; prefer the Glob/Read/Edit tools over shell file commands.
- **Files are UTF-8 (no BOM), `lang="zh-CN"`, Chinese content.** The Windows PowerShell console renders these filenames/content as mojibake unless you set `[Console]::OutputEncoding = [Text.Encoding]::UTF8`. Never re-save files as GBK/ANSI — that corrupts the Chinese text.
- Most pages are self-contained with inline `<style>` (and some inline `<script>` on the `10-1* animation` and `new-element3` pages). Shared stylesheets exist but are barely used: `css/css.css` (linked only by `5-1.html`), `css/news.css` (its link in `class4_央视新闻_标题样式.html` is commented out — leave it that way unless asked).
- Assets are numbered flat files: `img/10.jpg` … `img/34.jpg`. Reuse existing images; don't invent filenames.
- Keep the style of neighboring pages: Chinese UI copy, emoji in headings, inline styles are the norm here — do not "modernize" by adding frameworks, CDNs, or a build step.
