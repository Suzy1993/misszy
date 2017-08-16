[<< 回到主页](http://suzy1993.github.io/misszy/)

## CSS3 Flexbox布局

### 1 Flexbox布局
弹性布局，w3c提出，能够简便地、响应式地调整和分布一个容器里的项目布局，即使项目的大小未知或项目是动态的。
#### 1.1 基本思想
使得项目能够改变容器的宽度、高度甚至顺序，以最佳的方式填充可用的空间，以适应所有类型的显示设备和屏幕。Flexbox容器可以使项目扩展来填满可用的空间，也可以缩小项目以防止溢出容器。

#### 1.2 影响
设为Flexbox布局后，子元素的float、clear、vertical-align属性将失效。

#### 1.3 设置
* 任何一个容器都可以指定为Flexbox布局：display:flex;
* 行内元素的Flexbox布局：display:inline-flex;
* Webkit内核的浏览器，必须加上-webkit前缀：display:-webkit-flex;/*Safari*/

### 2 基本概念
#### 2.1 Flexbox容器
采用Flexbox布局的元素。

#### 2.2 Flexbox项目
Flexbox容器的所有子元素。

#### 2.3 Flexbox容器的两根轴
* 主轴（main axis）：不一定是水平的，主要取决于justify-content属性。项目默认沿主轴排列。
* 交叉轴（cross axis）：垂直于主轴，其方向主要取决于主轴方向。

### 3 Flexbox容器的属性
#### 3.1 flex-direction
确定主轴的方向，即项目的排列方向。
* row（默认）：主轴为水平方向，起点在左端。
* row-reverse：主轴为水平方向，起点在右端。
* column：主轴为垂直方向，起点在上端。
* column-reverse：主轴为垂直方向，起点在下端。

#### 3.2 flex-wrap
定义，如果一条轴线排不下，如何换行。
* nowrap（默认）：不换行。
* wrap：换行，第一行在上端。
* wrap-reverse：换行，第一行在下端。

#### 3.3 flex-flow
flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap。

#### 3.4 justify-content
定义项目在主轴上的对齐方式。
* flex-start（默认）：左对齐
* flex-end：右对齐
* center： 居中
* space-between：两端对齐，项目间的间隔都相等。
* space-around：每个项目两侧的间隔相等，故项目间的间隔比项目与边框的间隔大一倍。

#### 3.5 align-items
定义项目在交叉轴上的对齐方式。
* flex-start：交叉轴的起点对齐。
* flex-end：交叉轴的终点对齐。
* center：交叉轴的中点对齐。
* baseline: 项目的第一行文字的基线对齐。
* stretch（默认）：若项目未设置高度或设为auto，将占满整个容器的高度。

#### 3.6 align-content
align-content属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。
* flex-start：与交叉轴的起点对齐。
* flex-end：与交叉轴的终点对齐。
* center：与交叉轴的中点对齐。
* space-between：与交叉轴两端对齐，轴线间的间隔都相等。
* space-around：每根轴线两侧的间隔都相等。故轴线间的间隔比轴线与边框的间隔大一倍。
* stretch（默认）：轴线占满整个交叉轴。

### 4 Flexbox项目的属性
#### 4.1 order
定义项目的排列顺序。数值越小，排列越靠前，默认为0，可以为负值。

#### 4.2 flex-grow
定义项目的放大比例，默认为0，即使存在剩余空间也不放大。若所有项目的flex-grow属性都为1，则若存在剩余空间，它们将等分剩余空间；若一个项目的flex-grow属性为2，其他项目都为1，则该项目占据的剩余空间将比其他项多一倍。可以为负值。

#### 4.3 flex-shrink
定义项目的缩小比例，默认为1，即若空间不足，该项目将缩小。若所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小；若一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，该项目不缩小。负值对该属性无效。

#### 4.4 flex-basis
定义在分配剩余的空间之前，项目占据的主轴空间。浏览器根据该属性计算主轴是否有剩余的空间。默认为auto，即项目的本来大小。若设为跟width或height属性一样的值，则项目将占据固定的空间。

#### 4.5 flex
flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。该属性有两个快捷值：auto (1 1 auto) 和 none (0 0 auto)。建议优先使用该属性，而不使用三个分离的属性，因为浏览器会推算相关值。

#### 4.6 align-self
允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认为auto，表示继承父元素的align-items属性，若没有父元素，则等同于stretch。该属性可能取6个值，除了auto，其他都与align-items属性完全一致。
设为Flexbox布局后，子元素的float、clear、vertical-align属性将失效。

### 5 Flexbox布局的适用场景
1）大部分用于移动网页制作上。
2）react Native混合开发布局也是基于Flexbox布局 。
3）Angular等框架布局也采用Flexbox布局。

### 6 Flecbox的兼容性
flex布局分为旧版本display:box，过渡版本display:flex box，以及现在的标准版本display:flex。
```
.box{
    display: -webkit-flex; /* 新版本语法: Chrome 21+ */
    display: flex; /* 新版本语法: Opera 12.1, Firefox 22+ */
    display: -webkit-box; /* 老版本语法: Safari, iOS, Android browser, older WebKit browsers. */
    display: -moz-box; /* 老版本语法: Firefox (buggy) */
    display: -ms-flexbox; /* 混合版本语法: IE 10 */
}
.flex1 {
    -webkit-flex: 1; /* Chrome */
    -ms-flex: 1 /* IE 10 */
    flex: 1; /* NEW, Spec - Opera 12.1, Firefox 20+ */
    -webkit-box-flex: 1 /* OLD - iOS 6-, Safari 3.1-6 */
    -moz-box-flex: 1; /* OLD - Firefox 19- */
}
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
