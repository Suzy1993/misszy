[<< 回到主页](http://suzy1993.github.io/misszy/)

## JavaScript arguments对象

js函数不介意定义多少参数，也不在乎传递进来多少参数，也就是说，即使定义的函数只接收2个参数，在调用时候也未必传递2个参数，因为js的函数参数在内部使用一个数组表示的，在函数体内可以通过arguments对象访问此参数数组。因此，js函数可以不显式地使用命名参数。  
当函数被调用时，传入的参数将保存在arguments类数组对象中，通过arguments可以访问所有该函数被调用时传递给它的参数列表。  
arguments是一个类数组对象，因为arguments可以通过方括号语法访问每一个元素，且拥有一个length属性，但它缺少所有的数组方法，因此并不是一个真正的数组。  
使用arguments可以实现一个求和函数：
```
function add() {
    var sum = 0;
    for (var i = 0, len = arguments.length; i < len; i++)
        sum += arguments[i];
    return sum;
}
```
虽然arguments的主要用途是保存函数参数，但这个对象还有一个callee属性，该属性是一个指针，指向拥有这个arguments对象的函数。  
使用arguments.callee属性可以实现一个阶乘函数：
```
function factorial(num) {
    if (num <= 1)
        return 1;
    else
        return num * arguments.callee(num - 1);
}
```
注意：  
在严格模式下，不能使用arguments.callee属性，也不能对arguments对象赋值，更不能用arguments对象跟踪参数的变化。  
可以使用命名函数表达式来达成同样的效果：
```
var factorial = (function func(num) {
    if (num <= 1)
        return 1;
    else
        return num * func(num - 1);
));
```
由于js函数没有签名（定义接受的参数的类型和数量），js函数没有重载，对于同名函数，后定义的函数会覆盖先定义的函数。当然，通过检查传入的参数的类型和数量并做出不同的反应，可以模仿方法的重载。

[<< 回到主页](http://suzy1993.github.io/misszy/)
