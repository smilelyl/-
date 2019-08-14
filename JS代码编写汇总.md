JS代码编写汇总

# 数组

## 数组去重

1. filter

```
function test(arr){
    return arr.filter(function(item,index,array){
        return array.indexOf(item,0)==index;
    })
}
```

2. reduce

```
function test(arr){
    return arr.reduce(function(pre,cur){
        return pre.includes(cur)?pre:[...pre,cur]
    },[])
}
```

3. set

```
function test(arr){
    [...set(arr)];
}
```

4. indexOf

```
function test(arr){
    var result=[];
    for(var i=0;i<arr.length;i++){
        if(result.indexOf(arr[i])==-1){
            result.push(arr[i])
        }
    }
    return result;
}
```

5. includes

```
function test(arr){
    var result=[];
    for(var i=0;i<arr.length;i++){
        if(result.includes(arr[i])===false){
            result.push(arr[i])
        }
    }
    return result;
}
```

6. map

```
function test(arr){
    var result=[];
    var map=new Map();
    for(var i=0;i<arr.length;i++){
        if(map.has(arr[i])){
            
        }else{
            map.set(arr[i]);
            result.push(arr[i]);
        }
    }
    return result;
}
```

## 数组扁平化

1. 循环

```
function test(arr){
	var result=[];
    for(var i=0;i<arr.length;i++){
        var item=arr[i];
        if(Array.isArray(item)){
            result=result.concat(test(item));
        }else{
            result.push(item);
        }
    }
    return result;
}
```

2. reduce

```
function test(arr){
     return pre=pre.concat(Array.isArray(cur)?test(cur):cur);
}
```

3. toString()

```
function test(arr){
   return arr.toString().split(',').map((item)=>{
        return Number(item);
   })
}

```

4. join()也可将数组转为字符串

```
function test(arr){
   return arr.join(',').split(',').map((item)=>{
        return Number(item);
   })
}

```

5. 扩展运算符

```
function test(arr){
    while(arr.some((item)=>Array.isArray(item))){
        arr=[].concat(...arr)
    }
    return arr;
}

```

6. 按照深度展开数组
   扩展运算符每次展开一层

```
function test(arr,num){
    for(var i=0;i<num;i++){
        arr=[].concat(...arr)
    }
    return arr;
}

```

循环

```
function flat(arr,num){
    var result=[];
    for(var i=0;i<arr.length;i++){
       var numtemp=num;
       if(Array.isArray(arr[i])){
           if(numtemp>0){
               numtemp--;
               result=result.concat(flat(arr[i], numtemp));
           }else{
               result.push(arr[i])
           }
       }else{
               result.push(arr[i])
       }
    }
    return result;
}

```

# 排序

## 快排(最好nlogn，最坏n^2，平均nlogn)

```
function quickSort(arr){
    if (arr.length <= 1) {//如果数组长度小于等于1无需判断直接返回即可
        return arr;
    }
    var temp=arr[0];
    var left=[];
    var right=[];
    var result=[];
    for(var i=1;i<arr.length;i++){
        if(arr[i]<temp){
            left.push(arr[i]);
        }else{
            right.push(arr[i]);
        }
        }
   result=quickSort(left).concat(arr[0],quickSort(right));
   return result;
}

```

## 冒泡（最好n，最坏n^2，平均n^2）

```
function sort(arr){
    for(var i=0;i<arr.length-1;i++){
        for(var j=i+1;j<arr.length;j++){
            if(i>j){
                temp=arr[i];
                arr[i]=arr[j];
                arr[j]=temp;
            }
        }
    }
    return arr;
}

```

# 算法

## 斐波那契数列实现

```
function fibo(n){
	if(n==0||n==1){
        return n;
	}
    var arr=[];
    arr[0]=0;
    arr[1]=1;
    for(var i=2;i<n;i++){
        arr[i]=arr[i-1]+arr[i-2];
    }
    return arr;
}

```

## 找出数组中两数和为n的数字（map）

```
function test(arr,n){
	var map=new Map();
	var result=[];
    for(var i=0;i<arr.length;i++){
        map.set(arr[i],i);
    }
    for(var i=0;i<arr.length;i++){
        if(map.has(n-arr[i])){
            result.push(arr[i]);
            result.push(arr[map.get(n-arr[i])]);
            return result
        }
    }
    return result;
}

```

## 找出最大子序列和（动态规划）

```
function test(arr){
	var dp[0] =arr[0];
	var max=arr[0];
    for(var i=1;i<arr.length;i++){
        if(dp[i-1]<0){
			d[i]=arr[i];
        }else{
            dp[i]=dp[i-1]+arr[i]
        }
        max=Math.max(dp[i],max)
    }
    return max;
}

```

## 多层嵌套

取出所有key值

```
function test(obj){
	var arr=[];
    for(var item in obj){
        if(item.toString()==["object object"]){
        	arr.push(item);
            arr=arr.concat(test(item))
        }else{
            arr.push(item);
        }
    }
    return arr;
}

```

取出非object型的key值

```
function test(obj){
    var arr=[];
    for(var item in obj){
        arr=arr.concat(obj[item].toString()=="[object Object]"?test(obj[item]):item);
    }
    return arr;
}

```

# JS原生函数实现

## 实现apply、call、bind

1. apply

```
Function.prototype.myApply=function(context){
	if(typeof this!=="function"){
        throw new Error("erro");
	}
    context=context||window;
    context.fn=this;
    let result;
    if(arguments[1]){
        result=context.fn(...arguments[1]);
    }else{
        result=context.fn();
    }
    delete context.fn;
    return result;
}

```

2. call

```
Function.prototype.myCall=function(context){
    if(typeof this!=="function"){
        throw new Error("erro");
    }
    context=context||window;
    context.fn=this;
    let result;
    var args=[...arguments].slice(1);
    result=context.fn(...args);
    delete context.fn;
    return result;
}

```

3. bind
```
   Function.prototype.myBind=function(thisArgs){
    if(typeof this!=="function"){
        throw new Error("erro");
    }
    var fn=this;
    let args=[...arguments].slice(1);
    return function F(){
    	if(this instanceof F){
            return new fn(...args,...arguments);
    	}else{
            return fn.apply(thisArgs,args.concat(...arguments));
    	}        
    }
   }
```
4. new

```
funtion myNew(fun){
    return function(){
        let obj={
            __proto__:fun.prototype
        }
        fun.apply(obj,...arguments);
        return obj;
    }
}

```
## 实现promise
```
function Promise(executor) {
    let self = this;
    self.value  = undefined;
    self.reason = undefined;
    self.status = "pending";
    
    // 处理setTimeout使状态不能立即改变的情况
    self.onFullFilledCallbacks = [];
    self.onRejectedCallbacks = [];
    
    function resolve(value) {
        if (self.status == "pending") {
            self.value  = value;
            self.status = "resolved";
            
            self.onFullFilledCallbacks.forEach(onFulFilled => onFulFilled());
        }
    }
    
    function reject(reason) {
        if (self.status == "pending") {
            self.reason = reason;
            self.status = "rejected";
            
            self.onRejectedCallbacks.forEach(onRejected => onRejected());
        }
    }
    
    try {
        executor(resolve, reject);
    } catch (error) {
        reject(error);
    }
}

Promise.prototype.then = function(onFulfilled, onRejected) {
    let p2Resolve, p2Reject;
    let p2 = new promise((resolve, reject) => {
        p2Resolve = resolve;
        p2Reject  = reject;
    })
    if (this.status == "pending") {
        this.onFullFilledCallbacks.push(() => {
            onFulfilled(this.value);
            p2Resolve();
        });
        this.onRejectedCallbacks.push(() => {
            onRejected(this.reason);
            p2Reject();
        });
    } else if (this.status == "resolved") {
        onFulfilled(this.value);
        p2Resolve();
    } else if (this.status == "rejected") {
        onRejected(this.reason);
        p2Reject();
    }
    
    return p2;
}
```
实现promise.all()
```
function promiseAll(promises){
    return new Promise((resolve,reject)=>{
        if(!Array.isArray(promises)){
            return rejcet(new Error("arguments must be an array"))
        }
        let promiseCounter=0,
            promiseNum=promises.length,
            resolvedValues=new Array(promiseNum);
        for(let i=0;i<promiseNum;i++){
            (function(i){
                Promise.resolve(promises[i]).then((value)=>{
                    promiseCounter++;
                    resolvedValues[i]==value;
                    if(promiseCounter==promiseNum){
                        return resolve(resolvedValues);
                    }
                }).catch(error){
                    reject(error)
                }
            })(i)
        }
    })
}
```
## 功能性

### 节流和防抖

节流：在规定时间只触发一次------滚动条

```
function throttle(fn,delay){
    let pre=Date.now();
    return function(){
        let now=Date.now();
        let context=this;
        if(now-pre>delay){
            fn.apply(context,aguments);
            pre=Date.now();
        }
    }
}
```

防抖：在规定时间内未触发则执行，如果再次触发，则清除定时器后再重设

```
function debounce(fn,delay){
    let timer=null;
    return function(){
        let context=this;
        let arg=arguments;
        clearTimeout(timer);
        timer=setTimeout(function(){
            fn.apply(context,arg)
        },delay)
    }
}
```

### 深拷贝

```
function deepclone(origin){
    let result=Array.isArray(origin)?[]:{};
    for(var item in origin){
        if(origin.hasOwnProperty(item)){
            if(origin&&typeof origin[item]==="object"){
                result[item]=Array.isArray(origin[item])?[]:{};
                result[item]=deepclone(origin[item]);
            }else{
                result[item]=origin[item];
            }
        }
    }
    return result
}
```

### 函数柯里化

```
function curry(fn){
	var args=[...arguments].slice(1);
    var f=function(){
        return curry(fn,args.concat(arguments));
    }
    f.done=function(){
        return args.reduce((a,b)=>a+b);
    }
    return f;
}
```
