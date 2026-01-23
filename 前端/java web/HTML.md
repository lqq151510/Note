# HTML 超文本标记语言详解

## 📋 HTML 概述

**HTML**（HyperText Markup Language，超文本标记语言）是构建网页的标准标记语言。它通过标签来描述网页的结构和内容，是 Web 开发的基石。

### 核心特点：

- **标记语言**：不是编程语言，用于定义内容结构
    
- **平台无关**：可在任何设备和浏览器上显示
    
- **标签结构**：使用尖括号包围的标签定义元素
    
- **超文本**：支持链接到其他文档或资源
    

---

## 📁 HTML 文档结构

### 基本骨架

```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>页面标题</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <!-- 页面内容在这里 -->
    <h1>欢迎来到我的网站</h1>
    <p>这是一个段落。</p>
    <script src="script.js"></script>
</body>
</html>
```

### 各部分说明

|部分|描述|必需性|
|---|---|---|
|`<!DOCTYPE html>`|声明文档类型为 HTML5|必需|
|`<html>`|根元素，包裹整个文档|必需|
|`<head>`|文档头，包含元数据和链接|必需|
|`<body>`|文档主体，包含可见内容|必需|
|`<title>`|浏览器标签页标题|必需|

---

## 📊 HTML 标签分类详解

## 🔤 文本与标题标签

### 标题标签

```
<h1>一级标题 - 最重要，通常一个页面一个</h1>
<h2>二级标题</h2>
<h3>三级标题</h3>
<h4>四级标题</h4>
<h5>五级标题</h5>
<h6>六级标题 - 最不重要</h6>
```

**SEO建议**：`<h1>`应包含页面主要关键词，标题层级要合理。

### 段落与换行

```
<p>这是一个段落。HTML 会自动在段落前后添加空白。</p>
<p>这是另一个段落。</p>

<p>第一行<br>  <!-- 强制换行 -->
第二行</p>

<hr>  <!-- 水平分隔线 -->
```

### 文本格式化

```
<b>粗体文本（无强调含义）</b>
<strong>重要文本（有语义，浏览器会强调）</strong>

<i>斜体文本（无强调含义）</i>
<em>强调文本（有语义）</em>

<u>下划线文本</u>
<s>删除线文本</s>
<del>被删除的内容</del>
<ins>新插入的内容</ins>

<small>小号文本</small>
<mark>高亮标记文本</mark>

<sup>上标</sup>  <!-- 如 x² -->
<sub>下标</sub>  <!-- 如 H₂O -->

<code>代码片段</code>
<kbd>键盘输入</kbd>
<samp>程序输出示例</samp>
<var>变量名</var>

<abbr title="HyperText Markup Language">HTML</abbr>  <!-- 缩写 -->
<cite>引用标题</cite>
<blockquote>长引用</blockquote>
<q>短引用</q>

<address>联系信息</address>
<time datetime="2024-01-15">2024年1月15日</time>
```

### 预格式化文本

```
<pre>
    function hello() {
        console.log("Hello World!");
    }
</pre>
```

---

## 🔗 链接与多媒体标签

### 超链接

```
<!-- 外部链接 -->
<a href="https://example.com" target="_blank" rel="noopener noreferrer">
    访问示例网站
</a>

<!-- 内部链接 -->
<a href="#section-id">跳转到章节</a>
<a href="about.html">关于我们</a>

<!-- 邮件链接 -->
<a href="mailto:contact@example.com">发送邮件</a>

<!-- 电话链接（移动端） -->
<a href="tel:+8613800138000">打电话</a>

<!-- 下载链接 -->
<a href="document.pdf" download>下载PDF</a>
```

**属性说明**：

- `target="_blank"`：在新标签页打开
    
- `rel="noopener noreferrer"`：安全建议，防止恶意网站攻击
    
- `title="描述"`：鼠标悬停提示
    

### 图片

```
<!-- 基本用法 -->
<img src="image.jpg" alt="图片描述" width="300" height="200">

<!-- 响应式图片 -->
<img src="image.jpg" alt="描述" srcset="small.jpg 480w, medium.jpg 768w, large.jpg 1200w"
     sizes="(max-width: 600px) 480px, (max-width: 1200px) 768px, 1200px">

<!-- 带标题的图片 -->
<figure>
    <img src="image.jpg" alt="美丽的风景">
    <figcaption>这是一张风景照片</figcaption>
</figure>
```

**最佳实践**：

- 始终提供 `alt`属性（可访问性要求）
    
- 指定宽高防止布局偏移
    
- 使用 WebP 等现代格式优化性能
    

### 音频与视频

```
<!-- 音频 -->
<audio controls>
    <source src="audio.mp3" type="audio/mpeg">
    <source src="audio.ogg" type="audio/ogg">
    您的浏览器不支持音频元素。
</audio>

<!-- 视频 -->
<video controls width="640" height="360" poster="preview.jpg">
    <source src="video.mp4" type="video/mp4">
    <source src="video.webm" type="video/webm">
    <track src="subtitles.vtt" kind="subtitles" srclang="zh" label="中文">
    您的浏览器不支持视频元素。
</video>
```

**属性**：

- `controls`：显示播放控件
    
- `autoplay`：自动播放
    
- `loop`：循环播放
    
- `muted`：静音
    
- `preload`：预加载策略
    

---

## 📋 列表标签

### 无序列表

```
<ul>
    <li>列表项1</li>
    <li>列表项2
        <ul>
            <li>嵌套子项1</li>
            <li>嵌套子项2</li>
        </ul>
    </li>
    <li>列表项3</li>
</ul>
```

### 有序列表

```
<ol>
    <li>第一步</li>
    <li>第二步
        <ol type="A">  <!-- 类型：1, A, a, I, i -->
            <li>子步骤A</li>
            <li>子步骤B</li>
        </ol>
    </li>
    <li>第三步</li>
</ol>

<ol start="10" reversed>  <!-- 从10开始，倒序 -->
    <li>第十项</li>
    <li>第九项</li>
</ol>
```

### 描述列表

```
<dl>
    <dt>HTML</dt>
    <dd>超文本标记语言，用于创建网页结构</dd>
    
    <dt>CSS</dt>
    <dd>层叠样式表，用于设计网页样式</dd>
    
    <dt>JavaScript</dt>
    <dd>脚本语言，用于实现网页交互</dd>
</dl>
```

---

## 📊 表格标签

### 基本表格

```
<table border="1">
    <caption>学生成绩表</caption>
    <thead>
        <tr>
            <th>姓名</th>
            <th>语文</th>
            <th>数学</th>
            <th>总分</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>张三</td>
            <td>85</td>
            <td>90</td>
            <td>175</td>
        </tr>
        <tr>
            <td>李四</td>
            <td>92</td>
            <td>88</td>
            <td>180</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td>平均分</td>
            <td>88.5</td>
            <td>89</td>
            <td>177.5</td>
        </tr>
    </tfoot>
</table>
```

### 复杂表格

```
<table>
    <!-- 列分组 -->
    <colgroup>
        <col span="2" style="background-color:#f2f2f2">
        <col style="background-color:#e6f7ff">
    </colgroup>
    
    <!-- 跨行跨列 -->
    <tr>
        <th rowspan="2">学期</th>
        <th colspan="2">成绩</th>
    </tr>
    <tr>
        <th>期中</th>
        <th>期末</th>
    </tr>
    
    <!-- 表格主体 -->
    <tr>
        <td>2023秋季</td>
        <td>85</td>
        <td>90</td>
    </tr>
</table>
```

**语义化表格**：

- `<thead>`：表头
    
- `<tbody>`：表体
    
- `<tfoot>`：表脚
    
- `<th>`：表头单元格
    
- `<td>`：数据单元格
    
- `colspan`：跨列
    
- `rowspan`：跨行
    

---

## 📝 表单标签

### 基本表单结构

```
<form action="/submit" method="POST" enctype="multipart/form-data">
    <!-- 文本输入 -->
    <label for="username">用户名：</label>
    <input type="text" id="username" name="username" required
           placeholder="请输入用户名" minlength="3" maxlength="20">
    
    <!-- 密码 -->
    <label for="password">密码：</label>
    <input type="password" id="password" name="password" required>
    
    <!-- 邮箱 -->
    <label for="email">邮箱：</label>
    <input type="email" id="email" name="email" required>
    
    <!-- 数字 -->
    <label for="age">年龄：</label>
    <input type="number" id="age" name="age" min="1" max="120" step="1">
    
    <!-- 日期时间 -->
    <label for="birthday">生日：</label>
    <input type="date" id="birthday" name="birthday">
    
    <!-- 单选按钮 -->
    <fieldset>
        <legend>性别</legend>
        <label>
            <input type="radio" name="gender" value="male" checked>
            男
        </label>
        <label>
            <input type="radio" name="gender" value="female">
            女
        </label>
    </fieldset>
    
    <!-- 复选框 -->
    <fieldset>
        <legend>爱好</legend>
        <label>
            <input type="checkbox" name="hobby" value="reading">
            阅读
        </label>
        <label>
            <input type="checkbox" name="hobby" value="sports">
            运动
        </label>
    </fieldset>
    
    <!-- 下拉选择 -->
    <label for="city">城市：</label>
    <select id="city" name="city">
        <option value="">请选择</option>
        <option value="beijing">北京</option>
        <option value="shanghai">上海</option>
        <optgroup label="广东省">
            <option value="guangzhou">广州</option>
            <option value="shenzhen">深圳</option>
        </optgroup>
    </select>
    
    <!-- 多行文本 -->
    <label for="message">留言：</label>
    <textarea id="message" name="message" rows="4" cols="50"
              placeholder="请输入您的留言"></textarea>
    
    <!-- 文件上传 -->
    <label for="file">上传文件：</label>
    <input type="file" id="file" name="file" accept=".jpg,.png,.pdf">
    
    <!-- 范围滑块 -->
    <label for="volume">音量：</label>
    <input type="range" id="volume" name="volume" min="0" max="100" value="50">
    
    <!-- 颜色选择 -->
    <label for="color">选择颜色：</label>
    <input type="color" id="color" name="color" value="#ff0000">
    
    <!-- 按钮 -->
    <button type="submit">提交</button>
    <button type="reset">重置</button>
    <button type="button">普通按钮</button>
</form>
```

### 新输入类型（HTML5）

```
<!-- 搜索框 -->
<input type="search" placeholder="搜索...">

<!-- URL -->
<input type="url" placeholder="https://example.com">

<!-- 电话 -->
<input type="tel" pattern="[0-9]{3}-[0-9]{8}">

<!-- 数字 -->
<input type="number" min="0" max="100" step="5">

<!-- 日期时间 -->
<input type="datetime-local">
<input type="month">
<input type="week">
<input type="time">

<!-- 数据列表 -->
<input list="browsers" name="browser">
<datalist id="browsers">
    <option value="Chrome">
    <option value="Firefox">
    <option value="Edge">
</datalist>
```

---

## 🏗️ 布局与语义化标签

### 传统布局（div）

```
<div class="container">
    <div class="header">页眉</div>
    <div class="nav">导航</div>
    <div class="main">
        <div class="content">主要内容</div>
        <div class="sidebar">侧边栏</div>
    </div>
    <div class="footer">页脚</div>
</div>
```

### 语义化布局（HTML5）

```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>语义化网页</title>
</head>
<body>
    <!-- 页眉 -->
    <header>
        <h1>网站标题</h1>
        <nav>
            <ul>
                <li><a href="#home">首页</a></li>
                <li><a href="#about">关于</a></li>
            </ul>
        </nav>
    </header>
    
    <!-- 主导航 -->
    <nav aria-label="主导航">
        <!-- 导航内容 -->
    </nav>
    
    <!-- 主要内容 -->
    <main>
        <!-- 文章区域 -->
        <article>
            <header>
                <h2>文章标题</h2>
                <p>发布时间：<time datetime="2024-01-15">2024年1月15日</time></p>
            </header>
            
            <section>
                <h3>章节标题</h3>
                <p>章节内容...</p>
            </section>
            
            <footer>
                <p>标签：<mark>HTML</mark>, <mark>CSS</mark></p>
            </footer>
        </article>
        
        <!-- 侧边栏 -->
        <aside>
            <h3>相关文章</h3>
            <ul>
                <li><a href="#">HTML5 新特性</a></li>
            </ul>
        </aside>
    </main>
    
    <!-- 页脚 -->
    <footer>
        <p>&copy; 2024 我的网站</p>
        <address>
            联系：<a href="mailto:contact@example.com">contact@example.com</a>
        </address>
    </footer>
</body>
</html>
```

### 语义化标签说明

|标签|描述|使用场景|
|---|---|---|
|`<header>`|页眉或区块头部|网站标题、Logo、导航|
|`<nav>`|导航区域|主导航、面包屑导航|
|`<main>`|主要内容|页面唯一主体内容|
|`<article>`|独立内容|博客文章、新闻、帖子|
|`<section>`|文档分区|章节、标签页内容|
|`<aside>`|侧边栏|相关链接、广告、引用|
|`<footer>`|页脚|版权信息、联系方式|
|`<figure>`|配图|图片、图表、代码示例|
|`<figcaption>`|配图说明|为 `<figure>`添加标题|
|`<time>`|时间日期|发布日期、事件时间|

---

## 🔧 元信息与脚本标签

### 头部元信息

```
<head>
    <!-- 字符编码 -->
    <meta charset="UTF-8">
    
    <!-- 视口设置（响应式必需） -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    <!-- 页面描述（SEO重要） -->
    <meta name="description" content="页面描述，不超过150字">
    
    <!-- 关键词（现代SEO作用减小） -->
    <meta name="keywords" content="关键词1, 关键词2">
    
    <!-- 作者 -->
    <meta name="author" content="作者名">
    
    <!-- Open Graph 协议（社交媒体分享） -->
    <meta property="og:title" content="页面标题">
    <meta property="og:description" content="页面描述">
    <meta property="og:image" content="https://example.com/image.jpg">
    <meta property="og:url" content="https://example.com/page">
    
    <!-- 网站图标 -->
    <link rel="icon" href="/favicon.ico" type="image/x-icon">
    <link rel="apple-touch-icon" href="/apple-touch-icon.png">
    
    <!-- 样式表 -->
    <link rel="stylesheet" href="styles.css">
    
    <!-- 预加载重要资源 -->
    <link rel="preload" href="font.woff2" as="font" type="font/woff2" crossorigin>
    
    <!-- 标题 -->
    <title>页面标题 - 网站名称</title>
</head>
```

### 脚本标签

```
<!-- 内部脚本 -->
<script>
    console.log("Hello World!");
</script>

<!-- 外部脚本 -->
<script src="script.js" defer></script>

<!-- 异步加载 -->
<script src="analytics.js" async></script>

<!-- 模块 -->
<script type="module" src="app.js"></script>

<!-- 内联事件处理器（不推荐） -->
<button onclick="alert('点击')">点击我</button>
```

**最佳实践**：

- 使用 `defer`延迟执行，不阻塞页面解析
    
- 使用 `async`异步加载，不保证执行顺序
    
- 模块脚本默认 defer
    
- 避免内联 JavaScript
    

---

## 🎯 特殊功能标签

### 框架相关

```
<!-- 内联框架 -->
<iframe src="https://example.com" 
        title="示例网站"
        width="800" 
        height="600"
        loading="lazy"
        sandbox="allow-same-origin allow-scripts">
    您的浏览器不支持 iframe
</iframe>

<!-- 画布 -->
<canvas id="myCanvas" width="200" height="100">
    您的浏览器不支持 Canvas
</canvas>
<script>
    const canvas = document.getElementById('myCanvas');
    const ctx = canvas.getContext('2d');
    ctx.fillStyle = 'red';
    ctx.fillRect(0, 0, 100, 100);
</script>

<!-- SVG -->
<svg width="100" height="100">
    <circle cx="50" cy="50" r="40" fill="blue" />
</svg>
```

### 细节与摘要

```
<details>
    <summary>点击查看更多</summary>
    <p>这里是隐藏的详细内容。</p>
</details>
```

### 进度与度量

```
<progress value="70" max="100">70%</progress>
<meter value="6" min="0" max="10" low="3" high="8" optimum="5">6/10</meter>
```

---

## 💡 HTML 最佳实践

### 1. 语义化优先

```
<!-- 不推荐 -->
<div id="header">...</div>
<div class="nav">...</div>

<!-- 推荐 -->
<header>...</header>
<nav>...</nav>
```

### 2. 可访问性

```
<!-- 为图片添加替代文本 -->
<img src="photo.jpg" alt="描述图片内容">

<!-- 为表单元素添加标签 -->
<label for="name">姓名：</label>
<input type="text" id="name">

<!-- 使用语义化标题结构 -->
<h1>主标题</h1>
<h2>副标题</h2>
<h3>子标题</h3>

<!-- 添加ARIA属性 -->
<button aria-label="关闭菜单">×</button>
```

### 3. 性能优化

```
<!-- 懒加载图片 -->
<img src="image.jpg" loading="lazy" alt="...">

<!-- 预加载关键资源 -->
<link rel="preload" href="critical.css" as="style">

<!-- 使用现代图片格式 -->
<picture>
    <source srcset="image.avif" type="image/avif">
    <source srcset="image.webp" type="image/webp">
    <img src="image.jpg" alt="...">
</picture>
```

### 4. SEO优化

```
<!-- 正确的标题结构 -->
<h1>页面主标题</h1>
<h2>章节标题</h2>

<!-- 语义化标记 -->
<article>
    <h2>文章标题</h2>
    <p>文章内容...</p>
</article>

<!-- 结构化数据 -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "文章标题",
  "datePublished": "2024-01-15"
}
</script>
```

---

## 📊 HTML 标签速查表

|类别|常用标签|说明|
|---|---|---|
|**结构**​|`<!DOCTYPE>`, `<html>`, `<head>`, `<body>`|文档骨架|
|**元信息**​|`<title>`, `<meta>`, `<link>`, `<style>`|头部信息|
|**文本**​|`<h1>-<h6>`, `<p>`, `<br>`, `<hr>`|文本内容|
|**格式化**​|`<strong>`, `<em>`, `<mark>`, `<code>`|文本样式|
|**链接**​|`<a>`, `<link>`|超链接|
|**图片**​|`<img>`, `<picture>`, `<figure>`|图像媒体|
|**列表**​|`<ul>`, `<ol>`, `<li>`, `<dl>`|列表结构|
|**表格**​|`<table>`, `<tr>`, `<td>`, `<th>`|表格数据|
|**表单**​|`<form>`, `<input>`, `<button>`, `<select>`|用户输入|
|**布局**​|`<div>`, `<span>`, `<header>`, `<footer>`|页面布局|
|**语义**​|`<article>`, `<section>`, `<nav>`, `<aside>`|语义化标签|
|**媒体**​|`<audio>`, `<video>`, `<source>`|音视频|
|**脚本**​|`<script>`, `<noscript>`|脚本代码|
|**嵌入**​|`<iframe>`, `<canvas>`, `<svg>`|嵌入内容|

---

## 🎯 总结

### 核心原则

1. **语义化**：使用合适的标签表达内容含义
    
2. **可访问性**：确保所有用户都能访问内容
    
3. **响应式**：适应不同设备和屏幕尺寸
    
4. **SEO友好**：方便搜索引擎理解和收录
    
5. **性能优化**：减少加载时间，提升用户体验
    

### 学习路径

1. **基础阶段**：掌握常用标签和属性
    
2. **进阶阶段**：理解语义化和可访问性
    
3. **高级阶段**：掌握性能优化和最佳实践
    
4. **实战阶段**：构建符合标准的网页项目
    

### 工具推荐

- **验证工具**：W3C Validator
    
- **编辑器**：VS Code + HTML 插件
    
- **调试工具**：浏览器开发者工具
    
- **学习资源**：MDN Web Docs
    

HTML 是 Web 开发的基石，掌握好 HTML 是成为优秀前端开发者的第一步。不断实践，结合实际项目，才能真正掌握 HTML 的精髓。