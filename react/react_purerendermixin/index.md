[<< 回到主页](http://suzy1993.github.io/misszy/)

## React pureRenderMixin

### 1 原理
PureRenderMixin的原理是重写了shouldComponentUpdate，在shouldComponentUpdate内比较this.props、this.state和nexProps、nextState，当两者相等时返回false，这样组件就不会重新渲染生成，再进行虚拟DOM的diff比较。

### 2 用法
#### 2.1 ES5的用法
```
var PureRenderMixin = require('react-addons-pure-render-mixin');
React.createClass({
    mixins: [PureRenderMixin],
    render: function() {
        return <div className={this.props.className}>foo</div>;
    }
});
```

#### 2.2 ES6的用法
ES6不再支持mixin。
```
import PureRenderMixin from 'react-addons-pure-render-mixin';
class MyComponent extends React.Component {
    constructor(props) {
        super(props);
        this.shouldComponentUpdate = PureRenderMixin.shouldComponentUpdate.bind(this);
    }
    render() {
        return <div className={this.props.className}>foo</div>;
    }
}
```

#### 3 问题
PureRenderMixin内进行的仅仅是浅比较对象，即只通过===比较props的第一层属性，若修改了第一层属性的内容而没有修改其引用，则重写的shouldComponentUpdate的判断会得到相等的结果，导致组件不会重新渲染；而若修改了第一层属性的引用而没有修改其内容，则重写的shouldComponentUpdate的判断会得到不相等的结果，导致组件进行不必要的重新渲染。
PureRenderMixin仅用于拥有简单props和state的组件，若对象包含了复杂的数据结构，层次较深，对比的过程不仅会有较大性能消耗，还可能会产生误判。这个时候Immutable.js就要派上用场了。

[<< 回到主页](http://suzy1993.github.io/misszy/)
