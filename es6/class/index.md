[<< 回到主页](http://suzy1993.github.io/misszy/)

## ES6 class

ES6利用class实现更接近于面向对象编程，class其实就是个语法糖，大部分功能ES5都能做到，只是ES6的class看上去更像面向对象编程。

### 1 class
constructor是默认构造函数，生成实例时会自动调用，若不显式定义，会自动生成一个空的constructor，该方法默认返回this，this代表实例对象，其实constructor中也可以返回一个别的对象。
```
class Test {
    constructor(prop) {
        this.prop = prop;
    }
    toString() {
        return "Hello, " + prop;
    }
    ...
}
```
ES5中，直接写构造函数：
```
function Test(prop) {
    this.prop = prop;
}
Test.prototype.toString = function() {
    return "Hello, " + prop;
};
```

### 2 继承
super指代父类的实例，也就是父类的this对象。
子类如果想调用父类的构造器，用super(prop) 。
子类如果想调用父类的方法，用super.method()。
```
class Person {
    constructor(name) {
        this.name = name;
    }
    sayHello() {
        return 'Hello, ' + this.name;
    }
}
class Student extends Person {
    constructor(name, grade) {
        super(name);
        this.grade = grade;
    }
    toString() {
        console.log(this.name + ": “ + this.grade);
    }
}
var stu = new Student('Alice', 99);
stu.toString(); // Alice: 99
```
注意，子类必须在constructor中先调用super, 来继承父类的this对象，然后扩展成自己的，否则子类得不到this对象。

[<< 回到主页](http://suzy1993.github.io/misszy/)
