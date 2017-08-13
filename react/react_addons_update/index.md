[<< 回到主页](http://suzy1993.github.io/misszy/)

### 参考网址
[https://facebook.github.io/react/docs/update.html](https://facebook.github.io/react/docs/update.html)

### 1 概述
引入：import update from 'react-addons-update’;
React允许使用所需的任何风格的数据管理，包括mutation。但是，如果可以在应用程序的性能关键部分使用不可变数据，那么很容易实现一个快速的shouldComponentUpdate()方法来显着加快应用程序的速度。
在JavaScript中处理不可变数据比为Clojure设计的语言要困难得多。但是，我们提供了一个简单的immutability helper不可变数据帮助器update()，它使处理这种类型的数据变得更加容易，而不会从根本上改变数据的表示方式。还可以查看Facebook的Immutable-js和Advanced Performance部分，了解有关Immutable-js的更多详细信息。
如果像下面这样修改数据：
```
myData.x.y.z = 7;
myData.a.b.push(9);
```
将无法确定自上一个副本被覆盖以来哪些数据已更改。相反，需要创建一个新的myData副本，并只更改需要更改的部分，然后可以使用===将myData的旧副本与shouldComponentUpdate()中的新副本进行比较：
```
const newData = deepCopy(myData);
newData.x.y.z = 7;
newData.a.b.push(9);
```
不幸的是，深拷贝是昂贵的，有时是不可能的。可以通过复制需要更改的对象和重用未更改的对象来缓解此问题。不幸的是，在如今的JavaScript中，这可能很麻烦：
```
const newData = extend(myData, {
    x: extend(myData.x, {
        y: extend(myData.x.y, {z: 7}),
    }),
    a: extend(myData.a, {b: myData.a.b.concat(9)})
});
```
虽然这是相当的性能（因为它只是对logn个对象作了一个浅拷贝，重用其余部分），这是写的一个很大痛点。看看所有的重复！这不仅令人烦恼，而且还为bug提供了一个很大的表面积。
update()在这个模式下提供了简单的语法糖，使得这个代码更容易编写。该代码变成：
```
import update from 'react-addons-update';
const newData = update(myData, {
    x: {y: {z: {$set: 7}}},
    a: {b: {$push: [9]}}
});
```
虽然语法需要稍微习惯于没有冗余（虽然它受到MongoDB的查询语言的启发），但它是静态可分析的，并不比可变版本多得多。
以$为前缀的key称为命令。它们改变的数据结构称为目标。

### 2 可用的命令
* {$push: array}：push()在目标中的数组中的所有项目。
* {$unshift: array}：unshift()在目标中数组中的所有项目。
* {$splice: array of arrays}：数组中的每个项目使用项目提供的参数调用目标上的splice()。 
* {$set: any}：完全替换目标。 
* {$merge: object}：将对象的key与目标进行合并。
* {$apply: function}：将当前值传递给函数，并使用新的返回值进行更新。

### 3 实例
* {$push: array}
```
const initialArray = [1, 2, 3];
const newArray = update(initialArray, {$push: [4]}); // => [1, 2, 3, 4]
```
* {$splice: array of arrays}
```
const collection = [1, 2, {a: [12, 17, 15]}];
const newCollection = update(collection, {2: {a: {$splice: [[1, 1, 13, 14]]}}});
// => [1, 2, {a: [12, 13, 14, 15]}]
```
* {$merge: object}
```
const obj = {a: 5, b: 3};
const newObj = update(obj, {$merge: {b: 6, c: 7}}); // => {a: 5, b: 6, c: 7}
```
* {$apply: function} & {$set: any}
```
const obj = {a: 5, b: 3};
const newObj = update(obj, {b: {$apply: function(x) {return x * 2;}}}); // => {a: 5, b: 6}
const newObj2 = update(obj, {b: {$set: obj.b * 2}}); // 等价，但对于深嵌套的集合来说是冗长的
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
