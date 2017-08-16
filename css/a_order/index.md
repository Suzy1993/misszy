[<< 回到主页](http://suzy1993.github.io/misszy/)

## CSS <a>的正确顺序

### 1 <a>的四种类型
<table>
  <tr><th>类型</th><th>含义</th></tr>
  <tr><td>a:link</td><td>超链接的默认样式</td></tr>
  <tr><td>a:visited</td><td>已经访问过的链接的样式</td></tr>
  <tr><td>a:hover</td><td>处于鼠标悬停状态的链接的样式</td></tr>
  <tr><td>a:active</td><td>被激活（鼠标左键按下的一瞬间）的链接的样式</td></tr>
</table>

### 2 <a>的正确顺序
CSS定义超链接需要注意先后顺序，否则，在某些浏览器里可能会出现某个样式不起作用的bug，不能正确显示想要的效果。
正确的排列顺序是：L-V-H-A，L-V-H-A是link、visited、hover、active的简写。

[<< 回到主页](http://suzy1993.github.io/misszy/)
