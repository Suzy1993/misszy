[<< 回到主页](http://suzy1993.github.io/misszy/)

## ES6 对象扩展

### 1 属性的简洁表示法
ES6 允许直接写入变量和函数，作为对象的属性和方法。这样的书写更加简洁。
变量的简写：
```
var prop = 'name';
var obj = {prop};
// 等同于：
var obj = {prop: prop};
obj // {prop: "name"}
```
方法的简写：
```
var obj = {
    func() {
        return "Hello world!";
    }
};
// 等同于
var obj = {
    func: function() {
        return "Hello world!";
    }
};
```

### 2 属性名表达式
JavaScript定义对象的属性，有两种方法：
```
// 方法一
obj.name = "Alice!";
// 方法二
obj['first' + 'name'] = "Bruce";
```
但是，如果使用字面量方式定义对象（使用大括号），在 ES5 中只能使用方法一（标识符）定义属性。
ES6 允许字面量定义对象时，用方法二（表达式）作为对象的属性名，即把表达式放在方括号内：
```
let obj = {
    [age]: 24,
     ['first' + 'name']: 'Bruce'
};
```
表达式还可以用于定义方法名：
```
let obj = {
    ['get' + 'Name']() {
        return this.name;
    }
};
```
注意，属性名表达式与简洁表示法，不能同时使用，会报错：
```
// 报错
var prop = 'name';
var name = 'Bruce';
var obj = { [prop] };
// 正确
var prop = 'name';
var obj = { [prop]: 'Bruce'};
```
注意，属性名表达式如果是一个对象，默认情况下会自动将对象转为字符串[object Object]：
```
const key1 = {a: 1};
const key1 = {b: 2};
const obj = {
    [key1]: 'value1',
    [key2]: 'value2'
};
obj // Object {[object Object]: "value2"}
```
上面代码中，[key1]和[key2]得到的都是[object Object]，所以[key2]会把[key1]覆盖掉，而obj最后只有一个[object Object]属性。

### 3 方法的name属性
方法的name属性返回函数名（即方法名）：
```
const person = {
    name: 'Alice'
    getName() {
        console.log(this.name);
    },
};
person.getName.name // "getName"
```
有两种特殊情况：bind方法创造的函数，name属性返回bound加上原函数的名字；Function构造函数创造的函数，name属性返回anonymous：
```
(new Function()).name // "anonymous"
var func = function() { ...};
func.bind().name // "bound func"
```

### 4 Object.is()
ES5 比较两个值是否相等，只有两个运算符：相等运算符（==）和严格相等运算符（===）。它们都有缺点，前者会自动转换数据类型，后者的NaN不等于自身，以及+0等于-0。JavaScript 缺乏一种运算，在所有环境中，只要两个值是一样的，它们就应该相等。
Object.is用来比较两个值是否严格相等，与严格比较运算符（===）的行为基本一致：
```
Object.is('a', 'a') // true
Object.is({}, {}) // false
```
不同之处只有两个：一是+0不等于-0，二是NaN等于自身：
```
+0 === -0 //true
NaN === NaN // false
Object.is(+0, -0) // false
Object.is(NaN, NaN) // true
```

### 5 Object.assign()
Object.assign方法用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）：
```
var target = { a: 1 };
var source1 = { b: 2 };
var source2 = { c: 3 };
Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}
```
注意，如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性：
```
var target = { a: 1, b: 1 };
var source1 = { b: 2, c: 2 };
var source2 = { c: 3 };
Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}
```
如果只有一个参数，Object.assign会直接返回该参数：
```
var obj = {a: 1};
Object.assign(obj) === obj // true
```
如果该参数不是对象，则会先转成对象，然后返回：
```
typeof Object.assign(2) // "object"
```
由于undefined和null无法转成对象，所以如果它们作为参数，就会报错：
```
Object.assign(undefined) // 报错
Object.assign(null) // 报错
```
如果非对象参数出现在源对象的位置（即非首参数），那么处理规则有所不同。首先，这些参数都会转成对象，如果无法转成对象，就会跳过。这意味着，如果undefined和null不在首参数，就不会报错：
```
let obj = {a: 1};
Object.assign(obj, undefined) === obj // true
Object.assign(obj, null) === obj // true
```
其他类型的值（即数值、字符串和布尔值）不在首参数，也不会报错。但是，除了字符串会以数组形式，拷贝入目标对象，其他值都不会产生效果：
```
var value1 = 'abc';
var value2 = true;
var value3 = 10;
var obj = Object.assign({}, value1, value2, value3);
console.log(obj); // { "0": "a", "1": "b", "2": "c" }
```
Object.assign拷贝的属性是有限制的，只拷贝源对象的自身属性（不拷贝继承属性），也不拷贝不可枚举的属性（enumerable: false）：
```
Object.assign({key: 'value'},
    Object.defineProperty({}, 'invisible', {
        enumerable: false,
        value: 'hello'
    })
)
// { key: 'value' }
```
注意：Object.assign方法实行的是浅拷贝，而不是深拷贝。也就是说，如果源对象某个属性的值是对象，那么目标对象拷贝得到的是这个对象的引用：
```
var obj1 = {a: {b: 1}};
var obj2 = Object.assign({}, obj1);
obj1.a.b = 2;
obj2.a.b // 2
```
对于这种嵌套的对象，一旦遇到同名属性，Object.assign的处理方法是替换，而不是添加：
```
var target = { a: { b: 'c', d: 'e' } }
var source = { a: { b: 'hello' } }
Object.assign(target, source) // { a: { b: 'hello' } }
```
注意，Object.assign可以用来处理数组，但是会把数组视为对象：
```
Object.assign([1, 2, 3], [4, 5]) // [4, 5, 3]
```
Object.assign()方法的创建用途：
1）为对象添加属性
```
class Point {
    constructor(x, y) {
        Object.assign(this, {x, y});
    }
}
```
2）为对象添加方法
```
Object.assign(SomeClass.prototype, {
    someMethod(arg1, arg2) { ··· },
    anotherMethod() { ··· },
});
// 等同于：
SomeClass.prototype.someMethod = function (arg1, arg2) { ··· };
SomeClass.prototype.anotherMethod = function () { ··· };
```
3）克隆对象
```
function clone(origin) {
    return Object.assign({}, origin);
}
```
采用上面代码克隆，只能克隆原始对象自身的值，不能克隆它继承的值。如果想要保持继承链，可以采用下面的代码：
```
function clone(origin) {
    let originProto = Object.getPrototypeOf(origin);
    return Object.assign(Object.create(originProto), origin);
}
```
4）合并多个对象
将多个对象合并到某个对象：
```
const merge =
    (target, ...sources) => Object.assign(target, ...sources);
```
如果希望合并后返回一个新对象，可以改写上面函数，对一个空对象合并：
```
const merge =
    (...sources) => Object.assign({}, ...sources);
```
5）为属性指定默认值
```
const DEFAULTS = {
    logLevel: 0,
    outputFormat: 'html'
};
function processContent(options) {
    options = Object.assign({}, DEFAULTS, options);
    console.log(options);
    // ...
}
```

### 6 属性的遍历
ES6 一共有5种方法可以遍历对象的属性。  
1）for...in  
for...in循环遍历对象自身的和继承的可枚举属性（不含Symbol 属性）。  
2）Object.keys(obj)  
Object.keys返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含 Symbol 属性）。  
3）Object.getOwnPropertyNames(obj)  
Object.getOwnPropertyNames返回一个数组，包含对象自身的所有属性（不含 Symbol 属性，但是包括不可枚举属性）。  
4）Object.getOwnPropertySymbols(obj)  
Object.getOwnPropertySymbols返回一个数组，包含对象自身的所有 Symbol 属性。  
5）Reflect.ownKeys(obj)  
Reflect.ownKeys返回一个数组，包含对象自身的所有属性，不管属性名是 Symbol 或字符串，也不管是否可枚举。

### 7 Object.keys()，Object.values()，Object.entries()
#### 7.1 Object.keys()
ES5 引入了Object.keys方法，返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键名：
```
var obj = { name: 'Alice', age: 24 };
Object.keys(obj) // ["name", "age"]
```
ES6 引入了跟Object.keys配套的Object.values和Object.entries，作为遍历一个对象的补充手段，供for...of循环使用：
```
let {keys, values, entries} = Object;
let obj = { a: 1, b: 2, c: 3 };
for (let key of keys(obj)) {
    console.log(key); // 'a', 'b', 'c'
}
for (let value of values(obj)) {
    console.log(value); // 1, 2, 3
}
for (let [key, value] of entries(obj)) {
    console.log([key, value]); // ['a', 1], ['b', 2], ['c', 3]
}
```

#### 7.2 Object.values()
Object.values方法返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值：
```
var obj = { name: 'Alice', age: 24 };
Object.values(obj) // ["Alice", 24]
```
返回数组的成员顺序：
```
var obj = { 100: 'a', 2: 'b', 7: 'c' };
Object.values(obj) // ["b", "c", "a"]
```
Object.values只返回对象自身的可遍历属性：
```
var obj = Object.create({}, {p: {value: 42}});
Object.values(obj) // []
```
上面代码中，Object.create方法的第二个参数添加的对象属性（属性p），如果不显式声明，默认是不可遍历的，因为p的属性描述对象的enumerable默认是false，Object.values不会返回这个属性。只要把enumerable改成true，Object.values就会返回属性p的值：
```
var obj = Object.create({}, {
    p: {
        value: 42,
        enumerable: true
    }
});
Object.values(obj) // [42]
```
Object.values会过滤属性名为 Symbol 值的属性：
```
Object.values({ [Symbol()]: 123, name: 'Alice' }); // ['Alice']
```
如果Object.values方法的参数是一个字符串，会返回各个字符组成的一个数组：
```
Object.values('abc') // ['a', 'b', 'c']
```
如果参数不是对象，Object.values会先将其转为对象。由于数值和布尔值的包装对象，都不会为实例添加非继承的属性。所以，Object.values会返回空数组：
```
Object.values(24) // []
Object.values(true) // []
```

#### 7.3 Object.entries
Object.entries方法返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值对数组：
```
var obj = { name: 'Alice', age: 24 };
Object.entries(obj) // [ ["name", "age"], ["Alice", 24] ]
```
除了返回值不一样，该方法的行为与Object.values基本一致。
如果原对象的属性名是一个 Symbol 值，该属性会被忽略：
```
Object.entries({ [Symbol()]: 123, name: 'Alice' }); // [ [ 'name', 'Alice' ] ]
```
上面代码中，原对象有两个属性，Object.entries只输出属性名非 Symbol 值的属性。将来可能会有Reflect.ownEntries()方法，返回对象自身的所有属性。
Object.entries的基本用途是遍历对象的属性：
```
let obj = { one: 1, two: 2 };
for (let [k, v] of Object.entries(obj)) {
    console.log(
        `${JSON.stringify(k)}: ${JSON.stringify(v)}`
    );
}
// "one": 1
// "two": 2
```
Object.entries方法的另一个用处是，将对象转为真正的Map结构：
```
var obj = { name: 'Alice', age: 24 };
var map = new Map(Object.entries(obj));
map // Map { name: 'Alice', age: 24 }
```
自己实现Object.entries方法，非常简单：
```
// Generator函数的版本
function* entries(obj) {
    for (let key of Object.keys(obj)) {
        yield [key, obj[key]];
    }
}
// 非Generator函数的版本
function entries(obj) {
    let arr = [];
    for (let key of Object.keys(obj)) {
        arr.push([key, obj[key]]);
    }
    return arr;
}
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
