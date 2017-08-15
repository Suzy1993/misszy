## meta viewport设置移动端自适应

### 1 viewport
移动设备上的viewport是设备屏幕上用来显示网页的那部分区域，再具体一点就是浏览器上用来显示网页的那部分区域，但viewport又不局限于浏览器可视区域的大小，它可能比浏览器的可视区域大，也可能比浏览器的可视区域小。在默认情况下，移动设备上的viewport都是大于浏览器可视区域的，这是因为移动设备的分辨率相对于PC来说都比较小，所以为了能在移动设备上正常显示那些为PC浏览器设计的网站，移动设备上的浏览器都会把自己默认的viewport设为980px或1024px（也可能是其它值，由设备本身决定），但后果是浏览器出现横向滚动条，因为浏览器可视区域的宽度比默认的viewport的宽度小。

### 2 3个viewport
#### 2.1 layout viewport
如果把移动设备上浏览器的可视区域设为viewport的话，某些网站会因为viewport太窄而显示错乱，所以这些浏览器就默认会把viewport设为一个较宽的值，比如980px，使得即使是那些为PC浏览器设计的网站也能在移动设备浏览器上正常显示。这个浏览器默认的viewport叫做 layout viewport。layout viewport的宽度可以通过 document.documentElement.clientWidth来获取。

#### 2.2 visual viewport
layout viewport的宽度是大于浏览器可视区域的宽度的，所以还需要一个viewport来代表浏览器可视区域的大小，这个viewport叫做 visual viewport。visual viewport的宽度可以通过document.documentElement.innerWidth来获取。

#### 2.3 ideal viewport
ideal viewport是一个能完美适配移动设备的viewport。首先，不需要缩放和横向滚动条就能正常查看网站的所有内容；其次，显示的文字、图片大小合适，如14px的文字不会因为在一个高密度像素的屏幕里显示得太小而无法看清，无论是在何种密度屏幕，何种分辨率下，显示出来的大小都差不多。这个viewport叫做 ideal viewport。
ideal viewport并没有一个固定的尺寸，不同的设备有不同的ideal viewport。例如，所有的iphone的ideal viewport宽度都是320px，无论它的屏幕宽度是320还是640。
ideal viewport 的意义在于，无论在何种分辨率的屏幕下，针对ideal viewport 而设计的网站，不需要缩放和横向滚动条都可以完美地呈现给用户。

### 3 利用meta标签对viewport进行控制
移动设备默认的viewport是layout viewport，但在进行移动设备网站的开发时，需要的是ideal viewport。要得到ideal viewport，需要用到meta标签。
head标签中加入：
```
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0">
```
该meta标签的作用是让当前viewport的宽度等于设备的宽度，同时不允许用户手动缩放。当然maximum-scale=1.0, user-scalable=0不是必需的，是否允许用户手动播放根据网站的需求来定，但把width设为width-device基本是必须的，这样能保证不会出现横向滚动条。
meta viewport 的6个属性：
<table>
  <tr><th>属性</th><th>含义</th></tr>
  <tr><td>width</td><td>设置layout viewport 的宽度，为一个正整数，或字符串"width-device"</td></tr>
  <tr><td>initial-scale</td><td>设置页面的初始缩放值，为一个数字，可以带小数</td></tr>
  <tr><td>minimum-scale</td><td>允许用户的最小缩放值，为一个数字，可以带小数</td></tr>
  <tr><td>maximum-scale</td><td>允许用户的最大缩放值，为一个数字，可以带小数</td></tr>
  <tr><td>height</td><td>设置layout viewport 的高度，这个属性并不重要，很少使用</td></tr>
  <tr><td>user-scalable</td><td>是否允许用户进行缩放，值为"no"或"yes", no 代表不允许，yes代表允许</td></tr>
</table>
width能控制layout viewport的宽度，如果不指定该属性，layout viewport将默认为980px或1024px（也可能是其它值，由设备本身决定），如果把layout viewport的宽度设置为移动设备的宽度，那么layout viewport将成为ideal viewport。
其实，要把当前的viewport宽度设为ideal viewport的宽度，既可以设置width=device-width，也可以设置initial-scale=1，但有一个小缺陷，就是width=device-width会导致iphone、ipad横竖屏不分，initial-scale=1会导致IE横竖屏不分，都以竖屏的ideal viewport宽度为准。所以，最完美的写法两者都写上去， initial-scale=1 解决 iphone、ipad的缺陷，width=device-width解决IE的缺陷。
viewport设置移动端自适应的方法：
```
<meta name="viewport" content="width=device-width, initial-scale=1">
```
