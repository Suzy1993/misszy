[<< 回到主页](http://suzy1993.github.io/misszy/)

## CSS3 新特性——text-shadow & box-shadow & box-reflect

### 1 text-shadow
向文本添加一个或多个阴影。
语法：text-shadow: x-shadow y-shadow distance color;
<table>
  <tr><th>值</th><th>描述</th></tr>
  <tr><td>x-shadow</td><td>必需。水平阴影的位置。允许负值。</td></tr>
  <tr><td>y-shadow</td><td>必需。垂直阴影的位置。允许负值。</td></tr>
  <tr><td>distance</td><td>可选。模糊的距离。测试.</td></tr>
  <tr><td>color</td><td>可选。阴影的颜色.</td></tr>
</table>
eg：
```
text-shadow: 5px 5px 5px red;
```

### 2 box-shadow
向框添加一个或多个阴影。
语法：box-shadow: x-shadow y-shadow distance size color inset/outset;
<table>
  <tr><th>值</th><th>描述</th></tr>
  <tr><td>x-shadow</td><td>必需。阴影水平偏移量，可正可负，正值表示阴影在右边，负值表示阴影在左边。</td></tr>
  <tr><td>y-shadow</td><td>必需。阴影垂直偏移量，可正可负，正值表示阴影在上边，负值表示阴影在下边。</td></tr>
  <tr><td>distance</td><td>可选。阴影模糊距离。只能为正值，值为0时，表示阴影不具有模糊效果，值越大阴影的边缘就越模糊。</td></tr>
  <tr><td>size</td><td>可选。阴影扩展半径。可正可负，正值表示整个阴影都延展扩大，负值表示缩小。</td></tr>
  <tr><td>color</td><td>可选。阴影的颜色。</td></tr>
  <tr><td>inset</td><td>可选。将外部阴影 (outset) 改为内部阴影。</td></tr>
</table>
eg：
```
box-shadow: 10px 10px 10px red;
```

### 3 box-reflect
向框添加一个或多个倒影。
#### 3.1 direction
定义倒影的方向，取值包括：
* above：倒影在对象的上边。
* below：倒影在对象的下边。
* left：倒影在对象的左边。
* right：倒影在对象的右边。

#### 3.2 offset
定义倒影与对象之间的间隔，可正可负，默认为0。取值包括：
* 长度值
* 百分比（根据对象的尺寸进行确定）

#### 3.3 mask-box-image
定义遮罩图像，该图像将覆盖投影区域，默认为无遮罩。
* <url>：使用绝对或相对地址指定遮罩图像。
* <linear-gradient>：使用线性渐变创建遮罩图像。
* <radial-gradient>：使用径向(放射性)渐变创建遮罩图像。
* <repeating-linear-gradient>：使用重复的线性渐变创建背遮罩像。
* <repeating-radial-gradient>：使用重复的径向(放射性)渐变创建遮罩图像。
eg：
```
width:100px;
height:100px;
background:-webkit-linear-gradient(top,red,yellow,green);
-webkit-box-reflect:below 10px -webkit-linear-gradient(transparent,pink 50%,blue);
```
说明：遮罩只会把遮罩图里透明像素所对应的原图部分进行隐藏。

[<< 回到主页](http://suzy1993.github.io/misszy/)
