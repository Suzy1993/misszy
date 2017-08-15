[<< 回到主页](http://suzy1993.github.io/misszy/)

## JavaScript DOM操作

### 1 获取所有子节点对象
* childeNodes

### 2 获取第一个子节点
* firstChild

### 3 获取最后一个子节点
* lastChild

### 4 获取父节点
* parentNode

### 5 获取下一个兄弟节点
* nextSibling

### 6 获取前一个兄弟节点
* previousSibling

### 7 获取节点的HTML标记名称（大写字母）
* nodeName

### 8获取DOM元素
* document.getElementById("***");
* document.getElementsByClassName("***")[0];
* document.getElementsByTagName("***")[0];
* document.getElementsByName("***")[0];

### 9 修改CSS样式
* document.getElementById("***").style.***= "***";

### 10 修改CLASS属性
* document.getElementById("***").className = "***";
* document.getElementById("***").className = "";

### 11 修改文本
* document.getElementById("***").innerHTML = "***";

### 12 创建、插入、删除、复制节点
* document.createElement('div');
* document.createTextNode('123');
* ***.appendChild(***);
* ***.parentNode.removeChild(***)
* ***.cloneNode(true)或***.cloneNode()
* ***父节点.insertBefore(***待插入节点, ***参考节点)
注意：原生JS没有insertAfter()方法。

[<< 回到主页](http://suzy1993.github.io/misszy/)
