[<< 回到主页](http://suzy1993.github.io/misszy/)

## CSS 负margin

### 1 等高布局
1）把列的padding-bottom设为一个足够大的值  
2）把列的margin-bottom设为一个与padding-bottom的正值相抵消的负值  
3）父容器设置溢出隐藏
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

### 2 去除最右元素的右边距
当父元素的宽度固定时，每一行的最右端的元素的右边距多余，利用负margin可以去除多余的右边距。
```
<!DOCTYPE html>
<html>
  <head>
      <title></title>
      <style>
          body,ul,li{
              padding:0;
              margin:0;
          }
          ul,li{
              list-style:none;
          }
          #container{
              width:320px;
              height:100px;
              background:red;
          }
          #container ul{
              margin-right:-10px;
          }
          #container ul li{
              width:100px;
              height:100px;
              background:grey;
              margin-right:10px;
              float:left;
          }
      </style>
    </head>
    <body>
        <div id="container">
            <ul>
                <li>First</li>
                <li>Second</li>
                <li>Third</li>
            </ul>
        </div>
    </body>
</html>
```

### 3 左右栏宽度固定，中间栏宽度自适应，要求中间栏优先加载的三栏布局
左栏设置margin-left: -100%，右栏设置margin-left为右栏width的负值，中间栏设置margin-left为左栏width。
```
<!DOCTYPE html>
<html>
    <head>
        <title></title>
        <style>
             body{
                margin:0;
                padding:0;
            }
            .middle{
                float:left;
                width:100%;
            }
            .content{
                margin:0 110px;
                background:grey;
                height:100px;
            }
            .left,.right{
                float:left;
                width:100px;
                height:100px;
                background:red;
            }
            .left{
                margin-left:-100%;
            }
            .right{
                margin-left:-100px;
            }
        </style>
    </head>
    <body>
        <div class="middle">
            <div class="content">Middle</div>
        </div>
        <div class="left">Left</div>
        <div class="right">Right</div>
    </body>
</html>
```

### 4 水平垂直居中
使用绝对定位将div定位到body的中心，使用负margin(div宽高的一半)，将div的中心拉回到body的中心，实现水平垂直居中的效果。
```
<!DOCTYPE html>
<html>
    <head>
        <title></title>
        <style>
            body{margin:0;padding:0;}
            #content{
                width:800px;
                height:400px;
                background:red;
                position:absolute;
                left:50%;
                top:50%;
                margin-left:-400px;
                margin-top:-200px;
            }
        </style>
    </head>
    <body>
        <div id="content"></div>
    </body>
</html>
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
