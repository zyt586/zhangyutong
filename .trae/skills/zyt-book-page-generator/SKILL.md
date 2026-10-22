---
name: zyt-book-page-generator
description: Generate book showcase HTML pages with zyt prefix naming and design system. Use when user asks for book list category ranking or ecommerce page. Do not use for pure CSS practice.
---

# ZYT 图书展示页面生成器

基于本项目三个已存在的图书页面源码抽取的设计规范与代码模式。生成新页面时严格遵循。

## 参考源

所有规范来自以下三个页面，完整源码位于 `zyt_code_extract.txt` 仓库根目录：

| 页面 | 模式 |
|------|------|
| `new-element-list.html` | URL 参数 category 加 JS 数据对象驱动渲染 |
| `new-element2.html` | 固定 HTML 结构展示单个分类 |
| `new-element3.html` | Tab 切换 加 特色书卡 加 迷你书卡网格 |

## 一 命名规范 强制

所有 CSS class、id、JS 变量、函数名必须以 `zyt_` 开头。禁止无前缀的标识符。

## 二 设计规范

### 配色
- 主色 价格红：`#d53d3d` `#d63c3c` `#d92d2d` `#d9534f` 四选一
- 页面背景：`#f7f3ef` `#f5f2ee` `#f9f7f4` 米色调避免纯白
- 标签徽章：背景 `#f9dc77` `#f9d96d` 文字 `#8a5a00`
- 促销徽章：`linear-gradient(135deg, #ff5b5b, #df2929)` 或纯 `#d9534f`
- 星级评分：满星 `#ffb11d` 空星 `#d7d7d7`
- 旧价格：`#a5a5a5` `#b4b4b4` 加 text-decoration line-through

### 字体
```
font-family: "Microsoft YaHei", "PingFang SC", sans-serif;
```

### 圆角与阴影
- 容器卡片：圆角 14px-18px 阴影 `0 10px 28px rgba(112, 77, 67, 0.07)`
- 图书卡片：圆角 8px-16px 悬停 `translateY(-3px/-5px)` 加 `0 6px 20px rgba(0,0,0,0.1)`

### 布局
- 页面最大宽度 1180px-1200px 居中 margin 40px auto
- 卡片网格 CSS Grid `repeat(auto-fit/auto-fill, minmax(180px-220px, 1fr))`
- 图片必须相对路径 `img/19.jpg` 禁止根绝对路径 `/img/19.jpg` 该路径在 file 协议下断裂

## 三 JavaScript 渲染模式 数据驱动

### 数据对象
```javascript
const zytBooks = {
  '分类名': [
    { name: '书名', price: 34, old: 52, tag: '热销', cover: 'img/19.jpg' }
  ]
};
```

### URL 参数驱动 列表页模式
```javascript
const zytParams = new URLSearchParams(window.location.search);
const zytCategory = zytParams.get('category') || '默认分类';
const zytList = zytBooks[zytCategory] || zytBooks['默认分类'];
document.getElementById('zyt_bookGrid').innerHTML = zytList.map(item => `
  <article class="zyt_bookCard">
    <div class="zyt_cover"><img src="${item.cover}" alt="${item.name}" /></div>
    <div class="zyt_name">${item.name}</div>
    <div class="zyt_priceBox">
      <span class="zyt_price">¥${item.price}</span>
      <span class="zyt_oldPrice">¥${item.old}</span>
    </div>
    <span class="zyt_tag">${item.tag}</span>
  </article>
`).join('');
```

### Tab 切换 榜单页模式
```javascript
zytTabs.forEach(tab => {
  const el = document.createElement('div');
  el.className = 'zyt_tab' + (tab.id === 'hot' ? ' active' : '');
  el.textContent = tab.label;
  el.addEventListener('click', () => {
    document.querySelectorAll('.zyt_tab').forEach(i => i.classList.remove('active'));
    el.classList.add('active');
  });
  document.getElementById('zyt_tabs').appendChild(el);
});
```

### 星级渲染
```javascript
function zytRenderStars(rating) {
  const full = Math.round(rating);
  let stars = '';
  for (let i = 1; i <= 5; i++) {
    stars += `<span class="${i <= full ? '' : 'off'}">★</span>`;
  }
  return stars;
}
```

## 四 完整 HTML 骨架

所有 CSS 内联 禁止外部文件除非用户明确要求。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>zyt 页面标题</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      background: #f7f3ef;
      font-family: "Microsoft YaHei", "PingFang SC", sans-serif;
      color: #333;
    }
    /* 所有 class id 变量均 zyt_ 前缀 */
  </style>
</head>
<body>
  <div class="zyt_container">
    <div class="zyt_topbar">
      <div class="zyt_title">标题</div>
      <a href="index.html" class="zyt_back">← 返回</a>
    </div>
    <div class="zyt_bookGrid" id="zyt_bookGrid"></div>
  </div>
  <script>
    // JS 变量全部 zyt_ 前缀
  </script>
</body>
</html>
```

## 五 交付前检查

1. 所有 CSS class id JS 变量函数均以 zyt_ 开头
2. 图片用相对路径 img/xx.jpg 而非根绝对路径
3. 新建页面必须在 index.html 的 .links-grid 里添加 .link-card 链接 包含 a 加 h3 标题 加 p 一句说明
4. 新页面执行前追加当前提示词到 zyt_prompt.md 用 UTF-8 编码不要覆盖
