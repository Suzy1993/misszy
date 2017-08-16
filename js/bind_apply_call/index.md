[<< 回到主页](http://suzy1993.github.io/misszy/)

## JavaScript bind() & apply() & call()

### 1 call()和apply()
在JavaScript中，call()和apply()是为了改变某个函数运行时的上下文而存在的，也就是为了改变函数体内部this的指向。  
当一个对象没有某个方法，而其它对象有该方法时，可以借助call()或apply()调用其它对象的该方法。  
call()和apply()的作用完全一样，只是接受参数的方式不一样：call()把参数按顺序传递，而apply()把参数放在数组里传递。  
eg1：
```
var teacher = {
    name: "Alice",
    sayHello: function(arg1, arg2) {
        alert("Hello, " + this.name + '. No：' + arg1 + '. Age：' + arg2);
    }
};
teacher.sayHello(16, 23); // 输出：Hello, Alice. No：16. Age：23
var student = {
    name: "Bruce"
};
teacher.sayHello.call(student, 30, 24); // 输出：Hello, Bruce. No：30. Age：24
teacher.sayHello.apply(student, [30, 24]); // 输出：Hello, Bruce. No：30. Age：24
```
对象student没有sayHello()方法，而teacher对象有sayHello()方法，不希望重新定义sayHello()方法，因此借助call()或apply()调用teacher对象的sayHello()方法。

eg2：数组连接
```
var arr1 = ["Alice", 23, true];
var arr2 = ["Bruce", 30, false];
Array.prototype.push.apply(arr1, arr2);
alert(arr1); // arr1 = ["Alice", 23, true, "Bruce", 30, false] */
```

eg3：获取数组的最大值和最小值
```
var nums= [5, 1, 8, 6, 3];
alert(Math.max.apply(Math, nums)); // 输出：8
alert(Math.max.call(Math, 5, 1, 8, 6, 3)); // 输出：8
```

eg4：
```
function log(content) {
    console.log(content);
}
log("Alice"); //控制台输出：Alice
log("Alice", "Bruce"); //控制台输出：Alice
function log() {
    console.log.apply(console, arguments);
}
log("Alice"); //控制台输出：Alice
log("Alice", "Bruce"); //控制台输出：Alice Bruce
```

### 2 bind()
bind()方法与apply()和call()很相似，也是可以改变函数体内this的指向。  
bind()方法会创建一个新函数，称为绑定函数，当调用这个绑定函数时，绑定函数会以创建它时传入 bind()方法的第一个参数作为 this，传入 bind() 方法的第二个及以后的参数按照顺序作为原函数的参数来调用原函数。
```
var first = {
    "x" : 10
};
function add(y, z) {
    alert(this.x + y + z);
}
var func = add.bind(first, 20, 30); //输出：60
func();
```
连续bind()多次：
```
var first = {
    "x" : 10
};
var second = {
    "x" : 1
};
function add(y, z) {
    alert(this.x + y + z);
}
var func = add.bind(first, 20, 30).bind(second, 2, 3); //输出：60
func();
```
输出不是6，而是60。可见，在javascript中，多次bind()是无效的。

### 3 bind()、apply()、call()的异同
#### 3.1 相同点
* 三者都是用来改变函数的this对象的指向的，即改变上下文环境。
* 三者第一个参数都是this要指向的对象。
* 三者都可以利用后续参数传参。

#### 3.2 不同点
bind() 是生成一个新函数，不立即调用，需要的时候再调用；apply()和call()则是立即调用函数。  
当希望改变上下文环境后不立即执行，而是回调执行函数时，使用bind()方法，否则使用apply()或call()方法。    
使用bind()方法，若要立即调用，需要再加上一对圆括号：
```
var first = {
    "x" : 10
};
function add(y, z) {
    alert(this.x + y + z);
}
add.bind(first, 20, 30)(); //输出：60
```
使用bind()方法，要调用时使用函数名进行调用：
```
var first = {
    "x" : 10
};
function add(y, z) {
    alert(this.x + y + z);
}
var func = add.bind(first, 20, 30); //输出：60
func();
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
