# CSS
## 盒模型
  盒模型由content内容、内边距padding、border边框以及外边距margin组成。
  
  分为标准盒模型和IE怪异模型。
  
  标准盒模型的宽高指的是content内容的宽高，IE盒模型的宽高指的是content内容+padding内边距+border边框的宽高。
  设置两种盒模型使用：box-sizing:content-box(标准)   box-sizing:boder-box(怪异模型)

## css选择器权重
  !important>内联样式>id选择器>类选择器、伪类选择器、属性选择器>标签选择器、伪元素选择器
  
  优先级相等的情况，最晚出现的样式被采纳，写样式的时候使用较低的优先级，修改的时候较容易被覆盖

## 伪类和伪元素

  伪类没有创建一个文档树之外的元素
  伪元素创建了
  伪类： :link :visited :hover :active :first-child :nth-child(n) nth-of-type()等
  伪元素： ::before ::after ::first-letter ::first-line等
  在不指定类型时，:nth-child(n)选中父元素下的第n个子元素；:nth-of-type(n)选中的是父元素下不同标签类型的第n个。

## z-index
  层叠上下文(background/border)<负z-index<block平盒子<float浮动盒子<inline/inline-block内联盒子<z-index:0或auto<正z-index
## 定位
  正常文档流：把页面分成一行一行，块状元素占一行，相邻的行内元素从左到右排成一行直到排满。
  脱离正常文档流：使用float或者定位改变html文档结构。
  static：默认值，在正常流中。
  fixed：固定定位，相对于浏览器窗口
  relative：脱离正常文档流，但在文档流中的位置仍然存在，相对于最近的父元素，无论是何种定位
  ![relative1](http://images.51cto.com/files/uploadimg/20100909/1528450.jpg)
  ![relative2](http://images.51cto.com/files/uploadimg/20100909/1528452.jpg)
  absolute：脱离文档流，元素不存在于文档流中了，相对于最近的定义为absolute和relative的父层，不一定是直接父层。
  ![absolute1](http://images.51cto.com/files/uploadimg/20100909/1528451.jpg)
  ![absolute2](http://images.51cto.com/files/uploadimg/20100909/1528453.jpg)
## 清除浮动
浮动元素引起的问题：

* 父元素的高度无法撑开
* 同级元素会紧随其后可能被覆盖
* 如果一个元素浮动，该元素之前的元素也需要浮动，否则可能会影响页面显示的结构。
清除浮动的方法：

* 使用clear:both
* 使用伪元素选择器：：afeter清除浮动，内部使用clear:both
* 设置overflow: auto 或 overflow: hidden 触发 BFC
## BFC
  块级格式化上下文，是一个独立的渲染区域，规定了内部的box如何布局，与外部区域不相干
### 触发BFC
  * 根元素，即 HTML 元素
  * float 的值不为 none
  * overflow 的值不为 visible
  * display 的值为 inline-block、table-cell、table-caption
  * position 的值为 absolute 或 fixed
### 布局规则
  * 内部的 Box 会在垂直方向，一个接一个地放置。
  * Box 垂直方向的距离由 margin 决定。属于同一个 BFC 的两个相邻 Box 的 margin 会发生重叠。
  * 每个元素的 margin box 的左边，与包含块 border box 的左边相接触（对于从左向右的格式化，否则相反）。即使存在浮动也是如此。
  * BFC 的区域不会与 float box 重叠。
  * BFC 就是页面上一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
  * 计算 BFC 的高度时，浮动元素参与计算。
### 作用
  * 自适应两栏布局：
    利用BFC不会与float box重叠，左边设置float
  * 阻止元素被浮动元素覆盖：
    上面的 box：margin-bottom: 100px; 下面的 box：margin-top: 100px;（他们是同一侧的 margin，所以会发生 margin 重叠的情况，两个 div 的距离实际上只有 100px。）
    这个时候 BFC 的作用4：阻止 margin 重叠 就派上了用场：
    当两个相邻块级子元素分属于不同的 BFC 时可以阻止 margin 重叠。
    操作方法：给其中一个 div 外面包一个 div，然后通过触发外面这个 div 的 BFC，就可以阻止这两个 div 的 margin 重叠，具体触发方式可以参考上文给出的触发条件。
  * 可以包含浮动元素——清除内部浮动：
    给父 div 加上 overflow: hidden;
    清除浮动原理：触发父 div 的 BFC 属性，使下面的子 div 都处在父 div 的同一 BFC 区域之内，由于BFC计算高度时，浮动元素参与计算，因此可以清除浮动。 
    还可以使父 div 向同一个方向浮动来达到清除浮动的目的，清除浮动的原理是两个 div 都位于同一个浮动的 BFC 区域之中。
  * 分属于不同的 BFC 时可以阻止 margin 重叠

## flex布局
flex项目的基本布局
   ![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071004.png)
容器的6个属性：
- flex-direction（主轴方向）
      .box{
          flex-direction:row|row-reserse|column|column-reserve
      }
  ![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071005.png)
- flex-wrap(nowrap不换行，wrap换行，wrap-reserve第一行在下方)
- flex-flow（flex-flow：flex-direction||flex-wrap结合）
- justify-content（主轴排列）
     .box{
      justify-content:flex-start|flex-end|flex-center|space-between|space-around
     }
  ![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071010.png)
- align-items(定义了在交叉轴上的对齐方式)
      .box {
            align-items: flex-start | flex-end | center | baseline | stretch;
      }
  ![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071011.png)
- align-content（多根轴线对齐方式）
  项目的6个属性
  - order(定义项目的排列顺序，顺序越小，越在前面)
  - flex-grow（项目的放大比例）
  - flex-shrink（项目的缩小比例）
  - flex-basis
  - flex（上面三者放在一起）
  - align-self（允许单个项目与其它项目不同）
## 多行文本溢出显示省略号
  overflo:hidden;
  text-overflow:ellipsis
## 回到顶部
  window.scrollTo(0,0);
## px\em\rem的区别
  px像素，相对于屏幕分辨率而言
  em相对长度，相对于父元素的字体尺寸而言
  rem新增特性，相对于HTML根元素而言，通过调整根元素的字体就可以调整所有字体大小。

## vw/vh/vmin/vmax
  vw:100vw视口宽度的100
  vh:100vh视口高度的100%
  vmin:vw和vm中较小的值
  vmax:vw和vm中较大值
## background-origin的作用
  新增属性，改变背景起始位置
  border-box：背景图片的摆放以border区域为参考
  padding-box：背景图片的摆放以padding区域为参考
  content-box：背景图片的摆放以content区域为参考
## 精灵图
  当需要加载很多小图片时，为避免服务器拥堵，将许多小图片合成一个大图片，利用background-position定位获取相应小图片。
   如果需要改变一个小图片需要改动整个大图片
## png、JPEG、svg、webp、gif
  1. png
      透明性：png是完全支持alpha透明的（透明、半透明、不透明）
      动画：它不支持动画 
  2. jpeg
      透明性：它不支持透明性
      动画：它不支持动画 
      对jpeg图像的处理都会使得它的质量损失
  3. gif
      透明性：gif是一种布尔透明类型，即它可以使全透明，也可是全不透明，但是它并没有半透明的（alpha透明
      动画：gif格式支持动画。
      无损耗性：gif是一种无损耗的图像格式
  4. svg
      SVG 指可伸缩矢量图形 (Scalable Vector Graphics)
      SVG 用来定义用于网络的基于矢量的图形
      SVG 使用 XML 格式定义图形
      SVG 图像在放大或改变尺寸的情况下其图形质量不会有所损失
      SVG 是万维网联盟的标准
      SVG 与诸如 DOM和 XSL 之类的W3C标准是一个整体
  5. webp  
      WebP是同时支持有损和无损压缩的、使用直接色的、点阵图
## FOUC
  浏览器样式闪烁：由于CSS使用@important引用，或者存在多个CSS文件在页面底部引入，导致页面闪烁
  解决：link加载CSS放在head中
## 动画
  位置平移   方向旋转   大小缩放  透明度  其他-线形变换
  ### transform变形
   transform-origin：改变原点位置
   transform属性值：
    translate（）位移函数        
    scale（）缩放函数
    skew（）倾斜函数
    rotate（）旋转函数
    matrix（）矩阵函数   
  ### transition过渡动画
   transition-property：参与过度的属性名（如width）
   transition-duration：过度持续时间（2s）
   transition-delay：过渡开始前的延迟时间
   transition-timing-function：缓动函数，过渡期间的速度和节奏
    div{
        transtion:transform 500ms ease-out 1s;
    }
   触发条件：
   CSS伪类触发：
    div:hover{
        transform:rotate(30deg)
    }
   媒体查询触发：
    @media only sceen and （max-length:600px）
    div{
        transform:rotate(30deg)
    }
   JavaScript触发
    div.style.transform="rotate(30deg)"
  ### animation关键帧动画
   animation-name:名字
   animation-duration：持续时间
   animation-delay：延迟时间
   animation-timing-function：缓动函数steps（间隔数量，start跳过首帧/end跳过末帧）
   animation-iteration-count：循环次数
   animation-direction：播放方向(normal从左到右回到起点,reserve从右到左，回到起点,alternate从左到右，再从右到左,alternate-reserve从右到左再从左到右)
   animation-play-state：播放状态
   animation-fill-mode：开始之前或结束之后的状态（forward：动画结束后保持末帧；backwards：动画延迟时间段，保持首帧；both：结合两点）

  #### drift动画实例
    @keyframes drift{
        from{
            transform：translate（0，0）；
        }
        30%{
            transform：translate（20px，0）；
        }
        60%{
            transform：translate（40px，0）；
        }
        to{
            transform:translate(60px,0);
        }
    }
   ### 过渡与动画区别
   过渡只能有初始状态和结束状态，动画可以设置关键帧，控制变化过程中的更多状态
   动画不需要触发条件，HTML载入完成后立即触发
   动画子属性比过渡多，可以控制循环次数，播放方向和动画状态
## 编码
  ### 使一个div占满整个屏幕
给div设置定位
    div {
        width: 100%;
        height: 100%;
        background: yellow;
        position: absolute;
    }
通过html，body的宽高来让div充满屏幕
    * {
        margin: 0;
        padding: 0;
        }
        html, body {
        width: 100%;
        height: 100%;
        }
        div {
        width: 100%;
        height: 100%;
        background: red;
        }

  ### 硬币旋转
    <div id="euro">
        <div class="back"></div>
        <div class="middle" style="transform: translateZ(1px)"></div>
        <div class="middle" style="transform: translateZ(2px)"></div>
        <div class="middle" style="transform: translateZ(3px)"></div>
        <div class="middle" style="transform: translateZ(4px)"></div>
        <div class="middle" style="transform: translateZ(5px)"></div>
        <div class="middle" style="transform: translateZ(6px)"></div>
        <div class="middle" style="transform: translateZ(7px)"></div>
        <div class="middle" style="transform: translateZ(8px)"></div>
        <div class="middle" style="transform: translateZ(9px)"></div>
        <div class="front" style="transform: translateZ(10px)"></div>
    </div>
    #euro {
        width: 150px;
        height: 150px;
        margin-left: -75px;
        margin-top: -75px;
        position: absolute;
        top: 50%;
        left: 50%;
        transform-style: preserve-3d;
        animation: spin 2.5s linear infinite;
    }
    .back {
        background-image: url("images/backeuro.png");
        width: 150px;
        height: 150px;
    }
    .middle {
        background-image: url("images/backeuro.png");
        width: 150px;
        height: 150px;
        position: absolute;
        top: 0;
    }
    .front {
        background-image: url("images/faceeuro.png");
        width: 150px;
        height: 150px;
        position: absolute;
        top: 0;
    }
    @keyframes spin {
        from {
            transform: rotateY(0deg);
        }
        to {
            transform: rotateY(360deg);
        }
    }
 ### 三角形
   设置方块高度和宽度为0，其他三块的border-color为transparent 
   ```
    .triangle_up{
        width:0;
        height:0;
        border-width:0px 100px 100px 100px;
        border-style:solid;
        boder-color:transparent  transparent  #A76  transparent
    }
```
  ### 梯形
   设置一个方块color透明，其他不变
   ```
    .ladder_shape{
        width:100px;
        height:50px;
        border-width:0px 100px 100px 100px;
        border-style:solid;
        border-color:transparent transparent #A76 transparent
    }
```
  ### 平行四边形
   先进行div倾斜操作，在对里面文字反倾斜
```
    .shape{
        width:100px;
        height:50px;
        background:red;
        transform:skewX(-50deg);
        margin:20px auto;
    } 
    .shape span{
        width:100px;
        text-align:center;
        display:inline-block;
        line-height:50px;
        vetrical-align:middle;
        color:green;
        transform:skewX(50deg);
    }
```
  ## 三栏布局
  1、浮动布局
```
    <div class="container">
        <div class="left col">left</div>
        <div class="right col">right</div>
        <div class="main col">main</div>
    </div>
    .col {
        min-height: 100px;
        background: #eee;
        border: 1px solid #666;
    }
    .left {
        float: left;
        width: 100px;
    }
    .right {
        float: right;
        width: 100px;
    }
    .main {
        margin: 0 102px;
    }
```
2、绝对定位
```
    <div class="container">
        <div class="left col">Left</div>
        <div class="right col">Right</div>
        <div class="main col">Main</div>
    </div>
    .container {
        position: relative;
    }
    .col {
        min-height: 100px;
        background: #eee;
        border: 1px solid #666;
    }
    .left, .right {
        position: absolute;
        top: 0;
    }
    .left {
        left: 0;
        width: 100px;
    }
    .right {
        right: 0;
        width: 100px;
    }
    .main {
        margin: 0 102px;
    }
```
3、BFC
```
    <div class="container">
        <div class="left col">left</div>
        <div class="right col">right</div>
        <div class="main col">main</div>
    </div>
    .col {
        min-height: 100px;
        background: #eee;
        border: 1px solid #666;
    }
    .left {
        float: left;
        width: 100px;
    }
    .right {
        float: right;
        width: 100px;
    }
    .main {
        overflow: hidden;
    }
```
4、Flex
```
    <div class="container">
        <div class="left col">left</div>
        <div class="main col">main</div>
        <div class="right col">right</div>
    </div>
    .container {
        display: flex;
    }
    .col {
        min-height: 100px;
        background: #eee;
        border: 1px solid #666;
    }
    .left {
        width: 100px;
    }
    .main {
        flex: 1;
    }
    .right {
        width: 100px;
    }
```
5、table 布局
```
    <div class="container">
        <div class="left col">left</div>
        <div class="main col">main</div>
        <div class="right col">right</div>
    </div>
    .container {
        display: table;
        width: 100%;
        min-height: 100px;
    }
    .col {
        display: table-cell;
        background: #eee;
        border: 1px solid #666;
    }
    .left {
        width: 100px;
    }
    .right {
        width: 100px;
    }
```
6、双飞翼布局
设置一个container，里面包含center（center内部包含一个inner-center）、left、right，center在最前面，先加载center。
   center、left、right均设置float：left
   center的width为100%
   此时center占满了，设置left的margin-left：-100%，设置margin-right：-220px
   center内容被覆盖，给center加一个内层的inner-center，margin：0 220px 0 200px
   center设置overflow：hidden去浮动
```
    <div class="doubleFly">
        <div class="container">
            <div class="main col">doubleFly main</div>
        </div>
        <div class="left  col">left</div>
        <div class="right col">right</div>
    </div>
    .container {
        width: 100%;
        float: left;
    }
    .col {
        min-height: 100px;
    }
    .main {
        margin: 0 100px;
        background: #666;
    }
    .left {
        width: 100px;
        float: left;
        margin-left: -100%;
    }
    .right {
        width: 100px;
        float: right;
        margin-left: -100px;
    }
```
7、圣杯布局
三者都float：left，position：relative
```
    center部分width为100%
    此时center占满了，设置left的margin-left：-100%，设置margin-right：-220px
    会覆盖中心内容，设置container的padding：0 220px 0 200px
    还原left位置left:-200px right位置：-220px
    <div class="container">
        <div class="main col">main</div>
        <div class="left col">left</div>
        <div class="right col">right</div>
    </div>
    .container {
        margin: 0 100px;
    }
    .col {
        min-height: 100px;
        float: left;
    }
    .main {
        width: 100%;
        background: #666;
    }
    .left {
        width: 100px;
        position: relative;
        left: -100px;
        margin-left: -100%;
        background: #999;
    }
    .right {
        width: 100px;
        position: relative;
        right: -100px;
        margin-left: -100px;
        background: #333;
    }
```
  ## 垂直居中
   1、行高
```
    <div class="box">垂直居中</div>
    .box {
        width: 100px;
        height: 100px;
        background: #FEE;
        line-height: 100px;
    }
```
   2、内边距
```
    <div class="box">垂直居中</div>
    .box {
        width: 100px;
        background: #FEE;
        padding: 50px 0;
    }
```
   3、table
```
    <div class="container">
        <div class="box">垂直居中</div>
    </div>
    .container {
        width: 100px;
        height: 100px;
        background: #FEE 
        display: table;
    }
    .box {
        display: table-cell;
        vertical-align: middle;
    }
```
   4、定位
```
    <div class="container">
        <div class="box">垂直居中</div>
    </div>
    .container {
        width: 100px;
        height: 100px;
        background: #FEE;
        position: relative;
    }
    .box {
        position: absolute;
        top: 50%;
        transform: translateY(-50%);
    }
```
   5、Flex
```
    <div class="box">垂直居中</div>
    .box {
        width: 100px;
        height: 100px;
        background: #FEE;
        display: flex;
        align-items: center;
    }
```
  ## 水平居中
  1、外边距
  ```
    <div class="box"></div>
    .box {
        width: 100px;
        height: 100px;
        background: #FCC;
        margin: 0 auto;
    }
```
  2、行内块
  ```
    <div class="container">
        <div class="box"></div>
    </div>
    .container {
        text-align: center;
    }
    .box {
        width: 100px;
        height: 100px;
        background: #FCC;
        display: inline-block;
    }
```
  3、绝对定位
  ```
    <div class="box"></div>
    .box {
        width: 100px;
        height: 100px;
        background: #FCC;
        position: absolute;
        left: 50%;
        transform: translateX(-50%);
    }
```
  4、Flex
  ```
    <div class="container">
        <div class="box"></div>
    </div>
    .container {
        display: flex;
        justify-content: center;
    }
    .box {
        width: 100px;
        height: 100px;
        background: #FCC;
    }
```
