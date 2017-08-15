[<< 回到主页](http://suzy1993.github.io/misszy/)

## JavaScript 循环中setTimeout的执行顺序问题

期望：开始输出一个0，然后每隔一秒依次输出1,2,3,4
```
for (var i = 0; i < 5; i++) {
    setTimeout(function() {
        console.log(i);
    }, 1000 * i);
}
```
结果：输出5。
原因：setTimeout 使函数延迟1s执行，而for循环执行完成还不到0.1秒，到执行函数的时候，其实 i 已经变成5了，因此console.log(i)输出5。

解决方法一：使用let块作用域
```
for (let i = 0; i < 5; i++) {
        setTimeout(function() {
            console.log(i);
    }, 1000 * i);
}
```

解决方法二：加个闭包
```
for (var i = 0; i < 5; i++) {
    (function(i) {
        setTimeout(function() {
            console.log(i);
        }, 1000 * i);
    })(i);
}
```
结果：开始输出一个0，然后每隔一秒依次输出1,2,3,4。

失败方法：
```
for (var i = 0; i < 5; i++) {
    (function() {
        setTimeout(function() {
            console.log(i);
        }, 1000 * i);
    })();
}
```
结果：输出 5。
原因：去掉函数的参数i，则函数内部没有对i保持引用。

解决方法三：
```
for (var i = 0; i < 5; i++) {
    setTimeout((function(i) {
        console.log(i);
    })(i), i * 1000);
}
```
结果：立马输出0-4。
原因：setTimeout可以接受函数或者字符串作为参数，而给setTimeout传递了一个立即执行函数，该立即执行函数是undefined ，也就是说等价于setTimeout(undefined, ...)。立即执行函数会立即执行。

[<< 回到主页](http://suzy1993.github.io/misszy/)
