[<< 回到主页](http://suzy1993.github.io/misszy/)

## CSS 等高布局

### 1 典型网页结构
两列结构，左列用来导航，右列用来显示内容，左右两列的高度都不固定。

### 2 需求
两列的高度保持一致，避免出现“断层”的效果。

### 3 padding补偿法
实现CSS等高布局的方法有几种，其中浏览器兼容最好的方法是padding补偿法。
#### 3.1 步骤
* 把列的padding-bottom设为一个足够大的值。
* 把列的margin-bottom设为一个与padding-bottom的正值相抵消的负值。
* 父容器设置溢出隐藏。

#### 3.2 原理
当任一列高度增加了，则父容器的高度被撑到最高那列的高度，其他比其矮的列则会用它们的padding-bottom来补偿这部分高度差。
注：padding-bottom的值取决于实际情况，若不确定，可以设大些。
```
<!DOCTYPE html>
<html>
    <head>
        <title></title>
        <style>
            #container{
                width:800px;
                border:5px solid grey;
                overflow:hidden;
            }
            #left{
                width:50%;
                float:left;
                background:blue;
                padding-bottom:2000px;
                margin-bottom:-2000px;
            }
            #right{
                width:50%;
                float:left;
                background:red;
                padding-bottom:2000px;
                margin-bottom:-2000px;
            }
        </style>
    </head>
    <body>
        <div id="container">
            <div id="left">left</div>
            <div id="right">right</div>
        </div>
    </body>
</html>
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
