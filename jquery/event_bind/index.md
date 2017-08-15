## jQuery bind() & live() & delegate() & on() & off() & one()

### 1 bind()方法
格式：$(selector).bind(event,data,function);
<table>
  <tr><th>参数</th><th>描述</th></tr>
  <tr><td>event</td><td>必需。规定要绑定的一个或多个事件，由空格分隔多个事件。</td></tr>
  <tr><td>data</td><td>可选。规定传递到函数的额外数据。</td></tr>
  <tr><td>function</td><td>必需。规定当事件发生时运行的函数。</td></tr>
</table>

### 2 live()方法
在 jQuery 版本 1.7 中被废弃，在版本 1.9 中被移除。
格式：$(selector).live(event,data,function);
<table>
  <tr><th>参数</th><th>描述</th></tr>
  <tr><td>event</td><td>必需。规定要绑定的一个或多个事件，由空格分隔多个事件。</td></tr>
  <tr><td>data</td><td>可选。规定传递到函数的额外数据。</td></tr>
  <tr><td>function</td><td>必需。规定当事件发生时运行的函数。</td></tr>
</table>
live()与bind()方法的不同：
* bind()方法不可以向尚未创建的元素（如通过脚本动态创建的新元素）添加事件处理程序，而live()方法可以。

### 3 delegate()方法
格式：$(selector).delegate(childSelector,event,data,function);
<table>
  <tr><th>参数</th><th>描述</th></tr>
  <tr><td>childSelector</td><td>必需。规定要附加事件处理程序的一个或多个子元素。</td></tr>
  <tr><td>event</td><td>规定要绑定的一个或多个事件，由空格分隔多个事件。</td></tr>
  <tr><td>data</td><td>可选。规定传递到函数的额外数据。</td></tr>
  <tr><td>function</td><td>必需。规定当事件发生时运行的函数。</td></tr>
</table>
delegate()与on()方法的不同：
* childSelector和event参数的顺序不同。
* on()方法的childSelector可选，delegate()方法的childSelector必需。

### 4 on()方法
jquery 1.7开始，on()方法提供绑定事件处理程序所需的所有功能，是 bind()、live() 和 delegate() 方法的新的替代品。
格式：$(selector).on(event,childSelector,data,function,map);
<table>
  <tr><th>参数</th><th>描述</th></tr>
  <tr><td>event</td><td>必需。规定要绑定的一个或多个事件，由空格分隔多个事件。</td></tr>
  <tr><td>childSelector</td><td>可选。规定只能添加到指定子元素上的事件处理程序。</td></tr>
  <tr><td>data</td><td>可选。规定传递到函数的额外数据。</td></tr>
  <tr><td>function</td><td>必需。规定当事件发生时运行的函数。</td></tr>
  <tr><td>map</td><td>规定事件映射，将一个或多个事件一一绑定到当事件发生时运行的函数，格式为 ({event:function, event:function, ...})。</td></tr>
</table>
eg1：绑定多个事件
```
<!DOCTYPE html>
<html>
    <head>
        <script src="js/jquery-1.8.2.min.js"></script>
        <script>
            $(document).ready(function() {
                $("p").on("click mouseout", function() {
                    alert("click和mouseout触发");
                });
                $("span").on({
                    "click": function() { alert("click触发"); },
                    "mouseout": function() { alert("mouseout触发"); }
                });
            });
        </script>
    </head>
    <body>
        <p>为多个事件绑定相同的函数</p>
        <span>为不同事件绑定不同的函数</span>
    </body>
</html>
```
eg2：添加自定义事件
需要利用jquery trigger来触发自定义事件，trigger传入的第一个参数是自定义的事件名，第二个参数是一个数组，数组中的项与自定义事件中回调函数的参数一一对应。回调函数中的第一个参数是事件，后面是其他参数（参数可以是函数）。
```
<!DOCTYPE html>
<html>
    <head>
        <script src="js/jquery-1.8.2.min.js"></script>
        <script>
            $(document).ready(function() {
                $("p").on("SelfDefinitionEvent", function(event, param1, param2, func){
                    alert(param1);
                    alert(param2);
                    func();
                });
                $("button").click(function() {
                    $("p").trigger("SelfDefinitionEvent", ["First", "Second", function(){ alert("Success") }]);
                });
            });
        </script>
    </head>
    <body>
        <p>自定义事件</p>
        <button>触发自定义事件</button>
    </body>
</html>
```
eg3：为函数传递数据
```
<!DOCTYPE html>
<html>
    <head>
        <script src="js/jquery-1.8.2.min.js"></script>
        <script>
            $(document).ready(function() {
                $("p").on("click", {name: "Alice"}, function(event) {
                    alert(event.data.name);
                });
            });
        </script>
    </head>
    <body>
        <p>为函数传递数据</p>
    </body>
</html>
```
eg4：delegate()、on()、live()向尚未创建的元素（如通过脚本动态创建的新元素）添加事件处理程序
```
<!DOCTYPE html>
<html>
    <head>
        <script src="js/jquery-1.8.2.min.js"></script>
        <script>
            $(document).ready(function(){
                $("#parent").delegate("#p1", "click", function() {
                    alert("delegate方法绑定成功");
                });
                $("#p2").live("click", function() {
                    alert("live方法绑定成功");
                });
                $("#parent").on("click", "#p3", function() {
                    alert("on方法绑定成功");
                });
                $("#p4").bind("click", function() {
                    alert("bind方法绑定成功");
                });
                $("#p5").click("click", function() {
                    alert("click方法绑定成功");
                });
                $("<p id='p1'>delegate方法</p>").appendTo($("#parent"));
                $("<p id='p2'>live方法</p>").appendTo($("#parent"));
                $("<p id='p3'>on方法</p>").appendTo($("#parent"));
                $("<p id='p4'>bind方法</p>").appendTo($("#parent"));
                $("<p id='p5'>click方法</p>").appendTo($("#parent"));
            });
        </script>
    </head>
    <body>
        <div id="parent"></div>
    </body>
</html>
```

### 5 off()方法
eg5：移除事件处理程序
```
<!DOCTYPE html>
<html>
    <head>
        <script src="js/jquery-1.8.2.min.js"></script>
        <script>
            $(document).ready(function() {
                $("p").on("click", function() {
                    alert("off()方法");
                });
                $("button").on("click", function() {
                    $("p").off("click");
                });
            });
        </script>
    </head>
    <body>
        <p>off()方法</p>
        <button>移除click事件</button>
    </body>
</html>
```

### 6one()方法
eg6：添加只运行一次的事件处理程序然后移除
```
<!DOCTYPE html>
<html>
    <head>
        <script src="js/jquery-1.8.2.min.js"></script>
        <script>
            $(document).ready(function() {
                $("p").one("click", function() {
                    alert("one()方法");
                });
            });
        </script>
    </head>
    <body>
        <p>one()方法</p>
    </body>
</html>
```
