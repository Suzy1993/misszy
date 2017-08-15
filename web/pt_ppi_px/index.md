[<< 回到主页](http://suzy1993.github.io/misszy/)

## iOS尺寸单位pt、ppi与px

### 1 屏幕尺寸
通常所说的iPhone3GS屏幕尺寸为3.5英寸、iPhone4屏幕尺寸为4英寸，指的是显示屏对角线的长度。

### 2 iOS尺寸单位
#### 2.1 px
像素，是物理屏幕显示的基本单位，即使在程序中使用的不是px，但最后都会转化为px，显示在手机上。
#### 2.2 pt
ios开发单位，即point，绝对长度，1pt=1/72英寸。
#### 2.3 ppi
Pixels Per Inch，即沿着对角线，每英寸所拥有的像素数目，屏幕像素密度。
ppi = √(X^2 + Y^2) / Z
其中，X和Y是像素分辨率，Z是屏幕尺寸。
如：iphone3GS的像素分辨率是320px*480px，屏幕尺寸为3.5英寸，因此可以计算得出，iphone3GS的屏幕像素密度为163。

### 3 iphone的发展
1）iphone3GS的逻辑分辨率为320pt*480pt，而iphone3GS的像素分辨率为320px*480px，所以iphone3GS中一个点正好对应一个像素。所以，当要添加一个30pt*30pt的图片，只要告诉美工做一个30px*30px的图片即可。
2）iphone4的逻辑分辨率没有改变，仍为320pt*480pt，但像素分辨率改为640px*960px，这时一个点对应四个像素。所以，当要添加一个30pt*30pt的图片，该图片的像素为60px*60px，如果美工只做30px*30px的图片，系统会将这个图片放大，出现模糊的现象，因此需要告诉美工做一个60px*60px的image@2x.png图片。
3）iphone5的屏幕尺寸改为4英寸，但由于像素密度没有改变，仍为iphone3GS的2倍，每英寸所拥有的像素数目iphone3GS的2倍，也即每pt所拥有的像素数目iphone3GS的2倍，因此iphone5和iphone5s都用的是@2x的图片。
4）iphone6的像素分辨率没有改变，但iphone6 plus的像素分辨率改变了，一个点差不多对应2.46个像素，但不是应该做一个@2.46x的图片，而是应该做一个@3x的图片，然后再缩放到@2.46x上。所以当要添加一个30pt*30pt的图片，只要告诉美工做一个大小为90px*90px的image@3x.png图片。

4、iphone的尺寸规格
1）@1x，163ppi（iphone3gs）
2）@2x，326ppi（iphone4、4s、5、5s、6）
3）@3x，401ppi（iphone6+）
<table>
  <tr><th>设备iPhone</th><th>宽Width</th><th>高Height</th><th>对角线Diagonal</th><th>逻辑分辨率(point)</th><th>Scale Factor</th><th>设备分辨率(pixel)</th><th>PPI</th></tr>
  <tr><th>3GS</th><th>2.4 inches (62.1 mm)</th><th>4.5 inches (115.5 mm)</th><th>3.5-inch</th><th>320x480</th><th>@1x</th><th>320x480</th><th>163</th></tr>
  <tr><th>4(s)</th><th>2.31 inches (58.6 mm)</th><th>4.5 inches (115.2 mm)</th><th>3.5-inch</th><th>320x480</th><th>@2x</th><th>640x960</th><th>326</th></tr>
  <tr><th>5c</th><th>2.33 inches (59.2 mm)</th><th>4.90 inches (124.4 mm)</th><th>4-inch</th><th>320x568</th><th>@2x</th><th>640x1136</th><th>326</th></tr>
  <tr><th>5(s)</th><th>2.31 inches (58.6 mm)</th><th>4.87 inches (123.8 mm)</th><th>4-inch</th><th>320x568</th><th>@2x</th><th>640x1136</th><th>326</th></tr>
  <tr><th>6</th><th>2.64 inches (67.0 mm)</th><th>5.44 inches (138.1 mm)</th><th>4.7-inch</th><th>375x667</th><th>@2x</th><th>750x1334</th><th>326</th></tr>
  <tr><th>6+</th><th>3.06 inches (77.8 mm)</th><th>6.22 inches (158.1 mm)</th><th>5.5-inch</th><th>414x736</th><th>@3x</th><th>(1242x2208->)1080x1920</th><th>401</th></tr>
</table>

[<< 回到主页](http://suzy1993.github.io/misszy/)

