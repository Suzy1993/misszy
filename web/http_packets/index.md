[<< 回到主页](http://suzy1993.github.io/misszy/)

## HTTP请求报文和响应报文

### 1 HTTP请求报文
<table>
  <tr><th>字段</th><th>说明</th></tr>
  <tr><td>Accept</td><td>浏览器支持的MIME 类型：text/plain, text/html等</td></tr>
  <tr><td>Accept-Encoding</td><td>浏览器支持的web服务器返回内容压缩编码类型：compress, gzip等</td></tr>
  <tr><td>Accept-Language</td><td>浏览器支持的语言类型，并且优先支持靠前的语言类型：en, zh等</td></tr>
  <tr><td>Cache-Control</td><td>请求和响应遵循的缓存机制：no-cache等</td></tr>
  <tr><td>Connection</td><td>浏览器与服务器通信时对于长连接如何进行处理（是否需要持久连接）：close, keep-alive</td></tr>
  <tr><td>Cookie</td><td>HTTP请求发送时，会把保存在该请求域名下的所有cookie值一起发送给web服务器</td></tr>
  <tr><td>Host</td><td>当前页面的来源URL，先前页面的地址，当前页面紧随其后，即来路。</td></tr>
  <tr><td>Referer</td><td>当前页面的来源URL，先前页面的地址，当前页面紧随其后，即来路。</td></tr>
  <tr><td>User-Agent</td><td>发出请求的用户信息</td></tr>
</table>

### 2 HTTP响应报文
<table>
  <tr><th>字段</th><th>说明</th></tr>
  <tr><td>Cache-Control</td><td>告诉所有的缓存机制是否可以缓存及哪种类型：no-cache等</td></tr>
  <tr><td>Connection</td><td>浏览器与服务器通信时对于长连接如何进行处理（是否需要持久连接）：close, keep-alive</td></tr>
  <tr><td>Content-Encoding</td><td>web服务器支持的返回内容压缩编码类型：compress, gzip等</td></tr>
  <tr><td>Content-Type</td><td>返回内容的MIME 类型：text/plain, text/html等</td></tr>
  <tr><td>Date</td><td>原始服务器消息发出的时间</td></tr>
  <tr><td>Expires</td><td>响应过期的日期和时间</td></tr>
  <tr><td>Server</td><td>服务器名字，Servlet一般不设置该值，而是由web服务器自己设置</td></tr>
  <tr><td>Set-Cookie</td><td>设置和页面关联的cookie</td></tr>
  <tr><td>Transfer-Encoding </td><td>数据传输的方式</td></tr>
</table>

[<< 回到主页](http://suzy1993.github.io/misszy/)
