### HTML简介

就其核心而言，[HTML](https://developer.mozilla.org/zh-CN/docs/Glossary/HTML) 是一种相当简单的、由不同[元素](https://developer.mozilla.org/zh-CN/docs/Glossary/Element)组成的标记语言，它可以被应用于文本片段，使文本在文档中具有不同的含义（它是段落吗？它是项目列表吗？它是表格吗？），将文档结构化为逻辑块（文档是否有头部？有三列内容？有一个导航菜单？），并且可以将图片，影像等内容嵌入到页面中。本模块将介绍前两个用途，并且介绍一些 HTML 的基本概念和语法。

### 开始学习HTML

#### 什么是HTML

**HTML**（HyperText Markup Language，超文本标记语言）是一种用来告知浏览器如何组织页面的*标记语言*。HTML 可复杂、可简单，一切取决于 web 开发者。HTML 由一系列的**元素**组成，这些元素可以用来包围或*标记*不同部分的内容，使其以某种方式呈现或者工作。两端的**标签**可以使内容变成超链接，以连接到另一个页面；使字体表现为斜体等。例如，考虑如下内容：

```html
My cat is very grumpy
```

如果我们想要将这行文字单独呈现，可以将这行文字封装成一个段落（Paragraph）**<p>**元素：

```html
<p>My cat is very grumpy</p>
```

