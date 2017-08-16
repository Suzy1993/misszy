[<< 回到主页](http://suzy1993.github.io/misszy/)

## CSS 包含块Containing Block

### 1 CSS包含块（Containing Block）
CSS包含块（Containing Block）是视觉格式化模型的一个重要概念，它与框模型类似，也可以理解为一个矩形，而这个矩形的作用是为它里面包含的元素提供一个参考，元素的尺寸和位置的计算往往是由该元素所在的包含块决定的。
包含块是定位参考框或定位坐标参考系，元素一旦定义了position定位（absolute或relative或fixed），它所包含的定位元素都将以该包含块为坐标系进行定位和调整。

### 2 initial containing block（初始内容块）
用户代理（如浏览器）选择根元素(HTML/body)作为initial containing block（初始内容块）。

### 3 不同情况下的containing block
#### 3.1 position: absolute
找到其祖先元素中最近的position值不为static的元素，再判断：
1）若此元素为inline元素，则containing block取决于祖先的direction属性。
* 如果direction是ltr（左到右），祖先产生的第一个盒子的上、左padding边界是containing block的上方和左方，祖先的最后一个盒子的下、右padding边界是containing block的下方和右方。
* 如果direction是rtl（右到左），祖先产生的第一个盒子的上、右padding边界是containing block的上方和右方，祖先的最后一个盒子的下、左padding边界是containing block的下方和左方。
2）若此元素为block元素，则containing block由该祖先元素的padding框构成。
3）如果都找不到，则containing block为initial containing block。

#### 3.2 position: static或relative
containing block是它的父元素的内容框(即去掉padding的部分)。

#### 3.3 position: fixed
containing block为initial containing block。

[<< 回到主页](http://suzy1993.github.io/misszy/)
