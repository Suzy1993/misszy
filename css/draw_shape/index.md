[<< 回到主页](http://suzy1993.github.io/misszy/)

## 纯CSS 绘制圆形、椭圆形、菱形、三角形

### 1 圆形
```
width: 50px;
height: 50px;
border-radius: 50%; // 50%~100%之间都行
background: pink;
```
border-radius设置成50%~100%之间均可，原因如下：
W3C对于重合曲线有这样的规范：如果两个相邻的角的半径和超过了对应的盒子的边的长度，那么浏览器要重新计算保证它们不会重合。比如，如果border-radius设置成为80%，则两个相邻圆角合起来将是160%；如果border-radius设置成为100%，则两个相邻圆角合起来将是200%。这两种情况都超过了对应的盒子的边的长度，浏览器会重新计算保证它们不会重合，同时缩放两个相邻圆角的半径直到它们可以刚好符合盒子，因此两个相邻圆角的半径就会变成50%。

### 2 半圆形（以左圆形为例）
```
width: 50px;
height: 100px;
border-radius: 50px 0 0 50px;
background: pink;
```

### 3 四分之一圆形（以左上圆形为例）
```
width: 50px;
height: 50px;
border-radius: 50px 0 0 0;
background: pink;
```

### 4 椭圆形
```
width: 100px;
height: 75px;
border-radius: 50%;
background: pink;
```

### 5 半椭圆形（以左椭圆形为例）
```
width: 100px;
height: 75px;
border-radius: 100% 0 0 100%/50%;
/*相当于100% 0 0 100%/50% 50% 50% 50%;*/
background: pink;
```
通常情况下，border-radius设置圆角的水平和垂直半径是一样的，但也可以对圆角的水平和垂直半径进行单独设置，使用以“/”来区别，“/”前面的值表示圆角的水平半径，“/”后面的值表示圆角的垂直半径。
例如：border-radius: 10px / 5px 15px 相当于border-radius:  10px 10px 10px 10px / 5px 15px 5px 15px 。

### 6 四分之一椭圆形（以左上椭圆形为例）
```
width: 100px;
height: 75px;
border-radius: 100% 0 0 0;
background: pink;
```

### 7 菱形
```
width: 100px;
height: 50px;
background: pink;
transform: skew(-20deg);
text-align: center;
line-height: 50px;
```
问题：形状变形后，字体也跟着变形。
解决：让字体旋转回来。
```
transform: skew(20deg);
```

### 8 三角形（以下三角为例）
利用border属性，把左、右、下边框设置为透明色或与背景色相同的颜色，推荐透明色，这样拓展性更好。
```
width: 0;
height: 0;
border-width: 50px;
border-style: solid;
border-color: pink transparent transparent transparent;
```

### 9 带边框的三角形（以下三角为例）
三角形本身就是border，不可能再给border添加border属性，因此用叠加层的方法。
思路：将两个三角形叠加在一起，外层三角形是一个稍大些的div，颜色设置成边框所需的颜色；内层三角形是通过绝对定位放置在里面的一个div。
内层div的绝对定位应该根据相对定位外层div的内容边界计算，而外层div的宽高均为0，因此内层div应该根据外层div的中心点也就是外层三角形的下顶点来定位。
1）外层三角形
```
position:relative;
width: 0;
height: 0;
border-width: 50px;
border-style: solid;
border-color: grey transparent transparent transparent;
```
2）内层三角形
```
position: absolute;
top: -49px;
left: -48px;
width: 0;
height: 0;
border-width: 48px;
border-style: solid;
border-color: pink transparent transparent transparent;
```

### 10 东北、东南、西北、西南三角形（以西北三角形为例）
```
width: 0;
height: 0;
border-width: 50px 50px 0 0;
border-style: solid;
border-color: pink transparent transparent transparent;
```

### 11 带气泡框的三角形（以上气泡框为例）
1）绘制一个带圆角的矩形，设置相对定位以便外层三角形相对其进行绝对定位。
```
position:relative;
margin-top:20px;
width:100px;
height:50px;
background:pink;
padding:10px 20px;
border-radius:5px;
border:1px solid grey;
```
2）绘制外层三角形，相对矩形进行绝对定位，通过left和margin-left使其相对矩形水平居中，边框颜色设置为气泡框颜色。
```
display:block;
width:0;
height:0;
border-width:15px;
border-style:solid;
border-color:transparent transparent grey transparent;
position:absolute;
top:-30px;
left:50%;
margin-left:-15px;
```
3）绘制内层三角形，相对外层三角形进行绝对定位，边框颜色设置为与矩形背景色同色。
```
display:block;
width:0;
height:0;
border-width:15px;
border-style:solid;
border-color:transparent transparent pink transparent;
position:absolute;
top:-14px;
left:-15px;
```

[<< 回到主页](http://suzy1993.github.io/misszy/)

