[<< 回到主页](http://suzy1993.github.io/misszy/)

## JavaScript 原型及原型链

### 1 原型
每个函数都有一个prototype（原型）属性，这个属性是一个指针，指向一个对象，这个对象的用途是包含所有实例共享的属性和方法。Prototype就是通过调用构造函数而创建的那个对象实例的原型对象。使用原型对象的好处是可以让所有实例共享它所包含的属性和方法。

### 2 原型链
原型链是实现继承的主要方法。其基本思想是利用原型让一个引用类型继承另一个引用类型的属性和方法。
#### 2.1 构造函数、原型和实例的关系
* 构造函数有一个原型属性prototype指向一个原型对象。
* 原型对象包含一个指向构造函数的指针constructor。
* 实例包含一个指向原型对象的内部指针__propto__。
假如让原型对象等于另一个类型的实例，则此时的原型对象包含一个指向另一个原型对象的指针，相应地，另一个原型对象中也包含着一个指向另一个构造函数的指针。假如另一个原型又是另一个类型的实例，那么上述关系依然成立，如此层层递进，就构成了实例与原型的链条，这就是所谓的原型链的基本概念。

#### 2.2 简单理解
JavaScript在创建对象时，不论是普通对象还是函数对象，都有一个叫做__proto__的内置属性，用于指向创建它的函数对象的原型对象prototype。  
以Student类继承Person类为例（student为Student类的实例，person为Person类的实例）：  
1）student对象有__proto__属性，它指向创建它的函数对象Person的prototype。
```
student.__proto__ === Person.prototype
```
2）person.prototype对象也有__proto__属性，它指向创建它的函数对象Object的prototype。
```
person.prototype.__proto__ === Object.prototype
```
3）Object.prototype对象也有__proto__属性，但它较特殊，为null。
```
Object.prototype.__proto__ === null
```

由__proto__串起来的直到Object.prototype.__proto__为null的链，叫原型链。

[<< 回到主页](http://suzy1993.github.io/misszy/)
