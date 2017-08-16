[<< 回到主页](http://suzy1993.github.io/misszy/)

## CSS 单位px & pt & em & rem

### 1 px
像素（Pixel）。相对长度单位。像素px是相对于显示器屏幕分辨率而言的。  
屏幕设备物理上能显示出的最小的一个点，这个点不是固定宽度的，不同设备上点的长宽比例有可能会不同，也不一定是1:1的正方形。如，A显示器上1px宽=1mm，但B显示器1px宽=2mm，若定义一个div宽度为100px，A显示器上看该div是10cm，B显示器上看该div是20cm。

### 2 pt
磅，印刷业上常使用的单位，等于1/72英寸，表示绝对长度，一般用于页面打印排版。  
px和pt转换规则：pt = px * 3 / 4。

### 3 em
em实质是一个相对值，而非具体的数值，因此需要一个参考点，一般都是以<body>的font-size为基准，也即body元素的任何子元素的em值都等于font-size设置的大小。  
任意浏览器的默认字体高都是16px。所有未经调整的浏览器都符合: 1em=16px。那么12px=0.75em,10px=0.625em。为了简化font-size的换算，需要在css中的body选择器中声明Font-size=62.5%，这就使em值变为16px*62.5%=10px, 这样12px=1.2em, 10px=1em, 也就是说只需要将你的原来的px数值除以10，然后换上em作为单位就行了。  
由于em是相对于父元素计算的，因此多层嵌套情况下，每层都会从父元素继承em值，因此em的值并不是固定的。  
HTML：
```
<body>
    <div id="div1"><!--14 * 1.2 = 16.8px-->
        <div id="div2"><!--16.8 * 1.2 = 20.16px-->
            <div id="div3"><!--20.16px * 1.2 = 24.192px-->
            </div>
        </div>
    </div>
</body>
```
CSS：
```
body {
    font-size: 14px;
}
div {
    font-size: 1.2em;
}
```

### 4 rem
em的问题：子元素设置，需要知道父元素的大小。  
解决：rem相对于根元素<html>，因此，只需要在根元素上确定一个参考值即可。  
HTML：
```
<body>
    <div id="div1"><!--14 * 1.2 = 16.8px-->
        <div id="div2"><!--14 * 1.2 = 16.8px-->
            <div id="div3"><!--14 * 1.2 = 16.8px-->
            </div>
        </div>
    </div>
</body>
```
CSS：
```
html {
    font-size: 14px;
}
div {
    font-size: 1.2rem;
}
```
除了IE6-IE8，其它浏览器都支持em和rem。

[<< 回到主页](http://suzy1993.github.io/misszy/)
