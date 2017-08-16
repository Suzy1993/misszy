[<< 回到主页](http://suzy1993.github.io/misszy/)

## CSS 两栏 & 三栏自适应布局

### 1 左侧固定宽度，右侧自适应
#### 1.1 方法一：左侧设置float: left，右侧设置margin-left为左侧的宽度
注意：右侧不能设置float: left。
```
<!doctype html>
<html>
  <head>
  <style type="text/css">
    #left {
      width: 500px;
      height: 300px;
      float: left;
      background: #DDD;
    }
    #right {
      height: 300px;
      margin-left: 500px;
      background: #AAA;
    }
  </style>
  </head>
  <body>
    <div id="left"></div>
    <div id="right"></div>
  </body>
</html>
```

#### 1.2 方法二：左侧设置float: left，右侧设置overflow: hidden
利用的是创建一个新的BFC（块级格式化上下文）来防止文字环绕的原理来实现的。BFC就是一个相对独立的布局环境，它内部元素的布局不受外面布局的影响。
```
<!doctype html>
<html>
  <head>
    <style type="text/css">
    #left {
      width: 500px;
      height: 300px;
      float: left;
      background: #DDD;
    }
    #right {
      height: 300px;
      overflow: hidden;
      background: #AAA;
    }
    </style>
  </head>
  <body>
    <div id="left"></div>
    <div id="right"></div>
  </body>
</html>
```

#### 1.3 方法三：左侧设置绝对定位，右侧设置margin-left为左边的宽度
```
<!doctype html>
<html>
  <head>
    <style type="text/css">
    body{
      margin:0;
    }
    #left{
      width:500px;
      height:300px;
      position:absolute;
      left:0px;
      top:0px;
      background:#DDD;
    }
    #right{
      height:300px;
      margin-left:500px;
      background:#AAA;
    }
    </style>
  </head>
  <body>
    <div id="left"></div>
    <div id="right"></div>
  </body>
</html>
```

#### 1.4 方法四：父元素设置display:flex，右侧设置flex:1
```
<!doctype html>
<html>
  <head>
    <style type="text/css">
    body{
      display:flex;
    }
    #left{
      width:500px;
      height:300px;
      background:#DDD;
    }
    #right{
      height:300px;
      flex:1;
      background:#AAA;
    }
    </style>
  </head>
  <body>
    <div id="left"></div>
    <div id="right"></div>
  </body>
</html>
```

### 2 右侧固定宽度，左侧自适应
#### 2.1 方法一：左侧设置margin-right为右侧的宽度，右侧设置float: right
```
<!doctype html>
<html>
  <head>
    <style type="text/css">
    #left{
      height:300px;
      margin-right:300px;
      background:#DDD;
    }
    #right{
      width:500px;
      height:300px;
      float:right;
      background:#AAA;
    }
    </style>
  </head>
  <body>
    <div id="right"></div>
    <div id="left"></div>
  </body>
</html>
```
注意：right要放在left的前面，否则会出现right右浮动后内容换行下移的问题。出现该问题的原因是非float的元素和float的元素在一起时，如果非float的元素在先，那么float的元素将被排斥到下一行。由于right是float的元素，而left是非float的元素，为避免right内容换行下移，需要把right放在left的前面。

#### 2.2 方法二：左侧设置margin-right为右侧的宽度，右侧采用绝对定位，根据所需的右侧div与浏览器窗口的顶端和右边的距离分别设置top和right
```
<!doctype html>
<html>
  <head>
    <style type="text/css">
      body{
        margin:0;
      }
      #left{
        height:300px;
        margin-right:500px;
        background:#DDD;
      }
      #right{
        width:500px;
        height:300px;
        position:absolute;
        top:0;
        right:0;
        background:#AAA;
      }
    </style>
  </head>
  <body>
    <div id="left"></div>
    <div id="right"></div>
  </body>
</html>
```
此时left和right的位置前后无关紧要。
注意：由于body默认有margin，对绝对定位的right设置top:0和right:0会导致左右侧之间有缝隙且不等高，为此，可统一设置body的margin为0。

#### 2.3 方法三：左侧设置width为100%，float: left，margin-right为右侧的宽度的负值，右侧设置float: right
```
<!doctype html>
<html>
  <head>
    <style type="text/css">
      #left{
        height:300px;
        float:left;
        width:100%;
        margin-right:-500px;
        background:#DDD;
      }
      #right{
        width:500px;
        height:300px;
        float:right;
        background:#AAA;
      }
    </style>
  </head>
  <body>
    <div id="left"></div>
    <div id="right"></div>
  </body>
</html>
```

#### 2.4 方法四：父元素设置display:flex，左侧设置flex:1
```
<!doctype html>
<html>
  <head>
    <style type="text/css">
      body{
        display:flex;
      }
      #left{
        height:300px;
        flex:1;
        background:#DDD;
      }
      #right{
        width:500px;
        height:300px;
        background:#AAA;
      }
    </style>
  </head>
  <body>
    <div id="left"></div>
    <div id="right"></div>
  </body>
</html>
```

### 3 左右固定宽度，中间自适应
#### 3.1 方法一：左侧设置float:left，右侧设置float：right，中间设置margin-right为右侧的宽度，margin-left为左侧的宽度
```
<!doctype html>
<html>
  <head>
    <style type="text/css">
      #left{
        width:200px;
        height:300px;
        float:left;
        background:#AAA;
        }
      #middle{
        height:300px;
        margin-left:200px;
        margin-right:500px;
        background:#DDD;
        }
      #right{
        width:500px
        height:300px;
        float:right;
        background:#AAA;
      }
    </style>
  </head>
  <body>
    <div id="left"></div>
    <div id="right"></div>
    <div id="middle"></div>
  </body>
</html>
```
注意：right要放在middle的前面，否则会出现right右浮动后内容换行下移的问题。出现该问题的原因是非float的元素和float的元素在一起时，如果非float的元素在先，那么float的元素将被排斥到下一行。由于right是float的元素，而middle是非float的元素，为避免right内容换行下移，需要把right放在middle的前面。
若中间栏需要优先加载，请见：http://blog.csdn.net/zhouziyu2011/article/details/53857095

#### 3.2 方法二：中间设置margin-right为右侧的宽度，margin-left为左侧的宽度，左侧和右侧采用绝对定位，根据左侧div所需的left与浏览器窗口的顶端和左边的距离分别设置top和left，根据所需的右侧div与浏览器窗口的顶端和右边的距离分别设置top和right
```
<!doctype html>
<html>
  <head>
    <style type="text/css">
      body{
        margin:0;
      }
      #left{
        width:200px;
        height:300px;
        position:absolute;
        left:0;
        top:0;
        background:#AAA;
      }
      #middle{
        height:300px;
        margin-left:200px;
        margin-right:500px;
        background:#DDD;
      }
      #right{
        width:500px;
        height:300px;
        float:right;
        position:absolute;
        right:0;
        top:0;
        background:#AAA;
      }
    </style>
  </head>
  <body>
    <div id="left"></div>
    <div id="middle"></div>
    <div id="right"></div>
  </body>
</html>
```
此时middle和right的位置前后无关紧要。
注意：由于body默认有margin，对绝对定位的left设置top:0和left:0，right设置top:0和right:0会导致左右侧和中间之间有缝隙且不等高，为此，可统一设置body的margin为0。

#### 3.3 方法三：父元素设置display:flex，中间设置flex:1
```
<!doctype html>
<html>
  <head>
    <style type="text/css">
      body{
        margin:0;
        display:flex;
      }
      #left{
        width:200px;
        height:300px;
        background:#AAA;
      }
      #middle{
        height:300px;
        flex:1;
        background:#DDD;
      }
      #right{
        width:500px;
        height:300px;
        background:#AAA;
      }
    </style>
  </head>
  <body>
    <div id="left"></div>
    <div id="middle"></div>
    <div id="right"></div>
  </body>
</html>
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
