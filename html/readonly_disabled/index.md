## 表单元素的readonly和disabled属性

readonly和disabled是用在表单中的两个属性。

### 1 相同点
* 使用户不能够更改表单域中的内容。

### 2 不同点
* readonly只是使表单元素只读，即不能输入，外观不会变化；而disabled会使表单元素外观变化，如变灰。
* readonly只针对input(text / password)和textarea有效；而disabled对于所有的表单元素都有效，包括select、radio、checkbox、button等。
* 表单元素在使用了disabled后，当表单以POST或GET的方式提交的话，该元素的值不会随着表单提交，即不会被传递出去，也就是说，通过request.getParameter("...")得不到该元素的值；而readonly会将该值传递出去，也就是说，通过request.getParameter("...")能得到该元素的值。
