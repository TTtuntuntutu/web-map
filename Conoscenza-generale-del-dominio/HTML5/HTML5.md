## 简单了解

[Introduction to HTML5](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5/Introduction_to_HTML5)：简单地做了引入

1. `<!DOCTYPE html>` 标记这是一个HTML5文档
2. `<meta charset="UTF-8">` 指定UTF-8编码：[Unicode 和 UTF-8 有什么区别](https://www.zhihu.com/question/23374078)
3. 新的HTML5解析器：老的解析器缺乏对错误情况处理，导致不同浏览器行为不一致，新的解析器加了对错误情况的定义 （渲染引擎基于新的HTML5解析器）





## 语义化标签

通过使用语义化标签来表达语义化，但不是强制的。



它有哪些元素?



[Using HTML sections and outlines](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Using_HTML_sections_and_outlines)

Section Elements：

- `<nav>` ：提供导航链接，常见示例是菜单、目录、索引
  - 不用把所有链接都放在`<nav>`标签内，`<nav>`标签应该专注**主要的导航链接块**，比如一般`<footer>`元素内会有链接而不用包`<nav>`标签
  - 一个文档可以**有多个`<nav>`标签**，比如一个是站点导航、一个是页面内导航：[demo](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Heading_Elements#Labeling_section_content) ；**但是不可以嵌套**
  - 用户代理（screen reader），例如针对**残疾用户**的屏幕阅读器，使用此元素来确定**是否在首次呈现忽略导航**

- `<article>`：一条**独立的、完整的**内容
  - 一般会有一个`<h1>-<h6>`来标记
  - `<article>` 可以嵌套，但是内部的`<artilce>`是和外部的`<article>`是相关的，比如一个post下的评论
  - 可以用一个`<address>`在`<artilce>`增加作者信息，但是内嵌的就不可以用

- `<section>`：**独立的一节**
  - 包含标题，一般也是由`<h1>-<h6>` 来标记
  - **对比`<article>`：**如果`<section>`的内容联合起来是一个完整的个体，用`<article>`代替`<section>`
  - **对比`div`**：不将`<section>`作为一个通用的容器，尤其是仅仅加个容器来添加样式，`<section>`应该在逻辑上出现在文档的轮廓中
- `<aside>` ：是文档的一部分，但是是与文档主要内容间接相关的，**比如广告、注释**
  - 不在`<aside>`内嵌`<aside>`



Other Semantic HTML elements used in Sectioning：

- `<body>`：包含所有内容
- `<header>`
  - 定义一个页面区域，通常包含标题、logo、导航
  - 除了在`<body>`内，也可以在`<article>`、`<aside>`、`<section>`、`<nav>`
  - 虽然叫做header，但在视觉效果上不一定要在头部
- `<footer>`
  - 定义一个页面底部，通常是版权、法律声明、链接等
  - 除了在`<body>`内，也可以在`<article>`、`<aside>`、`<section>`、`<nav>`
  - 虽然叫做footer，但在视觉效果上但不一定要在底部



Form相关：

`<output>`元素：容器元素，网站或应用程序可以在其中插入计算结果或用户操作的结果



其他：

- `<figure>` ：还是一个比较有意思的元素，表示一个独立、完整的内容，可能是带一个标题的（用`<figcaption>`）

- `<main>`：表示文档`<body>`的主体部分

  - 为什么`<main>`会被归在这里呢？而不是在Sections

  - doesn't contribute to the document's outline

    

## 其他标签

扫了一下 [html-cheat-sheet](https://hostingcanada.org/html-cheat-sheet/) ，额外再补充一些标签。



`<pre>` ： 预格式化文本，比如下面这样是保留空格和换行的

```html
  <pre>
    L          TE
      A       A
        C    V
         R A
         DOU
         LOU
        REUSE
        QUE TU
        PORTES
      ET QUI T'
      ORNE O CI
       VILISÉ
      OTE-  TU VEUX
       LA    BIEN
      SI      RESPI
              RER       - Apollinaire
  </pre>
```



`<dl>` + `<dt>` + `<dd>`  ： 术语+解释的块

```html
<dl>
  <dt>Hostingcanada.org</dt>
  <dd>Helps you find the best tools for running a small business website</dd>
</dl>
```



`<details>` ： 展示详情小部件，点击三角形展开

```html
<details>
    <summary>Details</summary>
    Something small enough to escape casual notice.
</details>
```



`<dialog>` ：弹窗

当前工程化开发，是用的z-index来实现层级差异，从而展示弹窗。

以后会不会基于原生的`<dialog>` 去做。



`<meter>` 、`<progress>` 

原生的HTML元素来展示进度条。



## content categories: 标签分类

每个元素可能处于0、1、n个内容分类中，在同一个内容分类中的元素享有相同的行为、受相关规则限制。



![HTML标签分类](../Immagine/HTML标签分类.png)



有3大分类：

- Main content categories
  - Metadata content：描述页面的元数据，比如 `<link>`、`<meta>`、`<script>`、`<style>`、`<title>`等；
  - Flow content：通常包括文本或者嵌入式内容，比较细节的内容；
  - Sectioning content：比如`<article>`、`<aside>`、`<nav>`、`<section>`，下面详细解释
  - Heading content：定义标题，比如`h1~h6`、`<hgroup>`
  - phrasing content：文本、标记，对标HTML4的inline content
  - Embedded content：嵌入的内容会导入另一种资源，比如`<canvas>`、`<iframe>`等
  - Interactive content：和用户交互相关，比如`<button>`、`<a>`等
  - Palpable content：可感知的节点，渲染出来的
- Form-related content categories
- Specific content categories, sometimes only in a specific context



### Sectioning content

和[规范中的outline](https://html.spec.whatwg.org/multipage/sections.html#outline)关联，outline指的是大纲，而Sectioning content的作用是会在这个大纲中创立一个节点。这个节点在整个文档中是有实际意义的，它也定义了`<header>`、`<footer>` 、heading content的范围。它包括`<article>`、`<aside>`、`<nav>`、`<section>`，比如：

```html
<body>
   <nav>
    <p><a href="/">Home</a></p>
   </nav>
   <p>Hello world.</p>
   <aside>
    <p>My cat is cute.</p>
   </aside>
</body>
```

建立的大纲就是：

1. Untitled document
   1. Navigation
   2. Sidebar

这个建立的大纲就会被许多可获取设备、浏览器提供的reader views用到。

**但是：这一点并没有得到实现**

> **Important**: There are no implementations of the proposed outline algorithm in web browsers nor assistive technology; it was never part of a final W3C specification. Therefore the [outline](https://www.w3.org/TR/html5/sections.html#outline) algorithm *should not be used* to convey document structure to users. **Authors are advised to use heading [rank](https://www.w3.org/TR/html5/sections.html#rank) (`h1`-`h6`) to convey document structure.**



### 宽容的HTML5

[mdn-heading elements](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Heading_Elements)：

![heading-elements](../Immagine/heading-elements.png)

在`<h1>`里面放不是Phrasing content的`<section>`，也是可以正常渲染。



## 进一步 研究内容

### HTML5提供的能力

### HTML5标准的发展

目前是WHATWG和W3C两个组织在联合发展HTML5，主要的文档在WHATWG，这中间有曲曲折折好多故事：[知乎-HTML5发展简史](https://zhuanlan.zhihu.com/p/44164232)；

和JavaScript规范发展不同的是（每年会发布一个版本，每个特性会经历从0到1的多个阶段），HTML5发展的模式称为 [Living Standard](https://whatwg.org/faq#living-standard) ，它不会再有版本的概念，而是不断地演进、更新。和 css 类似的是，它会在某个时间节点发布快照。（这里Stackoverflow上也有类似的问题提出： [Frequency of HTML5 updates](https://stackoverflow.com/questions/5625411/frequency-of-html5-updates)）

HTML5 Living Standard演进的节奏在WHATWG FAQ有提到：

> It also means that new features get added to them over time, at a rate intended to keep the standard a little ahead of the implementations, but not so far ahead that the implementations give up.



### HTML开拓的方向

- 语义化
  - 它的价值 （语义化有什么用 => 组织文档结构 => 设备：浏览器、可获取工具（accessibility tools：screen readers、voice assistants））
  - 它是如何实现，和语义化标签有什么关系
  - [ARIA](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA)







## 思考

在之前看浏览器解析一份HTML文档时，发现其对HTML元素是最高效的（JavaScript会暂停解析、css请求外部资源也可能暂停关联的JavaScript执行），所以如果HTML原生的标签+标签属性提供的**能力**越强大，越应该是**解决方案的首选**。



扫描标签后，其实标签本身的使用，是参照说明书的搬运工作，不具有技术门槛。所以**了解HTML提供的能力（而不是逐文档阅读）**、关注**标准是如何推进的**（即功能是如何加出来的）、**在使用之上有哪些探索的可能性**是关键的。

  

## 链接

1. [HTML - Living Standard](https://html.spec.whatwg.org/multipage/)
2. [DOM - Living Standard](https://dom.spec.whatwg.org/)