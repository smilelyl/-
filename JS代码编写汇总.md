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
```
```