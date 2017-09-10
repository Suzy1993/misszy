[<< 回到主页](http://suzy1993.github.io/misszy/)

## ES6 函数扩展

### 1 函数参数默认值
ES6 允许为函数的参数设置默认值，即直接写在参数定义的后面。
```
function func(x, y = ' World') {
    console.log(x, y);
}
func ('Hello') // Hello World
func ('Hello', ', Alice') // Hello, Alice
func ('Hello', '') // Hello
```
参数变量是默认声明的，所以不能用let或const再次声明：
```
function func(x = 5) {
    let x = 1; // error
    const x = 2; // error
}
```
一个容易忽略的地方是，参数默认值不是传值的，而是每次都重新计算默认值表达式的值：
```
let i = 1;
function func(j = i + 1) {
    console.log(j);
}
func () // 2
i = 2;
foo() // 3
```
指定了默认值以后，函数的length属性，将返回没有指定默认值的参数个数。也就是说，指定了默认值后，length属性将失真。
```
(function (a) {}).length // 1
(function (a = 5) {}).length // 0
(function (a, b, c = 5) {}).length // 2
```

### 2 rest参数
ES6 引入 rest 参数（形式为...变量名），用于获取函数的多余参数，这样就不需要使用arguments对象了。rest 参数搭配的变量是一个数组，该变量将多余的参数放入数组中。
```
// arguments变量的写法
function sortNumbers() {
    return Array.prototype.slice.call(arguments).sort();
}
// rest参数的写法
const sortNumbers = (...numbers) => numbers.sort();
```
rest 参数中的变量代表一个数组，所以数组特有的方法都可以用于这个变量。下面是一个利用 rest 参数改写数组push方法的例子：
```
function push(array, ...items) {
    items.forEach(function(item) {
        array.push(item);
        console.log(item);
    });
}
var a = [];
push(a, 1, 2, 3);
```
注意，rest 参数之后不能再有其他参数（即只能是最后一个参数），否则会报错：
```
function f(a, ...b, c) { ... } // 报错
```
函数的length属性，不包括rest参数：
```
(function(a) {}).length  // 1
(function(...a) {}).length  // 0
(function(a, ...b) {}).length  // 1
```

### 3 name属性
函数的name属性，返回该函数的函数名。
```
function func() {}
func.name // "func"
```
这个属性早就被浏览器广泛支持，但是直到 ES6，才将其写入了标准。
需要注意的是，ES6 对这个属性的行为做出了一些修改，如果将一个匿名函数赋值给一个变量，ES5 的name属性，会返回空字符串，而 ES6 的name属性会返回实际的函数名:
```
var func = function() {};
// ES5
func.name // ""
// ES6
func.name // "func"
```
如果将一个具名函数赋值给一个变量，则 ES5 和 ES6 的name属性都返回这个具名函数原本的名字。
```
var func = function fn() {};
// ES5
func.name // "fn"
// ES6
func.name // "fn"
Function构造函数返回的函数实例，name属性的值为anonymous：
(new Function).name // "anonymous"
bind返回的函数，name属性值会加上bound前缀。
function func() {};
func.bind({}).name // "bound func"
(function(){}).bind({}).name // "bound "
```

### 4 箭头函数
ES6标准新增了一种新的函数：Arrow Function（箭头函数）
箭头函数相当于匿名函数，且简化了函数定义。
#### 4.1 箭头函数有两种格式：
a）只包含一个表达式，{ ... }和return都省略掉。
```
i => i * i
```
b）可以包含多条语句，不能省略{ ... }和return。
```
x => {
    if (x > 0)
        return x * x;
    else
        return - x * x;
}
```

#### 4.2 箭头函数的参数个数：
a）无参数：用括号()
```
() => 3.14
```
b）1个参数：
```
i => i * i
```
相当于：
```
function (i) {
    return i * i;
}
```
c）至少2个参数：用括号()括起来
```
(a, b) => a + b
```
相当于：
```
function (a, b) {
    return a + b;
}
```
d）可变参数：
```
(x, y, ..., arr) => {
    var i, sum = x + y;
    for (i = 0; i < arr.length; i++)
        sum += arr[i];
    return sum;
}
```
注意：
如果要返回一个对象，如果是单表达式，这么写会报错：x => { name: "Alice" }
因为和函数体的{ ... }有语法冲突，改为：x => ({ name: "Alice" })

#### 4.3 this
虽然箭头函数看上去是匿名函数的一种简写，但实际上，箭头函数和匿名函数有个明显的区别：箭头函数内部的this是词法作用域，由上下文确定。也就是说，this对象是定义时所在的对象，而不是运行时所在的对象。
1）箭头函数没有自己的this值，箭头函数内的this值继承自外围作用域：
什么时候不该使用箭头函数？
a）在对象上定义方法：
```
var name = 'Window';
var object = {
    name: 'Bruce',
    getName1: function() {
        console.log(this.name);
    },
    getName2: () => {
        console.log(this.name);
    },
    getName3() {
        console.log(this.name);
    }
};
object.getName1(); // My object
object.getName2(); // Window
object.getName3(); // My object
```
b）在原型上定义方法：
```
var name = 'Window'
function Person(name) {
    this.name = name;
}
Person.prototype.getName1 = () => {
    console.log(this.name);
};
Person.prototype.getName2 = function() {
    console.log(this.name);
};
var person = new Person('My object');
person.getName1(); // Window
person.getName2(); // My object
```
c）构造函数中：
```
function Person1(name) {
    this.name = name;
}
var Person2 = (name) => {
    this.name = name;
}
var person1 = new Person1('My object1');
var person2 = new Person2('My object2'); // 报错：Uncaught TypeError: Person2 is not a constructor
```
2）一个内部函数不能直接从外部函数访问到this：
```
var object = {
    name: "My object",
    getName: function() {
        var func = function() {
            return this.name;  // this指向window或undefined
        };
        return func();
    }
};
alert(object.getName()); // 输出：undefined
```
什么时候需要使用箭头函数？
除了可以通过将this对象存储在另一个变量（如that）中来解决这个问题，还可以使用箭头函数：
```
var object = {
    name: "My object",
    getName: function() {
        var func = () => {
            return this.name;  // this指向object
        };
        return func();
    }
};
alert(object.getName()); // 输出：My object
```
注意：由于this在箭头函数中已经按照词法作用域绑定了，所以，用call()或apply()调用箭头函数时，无法对this进行绑定，即传入的第一个参数被忽略。
```
var object = {
    name: "My object",
    getName: function() {
        var func = () => {
            return this.name;  // this指向object
        };
        return func.call({name: "Bruce"});
    }
};
alert(object.getName()); // 输出：My object
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
