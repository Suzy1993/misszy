[<< 回到主页](http://suzy1993.github.io/misszy/)

## CSS visibility: hidden VS display: none

### 1 visibility属性
对于CSS里的visibility属性，通常其值被设置成visible或hidden。  
当设置元素 visibility: collapse后，一般的元素的表现与visibility: hidden一样，也即其会占用空间。但如果该元素是与table相关的元素，例如table row、table column、table column group、table column group等，其表现却跟display: none一样，也即其占用的空间会释放。  
在不同浏览器下，对 visibility: collapse的处理方式不同：  
1）visibility: collapse的上述特性仅在Firefox下起作用。  
2）在IE即使设置了visibility: collapse，还是会显示元素。  
3）在Chrome下，即使会将元素隐藏，但无论是否是与table相关的元素，visibility: collapse都与visibility: hidden没有什么区别，即仍会占用空间。

### 2 visibility: hidden VS display: none
visibility: hidden相当于display:none，能把元素隐藏起来，但两者的区别在于：  
1）display:none 元素不再占用空间。  
2）visibility: hidden使元素在网页上不可见，但仍占用空间。  

[<< 回到主页](http://suzy1993.github.io/misszy/)
