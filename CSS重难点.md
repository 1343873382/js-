# CSS重难点

## BFC布局规则

Block formatting context

1内部的块级元素会在垂直方向，一个接一个地放置；
2.块级元素垂直方向的距离由margin决定。属于同一个BFC的两个相邻的块级元素会发生margin合并，不属于同一个BFC的两个相邻的块级元素不会发生margin合并；
3.每个元素的margin box的左边，与包含border box的左边相接触（对于从左往右的格式化，否则相反）。即使存在浮动也是如此；
4.BFC的区域不会与float box重叠；
5.BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素；外面的元素也不会影响到容器里面的子元素；
6.计算BFC的高度时，浮动元素也参与计算。

### 如何创建一个BFC

首先我们要知道怎样创建BFC。一个BFC可以被显式触发，只需满足以下条件之一：

float的值不为none；
overflow的值不为visible；
position的值为fixed / absolute；
display的值为table-cell / table-caption / inline-block / flex / inline-flex。
————————————————

### BFC的应用

#### 消除margin合并

#### 清除浮动

设置浮动会使父元素高度崩塌

在父元素设置overflow属性即可使父元素具有高度

## overflow属性

1.visible：默认值。内容不会被修剪，会呈现在元素框之外。

2.hidden：与visible相反，内容会被修剪，并且其余内容是不可见的。

这对应付使用动态的内容，而且可能会由于内容溢出而引起一些布局上的问题的确很有用。尽管如此，请记住用此方法隐藏的内容将彻底的看不到(除非去查看源代码)。 比如有的用户设置他们的浏览器的默认字体比你预期的要大些，你会将一些文字推到盒子的外面然后完全的隐藏之……

3.scroll：内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容。

不论是否需要，用户代理都会提供一种滚动机制。因此即使元素框中可以放下所有内容也会出现滚动条，而且使用scroll将会同时产生水平和垂直两个滚动条，就算内容只需要其中一个。

4.auto：如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。解决了scroll在不需要滚动条也会产生滚动条的问题。

5.inherit：规定应该从父元素继承 overflow 属性的值。

其中，overflow属性值中visible和hidden是对立的，scroll和auto是对立的。inherent是继承父级元素的overflow属性值，默认是scroll

## clear：both属性

就是该元素的哪边不允许出现浮动元素

clear:left

clear:right

## 清楚浮动的一些办法

### overflow：hidden生成BFC

### 在父元素内创建一个空div设属性clear：both

因为左右不准出现浮动元素所以div会在浮动元素下面从而制造了高度

### 父元素设置浮动生成BFC

### 用伪元素

```
.fa:after{  
        /* 加入元素的内容 */
        content: "";
        /* 将元素设置为块元素 */
        display: block;
        /* 将元素本身隐藏 */
        visibility: hidden;
        /* 设置元素的高度，如果没有内容可以不设置 */
        height: 0px;
        /* 清除浮动 */
        clear: both;
        /* 超出部分隐藏 */
        overflow: hidden; 
    }
```

## align-item和align-content的区别

align-item是在非主轴只有一个元素时

align-content是一般在在用了换行非主轴有多个元素时使用