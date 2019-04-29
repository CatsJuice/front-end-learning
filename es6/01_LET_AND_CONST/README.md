# `let` and `const`

## `let`

**用于变量的声明， 声明的变量有 `块级作用域`**
```
 {
    var a = 1
    let b = 1
}
console.log(a)
console.log(b)
```

`a` 正常打印
打印 `b` 报错： `Uncaught ReferenceError: b is not defined`

```
var a = []
for (var i=0; i<10; i++) {
    a[i] = function() {
        console.log(i)
    }
}

a[6]();    // 10

```
`for`循环中的`i`通过`var`变量声明， `i`是全局（浏览器中是`window`）对象， 在`a[6]()`调用时， 打印`i`是`for`循环后 `i` 的值
```
var a = []
for (let i=0; i<10; i++) {
    a[i] = function() {
        console.log(i)
    }
}

a[i]();      // 6
```
以上代码唯一不同的是 `for` 循环中的变量 `i` 使用 `let` 声明的， 所以变量 `i` 具有块级作用域， 每次循环的 `i` 都是一个新的变量

**不存在变量提升**

`var`声明的变量存在变量提升， 如下：
```
console.log(a)      // undefined
var a = 'this is a'
```
而如果不声明 `var a` ， 则报错 `Uncaught ReferenceError: a is not defined`

以上代码等同于：
```
var a;          // var a = undefined
console.log(a)  // undefined
a = 'this is a'
```

而使用 `let` 声明的变量， 不存在**变量提升**, 所以上面的代码用 `let` 如下：
```
console.log(a)         // Uncaught ReferenceError: a is not defined
let a = 'this is a'
```

**暂时性死区**
> `ES6` 明确规定，如果区块中存在 `let` 和 `const` 命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错

**不允许重复声明**
`let`声明的变量不允许在相同的作用域内重复声明一个变量


## **块级作用域**

**块级作用域与函数声明**

```
// 情况一
if (true) {
  function f() {}
}

// 情况二
try {
  function f() {}
} catch(e) {
  // ...
}
```
> 上面两种函数声明，根据 ES5 的规定都是非法的。
> 
> 但是，浏览器没有遵守这个规定，为了兼容以前的旧代码，还是支持在块级作用域之中声明函数，因此上面两种情况实际都能运行，不会报错。

`ES6` 规定，块级作用域之中，函数声明语句的行为类似于let，在块级作用域之外不可引用


## **const**

声明一个只读常量， 一旦声明就不可改变；
如下：
```
const a = 100
a++     //   Uncaught TypeError: Assignment to constant variable.
```
但是对于对象却有例外
```
const person = {'name':'张三', 'age':20}
person.name = "new name"        
console.log(person.name)    // new name
```