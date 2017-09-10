[<< 回到主页](http://suzy1993.github.io/misszy/)

## ES6 Module

ES6 Module模块功能主要由两个命令构成：export和import。

### 1 export命令
export命令用于规定模块的对外接口，import命令用于输入其他模块提供的功能。
一个模块就是一个独立的文件。该文件内部的所有变量，外部无法获取。如果你希望外部能够读取模块内部的某个变量，就必须使用export关键字输出该变量。下面是一个 JS 文件，里面使用export命令输出变量。
#### 1.1 输出变量
```
export let name = 'Alice';
export let age = 24;
```
等同于：
```
let name = 'Alice';
let age = 24;
export { name, age };
```
特别注意，export命令规定的是对外的接口，必须与模块内部的变量建立一一对应关系：
```
export 25; // 报错
var age = 25;
export age; // 报错
export var age = 25; // 正确
var age = 25;
export { age };// 正确
var age = 25;
export { age as age1 }; // 报错
```

#### 1.2 输出函数
```
export function func(x, y) {
    return x + y;
};
```
特别注意，export命令规定的是对外的接口，必须与模块内部的函数建立一一对应关系：
```
function func() { … }
export func; // 报错
export function func() { … } // 正确
function func() { … }
export {func};// 正确
```

#### 1.3 as重命名
重命名后，同一个接口可以用不同的名字输出多次。
```
function func1() { ... }
function func2() { ... }
export {
    func1 as f1,
    func2 as f2,
    func2 ad f3
};
```
注意：export命令可以出现在模块的任何位置，但必须处于模块顶层，如果处于块级作用域内，就会报错。
```
function func() {
    export age = 1;
}
func();
```

### 2 import命令
import命令接受一对大括号，里面指定要从其他模块导入的变量名。大括号里面的变量名，必须与被导入模块export时的名称相同，module-name的.js后缀可以省略。
```
import { name } from "module-name";
```
如果想为输入的变量重新取一个名字，import命令要使用as关键字，将输入的变量重命名。
```
import { name as name1 } from "module-name";
```
注意：
import命令具有提升效果，会提升到整个模块的头部，首先执行。
如果多次重复执行同一句import语句，那么只会执行一次，而不会执行多次。

### 3 模块的整体加载
除了指定加载某个输出值，还可以使用整体加载，即用星号（*）指定一个对象，所有输出值都加载在这个对象上面。
```
import * as obj from 'module-name';
console.log(obj.func1());
console.log(obj.func2());
```

### 4 export default命令
使用import命令的时候，需要知道所要加载的变量名或函数名，否则无法加载。
使用export default命令，可以为模块指定默认输出。
其他模块加载该模块时，import命令可以为被导入模块指定任意名字。需要注意的是，这时import命令后面，不使用大括号；其实是因为，一个模块只能有一个默认输出，因此export default命令只能使用一次，所以，import命令后面才不使用大括号。
本质上，export default就是输出一个叫做default的变量或方法。
正是因为export default命令其实只是输出一个叫做default的变量，所以它后面不能跟变量声明语句：
```
export var age = 25; // 正确
var age = 25;
export default age; // 正确
export default var age = 25; // 错误
```
同样地，因为export default本质是将该命令后面的值，赋给default变量以后再默认，所以直接将一个值写在export default之后：
```
export default 25; // 正确
export 25; // 报错
```

### 5 export 与 import 的复合写法
如果在一个模块之中，先输入后输出同一个模块，import语句可以与export语句写在一起：
```
export { fun1, fun2 } from 'module-name';
```
等同于：
```
import { fun1, fun2 } from 'module-name';
export { fun1, fun2 };
```
模块的接口改名和整体输出，也可以采用这种写法：
```
export { func1 as func } from ' module-name'; // 接口改名
export * from 'module-name'; // 整体输出
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
