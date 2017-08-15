[<< 回到主页](http://suzy1993.github.io/misszy/)

## JavaScript 数组方法

### 1 ECMAScript数组的特点
1）ECMAScript数组的每一项可以保存任何类型的数据。
2）ECMAScript数组的大小是可以动态调整的。

### 2 创建数组
#### 2.1 使用Array构造函数（new操作符可省略）
```
var arr1 = new Array();
var arr2 = new Array(3);
var arr3 = new Array("teacher", 3, true);
```

#### 2.2 使用数组字面量表示法
```
var arr1 = [];
var arr2 = ["teacher", 3, true];
```

### 3 length属性
ECMAScript数组的length属性不是只读的，通过设置这个属性可以从数组末尾移除项或向数组中添加新项。
eg1：从数组末尾移除项
```
var arr = ["teacher", 3, true];
arr.length = 1;
alert(arr[2]);//undefined
```
eg2：如果将length设置为大于当前数组长度的值，则新增的每一项都会取得undefined值
```
var arr = ["teacher", 3, true];
arr.length = 4;
alert(arr[3]);//undefined
```
eg3：向数组中添加新项
```
var arr = ["teacher", 3, true];
arr[arr.length] = “doctor”;
```
eg4：当一个值放在超出当前数组大小的位置上时，数组会重新计算其长度值，等于最后一项的索引加一。
```
var arr = ["teacher", 3, true];
arr[9] = “doctor”;
alert(arr.length);//10
```

### 4 检测数组
#### 4.1 instanceof操作符
```
if (value instanceof Array) {
    ...
}
```
适用范围：一个网页或一个全局作用域
问题：若网页中包含多个框架，则实际上存在两个以上不同的全局执行环境，从而存在两个以上不同版本的Array构造函数。若从一个框架向另一个框架传入一个数组，那么传入的数组在与第二个框架中原生创建的数组分别具有各自不同的构造函数。

#### 4.2 Array.isArray()方法
```
if (Array.isArray(value)) {
    ...
}
```
用途：确定给定值是否是数组，无论它是在哪个全局执行环境中创建的。

### 5 转换方法
#### 5.1 toString()
返回每一项的字符串形式拼接而成的一个以逗号分隔的字符串，为了取得每一项的值，调用的是每一项的toString()方法。

#### 5.2 valueOf()
返回的还是数组

#### 5.3 toLocaleString()
为了取得每一项的值，调用的是每一项的toLocaleString()方法，而不是toString()方法。

#### 5.4 join()
使用指定的分隔符来构建字符串
说明：alert()方法要接收字符串参数，所以它会在后台调用toString()方法。
eg1：
```
var friends = ["Alice","Bruce","Cindy"];
alert(friends.toString());//Alice,Bruce,Cindy
alert(friends.valueOf());//Alice,Bruce,Cindy
alert(friends.toLocaleString());//Alice,Bruce,Cindy
alert(friends);//Alice,Bruce,Cindy
alert(friends.join());//Alice,Bruce,Cindy
alert(friends.join(“|”));//Alice|Bruce|Cindy
```
eg2：
```
var person1 = {
    toLocaleString: function() {
        return "Alice";
    },
    toString: function() {
        return "Bruce";
    }
}
var person2 = {
    toLocaleString: function() {
        return "Cindy";
    },
    toString: function() {
        return "David";
    }
}
var person = [person1, person2];
alert(person);//Alice,Bruce
alert(person.toString());//Alice,Bruce
alert(person.toLocaleString());//Cindy,David
```

### 6 栈方法
#### 6.1 push()
接收任意数量的参数，逐个添加到末尾，返回修改后数组的长度。

#### 6.2 pop()
从数组末尾移除最后一项，数组的长度减一，返回移除的项。
```
var friends = new Array();
var len = friends.push("Alice","Bruce");
alert(len);//2
var friend = friends.pop();
alert(friend );//"Bruce"
alert(friends.length);//1
```

### 7 队列方法
#### 7.1 shift()
移除数组的第一项，数组的长度减一，返回移除的项。

#### 7.2 unshift()
在数组前端添加任意数量的项，返回修改后数组的长度。
```
var friends = new Array();
var len = friends.unshift("Alice","Bruce");
alert(len);//2
var friend = friends.shift();
alert(friend );//"Alice"
alert(friends.length);//1
```

### 8 重排序方法
#### 8.1 reverse()
翻转数组项的顺序

#### 8.2 sort()
按升序排列数组项
sort()方法会调用每项的toString()方法，然后比较得到的字符串。
```
var items=[0,1,3,15,18];
items.sort();
alert(items);//0,1,15,18,3
```
sort()方法可以接收一个比较函数作为参数：比较函数接收两个参数，若第一个参数应该位于第二个参数之前则返回一个负数；若两个参数相等则返回0；若第一个参数应该位于第二个参数之后则返回一个正数。
```
function compare(item1, item2) {
    if (item1 < item2)
        return -1;
    else if (item1 > item2)
        return 1;
    else
        return 0;
}
var items=[0,1,3,15,18];
items.sort(compare);
alert(items);//0,1,3,15,18
```
对于数值类型或其valueOf()方法会返回数值类型的对象类型，可以简写比较函数。
```
function compare(item1, item2) {
    return item1 - item2;
}
```

### 9 操作方法
#### 9.1 concat()
基于当前数组中的所有项创建一个新数组。先创建当前数组的一个副本，然后将接收到的参数添加到这个副本的末尾，最后返回新构建的数组。若没有给concat()传递参数，则只是复制当前数组并返回副本；若传递给concat()的是一或多个数组，则该方法会将这些数组中的每一项都添加到结果数组；若传递给concat()的不是数组，则这些值都简单地添加到结果数组的末尾。
```
var friends = ["Alice", "Bruce"];
var newFriends = friends.concat("Cindy", ["David", "Emy"]);
alert(newFriends);//Alice,Bruce,Cindy,David,Emy
```

#### 9.2 slice()
基于当前数组的一或多项创建一个新数组。接收一或两个参数，即要返回项的开始和结束位置（不包括结束位置）。slice()方法不会影响原始数组。若参数中有负数，则用数组长度加上该负数来确定相应的位置。若结束位置小于开始位置，则返回空数组。
```
var friends = ["Alice", "Bruce", "Cindy"];
var friends1 = friends.slice(1);
alert(friends1);//Bruce,Cindy
var friends2 = friends.slice(1, 2);
alert(friends2);//Bruce
```

#### 9.3 splice()
主要用途是向数组的中部插入项，返回一个包含从原始数组中删除的项的数组，若没有删除任何项，则返回空数组。使用方式有3种：
#### 9.3.1 删除
可以删除任意数量的项，只需指定2个参数：要删除的第一项的位置和要删除的项数。

#### 9.3.2 插入
可以向指定位置插入任意数量的项，只需指定3个参数：起始位置、要删除的项数和要插入任意数量的项。

#### 9.3.3 替换
可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需指定3个参数：起始位置、要删除的项数和要插入任意数量的项。插入的项数不必和删除的项数相等。
```
var friends = ["Alice", "Bruce", "Cindy"];
var friends1 = friends.splice(0, 1);
alert(friends1);//Bruce,Cindy
var friends2 = friends.slice(1, 0, "David", "Emy");
alert(friends2);//Bruce,David,Emy,Cindy
var friends3 = friends.slice(1, 1, "Fancy", "Gary");
alert(friends3);//Bruce,Fancy,Gary,Emy,Cindy
```

### 10 位置方法
#### 10.1 indexOf()
接收两个参数——要查找的项和可选的查找起点位置的索引，从开头开始查找，没找到则返回-1。

#### 10.2 lastIndexOf()
接收两个参数——要查找的项和可选的查找起点位置的索引，从末尾开始查找，没找到则返回-1。
在比较第一个参数与数组中的每一项时，会使用全等操作符，也就是要求查找的项必须严格相等。
```
var person = {name : "Alice"};
var people1 = [{name : "Alice"}, person];
alert(people1.indexOf(person));//1，不是0
```

### 11 迭代方法
ECMAScript数组有5个迭代方法。每个方法接收两个参数——要在每一项上运行的函数和可选的运行该函数的作用域对象（影响this的值）。传入的函数接收三个参数——数组项的值、该项在数组中的位置和数组对象本身。
#### 11.1 every()
对数组中的每一项运行给定函数，若该函数对每一项都返回true，则返回true。

#### 11.2 some()
对数组中的每一项运行给定函数，返回该函数会返回true的项组成的数组。

#### 11.3 filter()
对数组中的每一项运行给定函数，若该函数对每一项都返回true，则返回true。

#### 11.4 foreach()
对数组中的每一项运行给定函数，无返回值。

#### 11.5 map()
对数组中的每一项运行给定函数，若该函数对每一项都返回true，则返回true。
```
var nums = [1,2,3,4,1,2,3];
var every = nums.every(function(item, index, array) {
    return (item > 2);
});
alert(every);//false
var some = nums.some(function(item, index, array) {
    return (item > 2);
});
alert(some);//true
var filter = nums.filter(function(item, index, array) {
    return (item > 2);
});
alert(filter);//[3,4,3]
var map = nums.map(function(item, index, array) {
    return (item * 2);
});
alert(map);//[2,4,6,8,2,4,6]
nums.foreach(function(item, index, array) {
    ...
});
```

### 12 归并方法
迭代数组的所有项，然后构建一个最终返回的值。接收两个参数——一个在每一项上调用的函数和可选的作为归并基础的值。传入的函数接收四个参数——前一个值、当前值、项的索引和数组对象。函数返回的任何值都会作为第一个参数自动传给下一项。第一次迭代发生在数组的第二项上，因此第一个参数是数组的第一项，第二个参数是数组的第二项。
#### 12.1 reduce()
从数组的第一项开始，逐个遍历到最后。
```
var items = [1,2,3,4];
var sum = items.reduce(function(prev, cur, index, array) {
    return prev + cur;
});
alert(sum);//10
```

#### 12.2 reduceRight()
接收两个参数——一个在每一项上调用的函数和可选的作为归并基础的值。从数组的最后一项开始，向前遍历到第一项。
```
var items = [1,2,3,4];
var sum = items.reduceRight(function(prev, cur, index, array) {
    return prev + cur;
});
alert(sum);//10
```

[<< 回到主页](http://suzy1993.github.io/misszy/)
