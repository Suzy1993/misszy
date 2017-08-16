[<< 回到主页](http://suzy1993.github.io/misszy/)

## CSS 清除浮动

### 1 为什么会出现浮动？
浮动的产生的最根本的原因是为了实现文字环绕效果。

### 2 什么时候需要清除浮动？
清除浮动是为了清除使用浮动元素产生的影响。浮动的元素，高度会塌陷，而高度的塌陷使页面后面的布局不能正常显示，如背景色不能显示等。

### 3 清除浮动的方法
#### 3.1 使用clear属性
使用空标签清除浮动：在需要清除浮动的父级元素内部的所有浮动元素后添加一个空标签（理论上可以是任何标签，但常用<div>和<p>）清除浮动，并为其定义CSS代码clear:both。
缺点：增加了无意义的结构元素。
```
<html>
    <head>
        <style type="text/css">
            #layout{
                background: red;
            }
            #left{
                float: left;
                width: 20%;
                height: 200px;
                background: blue;
                line-height: 200px;
             }
            #right{
                float: right;
                width: 30%;
                height: 80px;
                background: yellow;
                line-height: 80px;
            }
            #clear{
                clear: both;
            }
        </style>
    </head>
    <body>
        <div id="layout">
            <div id="left">Left</div>
            <div id="right">Right</div>
            <div id="clear"></div>
        </div>
    </body>
</html>
```

#### 3.2 使用overflow属性
使用overflow清除浮动：只需在需要清除浮动的元素中定义CSS代码overflow:auto或overflow:hidden即可。
优点：有效地解决了使用空标签清除浮动而不得不增加无意义结构元素的弊端。
```
<html>
    <head>
        <style type="text/css">
            #layout{
                overflow: auto;
                background: red;
            }
            #left{
                float: left;
                width: 20%;
                height: 200px;
                background: blue;
                line-height: 200px;
            }
            #right{
                float: right;
                width: 30%;
                height: 80px;
                background: yellow;
                line-height: 80px;
            }
        </style>
    </head>
    <body>
        <div id="layout">
            <div id="left">Left</div>
            <div id="right">Right</div>
        </div>
    </body>
</html>
```

#### 3.3 对父元素使用:after伪元素
#### 3.3.1 方法一： 对父元素使用:after伪元素，设置display:table
display:table 使生成的元素以块级表格显示，占满剩余空间。
通过content: " "生成内容作为最后一个元素，至于content里面是什么都是可以的，CSS经典的是 content:"."，有些版本可能content里面内容为空。
通过分析发现，除了clear：both用来闭合浮动的，其他代码无非都是为了隐藏掉content生成的内容。
以上设置是现代浏览器方案，不支持IE6/7，*zoom: 1就是为了兼容IE6/7。
```
<html>
    <head>
        <style type="text/css">
            #layout{
                background: red;
                *zoom: 1;
            }
            #layout:after {
                content: " ";
                display: table;
                clear: both;
            }
            #left{
                float: left;
                width: 20%;
                height: 200px;
                background: blue;
                line-height: 200px;
             }
            #right{
                float: right;
                width: 30%;
                height: 80px;
                background: yellow;
                line-height: 80px;
            }
        </style>
    </head>
    <body>
        <div id="layout">
            <div id="left">Left</div>
            <div id="right">Right</div>
        </div>
    </body>
</html>
```

#### 3.3.2 方法二：对父元素使用:after伪元素，设置display:block
display: block使生成的元素以块级元素显示，占满剩余空间。
height: 0避免生成内容破坏原有布局的高度。
visibility: hidden使生成的内容不可见，并允许可能被生成内容盖住的内容可以进行点击和交互。
通过content: " "生成内容作为最后一个元素，至于content里面是什么都是可以的，CSS经典的是 content:"."，有些版本可能content里面内容为空。
通过分析发现，除了clear：both用来闭合浮动的，其他代码无非都是为了隐藏掉content生成的内容。
```
<html>
    <head>
        <style type="text/css">
            #layout{
                background: red;
            }
            #layout:after{
                content: ".";
                visibility: hidden;
                display: block;
                height: 0;
                clear: both;
            }
            #left{
                float: left;
                width: 20%;
                height: 200px;
                background: blue;
                line-height: 200px;
            }
            #right{
                float: right;
                width: 30%;
                height: 80px;
                background: yellow;
                line-height: 80px;
            }
        </style>
    </head>
    <body>
        <div id="layout">
            <div id="left">Left</div>
            <div id="right">Right</div>
        </div>
    </body>
</html>
```

[<< 回到主页](http://suzy1993.github.io/misszy/)

