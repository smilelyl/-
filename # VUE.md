# VUE 
## MVVM与MVC
1. MVVM
     model view view-model
       model 数据模型，数据和业务模型都在model中定义
       view ui视图，数据的展示
       view-model 监听model的数据变化控制视图的更新，处理用户交互操作
       model和view之间通过view-model进行联系，model和view双向数据绑定。model中数据改变时会触发view的刷新，view中操纵带来的数据改变也会在model中同步。用户不需要自己操作dom，只需要专注于对数据的维护操作即可。
2. MVC
   MVC中的model和controller与view强耦合，view中数据变化时，通知controller，controller向后台model发起请求，model获取数据传递给controller，controller进行一些处理渲染view。
   MVVM无需操作，vm的数据变化通过数据双向绑定，view直接变化。
## 数据劫持
   通过Object.defineProperty()劫持各个属性的setter和getter，监测到数据变动时发布消息给订阅者，触发相应的监听回调。
   vue监听的是对象的属性变化，如果监听数组，则在observer数据阶段会进行判断，如果是数组的则修改数组的原型，后续对数组的操作可以在劫持过程中控制。
   proxy与Object.defineProperty()对比：
   vue可以监听到数组的以下八种方法：push、pop、shift、unshift、splice、sort、reserve，其他属性无法检测
   proxy可以监听数组，返回一个新的对象，只操作新的对象
## 前端路由
   1. hash模式
      ##后面的哈希值发生变化时，不会向服务器发送请求，通过haschange事件监听到url变化进行页面跳转
      点击跳转或浏览器历史跳转->触发haschange-》跳转-》dom替换的方式更改页面
      手动刷新-》不会向服务器发送请求也不会触发haschange事件，通过load-》跳转页面-》dom替换的方式更改页面
      ![](https://user-gold-cdn.xitu.io/2018/7/11/164888109d57995f?w=942&h=493&f=png&s=39581)

   2. history模式
      比hash更美观
      利用h5的history中新增的pushState和replaceState和一个onpopstate事件
      pushState：向history对象中加入一个新的记录，replaceState 是替换history对象中的当前历史
      回退或前进会触发onpopstate事件
      ![](https://user-gold-cdn.xitu.io/2018/7/11/164888478584a217?w=1244&h=585&f=png&s=59637)
   3. 区别
      刷新浏览器或手动输入的时候，hash查询#号前，并不会返回404，history前端和后端的url必须一致，否则返回404，需要后端设置一下如果匹配不到静态资源则返回index.html
      pushState可以添加任意数据，hash只能加入短字符串
## vue的响应式原理
   当创建一个vue实例的时候，vue会遍历data属性，通过Object.defineProperty()的get和set在内部追踪相关依赖，在属性被访问和修改时通知变化。每个组件实例都有一个watcher实例，在组建渲染时把属性记录为依赖，当依赖项的setter调用时，通知watcher重新计算，使之关联的组件得以更新。
## 父子组件之间的通信
   父组件通过props传值给子组件，子组件通过$emit通知父组件修改相应的props值
## vue的指令
   v-html v-show v-if v-for
## v-if和v-show
   v-show控制元素的显示方式，v-if会控制dom节点的存在与否，需要经常切换时使用v-show，只需一次显示或隐藏时使用v-if
## 给data对象属性中添加一个新的属性时，视图未刷新，原因是新建的对象未声明，没有被vue转换为响应式的属性，需要使用$set()
## delete和vue.delete
   delete删除的元素变为了empty/undefined，其他元素键值不变
   vue.delete直接删除了该元素，改变了数组的键值
##  虚拟dom
   每一次 DOM 的变动，浏览器就得重新渲染一次页面。为了提高页面的性能，就应该尽量减少 DOM 的变更次数。现代框架通常会用一个对象来保存目标 DOM 节点的标签名、属性、内容、子节点等信息，也就是用 js 的对象结构来表示 DOM 树的结构，这个 js 对象就是虚拟 DOM。当状态变更的时候，js 会先更新虚拟 DOM，再通过 diff 算法比较虚拟 DOM 和真实 DOM 的差异，找出最少变更的方案，最后一并更新到真实 DOM 中。
   优势：更快。如果全程操作真实 DOM 的话，任何一个状态的变更，都会导致页面重绘，这个环节就非常耗性能了。采用虚拟 DOM 的话，就能避免这个问题。而且如果 diff 算法效率高的话，总是能用最少的改动来更新 DOM。总的来说，就是不会出现频繁的、大面积的 DOM 操作，从而提升了页面性能。
   算法的实现：1. 通过 JS 来模拟创建 DOM 对象 2. 判断两个对象的差异 3. 渲染差异
## Vue的diff过程
## 生命周期
   beforeCreated：实例刚刚被创建，data等组件属性还未计算
   created：data属性已绑定，dom未生成，el属性未存在
   beforeMount：即将挂载，dom生成，el获取到了关联节点，data未渲染
   mounted：完成挂载，data也成功被渲染
   beforeUpdate：修改data，更新渲染视图前会触发beforeUpdate
   update：数据更新完毕
   beforeDestroy：销毁当前组件时，会触发
   destroyed：销毁完毕
## compile
## 依赖收集
## compute和data区别
## nextTick
## vuex

  
