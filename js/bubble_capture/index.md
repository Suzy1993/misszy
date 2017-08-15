[<< 回到主页](http://suzy1993.github.io/misszy/)

## JavaScript 事件冒泡 VS 事件捕获

### 1 事件冒泡
在一个对象上触发某类事件，如onclick事件等，在其祖先节点上也会依次触发该事件。
```
<body onclick="alert('body')">
    <div onclick="alert('div')">
        <a href="" onclick="alert('a')">事件冒泡</a>
    </div>
</body>
```
依次输出：a、div、body
注意：不是所有的事件都能冒泡。blur、focus、load、unload等事件不冒泡。

### 2 阻止事件冒泡
若只希望事件发生在该子元素而不是在它的祖先元素上，则需要阻止事件冒泡。
IE浏览器和其他浏览器阻止事件冒泡的方式不同：
```
<div>
    <a href="">事件冒泡</a>
</div>
<script>
    function stopBubble(e){
        if(e && e.stopPropagation)
            e.stopPropagation(); // 非IE浏览器
        else
            window.event.cancelBubble = true; // IE浏览器
    }
    document.getElementsByTagName("body")[0].onclick = function(e) {
        stopBubble(e);
        alert('body');
    }
    document.getElementsByTagName("div")[0].onclick = function(e) {
        stopBubble(e);
        alert('div');
    }
    document.getElementsByTagName("a")[0].onclick = function(e) {
        stopBubble(e);
        alert('a');
    }
</script>
```
输出：a

### 3 事件冒泡与事件捕获
1）事件捕获：事件从document开始往下查找，直到捕获到事件目标(target)。
2）事件冒泡：事件从事件目标(target)开始，往上冒泡直到document为止。
传统的element.onclick = doSomething这样的事件绑定，一般采用的是事件冒泡形式。
```
<div>
    <p>传统的事件冒泡</p>
</div>
<script>
    document.getElementsByTagName("p")[0].onclick = function(e) {
        alert('p');
    };
    document.getElementsByTagName("div")[0].onclick = function(e) {
        alert('div');
    };
</script>
```
依次输出：p、div
其实，可以选择绑定事件时采用事件捕获还是事件冒泡，方法是绑定事件时通过addEventListener函数，它有3个参数，第3个参数若是true，则表示采用事件捕获，若是false，则表示采用事件冒泡，如element.addEventListener('click', doSomething, true)。
```
<div>
    <p>设置的事件冒泡</p>
</div>
<script>
    document.getElementsByTagName("p")[0].addEventListener('click', function(e) {
        alert('p');
    }, false);
    document.getElementsByTagName("div")[0].addEventListener('click', function(e) {
        alert('div');
    }, false);
</script>
```
依次输出：p、div
```
<div>
    <p>设置的事件捕获</p>
</div>
<script>
    document.getElementsByTagName("p")[0].addEventListener('click', function(e) {
        alert('p');
    }, true);
    document.getElementsByTagName("div")[0].addEventListener('click', function(e) {
        alert('div');
    }, true);
</script>
```
依次输出：div、p
注意：Chrome和Firefox都支持事件捕获和事件冒泡，但IE只支持事件冒泡，不支持事件捕获，也不支持addEventListener函数，提供了另一个函数attachEvent，如ele.attachEvent("onclick", doSomething)。

[<< 回到主页](http://suzy1993.github.io/misszy/)
