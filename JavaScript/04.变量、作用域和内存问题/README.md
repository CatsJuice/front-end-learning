# 4. 变量、作用域和内存问题



- [4.1 基本类型和引用类型的值](#41-基本类型和引用类型的值)
    - [4.1.1 动态的属性](#411-动态的属性)
    - [4.1.2 复制变量值](#412-复制变量值)
    - [4.1.3 传递参数](#413-传递参数)
    - [4.1.4 检测类型](#414-检测类型)
- [4.2 执行环境及作用域](#42-执行环境及作用域)
    - [4.2.1 延长作用域链](#421-延长作用域链)
    - [4.2.2 没有块级作用域](#422-没有块级作用域)
- [4.3 垃圾收集](#43-垃圾收集)
    - [4.3.1 标记清除](#431-标记清除)
    - [4.3.2 引用计数](#432-引用计数)
    - [4.3.3 性能问题](#433-性能问题)
    - [4.3.4 管理内存](#434-管理内存)
- [4.4 小结](#44-小结)




## 4.1 基本类型和引用类型的值

`ECMAScript`变量可能包含两种不同数据类型的值： **基本类型值** 和 **引用类型值**

在第三章讨论了5种基本数据类型， 这五种基本数据类型是 **按值访问** 的

**引用类型的值** 是保存在内存中的对象， 与其他语言不同， JavaScript 不允许直接访问内存中的位置， 也就是说不能直接操作对象的内存空间。 在操作对象时， 实际上是在操作 **对象的引用** 而不是实际的对象（这种说法不严密， 当复制保存着对象的某个变量时， 操作的是对象的引用， 但在为对象添加属性时， 操作的是实际的对象）

### 4.1.1 动态的属性

- 对于引用类型， 可以为其添加属性和方法；
- 但是， 不能给基本类型的值添加属性， 尽管这样做不会导致任何错误
```
var name = "Mike";
name.age = 27;
alert(name.age);        // undefined
```

### 4.1.2 复制变量值
- 如果从一个一个变量向另一个变量复制**基本类型**的值， 会在变量对象上创建一个新值， 然后把该值复制到为新变量分配的位置上
- 当从一个变量向另一个变量复制**引用型**的值时， 同样也会将储存在变量对象中的值复制一份放到为新变量分配的空间中， 不同的是， 这个值的副本实际上是一个指针， 指向存储在堆中的一个对象
    - 复制后， 两个变量实际上引用同一个对象， 改变其中一个变量会影响另一个变量

### 4.1.3 传递参数

`ECMAScript` 中所有函数的参数都是 **按值传递** 的。 也就是说， 把函数外部的值复制给函数内部的参数， 就和把值从一个变量复制到另一个变量一样。

!! 访问变量有按值和按引用， 而参数只能按值传递；
- 在向参数传递 **基本类型** 的值时， 被传递的值会被复制给一个局部变量（即命名参数， 或者用 `ECMAScript` 的概念来说， 就是 `arguments` 对象中的一个元素）
- 在向参数传递 **引用类型** 的值时， 会把这个值 **在内存中的地址** 复制给一个局部变量， 因此这个局部变量的变化会反映在函数的外部

```
function setName(obj) {
    obj.name = "张三";
}

var person = new Object();
setName(person);
alert(person.name);     // "张三"
```
在上述代码段的函数内部， `obj` 和 `person` 引用的是同一个对象， 在函数内为 `obj` 添加了属性， 在函数外部的 `person` 也将有所反应; 这里， 很多开发者错误地认为： 在局部作用域中修改的全局对象会在全局作用域中反映出来， 就说明参数是按引用传递的。以下代码段可证明对象是按值传递的：
```
function setName(obj) {
    obj.name = "张三";
    obj = new Object();
    obj.name = "李四";
}
var person = new Object();
setName(person);
alert(person.name);     // "张三"
```
与前一个代码段不同， 函数内部给 `obj` 设置 `name` 属性后， 又将一个新对象赋给变量 `obj` ， 同时将其 `name` 属性设置成其他值， 如果是 **按引用** 传递的， 那么 `person` 变量的引用会自动被修改为指向函数内部 `new Object()` 的对象， 访问 `person.name` 应该得到 '李四'， 而这里依旧是 '张三' ；

实际上， 当在函数背部重写 `obj` 时， 这个变量引用的就是一个局部对象了。 而这个局部对象会在函数执行完毕后立即被销毁.

### 4.1.4 检测类型
- `typeof` 操作符是检测一个变量是 **字符串** 、 **数值** 、 **布尔型** 还是 **undefined** 的最佳工具， 如果变量值是一个 对象 或者 `null` ， 则会返回 `object`
- 在检测引用类型的值时， `typeof` 操作符的用处不大
- 通常， 我们并不是想知道某个值是不是对象， 而是想知道它是什么类型的对象， 为此， `ECMAScript` 提供了 `instanceof` 操作符， 语法如下：
    - ```result = variable instanceof constructor```
    - 如果变量是给定引用类型（根据它的原型链来识别， 第六章将介绍原型链）的实例， 那么 `instanceof` 就会返回 `true`

## 4.2 执行环境及作用域

执行环境定义了变量或函数有权访问其他数据， 决定了他们各自的行为。
- 每个执行环境都有一个与之关联的 `变量对象` ， 环境中定义的所有 **变量** 和 **函数** 都保存在这个对象中。
- 全局执行环境是最外围的一个执行环境， 根据 `ECMAScript` 实现所在的宿主环境不同， 表示执行环境的对象也不一样。 在 `web` 浏览器中， 全局环境被认为是 `windows` 对象
- 每个函数都有自己的 **执行环境** ，当执行流进入一个函数时， 函数的环境就会被推入一个 **环境栈** 中。而在函数执行之后， 栈将其环境弹出， 把控制权返还给之前的执行环境。
- 当代码在一个环境中执行时， 会创建变量对象的一个 **作用域链** 。 作用域链的用途是保证对执行环境有权访问的所有变量和函数的有序访问。
    - 作用域的 `前端` ， 始终是当前执行的代码所在环境的变量对象。
    - 如果这个环境是函数， 则将其 **活动对象** 作为变量对象
    - 作用域链中的下一个变量对象来自包含（外部）环境，而再下一个来自下一个包含环境， 一直延续到全局执行环境
    - 内部环境可以通过作用域链访问所有的外部环境， 但外部环境不能访问内部环境中的任何变量和函数。

### 4.2.1 延长作用域链

有些语句可以在作用域链的前端临时增加一个变量对象， 该变量对象在代码执行后被移除
- `try-catch` 语句的 `catch` 块;
- `with` 语句;

对 `with` 来说， 会将指定的对象添加到作用域链中；

对 `catch` 语句来说， 会创建一个新的变量对象， 其中包含的是被抛出的错误对象的声明

### 4.2.2 没有块级作用域

在其他类 C 的语言中， 由花括号封闭的代码块都有自己的作用域（用 `ECMAScript` 的话来讲， 就是他们自己的执行环境）
```
if (true) {
    var color = "blue";
}
alert(color);   // "blue"
```
如果是在C、 C++ 或者 Java 中， color 会在 if 被执行完后被销毁。 但在 JavaScript 中， if 语句中的变量声明会将变量添加到当前的执行环境（上面的代码是全局环境）

**查询标识符**

搜索过程从作用域链的前端开始， 向上逐级查询与给定名字匹配的标识符。

## 4.3 垃圾收集
- 执行环境会负责管理代码执行过程中使用的内存

### 4.3.1 标记清除
**略**
### 4.3.2 引用计数
**略**
### 4.3.3 性能问题
**略**
### 4.3.4 管理内存
**略**

## 4.4 小结
`JavaScript` 变量可以用来保存两种类型的值：基本类型值和引用类型值。基本类型的值源自以下 5 种基本数据类型： `Undefined` 、`Null`、`Boolean`、`Number` 和 `String`。基本类型值和引用类型值具 有以下特点： 
- 基本类型值在内存中占据固定大小的空间，因此被保存在栈内存中； 
- 从一个变量向另一个变量复制基本类型的值，会创建这个值的一个副本； 
- 引用类型的值是对象，保存在堆内存中； 
- 包含引用类型值的变量实际上包含的并不是对象本身，而是一个指向该对象的指针； 
- 从一个变量向另一个变量复制引用类型的值，复制的其实是指针，因此两个变量最终都指向同 一个对象； 
- 确定一个值是哪种基本类型可以使用 typeof 操作符，而确定一个值是哪种引用类型可以使用 instanceof 操作符。 所有变量（包括基本类型和引用类型）都存在于一个执行环境（也称为作用域）当中，这个执 行环境决定了变量的生命周期，以及哪一部分代码可以访问其中的变量。以下是关于执行环境的几 点总结： 
- 执行环境有全局执行环境（也称为全局环境）和函数执行环境之分； 
- 每次进入一个新执行环境，都会创建一个用于搜索变量和函数的作用域链； 
- 函数的局部环境不仅有权访问函数作用域中的变量，而且有权访问其包含（父）环境，乃至全 局环境； 
- 全局环境只能访问在全局环境中定义的变量和函数，而不能直接访问局部环境中的任何数据； 
- 变量的执行环境有助于确定应该何时释放内存。 JavaScript 是一门具有自动垃圾收集机制的编程语言，开发人员不必关心内存分配和回收问题。可 以对 JavaScript的垃圾收集例程作如下总结。 
- 离开作用域的值将被自动标记为可以回收，因此将在垃圾收集期间被删除。 
- “标记清除”是目前主流的垃圾收集算法，这种算法的思想是给当前不使用的值加上标记，然 后再回收其内存。 
- 另一种垃圾收集算法是“引用计数”，这种算法的思想是跟踪记录所有值被引用的次数。JavaScript 引擎目前都不再使用这种算法；但在 IE中访问非原生 JavaScript对象（如 DOM元素）时，这种 算法仍然可能会导致问题。 
- 当代码中存在循环引用现象时，“引用计数”算法就会导致问题。 
- 解除变量的引用不仅有助于消除循环引用现象，而且对垃圾收集也有好处。为了确保有效地回 收内存，应该及时解除不再使用的全局对象、全局对象属性以及循环引用变量的引用。 