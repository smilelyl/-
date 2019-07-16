# CSS知识要点
##  CSS选择器
### 举例：
    标签选择器（div）
    id选择器（#myid）
    class选择器（#myclass）
    后代选择器（div a）  div下面所有的a标签
    子代选择器（div>p)  div下一层的p标签
    相邻选择器（div+p->div相邻p   div~p->div下所有元素） 
    通配符选择器（*） 一般使用通配选择器进行重置，来覆盖浏览器的默认规则
    否定选择器 :not(.link){}
    伪类选择器 ：:hover{} ：first-child{} 是一种状态
    伪元素选择器 ::before{} 伪元素本质上是创建了一个虚拟容器(元素)
    属性选择器 a[href] {color:red;} 
选择器 	描述
[attribute] 	用于选取带有指定属性的元素。
[attribute=value] 	用于选取带有指定属性和值的元素。
[attribute~=value] 	用于选取属性值中包含指定词汇的元素。
[attribute|=value] 	用于选取带有以指定值开头的属性值的元素，该值必须是整个单词。
[attribute^=value] 	匹配属性值以指定值开头的每个元素。
[attribute$=value] 	匹配属性值以指定值结尾的每个元素。
[attribute*=value] 	匹配属性值中包含指定值的每个元素。
### CSS定义权重
  !important > 行内样式 > ID选择器 > (类选择器 | 属性选择器 | 伪类选择器 ) > 元素选择器 
## hack
  让CSS代码能够兼容不同的浏览器，避免错误的布局：
   属性hack：IE6能识别下划线“_”和星号“*”，IE7能识别星号“*”，但不能识别下划线”_ ”，而firefox两个都不能认识。
   选择符hack：html #demo { color:red;} / 仅IE6 识别 */     +html #demo { color:red;} / 仅IE7 识别 */
   IE条件注释
##  BFC	
## 清除浮动
### 在父元素最后设置一个空div:
```
<div style="clear:both;"></div>
```

### 触发BFC：

```
<div  class="div1" style="overflow: hidden">
```

### after：
```
.clearfloat:after{content:"";display:table;clear:both}
```

## stacking level
  层叠顺序（右下至上）：背景和边框->负z-index->块级元素->浮动元素->行内和行内块元素->z-index:0或auto->正z-index
  形成层叠上下文的条件：
    根元素（html）
    z-index值不为auto的绝对/相对定位
    z-index值不为auto的flex项目
    opacity小于1的元素
    transform不为none的元素
    mix-blend-mode不为normal的元素
    filter不为none的元素
    isolation设置为isolate的元素
    perspective不为none的元素
  例子：

```
<div class="one">
    <div class="two"></div>
    <div class="three"></div>
</div>

<div class="four">
    <div class="five"></div>
    <div class="six"></div>
</div>
.one{
    position:relative;
    z-index:2;
    .two{
        z-index: 6;
    }
    .three{
        position: absolute;
        z-index: 5;
    }
}

.four{
    position:absolute;
    .five{}
    .six{
        position: absolute;
        left:0;
        top:0;
        z-index: -1;
    }
}

```
three two one five four six（由上至下）
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
##  居中
   ### 水平居中
   #### 行内元素居中
   只针对行内元素（如text、img、button等），或者块状元素display为inline时，而且没有float影响
   使用 text-align:center
   #### 块状元素居中
   ##### 定宽块状元素居中
   定宽、块状，设置左右margin为auto
    .div{
        width:100px;
        border:1px solid red;
        margin:0 auto;
    }
   ##### 不定宽块状元素居中
   将要居中的元素放在table里或设置元素的display为table，设置margin：0 auto；
    .wrap{
    background:#ccc;
    display:table;
    margin:0 auto;
    }
    <div class="wrap">
    Hello world 
    </div>
   改变块级元素的display为inline，使用text-align：center,但是不能设置宽高，可以变成inline-block
   给父元素设置float，给父元素设置position：relative和left:50% 子元素设置position：relative，left：-50%
   ### 垂直居中
   #### 父元素高度确定的单行文本
   设置度元素的height和line-height一样高
   #### 父元素高度确定的多行文本
   使用table标签或display设置为table或table-cell，同时设置vertical-align：middle
   #### 父元素高度不确定
   父元素position：relative；子元素position：absolute，top：50%，transform：-50%
## 精灵图
   当需要加载很多小图片时，为避免服务器拥堵，将许多小图片合成一个大图片，利用background-position定位获取相应小图片。
   如果需要改变一个小图片需要改动整个大图片
## css画三角形、梯形和平行四边形
   方法一：使用border-width
    .basic_triangle{
        width:50px;
        height:50px;
        border-width:100px 100px 100px 100px;
        border-style:solid;
        border-color:#f9a #f81 #A76 #153;
        margin:20px auto;
    }
   效果如下图：
   ![](https://img-blog.csdn.net/20180813174421697?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTAxNzYwOTc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
   ### 三角形
   设置方块高度和宽度为0，其他三块的border-color为transparent 
    .triangle_up{
        width:0;
        height:0;
        border-width:0px 100px 100px 100px;
        border-style:solid;
        boder-color:transparent  transparent  #A76  transparent
    }
   ### 梯形
   设置一个方块color透明，其他不变
    .ladder_shape{
        width:100px;
        height:50px;
        border-width:0px 100px 100px 100px;
        border-style:solid;
        border-color:transparent transparent #A76 transparent
    }
   ### 平行四边形
   先进行div倾斜操作，在对里面文字反倾斜
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
## 盒模型
### 标准盒模型
![](http://img.smyhvae.com/2015-10-03-css-27.jpg)
### IE盒模型
![](http://img.smyhvae.com/2015-10-03-css-30.jpg)
## 13. 文本溢出显示省略号
   单行文本溢出
    .ellipsis{
        overflow:hidden;
        text-overflow:ellipsis;
        white-space:nowrap;
    }
   多行文本溢出
    .ellipsis::after{
        content:"...";
        position:absolute;
        backgroud:-webkit-linear-gradient(left,transparent,#fff 62%);
        backgroud:-o-linear-gradient(left,transparent,#fff 62%);
        backgroud:-moz-linear-gradient(left,transparent,#fff 62%);
        background:linear-gradient(to right,transparent,#fff 63%)
    }
   js实现：
    function mitulineHide(num,con){ var contain = document.getElementById(con);
        console.log(con);
        var maxSize = num;
        var txt = contain.innerHTML;
        if(txt.length>num){ console.log('1');
            txt = txt.substring(0,num-1)+"...";
            contain.innerHTML = txt;
        }else{ console.log("error") } };
    mitulineHide(10,'ellipsis');

## 回到顶部
## canvas
## absolute fixed relative static
   正常文档流：把页面分成一行一行的，块元素单独占一行，相邻的行内元素从左到右排成一行直到该行排满
   脱离正常文档流：使用浮动或定位去改变HTML文档结构
    static：默认值。没有定位，元素在正常流中（忽略trbl或z-index声明）
    fixed:固定定位，相对于浏览器窗口进行定位
    relative：元素脱离正常文档流，但其在文档流中仍然存在；relative定位的层总是相对于最近的父元素，无论父元素是何种定位方式。
   ![relative1](http://images.51cto.com/files/uploadimg/20100909/1528450.jpg)
   ![relative2](http://images.51cto.com/files/uploadimg/20100909/1528452.jpg)
    absolute：元素脱离正常文档流，并且元素不存在于文档流中了；absolute定位的层相对于最近的定义为absolute和relative的父层，不一定是直接父层。
   ![absolute1](http://images.51cto.com/files/uploadimg/20100909/1528451.jpg)
   ![absolute2](http://images.51cto.com/files/uploadimg/20100909/1528453.jpg)
## 三栏布局
### 双飞翼布局
    设置一个container，里面包含center（center内部包含一个inner-center）、left、right，center在最前面，先加载center。
    center、left、right均设置float：left
    center的width为100%
    此时center占满了，设置left的margin-left：-100%，设置margin-right：-220px
    center内容被覆盖，给center加一个内层的inner-center，margin：0 220px 0 200px
    center设置overflow：hidden去浮动
### 圣杯布局
    三者都float：left，position：relative
    center部分width为100%
    此时center占满了，设置left的margin-left：-100%，设置margin-right：-220px
    会覆盖中心内容，设置container的padding：0 220px 0 200px
    还原left位置left:-200px right位置：-220px
### 伸缩盒布局
## SEO
## HTML5 为什么只需要写<!DOCTYPE HTML>
   HTML5不基于SGML，因此不需要对DTD进行引用，但是需要doctype来规范浏览器的行为。
   而HTML4.01基于SGML，所以需要对DTD进行引用，才能告知浏览器文档所使用的文档类型。
##  flex布局
   flex项目的基本布局
   ![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071004.png)
容器的6个属性：
- flex-direction（主轴方向）
  .box{
      flex-direction:row|row-reserse|column|column-reserve
  }
  ![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071005.png)
- flex-wrap(nowrap不换行，wrap换行，wrap-reserve第一行在下方)
- flex-flow（flex-flow：<flex-direction>||<flex-wrap>结合）
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


