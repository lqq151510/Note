# CSS 层叠样式表详解

## 📋 CSS 概述

**CSS**（Cascading Style Sheets，层叠样式表）是一种样式表语言，用于描述 HTML 或 XML 文档的呈现方式。它定义了网页的布局、颜色、字体、动画等视觉表现。

### 核心特点：

- **层叠性**：多个样式规则可以应用于同一元素，按特定规则决定最终样式
    
- **继承性**：某些样式属性可以从父元素传递给子元素
    
- **优先级**：不同选择器有不同的权重，决定样式应用的优先级
    
- **响应式**：支持创建适应不同设备的网页
    

---

## 🔗 CSS 引入方式

### 1. 内联样式

```
<p style="color: red; font-size: 16px;">内联样式示例</p>
```

**特点**：优先级最高，但不推荐大量使用

### 2. 内部样式表

```
<head>
    <style>
        p {
            color: blue;
            font-size: 18px;
        }
    </style>
</head>
```

**特点**：适用于单个页面

### 3. 外部样式表（推荐）

```
<head>
    <link rel="stylesheet" href="styles.css">
</head>
```

在 `styles.css`文件中：

```
p {
    color: green;
    font-size: 20px;
}
```

**特点**：可复用，易于维护，支持缓存

### 4. @import 导入

```
/* 在 CSS 文件中导入其他 CSS */
@import url("reset.css");
@import url("layout.css");

/* 或在 style 标签中 */
<style>
    @import url("styles.css");
</style>
```

---

## 🎯 CSS 选择器详解

### 1. 基本选择器

```
/* 元素选择器 */
p { color: blue; }
div { margin: 10px; }//选择的页面所有标签

/* 类选择器 */
.class-name { color: red; }
.important { font-weight: bold; }//选择页面所有class属性为对应类的标签

/* ID 选择器 */
#header { background: #333; }
#main-content { width: 80%; }//选择页面ID属性为属性值的标签

/* 通配符选择器 */
* { margin: 0; padding: 0; }

/* 属性选择器 */
[type="text"] { border: 1px solid #ccc; }
[href^="https"] { color: green; }  /* 以 https 开头 */
[href$=".pdf"]::after { content: " (PDF)"; }  /* 以 .pdf 结尾 */
[class*="btn"] { padding: 10px; }  /* 包含 btn */

/* 分组选择器 */
/* 为多个不同类型的元素设置相同的样式 */
h1, h2, h3 {
    color: #333;
    font-family: Arial, sans-serif;
    margin-bottom: 1rem;
}

/* 后代选择器 */
/* 选择所有在nav元素内的a元素 */
nav a {
    color: #333;
    text-decoration: none;
}

/* 选择所有在.content类内的p元素 */
.content p {
    line-height: 1.6;
    margin-bottom: 1em;
}

/* 选择table内所有th元素 */
table th {
    background-color: #f5f5f5;
    font-weight: bold;
}
```
### 2. 组合选择器

```
/* 后代选择器 */
div p { color: red; }  /* div 下的所有 p */

/* 子元素选择器 */
ul > li { list-style: none; }  /* 直接子元素 */

/* 相邻兄弟选择器 */
h1 + p { margin-top: 0; }  /* 紧跟在 h1 后的 p */

/* 通用兄弟选择器 */
h1 ~ p { color: gray; }  /* h1 后面的所有 p */


```

### 3. 伪类选择器

```
/* 链接状态 */
a:link { color: blue; }      /* 未访问 */
a:visited { color: purple; } /* 已访问 */
a:hover { color: red; }      /* 鼠标悬停 */
a:active { color: orange; }  /* 点击时 */
a:focus { outline: 2px solid blue; } /* 获得焦点 */

/* 结构伪类 */
li:first-child { color: red; }
li:last-child { color: blue; }
li:nth-child(odd) { background: #f0f0f0; }  /* 奇数行 */
li:nth-child(even) { background: #e0e0e0; } /* 偶数行 */
li:nth-child(3n) { color: green; }  /* 每3个 */
li:nth-child(3n+1) { color: purple; } /* 3n+1个 */

/* 表单状态 */
input:checked { background: blue; }
input:disabled { opacity: 0.5; }
input:required { border-color: red; }
input:valid { border-color: green; }
input:invalid { border-color: red; }
```

### 4. 伪元素选择器

```
/* 在元素前插入内容 */
p::before { 
    content: "→ "; 
    color: red;
}

/* 在元素后插入内容 */
p::after { 
    content: " ←"; 
    color: blue;
}

/* 选中文本样式 */
::selection {
    background: yellow;
    color: black;
}

/* 首字母 */
p::first-letter { 
    font-size: 2em; 
    font-weight: bold;
}

/* 首行 */
p::first-line { 
    font-weight: bold; 
}

/* 输入框占位符 */
::placeholder { 
    color: #999; 
    font-style: italic;
}
```

### 5. 选择器优先级

```
/* 优先级顺序（从高到低）：
   1. !important
   2. 内联样式
   3. ID 选择器
   4. 类选择器、属性选择器、伪类
   5. 元素选择器、伪元素
   6. 通用选择器
*/

/* 示例 */
#id { color: red; }           /* 权重：0,1,0,0 */
.class { color: blue; }       /* 权重：0,0,1,0 */
div p { color: green; }       /* 权重：0,0,0,2 */
* { color: black; }           /* 权重：0,0,0,0 */

/* !important 覆盖所有 */
p { 
    color: red !important;  /* 最高优先级 */
}
```

---

## 📦 CSS 盒模型

### 1. 标准盒模型

```
.box {
    width: 200px;           /* 内容宽度 */
    height: 100px;          /* 内容高度 */
    padding: 20px;          /* 内边距 */
    border: 5px solid #000; /* 边框 */
    margin: 30px;           /* 外边距 */
    
    /* 总宽度 = width + padding*2 + border*2 */
    /* 总高度 = height + padding*2 + border*2 */
}
```

### 2. 替代盒模型

```
.box {
    box-sizing: border-box;  /* 宽度和高度包含边框和内边距 */
    width: 200px;            /* 总宽度 */
    padding: 20px;           /* 内边距 */
    border: 5px solid #000;  /* 边框 */
    
    /* 内容宽度 = width - padding*2 - border*2 */
}
```

### 3. 边距塌陷

```
.parent {
    margin: 20px 0;
}

.child {
    margin: 30px 0;  /* 与父元素边距会塌陷，取较大值 */
}

/* 解决方案 */
.parent {
    padding: 1px 0;  /* 添加内边距 */
    /* 或 */
    border: 1px solid transparent;  /* 添加边框 */
    /* 或 */
    overflow: auto;  /* 创建 BFC */
}
```

---

## 🎨 常用 CSS 属性

### 1. 颜色和背景

```
.element {
    /* 颜色值 */
    color: #ff0000;                  /* 十六进制 */
    color: rgb(255, 0, 0);           /* RGB */
    color: rgba(255, 0, 0, 0.5);     /* RGBA，带透明度 */
    color: hsl(0, 100%, 50%);        /* HSL */
    color: hsla(0, 100%, 50%, 0.5);  /* HSLA */
    
    /* 背景 */
    background-color: #f0f0f0;
    background-image: url("bg.jpg");
    background-repeat: no-repeat;    /* repeat, repeat-x, repeat-y */
    background-position: center center;  /* x y */
    background-size: cover;          /* cover, contain, 100% 100% */
    background-attachment: fixed;    /* scroll, fixed */
    
    /* 渐变 */
    background: linear-gradient(to right, red, yellow);
    background: radial-gradient(circle, red, yellow);
    background: conic-gradient(red, yellow, green);
    
    /* 背景简写 */
    background: #fff url("bg.jpg") no-repeat center/cover;
}
```

### 2. 文本和字体

```
.element {
    /* 字体 */
    font-family: Arial, sans-serif;  /* 字体栈 */
    font-size: 16px;                 /* 绝对大小 */
    font-size: 1rem;                 /* 相对大小 */
    font-weight: bold;               /* normal, bold, 100-900 */
    font-style: italic;              /* normal, italic, oblique */
    font-variant: small-caps;        /* 小型大写字母 */
    
    /* 文本 */
    text-align: center;              /* left, right, center, justify */
    line-height: 1.5;                /* 行高 */
    text-decoration: underline;      /* none, underline, overline, line-through */
    text-transform: uppercase;       /* lowercase, capitalize */
    text-indent: 2em;                /* 首行缩进 */
    letter-spacing: 1px;             /* 字母间距 */
    word-spacing: 2px;               /* 单词间距 */
    text-shadow: 2px 2px 5px rgba(0,0,0,0.5);  /* x y blur color */
    
    /* 溢出处理 */
    white-space: nowrap;             /* normal, nowrap, pre, pre-wrap, pre-line */
    overflow: hidden;                /* 溢出隐藏 */
    text-overflow: ellipsis;         /* 溢出显示省略号 */
    
    /* 字体简写 */
    font: italic bold 16px/1.5 Arial, sans-serif;
}
```

### 3. 边框和轮廓

```
.element {
    /* 边框 */
    border-width: 2px;
    border-style: solid;      /* solid, dotted, dashed, double, groove, ridge, inset, outset */
    border-color: #000;
    border-radius: 10px;      /* 圆角 */
    
    /* 单独设置各边 */
    border-top: 1px solid red;
    border-right: 2px dashed blue;
    border-bottom: 3px dotted green;
    border-left: 4px double purple;
    
    /* 边框简写 */
    border: 2px solid #000;
    
    /* 轮廓（不占空间） */
    outline: 2px solid red;
    outline-offset: 5px;      /* 轮廓偏移 */
    
    /* 阴影 */
    box-shadow: 5px 5px 10px rgba(0,0,0,0.3);
    /* 参数：x偏移 y偏移 模糊半径 扩散半径 颜色 inset(内阴影) */
    
    /* 多重阴影 */
    box-shadow: 
        0 2px 5px rgba(0,0,0,0.1),
        0 5px 15px rgba(0,0,0,0.2);
}
```

### 4. 尺寸和定位

```
.element {
    /* 尺寸 */
    width: 100px;
    height: 100px;
    min-width: 50px;
    max-width: 200px;
    min-height: 50px;
    max-height: 200px;
    
    /* 内边距 */
    padding: 10px;                    /* 四个方向 */
    padding: 10px 20px;               /* 上下 左右 */
    padding: 10px 20px 15px;          /* 上 左右 下 */
    padding: 10px 20px 15px 25px;     /* 上 右 下 左 */
    
    /* 外边距 */
    margin: 10px;
    margin: auto;                     /* 水平居中 */
    
    /* 定位 */
    position: static;       /* 默认，正常文档流 */
    position: relative;     /* 相对定位，相对于自身 */
    position: absolute;     /* 绝对定位，相对于最近的定位父元素 */
    position: fixed;        /* 固定定位，相对于视口 */
    position: sticky;       /* 粘性定位，relative 和 fixed 的混合 */
    
    /* 定位偏移 */
    top: 10px;
    right: 20px;
    bottom: 30px;
    left: 40px;
    
    /* 层叠顺序 */
    z-index: 10;
}
```

### 5. 显示和可见性

```
.element {
    /* 显示类型 */
    display: block;         /* 块级元素 */
    display: inline;        /* 行内元素 */
    display: inline-block;  /* 行内块元素 */
    display: none;          /* 不显示，不占空间 */
    display: flex;          /* 弹性盒子 */
    display: grid;          /* 网格布局 */
    display: table;         /* 表格布局 */
    
    /* 可见性 */
    visibility: visible;    /* 可见 */
    visibility: hidden;     /* 隐藏，但占空间 */
    visibility: collapse;   /* 表格行/列隐藏 */
    
    /* 不透明度 */
    opacity: 0.5;           /* 0-1，影响子元素 */
    
    /* 溢出 */
    overflow: visible;      /* 默认，内容可见 */
    overflow: hidden;       /* 隐藏溢出 */
    overflow: scroll;       /* 显示滚动条 */
    overflow: auto;         /* 需要时显示滚动条 */
    
    /* 裁剪 */
    clip-path: circle(50% at 50% 50%);  /* 圆形 */
    clip-path: polygon(0 0, 100% 0, 50% 100%);  /* 三角形 */
}
```

---

## 🎪 CSS 布局

### 1. 传统布局

```
/* 浮动布局 */
.float-left {
    float: left;
    width: 50%;
}

.float-right {
    float: right;
    width: 50%;
}

.clearfix::after {  /* 清除浮动 */
    content: "";
    display: table;
    clear: both;
}

/* 定位布局 */
.container {
    position: relative;
}

.absolute-box {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);  /* 居中 */
}
```

### 2. Flexbox 弹性盒子

```
/* 容器属性 */
.container {
    display: flex;           /* 或 inline-flex */
    
    /* 主轴方向 */
    flex-direction: row;     /* row, row-reverse, column, column-reverse */
    
    /* 换行 */
    flex-wrap: nowrap;       /* nowrap, wrap, wrap-reverse */
    
    /* 主轴对齐 */
    justify-content: flex-start; /* flex-start, flex-end, center, space-between, space-around, space-evenly */
    
    /* 交叉轴对齐 */
    align-items: stretch;    /* stretch, flex-start, flex-end, center, baseline */
    
    /* 多行对齐 */
    align-content: stretch;  /* stretch, flex-start, flex-end, center, space-between, space-around */
    
    /* 简写 */
    flex-flow: row wrap;
}

/* 项目属性 */
.item {
    /* 排序 */
    order: 1;                /* 数值，越小越靠前 */
    
    /* 放大比例 */
    flex-grow: 0;           /* 默认为0，不放大 */
    
    /* 缩小比例 */
    flex-shrink: 1;         /* 默认为1，空间不足时缩小 */
    
    /* 基础大小 */
    flex-basis: auto;       /* auto, 长度值 */
    
    /* 简写 */
    flex: 1;                /* flex-grow flex-shrink flex-basis */
    
    /* 单独对齐 */
    align-self: auto;       /* auto, flex-start, flex-end, center, baseline, stretch */
}
```

### 3. Grid 网格布局

```
/* 容器属性 */
.container {
    display: grid;          /* 或 inline-grid */
    
    /* 定义列 */
    grid-template-columns: 100px 1fr 2fr;  /* 固定 分数 分数 */
    grid-template-columns: repeat(3, 1fr); /* 重复 */
    grid-template-columns: minmax(100px, 1fr) auto;
    
    /* 定义行 */
    grid-template-rows: 100px auto 100px;
    
    /* 定义区域 */
    grid-template-areas: 
        "header header header"
        "sidebar content content"
        "footer footer footer";
    
    /* 间隙 */
    gap: 10px;              /* 行列间隙 */
    row-gap: 10px;          /* 行间隙 */
    column-gap: 20px;       /* 列间隙 */
    
    /* 自动填充 */
    grid-auto-columns: 100px;  /* 自动创建列的尺寸 */
    grid-auto-rows: 100px;     /* 自动创建行的尺寸 */
    grid-auto-flow: row;       /* row, column, dense */
    
    /* 对齐 */
    justify-items: stretch;    /* 所有项目水平对齐 */
    align-items: stretch;      /* 所有项目垂直对齐 */
    place-items: center;       /* 简写：align justify */
    
    /* 容器对齐 */
    justify-content: start;    /* 网格水平对齐 */
    align-content: start;      /* 网格垂直对齐 */
}

/* 项目属性 */
.item {
    /* 放置位置 */
    grid-column: 1 / 3;        /* 起始线 / 结束线 */
    grid-row: 1 / span 2;      /* 起始线 / 跨几行 */
    
    /* 命名区域 */
    grid-area: header;         /* 对应 template-areas 中的名称 */
    
    /* 对齐单个项目 */
    justify-self: stretch;     /* 水平对齐 */
    align-self: stretch;       /* 垂直对齐 */
    place-self: center;        /* 简写：align justify */
}
```

### 4. 多列布局

```
.container {
    column-count: 3;          /* 列数 */
    column-width: 200px;      /* 列宽 */
    column-gap: 20px;         /* 列间距 */
    column-rule: 1px solid #ccc;  /* 列分隔线 */
    
    /* 跨列 */
    .span-all {
        column-span: all;     /* 横跨所有列 */
    }
}
```

---

## 🎬 CSS 动画与变换

### 1. 过渡效果

```
.element {
    /* 指定属性 */
    transition-property: all;             /* 所有属性 */
    transition-property: width, height;   /* 指定属性 */
    
    /* 持续时间 */
    transition-duration: 0.3s;            /* 秒或毫秒 */
    
    /* 时间函数 */
    transition-timing-function: ease;     /* ease, linear, ease-in, ease-out, ease-in-out */
    transition-timing-function: cubic-bezier(0.42, 0, 0.58, 1);  /* 贝塞尔曲线 */
    
    /* 延迟 */
    transition-delay: 0.5s;
    
    /* 简写 */
    transition: all 0.3s ease 0.5s;
    transition: width 0.5s, height 0.5s 0.1s;
}
```

### 2. 变换

```
.element {
    /* 2D 变换 */
    transform: translate(50px, 100px);    /* 移动 */
    transform: rotate(45deg);             /* 旋转 */
    transform: scale(1.5);                /* 缩放 */
    transform: scaleX(2);                 /* X轴缩放 */
    transform: scaleY(0.5);               /* Y轴缩放 */
    transform: skew(30deg, 20deg);        /* 倾斜 */
    
    /* 3D 变换 */
    transform: translate3d(50px, 100px, 200px);
    transform: rotateX(45deg);
    transform: rotateY(45deg);
    transform: rotateZ(45deg);
    transform: perspective(500px) rotateY(45deg);
    
    /* 变换原点 */
    transform-origin: 50% 50%;            /* 中心点 */
    transform-origin: 0 0;                /* 左上角 */
    
    /* 变换组合 */
    transform: translate(50px, 50px) rotate(45deg) scale(1.2);
}
```

### 3. 关键帧动画

```
/* 定义动画 */
@keyframes slideIn {
    0% {
        transform: translateX(-100%);
        opacity: 0;
    }
    100% {
        transform: translateX(0);
        opacity: 1;
    }
}

@keyframes bounce {
    0%, 20%, 50%, 80%, 100% {
        transform: translateY(0);
    }
    40% {
        transform: translateY(-30px);
    }
    60% {
        transform: translateY(-15px);
    }
}

/* 应用动画 */
.element {
    animation-name: slideIn;          /* 动画名称 */
    animation-duration: 1s;           /* 持续时间 */
    animation-timing-function: ease;  /* 时间函数 */
    animation-delay: 0.5s;            /* 延迟 */
    animation-iteration-count: 3;     /* 次数：数值或 infinite */
    animation-direction: normal;      /* normal, reverse, alternate, alternate-reverse */
    animation-fill-mode: forwards;    /* none, forwards, backwards, both */
    animation-play-state: running;    /* running, paused */
    
    /* 简写 */
    animation: slideIn 1s ease 0.5s 3 normal forwards;
}
```

---

## 📱 响应式设计

### 1. 媒体查询

```
/* 最大宽度 */
@media (max-width: 768px) {
    .container { width: 100%; }
}

/* 最小宽度 */
@media (min-width: 769px) {
    .container { width: 750px; }
}

/* 范围查询 */
@media (min-width: 768px) and (max-width: 1024px) {
    .container { width: 90%; }
}

/* 设备方向 */
@media (orientation: portrait) {  /* 竖屏 */
    .sidebar { display: none; }
}

@media (orientation: landscape) { /* 横屏 */
    .sidebar { width: 30%; }
}

/* 屏幕特性 */
@media (hover: hover) {  /* 支持悬停的设备 */
    .button:hover { background: blue; }
}

@media (prefers-color-scheme: dark) {  /* 深色模式 */
    body { background: #222; color: #fff; }
}

@media (prefers-reduced-motion: reduce) {  /* 减少动画 */
    * { animation-duration: 0.01ms !important; }
}
```

### 2. 相对单位

```
.element {
    /* 视口单位 */
    width: 100vw;           /* 视口宽度的 100% */
    height: 100vh;          /* 视口高度的 100% */
    font-size: 5vw;         /* 视口宽度的 5% */
    
    /* 相对字体单位 */
    font-size: 1rem;        /* 相对于根元素字体大小 */
    font-size: 1em;         /* 相对于父元素字体大小 */
    
    /* 百分比 */
    width: 50%;             /* 相对于父元素宽度 */
    height: 50%;            /* 相对于父元素高度 */
    
    /* 最小/最大函数 */
    width: min(100%, 500px);   /* 取较小值 */
    width: max(300px, 50%);    /* 取较大值 */
    width: clamp(300px, 50%, 800px);  /* 最小 理想 最大 */
}
```

### 3. 响应式图片

```
img {
    max-width: 100%;        /* 图片最大宽度为容器宽度 */
    height: auto;           /* 高度自动，保持比例 */
}

.responsive-bg {
    background-image: url("small.jpg");
    
    @media (min-width: 768px) {
        background-image: url("medium.jpg");
    }
    
    @media (min-width: 1200px) {
        background-image: url("large.jpg");
    }
}
```

---

## 💡 CSS 现代特性

### 1. CSS 变量

```
:root {
    /* 定义变量 */
    --primary-color: #3498db;
    --secondary-color: #2ecc71;
    --font-size: 16px;
    --spacing: 20px;
    --border-radius: 8px;
    
    /* 主题变量 */
    --bg-color: #fff;
    --text-color: #333;
}

body {
    /* 使用变量 */
    color: var(--text-color);
    background-color: var(--bg-color);
    font-size: var(--font-size);
}

.button {
    background-color: var(--primary-color);
    padding: calc(var(--spacing) / 2) var(--spacing);
    border-radius: var(--border-radius);
}

/* 变量回退值 */
.element {
    color: var(--custom-color, #000);  /* 如果 custom-color 未定义，使用 #000 */
}

/* 在 JavaScript 中修改变量 */
document.documentElement.style.setProperty('--primary-color', '#ff0000');
```

### 2. CSS 函数

```
/* 计算函数 */
.element {
    width: calc(100% - 100px);
    height: calc(50vh + 20px);
    font-size: calc(1rem + 1vw);
}

/* 自定义属性计算 */
:root {
    --base-size: 16px;
}

.element {
    font-size: calc(var(--base-size) * 1.5);
}

/* 渐变函数 */
.background {
    background: linear-gradient(45deg, red, yellow);
    background: radial-gradient(circle at center, red, blue);
    background: conic-gradient(red, yellow, green, blue, red);
}

/* 滤镜 */
.filtered {
    filter: blur(5px);
    filter: brightness(150%);
    filter: contrast(200%);
    filter: grayscale(100%);
    filter: hue-rotate(90deg);
    filter: opacity(50%);
    filter: saturate(200%);
    filter: sepia(100%);
    filter: drop-shadow(5px 5px 5px rgba(0,0,0,0.5));
    
    /* 多重滤镜 */
    filter: grayscale(50%) blur(2px) brightness(120%);
}
```

### 3. 逻辑属性

```
/* 支持 RTL（从右到左）布局 */
.element {
    /* 物理属性 */
    margin-left: 10px;
    padding-right: 20px;
    
    /* 逻辑属性 */
    margin-inline-start: 10px;  /* 左（LTR）或 右（RTL） */
    padding-inline-end: 20px;   /* 右（LTR）或 左（RTL） */
    
    /* 尺寸 */
    block-size: 100px;      /* 垂直尺寸（height） */
    inline-size: 200px;     /* 水平尺寸（width） */
    
    /* 文本对齐 */
    text-align: start;      /* 开始位置（LTR:左，RTL:右） */
    text-align: end;        /* 结束位置（LTR:右，RTL:左） */
}
```

---

## 🎨 CSS 最佳实践

### 1. 组织代码结构

```
/* 1. 重置和基本样式 */
* { box-sizing: border-box; }
body { font-family: sans-serif; }

/* 2. 变量定义 */
:root { --primary-color: #007bff; }

/* 3. 布局类 */
.container { width: 100%; max-width: 1200px; margin: 0 auto; }

/* 4. 通用组件 */
.button { padding: 10px 20px; border-radius: 4px; }

/* 5. 页面特定样式 */
.header { background: var(--primary-color); }

/* 6. 辅助类 */
.text-center { text-align: center; }
.mb-20 { margin-bottom: 20px; }

/* 7. 媒体查询 */
@media (max-width: 768px) { .container { padding: 0 15px; } }
```

### 2. BEM 命名规范

```
/* Block - 独立且有意义的组件 */
.button { }
.menu { }

/* Element - 块的一部分 */
.button__icon { }
.menu__item { }

/* Modifier - 块或元素的变化 */
.button--primary { }
.button--large { }
.menu__item--active { }
```

### 3. 性能优化

```
/* 避免昂贵的属性 */
.element {
    /* 避免在动画中使用这些属性 */
    width: 100px;           /* 重排 */
    height: 100px;          /* 重排 */
    margin: 10px;           /* 重排 */
    
    /* 使用这些属性替代 */
    transform: scale(1.1);  /* 重绘，性能更好 */
    opacity: 0.8;           /* 重绘 */
}

/* 使用 will-change 提示浏览器 */
.animated {
    will-change: transform, opacity;
}

/* 减少选择器复杂度 */
/* 不好 */
div.container ul.list li.item a.link { }
/* 好 */
.link { }
```

### 4. 可访问性

```
/* 焦点样式 */
a:focus, button:focus, input:focus {
    outline: 2px solid #0066cc;
    outline-offset: 2px;
}

/* 减少运动 */
@media (prefers-reduced-motion: reduce) {
    * {
        animation-duration: 0.01ms !important;
        transition-duration: 0.01ms !important;
    }
}

/* 高对比度 */
@media (prefers-contrast: high) {
    .button {
        border: 2px solid currentColor;
    }
}
```

---

## 📊 CSS 单位总结

|单位|描述|示例|
|---|---|---|
|**px**​|像素|`font-size: 16px`|
|**%**​|百分比|`width: 50%`|
|**em**​|相对于父元素字体大小|`margin: 2em`|
|**rem**​|相对于根元素字体大小|`font-size: 1.5rem`|
|**vw**​|视口宽度的 1%|`width: 50vw`|
|**vh**​|视口高度的 1%|`height: 100vh`|
|**vmin**​|视口较小尺寸的 1%|`font-size: 5vmin`|
|**vmax**​|视口较大尺寸的 1%|`font-size: 5vmax`|
|**ch**​|数字"0"的宽度|`width: 20ch`|
|**ex**​|当前字体的小写"x"高度|`height: 2ex`|
|**deg**​|角度|`transform: rotate(45deg)`|
|**turn**​|圈数|`transform: rotate(0.25turn)`|

---

## 🎯 实战示例

### 1. 卡片组件

```
.card {
    --card-padding: 20px;
    --card-radius: 8px;
    --card-shadow: 0 4px 6px rgba(0,0,0,0.1);
    
    background: white;
    border-radius: var(--card-radius);
    box-shadow: var(--card-shadow);
    overflow: hidden;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.card:hover {
    transform: translateY(-5px);
    box-shadow: 0 6px 12px rgba(0,0,0,0.15);
}

.card__image {
    width: 100%;
    height: 200px;
    object-fit: cover;
}

.card__content {
    padding: var(--card-padding);
}

.card__title {
    margin: 0 0 10px 0;
    font-size: 1.25rem;
    font-weight: 600;
}

.card__description {
    color: #666;
    line-height: 1.5;
    margin-bottom: 15px;
}

.card__button {
    display: inline-block;
    background: var(--primary-color, #0066cc);
    color: white;
    padding: 8px 16px;
    border-radius: 4px;
    text-decoration: none;
    transition: background 0.3s ease;
}

.card__button:hover {
    background: var(--primary-dark, #0052a3);
}
```

### 2. 响应式导航

```
.navbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1rem 2rem;
    background: #333;
    color: white;
}

.navbar__brand {
    font-size: 1.5rem;
    font-weight: bold;
}

.navbar__menu {
    display: flex;
    list-style: none;
    gap: 2rem;
}

.navbar__link {
    color: white;
    text-decoration: none;
    padding: 0.5rem 1rem;
    border-radius: 4px;
    transition: background 0.3s ease;
}

.navbar__link:hover {
    background: rgba(255,255,255,0.1);
}

.navbar__toggle {
    display: none;
    background: none;
    border: none;
    color: white;
    font-size: 1.5rem;
    cursor: pointer;
}

@media (max-width: 768px) {
    .navbar__menu {
        position: fixed;
        top: 0;
        right: -100%;
        height: 100vh;
        width: 80%;
        max-width: 300px;
        background: #333;
        flex-direction: column;
        padding: 2rem;
        transition: right 0.3s ease;
    }
    
    .navbar__menu.active {
        right: 0;
    }
    
    .navbar__toggle {
        display: block;
    }
}
```

---

## 📋 总结

### CSS 核心概念

1. **选择器**：精准定位要样式化的元素
    
2. **盒模型**：理解元素如何在页面上占据空间
    
3. **布局**：掌握 Flexbox 和 Grid 等现代布局技术
    
4. **响应式**：创建适应各种设备的网页
    
5. **动画**：增强用户体验的动态效果
    

### 学习路径建议

1. **基础阶段**：选择器、盒模型、常用属性
    
2. **布局阶段**：Flexbox、Grid、定位
    
3. **进阶阶段**：动画、变换、响应式设计
    
4. **高级阶段**：CSS 变量、函数、现代特性
    
5. **最佳实践**：性能优化、可访问性、代码组织
    

### 工具推荐

- **浏览器开发者工具**：Chrome DevTools、Firefox Developer Tools
    
- **CSS 验证器**：W3C CSS Validator
    
- **预处理器**：Sass、Less
    
- **构建工具**：PostCSS、Autoprefixer
    
- **在线编辑器**：CodePen、JSFiddle
    

掌握 CSS 需要不断的实践和探索。从简单的样式调整开始，逐步构建复杂的布局和交互效果，最终能够创建出美观、响应式、高性能的网页界面。