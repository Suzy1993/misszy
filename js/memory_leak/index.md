[<< 回到主页](http://suzy1993.github.io/misszy/)

## JavaScript 内存泄漏

### 1 内存泄漏的实例
```
function getId() {
    var div = document.getElementById("div1");
    div.onclick = function() {
        alert(div.id);
    }
}
```
以上代码创建了一个作为div事件处理程序的闭包，而这个闭包又创建了一个循环引用。由于匿名函数保存了一个对getId()的活动对象的引用，因此会导致无法减少div的引用数。只要匿名函数存在，div的引用次数至少为1，它所占用的内存永远不会被回收。
解决方法：
```
function getId() {
    var div = document.getElementById("div1");
    var id = div.id;
    div.onclick = function() {
        alert(id);
    }
    div = null;
}
```
以上代码通过把div.id的一个副本保存在一个变量中，并且在闭包中引用该变量消除了循环引用。但仅仅做到这一步，还不能解决内存泄漏的问题，因为闭包会引用包含函数的整个活动对象，而其中包含着div，即使闭包不直接引用div，包含函数的活动对象中也仍然会保存一个引用。因此，有必要把div变量设为null。这样能够解除对DOM对象的引用，顺利地减少其引用数，确保正常的对其内存进行回收。

### 2 内存泄漏的几种情况
1）当页面中元素被移除或替换时，若元素绑定的事件仍没被移除，在IE中不会作出恰当处理，导致内存泄露。
```
var btn = document.getElementById("btn");
btn.onclick = function() {
    ...
}
解决方法：
1-1）手动移除元素绑定的事件
```
var btn = document.getElementById("btn");
btn.onclick = function() {
    btn.onclick = null;
    ...
}
```
1-2）采用事件委托
```
document.onclick = function(event) {
    event = event || window.event;
    if (event.target.id == "myBtn") {
        ...
    }
}
```
2）对于对象而言，只要没有其他对象引用对象a、b，即它们只是相互之间的引用，那么仍然会被垃圾收集系统识别并处理。但是，在 IE中，垃圾收集系统不会发现它们之间的循环引用关系而释放它们，最终它们将被保留在内存中，直到浏览器关闭，这导致了内存泄漏。
```
var a = document.getElementById("a");
var b = document.getElementById("b");
a.name = b;
b.name = a;
```
3）一个DOM结点的事件处理函数内引用了该DOM对象，会形成闭包，产生一个循环引用，DOM对象在闭包释放之前不会被释放；而闭包作为DOM对象的事件处理函数而存在，所以在DOM对象释放前闭包也不会释放，由于这个循环引用的存在，DOM对象和闭包都不会被释放，造成内存泄露。
```
var btn = document.getElementById(btn);
btn.addEventListener('click', function() {
    alert('You clicked ' + btn.value);
});
```
4）在删除属性时，由于已经删除的属性引用依然存在，会造成内存泄露。所以在销毁属性的时候，要遍历属性中属性，依次删除。
```
a = {p: {x: 1}};
b = a.p;
delete a.p; //b.x仍为1
```
5）在定义一个基本类型的变量时，它没有其对应的引用类型的属性，所以当访问该属性时，js引擎会自动创建一个对应的临时对象封装该基本类型变量，这个自动类型装箱转换的对象在IE系列中会造成内存泄漏。
```
var s = "Hello";
alert(s.length);
```
解决方法：
对基本类型变量做转换，通过new方法将其转换为对应的引用类型后再访问属性。
```
var s = "Hello";
alert(new String(s).length);
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
