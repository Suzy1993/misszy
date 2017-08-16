[<< 回到主页](http://suzy1993.github.io/misszy/)

## CSS 等比例分割父级容器

以三等分为例：
```
<div class="parent">
    <div class="child"></div>
    <div class="child"></div>
    <div class="child"></div>
</div>
```

### 1 方法一：浮动布局+百分比
```
.parent{
    width:600px;
    height:600px;
}
.child{
    width:33.3%;
    height:100%;
    float:left;
}
```

### 2 方法二：父元素display:table+ 子元素display:table-cell
```
.parent{
    width:600px;
    height:600px;
    display:table;
}
.child{
    display:table-cell;
}
```

### 3 方法三：CSS3 flex布局
```
.parent{
    width:600px;
    height:600px;
    display:flex;
}
.child{
    flex:1;
}
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
