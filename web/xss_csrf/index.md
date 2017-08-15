[<< 回到主页](http://suzy1993.github.io/misszy/)

## XSS & CSRF跨站攻击

### 1 XSS：跨站脚本（Cross-site scripting）
实现XSS攻击可以通过JavaScript、ActiveX控件、Flash插件、Java插件等技术手段实现
#### 1.1 通过JavaScript实现的XSS攻击
#### 1.1.1 Cookie劫持
Cookie中往往会存储一些用户安全级别较高的信息，如用户的登陆凭证。当用户所访问的网站被注入恶意代码，恶意代码只需通过简单的JavaScript代码——document.cookie ，就可以顺利获取到用户当前访问网站的Cookie。如果攻击者能获取到用户登陆凭证的Cookie，甚至可以绕开登陆流程，直接设置这个Cookie的值，来访问用户的账号。

#### 1.1.2 构造请求
JavaScript可以通过多种方式向服务器发送GET与POST请求。如果恶意代码顺利模拟用户操作，向服务器发送有效请求，将对用户造成重大损失，如：更改用户资料、删除用户信息等。

#### 1.1.3 XSS钓鱼
伪造一个高度相似的网站，欺骗用户在钓鱼网站上面填写账号密码或进行交易。如：注入恶意代码，会弹出一个相似的弹窗，提示用户输入账号密码登陆，当用户输入后点击发送，账号密码信息已经发送到了攻击者的服务器上。

#### 1.1.4 CSS History Hack
结合浏览器历史记录和CSS的伪类a:visited，通过遍历一个网址列表来获取其中<a>标签的颜色，就能知道用户访问过什么网站。

#### 1.2 XSS防御技巧
#### 1.2.1 HttpOnly
服务器端在设置安全级别高的Cookie时，带上HttpOnly的属性，防止JavaScript获取。

#### 1.2.2 输入检查
任何用户输入的数据，都是“不可信”的。输入检查，一般是用于输入格式检查，但是，输入过滤不能完全交由前端负责，前端的输入过滤只是为了避免普通用户的错误输入，减轻服务器的负担，因为攻击者完全可以绕过正常输入流程，直接利用相关接口向服务器发送设置，所以前端和后端要做相同的过滤检查。

#### 1.2.2.1 格式检查
如邮箱、电话号码、用户名等是否按照规定的格式输入。

#### 1.2.2.2 检查特殊字符
如发现<、>、'、"等特殊字符，将其过滤或编码。

#### 1.2.2.3 检查XSS特征
如用户输入中是否包含了script标签、"javascript"等敏感字符。

#### 1.2.3 输出检查
相比输入检查，前端更适合做输出检查。HttpOnly和前端没直接关系，输入检查的关键点也不在于前端。JavaScript直接通过Ajax向服务器请求数据，接口把数据以JSON格式返回。前端整合处理数据后，输出页面。所以，前端的XSS防御点在于输出检查。在变量输出到HTML页面时，可以使用编码或转义的方式来防御XSS攻击，一般会对& < > " ' /等字符进行转换。在JavaScript中，可以用escape()函数。

### 2 CSRF：跨站请求伪造（Cross-site request forgery）
网站A：恶意网站。网站B：用户已登录的网站。当用户访问A站时，A站私自访问B站的操作链接，就可以模拟用户操作。假设B站有一个删除评论的链接，A站直接访问该链接，就能删除用户在B站的评论。
CSRF成功的前提用户必须登录到目标站点，且用户浏览了攻击者控制的站点。
#### 2.1 CSRF的攻击策略
因为浏览器访问 B站 相关链接时，会向其服务器发送 B站 保存在本地的Cookie，以判断用户是否登陆。所以通过 A站 访问的链接，也能顺利执行。

#### 2.2 CSRF 防御技巧
#### 2.2.1 验证码
验证码不单单用来防止注册机的暴力破解，还可以有效防止CSRF的攻击，是对抗CSRF攻击最简洁有效的方法。但使用验证码的问题在于，不可能在用户的所有操作上都需要输入验证码，只有一些关键的操作，才能要求输入验证码，不过随着HTML5的发展，利用canvas标签，前端也能识别验证码的字符，让CSRF生效。

#### 2.2.2 来源检测
HTTP Referer是Request Headers的一部分，当浏览器向服务器发出请求时，一般会带上Referer，告诉服务器用户从哪个站点链接过来的。服务器通过判断请求头中的Referer，也能避免CSRF的攻击。

#### 2.2.3 Token
CSRF能攻击成功，根本原因是：操作所带的参数均被攻击者猜测到。
当向服务器传参数时，带上Token，Token是一个随机值，并且由服务器和用户同时持有，Token可以存放在用户浏览器的Cookie中，当用户提交表单时带上Token值，服务器就能验证表单和Cookie中的Token是否一致。
前提：网站没有XSS漏洞，攻击者不能通过脚本获取用户的Cookie。

### 3 XSS和CSRF的区别
* XSS：用户过分信任网站，放任来自浏览器地址栏的那个网站代码在自己本地任意执行，如果没有浏览器的安全机制限制，XSS代码可以在用户浏览器为所欲为；
* CSRF：网站过分信任用户，放任来自所谓通过访问控制机制的代表合法用户的请求执行网站的某个特定功能。

[<< 回到主页](http://suzy1993.github.io/misszy/)