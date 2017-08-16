[<< 回到主页](http://suzy1993.github.io/misszy/)

## 实现多个标签页之间的通信

调用localstorge、cookies等本地存储方式。

### 1 方法一
localstorge在一个标签页里被添加、修改或删除时，都会触发一个storage事件，通过在另一个标签页里监听storage事件，即可得到localstorge存储的值，实现不同标签页之间的通信。  
eg：  
标签页1：
```
<input id="name">
<input type="button" id="btn" value="提交">
<script type="text/javascript">
    $(function() {
        $("#btn").click(function() {
            var name=$("#name").val();
            localStorage.setItem("name", name);
        });
    });
</script>
```
标签页2：
```
<script type="text/javascript">
    $(function() {
        window.addEventListener("storage", function(event) {
            console.log(event.key + "=" + event.newValue);
        });
    });
</script>
```

### 2 方法二
使用cookie+setInterval，将要传递的信息存储在cookie中，每隔一定时间读取cookie信息，即可随时获取要传递的信息。  
eg：  
标签页1：
```
<input id="name">
<input type="button" id="btn" value="提交">
<script type="text/javascript">
    $(function(){
        $("#btn").click(function(){
            var name=$("#name").val();
            document.cookie="name="+name;
        });
    });
</script>
```
标签页2：
```
<script type="text/javascript">
    $(function(){
        function getCookie(key) {
            return JSON.parse("{\"" + document.cookie.replace(/;\s+/gim,"\",\"").replace(/=/gim, "\":\"") + "\"}")[key];
        }
        setInterval(function(){
            console.log("name=" + getCookie("name"));
        }, 10000);
    });
</script>
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
