[<< 回到主页](http://suzy1993.github.io/misszy/)

## $(document).ready VS window.onload

### 1 文档加载完成的两种事件
#### 1.1 ready
表示文档结构已经加载完成（不包含图片等非文字媒体文件）。

#### 1.2 onload
指示页面包含图片等非文字媒体文件在内的所有元素都加载完成。

### 2 DOM ready
#### 2.1 文档加载的顺序
域名解析-->加载HTML-->加载JavaScript和CSS-->加载图片等非文字媒体文件。
DOM ready在加载javascript和CSS与加载图片等非文字媒体文件之间，表示DOM加载完成，可以对DOM进行操作。
例如，只要<img>标签加载完成，不用等该图片加载完成，就可以设置图片的属性或样式等。
在原生JavaScript中没有DOM ready的直接方法。

#### 2.2 jQuery的ready()方法
```
$(function(){
    ...
});
```
等价于
```
$(document).ready(function(){
    ...
});
```
等价于
```
$().ready(function(){
    ...
});
```

### 3 DOM load
#### 3.1 文档加载的顺序
域名解析-->加载HTML-->加载JavaScript和CSS-->加载图片等非文字媒体文件
DOM load在加载图片等非文字媒体文件之后，表示在document文档加载完成后才可以对DOM进行操作，document文档包括了加载图片等非文字媒体文件。
例如，需要等该图片加载完成，才可以设置图片的属性或样式等。

#### 3.2 在原生JavaScript中使用onload事件
#### 3.2.1 window load
```
window.onload = function() {
    ...
}
```

#### 3.2.2 图片load
```
document.getElementsByTagName("img")[0].onload = function() {
    ...
}
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
