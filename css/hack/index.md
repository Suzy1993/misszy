[<< 回到主页](http://suzy1993.github.io/misszy/)

## CSS hack

### 1 CSS hack
CSS hack是对付IE的利器。
不同的浏览器，甚至同一浏览器的不同版本对CSS的解析认识不一样，可能会导致生成的页面效果不一致。
有了CSS hack，就可以针对不同的浏览器来写不同的CSS。

### 2 常用的CSS hack的三种方式
#### 2.1 CSS内部hack（最常用）
<table>
  <tr><th></th><th>IE6</th><th>IE7</th><th>IE8</th><th>IE9</th><th>IE10</th><th>现代浏览器</th></tr>
  <tr><td>*</td><td>YES</td><td>YES</td><td></td><td></td><td></td><td></td></tr>
  <tr><td>+</td><td></td><td>YES</td><td></td><td></td><td></td><td></td></tr>
  <tr><td>-</td><td>YES</td><td></td><td></td><td></td><td></td><td></td></tr>
  <tr><td>!important</td><td></td><td>YES</td><td>YES</td><td>YES</td><td>YES</td><td>YES</td></tr>
  <tr><td>\9</td><td>YES</td><td>YES</td><td>YES</td><td>YES</td><td>YES</td><td></td></tr>
  <tr><td>\0</td><td></td><td></td><td>YES</td><td>YES</td><td>YES</td><td></td></tr>
  <tr><td>\9\0</td><td></td><td></td><td></td><td>YES</td><td>YES</td><td></td></tr>
</table>
eg：
```
div {
    background-color:#f1ee18; // 所有识别
    background-color:#00deff\9; // IE6、7、8识别
    +background-color:#a200ff; // IE6、7识别
    _background-color:#1e0bd1; // IE6识别
}
```

#### 2.2 选择器hack
选择器hack主要是针对IE浏览器。
<table>
  <tr><th></th><th>IE6</th><th>IE7</th><th>IE8</th><th>IE9</th><th>IE10</th><th>现代浏览器</th></tr>
  <tr><td>*</td><td>YES</td><td></td><td></td><td></td><td></td><td></td></tr>
  <tr><td>+</td><td></td><td>YES</td><td></td><td></td><td></td><td></td></tr>
  <tr><td>-</td><td></td><td></td><td></td><td>YES</td><td></td><td></td></tr>
</table>
eg：
```
:root div {
    background-color:green;
}
```

#### 2.3 HTML头部引用
HTML 头部引用类似于程序语句，只能使用在HTML文件里，而不能在CSS文件中使用，并且只有在IE浏览器下才能执行，这个代码在非IE浏览下非单不是执行该条件下的定义，而是当做注释视而不见。
* lte：Less than or equal to的简写，小于或等于。
* lt：Less than的简写，小于。
* gte：Greater than or equal to的简写，大于或等于。
* gt：Greater than的简写，大于。
* !：不等于。
eg：
```
<!-- 默认先调用css.css样式表 -->
<link rel="stylesheet" type="text/css" href="css.css" />
<!--[if IE 7]>
<!-- 如果IE浏览器版是7,调用ie7.css样式表 -->
<link rel="stylesheet" type="text/css" href="ie7.css" />
<![endif]-->
<!--[if lte IE 6]>
<!-- 如果IE浏览器版本小于等于6,调用ie.css样式表 -->
<link rel="stylesheet" type="text/css" href="ie.css" />
<![endif]-->
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
