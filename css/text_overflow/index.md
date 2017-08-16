[<< 回到主页](http://suzy1993.github.io/misszy/)

## CSS 单、多行文本溢出

### 1 单行文本溢出
#### 1.1 截断溢出文本
同时设置以下属性：
```
width:*px;
overflow: hidden;
text-overflow:clip;
white-space: nowrap;
```

#### 1.2 溢出文本显示省略号
同时设置以下属性：
```
width:*px;
overflow: hidden;
text-overflow:ellipsis;
white-space: nowrap;
```

### 2 多行文本溢出
同时设置以下属性：
```
width:*px;
display:-webkit-box;
overflow:hidden;
-webkit-box-orient:vertical;
-webkit-line-clamp:*;
```
-webkit-line-clamp用来限制在一个块元素显示文本的行数，需要结合WebKit属性display: -webkit-box和-webkit-box-orient属性。  
eg：
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
        <style type="text/css">
            .oneLineEllipsis {
                            width:500px;
                            border:1px solid red;
                overflow:hidden;
                white-space:nowrap;
                text-overflow:ellipsis;
            }
            .oneLineClip {
                            width:500px;
                            border:1px solid red;
                overflow:hidden;
                white-space:nowrap;
                text-overflow:clip;
            }
            .multiLineEllipsis {
                            width:500px;
                            border:1px solid red;
                display:-webkit-box !important;
                overflow:hidden;
                -webkit-box-orient:vertical;
                -webkit-line-clamp:3;
            }
        </style>
    </head>
    <body>
        <p class="oneLineEllipsis">
                     前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端
              </p>
        <p class="oneLineClip">
                     前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端
              </p>
              <p class="multiLineEllipsis">
                     前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端前端
              </p>
       </body>
</html>
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
