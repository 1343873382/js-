# Javascript高级程序设计

js没有块级作用域但是有函数作用域

函数内未声明会提高为全局变量

```
if(true){
        var color = "blue";
    }
        alert(color);  //"bule"
```

```
 function add(num1, num2) {
            num1 = 10;
             var num2 = 20;
            num3 = 40;
            var num4 = 50;
            sum = num1 + num2
            console.log(num1 + num2);

        }
        add() //30
        console.log(sum); //sum=30
        console.log(num3); //num3=40
        console.log(num2); // num2 is not defined
         console.log(num1); // num1 is not defined
        console.log(num4); // 由于在函数内var num4没有变为全局变量，多以num4 is not defined
```

### 使用自定义属性思想，解决参数不正确问题

假设用户点击第二个LI：开始执行第二个绑定的方法(方法执行：形成一个新的作用域，把之前存储的字符串变为代码执行console.log(i);但是此时i已经变为3了，所以无论点击哪一个都为3的原因就在于此，从函数原理核心来看，就说得通了)

<script></script>标签一般都放在body的下端，因为代码是从上至下执行的，要把上面的标签都显示完才行

也可以使用延迟脚本（从上至下）defer=‘defer’和异步脚本async=‘async’（不同异步脚本之间不知道谁先执行）

# typeof与instanceof区别,indexof("字符"，第几次)

typeof判断所有变量的类型，返回值有number，boolean，string，function，object，undefined。

typeof对于丰富的对象实例，只能返回"Object"字符串。

instanceof用来判断对象，代码形式为obj1 instanceof obj2（obj1是否是obj2的实例），obj2必须为对象，否则会报错！其返回值为布尔值。

instanceof可以对不同的对象实例进行判断，判断方法是根据对象的原型链依次向下查询，如果obj2的原型属性存在obj1的原型链上，（obj1 instanceof obj2）值为true。

每个函数函数都有自己的执行环境。每当执行流进入一个函数时，函数的环境就会被推入一个环境桟

在全局函数中，this等于window，当函数被作为某个对象的方法调用时，this等于那个对象

箭头函数不同，箭头函数的this总是指向函数定义生效时所在的对象

```
var name ='The Window';
var object ={
 name:"My Object"
 getNameFunc: function(){
 return function(){
  return this.name
 }
 }
}
alert(object.getNameFunc()())//"The window"
```

```
var name ="The Window";
var object={
 nem:'My Object',
 getName:function(){
 return this.name;
 }
}
object.getName();//"My Object"
```

var str="Hello world, welcome to the universe.";
var n=str.indexOf("e",5);

*n* 输出结果:

14

### 模仿块级作用域

利用js函数作用域，采用立即执行函数产生块级作用域，从而限制向全局作用域中添加过多的变量和函数，防止命名冲突

### 函数声明与函数表达式

解析器会率先读取函数声明，并在其执行任何代码前可用可访问；至于函数表达式，必须要等到解析器执行到它所在的代码行，才会真正被解释执行

```
alert(sum(10,10));
function sum(num1,num2){
return num1+num2
}
//执行成功，在执行那个函数前会先读取函数声明
```

```
alert(sum(10+10));
var sum=function(num1,num2){
return num1+num2;
}
//在执行到函数所在语句前，变量sum中不会保存有对函数的引用
```

navigator对象的属性通常用于检验显示网页浏览器的类型

### Dom树中的nodelist对象

nodelist分为静态和动态

动态NodeList tagname

### 关于变量提升

1. `var a = 2`提升的是`var a`;而不会提升赋值操作 `a=2`，赋值操作将原地保留;

2. ```
   console.log( a );
   var a = 2; //输出undefined
   复制代码
   ```

   因为

   ```
   var a; 
   console.log( a );
   a = 2;
   ```

### 优先级

> 后面的函数声明 > 函数声明 > 变量声明

```
foo(); // 1
var foo;
function foo() {
    console.log( 1 );
}
foo = function() { 
    console.log( 2 );
};
复制代码
```

会输出 1 而不是 2 ! 等同于

```
function foo() { 
    console.log( 1 );
}
foo(); // 1
foo = function() { 
    console.log( 2 );
};
```

# 

## 变量提升

最基础的概念大家都知道

```
console.log(a);``var` `a = 1;``// 输出 undefined
```

 

在代码中 **使用 var 来声明变量的时候，会提到当前作用域的顶端，而赋值操作在原处不变**

上面的两行代码相当于

```
var` `a;``console.log(a);``a = 1;
```

 var a声明向上提升，a=1赋值留在原处

### 函数提升			

js没有块级作用域但是有函数作用域

函数内未声明会提高为全局变量

```
if(true){
        var color = "blue";
    }
        alert(color);  //"bule"
```

```
 function add(num1, num2) {
            num1 = 10;
             var num2 = 20;
            num3 = 40;
            var num4 = 50;
            sum = num1 + num2
            console.log(num1 + num2);

        }
        add() //30
        console.log(sum); //sum=30
        console.log(num3); //num3=40
        console.log(num2); // num2 is not defined
         console.log(num1); // num1 is not defined
        console.log(num4); // 由于在函数内var num4没有变为全局变量，多以num4 is not defined
```

### 使用自定义属性思想，解决参数不正确问题

假设用户点击第二个LI：开始执行第二个绑定的方法(方法执行：形成一个新的作用域，把之前存储的字符串变为代码执行console.log(i);但是此时i已经变为3了，所以无论点击哪一个都为3的原因就在于此，从函数原理核心来看，就说得通了)

<script></script>标签一般都放在body的下端，因为代码是从上至下执行的，要把上面的标签都显示完才行

也可以使用延迟脚本（从上至下）defer=‘defer’和异步脚本async=‘async’（不同异步脚本之间不知道谁先执行）

# typeof与instanceof区别

typeof判断所有变量的类型，返回值有number，boolean，string，function，object，undefined。

typeof对于丰富的对象实例，只能返回"Object"字符串。

instanceof用来判断对象，代码形式为obj1 instanceof obj2（obj1是否是obj2的实例），obj2必须为对象，否则会报错！其返回值为布尔值。

instanceof可以对不同的对象实例进行判断，判断方法是根据对象的原型链依次向下查询，如果obj2的原型属性存在obj1的原型链上，（obj1 instanceof obj2）值为true。

## e.target和e.currenTarget

`e.target`指向用户点击的`li`，由于事件冒泡，`li`的点击事件冒泡到了`ul`上，通过给`ul`添加监听事件而达到了给每一个`li`添加监听事件的效果，而`e.currentTarget`指向的是给绑定事件监听的那个对象，即`ul`，从这里可以发现，`e.currentTarget===this`返回`true`，而`e.target===this`返回`false`。`e.currenttarget`和`e.target`是不相等的。

### 小程序中，

### 1.页面.js文件中存放事件回调函数时存放在data同层级下

### 2.组件.js文件中存放事件回调函数时必须放在 methods

# 视觉给的图可能由透明的边距，会导致一直填不满

.class1：hover .class2 要控制另一个元素，class1必须是class2的父元素才能控制class2的属性，如果是同级元素则无效。

## 正则表达式

```
 var really = /^[a-zA-Z]*$/;
        if (!really.test(value)) {
            alert('只能输入字母')
        }
```

如果没有以^开头和%结尾，例如/he/说明只要包含he的字符转就是符合标准的，但是如果有^$就必须完全相同

x|y 匹配x或y
{n} 精确匹配n次
{n,} 匹配n次以上
{n,m} 匹配n-m次
[xyz] 字符集(character set)，匹配这个集合中的任一一个字符(或元字符)
^xyz不匹配这个集合中的任何一个字符

\* 匹配前面元字符0次或多次，/ba*/将匹配b,ba,baa,baaa
\+ 匹配前面元字符1次或多次，/ba*/将匹配ba,baa,baaa
? 匹配前面元字符0次或1次，/ba*/将匹配b,ba

- \d ------> 数字
- \w ------> 匹配数字、字母、下划线
- \s ------> 匹配任意空白符

**常用反义词**

- D ------> 非数字
- \W ------> 匹配任意不是字母，数字，下划线，汉字的字符
- \S ------> 匹配任意不是空白符的字符

## 获取后端数据时的结构赋值

![](C:\Users\MECHREVO\Desktop\前端联系\学习文档\QQ图片20201130225820.png)

想要获取歌曲

```
 const { result: { songs } } = res;
```

```
const{result}=res；
const{songs}=result
```

### 对象里{age：age}可以简写为{age}

# 超级大坑：为什么无法获取浏览器input中的value呢？

```
<input type="submit" value="提交" />
<script>
    var subBtn = document.getElementById("myForm");
    var num1Val = document.getElementById("num1").value;
    var num2Val = document.getElementById("num2").value;
    subBtn.onsubmit = function () {
        alert(num1Val);
        return false;
    };

</script>
```

</form>

你页面加载好，num1Val的值就已经获得了，此时value值位空，你提交的时候肯定还是空。解决办法：
点击提交的时候在去获取

```
        var subBtn = document.getElementById("myForm");

        subBtn.onsubmit = function () {
                var num1Val = document.getElementById("num1").value;

                var num2Val = document.getElementById("num2").value;

            alert(num1Val);

            return false;}
```

## 字符串拼接

数字和字符串拼接成为字符串

```
xhr.open('get', 'http://musicapi.leanapp.cn/search?' + keywords);
```

### 模板字符串

```javascript
let x = 1;
let y = 2;
`${x} + ${y} = ${x + y}`
// "1 + 2 = 3"
```



