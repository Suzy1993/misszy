[<< 回到主页](http://suzy1993.github.io/misszy/)

## React 综述

### 官网
[https://facebook.github.io/react/](https://facebook.github.io/react/)

### 1React
React——颠覆式前端UI开发框架
传统方式将来自于服务器或用户输入的交互数据动态反映到复杂界面上时，代码量变得越来越大，越来越难以维护。基于此，先是谷歌推出自己的前端开发框架Angular，将对DOM的直接操作释放，通过relative来实现复杂的DOM修改。但Angular存在一些问题，如：其整体作为一个MVVM框架显得过重，不适用于对性能要求比较高的站点，如移动端的Web站点；其UI组件的封装相对复杂，不利于重用。而React另辟蹊径得解决了这些问题。
React是Facebook推出的面向视图层开发的一个框架，用于解决大型应用，包括如何很好地管理DOM结构，是构建大型，快速Web app的首选方式。
React使用JavaScript来构建用户界面，因此可以说是一个用来构建用户界面的JavaScript库。
#### 1.1 为什么使用 React？
React是为了解决一个问题：构建数据随着时间不断变化的大规模应用程序。
* 简单：仅仅只要表达出应用程序在任一个时间点应该呈现的样子，当数据变了，React会自动处理所有用户界面的更新。
* 声明式：数据变化后，与点击"刷新"按钮类似，但仅会更新变化的部分。
* 构建可组合的组件：React构建可复用的组件，事实上通过React唯一要做的事情就是构建组件，这得益于其良好的封装性，组件使代码复用、测试和关注分离更加简单。

#### 1.2 React的特点
* React不是一个完整的MVC、MVVM框架，其只负责View。React的设计团队不认为MVC的开发模式仍适用于某些场景下的开发，所以才有了围绕React的一系列理念，如Class、router。
* React只负责View，但React与Web Components不冲突，完全可以用React去实现一个真正意义上的Web Component。
* React不同的设计理念决定了React比Angular轻得多。
* React实现的原理是virtual DOM机制，virtual DOM以及数据单向数据绑定使得React的渲染、响应非常快。virtual DOM带来的不仅仅是性能上的提升，更重要的是其背后所蕴含的思想——组件化的开发思路。组件，就是封装起来具有独立功能的UI控件。React推崇的是用组件的方式去重新思考UI的构成，将UI上每一个功能相对独立的模块定义成组件，然后将小的组件通过组合或嵌套的方式最终构成一个大的组件，完成整体UI的构建，这就意味着它是高度可重用的。
#### 1.3 React的原理
在Web开发中，总需要将变化的数据实时反应到用户界面上，这就需要对DOM进行操作。而复杂或频繁的DOM操作通常是性能瓶颈产生的原因。
React为此引入了虚拟DOM的机制：在浏览器端用JavaScript实现一套DOM API。基于React，所有的DOM操作都通过虚拟DOM进行，每当数据变化时，React都会重新构建整个DOM树，然后将目前的整个DOM树和上次的DOM树进行对比，得到DOM树的区别，仅仅将变化的部分进行浏览器DOM更新。尽管每一次都要重新完整地构建虚拟DOM树，但因为虚拟DOM是内存数据，性能极高，而对真实DOM操作的仅仅是diff部分，因此能达到提高性能的目的。此外，React能批处理虚拟DOM的刷新，在一个事件循环内的两次数据变化会被合并，如连续的先将节点内容从x变成y，然后又从y变成x，React会认为UI不发生任何变化。总之，在保证性能的同时，开发者将不再需要关注数据的变化如何更新到实际的DOM元素，而只需要关心在任意一个数据状态下，整个界面是如何render的，每做一点界面的更新，都可以认为刷新了整个页面，至于如何进行局部更新以保证性能，则是React框架要完成的事情。
以视频聊天应用为例：当收到一条新消息时，传统的开发过程需要知道是哪条数据，如何将新的DOM结点添加到当前DOM树上；而基于React，永远只需要关心数据整体，两次数据之间的UI如何变化，然后可以完全交给React框架去做，这大大降低了逻辑复杂性和开发难度，产生bug的概率也更小。

#### 1.4 对React认识的误区
* React不是一个完整的MVC框架，最多是MVC中的V（View），甚至并不非常认可MVC开发模式；
* React和Web Component不能相提并论，两者并不是完全的竞争关系，完全可以用React去开发一个真正的Web Component；
* React不是一个新的模板语言，没有JSX的React也能工作。

#### 1.5 React的组件化
React以组件的方式去重新思考用户界面的构成，将用户界面上每一个功能相对独立的模块定义成组件，然后将小组件通过组合或嵌套的方式构成大组件，最终完成整体UI的构建。
MVC开发模式的思想：将模型—视图—控制器定义成不同的类，实现表现，数据，控制的分离。
组件化开发模式的思想：用户界面功能模块间的分离，完全是一个新思路，从功能的角度出发，将用户界面分成不同的组件，每个组件都独立封装。
#### 1.5.1 组件的特征
* 可组合：一个组件可以包含其它组件，也可以嵌套在另一个组件内部。也就是说，一个复杂的UI可以拆分成多个简单的UI组件；
* 可重用：每个组件都是具有独立功能的，可用于多个场景；
* 可维护：每个小组件仅包含自身的逻辑，更容易被维护。

#### 1.5.2 组件的嵌套
React是基于组件化的开发，组件化开发最大的优点是复用。实现复用的方式之一便是组件嵌套。

#### 1.6 React的应用场景
* 业务需求在复杂场景下还有着高性能要求，如：图片画廊的应用——所有图片分布在画廊上，有一张焦点图片位于正中间，随意点击任意一张图片，其获得焦点，剩下的图片重新分配位置，点击处于焦点处的图片可以翻转到其背面的介绍，同时底部也有导航条，可以翻转当前图片和切换焦点图片。
* 组件是高度可重用的，如图片画廊中导航条的单个元素以及单个图片都可以自成一个组件，维护开发都将变得异常容易。

### 2 虚拟DOM及diff算法
React非常快速是因为它从不直接操作DOM。React快速的致胜法宝是虚拟DOM及其高效的diff算法。
#### 2.1 虚拟DOM
虚拟DOM是在DOM的基础上建立了一个抽象层，对数据和状态所做的任何改动，都会被自动且高效的同步到虚拟DOM，最后再批量同步到DOM中。
在React中，render执行的结果得到的并不是真正的DOM节点，而仅仅是JavaScript对象，称之为虚拟DOM。
虚拟DOM具有批处理和高效的diff算法，可以无需担心性能问题而随时”刷新"整个页面，因为虚拟DOM可以确保只对界面上真正变化的部分进行实际的DOM操作。
在实际开发中无需关心虚拟DOM是如何运作的，但理解其运行机制不仅有助于更好的理解React组件的生命周期，而且对于进一步优化React程序也有很大帮助。
#### 2.1.1 传统App与React App的对比
传统App：
![image](images/1.png)
innerHTML：render html字符串 + 重新创建所有 DOM 元素
React App：
![image](images/2.png)
虚拟DOM：render 虚拟DOM + diff + 更新必要的 DOM 元素

#### 2.1.2 虚拟DOM的原理
React会在内存中维护一个虚拟DOM树，对这个树进行读或写，实际上是对虚拟DOM进行。当数据变化时，React会自动更新虚拟DOM，然后将新的虚拟DOM和旧的虚拟DOM进行对比，找到变更的部分，得出一个diff，然后将diff放到一个队列里，最终批量更新这些diff到DOM中。

#### 2.1.3 虚拟DOM的优点
最终表现在DOM上的修改只是变更的部分，可以保证非常高效的渲染。

#### 2.1.4 虚拟DOM的缺点
首次渲染大量DOM时，由于多了一层虚拟DOM的计算，会比innerHTML插入慢。

#### 2.1.5 虚拟DOM的理解误区
对虚拟DOM的理解往往停留在：通过JavaScript对象模拟原生DOM，加上DOM diff极大提升了DOM操作的性能。然而，虚拟DOM最大的意义不在于性能的提升（JavaScript对象比DOM对象性能高），而在于抽象了DOM的具体实现（对DOM进行了一层抽象），这在浏览器中使用 React时不是特别明显，因为写的DOM标签跟原生的没有区别，并且最终都被渲染成了DOM，在React Native中将会有很好的说明。

#### 2.2 diff算法
Web界面由DOM树构成，页面某部分发生变化，其实是某个DOM节点发生了变化。变化前后对应两套界面，需要React比较两个界面的区别，这就需要通过diff算法对DOM树进行分析，即针对变化前后的两棵DOM树，找到最少的转换步骤。
#### 2.2.1 diff算法的时间复杂度
标准的diff算法的复杂度为O(n^3)，Facebook工程师结合Web界面的特点做出了以下两个简单的假设，使得diff算法的复杂度直接降低到O(n)：
* 相同的组件产生类似的DOM结构，不同的组件产生不同的DOM结构；
* 对于同一层次的一组子节点，可以通过唯一的id进行区分。

#### 2.2.2 逐层进行节点比较
在React中，两棵DOM树只会对同一层的节点进行比较。若发现节点已不存在，则该节点及其子节点会被完全删除，不会用于进一步的比较。这样，只需要对树进行一次遍历，就能完成整个DOM树的比较。
对于同层节点，若节点本身完全相同（类型相同，属性相同），只是位置不同，则React只需要考虑同层节点的位置变换，不需要进行节点的销毁和重新创建，这就需要用到下面介绍的key属性，但对于不同层的节点，只能销毁和重新创建。

#### 2.2.3 比较两个虚拟DOM节点，可以分为以下三种情况
#### 2.2.3.1 节点类型不同
当在树中的同一位置前后的节点类型不同，React会直接删除原节点，然后创建并插入新的节点。
注意：删除节点即彻底销毁该节点，也就是说，后续不会查找是否有另外一个节点等同于删除的该节点。如果删除的该节点有子节点，那么子节点也会被删除。这也是diff算法复杂度能降到O(n)的原因。
同理，当树的同一个位置遇到前后不同的组件时，也是销毁原组件，把新的组件加上去。这应用了第一个假设，不同的组件一般会产生不同的DOM结构，与其浪费时间去比较不同的DOM结构，还不如完全创建一个新的组件加上去。

#### 2.2.3.2 节点类型相同，但是属性不同
React会对属性进行重设从而实现节点的转换。

#### 2.2.3.3 节点类型相同且属性相同
对于同层节点，若节点本身完全相同（类型相同且属性相同），只是位置不同，则React只需要考虑同层节点的位置变换，不需要进行节点的销毁和重新创建，这就需要用到下面介绍的key属性。
对于不同层的节点，即使节点本身完全相同（类型相同且属性相同），也只能销毁和重新创建。

### 3 JSX
React JSX，即JavaScript和XML，是Facebook为React框架开发的一套语法糖。
JSX是JavaScript的一种语法扩展。JSX像一种模板语言，但可以使用JavaScript的全部特性。
JSX用于产生React元素。建议在React使用JSX编写代码，以让其更直观。
JSX代码中出现的标签既不是一个字符串，也不是HTML。
JSX 类似于HTML，但不完全一样，如：HTML对大小写不敏感，JSX对大小写敏感；HTML中不强制要求标签闭合，但JSX要求标签一定要闭合；JSX的组件要以大写字母开头，以便与HTML标签区分。相比HTML，JSX更接近JavaScript，因此React DOM属性使用驼峰式命名法代替HTML属性命名法。如，在JSX中class需要写作className，tabindex需要写作tabIndex。
JSX带来的一大便利是可以直接在JS里面写类DOM的结构，比用原生JS去拼接字符串，然后再用正则替换的方式来渲染模板，实在是方便和简单太多了。除了原生HTML标签，还可以生成类似于原生HTML标签的自定义标签，它们统称为React Components。注意，这些标签代表的并不是真实的DOM节点，在React看来，只是React Components的实例而已。React Components通过React.render()方法最终呈现在页面上。React.render()方法的第一个参数是要渲染的React Components，第二个参数是React Components渲染完之后要插入的位置的容器Element。自定义的React Components通过调用React.createClass()，参数是JS的一个Object，其中最重要的key值是render函数，其返回值直接决定了自定义的React Components被渲染出来是个什么样的结构。大括号{}中的是JS表达式，this表示当前React Components的实例，props是在使用React Components时在其上添加的属性的集合。若想对React Components添加样式，不能使用class，因为class在JS的ES6标准里已经成为了一个关键字，用来声明一个类，在之前的ES5等的标准里也是一个保留字，因此不能直接在标签上写class，因为这是一个JS的运行环境，如果需要声明标签的类名，则需要换一种写法，用className。还有一种写法，直接使用内联样式，按照传统语法，应该写成style=”color: red”，但是在React中，行内样式不是用字符串的形式表示的，需要用一个样式对象来表示，样式对象的key值是样式名的驼峰标识写法，值是样式的值，因此应该写成style={{color: ‘red'}}，这等价于var styleObj = {color: ‘red'}，引用style={styleObj}。
#### 3.1 内嵌JavaScript表达式
在JSX中可以内嵌任何有效的JavaScript表达式，只需要将该表达式用一对花括号包裹即可，JSX遇到大括号就当作JavaScript表达式来看待。
可以将JSX换行缩进以提高代码的可读性，注意：为了避免分号自动插入的bug，即使只有一行JSX代码，仍建议将JSX代码包裹在小括号里面。

#### 3.2 JSX也是一种JavaScript表达式
Babel将JSX编译为React.createElement()调用，JSX语法中的元素、属性和子节点被转换成React.createElement()的参数。编译完，JSX表达式变成了普通的JS对象，这些对象就是React元素，这些元素详细描述了最终在屏幕上如何展示，React通过这些对象构造DOM，并持续对其更新。这意味着可以在if和for循环里面使用JSX，也可以把JSX赋值给变量，作为一个函数的参数，或作为一个函数的返回值。
React.createElement()会做适当的检查来帮助开发者写0 bug的代码。
#### 3.2.1 React.createElement()的参数
* 第一个参数（必须）：可以是一个html标签名称字符串，也可以是一个React类；
* 第二个参数（可选）：元素的属性值对对象，这些属性可以通过this.props.\*来调用；
* 第三个参数开始（可选）：元素的子节点。

#### 3.2.2 React.createElement()的返回值
一个给定类型的ReactElement元素

#### 3.3 JSX中的HTML标签和React组件
在JSX语法中，遇到HTML标签（以<开头）就用HTML规则解析，遇到代码块（以{开头）就用JavaScript规则解析。
React可以渲染HTML标签或React组件类：要渲染HTML标签，只需在JSX里使用小写字母的标签名；要渲染React组件，只需创建一个大写字母开头的本地变量。即，JSX 使用大、小写来区分React组件类和HTML标签。
注意：由于JSX就是JavaScript，class 和 for 等标识符不建议作为属性名，因此React DOM 使用 className 和 htmlFor作为相应的属性名。

#### 3.4 JSX属性的用法
可以在一对引号中直接将字符串字面量作为属性值，也可以在一对花括号中直接使用JavaScript表达式作为属性值。
对于HTML表单元素中的disabled, required, checked 和 readOnly等Boolean类型的属性，若省略属性的值会导致JSX将其当作 true。
要使用JavaScript表达式作为属性值，只需把表达式用一对花括号包起来，不要用引号”"。

#### 3.5 JSX子元素的用法
如果一个JSX标签为空，像XML一样添加/>自结束即可；也可以在JSX标签中嵌套子元素。

#### 3.6 JSX的新语法——展开属性
如果事先知道组件需要的全部props（属性），可以列出；
如果事先不知道要设置哪些 props，最好不要设置它，原因有两个：
* React不能检查属性类型，即使属性类型有错误也不能得到清晰的错误提示。
* props应该不可变的，修改props可能会导致预料之外的结果。
可以使用 JSX 的新特性——展开属性：...操作符是增强的操作符，已经被 ES6数组支持。
展开属性能被多次使用，也可以和其它属性一起用，但要注意顺序很重要，后面的会覆盖掉前面的。

#### 3.7 JSX自定义HTML属性
问题：在自定义的React组件中使用任意的属性都是被支持的，但如果往原生HTML元素里传入HTML规范里不存在的属性，React不会显示，会出现Warning。
解决：使用加 data-前缀的自定义属性。
例外：以 aria- 开头的网络无障碍属性在原生HTML元素里可以正常使用。

#### 3.8 JSX默认阻止XSS攻击
合法：HTML实体可以插入到JSX的文本中。
```
<div>First &middot; Second</div> {/* 显示：First • Second */}
```
问题：如果想在JSX表达式中显示HTML实体，会遇到二次转义的问题，因为React默认会转义所有字符串，为了防止XSS攻击。
```
<div>{'First &middot; Second'}</div> {/* 显示：First &middot; Second */}
```
解决方法：
* 最简单的方法：直接用Unicode字符，但要确保文件是UTF-8编码且网页也指定为UTF-8编码。
```
<div>{'First • Second'}</div>
```
* 安全的方法：先找到实体的Unicode编号，然后在JSX表达式里使用。
```
<div>{'First \ubb07 Second'}</div>
<div>{'First' + String.fromCharCode(183) + 'Second'}</div>
```
在JSX可以安全的嵌入渲染用户输入的内容；在渲染前React DOM默认对嵌入JSX中的值进行编码，以确保应用中不会被注入任何不安全代码。任何值在渲染之前都会被转换为字符串，以避免XSS攻击。

#### 3.9 JSX注释
当要在一个标签的子节点块添加注释时，要用{/*和*/}包围要注释的部分。

### 4 渲染元素
元素是React apps中最小的构建部件。一个元素描述的是希望屏幕上展示的样子。
不同于浏览器的DOM元素，React元素只是一个对象，并且创建一个元素是非常廉价的。React DOM只需要更新DOM到对应的React元素上。
#### 4.1 在DOM里渲染元素
HTML中的DOM节点称为根DOM节点，因为在它内部的一切都将被React DOM管理。
通常只有一个根DOM节点，当然也可以分离出多个根DOM节点。
通过ReactDOM.render() 方法，可以把React元素渲染到根DOM节点上。

#### 4.2 更新被渲染的元素
React元素是不可变的。创建一个元素后，不能改变它们的子元素或属性。
唯一能更新UI的方式是创建一个新的元素并传给ReactDOM.render()。

#### 4.3 React只更新需要的部分
React DOM会把元素当前的值（包括其子元素）与之前的值进行比较，只会进行必要的更新。
总结：正确的思维方式——应该思考在一个特定的时刻UI应该是什么样子，而不是怎样去改变它。

### 5 组件的属性props
组件能把UI切割成独立的、可重用的部件，可以独立的思考每一个部件。
从概念上看，components 就像Javascript 的函数，允许任意的输入（props），并返回React元素，描述元素应该怎么展示在屏幕上。
#### 5.1 函数式组件及类组件
#### 5.1.1 函数式组件
#### 5.1.1.1 JavaScript函数
定义组件最简单的方法就是写一个Javascript函数，这个函数是一个合法的React元素，因为它接收一个props对象参数并返回一个React元素。这样的组件称为函数式组件。
```
function MyBox(props) {
    return <div>Hello, {props.name}</div>;
}
MyBox.propTypes = {
    name: React.PropTypes.string
};
MyBox.defaultProps = {
    name: "Alice"
};
ReactDOM.render(
    <MyBox />,
    document.getElementById("container")
);
```

#### 5.1.1.2 ES6箭头函数
```
const MyBox = (props) => <div>Hello, {props.name}</div>;
MyBox.propTypes = {
    name: React.PropTypes.string
};
MyBox.defaultProps = {
    name: "Alice"
};
ReactDOM.render(
    <MyBox />,
    document.getElementById("container")
);
```
以上两种函数式组件必须没有任何状态，也没有组件生命周期方法，但仍然可以设置函数属性的方式来指定propTypes和defaultProps。
大多数组件都应该是无状态函数，如果可能，这将是推荐的模式。

#### 5.1.2 类组件
可以使用ES6的class来定义一个组件。
```
class Counter extends React.Component {
    constructor(props) {
        super(props);
        this.state = {count: props.initial};
    }
    count() {
        this.setState({count: this.state.count + 1});
    }
    render() {
        return (
            <div onClick={this.count.bind(this)}>
                Click times：{this.state.count}
            </div>
        );
    }
}
Counter.propTypes = {
    initial: React.PropTypes.number
};
Counter.defaultProps = {
    initial: 0
};
ReactDOM.render(
    <Counter />,
    document.getElementById("container")
);
```
使用ES6 class与React.createClass的不同：
* ES6 class对于方法的定义不需要再使用function关键字，也不需要逗号来分开多个方法。
* React.createClass()提供getInitialState()方法设置初始state，而ES6 class在构造函数constructor里设置初始state。
* React.createClass()提供getDefaultProps()方法和propTypes属性，而在ES6 class中defaultProps和propTypes都被定义为属性且在class body外定义，而不是在class body里。
* React.createClass()会自动绑定this到组件实例上，而ES6 class不会自动绑定this到组件实例上，必须显式使用.bind(this)或箭头函数来手动为方法绑定执行上下文。

#### 5.2 渲染组件
组件可以返回一个元素作为返回值，元素可以是HTML标签，也可以是用户定义的组件。
调用ReactDOM.render()方法渲染组件，当遇到一个用户定义的组件表示的元素时，React会把JSX里的属性作为一个对象传递到组件中，这个对象通常称为props。
组件的名字最好都是大写字母开头的，且要求是一个闭合标签。

#### 5.3 组合组件
组件可以引用其他的组件作为输出。
组件必须返回一个根组件，通常用一个<div>去包住所有的组件。

#### 5.4 切割组件
不要害怕把组件切割成更小的组件。
切割组件可能看起来像麻烦的工作，但对于一个大型的应用来说，拥有一个可重用的组件是一个回报很高的事情。
一个好的经验法则：如果UI中某些组件被重用了多次或者自身就足够复杂，将它们作为可重用的组件是一个好的选择。

#### 5.5 props的作用
props是一种父级向子级传递数据的方式。父、子组件只能通过props来传递数据。

#### 5.6 props的原理
在子组件中，可以使用this.props.\*来获取父组件的state值；在父组件中，更新state，并通过在子组件上使用this.props.*将其传递到子组件上。

#### 5.7 props是只读的
无论声明一个函数组件或一个类组件，它都不能修改props。
UI是动态的并且经常变化的，这就需要引入state。

#### 5.8 props的类型检测propTypes
随着应用的日益变大，保证组件的正确使用显得日益重要，为此引入React.propTypes：React.PropTypes 提供很多验证器来验证传入数据的有效性，当向props传入无效数据时，JavaScript控制台会抛出警告。
注意为了性能考虑，只在开发环境验证 propTypes。
* 声明props为指定的JS基本类型，可传可不传
```
React.PropTypes.array
React.PropTypes.bool
React.PropTypes.func
React.PropTypes.number
React.PropTypes.object
React.PropTypes.string
React.PropTypes.symbol
```
* 声明props为数字、字符串、DOM 元素或包含这些类型的数组或fragment
```
React.PropTypes.node
```
* 声明props为React元素（原生HTML元素或React类）
```
React.PropTypes.element
```
* 声明props为某个指定的类
```
React.PropTypes.instanceOf(MyBox)
```
* 声明props为某些指定值中的一个（用enum的方式）
```
* React.PropTypes.oneOf(['Alice', 'Bruce'])
```
* 声明props为某些类型中的一种
```
React.PropTypes.oneOfType([React.PropTypes.string, React.PropTypes.number, ...])
```
* 声明props为指定类型组成的数组
```
React.PropTypes.arrayOf(React.PropTypes.number)
```
8）声明props为指定类型的属性构成的对象
```
React.PropTypes.objectOf(React.PropTypes.number)
```
9）声明props为特定形状参数的对象
```
React.PropTypes.shape({
    color: React.PropTypes.string,
    fontSize: React.PropTypes.number
})
```
10）声明props为必须的某类型
```
React.PropTypes.number.isRequired
```
11）声明props为必须的任意类型
```
React.PropTypes.any.isRequired
```

### 6 组件间的通信
#### 6.1 父组件向子组件通讯
父组件可以向子组件通过传props的方式，向子组件进行通讯。在父组件中设置相关属性值或方法，子组件通过this.props的方式进行方法调用或属性赋值。

#### 6.2 子组件向父组件通讯
子组件向父组件通讯，同样也需要父组件向子组件传递props进行通讯，只是父组件传递的是作用域为父组件自身的函数，子组件调用该函数，将子组件想要传递的信息作为参数，传递到父组件的作用域中。
此外，为子组件添加ref属性，并进行唯一命名，父组件即可采用refs的方式调用子组件的属性或方法。

#### 6.3 兄弟组件间的通信
兄弟组件间的唯一关联点，是拥有相同的父组件。因此，可以借助父组件实现兄弟组件间的通信——通过父组件回调函数改变兄弟组件的props。可以先由子组件向父组件通信，再由父组件向兄弟组件通信。
上述方法只适用于组件层次很少的情况，当组件层次很深时，其效率就会很低，为此，React官方提供了Redux，通过store来存放应用程序的状态数据，能够在状态发生变化时去通知所发生的变化，新的状态能够被传递给根节点，然后再次发起自上而下的渲染，从而重新渲染DOM。

### 7 组件的状态state
state跟porps很相似，但是state是组件私有的，并受组件控制。
相比函数组件，类组件有一些额外的功能，比如添加state和生命周期。
#### 7.1 函数组件转化为类组件
可以通过5步把函数组件转化为类组件
* 创建一个同名的类组件并且继承React.Component；
* 添加一个空的方法render()；
* 把函数里面的内容移到render方法里面；
* 把render()里面的props替换成this.props；
* 删掉之前的函数声明。

#### 7.2 添加state到类组件：
可以通过3步把props移到state：
* 把render()方法里的this.props.\*替换成 this.state.\*；
* 添加constructor()用来初始化this.state，注意将props作为参数传到constructor，在constructor函数体中执行super(props)；当参数传递给ReactDOM.render()时，React调用组件中的constructor函数。
* 从元素中移除props。

#### 7.3 添加周期方法到类组件：
在有许多组件的应用里， 当组件被销毁的时候释放掉资源是非常重要的。
通常在类组件里声明一些特别的方法，当组件mounting和 unmounting时去运行这些方法，这些方法被称为生命周期方法钩子。
一个典型的实例是：在componentDidMount方法中设置定时器，在componentWillUnmount方法中卸载定时器。

#### 7.4 state的作用：
React 把用户界面当作状态机，根据状态变化渲染用户界面，可以让用户界面和数据保持一致。
state是React组件的一个对象，React只需更新组件的state，然后根据新的state重新渲染用户界面即可，不需要操作DOM。
React 来决定如何最高效地更新 DOM。

#### 7.5 state的原理：
常用的通知React数据变化的方法是调用 setState(data, callback)，这个方法会更新this.state，并重新渲染组件，重新渲染完成后，调用可选的回调函数callback（大部分情况不需要提供callback，React会把用户界面更新到最新状态）。

#### 7.6 哪些组件应该有state？
大部分组件是从props里获取数据并渲染页面，但有时需要对用户输入、服务器请求或时间变化等作出响应，这时才需要state。
将尽可能多的组件无状态化：把state放到最合理的地方。
常用的模式是：创建多个只负责渲染数据的无状态组件，在它们的上层创建一个有状态组件并把state通过props传给子组件，有状态组件封装了所有的用户交互逻辑，而无状态组件只负责声明式地渲染数据。

#### 7.7 哪些数据应该作为state？
state应该包括那些可能被组件的事件处理器改变并触发用户界面更新的数据，一般很小且能被JSON序列化。当创建一个有状态组件时，应该保持数据的精简，将最少的数据存入this.state，在render()里再根据 state 来计算需要的其它数据。

#### 7.8 哪些数据不应该作为state？
* state应该仅包括能表示用户界面状态所需要的最少数据。因此不应该包括：
* 计算所得数据（把所有的计算都放到render()里更容易保证用户界面和数据的一致性）；
* React组件（在render()里使用props和state来创建它）；
* 基于props的重复数据（尽可能用props来作为唯一的数据来源，当且仅当需要知道props以前的值时，才把props保存到state中，因为未来的props可能会变化）。

#### 7.9 正确使用state
#### 7.9.1 别直接修改state的值
修改state的值请用setState()，不要直接对this.state赋值，唯一能声明this.state的地方只有在constructor里。

#### 7.9.2 state的更新有可能是异步的
React为了性能，可能会把批量处理多次的setState()调用在一个的更新里。
因为this.props和this.state可能异步更新了，所有不应该依赖他们的值来计算下一个state。
为了解决这个问题，让setState()接收一个函数比接收一个对象的方式更好，这个函数会把前一个state作为第一个参数，更新的props作为第二参数：
```
this.setState((prevState, props) => ({
    counter: prevState.counter + props.increment
}))
```

#### 7.9.3 state的更新是合并后的
当调用setState()时，React会合并提供的对象到当前的state里，这意味着在state包含几个独立的变量时，能通过单独调用setState()，独立更新它们。

#### 7.10 单向数据流：
所有的父组件或者子组件都不知道一个组件是stateful或stateless的，并且它们也不应该关心自己是被定义成一个函数组件或者是类组件。
一个组件可能会把自己的state作为props传递给他们的子组件中，这就是单向数据流（从上往下）。任何        的state都属于一些特定的组件，并且任何的数据或UI视图只能影响在其组件树下面的组件。
在React app里，无论一个stateful或stateless的组件，组件独立的细节都可能随着时间而改变。可以用stateless组件代替stateful组件，反之亦然。
 
### 8 组件的生命周期
#### 8.1 组件的生命周期分成三个状态
* Mounting：组件被注入真实DOM。一个React Components被render解析生成对应的DOM节点并被插入浏览器的DOM结构的一个过程。当能在浏览器上看到从无到有的时候，mounting已经结束了。mounting结束，就说React Components被mounted了。
* Updating：组件被重新渲染来决定DOM是否应被更新。一个mounted的React Components被重新render的过程。重新渲染的过程并不是说相应的DOM结构就一定会发生改变，React会React Components的当前state和最近一次的state进行对比，只有当state确实发生了改变，并且影响到DOM结构时，React才会去改变对应的DOM结构。
* UnMounting：组件从真实DOM中移除。一个mounted的React Components对应的DOM节点被从DOM结构中移除的过程。

#### 8.2 组件的生命周期方法
#### 8.2.1 五种常见的生命周期方法
* componentWillMount()
* componentDidMount()
* componentWillUpdate(object nextProps, object nextState)
* componentDidUpdate(object prevProps, object prevState)
* componentWillUnmount()
will方法在进入某状态之前调用，did方法在进入某状态之后调用。

#### 8.2.2 两种特殊的生命周期方法
* componentWillReceiveProps(object nextProps)：已加载组件收到新的参数时调用
* shouldComponentUpdate(object nextProps, object nextState)：组件判断是否重新渲染时调用

#### 8.2.3 Mounting状态的生命周期方法：
* getDefaultProps()
* getInitialState()
* componentWillMount()
* render()
* componentDidMount()

#### 8.2.4 Updating状态的生命周期方法
* componentWillReceiveProps(object nextProps)
* shouldComponentUpdate(object nextProps, object nextState)
* componentWillUpdate(object nextProps, object nextState)
* render()
* componentDidUpdate(object prevProps, object prevState)

#### 8.2.5 UnMounting状态的生命周期方法
* componentWillUnmount()

### 9 处理事件
绑定事件的传统方法是使用addEventListener，处理React元素事件跟处理DOM元素事件很相似，但有一些不同：
* React是通过on驼峰式命名的方式来绑定事件的，而不是通过全小写的HTML属性在HTML标签上绑定事件。
* 绑定事件的组件并不是真实的DOM节点，只是一个React Element。事件处理函数往往需要拿到某个标签的内容，这可以通过refs属性来实现：通过refs属性给子组件起名，通过this.refs.子组件名可以索引到该组件，但是，要操作真实的DOM节点，如修改其样式，需要用到ReactDOM封装的findDOMNode()方法，参数是React Components。
* JSX里要传递函数给事件处理，而不是字符串，也即应该用花括号而不能用引号。
* 事件处理函数的参数的参数可以是React封装的event对象，event对象是在原生的JS的event基础上封装的，所以它同时具有了原生event方法，如阻止浏览器的默认行为、阻止冒泡等。不能通过return false来阻止默认事件，必须显示地调用e.preventDefault，这里的e是合成事件，React定义这些合成事件是根据W3C标准的，所以不需要担心浏览器兼容问题。
* 当用React时，当DOM被创建时，一般都不需要调用addEventListener来添加监听器到DOM上，相反，只需要在元素最初被渲染时添加一个监听器。当用es6的class定义一个组件时，一个通常的模式是在class上把事件处理定义成一个方法。
* 注意this的指向，在事件处理函数内部的this不指向该组件，通常的解决方法是对事件处理函数进行bind(this)操作或使用箭头函数。

[<< 回到主页](http://suzy1993.github.io/misszy/)
