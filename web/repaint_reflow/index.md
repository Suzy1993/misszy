[<< 回到主页](http://suzy1993.github.io/misszy/)

## 重绘 VS 重排/回流

### 1 重绘（repaint）和重排/回流（reflow）
任何对渲染树的修改都有可能会导致重绘（repaint）或重排/回流（reflow）操作。
#### 1.1 重排/回流（reflow）
Rendering Tree的部分节点的尺寸或位置发生了变化，会触发重排/回流操作。
每个页面至少在初始化时会有一次重排/回流操作。
重排/回流必然导致重绘。
重排/回流（reflow）的成本比重绘（repaint）的成本高得多。一个结点的重排/回流（reflow）很可能导致其子节点，甚至父节点及同级节点的重排/回流（reflow），甚至触发整个文档的重排/回流（reflow）和重绘（repaint）。

#### 1.2 重绘（repaint）
Rendering Tree的部分节点的尺寸或位置没有改变，但改变了visibility、outline、背景颜色等，会触发重绘操作。
重绘不会带来重新布局，所以并不一定伴随重排/回流。

### 2 触发reflow或repaint的操作
1）DOM Tree的结构变化，如增加、修改或删除DOM节点，移动DOM节点的位置：reflow & repaint  
2）增加或修改样式：repaint或reflow & repaint  
3）调整浏览器窗口大小：reflow & repaint  
4）获取某些属性：reflow & repaint  
浏览器引擎可能会针对reflow做优化，如Opera会等到有足够数量的变化，或一定的时间，或一个线程结束，再一起处理，这样就只发生一次reflow。但除了Render Tree的直接变化，当获取一些属性时，浏览器为取得正确的值也会触发重排。这样就使得浏览器的优化失效。这些属性包括：offsetTop、offsetLeft、 offsetWidth、offsetHeight、scrollTop、scrollLeft、scrollWidth、scrollHeight、 clientTop、clientLeft、clientWidth、clientHeight、getComputedStyle()（currentStyle in IE），因此在需要多次使用这些值时应进行缓存到变量。
5）DOM元素的几何属性变化，如display: none：reflow & repaint
注意：display: none会导致reflow和repaint，但visibility: hidden只会导致repaint。

### 3 优化点
1）将多次改变样式的操作合并成一次操作。  
手动合并：
```
var myDiv = document.getElementById(‘myDiv’);
myDiv.style.color = ‘#eee′;
myDiv.style.width = ‘200px’;
myDiv.style.height = ’200px’;
```
改为：
```
.divStyle {
    color: #eee;
    height: 200px;
    height: 200px;
}
var myDiv = document.getElementById(‘myDiv’);
myDiv.className = 'divStyle';
```
其实，浏览器引擎可能会针对reflow做优化，将多次改变样式的操作合并成一次操作。  
2）将需要多次重排的元素的position属性设为absolute或fixed。这样此元素就脱离了文档流，其变化不会影响到其他元素。  
3）多次操作节点，完成后再添加到文档中去，如要异步获取表格数据并渲染到页面，可以先获取数据后构建整个表格的HTML内容，再一次性添加到文档中去，而不是循环添加每一行。  
4）由于display属性为none的元素不在Rendering Tree中，对其进行操作不会引发其他元素的reflow，因此若要对一个元素进行复杂的操作，可以先隐藏，操作完成后显示。这样只在隐藏和显示该元素时触发2次重排。  
5）在需要经常获取那些会引起reflow的属性值时，要缓存到变量。

[<< 回到主页](http://suzy1993.github.io/misszy/)
