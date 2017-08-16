[<< 回到主页](http://suzy1993.github.io/misszy/)

## CSS 伪类和伪元素

### 1 伪类与伪元素
#### 1.1 CSS伪类
用于向某些选择器添加特殊的效果。
<table>
  <tr><th>伪类</th><th>作用</th></tr>
  <tr><td>:hover</td><td>将样式添加到鼠标悬浮的元素</td></tr>
  <tr><td>:active</td><td>将样式添加到被激活的元素</td></tr>
  <tr><td>:focus</td><td>将样式添加到获得焦点的元素</td></tr>
  <tr><td>:link</td><td>将样式添加到未被访问过的链接</td></tr>
  <tr><td>:visited</td><td>将样式添加到被访问过的链接</td></tr>
  <tr><td>:first-child</td><td>将样式添加到元素的第一个子元素</td></tr>
  <tr><td>:lang</td><td>定义指定的元素中使用的语言</td></tr>
</table>

### 1.2 CSS伪元素
用于将特殊的效果添加到某些选择器。伪元素代表了某个元素的子元素，这个子元素虽然在逻辑上存在，但却并不实际存在于文档树中。
<table>
  <tr><th>伪元素</th><th>作用</th></tr>
  <tr><td>::first-letter</td><td>将样式添加到文本的首字母</td></tr>
  <tr><td>::first-line</td><td>将样式添加到文本的首行</td></tr>
  <tr><td>::before</td><td>在某元素之前插入某些内容</td></tr>
  <tr><td>::after</td><td>在某元素之后插入某些内容</td></tr>
</table>

#### 1.3 伪类与伪元素的对比
伪类的效果可以通过添加一个实际的类来达到，而伪元素的效果则需要通过添加一个实际的元素才能达到，这也是为什么他们一个称为伪类，一个称为伪元素的原因。
CSS3为了区分伪类和伪元素，已经明确规定了伪类用一个冒号来表示，而伪元素则用两个冒号来表示。但因为兼容性的问题，所以现在大部分还是统一的单冒号，但是抛开兼容性的问题，我们在书写时应该尽可能养成好习惯，区分两者。
单冒号(:)用于css3伪类，双冒号(::)用于CSS3伪元素。伪元素由双冒号和伪元素名称组成。不过浏览器需要同时支持旧的已经存在的伪元素写法，比如:first-line、:first-letter、:before、:after等，而新的在CSS3中引入的伪元素则不允许再支持旧的单冒号的写法。

### 2 CSS3新增伪类
<table>
  <tr><th>新增伪类</th><th>作用</th></tr>
  <tr><td>p:first-of-type</td><td>选择属于其父元素的首个p元素的每个p元素</td></tr>
  <tr><td>p:last-of-type</td><td>选择属于其父元素的最后p元素的每个p元素</td></tr>
  <tr><td>p:only-of-type</td><td>选择属于其父元素唯一的子元素的每个p元素</td></tr>
  <tr><td>p:only-child</td><td>选择属于其父元素唯一的子元素的每个p元素</td></tr>
  <tr><td>p:nth-child(n)</td><td>选择属于其父元素的第n个子元素的每个p元素</td></tr>
  <tr><td>p:nth-last-child(n)</td><td>选择属于其父元素的倒数第n个子元素的每个p元素</td></tr>
  <tr><td>p:nth-of-type(n)</td><td>选择属于其父元素第n个p元素的每个p元素</td></tr>
  <tr><td>p:nth-last-of-type(n)</td><td>选择属于其父元素倒数第n个p元素的每个<p元素</td></tr>
  <tr><td>p:last-child</td><td>选择属于其父元素最后一个子元素的每个p元素</td></tr>
  <tr><td>p:empty</td><td>选择没有子元素的每个p元素（包括文本节点）</td></tr>
  <tr><td>p:target</td><td>选择当前活动的p元素</td></tr>
  <tr><td>:not(p)</td><td>选择非p元素的每个元素</td></tr>
  <tr><td>:enabled</td><td>控制表单控件的可用状态</td></tr>
  <tr><td>:disabled</td><td>控制表单控件的禁用状态</td></tr>
  <tr><td>:checked</td><td>单选框或复选框被选中</td></tr>
</table>

[<< 回到主页](http://suzy1993.github.io/misszy/)
