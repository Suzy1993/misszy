[<< 回到主页](http://suzy1993.github.io/misszy/)

## JavaScript 对象的浅拷贝和深拷贝

### 1 浅拷贝
仅仅复制对象的引用，而不是对象本身。
```
var person = {
    name: 'Alice',
    friends: ['Bruce', 'Cindy']
}
var student = {
    id: 30
}
student = simpleClone(person, student);
student.friends.push('David');
alert(person.friends);
function simpleClone(oldObj, newObj) {
    var newObj = newObj || {};
    for (var i in oldObj)
        newObj[i] = oldObj[i];
    return newObj;
}
```
给子对象的数组类型的属性添加一个新值，父对象的该属性值也被篡改。

### 2 深拷贝
把复制的对象所引用的全部对象都复制一遍，能够实现真正意义上的数组和对象的拷贝。  
浅拷贝的问题：如果父对象的属性值为一个数组或另一个对象，那么实际上子对象获得的只是一个内存地址，而不是对父对象的真正拷贝，因此存在父对象被篡改的可能。  
解决方法：使用深拷贝。
```
var person = {
    name: 'Alice',
    friends: ['Bruce', 'Cindy']
}
var student = {
    id: 30
}
student = deepClone(person, student);
student.friends.push('David');
alert(person.friends); // 'Bruce', 'Cindy'
function deepClone(oldObj, newObj) {
    var newObj = newObj || {};
    newObj = JSON.parse(JSON.stringify(oldObj));
    return newObj;
}
```

### 3 实现深拷贝的方法
#### 3.1 方法1：使用JSON.parse()方法
```
function deepClone(oldObj, newObj) {
    var newObj = newObj || {};
    newObj = JSON.parse(JSON.stringify(oldObj));
    return newObj;
}
```
#### 3.1.1 优点
* 简单易用

#### 3.1.2 缺点
* 会抛弃对象的constructor，即，深拷贝后，不管该对象原来的构造函数是什么，在深拷贝之后都会变成Object。
* 能正确处理的对象只有 Number, String, Boolean, Array，即那些能够被JSON直接表示的数据结构，RegExp对象等无法通过这种方式深拷贝。

#### 3.2 方法2：递归拷贝
```
function deepClone(oldObj, newObj) {
    var newObj = newObj || {};
    for (var i in oldObj) {
        if (typeof oldObj[i] === 'object') {
            newObj[i] = (oldObj[i].constructor === Array) ? [] : {};
            arguments.callee(oldObj[i], newObj[i]);
        }
        else
            newObj[i] = oldObj[i];
    }
    return newObj;
}
```
问题：当遇到两个互相引用的对象，会出现死循环的情况。  
解决方法：在遍历时判断两个对象是否相互引用（如oldObj.property === newObj），如果是则退出循环。
```
function deepClone(oldObj, newObj) {
    var newObj = newObj || {};
    for (var i in oldObj) {
        var prop = oldObj[i];
        if (prop === newObj)
            continue;
        if (typeof prop === 'object') {
            newObj[i] = (prop.constructor === Array) ? [] : {};
            arguments.callee(prop, newObj[i]);
        }
        else
            newObj[i] = prop;
    }
    return newObj;
}
```

#### 3.3 方法3：使用Object.create()方法
```
function deepClone(oldObj, newObj) {
    var newObj = newObj || {};
    for (var i in oldObj) {
        var prop = oldObj[i];
        if (prop === newObj)
            continue;
        if (typeof prop === 'object')
            newObj[i] = (prop.constructor === Array) ? [] : Object.create(prop);
        else
            newObj[i] = prop;
    }
    return newObj;
}
```

#### 3.4 方法4：使用jQuery.extend()和jquery.fn.extend()

[<< 回到主页](http://suzy1993.github.io/misszy/)
