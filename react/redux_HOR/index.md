### 参考网址：
[https://zhuanlan.zhihu.com/p/21648398?refer=turborender](https://zhuanlan.zhihu.com/p/21648398?refer=turborender)
[https://segmentfault.com/a/1190000010205508](https://segmentfault.com/a/1190000010205508)

### 1 高阶reducer的定义
高阶reducer指的是一个函数，该函数接收一个reducer函数作为参数或者返回一个reducer函数作为函数的返回值。
高阶reducer也可以被看作一个reducer工厂，combineReducers是高阶reducer一个典型的例子。
可以使用高阶reducer函数来创建一个符合自己要求的reducer的函数。

### 2 高阶reducer的应用场景
#### 2.1 应用场景一：不同的reducer具有相同的逻辑
当应用功能变大时，在reducer函数中那些通用的逻辑就会出现重复，容易发现很多reducer都是处理同样的逻辑，只是处理的数据不同而已。因此会想到重复使用reducer函数中那些通用的逻辑，从而减小应用的代码。
#### 2.1.1 实例场景
有一个课程列表和一个排班列表。
reducers/classes.js：
```
export const classes = (state, { type, payload }) => {
    switch (type) {
        case 'classes/FETCH_SUCCESS':
            return {
                ...state,
                loading: false,
                list: payload,
            };
        default:
            return state;
        }
    }
};
```
reducers/jobs.js：
```
export const jobs = (state, { type, payload }) => {
    switch (type) {
        case ‘jobs/FETCH_SUCCESS':
            return {
                ...state,
                loading: false,
                list: payload,
            };
        default:
            return state;
        }
    }
}
```

#### 2.1.2 高阶reducer的用法
两个reducer几乎是一样的，可以写一个reducer来把两个reducer相同的部分提取出来：
reducers/list.js：
```
export const list = (actionType) => {
    return (state=initialState, { type, payload }) => {
        switch (type) {
            case actionType:
                return {
                    ...state,
                    loading: false,
                    list: payload,
                };
            default:
                return state;
        }
    };
};
```
reducers/classes.js：
```
const reducer = (state, action) => {
    switch (action.type) {
        // 其他逻辑
        default:
            return state;
    }
};
export const classes = (state, action) => {
    state = list(‘classes/FETCH_SUCCESS')(state, action);
    return reducer(state, action);
};
```
reducers/jobs.js：
```
const reducer = (state, action) => {
    switch (action.type) {
        // 其他逻辑
        default:
            return state;
    }
};
export const jobs = (state, action) => {
    state = list('jobs/FETCH_SUCCESS')(state, action);
    return reducer(state, action);
};
```
随着抽象的东西越来越多，reducer会变成这样：
reducers/classes.js：
```
const reducer = (state, action) => {
    switch (action.type) {
        // 其他逻辑
        default:
            return state;
    }
};
export const classes = (state, action) => {
    state = list('classes/FETCH_SUCCESS')(state, action),
    state = query('classes/FETCH_SUCCESS')(state, action),
    state = pagination('classes/FETCH_SUCCESS')(state, action),
    return reducer(state, action),
};
```
实现一个composeReducers组合这些reducer：
```
const composeReducers = (…reducers) => {
    return (state = initialState, action) => {
        if (reducers.length === 0) {
            return state;
        }
        const last = reducers[reducers.length - 1];
        const rest = reducers.slice(0, -1);
        return rest.reduceRight((enhanced, reducer) => reducer(enhanced, action), last(state, action));
    };
};
const reducer = (state, action) => {
    switch (type) {
        // 其他逻辑
        default:
            return state;
    }
};
export const classes = composeReducers(
    reducer,
    list('classes/FETCH_SUCCESS'),
    query('classes/FETCH_SUCCESS'),
    pagination('classes/FETCH_SUCCESS’),
);
```

#### 2.2 应用场景二：相同的reducer具有不同的逻辑
有时候想要在store中处理特定类型数据的多个“实例”，如counter函数需要用于多个地方。redux的全局store可以很容易跟踪整个应用的state状态（for循环，所有的reducer都执行了一遍），但是，很难针对特定的action来更新特定的state的某一部分数据，特别是当使用combineReducers（因为dispatch时，采用的是for循环来对每一个reducer都进行了执行）。
#### 2.2.1 实例场景
想要跟踪应用中多个counter实例，分别为counterA、counterB、counterC。首先定义counter这个reducer，同时使用combineReducers来管理状态：
```
const counter = (state = 0, action) => {
    switch (action.type) {
        case 'INCREMENT':
            return state + 1;
        case 'DECREMENT':
            return state - 1;
        default:
            return state;
    }
};
const rootReducer = combineReducers({
    counterA : counter,
    counterB : counter,
    counterC : counter,
});
```
问题：以上代码是错误的，每个reducer都会处理同样的action。原因是combineReducer采用的是for循环来对所有的reducer使用相同的action都执行一遍，因此如果dispacth({type:"INCREMENT"})，则counterA、counterB、counterC都会执行一遍，而不是执行某一个reducer函数。
解决：需要使用一种机制来保证只有一个reducer会执行，这种机制就是高阶reducer。

#### 2.2.2 高阶reducer的用法
指定一个reducer最常用的方式就是使用一个后缀或前缀来产生一个reducer的action，或者将额外的信息添加到action对象上。
#### 2.2.2.1 常规高阶reducer函数
可以使用任意一个函数来产生自己的reducer，然后dispatch一个action，而该action只会影响关心的那部分的state的值。
```
const createCounterWithName = (counterName = '') => {
    return (state = 0, action) => {
        switch (action.type) {
            case `INCREMENT_${counterName}`:
                return state + 1;
            case `DECREMENT_${counterName}`:
                return state - 1;
            default:
                return state;
        }
    };
};
const rootReducer = combineReducers({
    counterA : createCounterWithName('A'),
    counterB : createCounterWithName('B'),
    counterC : createCounterWithName('C'),
});
```
#### 2.2.2.2 高阶reducer的通用库multireducer
使用multireducer可以重用相同的reducer，解决dispatch一个action后，所有的reducer都执行一遍的情况。具体参见API。
multireducer可以让把同一个reducer挂载到不同状态树节点，而且自动处理对应的action。
```
import multireducer from 'multireducer';
const counter = (state = 0, action) => {
    switch (action.type) {
        case 'INCREMENT':
            return state + 1;
        case 'DECREMENT':
            return state - 1;
        default:
            return state;
    }
};
const reducer = combineReducers({
    posts: multireducer({
        counterA: counter,
        counterB: counter,
        counterC: counter,
    })
});
```

#### 2.2.3 高阶reducer的好处
只用提供一个reducer函数，但却可以复用这部分的逻辑。只要dispatch的这个action明确指定需要改变哪一部分state状态即可。
