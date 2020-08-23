# Hello SVG

[sitepoint - what is svg](https://www.sitepoint.com/svg-101-what-is-svg/)

## what is SVG

它是什么？

- Scalable Vector Graphics：**可缩放矢量图**
- 基于**XML**(eXtensible Markup Html)，用于Web和其他环境的矢量图形格式 => 相对于HTML**更严格**，如果出现问题，整个SVG就不会渲染
- SVG文档不过是描述线条，曲线，形状，颜色和文本的简单纯文本文件，它是**可读的（human-readable）**
- W3C标准



相比于PNG、GIF、JPG？

- 当以 inline svg 形式嵌入HTML，SVG代码可以通过CSS、JavaScript更改，这就更加灵活和多功能性；



## why svg

SVG解决的问题：

- 可伸缩性和响应性：GIF、JPG、PNG使用像素网格的图像格式，在伸缩变化时会失真，而SVG基于形状、数字、坐标的绘制指令，在伸缩变化时不会失真，这就接住了当前网络响应式变化的需求；
- 可编程和可交互：各种动画和交互可通过css、javascript加入到inline svg中；
- 可获取（accessibility）：svg文件就是文本，它可以被屏幕阅读器、搜索引擎等其他设备获取到；



## svg case

- 普通插图和图表
- Logos和icons
- 动画：css动画、JavaScript、内建的SMIL动画功能
  - [appealing animations](https://codepen.io/thiennhat/pen/BNByzJ?editors=1010)
  - [line drawing effects](https://codepen.io/lindsayrusd/pen/kXpRvY)
- 交互（图形、图表、地图）
  - [Interactive SVG Infographic](https://tympanus.net/Tutorials/InteractiveSVG/)
- 其他特殊效果
  - [codepen svg shape morphing](https://codepen.io/collection/nGoLEj/)

