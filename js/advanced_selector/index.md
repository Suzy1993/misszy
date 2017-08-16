[<< 回到主页](http://suzy1993.github.io/misszy/)

## JavaScript 高级选择器querySelector & querySelectorAll

### 1 querySelector和querySelectorAll
querySelector和querySelectorAll根据CSS选择器规范，便捷定位文档中指定元素，主流浏览器均支持：包括IE8+、Firefox、Chrome、Safari、Opera。  
querySelector返回的是一个对象，querySelectorAll返回的一个集合(NodeList)。

### 2 querySelector系列方法与getElementBy系列方法的区别
#### 2.1 querySelector系列属于W3C中的Selectors API规范，而getElementBy系列则属于W3C 的DOM规范 [2]。

#### 2.2 querySelector系列已被IE 8+、FF 3.5+、Safari 3.1+、Chrome 和 Opera 10+良好支持，
而getElementBy系列，以最迟添加到规范中的getElementsByClassName 为例，IE 9+、FF 3 +、Safari 3.1+、Chrome 和 Opera 9+ 都已经支持。

#### 2.3 querySelector系列接收的参数是一个CSS选择符，而getElementBy系列接收的参数只能是单一的className、tagName和name。
```
var ele = document.querySelectorAll('#list .lis');
```
注意：querySelector系列所接收的参数是必须严格符合CSS选择符规范的，如不能以数字为开头等。
```
var ele = document.querySelectorAll('#3d'); // 报错：'#3d' is not a valid selector.
```

#### 2.4 querySelector系列返回的是一个静态的Node List，而getElementBy系列的返回的是一个动态的Node List。
NodeList对象本质上是一个动态的Node集合，只是规范中对querySelector系列有明确要求，规定其必须返回一个静态的NodeList对象。  
注意：querySelectorAll 返回的虽然是 NodeList ，但是实际上是元素集合，并且是静态的。
```
console.log(document.querySelectorAll('div').toString()); // 输出：[object NodeList]
console.log(document.querySelectorAll('div')[0].toString()); // 输出：[object HTMLDivElement]
console.log(document.getElementsByTagName('div').toString()); // 输出：[object HTMLCollection]
console.log(document.getElementsByTagName('div')[0].toString()); // 输出：[object HTMLDivElement]
```
eg1：lis是一个静态的Node List，是一个li集合的快照，对文档的任何操作都不会对其产生影响。
```
var ul = document.querySelectorAll('ul')[0],
lis = ul.querySelectorAll("li");
for(var i = 0; i < lis.length ; i++)
    ul.appendChild(document.createElement("li"));
```

eg2：lis是一个动态的Node List，每次调用lis都会重新对文档进行查询，导致无限循环的问题。
```
var ul = document.getElementsByTagName('ul')[0],
lis = ul.getElementsByTagName("li")
for(var i = 0; i < lis.length ; i++)
    ul.appendChild(document.createElement("li"));
```

### 3 HTMLCollection和NodeList
#### 3.1 相同点
* 动态的元素集合，每次访问都需要重新对文档进行查询。

#### 3.2 不同点
* HTMLCollection 属于Document Object Model HTML 规范，而NodeList属于Document Object Model Core规范。
* NodeList是节点集合，包含文档中的所有节点，如Element、Text和Comment等，而HTMLCollection 是元素集合，只会包含文档中的Element节点。

[<< 回到主页](http://suzy1993.github.io/misszy/)
