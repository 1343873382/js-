

# 一些js手写题

## es5实现类的继承

```
//寄生组合继承
function Parent(){
  this.name="jack"
  this.type="parent"
}
Parent.protoype.getName=function(){
 return this.name
}
function Child(){
 Parent.call(this)
 this.type="child"
}
Child.prototype=Object.create(Parent.prototype)
Child.prototype.consctructor=Child
const child1 = new Child();
const child2 = new Child();
console.log(child1); // Child { name: 'Cherry', play: [ 1, 2, 3 ], type: 'child' }
console.log(child1.getName()); // Cherry
child1.play.push(4);
console.log(child1.play, child2.play); // [ 1, 2, 3, 4 ] [ 1, 2, 3 ]

```

## 数组扁平化

```
//用递归处理
function flat(arr){
let arr1=[]
   for(let item of arr){
  item instanceof Array ?        arr1=arr1.concat(flat(item)):arr1.push(item)
   }
   return arr1
}
//数组的reduce方法
function flat(arr){
    return  arr.reduce((pre,cur)=>{
    return  pre.concat(cur instanceof Array?flat(cur):cur)
    },[])
}

```

## 数组去重

```
//建一个新数组
function quchong(arr){
let arr1=[]
    for(let i=0;i<arr.length;i++){
       if(arr1.indexOf(arr[i])===-1){
       arr1.push(arr[i])
       }
    }
    return arr1
}
//filter
function quchong(arr){
return arr.filter((item,index)=>{
  arr.indexof(item)===index
})
}
//Set
function quchong(arr){
 return [...new Set(arr)]
}
```

## 实现一个深拷贝

```
function deepCopy(obj){
 if(obj instanceof Object)
     for(let index in obj){
      if(typeof(obj[index])!=="object"){
      obj1[index]=obj[index]
      }
      else{
         deepCopy(obj[index])
      }
}
}
function deepCopy(obj){
   function isType(){
       let str=Object.prototype.toString.call(obj)
       return str.slice(8,-1)
   }
   let res
   if(isType(obj)==="Object"){
   res={}
   }
   if(isType(obj)==="Array"){
   res=[]
   }
   for(let item in obj){
    if(isType(obj[item])==="Object"||isType(obj[item])==="Array"){
  res[item]=deepCopy(res[item])
    }
    else{
    
      res[item]=obj[item]
    }
   }
   return res
}
```

## call

```
Function.prototype.maCall=function(context){
      context=context||window
      context.fn=this
      arg=[...arguments].slice(1)
      let res=context.fn(...arg)
      delete context.fn
      return res
}
```

## apply

```
Function.prototype.myApply=function(context){
      context=context||window
      context.fn=this
      let res
     if(arguments[1]){
        res=context.fn(argunments[1])
     }
      else{
        res=context.fn
}
      delete context.fn
      return res
}
```

## bind

```
Function.prototype.myBind=function(context){
   let that=this
   let arg=[...arguments].slice(1)
   return function(){
   return that.apply(contxet,arg.concat(...arguments))
   }
}
```



## 节流

```
function throttle(fn,delay){
   let timer
   return function(){
      let that=this
       let args=arguments
       if(timer){
       return
       }
       timer=setTimeout(function(){
         fn.apply(that,args)
         timer=null
       },delay)
   }
}
```

## 防抖

```
function debounce(fn,delay){
     let timer;
     return function(){
       let that=this
       let args=arguments
       if(timer){
           clearTimeout(timer)
       }
       timer=setTimeout(function(){
           fn.apply(this,args)
       },delay)
     }
}
```

## 函数柯里化

```
function curry(fn) {
  // 获取原函数的参数长度
  const argLen = fn.length;
  // 保存预置参数
  const presetArgs = [].slice.call(arguments, 1)
  // 返回一个新函数
  return function() {
    // 新函数调用时会继续传参
    const restArgs = [].slice.call(arguments)
    const allArgs = [...presetArgs, ...restArgs]
    if (allArgs.length >= argLen) {
      // 如果参数够了，就执行原函数
      return fn.apply(this, allArgs)
    } else {
      // 否则继续柯里化
      return curry.call(null, fn, ...allArgs)
    }
  }
}
```

## promise.all

```
function promiseAll(promises){
     return new Promise((resolve,reject)=>{
     let count=0
     let value=[]
           for(let i =0;i<promises.length;i++){
           promises[i].then(val=>{
            values[i]=val
            count++
              if(count===promises.length){
            resolve(values)}
           }).catch(err=>{
           reject(err)
           })
         
           
           }
     })
}
```

## jsonp

```
 function jsonp(url, data, callback) {
            var funcName = 'jsonp_' + Date.now() + Math.random().toString().substr(2, 5)
            //如果存在其他传入参数，需要进行拼接
            if (typeof data === 'object') {
                var tempArr = []
                for (var key in data) {
                    var value = data[key]
                    tempArr.push(key + '=' + value)
                }
                data = tempArr.join('&')
            }
            var script = document.createElement('script')
            script.src = url + '?' + data + '&callback= ' + funcName
            document.body.appendChild(script)
            window[funcName] = function(data) {
                callback(data)
                //清除全局函数和script标签
                delete window[funcName]
                document.body.removeChild(script)
            }
        }
        jsonp('http://127.0.0.1:3000/api', {}, function(res) {
                console.log(res)
        })

```

## instanceof

```
function instance(left,right){
 leftValue=left.__proto__
 rightProto=right.prototype										while(true){
  if(leftValue===null){
  return false
  if(leftValue===rightProto){
  return true
  }
  leftValue=leftValue.__proto__
  }
 }																					
}
```

## new

```
function _new(object,...args){
    let newobj=Object.create(object.prototype)
    let result=newobj.apply(object,args)
    return typeof(result)==="object"?result:newobj
}
```

## 懒加载

```
var imgs = document.querySelectorAll('img');

        //用来判断bound.top<=clientHeight的函数，返回一个bool值
        function isIn(el) {
            var bound = el.getBoundingClientRect();
            var clientHeight = window.innerHeight;
            return bound.top <= clientHeight;
        } 
        //检查图片是否在可视区内，如果不在，则加载
        function check() {
            Array.from(imgs).forEach(function(el){
                if(isIn(el)){
                    loadImg(el);
                }
            })
        }
        function loadImg(el) {
            if(!el.src){
                var source = el.dataset.src;
                el.src = source;
            }
        }
        window.onload = window.onscroll = function () { //onscroll()在滚动条滚动的时候触发
            check();
        }
```

## ajax

```
function ajax(url,method){
    let xhr=new XMLHttpRequest()
    xhr.open(method,url)
    xhr.onreadystatechange=()=>{
    if(xhr.readyState===4){
    if(xhr.status>=200&&xhr.status<300){
        let res=xhr.responseText
        console.log(res)
    }
    }
    }
    xhr.send()
    
}
```

