[<< 回到主页](http://suzy1993.github.io/misszy/)

## CSS3 新特性——multi-column layout

CSS3新增了一个多列布局模块Multi-column Layout Module，主要应用在文本的多列布局方面。

Multi-column可分为：
### 1 列数和列宽：column-count、column-width、columns
#### 1.1 column-count
值为正整数，不带单位，表示Multi-column分列的列数，默认为auto（列数根据column-width等其他参数来定）。IE不支持该属性，在Firefox和Webkit下需要加上前缀-moz和-webkit。

#### 1.2 column-width
表示Multi-column的列宽，其单位是px或em，但不能是负数，默认为auto（列数根据column-count等其他参数来定，但column-count此时不能为auto）。IE不支持该属性，Opera11+支持，在Firefox和Webkit下需要加上前缀-moz和-webkit。

#### 1.3 columns
把column-width和column-count两个属性合并在一起使用。这种简写模式只在Webkit和Opera下支持，Firefox不支持。  
若同时设置了column-width和column-gap，则实际列宽会根据column-gap调整，不一定等于设置的column-width值。  
当设置的列宽大于元素容器的宽度时，并不会让元素内容按列宽进行布局而撑破容器，只会把列宽降到与容器宽度相等。  
为了分列能适应各种屏幕大小，最好设置一个确切的列宽或列数，并相应指定相关属性，如元素的width、column-gap、column-rule-width等，如果column-gap、column-rule-width使用默认值，在多列设计中最好明确写定好、column-width和column-count的值。

### 2 列的间距和分列样式：column-gap、column-rule-color、column-rule-style、column-rule-width、column-rule
跨列布局中，column-gap相当于两列之间的空白，类似于margin；而column-rule相当于一条分隔线，类似于border。column-gap和olumn-rule是有高度的，其高度和列等高，最大区别是column-gap没有任何样式，且在列与列之间占有一定的空间，而column-rule有一定的样式，类似于border有样式。  
column-gap单位是px或em，但不能是负数，默认值为normal（1em）。IE不支持该属性，Opera11+支持，在Firefox和Webkit下需要加上前缀-moz和-webkit。  
虽然column-gap可以用来改变相邻列之间的距离，但在多列元素同时设置了column-width时，column-gap与column-width之和大于多列元素总宽度时，会导至列被撑破，并以第一列显示，此时的列宽自动调节到元素的总宽度。  
column-rule同样具有border类似的属性：宽度column-rule-width（默认值为medium），样式column-rule-style（默认值为none），颜色column-rule-color，不同的是border占有一定的空间位置，而column-rule不占有任何的空间位置，column-rule-width增大并不会影响列的布局，只会将其往元素两边扩展，直到元素边缘为止。

### 3 列的分栏符：break-before、break-after、break-inside
目前支持的浏览器很少，暂不介绍。

### 4 填充列：column-fill
目前支持的浏览器很少，暂不介绍。

### 5 跨越列：column-span
column-span主要用来定义一个分列元素中的子元素能跨列多少列。有时需要某个内容或某个标题不进行任何分列，需要其横跨所有列，此时就需要用到column-span属性。默认值为none，表示不跨越任何列；all表示跨越所有列。目前支持的浏览器只有Safari、Chrome、Opera11+，在Webkit下需要加上前缀-webkit。
```
.multiColumns {
    -moz-column-count: 3;
    -webkit-column-count: 3;
    column-count: 3;
    -moz-column-gap: 30px;
    -webkit-column-gap: 30px;
    column-gap: 30px;
}
.multiColumns h1 {
    background: red;
    -webkit-column-span: all;
    column-span: all;
}
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
