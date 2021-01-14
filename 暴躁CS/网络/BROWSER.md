做web开发目的是为了：improve performance and perceived performance

目前有两大阻力：

- **Latency** is our main threat to overcome to ensure a fast load. 
- For the most part, browsers are considered **single threaded**. 



## 前置流程

DNS查找：

- 通常来说，只需对加载页面的主机DNS查找一次，但是如果引用了不同来源的资源，那就相应的需要DNS查找。这在移动端会是一场灾难。

TCP握手

TLS协商：如果是HTTPS的话

![](/Immagine/8-trips-before-request.png)

TCP slow start：从14kb开始，28kb、56kb如此翻倍去测试网络情况，找到堵塞的阈值

Congestion control：使用发送的数据包和客户端返回的ACK来确定发送速率，连接的能力受到硬件和网络条件限制



## Paring

### Building the DOM tree

- HTML parsing involves [tokenization](https://developer.mozilla.org/en-US/docs/Web/API/DOMTokenList) and tree construction => 包括获取tokens和构建树
- 在解析过程中，是否阻塞解析？
  - 遇到非阻塞资源，比如图片，浏览器发起请求，解析继续；
  - 遇到css文件，解析继续；
  - 遇到`<script>`标签，尤其是没有`async`和`defer`，解析暂停
  
  

**Preload Scanner**

- 在HTML parsing**占据主进程**的同时，Preload Scanner在同时进行。它扫描内容中的资源，按照优先级去  请求，这是一种优化行为。当HTML解析到资源位置时，资源可能已经获取到了

  

### Processing CSS and building the CSSOM tree

- 就web性能而言，这一块没有什么优化空间，因为构建CSSOM树的时间还少于DNS查找时间



### 其他

- JavaScript Compilation：js code => AST => bytecode
- *Building the Accessibility Tree*：AOM（The accessibility object model），语义版的DOM，当它构建完成，辅助设备才可以获取内容

   

## Render

### Style

CSSOM + DOM = render tree （DOM树和CSSOM树合成一个渲染树）

会根据CSS级联规则计算应用的css规则。



有些不会出现在render tree中，比如`head`标签和它的子节点、

样式为`{display: none}`的节点。

`{visibilty: hidden}`的节点会出现在render tree中。



### Layout

从render tree的根结点开始，遍历整棵树，去计算好每个节点的**几何形状**（宽度、高度）、**位置**，以及和这个节点相关对象的几何形状和位置。



> On the web page, most everything is a box.

不同的设备、桌面，会有不同的viewport尺寸，layout是基于这个viewport。



如果遇到了image，在Layout过程资源还没有返回，这个时候不清楚尺寸大小，会给一个占位空间。等之后图片资源拿到了，再去计算这个节点和相关节点的尺寸和位置，把真实图片尺寸塞入。



Layout是第一次计算，后续的计算称为Reflow。



### Paint

把Layout计算好的结果，绘制到屏幕上的真实像素，它会把元素分到多个层。

首次绘制，the first meaningful paint；后续绘制，repaint。



在Paint过程，如果遇到了一些特殊的属性和元素，比如`video`、`canvas`，会独立地实例化一层(layer)。将内容提到CPU的层，可以提高paint和repaint的性能，但是在内存管理上是昂贵的。



### Composition

在某些场景需要。

不同的层，以正确的顺序呈现。



## 其他

### 建议

#### 限制在14kb

因为收到的首个数据包大小是14kb，以及无论这14kb是否包含完整的内容，浏览器都会开始解析、渲染，所以要求14kb包含，至少首次渲染的HTML和CSS



节点越多，构建DOM树的时间越长

过多脚本，虽然有Preload Scanner帮助提前请求，但也可能是一个重要的瓶颈



### 老生常谈问题

#### reflow vs repaint

这是一个经常会提到的对比。reflow会引起repaint、re-composition。

以image为例，如果提前没有设置好image的尺寸，layout过程中如果没有获取到资源，等资源过来的时候是会reflow -> repaint -> re-composition。

但是如果提前设置好了image的尺寸，资源到来之后不会触发reflow，仅仅会触发repaint -> re-composition



#### 为什么推荐`script`标签放在`body`标签底部？

因为在遇到没有`async`和`defer`的 script 标签时，HTML解析会暂停，所以会推荐把 script 标签放在 body最下面，不阻塞页面展示。



### 名词

Time to First Byte(TTFB)：从发送第一个请求到收到第一个数据包的时间，这个数据包一般是14kb

Time to Interactive(TTI)：从发送第一个请求到页面可交互操作的时间，50ms以内



## 链接

[Populating the page: how browsers work](https://developer.mozilla.org/en-US/docs/Web/Performance/How_browsers_work)

[How Browsers Work: Behind the scenes of modern web browsers](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/) ： 上古神作


