[<< 回到主页](http://suzy1993.github.io/misszy/)

## JavaScript 执行环境与作用域链

### 1 执行环境
* 每个执行环境都有一个与之关联的变量对象，环境中定义的所有变量和函数都保存在这个对象中。
* 执行环境包括全局执行环境和函数执行环境。
* 全局执行环境是最外围的一个执行环境，在浏览器中，全局执行环境被认为是是window对象，所有全局变量和属性都是作为window对象的属性和方法创建的。
* 函数执行环境是指函数的执行环境，当执行流进入一个函数时，函数的环境会被推入一个环境栈中，在函数执行之后，栈将其环境弹出，将控制权返回到之前的执行环境。

### 2 作用域链
* 当代码在一个环境中执行时，会创建变量对象的一个作用域链。
* 作用域链的用途：保证对执行环境有权访问的所有变量和函数的有序访问。
* 作用域链的前端，始终是当前执行的代码所在环境的变量对象，若此环境是函数，则将其活动对象作为变量对象。活动对象最开始时只包含一个变量，即arguments对象（该对象在全局环境中是不存在的），作用域链的下一个对象来自包含环境，再下一个变量则来自下一个包含环境，这样一直延续到全局执行环境。全局执行环境的变量对象始终是作用域链的最后一个对象。
* 每个环境都可以向上搜索作用域链，以查询变量和函数名，终点就是搜索到全局执行环境，但是任何环境不能通过向下搜索作用域链而进入另一个执行环境。内部环境可以通过作用域链访问所有的外部环境，但外部环境不能访问内部环境的任何变量和函数。

### 3 延长作用域链
虽然执行环境只有两种：全局执行环境和函数执行环境，但是可以有方法延长作用域链，因为有些语句可以在作用域链的前端临时增加一个变量对象，该变量对象会在代码执行后被移除。  

当执行流进入下列语句时，作用域链会延长：
#### 3.1 try-catch语句的catch块
catch语句会创建一个新的变量对象，其中包含的是被抛出的错误对象的声明，该变量对象只在catch块内部有效，在catch块外部无法访问到。

#### 3.2 with语句
with语句会将指定的对象添加到作用域链中。

eg1：
```
function setUrl() {
    var parameter="?name=Alice";
    var url = href + parameter;
    return url;
}
var result = setUrl();
alert(result); // 报错：href is no defined
```

eg2：
```
function setUrl() {
    var parameter="?name=Alice";
    with(location) {
        var url = href + parameter;
    }
    return url;
}
var result = setUrl();
alert(result); // http://localhost/text.html?name=Alice
```
with语句接收的是location对象，因此其变量对象中包含了location对象的所有属性和方法，location对象被添加到了作用域链的前端。

eg3：
```
var obj = {href : "http://www.baidu.com"};
var href = "http://www.sina.cn";
function setUrl() {
    var parameter="?name=Alice";
    with(obj) {
        href = "http://www.google.cn";
        var url = href + parameter;
    }
    return url;
}
var result = setUrl();
alert(result); // http://www.google.cn?name=Alice
alert(href); // http://www.sina.cn
```
with语句中并没有更改变量href的值，而是更改了obj对象的 href 属性。  
也就是说，with中首先查找的是相关对象的属性，如果没有，才改变变量的值。

eg4：
```
var obj = {};
var href = "http://www.sina.cn";
function setUrl() {
    var parameter="?name=Alice";
    with(obj) {
        href = "http://www.google.cn";
        var url = href + parameter;
    }
    return url;
}
var result = setUrl();
alert(result); // http://www.google.cn?name=Alice
alert(href); // http://www.google.cn
```
去掉obj对象的 href 属性后，才更改变量href的值。

[<< 回到主页](http://suzy1993.github.io/misszy/)
