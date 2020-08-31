# Hello SVG

[sitepoint - what is svg](https://www.sitepoint.com/svg-101-what-is-svg/)

[CSS with SVG: Real World Usage](https://www.sitepoint.com/css-with-svg/)



## what is SVG

它是什么？

- 【全称】Scalable Vector Graphics：**可缩放矢量图**
- 【语法】基于**XML**(eXtensible Markup Html)，用于Web和其他环境的矢量图形格式 => 相对于HTML**更严格**，如果出现问题，整个SVG就不会渲染
- 【描述内容】SVG文档不过是描述线条，曲线，形状，颜色和文本的简单纯文本文件，它是**可读的（human-readable）**
- 【标准】W3C标准



相比于PNG、GIF、JPG？

- 当以 inline svg 形式嵌入HTML，SVG代码可以通过CSS、JavaScript更改，这就**更加灵活和多功能性；**
- PNG、GIF、JPG这些是位图（Bitmaps），存储的是像素的RGB和透明度信息，而SVG是矢量图，存储的是路径；



## why svg

SVG解决的问题：

- 可伸缩性和响应性：GIF、JPG、PNG使用像素网格的图像格式，在伸缩变化时会失真，而SVG基于形状、数字、坐标的绘制指令，在伸缩变化时不会失真，这就接住了当前网络响应式变化的需求；
- 可编程和可交互：各种动画和交互可通过css、javascript加入到inline svg中；
- 可获取（accessibility）：svg文件就是文本，它可以被屏幕阅读器、搜索引擎等其他设备获取到；
- SVG的大小会比PNG、JPG小



## svg case

简单的图形：

- 普通插图、图表、Logos、icons
- 为啥？因为svg表达图形的信息是没有Bitmaps详细的，所以它的使用场景是简单的图形



可编程和可交互：

- 动画：css动画、JavaScript、内建的SMIL动画功能
  - [appealing animations](https://codepen.io/thiennhat/pen/BNByzJ?editors=1010)
  - [line drawing effects](https://codepen.io/lindsayrusd/pen/kXpRvY)
- 交互（图形、图表、地图）
  - [Interactive SVG Infographic](https://tympanus.net/Tutorials/InteractiveSVG/)
- 其他特殊效果
  - [codepen svg shape morphing](https://codepen.io/collection/nGoLEj/)



具体的使用形式：

1. 作为静态图片使用，和Bitmaps使用方法一致，优点可能在于SVG是可伸缩的；

2. 内嵌在HTML内使用：通过css属性设置 [standard svg attributes](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute) ，还有`:hover`、`:transition`、`animation`等;

3. SVG Effects on HTML Content，这个比较特殊，SVG的效果作为在绑定的HTML内容上，使用 `mask` 、`clip-path`、`filter`等属性；

4. svg + css + javascript 打包成一个，叫做 portable svg：

   ```html
   <svg id="invader" xmlns="https://www.w3.org/2000/svg" viewBox="35.4 35.4 195.8 141.8">
     <!-- invader images: https://github.com/rohieb/space-invaders !-->
     <style>/* <![CDATA[ */
       path {
         stroke-width: 0;
         fill: #080;
       }
   
       path:hover {
         fill: #c00;
       }
     /* ]]> */</style>
     <path d="M70.9 35.4v17.8h17.7V35.4H70.9zm17.7 17.8v17.7H70.9v17.7H53.2V53.2H35.4V124h17.8v17.7h17.7v17.7h17.7v-17.7h88.6v17.7h17.7v-17.7h17.7V124h17.7V53.2h-17.7v35.4h-17.7V70.9h-17.7V53.2h-17.8v17.7H106.3V53.2H88.6zm88.6 0h17.7V35.4h-17.7v17.8zm17.7 106.2v17.8h17.7v-17.8h-17.7zm-124 0H53.2v17.8h17.7v-17.8zm17.7-70.8h17.7v17.7H88.6V88.6zm70.8 0h17.8v17.7h-17.8V88.6z"/>
     <path d="M319 35.4v17.8h17.6V35.4H319zm17.6 17.8v17.7H319v17.7h-17.7v17.7h-17.7V159.4h17.7V124h17.7v35.4h17.7v-17.7H425.2v17.7h17.7V124h17.7v35.4h17.7V106.3h-17.7V88.6H443V70.9h-17.7V53.2h-17.7v17.7h-53.2V53.2h-17.7zm88.6 0h17.7V35.4h-17.7v17.8zm0 106.2h-35.5v17.8h35.5v-17.8zm-88.6 0v17.8h35.5v-17.8h-35.5zm0-70.8h17.7v17.7h-17.7V88.6zm70.9 0h17.7v17.7h-17.7V88.6z"/>
     <script>/* <![CDATA[ */
       const
         viewX = [35.4, 283.6],
         animationDelay = 500,
         invader = document.getElementById('invader');
   
       let frame = 0;
   
       setInterval(() => {
         frame = ++frame % 2;
         invader.viewBox.baseVal.x = viewX[frame];
       }, animationDelay);
   
     /* ]]> */</script>
   </svg>
   ```

   





## 注意事项

- svg不是自己代码敲出来的，而是用工具生成，在此基础之上，去做加法




