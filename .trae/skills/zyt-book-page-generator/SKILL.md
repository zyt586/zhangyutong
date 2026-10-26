---
name: zyt-book-page-generator
description: Generate book showcase HTML pages for coursework project. Use when the user asks for book list category ranking ecommerce page or new HTML practice page. Do not use for pure CSS exercise.
---

# zyt-book-page-generator

为高级网页设计课程项目生成图书展销类 HTML 页面。仅输出原生 HTML + CSS + JS，内联 `<style>` / `<script>`，单文件自包含。

## 必须遵守的项目规则

1. **`zyt_` 前缀**：所有 CSS 类名、id 名、JS 变量名、函数名必须以 `zyt_` 开头。任何无前缀的新标识符都是违规。
2. **页面头注释**：每个 HTML 顶部（`<title>` 之后）必须包含：
   ```html
   <!--
   姓名：张语桐
   日期：yyyy-mm-dd
   功能名称：<一句话描述本页功能>
   -->
   ```
3. **原生技术栈**：只用原生 HTML5 / CSS3 / JavaScript（ES6+），**禁止任何第三方库、框架或 CDN**（Vue、React、jQuery、Bootstrap、cdnjs、unpkg、Google Fonts 等）。
4. **图标统一 iconfont**：所有图标使用项目本地 `font/iconfont.css` + `font/iconfont.ttf`，通过 `<i class="iconfont icon-xxx"></i>` 引用。禁止 Font Awesome、Lucide、Material Icons，也禁止内嵌 SVG。
5. **数据获取用 fetch**：与后端交互一律使用原生 `fetch()` API，不使用 `axios`、`$.ajax()` 等。响应统一 `response.json()` 或 `response.text()`。
6. **提示词日志**：执行前先以追加方式写入 `zyt_prompt.md`（UTF-8 无 BOM），格式：
   ```
   ## yyyy-mm-dd-hh-mm
   模型: <model id>
   提示词: <提示词原文>
   ```
7. **新页面必须挂到首页**：新建 HTML 必须在 `index.html` 的 `.links-grid` 里添加 `.link-card` 链接（`<a>` + `<h3>` + `<p>`）。

## 设计系统

| 项目 | 值 |
|------|-----|
| 主色 | `#d53d3d` / `#b83333` |
| 背景 | `#f7f3ef` |
| 卡片 | `#ffffff` |
| 黄色标签 | `#f9dc77` / `#8a5a00` |
| 圆角 | 卡片 12–16px、按钮 8–999px |
| 阴影 | `0 8px 20px rgba(0,0,0,0.05)` |

## 布局与组件

- **网格**：`display: grid; grid-template-columns: repeat(auto-fill, minmax(148px, 1fr)); gap: 14px;`
- **顶部栏**：`position: sticky; top: 0; z-index: 900;` logo + 搜索框 + 操作按钮
- **底部导航**：`position: fixed; bottom: 0; z-index: 1000;` 4 项 `flex:1 1 0` 均分，含 `env(safe-area-inset-bottom)`
- **卡片**：封面图（含角标）→ 书名（2 行截断）→ 作者/元信息 → 价格 → 操作行
- **徽章**：`.zyt_badgeNew`（黄色新品）/ `.zyt_badgeSale`（红色折扣）
- **响应式断点**：480px / 350px 两档

## 模板代码结构

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>页面标题</title>
  <!--
  姓名：张语桐
  日期：yyyy-mm-dd
  功能名称：xxx
  -->
  <link rel="stylesheet" href="font/iconfont.css">
  <style>/* 内联 CSS */</style>
</head>
<body>
  <!-- 页面结构 -->
  <script>/* 内联 JS，全部 zyt_ 前缀 */</script>
</body>
</html>
```

## 注意事项

- 图片路径必须是相对路径 `img/xx.jpg`，**禁止根绝对路径** `/img/xx.jpg`（`file://` 下断裂）
- 所有页面 `lang="zh-CN"`，中文内容，UTF-8 无 BOM
- 文件名可含空格和中文，PowerShell 中用引号 + `-LiteralPath` 操作
- 资源复用：`img/10.jpg` … `img/34.jpg`，`font/iconfont.*`
- 风格参考：中文 UI 文案、emoji 标题、内联样式，不要"现代化"加框架或 CDN
