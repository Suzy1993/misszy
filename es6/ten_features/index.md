[<< 回到主页](http://suzy1993.github.io/misszy/)

## 10个最佳ES6特性

### 1 函数参数默认值
1）不使用ES6
```
function fn(width, height) {
    var width = width || 200;
    var height = height || 100;
    //...
}
```
问题：当参数对应的布尔值为false时，如通过foo(0, "", "")调用foo函数，因为0对应的的布尔值为false，则width的取值将是200而不是0，height的取值将是10。
2）使用ES6
```
function fn(width = 200, height = 100) {
    // ...
}
```

### 2 模板字符串
1）不使用ES6
使用+号将变量拼接为字符串：
```
var str = "Hello, " + firstName + " " + lastName;
```
2）使用ES6
将变量放在大括号之中：
```
var str = `Hello, ${firstName} ${lastName}`;
```

### 3 多行字符串
1）不使用ES6
使用“\n\t”将多行字符串拼接起来：
```
var info = "Hello, react!\n\t"
    + "Hello world!\n\t";
```
2）使用ES6
将多行字符串放在反引号``之间：
```
var info = `Hello, react!
    Hello world!`;
```

### 4 解构赋值
1）不使用ES6
当需要获取某个对象的属性值时，需要单独获取：
```
var info = getName();
var firstName = info.firstName;
var lastName = info.lastName;
```
2）使用ES6
一次性获取对象的子属性：
```
var { firstName, lastName } = getName();
```
对于数组也一样：
```
var [input1, input2]  = $(".input")；
```

### 5 对象属性简写
1）不使用ES6
对象中必须包含属性和值，显得多余：
```
var name = "Alice";
var getName = function () {
    // ...
};
var person = {
    name: name,
    getName: getName
};
```
2）使用ES6
对象中直接写变量，非常简单：
```
var name = "Alice";
var getName = function () {
    // ...
};
var person = { name，getName };
```

### 6 箭头函数
1）不使用ES6
普通函数体内this指向调用时所在的对象。
```
function getName() {
    console.log(this.name);
}
var name = "Alice";
getName(); // Alice
getName.call({ name: "Bruce" }); // Bruce
```
2）使用ES6
箭头函数体内的this指向定义时所在的对象，而不是调用时所在的对象。
```
var getName = () => {
    console.log(this.name);
}
var name = "Alice";
getName(); // "Alice"
getName.call({ name: "Bruce" }); // "Alice"
```

### 7 Promise
1）不使用ES6
嵌套两个setTimeout回调函数：
```
setTimeout(function() {
    console.log("Hello"); // 1秒后输出"Hello"
    setTimeout(function() {
        console.log("react"); // 2秒后输出"react"
    }, 1000);
}, 1000);
```
2）使用ES6
使用then实现异步编程串行化：
```
var wait1000 = new Promise(function(resolve, reject) {
    setTimeout(resolve, 1000);
});
wait1000
.then(function() {
console.log("Hello"); // 1秒后输出"Hello"
        return wait1000;
    })
.then(function() {
console.log("react"); // 2秒后输出"Fundebug"
});
```

### 8 let与const
1）使用var
var定义的变量为函数级作用域：
```
{
    var a = 10;
}
console.log(a); // 10
```
2）使用let与const
let定义的变量为块级作用域，因此会报错：
```
{
    let a = 10;
}
console.log(a); // 报错“ReferenceError: a is not defined”
```

### 9 类
1）不使用ES6
使用构造函数创建对象：
```
function Person(name) {
    this.name = name;
    this.getName = function() {
        return this.name;
    };
}
var person = new Person("Alice");
console.log(person.getName()); // "Alice"
```
2）使用ES6
使用Class定义类，更加规范，且能够继承：
```
class Person {
    constructor(name) {
this.name = name;
}
    getName() {
        return this.name;
    }
}
var person = new Person("Alice");
console.log(person.getName()); // "Alice"
```

### 10 模块化
JavaScript一直没有官方的模块化解决方案，开发者在实践中主要采用CommonJS和AMD规范。而ES6制定了模块(Module)功能。
1）不使用ES6
Node.js采用CommenJS规范实现模块化，而前端也可以采用，只是在部署时需要使用Browserify等工具打包：
module.js中使用module.exports导出name变量和getName函数：
```
module.exports = {
    name: "Alice",
    getName: function() {
        // ...
    }
}
```
module.js中使用require导入module.js：
```
var person = require("module.js");
console.log(person.name); // "Alice"
```
2）使用ES6
ES6中使用export与import关键词实现模块化。
module.js中使用export导出name变量和getName函数：
```
export var name = "Alice";
export function getName() {
    // ...
};
```
main.js中使用import导入module.js，可以指定需要导入的变量：
```
import { name, getName } from "module";
console.log(name); // "Alice"
```
也可以将全部变量导入：
```
import * as person from "module";
console.log(person.name); // "Alice"
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
