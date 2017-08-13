[<< 回到主页](http://suzy1993.github.io/misszy/)

### 1 Redux介绍
Redux是JavaScript状态容器，提供可预测化的状态管理。
#### 1.1 Redux的要点
* 所有的state都以一个对象树的形式储存在一个单一的store中。
* 惟一改变state的办法是触发action，一个描述发生什么的对象。
* 为了描述action如何改变state树，需要编写reducers。

#### 1.2 Redux的动机
随着JavaScript单页应用开发日趋复杂，JavaScript需要管理比任何时候都要多的state，管理不断变化的state非常困难，如果一个model的变化会引起另一个model变化，那么当view变化时，就可能引起对应model以及另一个model的变化，依次地，可能会引起另一个view的变化。
Redux试图让state的变化变得可预测。

#### 1.3 Redux的三大原则
#### 1.3.1 单一数据源
整个应用的state被储存在一棵对象树中，且这个对象树只存在于唯一的一个store中。

#### 1.3.2 state是只读的
唯一改变state的方法是触发action，action是一个用于描述已发生事件的普通对象。

#### 1.3.3 使用纯函数来执行修改
为了描述action如何改变state tree，需要编写reducers。

#### 1.4 Redux的先前技术——Flux
#### 1.4.1 Redux与Flux的联系
Redux的灵感来源于Flux的几个重要特性。和Flux一样，Redux 规定，将模型的更新逻辑全部集中于一个特定的层（Flux里的store，Redux里的reducer）。Flux 和 Redux 都不允许程序直接修改数据，而是用一个叫作 “action” 的普通对象来对更改进行描述。

#### 1.4.2 Redux与Flux的不同
不同于Flux，Redux并没有dispatcher的概念，原因是它依赖纯函数来替代事件处理器，纯函数构建简单，也不需额外的实体来管理它们。Flux常常被表述为 (state, action) => state，从这个意义上说，Redux无疑是Flux架构的实现，且得益于纯函数而更为简单。
和Flux的另一个重要区别，是Redux设想永远不会变动数据，可以很好地使用普通对象和数组来管理state ，而不是在多个reducer里变动数据。正确且简便的方式是，在reducer中返回一个新对象来更新state。

### 2 Redux基础
#### 2.1 Action
Action是把数据从应用传到store的有效载荷，它是store数据的唯一来源，一般来说会通过store.dispatch()将action传到store。
Action本质上是JavaScript普通对象。约定，action内必须使用一个字符串类型的 type字段来表示将要执行的动作。多数情况下type会被定义成字符串常量，当应用规模越来越大时，建议使用单独的模块或文件来存放action。除了type字段外，action对象的结构完全由自己决定。
应该尽量减少在action中传递的数据，如传递对象的index就比传递整个对象好。
Action创建函数就是生成action的方法，action和action创建函数这两个概念很容易混在一起，使用时最好注意区分。
在传统的Flux实现中，当调用action创建函数时，一般会触发一个dispatch：
```
function addTodoWithDispatch(text) {
    const action = {
        type: ADD_TODO,
        text
    }
    dispatch(action)
}
```
在Redux中，action创建函数只是简单的返回一个action，Redux只需把action创建函数的结果传给dispatch()方法，即可发起一次dispatch过程：
```
function addTodo(text) {
    return {
        type: ADD_TODO,
        text
    }
}
dispatch(addTodo(text))
```
或者创建一个被绑定的action创建函数来自动dispatch：
```
function addTodo(text) {
    return {
        type: ADD_TODO,
        text
    }
}
const boundAddTodo = (text) => dispatch(addTodo(text))
boundAddTodo(text);
```
store里能直接通过store.dispatch()调用dispatch()方法，但多数情况下会使用 react-redux提供的connect()帮助器来调用，bindActionCreators()可以自动把多个action创建函数绑定到dispatch()方法上。

#### 2.2 Reducer
Action只是描述了有事情发生这一事实，并没有指明应用如何更新state。而这正是reducer要做的事情。
确定了 state 对象的结构，就可以开始开发 reducer。reducer 就是一个纯函数，接收旧的 state 和 action，返回新的 state。
保持 reducer 纯净非常重要。永远不要在 reducer 里做这些操作：
* 修改传入参数；
* 执行有副作用的操作，如API 请求和路由跳转；
* 调用非纯函数，如Date.now() 或 Math.random()。
只要传入参数相同，返回计算得到的下一个 state 就一定相同。没有特殊情况、没有副作用，没有 API 请求、没有变量修改，单纯执行计算。
Redux 首次执行时，state 为 undefined，此时我们可借机设置并返回应用的初始 state。一个技巧是使用ES6默认参数语法来精简代码。
注意：
* 不要修改 state。 使用 Object.assign() 新建了一个副本时，state不能作为Object.assign()的第一个参数，因为它会改变第一个参数的值。
* 在 default 情况下返回旧的 state。遇到未知的 action 时，一定要返回旧的 state。
createStore()的第二个参数用于设置state初始状态，主reducer并不需要设置初始化时完整的state，因此createStore()的第二个参数是可选的，初始时如果传入 undefined, 子 reducer 将负责返回它们的默认值。
可开发一个函数combineReducers()来作为主reducer，它调用多个子reducer分别处理state中的一部分数据，再把这些数据合成一个大的单一对象。注意每个reducer只负责管理全局state中它负责的一部分。每个reducer的state参数都不同，分别对应它管理的那部分state数据。使用 将多个reducer成为一个。combineReducers()所做的只是生成一个函数，这个函数来调用一系列reducer，每个reducer根据它们的key来筛选出state中的一部分数据并处理，然后这个生成的函数再将所有reducer的结果合并成一个大的对象。如果combineReducers()中包含的所有reducers都没有更改state，那么也就不会创建一个新的对象。combineReducers()接收一个对象，可以把所有顶级的reducer放到一个独立的文件中，通过export暴露出每个reducer函数，然后使用import * as reducers得到一个以它们名字作为key的object。

#### 2.3 Store
Store就是把Action和Reducer联系到一起的对象。Store有以下职责：
* 维持应用的state；
* 提供getState()方法获取state；
* 提供dispatch(action)方法更新state；
* 通过subscribe(listener)注册监听器;
* 通过subscribe(listener)返回的函数注销监听器。

#### 2.4 数据流
严格的单向数据流是Redux架构的设计核心，这意味着应用中所有的数据都遵循相同的生命周期。
Redux应用中数据的生命周期遵循下面 4 个步骤：
* 调用store.dispatch(action)
Action就是一个描述“发生了什么”的普通对象。
* store调用传入的reducer函数
store会把两个参数传入reducer：当前的state树和action。
* 根reducer应该把多个子reducer输出合并成一个单一的state树
根reducer的结构完全由自己决定。Redux原生提供combineReducers()辅助函数，来把根reducer拆分成多个函数，用于分别处理state树的一个分支。
* Redux store保存了根reducer返回的完整state树
这个新的树就是应用的下一个 state！所有订阅store.subscribe(listener)的监听器都将被调用；监听器里可以调用store.getState() 获得当前 state。
现在，可以应用新的state来更新 UI。如果使用React Redux这类的绑定库，这时就应该调用component.setState(newState)来更新。

#### 2.5 搭配React
Redux和React之间没有关系。Redux支持React、Angular、Ember、jQuery 甚至纯JavaScript，只是Redux和React这类框架搭配来用最好，因为这类框架允许以state函数的形式来描述界面，Redux通过action的形式来发起state变化。
Redux默认并不包含react-redux这一React绑定库，需要单独安装。
<table>
    <tr><td></td><td>展示组件</td><td>容器组件</td></tr>
    <tr><td>作用</td><td>描述如何展现（骨架、样式）</td><td>描述如何运行（数据获取、状态更新）</td></tr>
    <tr><td>直接使用Redux</td><td>否</td><td>是</td></tr>
    <tr><td>数据来源</td><td>props</td><td>监听Redux state</td></tr>
    <tr><td>数据修改</td><td>从props调用回调函数</td><td>向Redux派发actions</td></tr>
    <tr><td>调用方式</td><td>手动</td><td>通常由React Redux生成</td></tr>
</table>
大部分的组件都应该是展示型的，但一般需要少数的几个容器组件把它们和store连接起来。 
技术上讲，可以直接使用store.subscribe()来编写容器组件，容器组件就是使用store.subscribe()从state树中读取部分数据，并通过props来把这些数据提供给要渲染的组件。可以手工来开发容器组件，但不建议这么做，因为这样就无法使用React Redux带来的性能优化，建议使用React Redux的connect()方法来生成，这个方法做了性能优化来避免很多不必要的重复渲染。 
使用connect()前，需要先定义mapStateToProps这个函数来指定如何把当前 store state映射到展示组件的props中。除了读取state，容器组件还能分发action。类似的方式，可以定义mapDispatchToProps()方法接收dispatch()方法并返回期望注入到展示组件的props中的回调方法。
所有容器组件都可以访问store，所以可以手动监听它。一种方式是把它以props的形式传入到所有容器组件中，但这太麻烦了，建议的方式是使用指定的React Redux组件<Provider>来让所有容器组件都可以访问store，而不必显式地传递它，只需要在渲染根组件时使用即可。

[<< 回到主页](http://suzy1993.github.io/misszy/)
