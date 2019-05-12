# 6. 面向对象的程序设计

ECMA-262 把对象定义为： “无序属性的集合， 其属性可以包含**基本值**， **对象**或者**函数**”

## 6.1 理解对象

### 6.1.1 属性类型

`ECMAScript` 中有 2 种属性： **数据属性** 和 **访问属性**

- 数据属性
  - 包含一个数据值的位置。在这个位置可以读取和写入值。
  - 数据属性有 4 个描述其行为的特性
    - `[[Configurale]]`: 表示能否通过 `delete` 删除属性从而重新定义属性， 能否修改属性的特性， 或者能否把属性修改为访问器属性。
    - `[[Enumerable]]`: 表示能否通过 `for-in` 循环返回属性。
    - `[[Writable]]`: 表示能否修改属性的值。
    - `[[Value]]`: 包含这个属性的数据值。
  - 要修改属性默认的特性， 必须使用 `ECMAScript 5` 的 `Object.defineProperty()` 方法。
    - 这个方法接收三个参数：
      - 属性所在的对象；
        - 属性的名字；
        - 描述符对象（ `configurale` 、 `enumerable`  、 `writable`  、 `value` 一个或者多个 ）
- 访问器属性
  - 访问器属性不包含数据值； 它们包含一对 `getter` 和 `setter` 函数;
  - 访问器属性有如下 4 个特性：
    - `[[Configurable]]`: 表示能否通过 delete 删除属性从而重新定义属性， 能否修改属性的特性， 或者能否把属性修改为数据属性。
    - `[[Enumerable]]`: 表示能否通过 `for-in` 循环返回属性;
    - `[[Get]]`: 在读取属性时调用的函数；
    - `[[Set]]`: 在写入属性时调用的函数;
  - 访问器属性不能直接定义， 必须使用 Object.defineProperty() 来定义。

### 6.1.2 定义多个属性

由于为对象定义多个属性的可能性很大， ECMAScript 5 又定义了一个 Object.defineProperties() 方法， 这个方法接收两个参数， 第一个是要添加和修改属性的对象， 第二个对象的属性与第一个对象中要添加或修改的属性一一对应

### 6.1.3 读取属性的特性

使用 `ECMAScript 5` 的 `Object.getOwnPropertyDescriptor()` 方法， 可以取得给定属性的描述符。

- 这个方法接收两个参数：
  - 属性所在的对象
  - 要读取其描述符的属性名称
- 返回值是一个对象
  - 如果是访问器属性
    - 对象属性有：  `configurable` 、 `enumerable` 、 `get` 、 `set`
  - 如果是数据属性
    - 对象属性有： `configurable` 、 `enumerable` 、 `writable` 、 `value`

## 6.2 创建对象

### 6.2.1 工厂模式

```js
function createPerson(name, age, job) {
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function() {
        alert(this.name);
    }
    return o;
}
```

在这个例子中， 每次调用 `createPerson` 都会返回一个包含三个属性一个方法的对象。 虽然解决了创建多个相似对象的问题， 但却没有解决对象识别的问题（即怎样知道一个对象的类型）

### 6.2.2 构造函数模式

可以创建自定义的构造函数， 从而定义自定义对象类型的属性和方法，例如：

```js
function Person(name, age, job) {
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName() = function() {
        alert(this.name);
    }
}

var person1 = new Person("Mike", 20, "Software Engineer");
var person1 = new Person("John", 22, "Software Engineer");
```

不同于工厂模式，构造函数模式

- 没有显式地创建对象
- 直接将属性和方法赋给了 `this` 对象
- 没有 `return` 语句
- 函数名首字母大写（按照惯例， 构造函数始终都应该以一个大写字母开头， 这个做法借鉴于其他 OO 语言）

以上方式调用构造函数实际上会经历以下 4 个步骤：

- 创建一个对象
- 将构造函数的作用域赋值给对象（因此 `this` 就指向了这个对象）
- 执行构造函数中的代码（为这个新对象添加属性）
- 返回新对象

在上述例子创建的 `person1`, `person2`分别保存着 `Person` 的一个不同实例。 这两个对象都有一个 `constructor` 属性， 该属性指向 `Person`:

```js
alert(person1.constructor == Person);       // true
alert(person2.constructor == Person);       // true
```

对象的 constructor 最初是用来标识对象类型的， 但是提到检测对象类型， 还是 instanceof 操作符更可靠一些：

```js
alert(person1 instanceof Object);       // true
alert(person1 instanceof Person);       // true
```

创建自定义的构造函数意味着将来可以将它的实例标识为一种特定的类型，这正是构造函数胜过工厂模式的地方

#### 1. 将构造函数当做函数

构造函数与其他函数的唯一区别， 就在于调用他们的方式不同。任何函数只要通过 new 操作符来调用， 那么它就可以作为构造函数；前面例子中的 Person 可以如下调用：

```js
Person("Greg", 27, "Doctor");       // 在全局作用域中调用， 添加到了window
window.sayName();                   // "Greg"

var o = new Object();
Person.call(o, "Jelly", 25, "Nurse");
o.sayName();        // "Jelly"
```

当在全局作用域中调用一个函数时， `this` 对象总是指向 `Global` 对象（在浏览器中始终是 `window` 对象）; 同理， 也可以使用 `call` （或者 `apply` ）在某个特殊对象的作用域中调用 `Person()` 函数。

#### 2. 构造函数的问题

构造函数的主要问题， 就是每个方法都要在每个对象实例上重新创建一遍。 在前面的例子中， person1 和 person2 都有一个 sayName() 的方法， 但这两个方法并不是同一个 Function 的实例。ECMAScript 中的函数是对象， 因此每定义一个函数， 就实例化了一个对象； 然而创建两个完成同样任务的 Function 实例的确没有必要了况且有 this 对象在， 根本不用在执行代码前就把函数绑定到特定对象上，因此可以像下面这样， 把函数定义转移到构造函数外部来解决这个问题：

```js
function Person(name, age, job) {
    this.name = name;
    this.job = job;
    this.age = age;
    this.sayName = sayName;
}

function sayName() {
    alert(this.name);
}

var person1 = new Person('Lily', 14, null);
var person2 = new Person('Tom', 8, 'mouse-catcher');
```

在构造函数内部， 我们将 sayName 属性设置成等于全局的 sayName 函数， 这样一来由于 sayName 包含的是一个指向函数的指针， 因此 person1 和 person2 就共享了全局作用域中的同一个 sayName 函数； 但这一做法也存在问题， 在全局作用域中定义的函数实际上只能被某个对象调用， 如果要定义很多方法， 就要定义很多个全局函数， 就没有封装性可言了。

### 6.2.3 原型模式

我们创建的每一个函数都有一个 `prototype` （原型）属性, 这个属性是一个指针， 指向一个对象， 这个对象的用途是 **包含可以由特定类型的所有实例共享的属性和方法**。 如果通过字面意思来理解， 那么 prototype 就是 **通过调用构造函数而创建的那个实例的原型对象** 。 使用原型对象的好处是 **可以让所有对象实例共享它所包含的属性和方法**， 换句话说， 不必在构造函数中定义对象实例的信息， 而是可以将这些信息直接添加到原型对象中， 如下：

```js
function Person() {}

Person.prototype.name = "Niicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function () {
    alert(this.name);
};

var person1 = new Person();
person1.sayName();      // "Niicholas"

var person2 = new Person();
person2.sayName();      // "Niicholas"

alert(person1.sayName == person2.sayName);      // true
```

与构造函数模式不同的是， 新对象的这些属性和方法是由所有实例共享的。换句话说， person1 和 person2 访问的是同一组属性和同一个 sayName() 函数。

#### 1. 理解原型对象

- 无论什么时候， 只要创建了一个新函数， 就会根据一组特定的规则为该函数创建一个 `prototype` 属性， 这个属性指向函数的原型对象
- 默认情况下， 所有原型对象都会自动获得一个 `constructor` （构造函数属性）， 这个属性包含一个指向 `prototype` 属性所在函数的指针。\
- 创建了自定义的构造函数之后， 其原型对象默认只会取得 `constructor` 属性， 其他方法则是从 `Object` 继承而来的；
- 当调用构造函数创建一个新实例后， 该实例的内部将包含一个指针（内部属性）， 指向构造函数的**原型对象**，虽然在脚本中没有标准的方式访问这个属性， 但在 `Firefox` 、 `Safari` 、`Chrome` 在每个对象上都支持一个属性`__proto__`; 而在其他实现中，这个属性对脚本是完全不可见的, 要明确的真正重要的一点就是: 这个连接存在于实例与构造函数的原型之间， 而不是实例与构造函数之间

以前面 Person 为例， 构造函数， 原型对象 及 实例间的关系如下图：

![6-1](https://catsjuice.cn/index/src/markdown/front-end/201905111145.png "图-1")

`ECMAScript 5` 增加了一个新的方法， 叫 `Object.getPrototyOf()`, 在所有支持的实现中， 这个方法返回`[[Prototype]]`的值； 使用 `Object.getPrototypeOf()` 可以方便地取得一个对象的原型， 而这在利用原型实现继承的情况下是非常重要的。
