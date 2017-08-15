[<< 回到主页](http://suzy1993.github.io/misszy/)

## JavaScript 继承

### 1构造函数、原型和实例的关系
* 构造函数有一个原型属性prototype指向一个原型对象
* 原型对象包含一个指向构造函数的指针constructor
* 实例包含一个指向原型对象的内部指针[[prototype]]

### 2 通过原型链实现继承
基本思想：利用原型让一个引用类型继承另一个引用类型的属性和方法，子类型可以访问超类型的所有属性和方法。原型链的构建是将一个类型的实例赋值给另一个构造函数的原型实现的。实现的本质是重写原型对象，代之以一个新类型的实例。
```function Person(name) {
    this.name = name;
}
Person.prototype.sayHello = function() {
    alert("Hello, " + this.name);
}
var person = new Person("Alice");
person.sayHello(); // Hello, Alice
function Student() {
}
Student.prototype = new Person("Bruce");
Student.prototype.id = 16;
Student.prototype.showId = function() {
    alert(this.id);
}
var student = new Student();
student.sayHello(); // Hello, Bruce
student.showId(); // 16
```
注意：不能用对象字面量创建原型方法，这样会重写原型链，导致继承无效。
```
function Person(name) {
    this.name = name;
}
Person.prototype.sayHello = function() {
    alert("Hello, " + this.name);
}
var person = new Person("Alice");
person.sayHello(); // Hello, Alice
function Student() {
}
Student.prototype = new Person("Bruce");
Student.prototype.id = 16;
Student.prototype = {
    showId: function() {
        alert(this.id);
    }
};
var student = new Student();
student.sayHello(); // 报错：student.sayHello is not a function
student.showId(); // 16
```
student指向Student的原型，Student的原型又指向Person的原型。
student.sayHello()原型链搜索机制：
1）搜索student实例中是否有sayHello()
2）搜索Student.prototype是否有sayHello()
3）搜索Person.prototype是否有sayHello()
子类型有时候需要覆盖超类型的某个方法，或者需要添加超类型中不存在的某个方法。
```
function Person(name) {
    this.name = name;
}
Person.prototype.sayHello = function() {
    alert("Hello, " + this.name);
}
var person = new Person("Alice");
person.sayHello(); // Hello, Alice
function Student() {
}
Student.prototype = new Person("Bruce");
Student.prototype.id = 16;
Student.prototype.showId = function() {
alert(this.id);
}
Student.prototype.sayHello = function() {
    alert("Hi, " + this.name);
}
var student = new Student();
student.sayHello(); //Hi, Bruce
student.showId(); // 16
```
注意：给原型覆盖或添加方法的代码一定要放在替换原型的语句之后。
```
function Person(name) {
    this.name = name;
}
Person.prototype.sayHello = function() {
    alert("Hello, " + this.name);
}
var person = new Person("Alice");
person.sayHello(); // Hello, Alice
function Student() {
}
Student.prototype.sayHello = function() {
    alert("Hi, " + this.name);
}
Student.prototype = new Person("Bruce");
Student.prototype.id = 16;
Student.prototype.showId = function() {
alert(this.id);
}
var student = new Student();
student.sayHello(); // Hello, Bruce
student.showId(); // 16
```
确定实例和原型的关系：
1）instanceof
```
alert(student instanceof Object); // true
alert(student instanceof Student); // true
alert(student instanceof Person); // true
```
2）isProtptypeOf
```
alert(Object.prototype.isPrototypeOf(student)); // true
alert(Student.prototype.isPrototypeOf(student)); // true
alert(Person.prototype.isPrototypeOf(student)); // true
```
3）getPrototypeOf
```
Object.getPrototypeOf(student1) == Student.prototype
```
使用原型链实现继承的问题：
1）引用类型的属性会被实例共享，原型实现继承时，原型会变成另外一个类型的实例，实例的属性则变成了现在的原型属性，从而被共享。
```
function Person(name, age) {
    this.friends = ["Cindy","David"];
}
function Student() {
}
Student.prototype = new Person();
var student1 = new Student();
student1.friends.push("Bruce");
alert(student1.friends); // "Cindy","David","Bruce"
var student2 = new Student();
alert(student1.friends); // "Cindy","David","Bruce"
```
2）在创建子类型的实例时，不能向超类型的构造函数中传递参数，实际上，应该是没有办法在不影响所有对象实例的情况下，给超类型的构造函数传递参数。
实际中很少单独使用原型链实现继承。

### 3 通过构造函数实现继承
基本思想：在子类型构造函数的内部调用超类型构造函数。通过使用apply()和call()方法也可以在新创建的对象上执行构造函数。
```
function Person(name, age) {
    this.name = name;
    this.age = age;
}
function Student() {
    Person.call(this,"Alice",22); // 继承了构造函数Person，同时还传递了参数
    this.id = 16; // 实例属性
}
var student = new Student();
alert(student.name); // "Alice"
alert(student.age); // 22
alert(student.id); // 16
```
使用构造函数实现继承的问题：
1）在超类型的原型中定义的方法，对子类型而言是不可见的，结果所有类型都只能使用构造函数模式。
2）要想子类能够访问超类定义的方法，方法只能在构造函数中定义，但方法在构造函数中定义时，函数复用无从谈起。
```
function Person(name, age) {
    this.name = name;
    this.age = age;
}
Person.prototype.showName = function() {
    alert(this.name);
};
function Student() {
    Person.call(this,"Alice",22);
    this.id = 16;
}
var student = new Student();
alert(student.showName()); // 报错：student.showName is not a function
```
实际中很少单独使用使用构造函数实现继承。

### 4 组合使用原型链和构造函数实现继承
思路：使用原型链继承共享的属性和方法，使用构造函数继承实例属性。
效果：既通过在原型上定义方法实现了函数复用，又能够保证每个实例都有自己的属性。
```
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.friends = ["Cindy","David"];
}
Person.prototype.sayHello = function() {
    alert("Hello, " + this.name);
}
function Student(name, age, id) {
    Person.call(this, name, age);
    this.id = id;
}
Student.prototype = new Person();
Student.prototype.showId = function() {
    alert(this.id);
}
var student1 = new Student("Alice", 22, 16);
student1.friends.push("Emy");
alert(student1.friends); // "Cindy","David","Emy"
student1.sayHello(); // Hello, Alice
student1.showId(); // 16
var student2 = new Student("Bruce", 23, 17);
alert(student2.friends); // "Cindy","David"
student2.sayHello(); // Hello, Bruce
student2.showId(); // 17
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
