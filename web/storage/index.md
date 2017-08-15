[<< 回到主页](http://suzy1993.github.io/misszy/)

## cookie & sessionStorage & localStorage

### 1 cookie & sessionStorage & localStorage的共同点
* 都是保存在浏览器端，且同源的

### 2 cookie & sessionStorage & localStorage的不同点
#### 2.1 数据传递
* Cookie的数据始终在同源的HTTP请求中携带（即使不需要），即会在浏览器和服务器之间来回传递。
* sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存。

#### 2.2 存储大小
* cookie的数据大小不能超过4k，同时因为每次HTTP请求都会携带cookie，所以cookie只适合保存很小的数据，如会话标识。
* sessionStorage和localStorage虽然也有存储大小的限制，但Web Storage数据完全存储在客户端，不需要通过浏览器请求将数据传给服务器，因此相比cookie来说能够存储更多的数据，可以达到5M或更大。

#### 2.3 有效时间
* localStorage存储持久数据，没有时间限制，浏览器关闭后数据不丢失，除非主动删除数据。
* sessionStorage数据在当前浏览器窗口关闭后自动删除。
* cookie在设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭。

#### 2.4 作用域
* sessionStorage不在不同的浏览器窗口中共享，即使是同源窗口（同一个页面的不同窗口）。
* localStorage和cookie在所有同源窗口中都是共享的。

### 3 cookie的安全问题
cookie信息是以明文文本的形式保存在浏览器的，如果能够截获某个用户的cookie变量，将cookie拷贝到自己的浏览器目录下，就可以以盗用的身份登录，伪造一个数据包发送过去，那么服务器还是认为是合法的。所以，使用cookie被攻击的可能性比较大，最好不要用cookie保存未加密的信息，否则会影响网站的安全性。

### 4 localStorage的安全问题
目前localStorage存储没有对XSS攻击有任何抵御机制，一旦出现XSS漏洞，那么存储在localStorage里的数据就极易被获取到，只要攻击者注入某些代码，就可以获取使用localStorage存储在本地的所有信息，也可以简单的使用localStorage.removeItem(key)和localStorage.clear()对存储数据进行清空。
使用localStorage时，需要时刻注意是否有代码存在XSS注入的风险，千万不要用localStorage存储系统中的身份验证信息或敏感信息。

### 5 Session对象
在服务器端有一个session池，用来存储每个用户session中的数据。session对于每一个客户端是“人手一份”。用户首次与Web服务器建立连接时，服务器会给用户分发一个SessionID作为标识，用户每次提交页面，浏览器都会把这个SessionID包含在HTTP头中提交给服务器，这样服务器就能区分当前请求页面的是哪一个客户端，而这个SessionID是以cookie的方式保存在客户端的，如果想要得到session池中的数据，服务器就会根据客户端提交的唯一SessionID标识给出相应的数据返回。
SessionID，是由Guid生成的一个由24个字符组成的随机字符串，使用GUID初始化SessionID类的新实例。

[<< 回到主页](http://suzy1993.github.io/misszy/)
