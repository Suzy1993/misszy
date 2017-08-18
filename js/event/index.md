[<< 回到主页](http://suzy1993.github.io/misszy/)

## JavaScript 事件模型

JavaScript和HTML之间的交互是通过事件实现的。

### 1 事件流
事件流描述的是从页面中接受事件的顺序，但IE和Netscape却提出了完全相反的事件流的概念，IE的事件流是事件冒泡流，而Netscape的事件流是事件捕获流。
#### 1.1 事件冒泡
事件开始时由最具体的元素（文档中嵌套层次最深的那个节点接收，然后逐级向上传播到较为不具体的节点（文档）。  
不支持事件冒泡的事件：blur、focus、load、unload。  
在一个对象上触发某类事件，如onclick事件等，在其祖先节点上也会依次触发该事件。
```
<body onclick="alert('body')">
    <div onclick="alert('div')">
        <a href="" onclick="alert('a')">事件冒泡</a>
    </div>
</body>
```
依次输出：a、div、body  
注意：不是所有的事件都能冒泡。blur、focus、load、unload等事件不冒泡。  
阻止事件冒泡:
若只希望事件发生在该子元素而不是在它的祖先元素上，则需要阻止事件冒泡。  
IE浏览器和其他浏览器阻止事件冒泡的方式不同：
```
<div>
    <a href="">事件冒泡</a>
</div>
<script>
    function stopBubble(e){
        if(e && e.stopPropagation)
            e.stopPropagation(); // 非IE浏览器
        else
            window.event.cancelBubble = true; // IE浏览器
    }
    document.getElementsByTagName("body")[0].onclick = function(e) {
        stopBubble(e);
        alert('body');
    }
    document.getElementsByTagName("div")[0].onclick = function(e) {
        stopBubble(e);
        alert('div');
    }
    document.getElementsByTagName("a")[0].onclick = function(e) {
        stopBubble(e);
        alert('a');
    }
</script>
```
输出：a

#### 1.2 事件捕获
不太具体的节点应该更早接收到事件，而最具体的节点应该最后接收到事件。事件捕获的用意在于事件到达预定目标之前捕获它。  
虽然IE9、Safari、Chrome、Firefox、Opera都支持事件捕获和事件冒泡，但IE8及其更早版本只支持事件冒泡，不支持事件捕获，因此。建议使用事件冒泡，在有特殊需要的时候再使用事件捕获。

#### 1.3 DOM事件流
DOM2级事件包括三个阶段：事件捕获阶段、处于目标阶段和事件冒泡阶段。实际上，在事件捕获阶段预定目标不会接收到事件，处于目标阶段事件在预定目标上发生。事件处理中，处于目标阶段被看成事件冒泡阶段的一部分。但是，即使“DOM2级事件”规范明确要求捕获阶段不会涉及事件目标，但IE9、 Safari、Chrome、Firefox和Opera9.5及更高版本都会在捕获阶段触发事件对象上的事件，结果是有两个机会在目标对象上操作事件。  
IE9、Firefox、Opera、Sarifi、Chrome都支持DOM事件流，IE8及其更早版本不支持DOM事件流。

### 2 事件处理程序
JavaScript中有五种事件处理程序方式：
#### 2.1 HTML事件处理程序
每种事件都可以使用一个与相应事件处理程序同名的HTML特性来指定，特性的值可以是能够执行的JavaScript代码，也可以是函数。函数中有一个局部变量event，通过event变量可以访问事件对象；在函数内部，this值等于事件的目标元素。  
在HTML中指定事件处理程序的几个缺点：
* 时差问题：用户可能在HTML元素一出现在页面上就触发相应的事件，但当时的事件处理程序有可能尚不具备执行条件，如用户在解析事件处理函数之前就触发事件。为此，很多HTML事件处理程序都会封装在一个try-catch块中，以便及时捕获错误，以免错误抛出被用户看到。
* 扩展事件处理程序的作用域链在不同浏览器中会导致不同的结果。不同JavaScript引擎遵循的标识符解析规则略有差异，很有可能会在访问非限定对象成员时出错。
* HTML代码与JavaScript代码紧密耦合，更换事件处理程序需要改动HTML代码与JavaScript代码。

#### 2.2 DOM0级事件处理程序
通过JavaScript指定事件处理程序的传统方式，将一个函数赋值给一个事件处理程序属性。使用DOM0级方法指定的事件处理程序被认为是元素的方法，因此this引用当前元素。  
DOM0级事件处理程序的优势：简单+跨浏览器。  
可以通过将事件处理程序的值设置为null来删除通过DOM0级方法指定的事件处理程序。

#### 2.3 DOM2级事件处理程序
DOM2级事件定义了两种方法，用于指定和删除事件处理程序的操作：addEventListener()和removeEventListener()，它们都接收3个参数：要处理的事件名、作为事件处理程序的函数和一个布尔值（true表示在捕获阶段调用事件处理程序，false表示在冒泡阶段调用事件处理程序）。  
DOM2级事件处理程序的优势：可以添加多个事件处理程序，它们会按照添加它们的顺序触发。  
通过addEventListener()添加的事件处理程序只能用removeEventListener()来移除，但要求移除时传入的参数与添加事件处理程序时使用的参数相同，因此通过addEventListener()添加的匿名函数将无法移除，需要给removeEventListener()传入addEventListener()中命名的函数才能正常移除。

#### 2.4 IE事件处理程序
IE事件定义了两个方法：attachEvent()和detachEvent()，它们都接收2个参数：要处理的事件名、作为事件处理程序的函数.由于IE8及其更早版本只支持事件冒泡，所以通过attachEvent()添加的事件处理程序都会被添加到冒泡阶段。  
注意：通过IE的attachEvent()添加的事件处理程序的名字以“on”开头，而通过DOM的addEventListener()添加的名字不是。
#### 2.4.1 在IE中使用attachEvent()与使用DOM0级方法的主要区别
事件处理程序的作用域不同。在IE中使用attachEvent()，事件处理程序会在全局作用域中运行，因此this等于window；而使用DOM0级方法，事件处理程序会在其所属元素的作用域中运行。

#### 2.4.2 在IE中使用attachEvent()与使用DOM2级方法的区别
添加多个事件处理程序的执行顺序不同。在IE中使用attachEvent()，可以添加多个事件处理程序，它们会按照添加它们的相反顺序触发；在DOM中使用addEventListener()，可以添加多个事件处理程序，但它们会按照添加它们的顺序触发。  
通过attachEvent()添加的事件处理程序只能用detachEvent()来移除，但要求移除时传入的参数与添加事件处理程序时使用的参数相同，因此通过attachEvent()添加的匿名函数将无法移除，需要给detachEvent()传入attachEvent()中命名的函数才能正常移除。

#### 2.4.5 跨浏览器的事件处理程序
要保证事件处理程序的代码在大多数浏览器下一致地运行，只需关注冒泡阶段。  
视情况分别使用DOM2级方法、IE方法、DOM0级方法来添加和移除事件，addHandler()和removeHandler()方法属于EventUtil对象。
* 先检测传入的元素是否存在DOM2级方法（传入的第三个参数为false以表示冒泡阶段）；
* 再检测传入的元素是否存在IE的方法；
* 最后检测传入的元素是否存在DOM0级方法（使用方括号语法将属性名指定为事件处理程序）。

### 3 事件对象event
事件对象event包含导致事件的元素、事件的类型以及其他与特定事件相关的信息。
#### 3.1 DOM中的事件对象
若直接将事件处理程序指定给了目标元素，则this，currentTarget和target包含相同的值；若事件处理程序存在于按钮的父节点中，则this和currentTarget等于父节点，而target等于按钮元素。  
注意：只有在事件处理程序执行期间，event对象才会存在；一旦事件处理程序执行完毕，event对象就会被销毁。
<table>
  <tr><th>属性/方法</th><th>描述</th></tr>
  <tr><td>bubbles</td><td>表明事件是否冒泡</td></tr>
  <tr><td>cancelabel</td><td>表明是否可以取消事件的默认行为</td></tr>
  <tr><td>currentTarget</td><td>事件处理程序当前正在处理事件的那个元素（监听事件的那个元素）</td></tr>
  <tr><td>defaultPrevented</td><td>true表示已经调用了preventDefault()</td></tr>
  <tr><td>detail</td><td>与事件相关的细节信息</td></tr>
  <tr><td>eventPhase</td><td>调用事件处理程序的阶段：1表示捕获阶段，2表示处于目标，3表示冒泡阶段</td></tr>
  <tr><td>preventDefault()</td><td>取消事件的默认行为，cancelable为true才可使用此方法</td></tr>
  <tr><td>stopImmediatePropagation()</td><td>取消事件的捕获或冒泡，同时阻止任何事件处理程序被调用</td></tr>
  <tr><td>stopPropagation()</td><td>取消事件的捕获或冒泡，bubbles为true才可使用此方法</td></tr>
  <tr><td>target</td><td>事件的目标</td></tr>
  <tr><td>trusted</td><td>true表示事件是浏览器生成的，false表示事件是由开发人员通过JavaScript创建的</td></tr>
  <tr><td>type</td><td>事件的类型</td></tr>
  <tr><td>view</td><td>与事件关联的抽象视图，等同于发生事件的window对象</td></tr>
</table>

#### 3.2 IE中的事件对象
访问IE中的event对象有几种不同的方式：
* 在使用DOM0级方法添加事件处理程序时，通过window.event访问event对象；
* 在使用attachEvent()方法添加事件处理程序时，event对象会作为参数被传入事件处理程序中，也可以通过window.event访问event对象；
* 在通过HTML特性指定事件处理程序时，还可以通过一个名为event的变量来访问event对象。
IE中的事件目标通过srcElement属性获取，this不一定等于事件目标：在使用DOM0级方法添加事件处理程序时，this等于事件目标，但在使用attachEvent()方法添加事件处理程序时，this则不等于事件目标。
<table>
  <tr><th>属性/方法</th><th>描述</th></tr>
  <tr><td>cancelBubble</td><td>默认为false，但将其设置为true就可以取消事件冒泡，由于IE不支持事件捕获，因此只能取消事件冒泡，而stopPropagation()则可以同时取消事件捕获或冒泡</td></tr>
  <tr><td>returnValue</td><td>默认为true，但将其设置为false就可以取消事件的默认行为</td></tr>
  <tr><td>srcElement</td><td>事件的目标</td></tr>
  <tr><td>type</td><td>事件的类型</td></tr>
</table>

### 4 事件委托
使用了事件冒泡的原理，从触发某事件的元素开始，递归地向上级元素传播事件。
#### 4.1 事件委托的优点
* 对于要大量处理的元素，不必为每个元素都绑定事件，只需要在它们的父元素上绑定一次即可，提高性能。
* 可以处理动态插入DOM中的元素，对动态插入DOM中的元素进行直接绑定是不行的。

#### 4.2 触发事件的元素
事件委托给父元素后，如何得知事件是哪个子元素触发的？——可以通过event.target对象来获取。

[<< 回到主页](http://suzy1993.github.io/misszy/)
