## 1.1 JavaScript简史

- `NetScape` 公司的`Brendan Eich`着手计划于1995年2月发布的 `NetScape Navigator 2` 开发一种名为 `LiveScript` 的脚本语言（在服务器上的名字叫 `LiveWire` ）, 在 `NetScape Navigator 2` 发布前夕， `NetScape` 为了搭上媒体热炒 `Java` 的顺风车， 临时改名为 `JavaScript`
- 因 `JavaScript` 的大获成功， `NetScape` 随即在 `NetScape Navigator 3` 中又发布了 `JavaScript 1.1` , 随即， 微软在自家的 `Internet Explorer 3` 中加入了名为 `JScript` 的 `JavaScript` 实现
- 由于没有标准规定 JavaScript 的语法和特性， 造成两个（多个）版本并存的局面，JavaScript 的标准化问题被提上了议事日程；
- 1997 年，以 `JavaScript 1.1` 为蓝本的建议被提交给了**欧洲计算机制造商协会**（ECMA，European Computer Manufacturers Association）。该协会指定 39 号技术委员会（TC39，Technical Committee #39） 负责“标准化一种通用、跨平台、供应商中立的脚本语言的语法和语义”（[http://www.ecmainternational.org/memento/TC39.htm](http://www.ecmainternational.org/memento/TC39.htm)）。TC39由来自 `Netscape`、`Sun`、微软、`Borland`及其他关注脚本语言 发展的公司的程序员组成，他们经过数月的努力完成了 ECMA-262——定义一种名为 `ECMAScript`（发 音为“ek-ma-script”）的新脚本语言的标准;

## 1.2 JavaScript实现

一个完整的 JavaScript 实现应该由下列三个不同部分组成：

- 核心 （ECMAScript）
- 文档对象模型 （DOM）
- 浏览器对象模型 （BOM）

### 1.2.1 ECMAScript

- 由 `ECMA-262` 定义的 `ECMAScript`与 Web 浏览器没有依赖关系,  Web 浏览器只是 `ECMAScript` 实现可能的宿主环境之一。

### 1.2.2 DOM

**文档对象类型**（DOM， Document Object Model）是针对 `XML` 但经过扩展用于 `HTML` 的应用程序编程接口（API） 

**DOM级别**

 - DOM 1级： 映射文档结构
 - DOM 2级： 
    - DOM 视图： 定义跟踪不同文档视图的接口
    - DOM 事件： 定义事件和事件处理的接口
    - DOM 样式： 定义基于CSS为元素应用样式的接口
    - DOM 遍历和范围： 定义遍历和操作文档的接口
- DOM 3级：
    - DOM 加载和保存： 以统一方式加载和保存文档
    - DOM 验证： 验证文档

### 1.2.3 BOM

由于没有 `BOM` 标准可以遵循，因此每个浏览器都有自己的实现。虽然也存在一些事实标准，例如 要有 `window` 对象和 `navigator` 对象等，但每个浏览器都会为这两个对象乃至其他对象定义自己的属 性和方法。现在有了 `HTML5` ，`BOM`实现的细节有望朝着兼容性越来越高的方向发展


## **小结**

JavaScript是一种专为与网页交互而设计的脚本语言，由下列三个不同的部分组成： 
- `ECMAScript` ，由 `ECMA-262` 定义，提供核心语言功能； 
- 文档对象模型（`DOM`），提供访问和操作网页内容的方法和接口； 
- 浏览器对象模型（`BOM`），提供与浏览器交互的方法和接口。 

`JavaScript`的这三个组成部分，在当前五个主要浏览器（IE、Firefox、Chrome、Safari和 Opera）中 都得到了不同程度的支持。其中，所有浏览器对 `ECMAScript` 第 3 版的支持大体上都还不错，而对 `ECMAScript 5`的支持程度越来越高，但对 `DOM`的支持则彼此相差比较多。对已经正式纳入 `HTML5`标 准的 `BOM`来说，尽管各浏览器都实现了某些众所周知的共同特性，但其他特性还是会因浏览器而异。 
