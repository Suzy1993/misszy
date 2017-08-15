[<< 回到主页](http://suzy1993.github.io/misszy/)

## JavaScript this对象

与别的语言不同，JavaScript的this总是指向一个对象，而具体指向哪个对象是在运行时基于函数的执行环境动态绑定的，而非函数被声明时的环境。
this是执行上下文的一个属性，其值在进入上下文时确定，且在上下文运行期间永久不变。
this 是动态绑定的，即在运行期绑定。
this可以是全局对象，当前对象或任意对象，取决于函数的调用方式。函数的调用方式有以下几种：作为普通函数调用，作为对象方法调用，作为构造函数调用，使用call()和apply()调用。

### 1 函数的调用方式
#### 1.1 作为普通函数调用
当函数不作为对象的属性被调用，即直接被调用时，this会被绑定到全局对象。在浏览器的javascript里，该全局对象是window对象。
```
var name = "Alice";
function getName (name) {
    return this.name;
}
alert(getName()); // 输出：Alice
var name = "Alice";
var obj = {
    name: 'Bruce',
    getName: function(name) {
        return this.name;
    }
};
var getName = obj.getName();
alert(getName()); // 输出：Alice
```
以上两个实例中，this都被绑定到了全局对象。
```
var firstname = "A";
var lastname = "B";
var person = {
    firstname : "Alice",
    lastname : "Bruce",
    setName : function(firstname, lastname) {
        var setFirstName = function(firstname) {
            this.firstname = firstname;
        };
        var setLastName = function(lastname) {
            this.lastname = lastname;
        };
        setFirstName(firstname);
        setLastName(lastname);
    }
};
person.setName("Cindy", "David");
alert(firstname);//Cindy
alert(lastname);//David
alert(person.firstname);//Alice
alert(person.lastname);//Bruce
```
问题：在函数内部定义的函数，this也可能会指向全局，而希望的是内部函数的this绑定到外部函数对应的对象上。
原因：内部函数永远不可能直接访问外部函数中的this变量。
解决：在外部函数体中，要进入内部函数时，将this保存到一个变量中，再运用该变量。
```
var firstname = "A";
var lastname = "B";
var person = {
    firstname: "Alice",
    lastname: "Bruce",
    setName: function(firstname, lastname) {
        var that = this;
        var setFirstName = function(firstname) {
            that.firstname= firstname;
        };
        var setLastName = function(lastname) {
            that.lastname= lastname;
        };
        setFirstName(firstname);
        setLastName(lastname);
    }
};
person.setName("Cindy", "David");
alert(firstname);//A
alert(lastname);//B
alert(person.firstname);//Cindy
alert(person.lastname);//David
```

#### 1.2 作为对象方法调用
当函数作为对象方法调用时，this会被绑定到当前对象。
```
var firstname = "A";
var lastname = "B";
var person = {
    firstname : "Alice",
    lastname : "Bruce",
    setName : function(firstname, lastname) {
        this.firstname = this.firstname + firstname;
        this.lastname = this.lastname + lastname;
    }
};
person.setName("Cindy", "David");
alert(firstname);//A
alert(lastname);//B
alert(person.firstname);//AliceCindy
alert(person.lastname);//BruceDavid
```
this被绑定到了当前对象，即person对象。

#### 1.3 作为构造函数调用
JavaScript中没有类，但可以从构造器中创建对象，同时也提供了new运算符，使得构造器看起来更像一个类。
利用构造函数创建新对象时，可以将this来指向新创建的对象，避免函数中的this指向全局。
```
var name = "Alice";
function Person(name) {
    this.name = name;
}
var person = new Person("Bruce");
alert(name);//Alice
alert(person.name);//Bruce
```
利用构造函数创建新对象person，this指向了person。
用new调用构造器时。还要注意一个问题，若构造器显式返回了一个object类型的对象，构造器返回的结果将是这个对象，而不是this。
```
function Person() {
    this.name = "Alice"
    return {
        name: "Bruce"
    }
}
var person = new Person();
alert(person.name);//Bruce
```

#### 1.4 call()和apply()调用
call()和apply()切换函数执行的上下文环境，即this绑定的对象；this指向的是apply()和call()中的第一个参数。
```
function Person(name) {
    this.name = name;
    this.setName = function(name) {
        this.name = name;
    }
}
var person1 = new Person("Alice");
var person2 = {"name": "Bruce"};
alert("person1: " + person1.name); // person1: Alice
person1.setName("David");
alert("person1: " + person1.name); // person1: David
alert("person2: " + person2.name); // person2: Bruce
person1.setName.apply(person2, ["Cindy"]);
alert("person2: " + person2.name); // person2: Cindy
```
apply()将person1的方法应用到person2上，this也被绑定到person2上。

### 2 this优先级
* 函数是否在new中调用，是的话this绑定到新创建的对象。
* 函数是否通过call、apply调用，是的话this绑定到指定对象。
* 函数是否在某个上下文对象中调用，是的话this绑定到该上下文对象。
* 若都不是，使用默认绑定，若在严格模式下，绑定到undefined，否则绑定到全局对象。

### 3 this丢失的问题
eg1：
```
var person = {
    name: "Alice",
    getName: function() {
        return this.name;
    }
};
alert(person.getName()); // Alice
var getName = person.getName;
alert(getName()); // undefined
```
当调用person.getName()时，getName()方法是作为person对象的属性被调用的，因此this指向person对象；
当用另一个变量getName来引用person.getName，并调用getName()时，是作为普通函数被调用，因此this指向全局window。
eg2：
```
<div id="div">正确的方式</div>
<script>
    var getId = function(id) {
        return document.getElementById(id);
    };
    alert(getId('div').innerText);
</script>
<div id="div">错误的方式</div>
<script>
    var getId = document.getElementById;
    alert(getId('div').innerText); // 抛出异常
</script>
```
问题：第一段调用正常，但第二段调用会抛出异常。
原因：许多引擎的document.getElementById()方法的内部实现中需要用到this，this本来期望指向的是document，当第一段代码在getElementById()方法作为document对象的属性被调用时，方法内部的this确实是指向document的，而第二段代码中，用getId来引用document.getElementById之后，再调用getId，此时就成了普通函数调用，函数内部的this指向了window，而不是原来的document。
解决：利用apply把document当作this传入getId函数，修正this。
```
 <div id="div">修正的方式</div>
<script>
    document.getElementById = (function(func) {
        return function() {
            return func.apply(document, arguments);
        };
    })(document.getElementById);
    var getId = document.getElementById;
    alert(getId('div').innerText); // 抛出异常
</script>
 ```

[<< 回到主页](http://suzy1993.github.io/misszy/)
