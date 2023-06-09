# 揭秘 JavaScript 中的函数和变量提升

> 原文：<https://blog.logrocket.com/demystifying-function-and-variable-hoisting-in-javascript/>

在使用 JavaScript 时，有几个主题是很难理解的，因为它们并不像我们期望的那样直观。

来自 JavaScript 以外的语言背景的开发人员可能会对某些概念感到特别困难。

在这篇文章中，我们将着眼于函数和变量提升的复杂性。

在 JavaScript 中有几种定义函数的方法。我们将了解以下三种方法:

*   函数声明
*   函数表达式
*   箭头功能。

```
// function declaration 
function welcome () {
console.log('Welcome to learning JavaScript');
}

// function expression 
// involves the assignment of a named or an anonymous function to a variable.
var welcome = function () {
console.log('Welcome to learning JavaScript');
}

// arrow function
var welcome = () => console.log('Welcome to learning JavaScript');

//we can simple call it with
welcome(); // Welcome to learning JavaScript
```

乍一看，上面定义函数的方式看起来是一样的。

然而，还是有细微的差别。

让我们来看看它们——出于本文的目的，我们将更多地关注函数声明和函数表达式。

```
double(5) // 10
square(2) // Uncaught ReferenceError: Cannot access 'square' before initialization
   // at <anonymous>:3:1
const square = function (x) {
 return x * x;
}

function double (x) {
return 2 * x;
}
```

正如我们所看到的，程序并没有像预期的那样运行。

但是，如果我们在第 3 行注释掉对 square 函数的调用，或者将它移到它的定义下面，我们可以看到程序按预期工作。

这种异常的原因是，我们可以在实际定义函数声明之前调用它，但我们不能对函数表达式做同样的事情。这与解释给定脚本的 JavaScript 解释器有关。

函数声明被提升，而函数表达式没有。JavaScript 引擎通过在实际执行脚本之前提升当前范围来提升函数声明。

因此，上面的代码片段实际上解释如下:

```
function double (x) {
return 2 * x;
}
double(5) // 10
square(2) // Uncaught ReferenceError: Cannot access 'square' before initialization
   // at <anonymous>:3:1
const square = function (x) {
 return x * x;
}
```

但是 square 函数没有被提升，这就是为什么它只能从定义向下应用到程序的其余部分。这导致调用它时出错。

函数表达式就是这种情况。

JavaScript 中还有另一种形式的提升，这发生在使用关键字`var`声明变量时。

让我们看几个例子来说明这一点:

```
    var language = 'javascript';
    function whichLanguage() {
            if (!language) {
                    var language = 'java';
            }
            console.log(language);
    }
    whichLanguage();

```

当我们运行上面的代码时，我们可以看到我们的控制台注销了`java`。

如果这让你感到惊讶，那你来对地方了。我们将仔细看看到底发生了什么。

以同样的方式提升函数声明，用关键字`var`声明变量。

关于它们在吊装方式上的差异，有几点需要注意:

1.  当函数声明被提升时，整个函数体被移动到当前作用域的顶部。
2.  使用关键字`var`声明的变量在提升时只会将变量名移动到当前作用域的顶部——而不是赋值。

3.  使用关键字`var`声明的变量仅由函数限定范围，而不是由`if`块或`for`循环限定范围。

4.  功能提升取代可变提升。

记住这些规则，让我们看看 JavaScript 引擎将如何解释上面的代码:

```
var language = 'javascript';
function whichLanguage() {
var language;
        if (!language) {
                language = 'java';
        }
        console.log(language);
}
whichLanguage();
```

正如我们所看到的，`var language`被移动到了当前作用域的顶部，因此它的值为`undefined`。这使得它进入`if`模块，该模块将它重新赋值为值`java`。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

让我们看另一个例子来进一步说明这一点:

```
var name = 'gbolahan';
function myName() {
        name = 'dafe';
        return;
        function name() {}
}
myName();
alert(name);
```

通过遵循 JavaScript 引擎解释文件的规则，我们可以推断出上面的代码会产生什么。

让我们看看它是如何被解读的:

```
var name = 'gbolahan';
function myName() {
function name() {} // hoisted name function
        name = 'dafe';  // name reassigned to a new value. 
        return;    
}
myName(); 
console.log(name);
```

`gbolahan`将被注销，因为在`myName`函数中定义的名称被该函数限定了作用域，并在函数执行后被丢弃。

### 结论

这涵盖了在 JavaScript 中使用提升时需要考虑的大部分事情。这些规则有一些例外，但是随着 ES6 的引入，您现在可以通过在声明变量时使用`const`和`let`关键字来避免这些警告。

理解提升是如何工作的会有所帮助，特别是因为您可能会在 JavaScript 面试中遇到它。

## 通过理解上下文，更容易地调试 JavaScript 错误

调试代码总是一项单调乏味的任务。但是你越了解自己的错误，就越容易改正。

LogRocket 让你以新的独特的方式理解这些错误。我们的前端监控解决方案跟踪用户与您的 JavaScript 前端的互动，让您能够准确找出导致错误的用户行为。

[![LogRocket Dashboard Free Trial Banner](img/cbfed9be3defcb505e662574769a7636.png)](https://lp.logrocket.com/blg/javascript-signup)

LogRocket 记录控制台日志、页面加载时间、堆栈跟踪、慢速网络请求/响应(带有标题+正文)、浏览器元数据和自定义日志。理解您的 JavaScript 代码的影响从来没有这么简单过！

[Try it for free](https://lp.logrocket.com/blg/javascript-signup)

.