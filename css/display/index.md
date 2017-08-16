[<< 回到主页](http://suzy1993.github.io/misszy/)

## CSS display属性

### 1 display可能的属性值
<table>
  <tr><th>值</th><th>描述</th></tr>
  <tr><td>none</td><td>缺省值。像行内元素类型一样显示</td></tr>
  <tr><td>block</td><td>块类型。默认宽度为父元素宽度，可设置宽高，换行显示</td></tr>
  <tr><td>inline</td><td>行内元素类型。默认宽度为内容宽度，不可设置宽高，同行显示</td></tr>
  <tr><td>inline-block</td><td>默认宽度为内容宽度，可以设置宽高，同行显示</td></tr>
  <tr><td>list-item</td><td>像块类型元素一样显示，并添加样式列表标记</td></tr>
  <tr><td>inherit</td><td>规定应该从父元素继承 display 属性的值</td></tr>
  <tr><td>table</td><td>此元素会作为块级表格显示(类似<table>)，表格前后有换行符</td></tr>
  <tr><td>inline-table</td><td></td></tr>
  <tr><td>table-row-group</td><td>此元素会作为内联表格显示(类似<table>)，表格前后无换行符</td></tr>
  <tr><td>table-header-group</td><td>此元素会作为一个或多个行的分组显示(类似<thead>)</td></tr>
  <tr><td>table-footer-group</td><td>此元素会作为一个或多个行的分组显示(类似<tfoot>)</td></tr>
  <tr><td>table-row</td><td>此元素会作为一个表格行显示(类似<tr>)</td></tr>
  <tr><td>table-column-group</td><td>此元素会作为一个或多个列的分组显示(类似<colgroup>)</td></tr>
  <tr><td>table-column</td><td>此元素会作为一个单元格列显示(类似<col>)</td></tr>
  <tr><td>table-cell</td><td>此元素会作为一个表格单元格显示(类似<td> 和<th>)</td></tr>
  <tr><td>table-caption</td><td>此元素会作为一个表格标题显示(类似<caption>)</td></tr>
</table>

### 2 display: block & display: inline & display: inline-block
#### 2.1 display: block
* block元素会独占一行，多个block元素会各自新起一行。默认情况下，block元素宽度自动填满其父元素宽度。
* block元素可以设置width和height属性，即使设置了宽度，仍然是独占一行。
* block元素可以设置margin和padding属性。

#### 2.2 display: inline
* inline元素不会独占一行，多个相邻的inline元素会排列在同一行里，直到一行排列不下，才会新换一行，其宽度随元素的内容而变化。
* inline元素设置width和height属性无效。
* inline元素的margin和padding属性，水平方向的padding-left、padding-right、margin-left、margin-right都产生边距效果；但竖直方向的padding-top、padding-bottom、margin-top、margin-bottom不会产生边距效果。

#### 2.3 display: inline-block
* 将对象呈现为inline对象，但是对象的内容作为block对象呈现。之后的内联对象会被排列在同一行内，可以设置width和height属性，也可以设置margin和padding属性。

[<< 回到主页](http://suzy1993.github.io/misszy/)
