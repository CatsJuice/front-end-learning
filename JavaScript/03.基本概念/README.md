# 3. 基本概念

## 3.1 语法

### 3.1.1 区分大小写

### 3.1.2 标识符

所谓**标识符**，就是指变量、函数、属性的名字，或者函数的参数

标识符的组合规则如下：

- 第一个字符必须是一个**字母**、**下划线（_）**或一个**美元符号（$）**；
- 其他字符可以是**字母**、**下划线**、**美元符号**或**数字**。 

### 3.1.3 注释

```
// 单行注释

/*
 *
 * 多行注释
 *
 */
```
### 3.1.4 严格模式
`ECMAScript 5` 引入了严格模式（`strict mode`）的概念, 是为 `JavaScript` 定义了一种不同的 解析与执行模型。在严格模式下，`ECMAScript 3` 中的一些不确定的行为将得到处理，而且对某些不安全 的操作也会抛出错误

要在整个脚本中启用严格模式，可以在顶部添加如下代码:
```
"use strict"; 
```
### 3.1.5 语句
- `ECMAScript` 中的语句以一个分号结尾；如果省略分号，则由解析器确定语句的结尾
- 虽然条件控制语句（如 `if` 语句）只在执行多条语句的情况下才要求使用代码块，但最佳实践是始终在控制语句中使用代码块——即使代码块中只有一条语句


## 3.2 关键字和保留字

`ECMA-262`描述了一组具有特定用途的关键字，这些关键字可用于表示控制语句的开始或结束，或 者用于执行特定操作等。按照规则，关键字也是语言保留的，不能用作标识符。以下就是 `ECMAScript` 的全部关键字（带`*`号上标的是第 5版新增的关键字）： 

  -| -| -| -
 :---|:---|:---|:---
break |do |instanceof  |typeof
case | else | new |var
catch| finally| return|void 
continue|for| switch|while 
debugger* | function  | this |with
default |  if  |  throw| delete 
in  | try       

 `ECMA-262`还描述了另外一组不能用作标识符的保留字。尽管保留字在这门语言中还没有任何特定 的用途，但它们有可能在将来被用作关键字。以下是 `ECMA-262`第3版定义的全部保留字
-| -| -| -
:---|:---|:---|:---
 abstract | enum |int | short 
 boolean |export |interface |static 
 byte |extends |long |super
 char |final  |native  |ynchronized 
 class  | floa |  package  |throws 
 const  | goto |  private  |  transient 
 debugger  |implements  |protected |volatile 
 double  |import  |public    

 在实现 `ECMAScript 3`的 `JavaScript`引擎中使用关键字作标识符，会导致“`Identifier Expected`”错误。 

 ## 3.3 变量
- ` ECMAScript` 的变量是**松散型**的， 即可以用来**保存任何类型的数据**， 换句话说， 每个变量仅仅是一个**用于保存值的占位符**而已
- 可以在修改变量值的同时修改变量的类型
- 使用`var`操作符定义的变量将成为定义该变量的**作用域**中的局部变量， 如果在函数中使用 `var` 定义了一个变量， 这个变量将在函数退出时销毁
- 省略 `var` 操作符定义变量， 将创建全局变量

## 3.4 数据类型
`ECMAScript` 中有 5 种**简单数据类型**(也称**基本数据类型**)
- Undefined
- Null
- String
- Number
- Boolean
还有 1 种复杂数据类型：
- Object
`Object` 本质上是由一组**无序名值对**组成的

所有值最终都将是上述6种数据类型之一

### 3.4.1 typeof 操作符

检测给定变量的数据类型, 可能返回下列某个字符串
- "`undefined`"——如果这个值未定义
- "`boolean`"——如果这个值是布尔值
- "`string`"——如果这个值是字符串； 
- "`number`"——如果这个值是数值
- "`object`"——如果这个值是对象或 `null`
- "`function`"——如果这个值是函数。 

`typeof` 是一个 **操作符** 而不是函数, 因此 圆括号 不是必须的

### 3.4.2 Undefined类型
- `Undefined` 类型只有一个值，即特殊的 `undefined` 。在使用 `var` 声明变量但未对其加以初始化时， 这个变量的值就是 `undefined`
- 第 3 版引入这个值是为了正式区分**空对象指针**与**未经初始化的变量**


### 3.4.3 Null类型
`Null`类型只有一个值：`null`; 从逻辑角度来看， `null`值 表示一个**空对象指针**， 而这也正是使用 `typeof` 操作符检测 `null` 值时会返回"`object`"的原因

- 如果定义的变量准备在将来用来保存对象， 那么最好将该变量初始化为 null 而不是其他值
- 