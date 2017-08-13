[<< 回到主页](http://suzy1993.github.io/misszy/)

## Redux 核心概念

### 1 Immutable State
Redux没有规定用什么方式来保存State，可能是Javascript对象，或是Immutable.js的数据结构。但是有一点，最好确保State中每个节点都是Immutable的，这样将确保在判断数据是否变化时，只要简单地进行引用比较即可，从而避免 Deep Equal 的遍历过程。
为了确保这一点，在reducer中更新State成员，需要遵循以下的方式，遵循这样的方式，无需Immutable.js也可以让State是Immutable的。
是：
```
score = {…score, {computer: 95}};
```
而不是：
```
score.computer = 95;
```
是：
```
students = […students, {name: ‘Alice’}];
```
而不是：
```
students.push({name: ‘Alice’});
```

### 2 reducer
* reducers根据传入的action.type分别更新不同的State字段。
* 当应用程序中存在很多action.type时，通过一个reducer和巨型switch显然会产生难以维护的代码。此时，比较好的方法就是通过combineReducers组合小的reducer来产生大的reducer。
* 响应的返回结果也会被合并到对应的State字段中。每个reducer如果遇到自己不能处理的action，那么必须原样返回传入的state，或设定的初始状态（如果传入的state是undefined）。
* 使用combineReducers的前提是，每一个被组合的小reducer仅仅和State的一部分数据相关，只负责处理State的一部分字段，而看不到State下的其他字段的内容。
* 减少reducer的样板代码:每次写action/action creator/reducer，都会写很多相似度很高的代码，可以通过一定封装，来减少这些样板代码：
```
function createReducer(initialState, handlers) {
    return function reducer(state = initialState, action) {
        if (handlers.hasOwnProperty(action.type)) {
            return handlers[action.type](state, action);
        } else {
            return state;
        }
    }
}
const todosreducer = createReducer([], {
    'ADD_TODO': addTodo,
    'TOGGLE_TODO': toggleTodo
});
```
* 特殊：有时需要在一个reducer之中访问另外一个reducer负责的State，也就是说，某些action需要触发跨reducers的状态改变，这需要创建更上一层的reducer（Root Reducer）来控制这个过程，因此，不能依靠combineReducers来完成这种需求，而是需要自己写Root Reducer。reduce-reducers也可以帮助完成类似的任务。
```
var reducers = reduceReducers(
    combineReducers({
        reducer1,
        reducer2,
        ...
    }),
    (state, action) => {
        switch (action.type) {
            case '***’:
                const state1 = state.reducer1;
                const state2 = state.reducer2;
                ...
        }
    }
);
```
注意，由于reduce-reducers组合（每个参数就是一个reducer）的每一个reducer都可以获取整个State，所以请不要滥用，在大部分情况下，如果严格遵循数据范式，通过计算的方法获得跨越reducers的状态是推荐的方法。

### 3 action
在Redux中，改变State只能通过action。并且，每一个action都必须是Javascript Plain Object。
事实上，创建action对象很少用每次直接声明对象的方式，更多地是通过一个创建函数。这个函数被称为Action Creator。
Action Creator看起来很简单，但如果结合上Middleware就可以变得非常灵活。

### 4 Higher-Order Store
有些时候需要对整个Store对象都进行扩充，这就引入了Higher-Order Store的概念。
Higher-Order Store提供一个函数，接受Store对象作为输入参数，产生一个新的Store对象作为返回值。
Redux建议在middleware不能满足扩展要求的前提下再使用Higher-Order Store。

### 5 Binding To React (React-Native)
* Redux解决的是应用程序状态存储以及如何变更的问题，至于怎么用，则依赖于其他模块。
* react-redux是React Components如何使用Redux的Binding。
* Components的嵌套：
可以在组件树的任何一个层次调用connect来为下层组件绑定状态和dispatch方法，但仅在顶层组件调用connect进行绑定是首选的方法。
* Provider Component：
connect函数其实并不知道Redux Store对象在哪里，所以需要有一个机制让connect知道从哪里获得Store对象，这是通过Provider Component来设定的，Provider Component也是react-redux提供的工具组件。
Provider Component应该是React Components树的根组件。
* Provider Component 和connect函数的配合，使得React Component在对Redux完全无感的情况下，仅通过React自身的机制来获取和维护应用程序的状态。
* selector：
connect中的mapStateToProps是函数通过返回一个映射对象，指定了哪些Store/State属性被映射到React Component的this.props，这个方法被称为 selector。
selector的作用就是为React Components构造适合自己需要的状态视图。selector的引入，降低了React Component对Store/State数据结构的依赖，利于代码解耦；同时由于 selector 的实现完全是自定义函数，因此也有足够的灵活性（如对原始状态数据进行过滤、汇总等）。
reselect提供了带cache功能的selector。如果Store/State和构造view的参数没有变化，那么每次Component获取的数据都将来自于上次调用/计算的结果。这得益于Store/State Immutable的本质，状态变化的检测是非常高效的。

### 6 总结
* Redux和React没有直接关系，它瞄准的目标是应用状态管理。
* Redux核心概念是Map/Reduce中的Reduce，且reducer的执行是同步的，产生的State是Immutable的。
* 改变State只能通过向reducer dispatch actions来完成。
* State的不同字段，可以通过不同的reducers来分别维护，combineReducers负责组合这些reducers，前提是每个reducers只能维护自己关心的字段。
* action对象只能是JavaScript Plain Object，但通过在Store上装载middleware，则可以任意定义action对象的形式，最终会有特定的middleware负责将此action对象变为JavaScript Plain Object。
* Redux仅仅专注于应用状态的维护，reducer、dispatch/middleware是两个常用扩展点，Higher-order Store仅针对需要扩展全部Store功能时使用。
* react-redux是Redux针对React/React-Native的Binding，connect/selector是扩展点，负责将Store中的状态添加到React Component的props中。

[<< 回到主页](http://suzy1993.github.io/misszy/)
