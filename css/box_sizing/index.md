[<< 回到主页](http://suzy1993.github.io/misszy/)

## CSS3 新特性——box-sizing属性

### 1 CSS盒子模型
#### 1.1 w3c 盒子模型
标准盒模型，是指块元素box-sizing属性为content-box的盒模型。一般在现代浏览器中使用的都是标准盒模型，它也是标准 w3c 盒子模型。

#### 1.2 IE盒子模型
怪异盒模型，是指块元素box-sizing属性为border-box的盒模型。一般在IE浏览器中默认为是怪异盒模型，但由于其自身的特殊性，手机页面中也有使用怪异盒模型。

### 2 box-sizing：content-box | border-box
默认值：content-box  
适用于：所有接受width和height的元素
#### 2.1 content-box
padding和border不被包含在定义的width和height之内。对象的实际宽度等于设置的width值和border、padding之和，即width + border + padding，表现为标准模式下的盒模型。box-sizing默认值为content-box，可以不写。
```
#square{
    width:300px;
    height:300px;
    background:red;
    padding:10px;
    border:10px solid transparent;
}
```

#### 2.2 border-box
padding和border被包含在定义的width和height之内。对象的实际宽度就等于设置的width值，即使定义border和padding也不会改变对象的实际宽度，表现为怪异模式下的盒模型。
```
#square{
    width:300px;
    height:300px;
    background:red;
    padding:10px;
    border:10px solid transparent;
    box-sizing:border-box;
}
```
注意：box-sizing 属性是CSS3的新特性——IE、Opera 以及 Chrome 支持 box-sizing 属性；Firefox需要加上-moz前缀；Safari需要加上-webkit前缀。

#### 2.3 box-sizing: border-box的典型应用
创建一个width为100%，border为10px的div。  
问题：不设置box-sizing的情况下，div会溢出。  
解决方法：设置box-sizing: border-box。
```
#square{
    width:100%;
    height:100px;
    border:10px solid grey;
    box-sizing:border-box;
    background:red;
}
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
