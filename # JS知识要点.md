# JS
## BOM
  浏览器对象模型，对浏览器窗口进行访问和操作
  Windows浏览器窗口（会获取文本框宽度、浏览器窗口宽度、打开关闭窗口）
  navigator包含客户端浏览器信息（包括浏览器名称、版本号、操作系统）
  sceen客户端显示屏信息
  history浏览器窗口访问过的URL（history.back返回上次访问地址，history.back(2)返回上上次）

|属性名|说明|
|:---:|:---:|
|history.length|表示会话历史中元素的数目，包括当前加载的页|
|history.scrollRestoration|允许web应用程序在历史导航上显式地设置默认滚动恢复行为，此属性可以是自动的（auto）或者手动的（manual)|
|history.state|返回一个表示历史堆栈顶部地状态的值|

|方法名|说明|
|:---:|:---:|
|history.back()|前往上一页，等价于history.go(-1)|
|history.forward()|前往下一页，等价于history.go(1)|
|history.go()|通过当前页面的相对位置从浏览器历史记录加载页面|
|**history.pushState()|按指定的名称和URL将数据push进会话历史栈**|
|history.replaceState()|按指定的数据，名称和URL，更新历史栈上最新的入口|
|location|当前URL的信息（浏览器中的地址栏，可以显示端口号服务器等）|

|属性名|例子|说明|
|:---:|:---:|:---:|
|hash|"#contents"|返回URL中的hash（#后跟零或多个字符）|
|host|"www.wrox.com:80" |返回服务器名称和端口号（如果有）|
|hostname|"www.wrox.com" |返回不带端口号的服务器名称|
|href|"https:/www.wrox.com"	|返回当前加载页面的完整URL|
|pathname|"/WileyCDA/" |返回URL中的目录和（或）文件名|
|port|"8080" |返回URL中指定的端口号（如果有）|
|protocol|"https:" |返回页面使用的协议|
|search|"?q=javascript |返回URL的查询字符串（以?开头）|
  ### offsetWidth/offsetHeight、clientWidth/clientHeight、scrollWidth/scrollHeight
   offsetWidth/offsetHeight返回content+padding+border
   
   clientWidth/clientHeight返回content+padding
   
   scrollWidth/scrollHeight返回content+padding+溢出内容的尺寸
## DOM
   ### DOM级别
   DOM0 xx.onclick=function(){};将一个函数赋值给一个事件处理属性
   
   DOM2 addEventListener，给一个处理程序添加多个处理函数
   
   DOM3 增加了更多的事件类型（UI事件、鼠标事件、键盘事件、焦点事件、滚轮事件）

   ### 事件流
   事件流：事件捕获阶段、处于目标阶段、事件冒泡阶段
   #### 事件捕获阶段：
   window->document->html->body->...->目标元素
   #### 事件冒泡阶段：
   反之
   #### 事件代理/委托：
   将事件绑定在父元素上，利用冒泡事件触发该事件。优点：可以减少事件注册，节省内存；可以将事件应用于动态添加的子元素上。
   点击UL下的li元素
```
    var ulClick=document.getElementById("ulClick");
    window.onload=function(){
        ulClick.onclick=function (ev) {
           var ev=ev||window.event;
           var target=ev.target||ev.srcElement;
            if(target.nodeName.toLowerCase() == 'li'){
                alert(target.innerHTML);
            }
        }
    }
```
e.target指向引发触发事件的元素（点击的li），e.currentTarget指向的是给绑定事件监听的那个对象（ul）

## 图片懒加载
  原理：将页面中的 img 标签 src 指向一张小图片或者 src 为空，然后定义 data-src 属性指向真实的图片，在可视区域内时，将data-src的值赋给src。src指向一张默认的图片，否则当src为空时也会向服务器发送一次请求。可以指向loading的地址。
## 常用的DOM操作
  新建：createElement创建建一个元素节点；createTextNode创建一个文本节点
  添加：appendChild添加子节点
  删除：removeChild  删除子节点
  查找：获取元素可以根据标签名获取、也可以根据id、name、class属性来获取。根据id属性获取的结果是一个元素，而其它的获取的是一个集合。
## 回流和重绘
  重绘是当节点需要更改外观而不会影响布局的，比如改变 color 就叫称为重绘
  回流是布局或者几何属性需要改变就称为回流。
  减少重绘和回流：
  1. 使用 translate 替代 top
  2. 使用 visibility 替换 display: none ，因为前者只会引起重绘，后者会引发回流（改变了布局）
  3. 不要使用 table 布局，可能很小的一个小改动会造成整个 table 的重新布局
  4. 动画实现的速度的选择，动画速度越快，回流次数越多，也可以选择使用 requestAnimationFrame
  5. CSS 选择符从右往左匹配查找，避免 DOM 深度过深
## 函数防抖和节流
   防抖  在事件触发n秒后执行，如果n秒内又触发了该事件，就以新事件为准，n秒后执行
```JS
function debounce(func,wait){
	let timeout=null;
    return function () {
    	let context=this;
        let args=arguments;
        clearTimeout(timeout);
        timeout=setTimeout(function () {
            func.apply(context,args);
        },wait)
    }
}
```
   /*节流  持续触发该事件，只执行一次*/
   //定时器设置 再次触发事件时，如果定时器存在则不触发，
   // 直到定时器执行函数，并清空定时器，开启下一轮定时
```JS
    function throttle(func,wait) {
        let timeout;
        return function () {
            let context = this;
            let args=arguments;
            if(!timeout){
                timeout=setTimeout(function () {
                    timeout=null;
                    func.apply(context,args)
                },wait)
            }
        }
    }
```
   //利用时间戳
```
    function throttle1(func,wait) {
        var lastTime=0;
        return function () {
            var context=this;
            var args=arguments;
            var nowTime=new Date().getTime();
            if((nowTime-lastTime)>wait){
                func.apply(context,args);
                lastTime=nowTime;
            }
        }
    }
```
## 实现new
   new一个函数的步骤：
   1. 创建一个空对象作为将要返回的函数实例
   2. 将空对象的原型指向构造函数的原型
   3. 将空对象赋值给构造函数内部的this
   4. 开始执行构造函数内部的代码
      如果构造函数返回值为简单的基本数据类型，而不是一个显示的对象（包括基本类型的对象）的话，那么返回值仍然为新创建的对象。

    function objectFactory() {
      var obj = new Object(),
      Constructor = [].shift.call(arguments);
      obj.__proto__ = Constructor.prototype;
      var ret = Constructor.apply(obj, arguments);
      return typeof ret === 'object' ? ret : obj;
    }；
## cookie webstorage
### cookie
   在客户端记录用户身份，cookie数据始终在同源http请求中携带，会在浏览器和服务器中来回传递。大小为4k，设置的cookie过期时间内一直有效，可以在同源且符合path规则的文档间共享。
   http-only 	不能通过 JS 访问 Cookie，减少 XSS 攻击
   secure 	只能在协议为 HTTPS 的请求中携带
   same-site 	规定浏览器不能在跨域请求中携带 Cookie，减少 CSRF 攻击
### localStorage
   localStorage仅在本地存储，不会发送给服务器。大小为5M，数据长期有效，可以同源共享。
### sessionStorage
   sessionStorage：仅在本地存储，不会发送给服务器。大小为5M，窗口关闭后数据删除，不能共享。
## 跨域
### jsonp
   只能使用get方法，datatype ：jsonp
   原理:可以通过相应的标签从不同的域名下加载静态资源，基于此原理，可以动态创建script标签，请求一个带参网址实现跨域通信。
   原生实现：
   /*jsonp跨域*/
```
<script>
    var script=document.createElement('script');
    script.type='text/javascript';
    //传参并指定回调函数
    script.src='http://www.domain2.com:8080/login?user=admin&callback=onback';
    function onback(res) {
        alert(JSON.stringify(res))
    }
</script>
```
### CROS
   如果不带cookie，则前端不需要设置，如果带cookie则前端后台均需设置。服务器端设置Access-Control-Allow-Origin（可以设置单个或多个域名通过，或者所有域名通过）即可。
   若有cookie，前端原生设置：
```
	var xhr=new XMLHttpRequest();
    //前端设置是否带cookie
    xhr.withCredentials=true;
    xhr.open('post','http://www.domain.com:8080/login',true);
    xhr.send('user=admin');
    xhr.onreadystatechange=function () {
        if (xhr.readyState==4&&xhr.readyState==200){
            alert(xhr.responseText);
        }
    }
```
### document.domain
   适用于二级域名相同的情况下
### postMessage
   用于获取嵌入页面的第三方数据，一个页面发送消息，另一个页面判断来源并接收消息
   // 发送消息端
```
    window.parent.postMessage('message', 'http://test.com')
    // 接收消息端
    var mc = new MessageChannel()
    mc.addEventListener('message', event => {
      var origin = event.origin || event.originalEvent.origin
      if (origin === 'http://test.com') {
        console.log('验证通过')
      }
    })
```
## 原型 原型链 继承
   typeof判断值类型  instanceof判断对象类型（对象、数组、普通函数、构造函数）
   对象是属性的集合
    原型：
   所有的函数都有一个prototype属性，他的属性值是一个对象（属性的集合），默认的叫constructor，指向他自己，可以向prototype中添加自己的属性。
   每个对象还有一个隐藏的属性"__proto__",称为隐式原型。对象的隐式模型指向创建这个对象的原型。fn.__proto__=Fn.prototype
   描述了对象的隐式原型与函数的原型之间的关系：
    ![](https://images0.cnblogs.com/blog/138012/201409/181637013624694.png)
   A instanceof B 沿着A的__proto__来找，同时沿着B的prototype来找，如果找到同一个引用，则为同一个对象。
    原型链：
   当访问一个对象的属性的时候，先在基本属性中找，再顺着__proto__这条链上找。
   Object.create()要添加到新对象的可枚举（新添加的属性是其自身的属性，而不是其原型链上的属性）的属性。

## 执行上下文
   在执行一段代码之前，会进行一些准备工作，这些准备情况称为执行上下文（把所有变量都是先拿出来，有一些直接赋值了，有一些用undefined占位）：
   ### 全局代码的上下文环境：
    变量、函数表达式---变量声明，默认undefined
    this---赋值
    函数声明---赋值
   ### 函数体的上下文环境：
   参数---赋值
   arguments---赋值
   自由变量的取值作用域---赋值
   函数每被调用一次就产生一个新的执行上下文环境

   ### 执行上下文栈
   每次调用函数的时候都会产生执行上下文环境，函数完成时，执行上下文被删除，重新回到只有全局上下文的状态。
## this
   this的取值实在函数真正被调用执行的时候确定的
    构造函数
   this代表它即将new出来的对象
    函数作为对象的一个属性：
   this指向该对象
    函数用call或apply调用
   this的值取传入的对象的值
    全局或调用普通函数
   this指向window

## 作用域
   JavaScript除了全局作用域之外，只有函数可以创建作用域。
   作用域在函数定义的时候就已经确定了。
   如果要查找一个作用域下某个变量的值，需要找到这个作用域对应的执行上下文环境，再在其中寻找变量的值。
   当查找变量的时候，会先从当前上下文的变量对象中查找，如果没有找到，就会从父级(词法层面上的父级)执行上下文的变量对象中查找，一直找到全局上下文的变量对象，也就是全局对象。这样由多个执行上下文的变量对象构成的链表就叫做作用域链。

## 自由变量
   在A作用域下面使用了变量x，却没有在A作用域中声明，对A作用域来说，x是自由变量。
   自由变量要到创建这个函数的那个作用域取。

## 闭包
   定义在一个函数内部的函数。
   一种函数作为返回值传递；另一种函数作为参数被传递
   函数调用完成后，其上下文环境不会被摧毁
   创建闭包环境，将i值存在闭包中
   ```
    function clickByOneS() {
        for (var i=0;i<5;i++){
            (function (i) {
                setTimeout(function(){ console.log(i);},i*1000)
            })(i)
        }
    }

    clickByOneS();
```
## 事件循环机制
   执行函数前先入栈，执行后出栈，如果遇到了异步函数，先将他们放在浏览器的其他模块去执行，执行之后，把回调函数放入任务队列，当所有调用栈执行完之后再去执行任务列中的函数。在这之中，需要先执行为微任务（process.nextClick\promise），再执行宏任务(setTimeout\setInterval>setImmediate)。
  1. 执行同步代码，这属于宏任务
  2. 执行栈为空，查询是否有微任务需要执行
  3. 执行所有微任务
  4. 必要的话渲染 UI
  5. 然后开始下一轮 Event loop，执行宏任务中的异步代码
## 变量对象
  ### 为什么0.1+0.2不等于0.3？
   因为计算机不能精确表示0.1， 0.2这样的浮点数，计算时使用的是带有舍入误差的数
   并不是所有的浮点数在计算机内部都存在舍入误差，比如0.5就没有舍入误差
   具有舍入误差的运算结可能会符合我们的期望，原因可能是“负负得正”
## 堆和栈
   栈：后进先出，由程序员分配释放，存放基本类型
   堆：先进先出，由操作系统分配释放，引用类型
## 深浅拷贝
   JSON.stringfy()变成值类型  JSON.parse()变成对象类型（ 忽略 undefined、   会忽略 symbol、   不能序列化函数、   不能解决循环引用的对象）
   循环递归自己创建深拷贝
   ```
    function deepClone(origin) {
        var target=Array.isArray(origin)?[]:{};
        if (origin&&typeof origin==="object"){
            for(var i in origin){
                var prop=origin[i];
                if(prop==origin){
                    continue;
                }
                if(origin.hasOwnProperty(i)){
                    if(prop&&typeof prop==="object"){
                        var target=Array.isArray(origin)?[]:{};
                        target[i]=deepClone(prop);
                    }else {
                        target[i]=prop;
                    }
                }
            }
        }
        return target
    }
```
   浅拷贝
   数组slice（） 数组concat()  遍历 对象Object.assign({},origin) 对象扩展运算符return {...origin}

## 数组的方法
  会改变自身：
   pop()、push()、reverse()、shift()、unshift()、sort()、splice()
  不会改变自身
   concat()、join()、slice()、toString()、indexOf()
  遍历方法
   foreach()、entries()、filter()、keys()、map()、reduce()
## 数组去重
   filter过滤
   ```
    function filterData(obj){
        obj.filter(function(value,index,self){
            return self.indexof(value)===index;
        })
    }
```
   ES6的set
    利用Array.from()将set结构转换为数组
```
    function dedepe(array){
        return Array.from(new Set(array));
    }
```
   使用扩展运算符
    let result=[...new Set(array)]
### 数组扁平化
## get和post
   get后退无害，post会重新加载并提醒用户是否重新提交表单
   get能被缓存，post不能
   get通过URL传递，POST放在request body中
   get的参数会完整的保存在浏览器历史记录里，而post不会，因此post比get更安全
   get传递的参数长度有限，post没有
   在用法上一个用于获取数据，一个用于更改数据
   在根本上二者并没有什么区别，使用哪个方法与数据如何传输没有相互关系
   post可以发送的文件类型
   1. application/x-www-form-urlencoded
      发送前编码所有字符
   2. multipart/form-data
      键值对，文件
   3. application/json
      序列化后的 JSON 字符串
   4. text/xml
      xml
## SEO
   合理的title description keywords
   语义化的HTML代码，让搜索引擎更容易理解代码
   重要的HTML代码放在前面
   重要内容不要用js输出，爬虫不会获取js内容
   少用iframe，不会抓取iframe内容
   非装饰性图片加alt
   提高网站速度(CDN与DNS)
## 网络优化
   合并资源文件，减少HTTP请求
   缓存分类
   DNS预解析
   CDN
   预加载
   图片优化（不用图片，使用css；雪碧图；采用正确的图片格式）

## 那些操作会造成内存泄漏
  JS内存泄漏指对象在不需要使用它时仍然存在，导致占用的内存不能够被使用或回收。
   未使用var声明的全局变量
   闭包函数
   循环引用（两个对象相互引用）
   控制台日志（console.log）
   移除存在事件绑定的DOM元素（IE）
## fetch与ajax
  Ajax：利用的是XMLHttpRequest对象来请求数据的。
  Fetch：window对象的一个方法，主要特点是：
   第一个参数是URL
   第二个参数可选，可以控制不同的init对象
   使用了es6的promise对象
  fetch和ajax的主要区别
   当接收到一个代表错误的HTTP状态码时，从fetch()返回的Promise不会被标记为reject，即便该HTTP响应的状态码是404或500。相反，它会将Promise状态标记为resolve（但是会将resolve的返回值的ok属性设置为false），仅当网络故障时或请求被阻止时，才会标记为reject。
   在默认情况下fetch不会从服务器端发送或接收任何cookies
## ==运算符
  字符串和数字之间的比较，是将字符串转换为数字
  其他类型和布尔类型的比较，是将布尔类型转换为数字
  在 == 中null 和 undefined 相等（他们也与自身相等），除此之外其他值都不存在这种情况
  对象和非对象之间的比较，调用ToPrimitive将对象转换为原始类型，ToPrimitive操作规则如下：
  ToPrimitive(obj)等价于：先计算obj.valueOf()，如果为原始值，则返回此结果；
  否则，计算**obj.toString()**，如果结果是原始值，则返回此结果；否则，抛出异常。
## 假值
  undefined null NAN 0 false ""
## XML和JSON
  解码难度：JSON的解码难度远小于XML
  数据体积：JSON数据体积小，传递的速度比较快
  数据交互：JSON与JavaScript交互更加方便，更容易解析处理
  数据描述：XML对数据的描述性更好
  传输速度：JSON速度远远快于XML

## 数组对象的常见方法
  不改变原数组：
   concat():返回被连接数组的一个副本
   join(separator):把所有元素放入一个字符串中
   .slice(start,end):包含start不包含end
   toString():把一个逻辑值转为字符串，并返回结果
   filter(function(cur,index,arr){})
  改变元素组：
   pop():用于删除并返回数组的最后一个元素
   push():在数组的末尾添加一个或多个元素，返回添加之后数组的长度
   shift():删除并返回数组的第一个元素
   unshift():向数组的开头添加一个或多个元素，返回数组的新长度
   splice(index,howmany,item1,..,itemx):向数组中添加或从数组中删除项目
   reserve():颠倒数组中的顺序
  数组的原型上的方法：
   indexof：元素第一个出现的位置
   lastIndexof:元素最后出现的位置    
  ES6新特性：
   Array.of()
   Array.from()

## for..of for...in区别：
   for...of无法遍历对象
   for...of遍历数组的值，而for...in遍历数组的键值(索引)
   for...in会遍历自定义的属性，for...of不会
## 斐波那契数列实现
```
    function* fibonacci() {
        let [prev, curr] = [0, 1];
        for (;;) { // 这里请思考：为什么这个循环不设定结束条件？
            [prev, curr] = [curr, prev + curr];
            yield curr;
        }
    }
    for (let n of fibonacci()) {
        if (n > 1000) {
        	break;
        }
        console.log(n);
    }
```
## 尾递归
## 输入一个网址之后发生了什么
   首先进行DNS域名解析，然后进行TCP连接+HTTP请求，接着浏览器渲染页面，最后断开连接。
### TCP请求三次握手
   ACK：验证字段   SYN：表示建立TCP连接    FIN：表示断开TCP连接
   ![](https://s1.51cto.com/images/blog/201801/26/cf32189db2b594d6f239ea75d4f7b652.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)

### 浏览器渲染页面
   ![](https://user-images.githubusercontent.com/25027560/46640050-6420ad80-cb9c-11e8-991f-4f039e0eb4a9.png)
   处理HTML标记并构建DOM树
   处理CSS标记构建CSSOM树
   处理CSSOM和DOM树合并成一个渲染树
   根据渲染树布局，以计算每个节点的几何信息
   调用GPU绘制，合成图层，显示在屏幕
   DOMContentLoaded表示初始的HTML加载完，不需要等css，js和图片的加载,绑定在document上面
   load代表HTML完全加载完，绑定在window上
   jquery的ready方法：当dom已经完全加载，并且页面的内容已经完全呈现的时候触发ready事件
   defer 载入js时不阻塞HTML的解析，执行阶段在html解析后
   async 载入后即执行，在 DOMContentLoaded之前之后不确定，但在loaded之前执行

### TCP四次挥手
   ![](https://s1.51cto.com/images/blog/201801/26/7b0328d95ac0eb055d1e38e0e27e0ba8.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)
## 进程/线程
   进程：程序的一次执行，CPU能处理的单个任务
   线程：一个进程可以包括多个线程，一个进程的内存空间是共享的，每个线程都可以使用这些共享内存，CPU的基本调度单位

1. 简而言之，一个程序至少有一个进程，一个进程至少有一个线程
2. 线程的划分尺度小于进程，使得多线程应用程序的并发性高
3. 进程在运行过程中拥有独立的内存单元，而多个线程共享内存，极大地提高了程序的运行效率
4. 每个独立的线程有一个程序运行的入口、顺序执行序列和程序的出口。但是线程不能独立运行，必须依存在应用程序中，由应用程序提供多个线程执行控制
5. 从逻辑角度来看，多线程的意义在于一个应用程序中，有多个执行部分可以同时执行。但操作系统并没有将多个线程看作是多个独立的应用，来实现进程的调度和管理以及资源分配

## js异步的几种方式
   1. 回调函数
   2. 订阅发布
      执行一个任务进行通知，订阅该任务的都可以接收该通知
   3. promise
      promise.then()链式调用
   4. generator
      调用时不会立即执行，.next()才会执行，会打破暂停状态去执行，直到遇到下一个yield或者return
      遇到yield时，会执行yeild后面的表达式，并返回执行之后的值，然后再次进入暂停状态，此时done: false。
      遇到return时，会返回值，执行结束，即done: true
## ES6
### 新特性
  let和const、promise、参数默认值、箭头函数、类
### var let const
   全局申明的var会挂载在window上，let和const不会
   var存在变量提升，let和const不会
   let、const是块级作用域，var是函数作用域
   同一作用域下let和const不能声明同名变量，var可以
   同一作用域下，let和const在声明前使用存在暂时性死区
   const：一旦声明必须赋值；声明后不可改变；如果是复合型，可改变其属性
### map
  map数据结构是键值对的集合，键的范围不局限于字符串。
  set和map可以用来生成新的map
  ```
    const set = new Set(
       ['foo', 1]，
       ['bar', 2]
    ]);
    const m1 = new Map(set);
    m1.get('foo') // 1

    const m2 = new Map([['baz', 3]]);
    const m3 = new Map(m2);
    m3.get('baz') // 3
```
  map的键与内存地址绑定在一起，如果键是一个简单类型的值，只要严格相等，则视为一个键。
  map方法：size(),set(key,value),get(key),has(key),delete(key),clear()
  遍历：keys(),values(),entries(),forEach()
  map转为数组：使用扩展运算符
  数组转为map：将数组传入map构造函数
  map转为对象：
```
    function strMapToObj(strMap) {
    let obj = Object.create(null);
    for (let [k,v] of strMap) {
    obj[k] = v;
    }
    return obj;
    }
```
  对象转为map
  ```
    function objToStrMap(obj) {
    let strMap = new Map();
    for (let k of Object.keys(obj)) {
    strMap.set(k, obj[k]);
    }
    return strMap;
    }
```
  map转json
  map的键名均为字符串
```
    function strMapToJson(strMap){
        return JSON.stringify(strMapToObj(strmap))
    }
```
  map键名含有非字符串
    function mapToArrayJson(map){
        return JSON.stringify(...[map]);
    }
  Json转为map
  键名均为字符串
```
    function jsonToStrMap(jsonStr){
        return objToStrMap(JSON.parse(jsonStr))
    }
```
  整个json是个数组，每个数组成员包含两个成员，可以一一对应的转换为map
```
    function jsonToMap(jsonStr){
        return new map(JSON.parse(jsonStr))
    }
    jsonToMap('[[true,7],[{"foo":3},["abc"]]]')
    // Map {true => 7, Object {foo: 3} => ['abc']}
```
### get
   存储无重复值的有序列表
### set
   是一个构造函数，用于生成set数据结构，类似于数组，但成员没有重复的值
   不会进行类型转换，5和‘5’是不同的值，Set 内部判断两个值是否不同，使用的算法叫做“Same-value-zero equality”，它类似于精确相等运算符（===），主要的区别是NaN等于自身，而精确相等运算符认为NaN不等于自身。
   add(),delete(),has(),clear()
   Array.from(new set([1,2,2,2]))可将set转为数组
   遍历：keys()根据键名   values()根据键值  entries()根据键值对  forEach()使用回调函数遍历每个成员 set结构没有键名，所以keys()和values()一样
   可以利用set求交集、并集和差集
   // 并集
    let union = new Set([...a, ...b]);
   // 交集
    let intersect = new Set([...a].filter(x => b.has(x)));
   // 差集
    let difference = new Set([...a].filter(x => !b.has(x)));
   weakSet只能接受对象，不能是值或symbol等

### 类数组
   可以通过索引访问对象，并且有length属性，但是没有数组的方法，push、indexof等
   可以通过Array.prototype.slice.call 、Array.from()、扩展运算符转换

### filter
   filter():过滤不符合条件的元素
### reduce
  reduce(function（total,currentValue,index,arr),initalValue)：total作为返回值（上一次回调返回的值）
### 箭头函数
   普通函数的声明有函数提升，箭头函数没有
   箭头函数没有自己的this，arguments，this继承它作用域父级的this
   箭头函数不能作为构造函数
   call和apply方法只有参数没有作用域
### 继承
JavaScript继承是依靠prototype实现的，先面介绍六种继承方式并介绍其优缺点，参考自[JavaScript常见的几种继承方式](https://segmentfault.com/a/1190000016708006)
先定义一个父类
```
  //父类
  function Person(name){
      this.name=name;
      this.sum=function(){
          alert(this.name)
      }
  }
  Person.peototype.age=10;
```
1.原型链继承
```
  //子类
  function Stu(){
      this.name="ker";
  }
  Stu.prototype=new Person(); //子类的原型为父类的一个实例对象
  var stu1=new Stu();
  console.log(stu1.age)  //10
```
方法：让子类的原型等于父类的一个实例对象。

特点：简单易于实现，子类能够访问到父类新增的原型属性和方法

缺点：子类无法向父类传参，想要向子类新增属性的时候必须在Stu.prototype=new Person();之后执行，来自原型对象的所有属性被所有实例共享，无法实现多继承。

2.借用构造函数继承
  function Stu(){
      Person.call(this,"jer");
      this.age=12;
  }
  var stu1=new Stu();
  console.log(stu1.name); //"jer"
  console.log(stu1 instanceof Person) //"false"
方法：使用call和apply方法在子类构造函数中调用父类构造函数

特点：可以实现多继承，创造子类实例时可以向父类传递参数，无法继承父类原型属性

缺点：只能继承父类的实例属性和方法，不能继承原型属性和方法，无法实现函数复用，每个子类都有父类实例函数的副本，影响性能。

3.组合继承（原型链+构造函数）
```
  function Stu(name){
      Person.call(this,arguments);
  }
  Stu.prototype=new Person();
  var stu1=new Stu("jer")
  console.log(stu1.name); //"jer"
  console.log(stu1.age); //10
```
方法：将上述两种方式结合

特点：可以传参和复用，每个新实例引入的构造函数属性是私有的

缺点：调用了两次父类构造函数，生成了两份实例。

4.寄生继承方法
```
  //用于创建一个对象副本
  function content(obj){
      function F(){};
      F.prototype=obj;
      return new F();
  }
  var stu=new Person();
  //上面是原型式继承：无法复用
  function stuobject(obj){
      var stu=content(obj);
      stu.name="jer";
      return sub;
  }
  var stu2=stuobject(stu);
```
方法：子类的原型指向父类副本的实例，从而实现原型共享

使用Object.create(父对象)，可以传参，无法复用。

5.寄生组合方式（寄生+构造）
```
  function content(obj){
      function F(){};
      F.prototype=obj;
      return new F();
  }
  var con=content(Person.prototype);
  function Stu(){
      Person.call(this);
  }
  Stu.prototypr=con;
  con.constuctor=Stu;
```

6.ES5：
```
    function Super() {}
    Super.prototype.getNumber = function() {
      return 1
    }

    function Sub() {}
    let s = new Sub()
    Sub.prototype = Object.create(Super.prototype, {
      constructor: {
        value: Sub,
        enumerable: false,
        writable: true,
        configurable: true
      }
    })
```

   ES5实现思路就是将子类的原型设置为父类的原型
   
7.ES6使用class实现
```
    class MyDate extends Date {
      test() {
        return this.getTime()
      }
    }
    let myDate = new MyDate()
    myDate.test()
```
### promise
   可以看作是一个存着未来才会结束的事件的结果，可以获取异步操作的消息。
   等待中（pending）完成了（resolved）拒绝了（rejected），一旦状态改变就不会再回退
   构造promise的内部代码是立即执行的，实现了链式调用，解决了地狱回调问题，但是无法取消promise，错误需要回调函数获取（catch）
   promise.all() 包含一个含有多个promise对象的数组， 一起执行结束之后，再做下一步操作，如果全部成功执行，则以数组的方式返回所有`Promise`任务的执行结果；如果有一个`Promise`任务 `rejected`，则只返回 `rejected `任务的结果。。
   promise.race() 包含一个含有多个promise对象的数组，其中一个执行结束之后就可进行下一步操作。
   readyState：0请求未初始化  1服务器连接已建立 2请求已接收 3请求处理中  4请求已完成
   封装ajax
```
    /*promise封装ajax*/
    const getJson=function (url) {
        const promise=new Promise(function (resolve, reject) {
            const handler=function () {
                if(this.readyState==4&&this.status==400){
                    console.log(this.response);
                }else {
                    console.log(this.statusText)
                }
            }
            const xhr=new XMLHttpRequest();
            xhr.open("GET",json);
            xhr.onreadystatechange=handler;
            xhr.responseType("json")
            xhr.setRequestHeader("Accept","application/json");
            xhr.send();
        })
    }
    function postJson(url,data) {
        return new Promise(function (resolve,reject) {
            const xhr=new XMLHttpRequest();
            xhr.open("POST",url,true);
            xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded");
            xhr.onreadystatechange=function () {
                if(this.readyState===4){
                    if(this.status===200){
                        resolve(JSON.parse(this.responseText))
                    }else {
                        reject(this.statusText)
                    }
                }
            }
            xhr.send(JSON.stringify(data))
        })
    }
    getJson("/XXX").then(function (json) {
        console.log(json)
    }).catch(function (erro) {
        console.log(erro);
    })
```
### async和await
   函数加上async，函数会返回一个promise，await只能和async配套，相对于promise代码更清晰
   是generator和promise的语法糖
   promise中不能使用自定义的try/catch,async和await可以
### Generator生成器
   返回一个迭代器，第一次执行next(),传参会被忽略。当yield产生一个值后，生成器的执行上下文会从栈中弹出，但迭代器中保持着对执行上下文的引用，所以执行上下文不会消失
### 实现apply和bind
   bind
```
    Function.prototype.myOwnBind=function (context) {
        var self=this;
        var args=[].slice.call(arguments,1);
        var f=function () {     
        }
        var bandFunc=function () {
            var _args=[].slice.call(arguments);
            self.apply(this instanceof self?this:context,args.concat(_args));
        }
        f.prototype=this.prototype;
        bandFunc.prototype=new f();
    }
```
   apply改变了 this 指向，让新的对象可以执行该函数，相当于给新对象一个函数，执行之后删除，apply可以接受一个参数数组，call可以接受一个参数列表
```
    Function.prototype.myOwnApply=function(context){
        context=context||window;
        context.fn=this;
        let result;
        if(arguments[1]){
            result=context.fn(...arguments[1]);
        }else {
            result=context.fn();
        }
        delete context.fn;
        return result;
    }
```
### 模块化
  ES6的module：export和import。export命令用于规定模块的对外接口，import命令用于输入其他模块提供的功能。
