[<< 回到主页](http://suzy1993.github.io/misszy/)

## CSS 水平居中 & 垂直居中

### 1 水平居中
#### 1.1 行内元素
设置父元素：
text-align: center;
```
<head>
    <style type="text/css">
        #out {
            width: 200px;
            height: 100px;
            background-color: red;
            text-align: center;
        }
    </style>
</head>
<body>
    <div id="out">
        <span>CSS</span>
    </div>
</body>
```

#### 1.2 确定宽度的块级元素
设置子元素：
width: 百分比或数值;
margin: 0 auto;
注意：必须设置width，否则margin:0 auto不能实现水平居中。
```
<head>
    <style type="text/css">
        #out {
            width: 400px;
            height: 200px;
            background: red;
        }
        #in {
            width: 50%;
            height: 50%;
            background: yellow;
            margin:0 auto;
        }
    </style>
</head>
<body>
    <div id="out">
        <div id="in"></div>
    </div>
</body>
```

#### 1.3 不确定宽度的块级元素
1）设置父元素和子元素
设置父元素：
text-align: center;
设置子元素：
display: inline-block或inline;
```
<head>
    <style type="text/css">
        #out {
            width: 200px;
            height: 100px;
            background-color: red;
            text-align: center;
        }
        #in {
            background-color: yellow;
            display: inline-block;
        }
    </style>
</head>
<body>
    <div id="out">
        <div id="in">Hello</div>
    </div>
</body>
```
2）设置子元素
display: table;
margin: 0 auto;
```
<head>
    <style type="text/css">
        #out {
            width: 200px;
            height: 100px;
            background-color: red;
        }
        #in {
            background-color: yellow;
            display: table;
            margin: 0 auto;
        }
    </style>
</head>
<body>
    <div id="out">
        <div id="in">Hello</div>
    </div>
</body>
```

### 2 垂直居中
#### 2.1 行内元素
设置子元素：
line-height: 父元素height;
```
<head>
    <style type="text/css">
        #out {
            width: 200px;
            height: 100px;
            background-color: red;
        }
        span {
            line-height: 100px;
        }
    }
    </style>
</head>
<body>
    <div id="out">
        <span>CSS</span>
    </div>
</body>
```

#### 2.2 非行内元素
设置父元素：
position: relative;
设置子元素：
position: absolute;
margin-top: (父元素height-子元素height) / 2;
```
<head>
    <style type="text/css">
        #out {
            position: relative;
            width: 200px;
            height: 100px;
            background: red;
        }
        #in {
            position: absolute;
            width: 100px;
            height: 50px;
            background: yellow;
            margin-top: 25px;
        }
    </style>
</head>
<body>
    <div id="out">
        <div id="in"></div>
    </div>
</body>
```

#### 2.3 水平和垂直居中
#### 2.3.1 在浏览器窗口中水平和垂直居中
width: 数值（不能是百分比）;
height: 数值（不能是百分比）;
position: fixed;
left: 50%;
top: 50%;
margin-top: -height/2;
margin-left: -width/2;
```
<head>
    <style type="text/css">
        #out {
            position: fixed;
            width: 800px;
            height: 400px;
            background: red;
            margin: -200px 0 0 -400px;
            left: 50%;
            top:50%;
        }
    }
    </style>
</head>
<body>
    <div id="out"></div>
</body>
```

#### 2.3.2 子元素在父元素中水平和垂直居中
#### 2.3.2.1 利用 flex 布局，实际使用时应考虑兼容性
设置父元素：
display: flex;
align-items: center;   /*垂直居中*/
justify-content: center;  /*水平居中*/
```
<head>
    <style type="text/css">
        #out{
            width: 400px;
            height: 200px;
            background: red;
            display: flex;
            align-items: center;                /* 垂直居中 */
            justify-content: center;            /* 水平居中 */
        }
        #in{
            width: 200px;
            height: 100px;
            background: yellow;
        }
    </style>
</head>
<body>
    <div id="out">
        <div id="in"></div>
    </div>
</body>
```

#### 2.3.2.2 未知子元素的宽高，利用transform属性
设置父元素：
position: fixed;
设置子元素：
width: **%;
height: **%;
position: absolute或relative;
left: 50%;
top: 50%;
transform: translate(-50%,-50%);
```
<head>
    <style type="text/css">
        #out {
            position: fixed;
            width: 400px;
            height: 200px;
            background: red;
        }
        #in {
            width: 50%;
            height: 50%;
            background: yellow;
            position: absolute;
            transform: translate(-50%,-50%);
            top: 50%;
            left: 50%;
        }
    </style>
</head>
<body>
    <div id="out">
        <div id="in"></div>
    </div>
</body>
```

#### 2.3.2.3 已知子元素的宽高
设置父元素：
position: fixed;
设置子元素：
width: 数值;
height: 数值;
padding: (可有可无);
position: absolute或relative;
left: 50%;
top: 50%;
margin-left: -(width + 2 * padding) / 2;
margin-top: -(height + 2* padding) / 2;
```
<head>
    <style type="text/css">
        #out {
            position: fixed;
            width: 400px;
            height: 200px;
            background: red;
        }
        #in {
            width: 100px;
            height: 50px;
            background: yellow;
            padding: 20px;
            position: absolute;
            top: 50%;
            left: 50%;
            margin-left: -70px; /* (width + padding)/2 */
            margin-top: -45px; /* (height + padding)/2 */
        }
    </style>
</head>
<body>
    <div id="out">
        <div id="in"></div>
    </div>
</body>
```

[<< 回到主页](http://suzy1993.github.io/misszy/)

