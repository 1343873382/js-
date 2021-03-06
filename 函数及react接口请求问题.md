## 箭头函数里单行没有大括号表示return

```
let res1= (r1,r2) => r1+r2;//3
    let res2= (r1,r2) => {r1+r2};//underfined
    let res3= (r1,r2) => {return (r1+r2)};3

```

## 出现在后面的函数声明会覆盖掉前面的

## 闭包中的变量都保存在内存中

```

function a(){
	var d=5;d=d+1;
	var b=1;
	function c(){
		console.log(b);
		console.log(d);
		return b++;
	}
	return c;
};
var e=a();
e();
e();
e();
//1,6,2,6,3,6
```

闭包不会重新执行，只会根据返回值调用c函数，从第一个e()开始，进入c()。右侧的变量值仍然变化

```
function a(){
	var d=5;
	var b=1;
	function c(){
		console.log(b);
		console.log(d);
		d++;
		return b++;
	}
	return c;
};
var e=a();
e();
e();
e();
1,5,2,6,3,7
```

## 箭头函数this的作用

```
var obj = {
count: 0,
cool: function coolFn() {
var that = this;
if (self.count < 1) {
setTimeout( function timer(){
that.count++;
console.log( "awesome?" );
}, 100 );
}
}
};
obj.cool(); // 酷吧？
var that = this 这种解决方案圆满解决了理解和正确使用 this 绑定的问题
```

```
var obj = {
count: 0,
cool: function coolFn() {
if (this.count < 1) {
setTimeout( () => { // 箭头函数是什么鬼东西？
this.count++;
console.log( "awesome?" );
}, 100 );
}
}
};
obj.cool(); // 很酷吧 ?
简单来说，箭头函数在涉及 this 绑定时的行为和普通函数的行为完全不一致
```

```
var obj = {
count: 0,
cool: function coolFn() {
if (this.count < 1) {
setTimeout( function timer(){
this.count++; // this 是安全的
// 因为 bind(..)
console.log( "more awesome" );
}.bind( this ), 100 ); // look, bind()!
}
}
};
obj.cool(); // 更酷了。
无论你是喜欢箭头函数中 this 词法的新行为模式，还是喜欢更靠得住的 bind() ，都需要
注意箭头函数不仅仅意味着可以少写代码
```

### 对象属性引用链中只有最顶层或者说最后一层会影响调用位置

```
function foo() {
console.log( this.a );
}
var obj2 = {
a: 42,
foo: foo
};
var obj1 = {
a: 2,
obj2: obj2
};
obj1.obj2.foo(); // 42
```

## 对象上属性原型的添加问题

```
function Animal() {}
Animal.prototype = {
 constructor: Animal,
 number: "very much",
 fish: ["shark","sardine"],
 bird:{
     ability: "fly",
     feature: "feather"
 }
};
var animal1 = new Animal();
var animal2 = new Animal();
var animal3 = new Animal();
//没有改写原型中的fish属性,此时animal1实例对象中有了自己的fish属性，向其自己的fish属性中推入和弹出项不会改变原型的fish属性。
animal1.fish = ["cold fish"];
//通过实例对象animal3向fish属性中推入项，改变了原型对象的fish属性，因为实例对象中没有自己的fish属性
animal3.fish.push("voladao");
animal1.bird
//{ability: "fly", feature: "feather"}
```

1.如果对象原型上本来没有这个属性，你新创建的这个对象会有你写的那个属性，但是原型上不会有

2.如果原型上有这个属性，那你新赋值的属性会在对象体现，而且仅有你给的属性值，原来原型上的属性不会在对象上体现但是会在原型中体现

3.你直接调用改对象原型上的属性，而你没有新添加过那个属性值，那就会显示原型上的属性值

4.animal3.fish.push("voladao");通过实例对象animal3向fish属性中推入项，改变了原型对象的fish属性，因为实例对象中没有自己的fish属性

5.但如果新定义过属性值就不能在属性上push了

## 函数柯里化

## console.log是同步还是异步

![](C:\Users\MECHREVO\Desktop\前端联系\学习文档\console.png)

我们可以发现，异常输出的时候，没展开的时候，显示的name值是`Tom`，点击箭头展开对象里的name则是`Jack`，而且，此时的输出值`Jack`执行语句，明显是在赋值语句`obj.per.name = 'Jack'`之前调用的，展开的时候，却变成了`Jack`，明显有些异常，是不是很神奇？我刚遇到这个问题的时候，也是懵逼状态的。

```
在分析之前，我们得知道一点，JS中对象是引用类型，每次使用对象时，都只是使用了对象在堆中的引用。

当我们在使用obj.per.name = 'Jack'改变了对象的属性值时，它在堆中name的值也变成了'Jack'，而当我们不展开对象看的时候，console.log打印的是对象当时的快照，所以我们看到的name属性值是没改变之前的'Tom'，展开对象时，它其实是重新去内存中读取对象的属性值，所以当我们展开对象后看到的name属性值是Jack。
```

尤其要提出的是，在某些条件下，某些浏览器的console.log(..) 并不会把传入的内容立即输出。出现这种情况的主要原因是，在许多程序（不只是JavaScript）中，I/O 是非常低速的阻塞部分。所以，（从页面/UI 的角度来说）浏览器在后台异步处理控制台I/O 能够提高性能，这时用户甚至可能根本意识不到其发生。

总结：

由此可见，console.log打印出来的内容并不是一定百分百可信的内容。一般对于基本类型`number、string、boolean、null、undefined`的输出是可信的。但对于`Object`等引用类型来说，则就会出现上述异常打印输出。

![](C:\Users\MECHREVO\Desktop\前端联系\学习文档\console_JSON.stringfy().png)

## 跨域问题

### react实战解决

#### 利用proxy代理

在package.json里写

```
  "proxy": "http://localhost:5000"

```

## onChange触发是当前对象属性改变，并且是由键盘或鼠标事件激发的，用dom操作改变无效

而onInput是value值改变时就会发生

## axios

### axios.create

我们应该会遇到这样一个问题，就是使用多个axios，需要配置 url,header,type 等等，那么我们多给请求就会面临写多个配置，看下面我们是怎么来解决他。

```
var instance = axios.create({
  baseURL: 'https://s-domain.com/api/',
  timeout: 1000,
  headers: {'X-Custom-Header': 'foobar'}
});
```

### 拦截器

## react路由跳转

this.props.history.push("")在之前的路由前推一个，就是还能退回到之前的

this.props.history.replace(""),替代之前的，就是不能够退回

