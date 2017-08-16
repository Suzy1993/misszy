[<< 回到主页](http://suzy1993.github.io/misszy/)

## CSS position定位

### 1 静态定位（static）
left、right、top、bottom、z-index等属性无效。

### 2 绝对定位（absolute）
将被赋予绝对定位的元素从文档流中拖出，使用left、right、top、bottom等属性相对于其最接近的一个最有定位设置的父级元素进行绝对定位，如果元素的父级没有设置定位属性，则依据 body 元素左上角作为参考进行定位。绝对定位元素可层叠，层叠顺序可通过 z-index 属性控制，z-index值为无单位的整数，大的在上面，可以有负值。
绝对定位的定位方法：如果它的父元素设置了除static之外的定位，比如position:relative或position:absolute及position:fixed，那么它就会相对于它的父元素来定位，位置通过left , top ,right ,bottom属性来规定，如果它的父元素没有设置定位，那么就得看它父元素的父元素是否有设置定位，如果还是没有就继续向更高层的祖先元素类推，总之它的定位就是相对于设置了除static定位之外的定位的第一个祖先元素，如果所有的祖先元素都没有以上三种定位中的其中一种定位，那么它就会相对于文档body来定位（并非相对于浏览器窗口，相对于窗口定位的是fixed）。
```
<head>
    <style type="text/css">
        .box {
            background: red;
            width: 100px;
            height: 100px;
            float: left;
            margin: 5px;
        }
        #two {
            position: absolute;
            top: 50px;
            left: 50px;
        }
    </style>
</head>
<body>
    <div class="box" id="one">One</div>
    <div class="box" id="two">Two</div>
    <div class="box" id="three">Three</div>
    <div class="box" id="four">Four</div>
</body>
```
将id="two"的div定位到距离<body>的顶部和左侧分别50px的位置。会改变其他元素的布局，不会在此元素本来位置留下空白。

### 3 相对定位（relative）
相对定位元素不可层叠，依据left、right、top、bottom等属性在正常文档流中偏移自身位置。同样可以用z-index分层设计。
```
<head>
    <style type="text/css">
        .box {
            background: red;
            width: 100px;
            height: 100px;
            float: left;
            margin: 5px;
        }
        #two {
            position: relative;
            top: 50px;
            left: 50px;
        }
    </style>
</head>
<body>
    <div class="box" id="one">One</div>
    <div class="box" id="two">Two</div>
    <div class="box" id="three">Three</div>
    <div class="box" id="four">Four</div>
</body>
```
将id="two"的div定位到距离它本来位置的顶部和左侧分别50px的位置。不会改变其他元素的布局，会在此元素本来位置留下空白。

### 4 固定定位（fixed）
固定定位与绝对定位类似，但它是相对于浏览器窗口定位，并且不会随着滚动条进行滚动。
固定定位的最常见的一种用途是在页面中创建一个固定头部、固定脚部或者固定侧边栏，不需使用margin、border、padding。

### 5 绝对定位 VS 相对定位
绝对定位好像把不同元素安排到了一栋楼的不同楼层（除首层，文本流放在首层），它们之间互不影响；相对定位元素在首层，与文本流一起存放，它们之间互相影响。
被设置了绝对定位的元素，在文档流中是不占据空间的，如果某元素设置了绝对定位，那么它在文档流中的位置会被删除，它浮了起来，其实设置了相对定位也会让元素浮起来，但它们的不同点在于，相对定位不会删除它本身在文档流中占据的空间，其他元素不可以占据该空间，而绝对定位则会删除掉该元素在文档流中的位置，使其完全从文档流中抽了出来，其他元素可以占据其空间，可以通过z-index来设置它们的堆叠顺序。

[<< 回到主页](http://suzy1993.github.io/misszy/)
