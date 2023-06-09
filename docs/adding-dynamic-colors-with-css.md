# 如何用 CSS 添加动态颜色

> 原文：<https://blog.logrocket.com/adding-dynamic-colors-with-css/>

大多数开发者看到“动态”这个词，首先想到的就是 JavaScript。然而，大多数情况下，您可以仅使用 CSS 来实现动态功能。例如，考虑实现一个带有切换按钮的响应式标题，或者通过不同 div 或元素上的单击或悬停事件来影响一个 div。仅使用 CSS 就可以做到这一点。

在这篇文章中，我们将学习如何仅使用 CSS 在我们的应用程序中添加动态颜色。动态颜色是由内容编辑器定义的，这意味着设计系统不一定事先知道颜色。

我们可以使用 CSS 变量实现动态颜色特性，这在大多数现代浏览器中都是受支持的，除了 Internet Explorer 11。用 CSS 添加和操作动态颜色有几种不同的方法，在本文中，我们将探讨几种:

我们开始吧！

## 使用透明度

如果您以前在 CSS 中使用过颜色，您可能已经熟悉了使用自定义属性和 alpha 通道创建颜色。为了唤起您的记忆，请考虑下面的代码:

```
:root {
--color: 255 255 0;
}

.selector {
background-color: rgb(var(--color) / 0.5);
}

```

![Transparency CSS Colors Example Output](img/c46344d1b4d0a057a73a5d4682660ade.png)

上面的代码涵盖了创建自定义颜色属性的最简单的方法，但是这种方法是有缺陷的。它要求您在支持 alpha 通道的颜色空间中定义自定义属性`color`，就像`rgb(), rgba(), and hsla().`:

```
:root {
--color-rgb: 255 255 0;
--color-hsl: 5 30% 20%;
}

.selector {
background-color: rgb(var(--color-rgb) / 0.5);
background-color: hsl(var(--color-hsl) / 0.5);
}

```

![Main DIV CSS Alpha Channel](img/3a2eb0fc8fa2a67fdadd55d753092151.png)

因此，您不能允许自定义属性的颜色值从一种类型切换到另一种类型。在这个上下文中，我使用了`switch`，这有点像 JavaScript 和 Python 等语言中的类型转换。

这种不可能性的一个很好的例子如下:

```
:root {
--color: #fa0000;
}

.selector {
/*
Trying to convert a HEX color to an RGB one doesn't work
This snippet will not work. this just return a blank white background
*/
background-color: rgb(var(--color) / 0.5);
}

```

![CSS Switch Statement](img/802bccedd73ac8355ae980cc395bb844.png)

也就是说，在使用十六进制颜色值的 CSS 中使用动态颜色实际上是不可能的。即使您可以为十六进制颜色指定 alpha 通道，例如`#FA000060`，您也只能以声明的方式这样做。CSS 没有真正的字符串连接，这意味着您不能动态地指定 alpha 通道:

```
:root {
--color: #fa0000;
}

.selector {
/* You can’t dynamically specify the alpha channel. This will still not do anything */
background-color: var(--color) + "60";
}

```

由于缺乏指导，尝试用命名的颜色创建动态颜色是不可取的。然而，相对颜色 能够以一种更强大和更有用的方式创造动态颜色。

### 相对颜色

使用相对颜色，您可以声明一个具有任意颜色类型值的自定义属性，即`rgb, rgba, hsla, hsl, hex`等。你可以随时将它转换成任何其他类型。

如果你问我，这是迄今为止添加和操作动态颜色的最好方法。下面是如何做到这一点的示例:

```
/* - - - - - - - - - - - - Using hex Colors - - - - - - - - - - - - - - */
:root {
  --color: #fa0000;
}

.selector {
  /* can’t do this */
  background-color: rgb(var(--color) / 0.5);

  /* can do this */
  background-color: rgb(from var(--color) r g b / .5);
}

```

大多数人会尝试遵循第 8 行的语法，但是，这是行不通的。相反，第 11 行的正确语法显示了如何使用相对颜色来创建或操作动态颜色。这种方法甚至适用于正常命名的颜色:

```
/* - - - - - - - - - - - - Using Named Colors - - - - - - - - - - - - - - */
:root {
  --color: red;
}

.selector {  
  background-color: rgb(from var(--color) r g b / .5);
}

```

正如我前面提到的，当涉及到类型转换或类型强制时，这与 JavaScript 和 Python 等语言发生的事情是一样的。

## 使用`calc()`功能

尽管使用 alpha 通道添加动态颜色是可行的，但您一定会意识到它有缺点。这么想吧。透明色不会总是和白色融为一体。相反，它们会融入它们所在的颜色中。

基本上，你可以得到一种更浅的颜色，但它不会在整个页面上保持一致，因为它所在的颜色会影响它的外观。为了确保它所在的颜色不会影响它，你需要使用浅色或深色的不透明版本。

以前，您可以在 CSS 中通过在自定义属性定义中指定并单独定义所有通道来处理这一问题:

```
:root {
  /* Define individual channels of a specific color */
  --color-h: 0;
  --color-s: 100%;
  --color-l: 50%;
}

.selector {
  /* Dynamically change individual channels */
  color: hsl(
    var(--color-h),
    calc(var(--color-s) - 10%),
    var(--color-l)
  );
}

```

![CSS Calc Function](img/374e7dfca43c86956d78d6f12bf3a239.png)

然而，你已经可以看出你的代码会变得非常冗长。话说回来，像`HEX`这样的值并不真正被支持。对于 CSS 相对颜色，您可以使用`calc()`函数来处理，它将产生与上图相同的颜色:

```
:root {
  --color: #ff0000;
}
.selector {  
  color: hsl(from var(--color) h calc(s - 10%) l);
}

```

您会注意到相对于冗长的代码有了显著的改进。

## 过滤器百分比值

这些只是你用纯 CSS 添加动态颜色的主要方法。但是，除此之外，您还可以通过使用旧的滤镜百分比值来控制颜色。

`filter: brightness(x%)`功能用于按百分比动态改变颜色。`x%`值会将颜色控制为特定值。这种处理颜色的方法不太受欢迎，因为它会影响特定的元素，这意味着页面的某些部分可能会比其他部分更亮。

## SASS 和 JavaScript 中的其他方法

如果你想超越 CSS，你可以看看 SASS 和 JavaScript。SASS 立即提供了颜色处理功能，包括:

```
lighten() and >darken()
complement()
hue()
mix()
contrast-color()

```

我可以尝试进一步扩展这些，但这超出了这篇文章的范围。也就是说，为了更好地理解，你可以在他们的[文档](https://sass-lang.com/documentation/modules/color)中进一步阅读 SASS 颜色函数。

当使用 JavaScript 动态添加颜色时，您只需要使用 DOM 和 CSS color 属性。您可以选择页面加载时改变颜色、页面加载五秒钟后改变颜色等等。您基本上是在尝试创建对 DOM 可以监听的事件的响应，比如点击、鼠标滚动和上下键。真的，可能性是无穷的。

## 结论

你实际上并不需要动态的颜色，但话说回来，你有点需要。这完全取决于您的偏好、手头的当前任务以及您想要编写多少代码。从这个角度来看，如果你不做任何动态的事情，你就不需要动态的颜色。例如，思考像黑暗模式或颜色随鼠标移动而变化这样的事情。

你不需要 JavaScript 来使你的 CSS 颜色动态化。你只需要利用动态的颜色。知道如何创建和操作它们是你的 CSS 锦囊中的一个很好的工具。试试吧，记得玩得开心。编码快乐！

## 你的前端是否占用了用户的 CPU？

随着 web 前端变得越来越复杂，资源贪婪的特性对浏览器的要求越来越高。如果您对监控和跟踪生产环境中所有用户的客户端 CPU 使用、内存使用等感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/css-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/dacb06c713aec161ffeaffae5bd048cd.png)](https://lp.logrocket.com/blg/css-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/css-signup)

LogRocket 就像是网络和移动应用的 DVR，记录你的网络应用或网站上发生的一切。您可以汇总和报告关键的前端性能指标，重放用户会话和应用程序状态，记录网络请求，并自动显示所有错误，而不是猜测问题发生的原因。

现代化您调试 web 和移动应用的方式— [开始免费监控](https://lp.logrocket.com/blg/css-signup)。