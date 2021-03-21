# Javascript高级程序设计系统学习

## HTMl中的Javascript

###  script 标签的位置

把所有 JavaScript
文件都放在 <head> 里，也就意味着必须把所有 JavaScript 代码都下载、解析和解释完成后，才能开始渲
染页面（页面在浏览器解析到 <body> 的起始标签时开始渲染）。对于需要很多 JavaScript 的页面，这会
导致页面渲染的明显延迟，在此期间浏览器窗口完全空白。为解决这个问题，现代 Web 应用程序通常
将所有 JavaScript 引用放在 <body> 元素中的页面内容后面

### script标签的属性

可以使用 defer 属性把脚本推迟到文档渲染完毕后再执行。推迟的脚本原则上按照它们被列出的次序执行。
 可以使用 async 属性表示脚本不需要等待其他脚本，同时也不阻塞文档渲染，即异步加载。异步脚本不能保证按照它们在页面中出现的次序执行。

### 为什么要使用外部标签

 可维护性

 缓存。浏览器会根据特定的设置缓存所有外部链接的 JavaScript 文件，这意味着如果两个页面都用到同一个文件，则该文件只需下载一次。这最终意味着页面加载更快。

### noscript 标签

在下列两种情况下的任意一种，浏览器将显示包含在 <noscript> 中的内容：
 浏览器不支持脚本；
 浏览器对脚本的支持被关闭。

## 变量和数据类型

### 变量声明

#### var

##### 函数作用域

使用 var在一个函数内部定义一个变量，就意味着该变量将在函数退出时被销毁

```
function test() {
var message = "hi"; // 局部变量
}
test();
console.log(message); // 出错！
```

过，在函数内定义变量时省略 var 操作符，可以创建一个全局变量去掉之前的 var 操作符之后， message 就变成了全局变量。只要调用一次函数 test() ，就会定义这个变量，并且可以在函数外部访问到

```
function test() {
message = "hi"; // 全局变量
}
test();
console.log(message); // "hi"
```

虽然可以通过省略 var 操作符定义全局变量，但不推荐这么做。在局部作用域中定义的全局变量很难维护

##### 变量提升

使用var声明的变量会自动提升到函数作用域顶部

```
function foo() {
console.log(age);
var age = 26;
}
foo(); // undefined
```

之所以不会报错，是因为 ECMAScript 运行时把它看成等价于如下代码

```
function foo() {
var age;
console.log(age);
age = 26;
}
foo(); // undefined
```

不是函数内也同理

```
 console.log(a);//undefind
        var a = 6;
```

此外，反复多次使用 var 声明同一个变量也没有问题

#### let

##### 块级作用域

let 声明的范围是块作用域，而 var 声明的范围是函数作用域

```
if (true) {
var name = 'Matt';
console.log(name); // Matt
}
console.log(name); // Matt

if (true) {
let age = 26;
console.log(age); // 26
}
console.log(age); // ReferenceError: age 没有定义
```

 age 变量之所以不能在 if 块外部被引用，是因为它的作用域仅限于该块内部

##### 冗余声明

```
var name;
var name;
let age;
let age; // SyntaxError；标识符 age 已经声明过了
```

```
var name;
var name;
let age;
let age; // SyntaxError；标识符 age 已经声明过了
```

##### 暂时性死区

```
// name 会被提升
console.log(name); // undefined
var name = 'Matt';
// age 不会被提升
console.log(age); // ReferenceError：age 没有定义
let age = 26;
```

##### 不会成为window属性

```
var name = 'Matt';
console.log(window.name); // 'Matt'
let age = 26;
console.log(window.age); // undefined
```

不过， let 声明仍然是在全局作用域中发生的，相应变量会在页面的生命周期内存续

必须确保页面不会重复声明同一个变量

#### for循坏中的var与let

在 let 出现之前， for 循环定义的迭代变量会渗透到循环体外部：

```
for (var i = 0; i < 5; ++i) {
// 循环逻辑
}
console.log(i); // 5
```

```
for (let i = 0; i < 5; ++i) {
// 循环逻辑
}
console.log(i); // ReferenceError: i 没有定义
```

```
for (var i = 0; i < 5; ++i) {
setTimeout(() => console.log(i), 0)
}
// 你可能以为会输出 0、1、2、3、4
// 实际上会输出 5、5、5、5、5
```

之所以会这样，是因为在退出循环时，迭代变量保存的是导致循环退出的值：5。在之后执行超时逻辑时，所有的 i 都是同一个变量，因而输出的都是同一个最终值。
而在使用 let 声明迭代变量时，JavaScript 引擎在后台会为每个迭代循环声明一个新的迭代变量。
每个 setTimeout 引用的都是不同的变量实例，所以 console.log 输出的是我们期望的值，也就是循环执行过程中每个迭代变量的值

```
for (let i = 0; i < 5; ++i) {
setTimeout(() => console.log(i), 0)
}
// 会输出 0、1、2、3、4
```

#### const

const 的行为与 let 基本相同，拥有块级作用域，唯一一个重要的区别是用它声明变量时必须同时初始化变量，且尝试修改 const 声明的变量会导致运行时错误。

##### 不允许变量重复赋值，对象除外，也不允许重复声明

```
const age = 26;
age = 36; // TypeError: 给常量赋值
// const 也不允许重复声明
const name = 'Matt';
const name = 'Nicholas'; // SyntaxError
// const 声明的作用域也是块
const name = 'Matt';
if (true) {
const name = 'Nicholas';
}
console.log(name); // Matt
```

const 声明的限制只适用于它指向的变量的引用。换句话说，如果 const 变量引用的是一个对象，那么修改这个对象内部的属性并不违反 const 的限制。

```
const person = {};
person.name = 'Matt'; // ok
```

### 数据类型

以下为简单数据类型

#### Undefined

#### Null

```
console.log(null == undefined); // true
```

#### Boolean

 if 等流控制语句会自动执行其他类型值到布尔值的转换

例如

```
let message = "Hello world!";
if (message) {
console.log("Value is true");
}
```

#### Number

 Number 类型使用 IEEE 754格式表示整数和浮点值（在某些语言中也叫双精度值）

因为存储浮点值使用的内存空间是存储整数值的两倍，所以 ECMAScript 总是想方设法把值转换为整数。在小数点后面没有数字的情况下，数值就会变成整数。

##### 精度问题

浮点值的精确度最高可达 17 位小数，但在算术计算中远不如整数精确。例如，0.1 加 0.2 得到的不是 0.3，而是 0.300 000 000 000 000 04。由于这种微小的舍入错误，导致很难测试特定的浮点值。比如下面的例子：

```
if (a + b == 0.3) { // 别这么干！
console.log("You got 0.3.");
}
```

##### 精度问题探讨

不是所有小数相加都会丧失精度，虽然他们可能确实不是精确的小数，例如

```
var a = 0.1;
        var b = 0.4;
        console.log(a + b);//0.5
```

也就是说如果这个精度丢失在js数值表示范围之内就会体现，但是在范围之外就不会体现，0.1+0.4严格来讲也有精度丢失，但它是属于在js数值范围之外的丢失，所以看不出来

##### 精度丢失解决办法

1.Number.EPSILON，而这个值正等于2^-52。这个值非常非常小，在底层计算机已经帮我们运算好，并且无限接近0，但不等于0,。这个时候我们只要判断(0.1+0.2)-0.3小于Number.EPSILON，在这个误差的范围内就可以判定0.1+0.2===0.3为true。

2.把计算数字 提升 10 的N次方 倍 再 除以 10的N次方。一般都用 1000 就行了

```
(0.1*1000+0.2*1000)/1000==0.3
//true
```

#### String

##### 字符串的特点

ECMAScript 中的字符串是不可变的（immutable），意思是一旦创建，它们的值就不能变了。要修改某个变量中的字符串值，必须先销毁原始的字符串，然后将包含新值的另一个字符串保存到该变量，如下所示：
let lang = "Java";
lang = lang + "Script";
这里，变量 lang 一开始包含字符串 "Java" 。紧接着， lang 被重新定义为包含 "Java" 和 "Script"的组合，也就是 "JavaScript" 。整个过程首先会分配一个足够容纳 10 个字符的空间，然后填充上"Java" 和 "Script" 。最后销毁原始的字符串 "Java" 和字符串 "Script" ，因为这两个字符串都没有用了。所有处理都是在后台发生的，而这也是一些早期的浏览器（如 Firefox 1.0 之前的版本和 IE6.0）在拼接字符串时非常慢的原因。

##### 模板字符串与字符串拼接

```
let value = 5;
let exponent = 'second';
// 以前，字符串插值是这样实现的：
let interpolatedString =
value + ' to the ' + exponent + ' power is ' + (value * value);
// 现在，可以用模板字面量这样实现：
let interpolatedTemplateLiteral =
`${ value } to the ${ exponent } power is ${ value * value }`;
console.log(interpolatedString); // 5 to the second power is 25
console.log(interpolatedTemplateLiteral); // 5 to the second power is 25
```



#### Symbol

Symbol （符号）是 ECMAScript 6 新增的数据类型。符号是原始值，且符号实例是唯一、不可变的。
符号的用途是确保对象属性使用唯一标识符，不会发生属性冲突的危险。

##### 基本用法

```
let genericSymbol = Symbol();
let otherGenericSymbol = Symbol();
let fooSymbol = Symbol('foo');
let otherFooSymbol = Symbol('foo');
console.log(genericSymbol == otherGenericSymbol); // false
console.log(fooSymbol == otherFooSymbol); // false
```

##### 使用Symbol来作为对象属性名(key)防止被访问

注意symbol放对象里时要加中括号

```js
var b = Symbol()
let obj = {
   [b]: '一斤代码',
   age: 18,
   title: 'Engineer'
}

Object.keys(obj)   // ['age', 'title']

for (let p in obj) {
   console.log(p)   // 分别会输出：'age' 和 'title'
}
Object.getOwnPropertyNames(obj)   // ['age', 'title']
// 使用Object的API
Object.getOwnPropertySymbols(obj) // [Symbol(name)]

// 使用新增的反射API
Reflect.ownKeys(obj) // [Symbol(name), 'age', 'title']
```

##### 使用Symbol来替代常量

们经常定义一组常量来代表一种业务逻辑下的几个不同类型，我们通常希望这几个常量之间是唯一的关系，为了保证这一点，我们需要为常量赋一个唯一的值

##### 使用Symbol定义类的私有属性/方法

在a.js文件

```js
const PASSWORD = Symbol()

class Login {
  constructor(username, password) {
    this.username = username
    this[PASSWORD] = password
  }

  checkPassword(pwd) {
      return this[PASSWORD] === pwd
  }
}

export default Login
```

在文件 b.js 中

```js
import Login from './a'

const login = new Login('admin', '123456')

login.checkPassword('123456')  // true

login.PASSWORD  // oh!no!
login[PASSWORD] // oh!no!
login["PASSWORD"] // oh!no!
```

由于Symbol常量PASSWORD被定义在a.js所在的模块中，外面的模块获取不到这个Symbol，也不可能再创建一个一模一样的Symbol出来（因为Symbol是唯一的），因此这个PASSWORD的Symbol只能被限制在a.js内部使用，所以使用它来定义的类属性是没有办法被模块外访问到的，达到了一个私有化的效果。

#### Object复杂数据类型

## 变量的作用域与内存

### 原始值和引用值

#### 引用类型

这里， num1 包含数值 5。当把 num2 初始化为 num1 时， num2 也会得到数值 5。这个值跟存储在num1 中的 5 是完全独立的，因为它是那个值的副本

```
let num1 = 5;
let num2 = num1;

```

在把引用值从一个变量赋给另一个变量时，存储在变量中的值也会被复制到新变量所在的位置。区别在于，这里复制的值实际上是一个指针，它指向存储在堆内存中的对象。

```
let obj1 = new Object();
let obj2 = obj1;
obj1.name = "Nicholas";
console.log(obj2.name); // "Nicholas"
```

#### 传递参数

##### 传递普通值

这里，函数 addTen() 有一个参数 num ，它其实是一个局部变量。在调用时，变量 count 作为参数传入。 count 的值是 20，这个值被复制到参数 num 以便在 addTen() 内部使用。在函数内部，参数 num的值被加上了 10，但这不会影响函数外部的原始变量 count 

```
function addTen(num) {
num += 10;
return num;
}
let count = 20;
let result = addTen(count);
console.log(count); // 20，没有变化
console.log(result); // 30
```

##### 变量传递的是对象

```
function setName(obj) {
obj.name = "Nicholas";
obj = new Object();
obj.name = "Greg";
}
let person = new Object();
setName(person);
console.log(person.name); // "Nicholas"
```

#### instanceof与typeof

typeof 虽然对原始值很有用，但它对引用值的用处不大。我们通常不关心一个值是不是对象，而是想知道它是什么类型的对象

```
console.log(person instanceof Object); // 变量 person 是 Object 吗？
console.log(colors instanceof Array); // 变量 colors 是 Array 吗？
console.log(pattern instanceof RegExp); // 变量 pattern 是 RegExp 吗？
```



### 执行上下文与作用域

#### 作用域链

```
var color = "blue";
function changeColor() {
let anotherColor = "red";
function swapColors() {
let tempColor = anotherColor;
anotherColor = color;
color = tempColor;
// 这里可以访问 color、anotherColor 和 tempColor
}
// 这里可以访问 color 和 anotherColor，但访问不到 tempColor
swapColors();
}
// 这里只能访问 color
changeColor();
```



### 垃圾回收

离开作用域的值会被自动标记为可回收，然后在垃圾回收期间被删除

9090909000000000000990000000990记清理，即先给当前不使用的值加上标记，再回来回收它们的内存

引用计数是另一种垃圾回收策略，需要记录值被引用了多少次。JavaScript 引擎不再使用这种算法，但某些旧版本的 IE 仍然会受这种算法的影响，原因是 JavaScript 会访问非原生 JavaScript 对象（如 DOM 元素）

解除变量的引用不仅可以消除循环引用，而且对垃圾回收也有帮助。为促进内存回收，全局对象、全局对象的属性和循环引用都应该在不需要时解除引用

```
function createPerson(name){
let localPerson = new Object();
localPerson.name = name;
return localPerson;
}
let globalPerson = createPerson("Nicholas");
// 解除 globalPerson 对值的引用
globalPerson = null;
将内存占用量保持在一个较小的值可以让页面性能更好。优化内存占用的最佳手段就是保证在执行代码时只保存必要的数据。如果数据不再必要，那么把它设置为 null ，从而释放其引用。这也可以叫作解除引用。这个建议最适合全局变量和全局对象的属性。局部变量在超出作用域后会被自动解除引用
```

#### 垃圾回收之性能提升

##### 1. 通过 const 和 let 声明提升性能

使用这两个新关键字可能会更早地让垃圾回收程序介入，尽早回收应该回收的内存。在块作用域比函数作用域更早终止的情况下，这就有可能发生

## 引用类型与方法

一些date（），Array，字符串的一些方法就不在此举例，看掘金收藏夹的思维导图很详细。

### 原始包装值类型

思考字符串本不是对象，按理他应该没有方法，同理Number和Boolean

```
let s1 = "some text";
let s2 = s1.substring(2);
let s1 = new String("some text");
let s2 = s1.substring(2);
s1 = null;
在以读模式访问字符串
值的任何时候，后台都会执行以下 3 步：
(1) 创建一个 String 类型的实例；
(2) 调用实例上的特定方法；
(3) 销毁实例。
```

判断类型时

```
let numberObject = new Number(10);
let numberValue = 10;
console.log(typeof numberObject); // "object"
console.log(typeof numberValue); // "number"
console.log(numberObject instanceof Number); // true
console.log(numberValue instanceof Number); // false
```

由于原始值包装类型的存在，JavaScript 中的原始值可以被当成对象来使用。有 3 种原始值包装类
型： Boolean 、 Number 和 String 。它们都具备如下特点。
 每种包装类型都映射到同名的原始类型。
 以读模式访问原始值时，后台会实例化一个原始值包装类型的对象，借助这个对象可以操作相
应的数据。
 涉及原始值的语句执行完毕后，包装对象就会被销毁。

### Array里的map与foreach

map返回一个数组，但foreach不返回数组，而是对数组内每个值进行调用

1：Object对象有原型， 也就是说他有默认的key值在对象上面， 除非我们使用Object.create(null)创建一个没有原型的对象；
　2：在Object对象中， 只能把String和Symbol作为key值， 但是在Map中，key值可以是任何基本类型(String, Number, Boolean, undefined, NaN….)，或者对象(Map, Set, Object, Function , Symbol , null….);
　3：通过Map中的size属性， 可以很方便地获取到Map长度， 要获取Object的长度， 你只能用别的方法了；
　　Map实例对象的key值可以为一个数组或者一个对象，或者一个函数，比较随意 ，而且Map对象实例中数据的排序是根据用户push的顺序进行排序的， 而Object实例中key,value的顺序就是有些规律了， (他们会先排数字开头的key值，然后才是字符串开头的key值)；

### js里的Map、Set、对象的区别

ES6中Map相对于Object对象有几个区别：

　1：Object对象有原型， 也就是说他有默认的key值在对象上面， 除非我们使用Object.create(null)创建一个没有原型的对象；
　2：在Object对象中， 只能把String和Symbol作为key值， 但是在Map中，key值可以是任何基本类型(String, Number, Boolean, undefined, NaN….)，或者对象(Map, Set, Object, Function , Symbol , null….);
　3：通过Map中的size属性， 可以很方便地获取到Map长度， 要获取Object的长度， 你只能用别的方法了；
　　Map实例对象的key值可以为一个数组或者一个对象，或者一个函数，比较随意 ，而且Map对象实例中数据的排序是根据用户push的顺序进行排序的， 而Object实例中key,value的顺序就是有些规律了， (他们会先排数字开头的key值，然后才是字符串开头的key值)

## 理解对象、原型、面向对象编程

### 原型与原型链

![](C:\Users\MECHREVO\Desktop\前端联系\学习文档\原型链.png)

- 所有的对象都有"__proto__"属性，该属性对应该对象的原型
- 所有的函数对象都有"prototype"属性，该属性的值会被赋值给该函数创建的对象的"__proto__"属性
- 所有的原型对象都有"constructor"属性，该属性对应创建所有指向该原型的实例的构造函数
- 函数对象和原型对象通过"prototype"和"constructor"属性进行相互关联

```
function Person(name, age, job){
this.name = name;
this.age = age;
this.job = job;
this.sayName = function() {
console.log(this.name);
};
}
let person1 = new Person("Nicholas", 29, "Software Engineer");
let person2 = new Person("Greg", 27, "Doctor");
person1.sayName(); // Nicholas
person2.sayName(); // Greg

console.log(person1.constructor == Person); // true
console.log(person2.constructor == Person); // true

```

### 原型链面试题

```javascript
function Fn() {
    this.x = 100;
    this.y = 200;
    this.getX = function () {
        console.log(this.x);
    }
}
Fn.prototype.getX = function () {
    console.log(this.x);
};
Fn.prototype.getY = function () {
    console.log(this.y);
};
let f1 = new Fn;
let f2 = new Fn;
console.log(f1.getX === f2.getX);//=>false
console.log(f1.getY === f2.getY);//=>true
console.log(f1.__proto__.getY === Fn.prototype.getY);//=>true
console.log(f1.__proto__.getX === f2.getX);//=>flase
console.log(f1.getX === Fn.prototype.getX);//=>flase
console.log(f1.constructor);//=>Fn
console.log(Fn.prototype.__proto__.constructor);//=Object
f1.getX();//=>100
f1.__proto__.getX();//=>undefined
f2.getY();//=>200
Fn.prototype.getY();//=>undefinedget Y中的this指的是Fn.prototype，Fn.prototype中没有属性Y，所以打印出undefined
==========================================================
  构造函数函数中this都属性和方法是new出来的实例的私有的属性和方法，所以f1和f2都有自己的私有属性【x、y】和私有方法【getX】
  console.log(f1.getX === f2.getX);//=>false
getY这个函数是定义在Fn.prototype上，f1和f2私有中都没有此方法，沿着原型链查找到Fn.prototype上。

```



### 深拷贝与浅拷贝（还有赋值的区别）

在我们进行赋值操作的时候，基本数据类型的赋值（=）是在内存中新开辟一段栈内存，然后再把再将值赋值到新的栈中

所以说，基本类型的赋值的两个变量是两个独立相互不影响的变量。

但是引用类型的赋值是传址。只是改变指针的指向，例如，也就是说引用类型的赋值是对象保存在栈中的地址的赋值，这样的话两个变量就指向同一个对象，因此两者之间操作互相有影响

那么赋值和浅拷贝有什么区别呢，我们看下面这个例子：

```
var obj1 = {
        'name' : 'zhangsan',
        'age' :  '18',
        'language' : [1,[2,3],[4,5]],
    };

    var obj2 = obj1;


    var obj3 = shallowCopy(obj1);
    function shallowCopy(src) {
        var dst = {};
        for (var prop in src) {
            if (src.hasOwnProperty(prop)) {
                dst[prop] = src[prop];
            }
        }
        return dst;
    }

    obj2.name = "lisi";
    obj3.age = "20";

    obj2.language[1] = ["二","三"];
    obj3.language[2] = ["四","五"];

    console.log(obj1);  
    //obj1 = {
    //    'name' : 'lisi',
    //    'age' :  '18',
    //    'language' : [1,["二","三"],["四","五"]],
    //};

    console.log(obj2);
    //obj2 = {
    //    'name' : 'lisi',
    //    'age' :  '18',
    //    'language' : [1,["二","三"],["四","五"]],
    //};

    console.log(obj3);
    //obj3 = {
    //    'name' : 'zhangsan',
    //    'age' :  '20',
    //    'language' : [1,["二","三"],["四","五"]],
    //};

也就是说改变赋值对象里的属性值也会改变原来对象的属性值
而浅拷贝就不会这样，但是浅拷贝时改变对象里的对象（数组也算对象），也会改变原对象里的对象。
而深拷贝就是改变什么都不会改变原对象里的值
```

##### 实现深拷贝

其实深拷贝可以拆分成 2 步，浅拷贝 + 递归，浅拷贝时判断属性值是否是对象，如果是对象就进行递归操作，两个一结合就实现了深拷贝。

```
function cloneDeep1(source) {
    var target = {};
    for(var key in source) {
        if (Object.prototype.hasOwnProperty.call(source, key)) {
            if (typeof source[key] === 'object') {
                target[key] = cloneDeep1(source[key]); // 注意这里
            } else {
                target[key] = source[key];
            }
        }
    }
    return target;
}

```

## call()、apply()、bind() 的用法

```
obj.myFun.call(db,'成都','上海')；　　　　 // 德玛 年龄 99  来自 成都去往上海
obj.myFun.apply(db,['成都','上海']);      // 德玛 年龄 99  来自 成都去往上海  
obj.myFun.bind(db,'成都','上海')();       // 德玛 年龄 99  来自 成都去往上海
obj.myFun.bind(db,['成都','上海'])();　　 // 德玛 年龄 99  来自 成都, 上海去往 undefined　　
```

从上面四个结果不难看出:

第一个参数为null或undefined时指向window或global

call 、bind 、 apply 这三个函数的第一个参数都是 this 的指向对象，第二个参数差别就来了：

call 的参数是直接放进去的，第二第三第 n 个参数全都用逗号分隔，直接放到后面 **obj.myFun.call(db,'成都', ... ,'string' )**。

apply 的所有参数都必须放在一个数组里面传进去 **obj.myFun.apply(db,['成都', ..., 'string' ])**。

bind 除了返回是函数以外，它 的参数和 call 一样

### super（）

1.es6里面向对象super（）是用来调用父类中的构造函数，并且super要放在this.x等等属性之前，因为它是来初始化this的，好让子类引用父类函数的时候可以使用子类的属性。

2.recat里super(props)的目的：在constructor中可以使用this.props

## 函数

### 暂时性死区

函数可设置默认参数

```
function makeKing(name = 'Henry') {
return `King ${name} VIII`;
}
```

### 前面定义的参数不能引用后面定义的

```
// 调用时不传第一个参数会报错
function makeKing(name = numerals, numerals = 'VIII') {
return `King ${name} ${numerals}`;
}
```

### 函数递归中的arguments.callee

```
function factorial(num) {
if (num <= 1) {
return 1;
} else {
return num * arguments.callee(num - 1);
}
}
let trueFactorial = factorial;
factorial = function() {
return 0;
};
console.log(trueFactorial(5)); // 120
console.log(factorial(5)); // 0
这里， trueFactorial 变量被赋值为 factorial ，实际上把同一个函数的指针又保存到了另一个
位置。然后， factorial 函数又被重写为一个返回 0 的函数。如果像 factorial() 最初的版本那样不
使用 arguments.callee ，那么像上面这样调用 trueFactorial() 就会返回 0 。不过，通过将函数与
名称解耦， trueFactorial() 就可以正确计算阶乘，而 factorial() 则只能返回 0。
```

### 闭包

概念：那些引用了其他函数作用域中变量的函数

```
　function f1(){

　　　　var n=999;

　　　　function f2(){
　　　　　　alert(n);
　　　　}

　　　　return f2;

　　}

　　var result=f1();

　　result(); // 999
　　既然f2可以读取f1中的局部变量，那么只要把f2作为返回值，我们不就可以在f1外部读取它的内部变量了吗！
```

#### 闭包的作用

一个是前面提到的可以读取函数内部的变量，另一个就是让这些变量的值始终保持在内存中

```
function f1(){

　　　　var n=999;

　　　　nAdd=function(){n+=1}

　　　　function f2(){
　　　　　　alert(n);
　　　　}

　　　　return f2;

　　}

　　var result=f1();

　　result(); // 999

　　nAdd();

　　result(); // 1000
　　在这段代码中，result实际上就是闭包f2函数。它一共运行了两次，第一次的值是999，第二次的值是1000。这证明了，函数f1中的局部变量n一直保存在内存中，并没有在f1调用后被自动清除。

为什么会这样呢？原因就在于f1是f2的父函数，而f2被赋给了一个全局变量，这导致f2始终在内存中，而f2的存在依赖于f1，因此f1也始终在内存中，不会在调用结束后，被垃圾回收机制（garbage collection）回收。

这段代码中另一个值得注意的地方，就是"nAdd=function(){n+=1}"这一行，首先在nAdd前面没有使用var关键字，因此nAdd是一个全局变量，而不是局部变量。其次，nAdd的值是一个匿名函数（anonymous function），而这个匿名函数本身也是一个闭包，所以nAdd相当于是一个setter，可以在函数外部对函数内部的局部变量进行操作。
```

#### 闭包中的this指向性问题

```
　var name = "The Window";

　　var object = {
　　　　name : "My Object",

　　　　getNameFunc : function(){
　　　　　　return function(){
　　　　　　　　return this.name;
　　　　　　};

　　　　}

　　};

　　alert(object.getNameFunc()());//The Window
　　每个函数在被调用时都会自动创建两个特殊变量： this 和 arguments 。内部函数永
远不可能直接访问外部函数的这两个变量
```

果把 this 保存到闭包可以访问的另一个变量中，
则是行得通的

```
window.identity = 'The Window';
let object = {
identity: 'My Object',
getIdentityFunc() {
let that = this;
return function() {
return that.identity;
};
}
};
console.log(object.getIdentityFunc()()); // 'My Object'
```

### 函数赋值中的一些坑

```
　function f1(){

　　　　var n=999;

　　　　nAdd=function(){n+=1}

　　　　function f2(){
　　　　　　alert(n);
　　　　}

　　　　return f2;

　　}

　　var result=f1();

　　result(); // 999
　
```

当如上return的是一个函数时引用时就要加（）

```
var n = 1;


function sj() {
    n = n + 1;
    return n
}
var dj = sj()
dj//2
```

但如果return的是一个值那就不用（）

### 立即执行函数

它类似于函数声明，但由于被包含在括号中，所以会被解释为函数表达式。紧跟在第一组括号后面的第二组括号会立即调用前面的函数表达式

```
(function() {
// 块级作用域
})();
```

```
var lis = document.querySelectorAll('li');
        for (var i = 0; i < lis.length; i++) {

            (function(i) {
                setTimeout(
                    function() {
                        console.log(i);
                    }, 0
                )
            })(i)
        }
```

### 异步函数

async/await 中真正起作用的是 await 。 async 关键字，无论从哪方面来看，都不过是一个标识符。
毕竟，异步函数如果不包含 await 关键字，其执行基本上跟普通函数没有什么区别

#### async和await

##### async

使输出的是一个Promise对象

```
async function testAsync(){
return "hello async"
}
const result=testAsync();
console.log(result);//Promise{'hello async'}
```

##### await

await后面是可以是直接函数和一个值的，它内部所有的阻塞都会封装在一个Promise对象中异步执行

##### 执行规律

要完全理解 await 关键字，必须知道它并非只是等待一个值可用那么简单。JavaScript运行时在碰到 await 关键字时，会记录在哪里暂停执行。等到 await 右边的值可用了，JavaScript 运行时会向消息队列中推送一个任务，这个任务会恢复异步函数的执行。
因此，即使 await 后面跟着一个立即可用的值，函数的其余部分也会被异步求值。

```
async function foo() {
console.log(2);
console.log(await Promise.resolve(8));
console.log(9);
}
async function bar() {
console.log(4);
console.log(await 6);
console.log(7);
}
console.log(1);
foo();
console.log(3);
bar();
console.log(5);
// 1
// 2
// 3
// 4
// 5
// 6
// 7
// 8
// 9
运行时会像这样执行上面的例子：
(1) 打印 1；
(2) 调用异步函数 foo() ；
(3)（在 foo() 中）打印 2；
(4)（在 foo() 中） await 关键字暂停执行，向消息队列中添加一个期约在落定之后执行的任务；
(5) 期约立即落定，把给 await 提供值的任务添加到消息队列；
(6) foo() 退出；
(7) 打印 3；
(8) 调用异步函数 bar() ；
(9)（在 bar() 中）打印 4；
(10)（在 bar() 中） await 关键字暂停执行，为立即可用的值 6 向消息队列中添加一个任务；
(11) bar() 退出；
(12) 打印 5；
(13) 顶级线程执行完毕；
(14) JavaScript 运行时从消息队列中取出解决 await 期约的处理程序，并将解决的值 8 提供给它；
(15) JavaScript 运行时向消息队列中添加一个恢复执行 foo() 函数的任务；
(16) JavaScript 运行时从消息队列中取出恢复执行 bar() 的任务及值 6；
(17)（在 bar() 中）恢复执行， await 取得值 6；
(18)（在 bar() 中）打印 6；
(19)（在 bar() 中）打印 7；
(20) bar() 返回；
(21) 异步任务完成，JavaScript 从消息队列中取出恢复执行 foo() 的任务及值 8；
(22)（在 foo() 中）打印 8；
(23)（在 foo() 中）打印 9；
(24) foo() 返回。
```

## BOM

### window对象

### location对象

### navigator对象

navigator 对象的属性通常用于确定浏览器的类型

### screen对象

客户端显示器的信息

### history对象

## JSON

let jsonText = JSON.stringify(book)//将javascript转化为JSON字符串

let bookCopy = JSON.parse(jsonText)//将JSON对象转为JavaScript值

## 网络请求



## WebSocket

WebSocket 是 HTML5 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。

因为 HTTP 协议有一个缺陷：通信只能由客户端发起。

很多网站为了实现推送技术，所用的技术都是 Ajax 轮询。轮询是在特定的的时间间隔（如每1秒），由浏览器对服务器发出HTTP请求，然后由服务器返回最新的数据给客户端的浏览器。这种传统的模式带来很明显的缺点，即浏览器需要不断的向服务器发出请求，然而HTTP请求可能包含较长的头部，其中真正有效的数据可能只是很小的一部分，显然这样会浪费很多的带宽等资源

```
  function WebSocketTest()
         {
            if ("WebSocket" in window)
            {
               alert("您的浏览器支持 WebSocket!");
               
               // 打开一个 web socket
               var ws = new WebSocket("ws://localhost:9998/echo");
                
               ws.onopen = function()
               {
                  // Web Socket 已连接上，使用 send() 方法发送数据
                  ws.send("发送数据");
                  alert("数据发送中...");
               };
                
               ws.onmessage = function (evt) 
               { 
                  var received_msg = evt.data;
                  alert("数据已接收...");
               };
                
               ws.onclose = function()
               { 
                  // 关闭 websocket
                  alert("连接已关闭..."); 
               };
            }
            
            else
            {
               // 浏览器不支持 WebSocket
               alert("您的浏览器不支持 WebSocket!");
            }
         }
```

## 客户端存储

### cookie

一般由服务器生成，可设置失效时间如果在浏览器生成cookie，默认是关闭浏览器后失效

存放大小4k左右

每次都会携带在http请求头中，如果使用cookie保存过多数据会带来性能问题

需要程序员自己封装，源生的Cookie接口不友好

```
设置 cookie 值的函数
function setCookie(cname,cvalue,exdays)
{
  var d = new Date();
  d.setTime(d.getTime()+(exdays*24*60*60*1000));
  var expires = "expires="+d.toGMTString();
  document.cookie = cname + "=" + cvalue + "; " + expires;
}
```

```
获取 cookie 值的函数
function getCookie(cname)
{
  var name = cname + "=";
  var ca = document.cookie.split(';');
  for(var i=0; i<ca.length; i++) 
  {
    var c = ca[i].trim();
    if (c.indexOf(name)==0) return c.substring(name.length,c.length);
  }
  return "";
}
```



### localStorage

除非被清除，否则永久存在

一般为5mb

仅在客户端即浏览器中保存，不参与和服务器的通信

源生接口可以接受，亦可再次封装来对Object和Array有更好的支持

#### 清空localStorage



```javascript
localStorage.clear()    // undefined    
localStorage            // Storage {length: 0}
```

#### 存储数据



```javascript
localStorage.setItem("name","caibin") //存储名字为name值为caibin的变量
localStorage.name = "caibin"; // 等价于上面的命令
localStorage // Storage {name: "caibin", length: 1}
```

#### 读取数据



```javascript
localStorage.getItem("name") //caibin,读取保存在localStorage对象里名为name的变量的值
localStorage.name // "caibin"
localStorage.valueOf() //读取存储在localStorage上的所有数据
localStorage.key(0) // 读取第一条数据的变量名(键值)
//遍历并输出localStorage里存储的名字和值
for(var i=0; i<localStorage.length;i++){
    console.log('localStorage里存储的第'+i+'条数据的名字为：'+localStorage.key(i)+',值为：'+localStorage.getItem(localStorage.key(i)));
}
```

#### 删除某个变量



```javascript
localStorage.removeItem("name"); //undefined
localStorage // Storage {length: 0} 可以看到之前保存的name变量已经从localStorage里删除了
```

#### 检查localStorage里是否保存某个变量



```javascript
// 这些数据都是测试的，是在我当下环境里的，只是demo哦～
localStorage.hasOwnProperty('name') // true
localStorage.hasOwnProperty('sex')  // false
```

#### 将数组转为本地字符串



```javascript
var arr = ['aa','bb','cc']; // ["aa","bb","cc"]
localStorage.arr = arr //["aa","bb","cc"]
localStorage.arr.toLocaleString(); // "aa,bb,cc"
```





### sessionStorage

仅在当前会话下有效，关闭页面或浏览器后被清除，其他同上

方法也基本同上

## js性能

### 避免全局查找

访问全局变量始终比访问局部变量慢，因为必须遍历作用域链。任何可以缩短遍历作用域链时间的举措都能提升代码性能。

因为函数执行会搜索作用域链，而全局变量的搜索复杂度一定大于局部变量

```
function updateUI() {
let imgs = document.getElementsByTagName("img");
for (let i = 0, len = imgs.length; i < len; i++) {
imgs[i].title = '${document.title} image ${i}';
}
let msg = document.getElementById("msg");
msg.innerHTML = "Update complete.";
}
//进行如下的改动
function updateUI() {
let doc = document;
let imgs = doc.getElementsByTagName("img");
for (let i = 0, len = imgs.length; i < len; i++) {
imgs[i].title = '${doc.title} image ${i}';
}
let msg = doc.getElementById("msg");
msg.innerHTML = "Update complete.";
}
```

### 避免不必要的属性查找

#### 使用变量和数组相比访问对象属性效率更高

访问数组元素和变量是 O(1)操作

访问对象属性的算法复杂度是 O(n)

访问对象的每个属性都比访问变量或数组花费的时间长，因为查找属性名要搜索原型链

#### 反复查询时将对象属性保存在一个局部变量中

注意避免通过多次查找获取一个值

```
let   query=window.location.href.substring(window.location.href.indexOf("?"));
//这里有 6 次属性查找：3 次是为查找 window.location.href.substring() ，3 次是为查找
window.location.href.indexOf() 。通过数代码中出现的点号数量，就可以知道有几次属性查找
```

只要使用某个 object 属性超过一次，就应该将其保存在局部变量中。第一次仍然要用 O(n)的复杂度去访问这个属性，但后续每次访问就都是 O(1)，这样就是质的提升了

前面的代码可以重写为

```
let url = window.location.href;
let query = url.substring(url.indexOf("?"));
```

只要能够降低算法复杂度，就应该尽量通过在局部变量中保存值来替代属性查找，另外，如果实现某个需求既可以使用数组的数值索引，又可以使用命名属性（比如 NodeList 对象），那就都应该使用数值索引

### 优化DOM交互

使用文档片段构建 DOM结构，然后一次性将它添加到 list 元素。这
个办法可以减少实时更新，也可以避免页面闪烁

```
let list = document.getElementById("myList"),
item;
for (let i = 0; i < 10; i++) {
item = document.createElement("li");
list.appendChild(item);
item.appendChild(document.createTextNode('Item ${i}');
}
//以上代码向列表中添加了 10 项。每添加 1 项，就会有两次实时更新：一次添加 <li> 元素，一次为
它添加文本节点。因为要添加 10 项，所以整个操作总共要执行 20 次实时更新
```

```
let list = document.getElementById("myList"),
fragment = document.createDocumentFragment(),
item;
for (let i = 0; i < 10; i++) {
item = document.createElement("li");
fragment.appendChild(item);
item.appendChild(document.createTextNode("Item " + i));
}
list.appendChild(fragment);
//这样修改之后，完成同样的操作只会触发一次实时更新。这是因为更新是在添加完所有列表项之后
一次性完成的。文档片段在这里作为新创建项目的临时占位符。最后，使用 appendChild() 将所有项
目都添加到列表中。别忘了，在把文档片段传给 appendChild() 时，会把片段的所有子元素添加到父
元素，片段本身不会被添加
```

只要是必须更新 DOM，就尽量考虑使用文档片段来预先构建 DOM 结构，然后再把构建好的 DOM结构实时更新到文档中