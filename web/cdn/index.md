[<< 回到主页](http://suzy1993.github.io/misszy/)

## CDN——内容分发网络

### 1 CDN
CDN的全称是Content Delivery Network，即内容分发网络。
CDN是一组分布在多个不同地理位置的Web服务器，用于更加有效地向用户发布内容，在优化性能时，会根据距离的远近来选择。
CDN系统能实时地根据网络流量和各节点的连接，负载状况及用户的距离和响应时间等综合信息将用户的请求重新导向离用户最近的服务节点上，其目的是使用户能就近地获取请求数据，解决网络拥塞，提高访问速度，解决由于网络带宽小、用户访问量大、网点分布不均等原因导致的访问速度慢的问题。
由于CDN部署在网络运营商的机房，这些运营商又是终端用户网络的提供商，因此用户请求的第一跳就到达CDN服务器，当CDN服务器中缓存有用户请求的数据时，就可以从CDN直接返回给浏览器，因此可以提高访问速度。
CDN用于部署静态内容：JS脚本、CSS样式表、图片、图标、Flash等，不包括HTML页面，这些静态资源文件的访问频率很高，将其缓存在CDN可以极大地提高网站的访问速度，但由于CDN是部署在网络运营商的机房，所以在一般的网站中都很少用CDN加速。

### 2 最简单的CDN实例
1）以博客园为例，博客园有很多站点，如www.cnblogs.com, news.cnblogs.com等等，它们之间会共享某些内容（如JavaScript、CSS、image、jQuery等），这些公共资源可以放在common.cnblogs.com这样的公共站点上。
2）以京东为例，广州的用户请求某图片，只需要从广州的网络运营商机房的CDN缓存服务器获取该图片即可。

### 3 CDN用于前端性能优化
#### 3.1 CDN用于前端性能优化的背景
浏览器是根据域（Domain）来缓存内容资源的，只要域（Domain）不一样，那么即使是同一个资源，也需要重复下载，且使用同样的方式缓存起来，这需要需要占用带宽和本地缓存空间。

#### 3.2 CDN用于前端性能优化
#### 3.2.1 将静态资源缓存到离用户最近的相同网络运营商的CDN节点上
不同地区的用户访问同一个域名能得到不同CDN节点的IP地址，这要依赖于CDN服务商提供的智能DNS服务，浏览器发起域名查询时，智能DNS服务会根据用户IP计算并返回离它最近的相同网络运营商的CDN节点IP，通过智能DNS服务获取最近的相同网络运营商的CDN节点IP后，不同地区的用户会向离自己最近的相同网络运营商的CDN节点发起请求，当请求达到CDN节点后，节点会判断自己的内容缓存是否有效，一个地区内只要有一个用户先加载资源，就会在CDN中建立缓存，该地区的其他后续用户都能直接读取缓存数据。

#### 3.2.2 加载静态资源使用与主页面不同的域名（不是用独立的二级或三级域名，而是用独立的一级域名）
静态资源和主页面不同域，加载静态资源的HTTP请求就不会带上主页面中的cookie等数据，减少了数据传输量，节省流量，提升上传效率。

### 4 知名的CDN服务
1）微软的CDN服务
2）Google的CDN服务

### 5 使用CDN的劣势
使用CDN，尤其是第三方的CDN，需要考虑网络的可到达性。第三方的CDN的Host在别人的服务器上，从一定意义上说并非很可控。

[<< 回到主页](http://suzy1993.github.io/misszy/)