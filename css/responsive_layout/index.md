[<< 回到主页](http://suzy1993.github.io/misszy/)

## CSS 响应式布局

### 1 media query（媒介查询）
#### 1.1 media query的作用
* 使用 @media 查询可以针对不同的媒体类型定义不同的样式。
* @media 可以针对不同的屏幕尺寸设置不同的样式，特别是如果需要设置设计响应式的页面。
* 重置浏览器大小的过程中，页面也会根据浏览器的宽度和高度重新渲染页面。

#### 1.2 media query的语法
```
@media 媒介类型and|not|only (媒介特征) {
    ...
}
```
#### 1.2.1 媒介类型
* print：用于打印机和打印预览
* screen：用于电脑屏幕，平板电脑，智能手机等
* all：用于所有媒体设备类型

#### 1.2.2 媒介特征
* device-height：定义输出设备的屏幕可见高度。
* device-width：定义输出设备的屏幕可见宽度。
* height：定义输出设备中的页面可见区域高度。
* width：定义输出设备中的页面可见区域宽度。
* max-device-height：定义输出设备的屏幕可见的最大高度。
* max-device-width：定义输出设备的屏幕可见的最大宽度。
* max-height：定义输出设备中的页面可见的最大高度。
* max-width：定义输出设备中的页面可见的最大宽度。
* min-device-height：定义输出设备的屏幕可见的最小高度。
* min-device-width：定义输出设备的屏幕可见的最小宽度。
* min-height：定义输出设备中的页面可见的最小高度。
* min-width：定义输出设备中的页面可见的最小宽度。

#### 1.2.3 max-device-width与max-width的区别
* max-width指的是显示区域的宽度，比如浏览器的显示区域宽度；max-device-width指的是设备整个显示区域的宽度，比如设备的实际屏幕宽度，也就是设备分辨率。
* max-width在窗口大小改变或横竖屏转换时会发生变化；max-device-width只与设备相关，在窗口大小改变或横竖屏转换时都不会发生变化。

#### 1.3 media query的引入方法
#### 1.3.1 在head中引入
```
<link media="screen and (max-width:600px)" rel="stylesheet" href="example.css" />
```

#### 1.3.2 在@import中引入
```
<style type="text/css" media="screen and (min-width:600px) and (max-width:900px)">
    @import url("css/style.css");
</style>
```

#### 1.3.3 直接在CSS中使用
```
@media screen and (max-width: 800px) {
    // CSS样式
}
```

### 2 Bootstrap栅格化布局
<table>
  <tr><th></th><th>超小设备手机（<768px）</th><th>小型设备平板电脑（≥768px）</th><th>中型设备台式电脑（≥992px）</th><th>大型设备台式电脑（≥1200px）</th></tr>
  <tr><td>网格行为</td><td>一直是水平的</td><td>以折叠开始，断点以上是水平的</td><td>以折叠开始，断点以上是水平的</td><td>以折叠开始，断点以上是水平的</td></tr>
  <tr><td>最大容器宽度</td><td>None (auto)</td><td>750px</td><td>970px</td><td>1170px</td></tr>
  <tr><td>Class前缀</td><td>.col-xs-</td><td>.col-sm-</td><td>.col-md-</td><td>.col-lg-</td></tr>
  <tr><td>列数量和</td><td>12</td><td>12</td><td>12</td><td>12</td></tr>
  <tr><td>最大列宽</td><td>Auto</td><td>60px</td><td>78px</td><td>95px</td></tr>
  <tr><td>间隙宽度</td><td>30px（一列的每边分别 15px）</td><td>30px（一列的每边分别 15px）</td><td>30px（一列的每边分别 15px）</td><td>30px（一列的每边分别 15px）</td></tr>
  <tr><td>最大列宽</td><td>Auto</td><td>60px</td><td>78px</td><td>95px</td></tr>
</table>
#### 2.1 偏移列
偏移可用来给列腾出更多的空间。
.col-xs-* 类不支持偏移，但是可以简单地通过使用一个空的单元格来实现该效果。
为了在大屏幕显示器上使用偏移，请使用 .col-md-offset-* 类，这些类会把一个列的左外边距margin增加 * 列，其中 *范围是从 1 到 11。
例如：<div class="col-md-6 col-md-offset-3">...</div>可居中div。

#### 2.2 嵌套列
为了在内容中嵌套默认的网格，在一个已有的 .col-md-* 列内添加添加一个新的 .row，并在新的 .row内添加一组 .col-md-* 列，这组列个数不能超过12，其实，也没有要求必须占满12列。

[<< 回到主页](http://suzy1993.github.io/misszy/)
