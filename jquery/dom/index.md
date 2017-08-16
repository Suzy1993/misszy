[<< 回到主页](http://suzy1993.github.io/misszy/)

## jQuery DOM操作 VS 原生JS DOM操作

### 1 创建元素节点
1）原生js创建元素节点
```
document.createElement("div");
```
2）jQuery创建元素节点
```
$('<div></div>');
```

### 2 创建并添加文本节点
1）原生JS创建文本节点
```
var text = document.createTextNode("Hello World!");
var div = document.createElement("div");
div.appendChild(text);
```
2）jquery创建并添加文本节点
```
var $div = $('<div>Hello World!</div>');
```

### 3 复制节点
1）原生JS复制节点
```
var div = document.getElementById("div1");
var node = div.cloneNode(true);
```
注意：true表示克隆整个节点，包括所有子节点；false表示该节点，不克隆子节点。  
2）jQuery复制节点
```
var node = $('#div1).clone(true);
```
注意：复制节点要避免id重复。

### 4 插入节点
1）原生JS插入节点  
1-1）原生JS在末尾插入新的子节点
```
parentNode.appendChild(newNode);
```
1-2）原生JS在已有子节点之前插入新的子节点
```
parentNode.insertBefore(newNode, targetNode);
```
注意：原生JS没有insertAfter()方法。

2）jQuery插入节点  
2-1）在匹配元素子节点末尾插入节点
```
$('#div1).append("<div>Hello World!</div>");
```
2-2）把节点插入到匹配元素子节点末尾
```
$('<div>Hello World!</div>').appendTo('#div1);
```
2-3）在匹配元素子节点开头插入节点
```
$('#divl').prepend('<div>Hello World!</div>');
```
2-4）把节点插入到匹配元素子节点开头
```
$('<div>Hello World!</div>').prependTo('#divl');
```
2-5）在匹配元素之前插入节点
```
$('#divl').before('<div>Hello World!</div>');
```
2-6）把节点插入到匹配元素之前
```
$('<div>Hello World!</div>').insertBefore('#divl');
```
2-7）在匹配元素之后插入节点
```
$('#divl').after('<div>Hello World!</div>');
```
2-8）把节点插入到匹配元素之后
```
$('<div>Hello World!</div>').insertAfter('#divl');
```

### 5 删除节点
1）原生JS删除节点
```
node.parentNode.removeChild(node);
```
2）jQuery删除节点
```
$('#divl').remove();
```

### 6 替换节点
1）原生JS替换节点
```
parentNode.repalceChild(newNode, oldNode);
```
注意：oldNode必须是parentNode真实存在的一个子节点。  
2）jQuery替换节点
```
$('#div1').replaceWith('<div>Hello World!</div>');
```

### 7 设置/获取属性
1）原生JS设置/获取属性   
1-1）设置属性
```
node.setAttribute("background", "red");
checkboxEl.checked = true;
```
1-2）获取属性
```
imgEl.getAttribute("background");
checkboxEl.checked;
```

2）jQuery设置/获取属性  
2-1）设置属性
```
$("#div1").attr({"background": "red"});
$("#checkbox1").prop({"checked": true});
```
2-2）获取属性
```
$("#div1").attr("background");
$("#checkbox1").prop("checked");
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
