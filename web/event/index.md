[<< 回到主页](http://suzy1993.github.io/misszy/)

## IE事件处理与其他浏览器的差异

### 1 表示发生事件
1）非IE浏览器下，事件对应的函数有一个隐藏的变量e，表示发生事件。  
2）IE下，不需要e变量，window.event表示发生事件。
解决方案：用e||window.event来兼容。

### 2 触发事件对象（触发事件的元素被认为是目标target）
1）IE下，window.event对象有srcElement属性，但没有target属性。  
2）Firefox下，e对象有target属性，但没有srcElement属性。  
3）Chrome下，e对象同时具有target和srcElement属性。  
解决方案：event.srcElement ? event.srcElement : event.target来兼容。

### 3 按键码（字符代码）
1）IE下，window.event对象只有keyCode属性。  
2）FireFox下，e对象有which和charCode属性。  
3）Opera下，e对象有keyCode和which属性。  
4）Chrome下，e对象有keyCode、which和charCode属性。  
解决方案：用e.keyCode || e.which || e.charCode来兼容。

### 4 阻止事件的默认行为
1）IE 中阻止事件的默认行为需要将window.event.returnValue属性设置为false。  
2）非IE阻止事件的默认行为需要调用 e.preventDefault() 方法。  
解决方案：条件判断浏览器是否具有event.preventDefault再做相应处理。

### 5 阻止事件冒泡
1）IE阻止事件冒泡需要设置window.event.cancelBubble = true。  
2）非IE阻止事件冒泡需要调用e.stopPropagation()。  
解决方案：条件判断浏览器是否具有event.stopPropagation再做相应处理。

[<< 回到主页](http://suzy1993.github.io/misszy/)
