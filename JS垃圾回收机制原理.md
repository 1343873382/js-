# JS垃圾回收机制原理

## 用null将数据回收

```
let user = {
  name: "John"
};
user = null;
```

现在 **John** 变成不可达的状态，没有办法访问它，没有对它的引用。垃圾回收器将丢弃 **John** 数据并释放内存。

### 当有两个引用时

```
let user = {
  name: "John"
};
let admin = user;
user = null;
```

该对象仍然可以通过 `admin` 全局变量访问，所以它在内存中。如果我们也覆盖`admin`（即让admin=null），那么它可以被释放。

### 相互关联的对象

现在来看一个更复杂的例子， family 对象：

```
function marry (man, woman) {
  woman.husban = man;
  man.wife = woman;

  return {
    father: man,
    mother: woman
  }
}

let family = marry({
  name: "John"
}, {
  name: "Ann"
})
```

函数 `marry` 通过给两个对象彼此提供引用来“联姻”它们，并返回一个包含两个对象的新对象。

产生的内存结构:

![图片描述](https://segmentfault.com/img/bVbqcXI)

到目前为止，所有对象都是可访问的。

现在让我们删除两个引用:

```
delete family.father;
delete family.mother.husband;
```

![图片描述](https://segmentfault.com/img/bVbqcZj)

仅仅删除这两个引用中的一个是不够的，因为所有对象仍然是可访问的。

但是如果我们把这两个都删除，那么我们可以看到 **John** 不再有传入的引用:

![图片描述](https://segmentfault.com/img/bVbqcZ4)

输出引用无关紧要。只有传入的对象才能使对象可访问，因此，**John** 现在是不可访问的，并将从内存中删除所有不可访问的数据。

垃圾回收之后：

![图片描述](https://segmentfault.com/img/bVbqc0I)

### 内部算法

基本的垃圾回收算法称为**“标记-清除”**，定期执行以下“垃圾回收”步骤:

- 垃圾回收器获取根并**“标记”**(记住)它们。
- 然后它访问并“标记”所有来自它们的引用。
- 然后它访问标记的对象并标记它们的引用。所有被访问的对象都被记住，以便以后不再访问同一个对象两次。
- 以此类推，直到有未访问的引用(可以从根访问)为止。
- 除标记的对象外，所有对象都被删除

## 面试怎么回答

**1）问什么是垃圾**

一般来说没有被引用的对象就是垃圾，就是要被清除， 有个例外如果几个对象引用形成一个环，互相引用，但根访问不到它们，这几个对象也是垃圾，也要被清除。

**2）如何检垃圾**

一种算法是标记 **标记-清除** 算法