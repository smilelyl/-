# JS知识要点
## BOM
   浏览器对象模型，对浏览器窗口进行访问和操作
    Windows浏览器窗口（会获取文本框宽度、浏览器窗口宽度、打开关闭窗口）
    navigator包含客户端浏览器信息（包括浏览器名称、版本号、操作系统）
    sceen客户端显示屏信息
    history浏览器窗口访问过的URL（history.back返回上次访问地址，history.back(2)返回上上次）
    location当前URL的信息（浏览器中的地址栏，可以显示端口号服务器等）
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
    e.target指向引发触发事件的元素（点击的li），e.currentTarget指向的是给绑定事件监听的那个对象（ul）
## 常用的DOM操作，新建、添加、删除、移动、查找等
## 函数防抖和节流
   /*防抖  在事件触发n秒后执行，如果n秒内又触发了该事件，就以新事件为准，n秒后执行*/
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
   /*节流  持续触发该事件，只执行一次*/
   //定时器设置 再次触发事件时，如果定时器存在则不触发，
   // 直到定时器执行函数，并清空定时器，开启下一轮定时
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
   //利用时间戳
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
      <script>
          var script=document.createElement('script');
          script.type='text/javascript';
          //传参并指定回调函数
          script.src='http://www.domain2.com:8080/login?user=admin&callback=onback';
          function onback(res) {
              alert(JSON.stringify(res))
          }
      </script>
### CROS
   如果不带cookie，则前端不需要设置，如果带cookie则前端后台均需设置。服务器端设置Access-Control-Allow-Origin即可。
   若有cookie，前端原生设置：
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
## 自由变量
   在A作用域下面使用了变量x，却没有在A作用域中声明，对A作用域来说，x是自由变量。
   自由变量要到创建这个函数的那个作用域取。
## 闭包
   定义在一个函数内部的函数。
   一种函数作为返回值传递；另一种函数作为参数被传递
   函数调用完成后，其上下文环境不会被摧毁
   创建闭包环境，将i值存在闭包中
    function clickByOneS() {
        for (var i=0;i<5;i++){
            (function (i) {
                setTimeout(function(){ console.log(i);},i*1000)
            })(i)
        }
    }

    clickByOneS();
## 事件循环机制
   执行函数前先入栈，执行后出栈，如果遇到了异步函数，先将他们放在浏览器的其他模块去执行，执行之后，把回调函数放入任务队列，当所有调用栈执行完之后再去执行任务列中的函数。
## 变量对象
1. 为什么0.1+0.2不等于0.3？
   因为计算机不能精确表示0.1， 0.2这样的浮点数，计算时使用的是带有舍入误差的数
   并不是所有的浮点数在计算机内部都存在舍入误差，比如0.5就没有舍入误差
   具有舍入误差的运算结可能会符合我们的期望，原因可能是“负负得正”
## 堆和栈
   栈：后进先出，由程序员分配释放，存放基本类型
   堆：先进先出，由操作系统分配释放，引用类型
## 深浅拷贝
   JSON.stringfy()变成值类型  JSON.parse()变成对象类型
   循环递归自己创建深拷贝
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
   浅拷贝
   数组slice（） 数组concat()  遍历 对象Object.assign({},origin) 对象扩展运算符return {...origin}
## 数组去重
    filter过滤
    function filterData(obj){
        obj.filter(function(value,index,self){
            return self.indexof(value)===index;
        })
    }
    ES6的set
    利用Array.from()将set结构转换为数组
    function dedepe(array){
        return Array.from(new Set(array));
    }
    使用扩展运算符
    let result=[...new Set(array)]
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
    Array.fraom()
## for..of for...in区别：
    for...of无法遍历对象
    for...of遍历数组的值，而for...in遍历数组的键值(索引)
    for...in会遍历自定义的属性，for...of不会
## 斐波那契数列实现
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
   DOMContentLoaded表示初始的HTML加载完，不需要等css，js和图片的加载
   load代表HTML完全加载完
   defer 载入js时不阻塞HTML的解析，执行阶段在html解析后
   async 载入后即执行，在 DOMContentLoaded之前之后不确定，但在loaded之前执行
### TCP四次挥手
   ![](https://s1.51cto.com/images/blog/201801/26/7b0328d95ac0eb055d1e38e0e27e0ba8.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_100,g_se,x_10,y_10,shadow_90,type_ZmFuZ3poZW5naGVpdGk=)
## 进程/线程
   进程：程序的一次执行，CPU能处理的单个任务
   线程：一个进程可以包括多个线程，一个进程的内存空间是共享的，每个线程都可以使用这些共享内存，CPU的基本调度单位
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
    const set = new Set(
       ['foo', 1]，
       ['bar', 2]
    ]);
    const m1 = new Map(set);
    m1.get('foo') // 1

    const m2 = new Map([['baz', 3]]);
    const m3 = new Map(m2);
    m3.get('baz') // 3
  map的键与内存地址绑定在一起，如果键是一个简单类型的值，只要严格相等，则视为一个键。
  map方法：size(),set(key,value),get(key),has(key),delete(key),clear()
  遍历：keys(),values(),entries(),forEach()
  map转为数组：使用扩展运算符
  数组转为map：将数组传入map构造函数
  map转为对象：
    function strMapToObj(strMap) {
    let obj = Object.create(null);
    for (let [k,v] of strMap) {
    obj[k] = v;
    }
    return obj;
    }
  对象转为map
    function objToStrMap(obj) {
    let strMap = new Map();
    for (let k of Object.keys(obj)) {
    strMap.set(k, obj[k]);
    }
    return strMap;
    }
  map转json
  map的键名均为字符串
    function strMapToJson(strMap){
        return JSON.stringify(strMapToObj(strmap))
    }
  map键名含有非字符串
    function mapToArrayJson(map){
        return JSON.stringify(...[map]);
    }
  Json转为map
  键名均为字符串
    function jsonToStrMap(jsonStr){
        return objToStrMap(JSON.parse(jsonStr))
    }
  整个json是个数组，每个数组成员包含两个成员，可以一一对应的转换为map
    function jsonToMap(jsonStr){
        return new map(JSON.parse(jsonStr))
    }
    jsonToMap('[[true,7],[{"foo":3},["abc"]]]')
    // Map {true => 7, Object {foo: 3} => ['abc']}
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
### filter
   filter():过滤不符合条件的元素
### reduce
  reduce(function（total,currentValue,index,arr),initalValue)：total作为返回值（上一次回调返回的值）
### 箭头函数
   普通函数的声明有函数提升，箭头函数没有
   箭头函数没有自己的this，arguments this继承它作用域父级的this
   箭头函数不能作为构造函数
   call和apply方法只有参数没有作用域
### promise
   等待中（pending）完成了（resolved）拒绝了（rejected），一旦状态改变就不会再回退
   构造promise的内部代码时立即执行的，实现了链式调用，解决了地狱回调问题，但是无法取消promise，错误需要回调函数获取（catch）
   promise.all() 包含一个含有多个promise对象的数组， 一起执行结束之后，再做下一步操作。
   promise.race() 包含一个含有多个promise对象的数组，其中一个执行结束之后就可进行下一步操作。
   readyState：0请求未初始化  1服务器连接已建立 2请求已接收 3请求处理中  4请求已完成
   封装ajax
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
### async和await
   函数加上async，函数会返回一个promise，await只能和async配套，相对于promise代码更清晰
   是generator和promise的语法糖
### Generator生成器
   返回一个迭代器，第一次执行next(),传参会被忽略。当yield产生一个值后，生成器的执行上下文会从栈中弹出，但迭代器中保持着对执行上下文的引用，所以执行上下文不会消失
### 实现apply和bind
   bind
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
   apply
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
