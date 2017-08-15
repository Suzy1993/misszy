[<< 回到主页](http://suzy1993.github.io/misszy/)

## jQuery.extend VS jQuery.fn.extend

jQuery是一个JavaScript类，如$("#input1")生成一个 jquery类的实例。
jQuery为开发插件提拱了两个方法：jQuery.fn.extend()和jQuery.extend()。

### 1 jQuery.extend()
#### 1.1 扩展jQuery类本身，为jQuery类添加类方法（静态方法）
```
jQuery.extend({
　　add: function(a, b) { alert(a + b); }
});
jQuery.add(10,20); //30
```

#### 1.2 jQuery.extend(object, object1, [objectN])用一个或多个其他对象来扩展一个对象，返回被扩展的对象
```
var obj = { name: 'Alice', age: 25, career: "teacher" };
var object = { name: 'Bruce', career: "doctor" };
jQuery.extend(obj, object); //obj = { name: 'Bruce', age: 25, career: "doctor" }
```

### 2 jQuery.fn.extend()
把对象挂载到 jQuery的prototype属性，来扩展一个新的jQuery实例方法，也就是通过这个extend添加的新方法，实例化的jQuery对象都能使用，因为它是挂载在jQuery.fn上的方法。
查看jQuery源码可发现，jQuery.fn = jQuery.prototype。jQuery.fn挂在原型上，由于对原型的修改会影响所有实例，因此fn上的方法会对每一个jQuery实例有效。
对jQuery.fn的扩展，就是为jQuery类添加成员函数，jQuery类的实例可以使用这个成员函数。
```
jQuery.fn.extend({
     clickFunc: function() {
           $(this).click(function(){
                  alert($(this).val());
            });
      }
});
$("#input1").clickFunc(); //输出文本框的文本
```

### 3 jQuery.extend()与 jQuery.fn.extend()的区别
1）jQuery.extend()是为jQuery类添加类方法（静态方法），需要通过jQuery类来调用（直接使用$.xxx调用）；

2）3.2 jQuery.fn.extend()是为jQuery类添加成员函数（实例方法），所有jQuery实例都可以直接调用（需要使用$().xxx调用）。

[<< 回到主页](http://suzy1993.github.io/misszy/)
