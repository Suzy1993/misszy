[<< 回到主页](http://suzy1993.github.io/misszy/)

## JavaScript 创建对象的几种方式

JS中没有类的概念。

### 1 传统的创建对象的方式
#### 1.1 创建Object的实例
```
var person = new Object();
person.name = "Alice";
person.age = 12;
person.showName = function() {
    alert(this.name);
};
```

#### 1.2 对象字面量形式创建单个对象
```
var person = {
    name : "Alice";
    age : 12;
    showName : function() {
        alert(person.name);
    }
};
```

### 2 创建对象的五种设计模式
#### 2.1 工厂模式
虽然Object构造函数和对象字面量都可以用来创建单个对象，但这个方式有个明显的缺点：使用同一个接口创建很多对象，会产生大量重复的代码。为了解决这个问题，开始使用工厂模式。
```
function createPerson(name, age) {
    var obj = new Object();
    obj.name = name;
    obj.age = age;
    obj.showName = function() {
        alert(this.name);
    };
    return obj;
}
var person1 = createPerson("Alice", 23);
var person2 = createPerson("Bruce", 22);
```

#### 2.2 构造函数模式
工厂模式虽然解决了创建多个相似对象的问题，但却没有解决对象识别的问题（即不知道对象的类型），于是，又出现了构造函数模式，自定义的构造函数意味着将来可以把它的实例识别为一种特定的类型。这是构造函数模式胜过工厂模式的地方。
```
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.showName = function() {
        alert(this.name);
    };
}
var person1 = new Person("Alice", 23);
var person2 = new Person("Bruce", 22);
```
构造函数模式与工厂模式的不同之处：
* 没有显式地创建对象；
* 直接将属性和方法赋给了this对象；
* 没有return语句。

构造函数的问题：  
每个方法都要在每个实例上重新创建一遍。由于JavaScript中的函数是对象，每定义一个函数，就是实例化了一个Funtion对象，因此，使用构造函数创建的每个实例都有一个名为showName()的方法，但这些方法不是同一个Function的实例。不同实例上的同名函数是不相等的，因此person1.showName == person2.showName返回false。  
可以通过把函数定义转移到构造函数外部来解决这个问题，如下：
```
function Person(name,age,job) {
    this.name = name;
    this.age = age;
    this.showName = showName;
}
function showName(){
    alert(this.name);
}
var person1 = new Person("Alice", 23);
var person2 = new Person("Bruce", 22);
```
这样虽然解决了方法多次创建问题，但又出现了新的问题：
* 在全局作用域中定义的函数实际上只能被某个对象调用，这让全局作用域名不副实。
* 如果对象需要定义很多方法，那么就需要定义很多个全局函数，那么就毫无封装性可言了。
这些问题可以通过使用原型模式来解决。

#### 2.3 原型模式
每个函数都以一个原型prototype属性，是一个指针，指向一个对象。  
使用原型对象的好处是可以让所有对象实例共享它所包含的属性和方法。也就是说，不必在构造函数中定义对象实例的信息，而是可以直接将这些信息添加到原型对象中。
```
function Person() {
}
Person.prototype.name = name;
Person.prototype.age = age;
Person.prototype.showName = function(){
    alert(this.name);
};
var person1 = new Person();
var person2 = new Person();
```
使用原型模式创建的新对象具有相同的属性和方法。与构造函数模式不同的是，新对象的这些属性和方法是由所有对象所共享的。这会导致所有实例默认有一样的属性值，因此person1.showName == person2.showName返回true。  
读取某个对象的某个属性的搜索方法：  
1）首先在实例中搜索，若找到指定属性，则返回该属性的值。  
2）否则继续搜索指针指向的原型对象。  
使用delete 实例名.属性名可以删除实例的某一属性。  
使用hasOwnProperty()方法可以判断属性是存在于实例中，还是存在于原型中。只有给定属性存在于实例中，才会返回true。  
in操作符会在通过对象能够访问给定属性时返回true，无论该属性存在于实例中还是原型中。  
同时使用hasOwnProperty()方法和in操作符，能够确定属性到底是存在于对象中，还是存在于原型中。
```
function Person () {
}
Person.prototype.name = "Alice";
Person.prototype.age = "22";
Person.prototype.showName = function(){
    alert(this.name);
};
var person1 = new Person();
var person2 = new Person();
person1.name = "Bruce";
alert(person1.name);//Bruce
alert(person1.hasOwnProperty("name"));//true
alert("name" in person1);//true
alert(person2.name);//Alice
delete person1.name;
alert(person1.hasOwnProperty("name"));//false
alert("name" in person1);//true
alert(person1.name);//Alice
```
原型模式更简单的语法：以一个包含所有属性和方法的对象字面量来创建原型对象。
```
function Person () {
}
Person.prototype = {
        name:"Alice",
        age : "22",
        showName: function(){
            alert(this.name);
    }
};
```
用对象字面量来创建原型对象的结果相同，只是constructor属性不再指向Person。这是由于这样已经完全重写了默认的prototype对象，因此constructor属性也就变成了新对象的constructor属性，指向Object构造函数但不指向Person函数。此时，instanceof操作符还能返回正确的结果但通过constructor已经无法确定对象的类型了。
```
var person = new Person();
alert(person instanceof Object);//true
alert(person instanceof Person);//true
alert(person.constructor == Object);//true
alert(person.constructor == Person);//false
```
如果constuctor的值很重要，可以特意将其设置回适当的值。
```
function Person () {
}
Person.prototype = {
    constructor:Person,
    name:"Alice",
    age : "22",
    showName: function(){
        alert(this.name);
    }
};
```
重写原型对象切断了现有原型与任何之前已经存在的对象实例之间的联系，对象实例引用的仍然是最初的原型。
```
function Person () {
}
var person = new Person();
Person.prototype = {
    constructor:Person,
    name:"Alice",
    age : "22",
    showName: function(){
        alert(this.name);
    }
};
person.showName();//报错：person.showName is not a function
```

#### 2.4 组合使用构造函数模式和原型模式
原型对象的问题：最大问题是由于共享属性导致的。原型中所有属性是被实例共享的，这对于函数很合适，对那些包含基本值的属性也还说得过去，因为可以通过在实例上添加同名属性，隐藏原型中的对应属性。然而，对于包含引用值的属性来说，问题就比较突出了，修改某个实例的引用类型的属性也会通过原型影响到其它实例的该属性。  
创建自定义类型的最常见方法是组合使用构造函数模式和原型模式，构造函数模式用于定义实例属性，原型模式用于定义方法和共享的属性。
```
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.friends = ["Bruce", "Cindy"];
}
Person.prototype = {
    constructor : Person,
    showName : function(){
        alert(this.name);
    }
};
var person1 = new Person("Alice",23);
var person2 = new Person("David",22);
person1.friends.push("Vincy");//包含引用值的属性friends
alert(person1.friends);//"Bruce", "Cindy","Vincy"
alert(person2.friends);//"Bruce","Cindy"
alert(person1.friends == person2.friends);//false
alert(person1.showName == person2.showName);//true
```

### 2.5 动态原型模式
动态原型模式把所有信息都封装在了构造函数中，而通过在构造函数中初始化原型，又保持了同时使用构造函数和原型的优点。  
可以通过检查某个应该存在的方法是否有效，来决定是否需要初始化原型。  
如：只在showName()方法不存在的情况下，才会将它添加到原型中，这段代码只会在初次调用构造函数时才会执行。
```
function Person(name,age) {
    this.name=name;
    this.age=age;
    if(typeof this.showName!="function"){
        Person.prototype.showName=function(){
            alert(this.name);
        }
    }
}
alert(person1.hasOwnProperty("name"));//true
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
