[<< 回到主页](http://suzy1993.github.io/misszy/)

## JavaScript 类数组对象转换为数组对象

### 1 JavaScript如何判断一个对象是否为数组？
使用typeof运算符不能判断一个对象是否为数组，因为typeof arr返回的是object而不是array。
#### 1.1 arr instanceof Array返回true

#### 1.2 arr.constructor == Array返回true
说明：使用instanceof和constructor判断数组类型的问题在于，它假定只有一个运行环境，如果网页中包含多个框架，那么实际上存在两个以上不同的全局执行环境，进而存在两个不同版本的Array构造函数，如果从一个框架向另一个框架传入一个数组，那么传入的数组与第二个框架中原生创建的数组分别具有各自不同的构造函数，也就是说，object.constructor == Array 会返回false。
原因：Array属于引用型数据，传递过程仅仅是引用地址的传递，每个页面的Array原生对象所引用的地址是不一样的，在子页面声明的Array所对应的构造函数，是子页面的Array对象，父页面进行判断时使用的Array并不等于子页面的Array。

#### 1.3 Array.isArray(arr)方法返回true
ES5新增了Array.isArray()方法，这个方法的目的是：最终确定一个值是否是数组，不管它是在哪个全局环境创建的。

#### 1.4 Object.prototype.toString.call(arr) === "[object Array]"返回true
这是最简单的判断一个对象是否为数组的方法。

### 2 类数组对象
1）拥有length属性，可以通过下标访问；
2）不具有数组所具有的方法。

### 3 为什么要将类数组对象转换为数组对象？
数组对象Array有很多方法：shift、unshift、splice、slice、concat、reverse、sort，ES6又新增了一些方法：forEach、isArray、indexOf、lastIndexOf、every、some、map、filter、reduce等。由于类数组不具有数组所具有的操作数组的方法，将类数组转换为数组之后就能调用这些强大的方法，方便快捷。

### 4 类数组对象转换为数组对象的方法
#### 4.1 Array.prototype.slice.call(arrayLike) 或Array.prototype.slice.call(arrayLike, 0) 或[].slice.call (arrayLike) 或 [].slice.call (arrayLike, 0)
```
var div1 = Array.prototype.slice.call(document.querySelectorAll('div'), 0);
var div2 = Array.prototype.slice.call(document.querySelectorAll('div'));
var div3 = [].prototype.slice.call(document.querySelectorAll('div'), 0);
var div4 = [].prototype.slice.call(document.querySelectorAll('div'));
```

#### 4.2 Array.from(arrayLike)
```
var divs = Array.from(document.querySelectorAll('div'));
```

#### 4.3 原生javascript转换
```
var length = arrayLike.length;
var arr = [];
for (var i = 0; i < length; i++) {
    arr.push(arrayLike[i]);
    return arr;
}
```

[<< 回到主页](http://suzy1993.github.io/misszy/)

