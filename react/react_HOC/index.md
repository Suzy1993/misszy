[<< 回到主页](http://suzy1993.github.io/misszy/)

### 参考网址
[http://www.css88.com/react/docs/higher-order-components.html](http://www.css88.com/react/docs/higher-order-components.html)  
[https://zhuanlan.zhihu.com/p/24776678?group_id=802649040843051008](https://zhuanlan.zhihu.com/p/24776678?group_id=802649040843051008)  
[https://zhuanlan.zhihu.com/p/27985956](https://zhuanlan.zhihu.com/p/27985956)  
[http://blog.csdn.net/cqm1994617/article/details/54800360](http://blog.csdn.net/cqm1994617/article/details/54800360)  
[http://www.cnblogs.com/libin-1/p/7087605.html](http://www.cnblogs.com/libin-1/p/7087605.html)  
[http://www.jianshu.com/p/4780d82e874a](http://www.jianshu.com/p/4780d82e874a)  

### 1 高阶组件
高阶函数：higher-order function在函数式编程中是一个基本的概念 ，它描述的是这样的一种函数：接受函数作为输入，或是输出一个函数。常用的工具方法map、reduce和sort等都是高阶函数。
高阶组件类似于高阶函数，通俗来说，当React组件被包裹时（wrapped），高阶组件会返回一个增强（enhanced）的React组件。高阶组件让代码更有复用性、逻辑性与抽象特性。它可以对render方法作劫持，也可以控制props与state。
在React中，高阶组件是React应用中重用组件逻辑的一项高级技术，最大的特点就是重用组件逻辑。高阶组件并不是React API的一部分，而是由React的组合特性衍生出来的一种设计模式。
具体来说，高阶组件是一个函数，能够接受一个组件并返回一个新的组件。组件是将props转化成UI，然而高阶组件将一个组价转化成另外一个组件。
高阶组件既不会修改输入组件，也不会通过继承来复制行为。相反，通过包裹的形式，高阶组件将原先的组件组合在container组件中。高阶组件是纯函数，没有副作用。
被包裹的组件接受container的所有props和新的props，并使用其渲染输出。高阶组件并不关心数据将会如何或者为什么使用，并且被包裹的元素并不关心数据的源头。
装饰者模式：高阶组件可以看做是装饰者模式（Decorator Pattern）在React的实现，即允许向一个现有的对象添加新的功能，同时又不改变其结构，属于包装模式（Wrapper Pattern）的一种。ES7中添加了一个decorator的属性，使用@符号表示，decorator可以通过返回特定的descriptor 来“修饰” 类属性，也可以直接”修饰”一个类，即传入一个已有的类，通过decorator函数“修饰”成了一个新的类。使用decorator可以更精简地书写高阶组件。但是，这是存在兼容性问题的，通常都是通过babel去编译：babel提供了plugin，高阶组件用的是类装饰器，所以用transform-decorators-legacy。

### 2 高阶组件最常见的签名
```
react-redux中的connect就是一个高阶组件。
const ContainerComponent = connect(stateSelector, actionsType)(WrappedComponent);
```
拆分得到：
```
const enhance = connect(stateSelector, actionsType); // connect是一个返回函数的函数，返回的函数就是一个高阶组件
const ContainerComponent = enhance(WrappedComponent); // 该高阶组件返回一个与Redux Store关联起来的新组件
```
可见，connect是一个返回高阶组件的高阶函数！

### 3 实现高阶组件的两种方法
#### 3.1 属性代理（Props Proxy）
属性代理：通过被包裹的React组件来操作props。
属性代理是常见高阶组件的实现方法。
HOC在render方法中返回了一个WrappedComponent类型的React Element，还可以传入HOC接收到的props，这就是名字Props Proxy的由来。
当使用属性代理构建高阶组件时，执行生命周期的过程类似于堆栈调用：
didmount —> HOC didmount —> (HOCs didmount) —> (HOCs will unmount) —> HOC will unmount —> will unmount
#### 3.1.1 控制props
可以读取、增加、编辑或移除从WrappedComponent传进来的props，但需要小心编辑重要的props，应该尽可能对高阶组件的props作新的命名以防止混淆。
```
import React, { Component } from 'react';
const ControlProps = (WrappedComponent) =>
    class extends Component {
        render() {
            const newProps = {
                text: 'react',
            }
            return (
                <WrappedComponent {...this.props} {...newProps} />
            );
        }
    };
class MyComponent extends Component {
    render() {
        return (
            <div name={this.props.name}>
                <h5>ControlProps：</h5>
                {this.props.text}
            </div>
        );
    }
}
export default ControlProps(MyComponent);
```
使用：
```
class App extends Component {
    render() {
        return <ControlProps name={this.props.name} />;
    }
}
export default App;
```

#### 3.1.2 通过refs使用引用
在高阶组件中，可以接受refs使用WrappedComponent的引用，以拿到一份WrappedComponent实例的引用，这就可以方便地用于读取或增加实例的props，并调用实例的方法。
```
import React, { Component } from 'react';
const Refs = (WrappedComponent) =>
    class extends Component {
        func() {
            console.log(this.props);
        }
        render() {
            const newProps = {ref: () => this.func()};
            return (
                <WrappedComponent {...this.props} {...newProps} />
            );
        }
    };
class MyComponent extends Component {
    return (
        <div>
            <h5>Refs：</h5>
            <input type="text" {...this.props} />
        </div>
    );
}
export default Refs(MyComponent);
```
使用：
```
class App extends Component {
    render() {
        <Refs name={this.props.name} />
    }
}
export default App;
```

#### 3.1.3 抽象state
可以通过WrappedComponent提供的props和回调函数抽象state，即可以通过传入的props和回调函数把state提取出来放到HOC里，实现将原组件抽象为简单、只负责展示的展示型组件，分离内部状态。
```
import React, { Component } from 'react';
const AbstractState = (WrappedComponent) =>
    class extends Component {
        constructor(props) {
            super(props);
            this.state = {
                name: '',
            };
            this.onNameChange = this.onNameChange.bind(this);
        }
        onNameChange(event) {
            this.setState({
                name: event.target.value,
            })
        }
        render() {
            const newProps = {
                prop: {
                    value: this.state.name,
                    onChange: this.onNameChange,
                },
            }
            return <WrappedComponent {...this.props} {...newProps} />;
        }
    };
class MyComponent extends Component {
    render() {
        return (
            <div>
                <h5>AbstractState：</h5>
                <input type="text" {...this.props.prop} />
            </div>
        );
    }
}
export default AbstractState(MyComponent);
```
使用：
```
class App extends Component {
    render() {
        <AbstractState />
    }
}
export default App;
```

#### 3.1.4 使用其他元素包裹WrappedComponent
可以使用其他元素包裹WrappedComponent，这既可以是为了加样式，也可以是为了布局。
```
import React, { Component } from 'react';
const WrapForCSS = (WrappedComponent) =>
    class extends Component {
        render() {
            const newProps = {
                text: 'react',
            }
            return (
                <div style={{marginLeft: '50px'}}>
                    <WrappedComponent {...this.props} {...newProps} />
                </div>
            );
        }
    };
class MyComponent extends Component {
    render() {
        return (
            <div name={this.props.name}>
                <h5>WrapForCSS：</h5>
                {this.props.text}
            </div>
        );
    }
}
export default WrapForCSS(MyComponent);
```
使用：
```
class App extends Component {
    render() {
        <WrapForCSS />
    }
}
export default App;
```

#### 3.2 反向继承（Inheritance Inversion）
高阶组件继承于被包裹的React组件。
高阶组件返回的组件继承于WrappedComponent，因为被动地继承了WrappedComponent，所有的调用都会反向，这也是这种方法的由来。
与属性代理不太一样，反向继承通过super来顺序调用。因为依赖于继承的机制，HOC的调用顺序和队列是一样的：
didmount —> HOC didmount —> (HOCs didmount) —> will unmount —> HOC will unmount —> (HOCs will unmount)
在反向继承方法中，高阶组件可以使用WrappedComponent引用，这意味着它可以使用WrappedComponent的state、props、生命周期和render方法。但它不能保证完整的子组件树被解析。
#### 3.2.1 渲染劫持
渲染劫持指的是高阶组件可以控制WrappedComponent的渲染过程， 渲染各种各样的结果。
通过渲染劫持可以：
#### 3.2.1.1 在由render输出的任何React元素输出的结果中读取、增加、修改、删除props
```
import React, { Component } from 'react';
const OperateProps = (WrappedComponent) =>
    class extends WrappedComponent {
        render() {
            const elementsTree = super.render();
            let newProps = {};
            const newTree = React.cloneElement(elementsTree, elementsTree.props, elementsTree.props.children.map((ele, index) => {
                if (ele && ele.type === "input") {
                    newProps = {value: 'Value is modified', key: index};
                    return React.cloneElement(ele, Object.assign({}, ele.props, newProps), ele.props.children);
                } return ele;
            }));
            return newTree;
        }
    };
class MyComponent extends Component {
    handlerChange() {
    }
    render() {
        return (
            <div>
                <h5>OperateProps：</h5>
                <input type="text" value="First value" onChange={ () => this.handlerChange() } />
                <input type="text" value="Second value" onChange={ () => this.handlerChange() } />
            </div>
        )
    }
}
export default OperateProps(MyComponent);
```
使用：
```
class App extends Component {
    render() {
       <OperateProps />
    }
}
export default App;
```

#### 3.2.1.2 读取或修改由render输出的React元素树；
```
import React, { Component } from 'react';
const OperateElementTree = (WrappedComponent) =>
    class extends WrappedComponent {
        render() {
            const elementsTree = super.render();
            let newProps = {};
            const newTree = React.cloneElement(elementsTree, elementsTree.props, elementsTree.props.children.filter((ele) => {
                return ele && ele.type !== "ul";
            }));
            return newTree;
        }
    };
class MyComponent extends Component {
    render() {
        return (
            <div>
                <h5>OperateElementTree：</h5>
                <ul>
                    <li>1-1</li>
                    <li>1-2</li>
                </ul>
                <div>
                    <ul>
                        <li>2-1</li>
                        <li>2-2</li>
                    </ul>
                </div>
            </div>
        )
    }
}
export default OperateElementTree(MyComponent);
```
使用：
```
class App extends Component {
    render() {
       <OperateElementTree />
    }
}
export default App;
```

#### 3.2.1.3 有条件地渲染元素树；
```
import React, { Component } from 'react';
const RenderHajack = (WrappedComponent) =>
    class extends WrappedComponent {
        render() {
            if (this.props.condition) {
                return super.render();
            } else {
                return null;
            }
        }
    };
class MyComponent extends Component {
    render() {
        return <h3>React</h3>;
    }
}
export default RenderHajack(MyComponent);
```
使用：
```
class App extends Component {
    render() {
        <ConditionalRender condition={false} />
    }
}
export default App;
```

#### 3.2.1.4 用样式控制包裹元素树（同属性代理）。
不能编辑或添加WrappedComponent实例的props，因为React组件不能编辑它接收到的props，但可以修改由render方法返回的组件的props。
反向继承不能保证完整的子组件树解析，这意味着将限制渲染劫持功能。但如果元素树中包括了函数类型的React组件，就不能操作组件的子组件。使用渲染劫持可以完全操作WrappedComponent的render方法返回的元素树。但如果元素树包括一个函数类型的React组件，就不能操作它的子组件了。

#### 3.2.2 控制state
高阶组件可以读取、修改或删除WrappedComponent实例中的state，如果需要，也可以增加state，但这样做可能会让WrappedComponent组建内部状态变得一团糟。大部分的高阶组件都应该限制读取或增加state， 其是后者，可以通过重新命名state，以防止混淆。

### 4 组件命名和组件参数
#### 4.1 组件命名
当包裹一个高阶组件时，失去了原始WrappedComponent的displayName，而组件名字是方便开发与调试的重要属性。
```
HOC.displayName = `HOC(${getDisplayName(WrappedComponent)})`;
```
或
```
class HOC extends ... {
    static displayName = `HOC(${getDisplayName(WrappedComponent)})`;
    …
}
```
getDisplayName()方法：
```
function getDisplayName(WrappedComponent) {
    return WrappedComponent.displayName ||
        WrappedComponent.name ||'Component';
}
```

#### 4.2 组件参数
有时，调用高阶组件时需要传入一些参数。
实现：
```
function HOCFactoryFactory(...params) {
    // 可以做一些改变params的事
    return function HOCFactory(WrappedComponent) {
        return class HOC extends Component {
            render() {
                return <WrappedComponent {...this.props} />;
            }
        }
    }
}
```
使用：
```
HOCFactoryFactory(params)(WrappedComponent)
// 或
@HOCFatoryFactory(params)
class WrappedComponent extends React.Component{}
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
