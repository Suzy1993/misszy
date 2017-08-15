[<< 回到主页](http://suzy1993.github.io/misszy/)

## 浏览器的缓存机制

浏览器缓存机制有两种：HTML Meta标签、HTTP头信息。

### 1 HTML meta标签
可以在HTML的<head>中加入<meta>标签：
<meta http-equiv="Pragma" content="no-cache”>
上述代码的作用是告诉浏览器当前页面不被缓存，每次访问都需要去服务器拉取。使用上很简单，但只有部分浏览器可以支持，而且所有缓存代理服务器都不支持，因为代理不解析HTML内容本身。

### 2 HTTP头信息
浏览器缓存机制，其实主要是HTTP协议定义的缓存机制，如Expires；Cache-control等。广泛应用的是HTTP头信息来控制缓存。
#### 2.1 Expires策略
```
<meta http-equiv="Expires" content="Sun, 31 Dec 2017 23:59:59 GMT" />
```
在HTML中设置meta的Expires需要Web服务器支持，但基本上现在的主流服务器都不会识别这个Tag，因此不建议这样设置。
Expires是服务器响应消息头字段，在响应http请求时，告诉浏览器在过期时间前，浏览器可以直接从浏览器缓存读取数据（200 from cache），而无需再次请求。
然而，由于Expires是HTTP 1.0的，而现在浏览器均默认使用HTTP 1.1，所以Expires的作用基本忽略。
Expires的缺点：返回的到期时间是服务器端的时间，如果浏览器的时间与服务器的时间相差很大（如时钟不同步或跨时区），那么误差就很大。所以在HTTP 1.1开始，使用Cache-Control替代。

#### 2.2 Cache-control策略
```
<meta http-equiv="Cache-Control" content="max-age=2400" />
```
在HTML中设置meta的Cache-Control需要Web服务器支持，但基本上现在的主流服务器都不会识别这个Tag，因此不建议这样设置。
Cache-Control与Expires的作用相同，都是指明当前资源的有效期，控制浏览器是直接从浏览器缓存取数据还是重新发送请求到服务器读取数据。但是Cache-Control的选择更多，如果和Expires同时设置，其优先级高于Expires。

#### 2.3 Last-Modified/If-Modified-Since（要配合Cache-Control使用）
#### 2.3.1 Last-Modified
标示当前资源的最后修改时间。服务器在响应请求时，会将资源最后更改的时间以“Last-Modified: GMT”的形式加在响应消息包头上一起返回给浏览器。浏览器会为资源标记上该信息，下次再次请求时，会附带头 If-Modified-Since给服务器，比较其与服务器上该资源的最后修改时间，来判断该资源是否被修改过，决定返回200状态码和整个资源内容还是直接返回304状态码和响应消息包头。

#### 2.3.2 If-Modified-Since
当资源过期时（超过Cache-Control中的max-age），若发现资源具有Last-Modified声明，则再次向服务器请求时带上头If-Modified-Since（Last-Modified的值）。服务器收到请求后发现带有头If-Modified-Since ，则与资源的最后修改时间进行比对。若最后修改时间较新，说明资源被修改过，则响应HTTP 200和整个资源内容（写在响应消息包体内）；若最后修改时间较旧，说明资源未被修改过，则响应HTTP 304和响应消息包头（无需响应消息包体），告知浏览器直接从浏览器缓存取数据。

#### 2.4 Etag/If-None-Match（要配合Cache-Control使用）
Last-Modified/If-Modified-Since的问题：如果在服务器上一个资源被修改过，但其实际内容并没发生改变，会因为Last-Modified时间匹配不上而返回了整片资源内容（即使浏览器缓存里有一模一样的资源）。
为了解决上述问题，Http1.1还推出了ETag 实体首部字段。但是，Last-Modified与ETag一起使用时，服务器会优先验证ETag。
#### 2.4.1 Etag
服务器会通过某种算法，给资源计算得出一个唯一标志符，仅仅是一个和文件相关的标记，可以是一个版本标记，如v1.0.0或c782-465-5fe381e7，服务器在响应请求时，会在响应消息包头上加上“ETag: 唯一标识符”一起返回给浏览器。浏览器会为资源标记上该信息，下次再次请求时，会附带头If-None-Match给服务器，比较其与服务器上该资源的ETag是否一致，决定返回200状态码和整个资源内容还是直接返回304状态码和响应消息包头。

#### 2.4.2 If-None-Match
当资源过期时（超过Cache-Control中的max-age），若发现资源具有Etag声明，则再次向服务器请求时带上头If-None-Match （Etag的值）。服务器收到请求后发现带有头If-None-Match，则与资源的ETag进行比对。若ETag不一致，则响应HTTP 200和整个资源内容（写在响应消息包体内，也包括新的ETag）；若ETag一致，则响应HTTP 304和响应消息包头（无需响应消息包体），告知浏览器直接从浏览器缓存取数据。

### 3 用户操作与浏览器缓存：
#### 3.1 Ctrl+F5强制刷新
所有缓存都失效, 直接发送请求到服务器读取数据。

#### 3.2 F5刷新
Expires和Cache-Control失效，发送请求到服务器，根据Last-Modified和Etag判断是从浏览器缓存读取数据还是从服务器读取数据。

#### 3.3 地址栏回车or页面链接跳转or新开窗口or前进后退
所有缓存都生效，先根据Expires和Cache-Control判断资源是否过期，若未过期则直接从浏览器缓存读取数据，否则发送请求到服务器，根据Last-Modified和Etag判断是从浏览器缓存读取数据还是从服务器读取数据。

[<< 回到主页](http://suzy1993.github.io/misszy/)

