[<< 回到主页](http://suzy1993.github.io/misszy/)

## CSS3 新特性——gradient

### 1 渐变
#### 1.1 渐变
两种或多种颜色之间的平滑过渡。

#### 1.2 线性渐变
两种或多种多种颜色沿渐变线过渡。

#### 1.3 渐变的组成
渐变线和色标。渐变线用来控制渐变的方向；色标包含颜色值和位置，用来控制渐变的颜色变化。

#### 2 线性渐变的语法
linear-gradient([[<angle> | to <side-or-corner>] ,]? <color-stop>[, <color-stop>]+)
<side-or-corner> = [left | right] || [top | bottom]
<color-stop> = <color> [ <length> | <percentage> ]?
#### 2.1 渐变线
<angle>——指定渐变的方向（或角度）。
to left：设置渐变为从右到左。相当于：270deg。
to right：设置渐变从左到右。相当于：90deg。
to top：设置渐变从下到上。相当于：0deg。
to bottom：设置渐变从上到下。相当于：180deg（默认值）。
to left top：设置渐变为从右上到左下。相当于：-45deg或315deg。
to ri
 ght top：设置渐变为从左上到右下。相当于：45deg。
to bottom left：设置渐变为从左上到右下。相当于：-135deg或225deg。
to bottom right：设置渐变为从左上到右下。相当于：135deg。
说明：
标准浏览器：0deg表示沿着元素的中心线由下向上的方向(类似于y轴)，且正角度表示顺时针旋转；
-webkit-非标准浏览器，0deg表示沿着元素中心线从左向右的方向(类似于x轴)，且正角度表示逆时针旋转。
-webkit-非标准浏览器与标准浏览器间的线性渐变的角度关系为：-webkit-浏览器 = 90deg - 标准浏览器，如-webkit-linear-gradient(90deg, red, blue) = linear-gradient(0deg, red, blue)。

#### 2.2 色标
<color-stop>——指定渐变的起止颜色。
<color>：指定颜色。
<length>：用长度值指定起止色位置，不允许负值。
<percentage>：用百分比指定起止色位置。
说明：
色标没有默认值，且必须设置至少两个色标。
色标由颜色和位置组成，位置可使用百分比或数值，可设置负值。
位置可以省略，默认第一个颜色的位置设置为0%，最后一个颜色的位置设置为100%，其他颜色均匀分布。
必须是颜色在前，位置在后。
若渐变只有两种颜色，第一个颜色的位置设置为x%，第二个颜色的位置设置为y%，则0%-x%的范围设置为第一个颜色，x%-y%的范围设置为第一个颜色到第二个颜色的渐变，y%-100%的范围设置为第二个颜色。
若多色占据同一个位置，则中间的颜色是无用的。如x、y、z三色均占据x%这一位置，则0%-x%为前一种颜色与x颜色的渐变；x%-x%为x颜色与z颜色的颜色突变；x%-100%为z颜色与后一种颜色的渐变。

[<< 回到主页](http://suzy1993.github.io/misszy/)