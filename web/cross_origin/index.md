[<< 回到主页](http://suzy1993.github.io/misszy/)

## 同源策略与跨域问题

### 1 同源策略
出于安全方面的考虑，为了保证用户信息的安全，防止恶意的网站盗取数据，不允许跨域调用其他页面的对象。  
同源：
* 协议相同
* 域名相同
* 端口相同
<table>
  <tr><th>域名一</th><th>域名二</th><th>是否允许通信</th></tr>
  <tr><td>http://www.domain.com/a.html</td><td>http://www.domain.com/b.html</td><td>同一域名，允许通信</td></tr>
  <tr><td>http://www.domain.com/main/a.html</td><td>http://www.domain.com/rest/b.html</td><td>同一域名，不同文件夹，允许通信</td></tr>
  <tr><td>http://www.domain.com:8000/a.html</td><td>http://www.domain.com/b.html</td><td>同一域名，不同端口，不允许通信</td></tr>
  <tr><td>http://www.domain.com/a.html</td><td>https://www.domain.com/b.html</td><td>同一域名，不同协议，不允许通信</td></tr>
  <tr><td>http://www.domain.com/a.html</td><td>http://10.3.8.211/b.html</td><td>域名和域名对应IP 不允许通信</td></tr>
  <tr><td>http://www.domain.com/a.html</td><td>http://suzy.domain.com/b.html</td><td>相同主域，不同二级域名，不允许通信</td></tr>
  <tr><td>http://www.domain.com/a.html</td><td>http://domain.com/b.html</td><td>相同主域，不同二级域名，不允许通信</td></tr>
  <tr><td>http://www.domain.com/a.html</td><td>http://www.suzy.com/b.html</td><td>不同域名，不允许通信</td></tr>
</table>
注意：<script>标签的src属性不被同源策略所约束，所以可以获取任何服务器上的JS脚本并执行。

### 2 非同源时受限的三种行为
* cookie无法获取
* localStorage无法获取
* DOM无法获取
* Aax请求不能发送

### 3 跨域的解决方案
#### 3.1 跨域获取cookie
一级域名相同，只是二级域名不同的情况下，浏览器允许通过设置document.domain共享Cookie。也就是说，Cookie只能跨二级域名来访问，不能跨一级域名来访问。

#### 3.2 跨域获取localStorage
假设有http://javascript.exam.com/text.html和http://jquery.exam.com/text.html两个页面。  
通过http://javascript.exam.com/text.html页面去修改http://jquery.exam.com/text.html页面的本地数据：
* 在http://javascript.exam.com/text.html页面创建一个iframe，嵌入http://jquery.exam.com/text.html页面。
* http://javascript.exam.com/text.html页面通过postMessage传递指定格式的消息给http://jquery.exam.com/text.html页面。
* http://jquery.exam.com/text.html页面解析http://javascript.exam.com/text.html页面传递过来的消息内容，调用localStorage API 操作本地数据。
* http://jquery.exam.com/text.html页面包装localStorage的操作结果，并通过postMessage传递给http://javascript.exam.com/text.html页面。
* http://javascript.exam.com/text.html页面解析http://jquery.exam.com/text.html页面传递回来的消息内容，得到 localStorage 的操作结果。

#### 3.3 跨域获取DOM
如果两个网页不同源，就无法拿到对方的DOM。典型的例子是iframe窗口和window.open方法打开的窗口，它们与父窗口无法通信。
#### 3.3.1 如果两个窗口一级域名相同，只是二级域名不同，那么设置document.domain属性，即可跨域获取DOM
http://blogs.msnova.net/b.html：
```
document.domain = "msnova.net";
```
http://www.msnova.net/a.html：
```
document.domain = "msnova.net";
var ifr = document.createElement('iframe');
ifr.src = 'http://blogs.msnova.net/b.html';
ifr.style.display = 'none';
document.body.appendChild(ifr);
ifr.onload = function(){
    var x = ifr.contentDocument;
    alert(x.getElementsByTagName("h1")[0].childNodes[0].nodeValue); //操作b.html
    ifr.onload = null;
}
```

#### 3.3.2 否则，可以通过window.name和window.postMessage跨域获取DOM
#### 3.3.2.1 通过window.name跨域获取DOM
在iframe指向的页面写入window.name，在本页面通过iframe的contentWindow.name获取。  
http://JavaScript.exam.com/text.html：
```
<!DOCTYPE html>
<html>
  <head>
  </head>
  <body>
    <script>
      window.name = "value";
    </script>
  </body>
</html>
```
http://catagory.exam.com/text.html：
```
<!DOCTYPE html>
<html>
  <head>
`</head>
  <body>
    <iframe id="iframe" onload="loading()" src="http://javascript.exam.com/text.html"></iframe>
    <script>
      var load = false;
      function loading() {
        if (load == false) {
          // 同域处理，请求后会再次重新加载iframe
          document.getElementById('iframe').contentWindow.location = 'http://catagory.exam.com/index.html';
          load = true;
        }
        else {
          // 获取window.name的内容，注意必须进行同域处理后方可访问！
          alert(document.getElementById('iframe').contentWindow.name); //输出：value
          load = false;
        }
      }
    </script>
  </body>
</html>
```

#### 3.3.2.2 通过window.postMessage跨域获取DOM
在本页面写入iframe的contentWindow.postMessage，在iframe指向的页面监听message事件获取。  
http://catagory.exam.com/text.html：
```
<!DOCTYPE html>
<html>
  <head>
  </head>
  <body>
    <iframe id="iframe" src="http://JavaScript.exam.com/Test/text.html"></iframe>
    <script>
      window.onload = function() {
        document.getElementById('iframe').contentWindow.postMessage('Hello', "http://JavaScript.exam.com");
      };
    </script>
  </body>
</html>
```
http://JavaScript.exam.com/text.html：
```
<!DOCTYPE html>
<html>
  <head>
    <title></title>
  </head>
  <body>
    <script>
      window.addEventListener('message', function(event){
        // 通过origin属性判断消息来源地址
        if (event.origin == 'http://catagory.exam.com')
          alert(event.data); //输出：Hello
      }, false);
    </script>
  </body>
</html>
```

#### 3.4 跨域获取Ajax请求
Ajax跨域的解决方案有2种：CORS、JSONP。
#### 3.4.1 CORS
CrossOrigin Resource Sharing跨域资源共享。  
当前几乎所有的浏览器（Internet Explorer 8+，Firefox 3.5+，Safari 4+和Chrome）都可通过名为跨域资源共享（Cross-Origin Resource Sharing）的协议支持ajax跨域调用。   
对一个简单的请求，没有自定义头部，要么使用GET，要么使用POST，它的主体是text/plain，请求用一个名叫Origin的额外的头部发送。Origin头部包含请求页面的头部（协议，域名，端口），这样服务器可以很容易的决定它是否应该提供响应。  
服务器端：JSP页面中设置response.addHeader("Access-Control-Allow-Origin", "http://www.yoursite.com:8080")。  
在请求信息中，浏览器使用 Origin 这个 HTTP 头来标识该请求来自于 http://www.yoursite.com:8080（发出跨区请求的url）。  
在返回的响应信息中，使用 Access-Control-Allow-Origin 头来控制哪些域名的脚本可以访问该资源。  
如果设置 Access-Control-Allow-Origin为*，则允许所有域名的脚本访问该资源。如果有多个，则只需要使用逗号分隔开即可。

#### 3.4.2 JSONP
通过callback形式实现跨域访问。JSONP比JSON外面有多了一层callback()，也就是说，在服务器端需要先将查询结果转换成JSON格式，然后用参数callback在JSON外面再套一层，就变成了JSONP。
#### 3.4.2.1 JavaScript与JSONP
JSONP的简单实现模式：考虑到script标签的src属性可以跨域，动态创建一个script标签，在src中传递要发送的数据和回调函数，服务器接收数据处理后生成JSON数据，用回调函数包裹后，返回给客户端，完成回调。

#### 3.4.2.2 jQuery与JSONP
jQuery框架也支持JSONP，可以使用$.getJSON(url,[data],[callback])方法和$.ajax()方法。
#### 3.4.2.2.1 getJSON()方法
getJSON()方法在url的后面必须添加一个callback参数，这样getJSON()方法才知道是用JSONP方式，callback后面的问号是内部自动生成的一个回调函数名。

#### 3.4.2.2.2 $.ajax()方法
使用$.ajax形式可以不在url的后面添加一个callback参数，但此时需要指定jsonp参数。jsonp: “callback”代表的是服务端通过String callback = request.getParameter("callback") 接收客户端回调函数名的参数名，ajax请求中jsonp参数的默认值就是callback，也可以自己随便定义。jsonpCallback: “callbackHandler”代表的是服务端调用结束后的本地回调函数名，比如jsonp: “callback”中的客户端回调函数名，jsonpCallback的参数值也可以自己随便定义，也可以不给jsonpCallback参数，其实jQuery会自动生成一个函数和函数名，远程服务调用成功后，既执行了success这个回调函数，也执行自己定义的jsonpCallback指定的回调函数，所以完全可以使用jQuery生成的回调函数，在调用结束后在success回调中做相应的处理即可。

[<< 回到主页](http://suzy1993.github.io/misszy/)
