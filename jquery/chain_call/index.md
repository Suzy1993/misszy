[<< 回到主页](http://suzy1993.github.io/misszy/)

## jQuery 链式调用

### 1 链式调用
实现链式的基本条件就是要实例对象先创建好，调用自己的方法。
链式调用是通过return this的形式来实现的。通过对象上的方法最后加上return this，把对象再返回回来，对象就可以继续调用方法，实现链式操作了。
```
Obj().init().setFlag();
```
分解：
```
obj = Obj();
obj.init();
obj.setFlag();
```
如果需要链式的处理，只需要在方法内部返回当前的这个实例对象this就可以了，因为返回当前实例的this，就又可以访问自己的原型了。
```
Obj.prototype = {
  init: function() {
    ...
    return this;
  },
  setFlag: function() {
    ...
    return this;
  }
}
```

### 2 链式调用的好处
节省代码量，代码看起来更优雅。

### 3 链式调用的问题
所有对象的方法返回的都是对象本身，也就是说没有返回值，所以这种方法不一定在任何环境下都适合。

[<< 回到主页](http://suzy1993.github.io/misszy/)
