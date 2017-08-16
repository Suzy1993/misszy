[<< 回到主页](http://suzy1993.github.io/misszy/)

## Redux reselect

### 1 selector
mapStateToProps也称为selector，在store发生变化时都会被调用，也就是说，无论是不是selector关心的数据发生变化它都会被调用，因此如果selector计算量非常大，每次更新都重新计算可能会带来性能问题。
* selector可以计算派生数据，允许Redux存储最小可能的状态。
* selector效率高， 除非其中一个参数发生变化，否则不会重新计算选择器。
* selector是可组合的，它们可以用作其他选择器的输入。
selector实际上就是一个纯函数。

```
selector(state) => props
```

### 2 reselect
reselect其实就是个state选择器，可以从store里面方便地将需要的数据抽出。
reselect的原理：每次调用selector函数之前，它会判断参数与之前缓存的是否有差异，若无差异，则直接返回缓存的结果，反之则重新计算。
reselect能省去没必要的重新计算：reselect提供createSelector函数来创建可记忆的selector。createSelector接收一个input-selectors数组和一个转换函数作为参数。如果state tree的改变会引起input-selector值变化，那么selector会调用转换函数，传入input-selectors作为参数，并返回结果。如果input-selectors的值和前一次的一样，它将会直接返回前一次计算的数据，而不会再调用一次转换函数。这样就可以避免不必要的计算，为性能带来提升。
eg1：
```
import { createSelector } from 'reselect';
import React from 'react';
import PropTypes from 'prop-types';
import { connect }from 'react-redux';
import { increase, decrease } from '../actions';
class Counter extends React.Component {
    render() {
        return (
            <div>
                <h3>Counter：{this.props.counter}</h3>
                <button onClick={() => this.props.increase()}>+</button>
                <button onClick={() => this.props.decrease()}>-</button>
            </div>
        );
    }
}
const { number } = PropTypes;
Counter.propTypes = {
    counter: number,
};
const countSelector = createSelector(
    state => state.counter,
    counter => {
        console.log("The state counter has been changed!");
        return counter;
    }
);
const mapStateToProps = state => ({
    counter: countSelector(state),
});
export default connect(
    mapStateToProps,
    {
        increase,
        decrease,
    }
)(Counter);
```
eg2：
```
import { createSelector } from ‘reselect';
const shopItemsSelector = state => state.shop.items;
const taxPercentSelector = state => state.shop.taxPercent;
const subtotalSelector = createSelector(
    shopItemsSelector,
    items => items.reduce((acc, item) => acc + item.value, 0)
);
const taxSelector = createSelector(
    subtotalSelector,
    taxPercentSelector,
    (subtotal, taxPercent) => subtotal * (taxPercent / 100)
);
export const totalSelector = createSelector(
    subtotalSelector,
    taxSelector,
    (subtotal, tax) => ({ total: subtotal + tax })
);
let exampleState = {
    shop: {
        taxPercent: 8,
        items: [
            { name: 'apple', value: 1.20 },
            { name: 'orange', value: 0.95 },
        ]
    }
};
console.log(subtotalSelector(exampleState)); // 2.15
console.log(taxSelector(exampleState)); // 0.172
console.log(totalSelector(exampleState)); // { total: 2.322 }
```
eg3：改进的todos
```
import { createSelector } from 'reselect';
import React from 'react';
import PropTypes from 'prop-types';
import { connect } from 'react-redux';
import { toggleTodo } from '../actions';
import Todo from '../components/Todo';
class VisibleTodoList extends React.Component {
    render() {
        const { todos, toggleTodo } = this.props;
        return (
            <ul>
                {todos.map(todo =>
                    <Todo
                        key={todo.id}
                        {...todo}
                        onClick={() => toggleTodo(todo.id)}
                    />
                )}
            </ul>
        );
    }
}
const { array, func } = PropTypes;
VisibleTodoList.propTypes = {
    todos: array,
    toggleTodo: func,
};
const todoSelector = createSelector(
    state => state.todos,
    todos => {
        console.log("The state todos has been changed!");
        return todos.filter(todo => todo.completed);
    }
);
const mapStateToProps = (state) => ({
    todos: todoSelector(state),
});
export default connect(
    mapStateToProps,
    {
        toggleTodo,
    }
)(VisibleTodoList);
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
