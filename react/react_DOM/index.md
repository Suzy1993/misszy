[<< 回到主页](http://suzy1993.github.io/misszy/)

## React DOM操作

### 1 ref的字符串属性
* 给从render返回的元素（HTML元素或React元素）分配ref属性。
* 通过this.refs访问实例。

```
var MyComponent = React.createClass({
    handleClick: function() {
        this.refs.myInput.focus();
    },
    render: function() {
return (
    <div>
        <input type="text" ref="myInput" />
        <input type="button" value="焦点获取" onClick={this.handleClick} />
    </div>
        );
    }
});
ReactDOM.render(
    <MyComponent />,
    document.getElementById('example')
);
```

### 2 ref的回调属性
```
var MyComponent = React.createClass({
    handleClick: function() {
        input.focus();
    },
    render: function() {
        return (
            <div>
        <input ref={ node => { input = node; } } />
        <input type="button" value="焦点获取" onClick={this.handleClick} />
    </div>
        );
    }
});
ReactDOM.render(
    <MyComponent />,
    document.getElementById('example')
);
```
* 说明：<input ref={ node => { input = node; } } />可简写为<input ref={ node => input = node } />。
* 注意：新版本的React已经不推荐使用ref的字符串属性，而推荐使用ref回调属性。
* 问题：ref属性不能被用于一个函数式组件上。

```
function MyInput() {
    return <input />;
}
var MyComponent = React.createClass({
    handleClick: function() {
        input.focus(); // 报错：Cannot read property 'focus' of null
    },
    render: function() {
        return (
            <div>
        <MyInput ref={ node => { input = node; } } />
        <input type="button" value="焦点获取" onClick={this.handleClick} />
    </div>
        );
    }
});
ReactDOM.render(
    <MyComponent />,
    document.getElementById('example')
);
```
* 原因：函数式组件没有实例。
* 解决：将函数式组件改为类组件。
* 问题：将函数式组件改为类组件后，虽然能获取到实例，但获取不到DOM。

```
class MyInput extends React.Component {
    render() {
        return <input />;
    }
}
var MyComponent = React.createClass({
    handleClick: function() {
        input.focus(); // 报错：input.focus is not a function
    },
    render: function() {
        return (
            <div>
        <MyInput ref={ node => { input = node; } } />
        <input type="button" value="焦点获取" onClick={this.handleClick} />
    </div>
        );
    }
});
ReactDOM.render(
    <MyComponent />,
    document.getElementById('example')
);
```
* 原因：ref添加到Component上获取的是Component实例而不是DOM，添加到原生HTML上获取的才是DOM。
* 解决：为获得Component实例对应的DOM，需要使用 ReactDOM.findDOMNode()方法。

```
class MyInput extends React.Component {
    render() {
        return <input />;
    }
}
var MyComponent = React.createClass({
    handleClick: function() {
        ReactDOM.findDOMNode(input).focus();
    },
    render: function() {
        return (
            <div>
        <MyInput ref={ node => { input = node; } } />
        <input type="button" value="焦点获取" onClick={this.handleClick} />
    </div>
        );
    }
});
ReactDOM.render(
    <MyComponent />,
    document.getElementById('example')
);
```

### 3 ref用于原生HTML和Component
#### 3.1 ref用于原生HTML
```
<input type="text" ref="myInput”/>
// 以下四种方法都可以获取到真实的DOM
var inputDOM = this.refs.myInput.getDOMNode();
var inputDOM = React.findDOMNode(this.refs.myInput);
var inputDOM = this.refs['myInput'].getDOMNode();
var inputDOM = React.findDOMNode(this.refs['myInput']);
```
* 在调用ReactDOM.findDOMNode()时，当参数是DOM，返回值就是该DOM。
* ref添加到原生HTML上获取的本身就是DOM。
* 在原生HTML上使用ReactDOM.findDOMNode()没有意义，上述第2种方法和第4种方法是等效的。

#### 3.2 ref用于原生Component
* ref添加到原生Component时，获取的是Component实例而不是DOM，为获得Component实例对应的DOM，需要使用 ReactDOM.findDOMNode()方法。
* findDOMNode() VS getDOMNode()：
    * findDOMNode和getDOMNode方法都会返回一个DOM元素。
    * 调用方法：this.refs.myInput.getDOMNode() VS ReactDOM.findDOMNode(this.refs.myInput)。
    * findDOMNode方法找不到元素生成的DOM元素时，可能会返回NULL。
    * getDOMNode()方法在react@0.14中被弃用。

### 4 总结
* 有了ref与findDOMNode，父子组件之间不再是仅仅通过props来传递数据。
* 无状态组件不支持ref，原因是无状态组件没有实例化的过程。

[<< 回到主页](http://suzy1993.github.io/misszy/)
