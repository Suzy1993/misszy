[<< 回到主页](http://suzy1993.github.io/misszy/)

## ES6 解构赋值

本质上，解构赋值的写法属于“模式匹配”，只要等号两边的模式相同，左边的变量就会被赋予对应的值。
如果解构不成功，变量的值就等于undefined。

### 1 数组的解构赋值
#### 1.1 嵌套数组解构
```
let [first, [[second], third]] = [1, [[2], 3]];
first // 1
second // 2
third // 3
let [ , , third] = [1, 2, 3];
third // 3
let [first, , third] = [1, 2, 3];
first // 1
third // 3
let [head, ...tail] = [1, 2, 3];
head // 1
tail // [2,3]
let [left, middle, ...right] = [1];
left // 1
middle // undefined
right // []
```

#### 1.2 不完全解构
等号左边的模式，只匹配一部分的等号右边的数组。
```
let [a] = [1, 2, 3];
a // 1
let [a, [b], d] = [1, [2, 3], 4];
a // 1
b // 2
d // 4
```
如果等号的右边不是数组等可遍历的结构，将会报错：
```
let [a] = 1; // 报错
let [a] = false; // 报错
let [a] = NaN; // 报错
let [a] = undefined; // 报错
let [a] = null; // 报错
let [a] = {}; // 报错
对于 Set 结构，也可以使用数组的解构赋值：
let [a, b, c] = new Set([1, 2, 3]);
a // 1
```

#### 1.3 默认值
解构赋值允许指定默认值：
```
let [a = 1] = [];
a // true
let [a, b = 2] = [1]; // a = 1, b = 2
let [a, b = 2] = [1, undefined]; // a = 1, b = 2
```
注意，ES6 内部使用严格相等运算符（===），判断一个位置是否有值，所以，如果一个数组成员不严格等于undefined，如null，默认值是不会生效的：
```
let [a = 1] = [undefined];
a // 1
let [a = 1] = [null];
a // null
```

### 2 对象的解构赋值
对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。
```
let { name, age } = { name: "Alice", age: 24 };
name // "Alice"
age // 24
let { name1 } = { name: "Alice", age: 24 };
name1 // undefined
```
#### 2.1 嵌套对象解构
```
let obj = {
    prop: [
        'Hello',
        { b: 'World' }
    ]
};
let { prop: [a, { b }] } = obj;
a // "Hello"
b // "World"
```
注意，此时的prop是模式，不是变量，因此不会被赋值。
如果prop也要作为变量赋值，可以写成：
```
let obj = {
    prop: [
        'Hello',
        { b: 'World' }
    ]
};
let { prop, prop: [a, { b }] } = obj;
a // "Hello"
b // "World"
prop // ["Hello", { b: "World" }]
```
下面的例子中最后一次对line属性的解构赋值之中，只有x是变量，position和start都是模式，不是变量：
```
var node = {
    position: {
        start: {
            x: 100,
            y: 50
        }
    }
};
var { position, position: { start }, position: { start: { x }} } = x;
x // 1
position // Object {position: Object}
start // Object {x: 100, y: 50}
```
嵌套赋值的例子：
```
let obj = {};
let arr = [];
({ name: obj.prop, age: arr[0] } = { name: "Alice", age: 24 });
obj // {prop: age}
arr // [24]
```

#### 2.2 默认值
```
var {a = 3} = {};
a // 3
var {a, b = 5} = {x: 1};
a // 1
b // 5
var {a: b= 3} = {};
b // 3
var {a: b = 3} = {a: 5};
b // 5
```
默认值生效的条件是，对象的属性值严格等于undefined：
```
var {a = 3} = {a: undefined};
a // 3
var {a = 3} = {a: null};
a // null
```
如果解构模式是嵌套的对象，而且子对象所在的父属性不存在，那么将会报错：
```
let {a: {b}} = {c: 3}; // 报错
```

### 3 字符串的解构赋值
字符串也可以解构赋值。这是因为此时，字符串被转换成了一个类似数组的对象：
```
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"
```
类似数组的对象都有一个length属性，因此还可以对这个属性解构赋值：
```
let {length : len} = 'hello';
len // 5
```

### 4 函数参数的解构赋值
函数的参数也可以使用解构赋值：
```
function add([x, y]){
    return x + y;
}
add([1, 2]); // 3
```
函数参数的解构也可以使用默认值：
```
function getArr({a = 10, b = 20} = {}) {
    return [a, b];
}
getArr({a: 30, b: 60}); // [30, 60]
getArr({a: 30}); // [30, 20]
getArr({}); // [10, 20]
getArr(); // [10, 20]
```
注意，下面的写法是为函数move的参数指定默认值，而不是为变量x和y指定默认值，所以会得到与前一种写法不同的结果：
```
function move({x, y} = { x: 0, y: 0 }) {
    return [x, y];
}
getArr({a: 30, b: 60}); // [30, 60]
getArr({a: 30}); // [30, undefined]
getArr({}); // [undefined, undefined]
getArr(); // [10, 20]
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
