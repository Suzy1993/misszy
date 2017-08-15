[<< 回到主页](http://suzy1993.github.io/misszy/)

## JavaScript 闭包

### 1 与闭包有关的两个概念
#### 1.1 变量的作用域
不带有关键字var的变量会成为全局变量；
在函数中使用关键字var声明的变量是局部变量。
局部变量只有在函数内部才能访问到，在函数外面是访问不到的。但在函数内部可以通过作用域链一直向上搜索直到全局对象，也就是说，函数内部可以访问函数外部的变量。

#### 1.2 变量的生存周期
对于全局变量，其生存周期是永久的，除非主动销毁这个全局变量；
而对于在函数内用关键字var声明的局部变量，当退出函数时，这些局部变量会随着函数调用结束而被销毁。
```
var func = function() {
    var i = 1;
    alert(i); // 输出：1
};
alert(i); // 报错：i is not defind.
```
例外情况：闭包
```
var func = function() {
    var i = 1;
    return function() {
        alert(i);
        i++;
    }
};
var f1 = func();
f1(); // 输出：1
f1(); // 输出：2
var f2 = func();
f2(); // 输出：1
f2(); // 输出：2
```

### 2 从闭包的一个经典应用谈起
```
<div>0</div>
<div>1</div>
<div>2</div>
<div>3</div>
<div>4</div>
<script>
    var divs = document.getElementsByTagName("div");
    for (var i = 0; i < divs.length; i++) {
        divs[i].onclick = function() {
            alert(i);
        };
    }
</script>
```
问题：无论单击哪个div，都会弹出5。
原因：onclick事件是异步触发的，当事件被触发时，for循环早已结束，此时变量i的值早已经是5。
解决：在闭包的帮助下，把每次循环的i值都封闭起来。当事件函数顺着作用域链从内到外查找变量i时，会先找到被封闭在闭包环境的i，单击div时，会分别输出0,1,2,3,4。
```
<div>0</div>
<div>1</div>
<div>2</div>
<div>3</div>
<div>4</div>
<script>
var divs = document.getElementsByTagName("div");
for (var i = 0; i < divs.length; i++) {
    divs[i].onclick = (function(num) {
        return function() {
            alert(num);
        };
    })(i);
}
</script>
```
类似实例：闭包直接赋给数组
```
function createFunctions() {
    var result = new Array();
    for (var i = 0; i < 10; i++){
        result[i] = function(){
            return i;
        };
    }
    return result;
}
for (var i = 0; i < 10; i++)
    alert(createFunctions()[i]());
```
结果：result的每个元素都返回10。
说明：闭包的作用域链有明显的副作用——闭包总是获得外部函数变量的最终值。上面代码中，外部函数产生一个函数数组result并返回。函数数组中的每个元素都是一个函数，每个函数都返回 i变量。似乎每个函数应该返回每次循环的i值，即依次返回0到9，但事实是，每个函数的返回结果都是10。这是因为每个内部函数返回的是变量i，而不是i在某个时刻的特定值，而i的作用域是整个外部函数，当外部函数执行完成后，i的值是10。
解决：在每个内部函数的内部，再产生一个匿名函数并返回。
```
function createFunctions() {
    var result = new Array();
    for (var i = 0; i < 10; i++) {
        result[i] = (function(num) {
            return function() {
                return num;
            };
        })(i);
    }
    return result;
}
for (var i = 0; i < 10; i++)
    alert(createFunctions()[i]());
```
结果：result依次返回0到9。
说明：(i)使得该层匿名函数立即执行。

### 3 闭包
有时候需要得到函数内的局部变量。如何从外部读取局部变量？那就是在函数的内部，再定义一个函数。
闭包是指有权访问另一个函数作用域中变量的函数，创建闭包的最常见的方式就是在一个函数内创建另一个函数，通过另一个函数访问这个函数的局部变量，利用闭包可以突破作用链域，将函数内部的变量和方法传递到外部。
#### 3.1 闭包的原理
* 后台执行环境中，闭包的作用域链包含着自己的作用域、函数的作用域和全局作用域。
* 通常，函数的作用域和变量会在函数执行结束后销毁。
* 但是，当函数返回一个闭包时，这个函数的作用域将会一直在内存中保存到闭包不存在为止。

#### 3.2 闭包的特性
* 函数内再嵌套函数。
* 内部函数可以引用外层的参数和变量。
* 参数和变量不会被垃圾回收机制回收。

#### 3.3 闭包的用途
#### 3.3.1 读取函数内部的变量
```
function f1(){
    var n = 999;
    function f2(){
        alert(n);//999
    }
}
```
在上面的代码中，函数f2就被包括在函数f1内部，这时f1内部的所有局部变量，对f2都是可见的。但是反过来就不行，f2内部的局部变量，对f1就是不可见的。既然f2可以读取f1中的局部变量，那么只要把f2作为返回值，就可以在f1外部读取它的内部变量了。
```
function f1(){
    var n = 999;
    function f2(){
        alert(n);
    }
    return f2;
}
var result=f1();
result();//999
```
闭包就是能够读取其他函数内部变量的函数。由于在JavaScript语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成"定义在一个函数内部的函数"。所以，在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。
```
function f1(){
    var n = 999;
    nAdd = function(){n += 1}
    function f2(){
        alert(n);
    }
    return f2;
}
var result=f1();
result();//999
nAdd();
result();//1000
```
result实际上就是闭包f2函数。它一共运行了两次，第一次的值是999，第二次的值是1000。这证明了，函数f1中的局部变量n一直保存在内存中，并没有在f1调用后被自动清除。原因就在于f1是f2的父函数，而f2被赋给了一个全局变量，这导致f2始终在内存中，而f2的存在依赖于f1，因此f1也始终在内存中，不会在调用结束后，被垃圾回收机制回收。

#### 3.3.2 让函数内部的变量的值始终保持在内存中（延长局部变量的寿命）
```
var print, add, set;
function closure() {
    var number = 8;
    print = function() {
        alert(number);
    }
    add = function() {
        number++;
    }
    set = function(x) {
        number = x;
    }
}
closure();//创建一个闭包
add();
print();//9
set(0);
print();//0
var oldClosure = print;
closure();//创建一个新的闭包
print();//8
oldClosure();//0
```
使用闭包的注意点：由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。解决方法是，在退出函数之前，将不使用的局部变量全部删除。也就是说，闭包会引用外部函数作用域，会占用更多的内存，过度使用闭包，会导致性能问题。所以，仅当必要时才使用闭包。对产生闭包的函数，使用后应该解除引用。

#### 3.3.3 自执行函数+闭包减少全局变量污染（封装私有变量）
```
var person = (function() {
    var_name = "Alice";
    var _id = 16;
    return {
        getUserInfo: function() {
            return _name + ": " + _id;
        }
    }
})();
```
使用下划线来约定私有变量_name和_age，它们被封装在闭包产生的作用域中，外部是访问不到这两个变量的，这就避免了对全局的命令污染。

#### 3.4 闭包的缺点
* 需要维护额外的作用域
* 过渡使用闭包会占用大量内存

[<< 回到主页](http://suzy1993.github.io/misszy/)
