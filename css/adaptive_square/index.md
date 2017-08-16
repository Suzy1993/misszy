[<< 回到主页](http://suzy1993.github.io/misszy/)

## 纯CSS 实现自适应浏览器宽度/高度的正方形

JavaScript实现自适应浏览器宽度，可以监听onresize事件。

纯CSS实现自适应浏览器宽度的正方形有以下三种方法：
### 1 方案一：CSS3 vw 单位
css3 中新增了一组相对于可视区域百分比的长度单位vw、vh、vmin、vmax。其中vw是相对于视口宽度百分比的单位，1vw = 1% viewport width，vh是相对于视口高度百分比的单位，1vh = 1% viewport height；vmin是相对当前视口宽高中较小的一个的百分比单位，同理 vmax是相对当前视口宽高中较大的一个的百分比单位。
```
#square{
    width:30%;
    height:30vw;
    background:red;
}
```
1）类比：纯CSS实现自适应浏览器高度的正方形只需要设置width的单位为vh即可。
2）优点：简洁方便。
3）缺点：浏览器兼容不好。

### 2 方案二：设置垂直方向的padding撑开容器
由于margin, padding 的百分比数值是相对父元素的宽度计算的，只需将元素垂直方向的一个padding值设定为与width相同的百分比就可以制作出自适应正方形了。
但要注意，仅仅设置padding-bottom是不够的，若向容器添加内容，内容会占据一定高度，为了解决这个问题，需要设置height: 0。
```
#square{
    width:30%;
    height:0;
    padding-bottom: 30%;
    background:red;
}
```
1）优点：简洁明了，且兼容性好。
2）缺点：会导致在元素上设置的max-width属性失效（max-height不收缩）。

### 3 利用伪元素的margin(padding)-top 撑开容器
max-width属性失效的原因是：max-width属性只限制于width，也就是只会对元素的content width起作用。
解决方法是：用一个子元素撑开content部分的高度，从而使max-height属性生效。
首先需要设置伪元素，其内容为空，margin-top设置为100%。
但要注意，若使用垂直方向上的margin撑开父元素，仅仅设置伪元素是不够的，这涉及到margin collapse外边距合并的概念，由于容器与伪元素在垂直方向发生了外边距合并，所以撑开父元素高度并没有出现，解决方法是在父元素上触发BFC：设置overflow:hidden。
```
#square{
    width:30%;
    background:red;
    overflow:hidden;
    max-width:200px;
}
#square:after{
    content: '';
    display: block;
    margin-top:100%;
}
```
若使用垂直方向上的padding撑开父元素，则不需要触发BFC。
```
#square{
    width:30%;
    background:red;
    max-width:200px;
}
#square:after{
    content: '';
    display: block;
    padding-top:100%;
}
```

[<< 回到主页](http://suzy1993.github.io/misszy/)

