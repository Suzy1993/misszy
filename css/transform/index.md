[<< 回到主页](http://suzy1993.github.io/misszy/)

## CSS3 新特性——transform

### 1 CSS3 transform属性
应用于2D或3D转换，允许对元素进行倾斜、移动、缩放或旋转。  
2D转换元素能够改变元素x和y轴，3D转换元素还能改变其Z轴。
<table>
  <tr><th>值</th><th>描述</th></tr>
  <tr><td>translate(x,y)</td><td>定义2D移动</td></tr>
  <tr><td>translate3d(x,y,z)</td><td>定义3D移动</td></tr>
  <tr><td>translateX(x)</td><td>定义x轴的移动</td></tr>
  <tr><td>translateY(x)</td><td>定义y轴的移动</td></tr>
  <tr><td>translateZ(x)</td><td>定义z轴的3D移动</td></tr>
  <tr><td>scale(x,y)</td><td>定义2D缩放</td></tr>
  <tr><td>scale3d(x,y,z)</td><td>定义3D缩放</td></tr>
  <tr><td>scaleX(x)</td><td>通过设置x轴的值定义缩放</td></tr>
  <tr><td>scaleY(x)</td><td>通过设置y轴的值定义缩放</td></tr>
  <tr><td>scaleZ(x)</td><td>通过设置z轴的值定义3D缩放</td></tr>
  <tr><td>rotate(angle)</td><td>定义2D旋转</td></tr>
  <tr><td>rotate3d(x,y,z,angle)</td><td>定义3D旋转</td></tr>
  <tr><td>rotateX(angle)</td><td>定义沿着x轴的3D旋转</td></tr>
  <tr><td>rotateY(angle)</td><td>定义沿着y轴的3D旋转</td></tr>
  <tr><td>rotateZ(angle)</td><td>定义沿着z轴的3D旋转</td></tr>
  <tr><td>skew(x-angle,y-angle)</td><td>定义沿着x和y轴的2D倾斜</td></tr>
  <tr><td>skewX(angle)</td><td>定义沿着x轴的2D倾斜</td></tr>
  <tr><td>skewY(angle)</td><td>定义沿着y轴的2D倾斜</td></tr>
  <tr><td>none</td><td>定义不进行转换</td></tr>
</table>

### 2 css3 transform-origin属性
元素转换的中心点默认为元素中心点的位置，使用transform-origin属性可以改变转换中心点的位置。  
2D转换元素能够改变转换中心点的 x 和 y 轴位置，3D转换元素还能改变转换中心点的Z 轴位置。 
必须与transform属性一同使用。  
语法：transform-origin : x-axis y-axis z-axis  
默认：transform-origin : 50% 50% 0  
x-axis可能的值：left | center | right | 数值(px) | 百分比(%)  
y-axis可能的值：top | center | bottom | 数值(px) | 百分比(%)  
z-axis可能的值：数值(px)

### 3 倾斜
```
<!DOCTYPE html>
<html>
    <head>
        <title></title>
        <style>
            #square{
                background:blue;
                width:200px;
                height:200px;
                position:absolute;
                left:100px;
                top:100px;
                transform:skew(5deg, 5deg);
            }
        </style>
    </head>
    <body>
        <div id="square"></div>
    </body>
</html>
```

### 4 移动
```
<!DOCTYPE html>
<html>
    <head>
        <title></title>
        <style>
            #square{
                background:blue;
                width:200px;
                height:200px;
                position:absolute;
                left:0;
                top:0;
                transform:translate(50px, 50px);
            }
        </style>
    </head>
    <body>
        <div id="square"></div>
    </body>
</html>
```

### 5 缩放
```
<!DOCTYPE html>
<html>
    <head>
        <title></title>
        <style>
            #circle{
                background:blue;
                width:200px;
                height:200px;
                border-radius:50%;
                position:absolute;
                left:100px;
                top:100px;
                transform:scale(0.5, 2);
            }
        </style>
    </head>
    <body>
        <div id="circle"></div>
    </body>
</html>
```

### 6 旋转
```
<!DOCTYPE html>
<html>
    <head>
        <title></title>
        <style>
            #circle{
                background:blue;
                width:200px;
                height:200px;
                border-radius:50%;
                position:absolute;
                left:100px;
                top:100px;
            }
            #track{
                width:300px;
                height:300px;
                border:1px solid grey;
                border-radius:50%;
                position:absolute;
                left:50px;
                top:50px;
            }
            #plane{
                width:60px;
                height:30px;
                background:grey;
                border-radius:10px;
                position:absolute;
                left:170px;
                top:35px;
                transform-origin:center 165px;
                transform:rotate(45deg);
            }
        </style>
    </head>
    <body>
        <div id="circle"></div>
        <div id="track"></div>
        <div id="plane"></div>
    </body>
</html>
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
