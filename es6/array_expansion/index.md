[<< 回到主页](http://suzy1993.github.io/misszy/)

## ES6 数组扩展

### 1 扩展运算符
扩展运算符是三个点（...），好比 rest 参数的逆运算，将一个数组转为用逗号分隔的参数序列。
```
console.log(...[1, 2, 3]) // 1 2 3
console.log(1, ...[2, 3, 4], 5) // 1 2 3 4 5
[...document.querySelectorAll('div')] // [<div>, <div>, <div>]
```
扩展运算符主要用于函数调用：
```
function push(array, ...items) {
    array.push(...items);
}
function add(x, y) {
    return x + y;
}
var numbers = [10, 20];
add(...numbers) // 30
```
扩展运算符与正常的函数参数可以结合使用：
```
function func(a, b, c, d, e) { }
var args = [1, 2];
func(0, ...args, 3, ...[4]);
```
扩展运算符后面还可以放置表达式：
```
const arr = [
    ...(x > 0 ? ['a'] : []),
    'b',
];
```
如果扩展运算符后面是一个空数组，则不产生任何效果：
```
[...[], 1] // [1]
```
扩展运算符的应用：
1）合并数组
扩展运算符提供了数组合并的新写法：
```
var arr1 = ['a', 'b'];
var arr2 = ['c'];
var arr3 = ['d', 'e'];
// ES5
arr1.concat(arr2, arr3); // [ 'a', 'b', 'c', 'd', 'e' ]
// ES6
[...arr1, ...arr2, ...arr3] // [ 'a', 'b', 'c', 'd', 'e' ]
```
2）与解构赋值结合
扩展运算符可以与解构赋值结合起来，用于生成数组。
```
// ES5
a = list[0], rest = list.slice(1)
// ES6
[a, ...rest] = list
const [first, ...rest] = [1, 2, 3, 4, 5];
first // 1
rest // [2, 3, 4, 5]
const [first, ...rest] = [];
first // undefined
rest // []
const [first, ...rest] = ["a"];
first // "a"
rest // []
```
如果将扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错。
```
const [...left, right] = [1, 2, 3, 4, 5]; // 报错
const [left, ...middle, right] = [1, 2, 3, 4, 5]; // 报错
```
3）函数的返回值
JavaScript 的函数只能返回一个值，如果需要返回多个值，只能返回数组或对象。扩展运算符提供了解决这个问题的一种变通方法。
```
var date = generateDate();
var d = new Date(...date);
```
4）字符串
扩展运算符还可以将字符串转为真正的数组：
```
[...'hello'] // [ "h", "e", "l", "l", "o" ]
```

### 2 Array.from()
用于将两类对象转为真正的数组：
1）类数组对象
```
let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};
// ES5
var arr1 = [].slice.call(arrayLike); // ['a', 'b', 'c']
// ES6
let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']
```
实际应用中，常见的类数组对象是DOM操作返回的NodeList集合，以及函数内部的arguments对象。Array.from都可以将它们转为真正的数组：
```
// NodeList集合
let divs = document.querySelectorAll('div');
Array.from(divs).forEach(function(div) {
    console.log(div);
});
// arguments对象
function func() {
    var args = Array.from(arguments);
    // ...
}
```
2）可遍历对象（包括ES6新增的数据结构Set和Map等）
```
Array.from('hello') // ['h', 'e', 'l', 'l', 'o']
let set = new Set(['a', 'b'])
Array.from(set) // ['a', 'b']
```
注意：扩展运算符（...）也可以将NodeList集合和arguments对象转为数组：
```
// NodeList集合
[...document.querySelectorAll('div')]
// arguments对象
function func() {
    var args = [...arguments];
}
```
扩展运算符背后调用的是遍历器接口（Symbol.iterator），如果一个对象没有部署这个接口，就无法转换。Array.from方法还支持类似数组的对象。所谓类数组对象，本质特征只有一点，即必须有length属性。因此，任何有length属性的对象，都可以通过Array.from方法转为数组，而此时扩展运算符就无法转换：
```
Array.from({ length: 3 }); // [ undefined, undefined, undefined ]
```
对于还没有部署Array.from方法的浏览器，可以用Array.prototype.slice方法替代。
```
const toArray = (() =>
    Array.from ? Array.from : obj => [].slice.call(obj)
)();
```
Array.from还可以接受第二个参数，作用类似于数组的map方法，用来对每个元素进行处理，将处理后的值放入返回的数组。
```
Array.from(arrayLike, x => x * x);
// 等同于：
Array.from(arrayLike).map(x => x * x);
Array.from([1, 2, 3], (x) => x * x) // [1, 4, 9]
```

### 3 Array.of()
Array.of方法用于将一组值，转换为数组：
```
Array.of(3, 11, 8) // [3,11,8]
Array.of(3) // [3]
Array.of(3).length // 1
```
这个方法的主要目的，是弥补数组构造函数Array()的不足。因为参数个数的不同，会导致Array()的行为有差异，只有当参数个数不少于2个时，Array()才会返回由参数组成的新数组。参数个数只有一个时，实际上是指定数组的长度：
```
Array() // []
Array(3) // [, , ,]
Array(3, 11, 8) // [3, 11, 8]
```
Array.of基本上可以用来替代Array()或new Array()，并且不存在由于参数不同而导致的重载。它的行为非常统一，总是返回参数值组成的数组。如果没有参数，就返回一个空数组：
```
Array.of() // []
Array.of(undefined) // [undefined]
Array.of(1) // [1]
Array.of(1, 2) // [1, 2]
Array.of方法可以用下面的代码模拟实现：
function ArrayOf(){
    return [].slice.call(arguments);
}
```

### 4 数组实例的 copyWithin()
在当前数组内部，将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组。也就是说，使用这个方法，会修改当前数组：
```
Array.prototype.copyWithin(target, start = 0, end = this.length)
```
它接受三个参数，这三个参数都应该是数值，如果不是，会自动转为数值。
target（必需）：从该位置开始替换数据。
start（可选）：从该位置开始读取数据，默认为0。如果为负值，表示倒数。
end（可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示倒数。
```
[1, 2, 3, 4, 5].copyWithin(0, 3); // [4, 5, 3, 4, 5]
[1, 2, 3, 4, 5].copyWithin(0, -2, -1); // [4, 2, 3, 4, 5]
[].copyWithin.call({length: 5, 3: 1}, 0, 3); // {0: 1, 3: 1, length: 5}
[1, 2, 3, 4, 5].copyWithin(0, 2); // [3, 4, 5, 4, 5]
```

### 5 数组实例的find()和findIndex()
数组实例的find方法，用于找出第一个符合条件的数组成员。它的参数是一个回调函数，所有数组成员依次执行该回调函数，直到找出第一个返回值为true的成员，然后返回该成员。如果没有符合条件的成员，则返回undefined：
```
[1, 4, -5, 10].find((n) => n < 0); // -5
```
find方法的回调函数可以接受三个参数，依次为当前的值、当前的位置和原数组：
```
[1, 5, 10, 15].find(function(value, index, arr) {
    return value > 9;
}) // 10
```
数组实例的findIndex方法的用法与find方法非常类似，返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回-1：
```
[1, 5, 10, 15].findIndex(function(value, index, arr) {
    return value > 9;
}) // 2
```
这两个方法都可以接受第二个参数，用来绑定回调函数的this对象。
另外，这两个方法都可以发现NaN，弥补了数组的IndexOf方法的不足，indexOf方法无法识别数组的NaN成员，但是findIndex方法可以借助Object.is方法做到：
```
[NaN].indexOf(NaN);// -1
[NaN].findIndex(y => Object.is(NaN, y));// 0
```

### 56 数组实例的 fill()
fill方法使用给定值，填充一个数组：
```
['a', 'b', 'c'].fill(7); // [7, 7, 7]
new Array(3).fill(7); // [7, 7, 7]
```
fill方法用于空数组的初始化非常方便。数组中已有的元素，会被全部抹去。
fill方法还可以接受第二个和第三个参数，用于指定填充的起始位置和结束位置：
```
['a', 'b', 'c'].fill(7, 1, 2); // ['a', 7, 'c']
```

7、数组实例的 entries()，keys() 和 values()
ES6 提供三个新的方法——entries()，keys()和values()——用于遍历数组。它们都返回一个遍历器对象Iterator，可以用for...of循环进行遍历，唯一的区别是keys()是对键名的遍历、values()是对键值的遍历，entries()是对键值对的遍历：
```
for (let index of ['a', 'b'].keys()) {
    console.log(index);
}
// 0
// 1
for (let elem of ['a', 'b'].values()) {
    console.log(elem);
}
// 'a'
// 'b'
for (let [index, elem] of ['a', 'b'].entries()) {
    console.log(index, elem);
}
// 0 "a"
// 1 "b"
```
如果不使用for...of循环，可以手动调用遍历器对象的next方法，进行遍历：
```
let elements = ['a', 'b', 'c'];
let entries = letter.entries();
console.log(elements.next().value); // [0, 'a']
console.log(elements.next().value); // [1, 'b']
console.log(elements.next().value); // [2, 'c']
```

### 8 数组实例的 includes()
Array.prototype.includes方法返回一个布尔值，表示某个数组是否包含给定的值，与字符串的includes方法类似：
```
[1, 2, 3].includes(2) // true
```
该方法的第二个参数表示搜索的起始位置，默认为0。如果第二个参数为负数，则表示倒数的位置，如果这时它大于数组长度，则会重置为从0开始：
```
[1, 2, 3].includes(3, 3);  // false
[1, 2, 3].includes(3, -1); // true
```
没有该方法之前，通常使用数组的indexOf方法，检查是否包含某个值，indexOf方法有两个缺点，一是不够语义化，它的含义是找到参数值的第一个出现位置，所以要去比较是否不等于-1，表达起来不够直观。二是，它内部使用严格相等运算符（===）进行判断，这会导致对NaN的误判：
```
[NaN].indexOf(NaN); // -1
[NaN].includes(NaN) // true
```

### 9 数组的空位
数组的空位指，数组的某一个位置没有任何值。
注意，空位不是undefined，一个位置的值等于undefined，依然是有值的。空位是没有任何值，in运算符可以说明这一点：
```
0 in [undefined, undefined, undefined] // true
0 in [, , ,] // false
```
ES5 对空位的处理，已经很不一致了，大多数情况下会忽略空位：
forEach(), filter(), every()和some()都会跳过空位。
map()会跳过空位，但会保留这个值
join()和toString()会将空位视为undefined，而undefined和null会被处理成空字符串。
```
[,'a'].forEach((x,i) => console.log(i)); // 1
['a',,'b'].filter(x => true) // ['a','b']
[,'a'].every(x => x==='a') // true
[,'a'].some(x => x !== 'a') // false
[,'a'].map(x => 1) // [,1]
[,'a',undefined,null].join('#') // "#a##"
[,'a',undefined,null].toString() // ",a,,"
```
ES6 则是明确将空位转为undefined。
Array.from方法会将数组的空位，转为undefined，也就是说，这个方法不会忽略空位：
```
Array.from(['a',,'b']); // [ "a", undefined, "b" ]
```
扩展运算符（...）也会将空位转为undefined：
```
[...['a',,'b']]; // [ "a", undefined, "b" ]
```
copyWithin()会连空位一起拷贝：
```
[,'a','b',,].copyWithin(2,0) // [,"a",,"a"]
``
fill()会将空位视为正常的数组位置：
```
new Array(3).fill('a') // ["a","a","a"]
```
for...of循环也会遍历空位：
```
let arr = [, ,];
for (let i of arr) {
    console.log(1);
}
// 1
// 1
```
entries()、keys()、values()、find()和findIndex()会将空位处理成undefined：
```
[...[,'a'].entries()] // [[0,undefined], [1,"a"]]
[...[,'a'].keys()] // [0,1]
[...[,'a'].values()] // [undefined,"a"]
[,'a'].find(x => true) // undefined
[,'a'].findIndex(x => true) // 0
```
由于空位的处理规则非常不统一，所以建议避免出现空位。

[<< 回到主页](http://suzy1993.github.io/misszy/)
