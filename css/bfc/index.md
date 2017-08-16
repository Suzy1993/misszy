[<< 回到主页](http://suzy1993.github.io/misszy/)

## CSS BFC

### 1 BFC
Block Formatting Context，即块级格式上下文。
创建了 BFC的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，也不会受外部影响。
只有块级元素可以参与创建BFC，它规定了内部的块级元素如何布局。

### 2 BFC的特性
BFC具有普通容器所不具有的特性。
* 内部的Box会在垂直方向，从顶部开始一个接一个地放置。
* Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生叠加
* 每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
* BFC的区域不会与float box叠加。
* BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之亦然。
* 计算BFC的高度时，浮动元素也参与计算。

### 3 创建BFC
* body根元素。
* float设置为除了none以外的值。
* position设置为absolute，fixed。
* display设置为table-cell，table-caption，inline-block, flex, inline-flex。
* overflow设置为除了visible以外的值，即hidden，auto，scroll。

### 4 BFC的应用
1）BFC可以包含浮动的元素，清除浮动只需要将父元素变成一个BFC即可，常用的方法是给父元素设置overflow:hidden。
浮动布局经常遇到的一个问题：对子元素设置浮动后，父元素会发生高度塌陷，其高度变为0。
根据BFC规则——计算BFC的高度时，浮动元素也会参与计算，解决该问题的方法是把父元素变成一个BFC，常用的是给父元素设置overflow:hidden。

2）同一BFC下外边距会发生合并，若想避免外边距合并，可以将其放在不同的BFC容器中。
```
<head>
    div {
        width:100px;
        height:100px;
        background:red;
        margin:100px;
    }
</head>
<body>
    <div></div>
    <div></div>
</body>
```
两个div都处于同一个BFC容器中（body根元素），因此第一个div的下边距与第二个div的上边距发生合并。
若想避免外边距的合并，可以将其放在不同的BFC容器中。
```
<head>
    p {
        width:100px;
        height:100px;
        background:red;
        margin:100px;
    }
</head>
<body>
    <div class="container">
        <p></p>
    </div>
    <div class="container">
        <p></p>
    </div>
</body>
```

3）BFC可以阻止元素被浮动元素覆盖，创建自适应两栏布局，左边宽度固定，右边自适应宽度。
左图片+右文字的两栏结构：右边元素有一部分被浮动元素覆盖（但是文本信息不会被浮动元素覆盖，文字环绕）。
```
<head>
    <style type="text/css">
        *{
            font-size: 14px;
        }
        .box {
            width:300px;
            border: 1px solid #000;
        }
        img {
            float: left;
        }
    </style>
</head>
<body>
    <div>
        <div class="box">
            <img src="images/logo-green.gif">
            <div class="info">端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端</div>
        </div>
    </div>
</body>
```
若想避免元素被覆盖，可以触发右边元素的BFC特性，在第二个元素中加入overflow: hidden。
```
.info {
    overflow:hidden;
}
```
改变图片的大小，两栏布局的结构依然没有改变，实现两栏自适应布局。

4）position设置为absolute，fixed。

[<< 回到主页](http://suzy1993.github.io/misszy/)
