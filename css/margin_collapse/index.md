[<< 回到主页](http://suzy1993.github.io/misszy/)

## CSS 外边距合并

### 1 外边距合并
外边距合并指的是，当两个垂直外边距相遇时，它们将形成一个外边距。

### 2 发生外边距合并的三种情况
#### 2.1 相邻兄弟元素
eg1：
```
#div1 {
    width: 100px;
    height: 100px;
    margin-bottom: 50px;
    background: red;
}
#div2 {
    width: 100px;
    height: 100px;
    margin-top: 20px;
    background: yellow;
}
```
当margin 都是正值时，取的是其中较大的。
两个div的间距，不是div1的下边距与div2的上边距的和70px，而是两者中的较大者50px。  
eg2：
```
#div1 {
    width: 100px;
    height: 100px;
    margin-bottom: -50px;
    background: red;
}
#div2 {
    width: 100px;
    height: 100px;
    margin-top: -20px;
    margin-left: 50px;
    background: yellow;
}
```
当 margin 都是负值时，取的是其中绝对值较大的，然后从0位置负向位移。
两个div的间距，是两者中绝对值较大的-50px。  
eg3：
```
#div1 {
    width: 100px;
    height: 100px;
    margin-bottom: -20px;
    background: red;
}
#div2 {
    width: 100px;
    height: 100px;
    margin-top: 50px;
    background: yellow;
}
```
当 margin 有正有负时，先取出负margin 中绝对值中最大的，然后和正margin值中最大的margin相加。
两个div的间距，是(-20px) + 50px = 30px。

#### 2.2 块级父元素与其第一个/最后一个子元素
块级父元素中，不存在border-top、padding-top、inline content、清除浮动这四条属性，那么这个块级元素和其第一个子元素的上边距就会挨到一起，即发生上外边距合并现象，换句话说，此时这个父元素的外边距将直接变成这个父元素和其第一个子元素的margin-top的较大者。
类似的，若块级父元素的margin-bottom与它的最后一个子元素的margin-bottom之间没有父元素的border-bottom、padding-bottom、inline content、height、min-height、max-height分隔时，就会发生下外边距合并现象。
```
#div1 {
    width: 100px;
    height: 100px;
    margin-top: 50px;
    background: red;
}
#div2 {
    width: 50px;
    height: 50px;
    margin-top: 20px;
    background: yellow;
}
```
父div的外边距将为其与子div的margin-top的较大者50px。

#### 3.3 空块元素
如果存在一个空的块级元素，其border、padding、inline content、height、min-height都不存在，此时它的上下外边距将会合并。
```
p {
    background:red;
}
div{
    margin-top: 20px;
    margin-bottom: 50px;
}
```
段落一与段落二间的距离将为上边距与下边距中的较大者50px。

### 3 不发生外边距合并的两种情况
#### 3.1 设置了overflow: hidden属性的元素，不和它的子元素发生margin合并
```
#div1 {
    width: 100px;
    height: 100px;
    margin-top: 50px;
    background: red;
    overflow: hidden;
}
#div2 {
    width: 50px;
    height: 50px;
    margin-top: 20px;
    background: yellow;
}
```
父div的外边距没有与子div的外边距合并，父元素与子元素的间距为20px。

#### 3.2 只有块级元素之间的垂直外边距才会发生外边距合并。行内元素、浮动元素或绝对定位元素之间的垂直外边距不会合并
```
#div1 {
    width: 100px;
    height: 100px;
    margin-bottom: 20px;
    background: red;
}
#div2 {
    width: 100px;
    height: 100px;
    margin-top: 50px;
    background: yellow;
    float: left;
}
```
两个div的间距，是div1的下边距与div2的上边距的和70px，而不是两者中的较大者50px。

[<< 回到主页](http://suzy1993.github.io/misszy/)
