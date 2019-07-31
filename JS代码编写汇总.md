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
