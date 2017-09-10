[<< 回到主页](http://suzy1993.github.io/misszy/)

## ES6 字符串扩展

### 1 includes()，startsWith()，endsWith()
传统上，JavaScript只有indexOf方法，可以用来确定一个字符串是否包含在另一个字符串中。ES6又提供了三种新方法。
1) includes()
返回布尔值，表示是否找到了参数字符串。
2）startsWith()
返回布尔值，表示参数字符串是否在原字符串的头部。
3）endsWith()
返回布尔值，表示参数字符串是否在原字符串的尾部。
```
var str = 'Hello world!';
str.startsWith('Hello') // true
str.endsWith('!') // true
str.includes('world') // true
```
这三个方法都支持第二个参数，表示开始搜索的位置：
```
var str = 'Hello world!';
str.startsWith('world', 6) // true
str.endsWith('Hello', 5) // true
str.includes('Hello', 6) // false
```

### 2 repeat()
返回一个新字符串，表示将参数字符串重复n次：
```
'x'.repeat(3) // "xxx"
```
如果参数是小数，会被取整：
```
'x'.repeat(2.9) // "xxx"
```
如果参数是负数或Infinity，会报错：
```
'x'.repeat(Infinity)// RangeError
'x'.repeat(-1)// RangeError
```
但是，如果参数是0到-1之间的小数，则等同于0，这是因为0到-1之间的小数，取整以后等于-0，repeat视同为0：
```
'x'.repeat(-0.9) // ""
```
如果参数是字符串，则会先转换成数字：
```
'x'.repeat('x') // ""
'x'.repeat('3') // "xxx"
```
参数NaN等同于0：
```
'x'.repeat(NaN) // ""
```

### 3 padStart()，padEnd()
ES6引入了字符串补全长度的功能。如果某个字符串不够指定长度，会在头部或尾部补全。padStart()用于头部补全，padEnd()用于尾部补全。
padStart和padEnd一共接受两个参数，第一个参数用来指定字符串的最小长度，第二个参数是用来补全的字符串。
```
'x'.padStart(5, 'ab') // 'ababx'
'x'.padStart(4, 'ab') // 'abax'
'x'.padEnd(5, 'ab') // 'xabab'
'x'.padEnd(4, 'ab') // 'xaba'
```
如果省略第二个参数，默认使用空格补全长度：
```
'x'.padStart(4) // '   x'
'x'.padEnd(4) // 'x   '
```
如果原字符串的长度，等于或大于指定的最小长度，则返回原字符串：
```
'xxx'.padStart(2, 'ab') // 'xxx'
'xxx'.padEnd(2, 'ab') // 'xxx'
```
如果用来补全的字符串与原字符串，两者的长度之和超过了指定的最小长度，则会截去超出位数的补全字符串：
```
'abc'.padStart(10, '0123456789') // '0123456abc'
```
padStart的常见用途是为数值补全指定位数：
```
'1'.padStart(10, '0') // "0000000001"
'123456'.padStart(10, '0') // "0000123456"
```
padStart的另一个用途是提示字符串格式。
```
'12'.padStart(10, 'YYYY-MM-DD') // "YYYY-MM-12"
'09-12'.padStart(10, 'YYYY-MM-DD') // "YYYY-09-12"
```

### 4 模板字符串
模板字符串（template string）是增强版的字符串，用反引号（`）标识。它可以当作普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量。
```
// 普通字符串
console.log(`Hello world`);
// 多行字符串
console.log(`Hello world
in 2017`);
```
如果模板字符串中的变量没有声明，将报错：
```
var msg = `Hello, ${name}`; // 报错
```
如果在模板字符串中需要使用反引号，则前面要用反斜杠转义。
```
var greeting = `\`Hello\` world!`;
```
如果使用模板字符串表示多行字符串，所有的空格和缩进都会被保留在输出之中：
```
$('#list').html(`
<ul>
    <li>first</li>
    <li>second</li>
</ul>
`);
```
模板字符串中嵌入变量，需要将变量名写在${}之中：
```
var name = "Alice";
console.log(`Hello ${name}!`);
```
大括号内部可以放入任意的JavaScript表达式，可以进行运算，以及引用对象属性。
```
var x = 1;
var y = 2;
console.log(`${x} + ${y} = ${x + y}`); // "1 + 2 = 3"
console.log(`${x} + ${y * 2} = ${x + y * 2}`); // "1 + 4 = 5"
var obj = {x: 1, y: 2};
console.log(``${obj.x + obj.y}`); // "3"
```
大括号内部还能调用函数：
```
function fn() {
    return "Hello ";
}
console.log(`${fn()} world!`); // Hello world!
```
如果大括号内部是一个字符串，将会原样输出：
```
console.log(`Hello ${'World'}`); // Hello world!
```
模板字符串甚至还能嵌套：
```
const generator = params => `
    <table>
    ${params.map(param => `
        <tr><td>${param.first}</td></tr>
        <tr><td>${param.last}</td></tr>
    `).join('')}
    </table>
`;
const data = [
    { first: 1, last: 2 },
    { first: 3, last: 4 },
];
console.log(generator(data));
/**
<table>

    <tr><td>1</td></tr>
    <tr><td>2</td></tr>

    <tr><td>3</td></tr>
    <tr><td>4</td></tr>

</table>
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
