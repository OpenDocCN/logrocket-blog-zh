# 用顺风 CSS 应用动态样式

> 原文：<https://blog.logrocket.com/applying-dynamic-styles-tailwind-css/>

Tailwind CSS 为开发创造了奇迹——它可以让您在几分钟内启动并运行。它包含开箱即用的正确构建模块和选项，可以定制整个系统中的任何内容。

如果你从来没有从头开始构建过一个设计系统，很容易认为像 [Tailwind CSS](https://blog.logrocket.com/creating-custom-themes-tailwind-css/) 这样的东西是理所当然的，尤其是在设置字体比例、间距网格和颜色的时候(这是 Tailwind 真正的亮点)。顺风的世界级设计师在选择合适的色调和色调时一丝不苟，能够让任何东西看起来都很棒。

有时，你可以得到一个带有品牌颜色的设计，当你预先知道这些价值时，你可以在顺风中扩展或者用你喜欢的任何颜色覆盖默认颜色。

但是，如果您无法控制最终出现在用户浏览器中的确切颜色，会发生什么呢？如果颜色来自后端，或者是动态的，并通过用户输入来控制呢？你会重新使用嵌入式风格吗？或者在 Tailwind 之外为这些用例生成单独的 CSS 样式？

希望不会！我在这里向您展示了一种更好的方式，一种 Tailwind 工作方式的原生解决方案，它的可扩展 API 允许我们比开箱即用的默认方式更进一步。

## 对顺风 CSS 应用动态颜色

我将展示我是如何用一个名为 NodCards 的 SaaS 小产品解决一个真实世界的用例的。这个概念很简单——它是你的数字名片。使用 NodCards，您的详细信息可以出现在个人登录页面上，在您的电子邮件签名中共享，在社交媒体上的个人简介链接中共享，或者在您希望共享信息的任何其他地方共享。

NodCards 是一个很好的例子，因为用户可以选择任何颜色作为主要设计颜色，这需要 NodCards 动态适应。我开始使用 Tailwind CSS 进行样式设计，那么我是怎么做的呢？

考虑到这种情况，并知道 Tailwind 类是如何工作的，我们希望将文本颜色`text-{primary}`作为人名的目标，以及按钮的背景颜色`bg-{primary}`，可能还需要更多的悬停效果。

![Profile With Blank Colors](img/00de8c38d2e6c2ebcf0b037eda8b81ed.png)

Here, the name needs the primary text color, and the buttons need the primary background color.

你的第一个想法可能是，感谢[实时(JIT)编译器](https://blog.logrocket.com/exploring-jit-mode-tailwind-css/)，它可以像`bg-[#6231af]`一样动态地构建样式。但我认为它并不是我们必须做的事情的真正竞争者，你很快就会明白为什么。

对于我的用例，我有一些注意事项和要求:

1.  如何在不改变标记(CSS 类)的情况下应用动态颜色？我可能会有成千上万的用户想要不同的颜色，所以这不应该在扩展标记或样式表时增加任何开销
2.  我如何创建一个单一原色的各种色调？
3.  如何确定在动态原色之上显示的最佳可访问颜色？

基于这些标准，我[制作了这个项目的一个简短演示](https://dynamic-colors-in-tailwind-demo.vercel.app/)。为了帮助可视化，我通过添加颜色选择器来模拟 UI 中颜色的动态变化，从而使演示具有交互性。如您所见，它会立即改变，而不会影响样式表和/或标记。

让我们看看我们是如何做到这一点的。

## 使用 React 构建 Next.js 站点

我发现最好的方法是利用 CSS 变量的力量。同样的方法可以应用于任何语言或框架。在我的例子中，我使用 React 构建了一个 Next.js 站点，但是概念是可以转移的。下面是完整的[源代码](https://github.com/fbrill/dynamic-colors-in-tailwind)。

## 编写助手函数

大多数时候，我们会从后端得到一个十六进制的颜色。在这种情况下，我编写了一个快速助手函数来帮助我们将十六进制颜色从后端转换成正确的 RGB 格式。

我们需要的第二个效用函数是一种方法，用于获得高对比度的可访问颜色(根据 [WCAG 指南](https://www.w3.org/WAI/standards-guidelines/wcag/))以显示在动态原色之上。我创建了一个实用程序文件来集中我的助手函数，我们稍后可以使用:

```
// utils/index.js

/////////////////////////////////////////////////////////////////////
// Change hex color into RGB /////////////////////////////////////////////////////////////////////
export const getRGBColor = (hex, type) => {
  let color = hex.replace(/#/g, "")
  // rgb values
  var r = parseInt(color.substr(0, 2), 16)
  var g = parseInt(color.substr(2, 2), 16)
  var b = parseInt(color.substr(4, 2), 16)

  return `--color-${type}: ${r}, ${g}, ${b};`
}

/////////////////////////////////////////////////////////////////////
// Determine the accessible color of text
/////////////////////////////////////////////////////////////////////
export const getAccessibleColor = (hex) => {
  let color = hex.replace(/#/g, "")
  // rgb values
  var r = parseInt(color.substr(0, 2), 16)
  var g = parseInt(color.substr(2, 2), 16)
  var b = parseInt(color.substr(4, 2), 16)
  var yiq = (r * 299 + g * 587 + b * 114) / 1000
  return yiq >= 128 ? "#000000" : "#FFFFFF"
}

```

`getAccessibleColor`功能的工作原理是将 RGB 色彩空间转换成 [YIQ](https://en.wikipedia.org/wiki/YIQ) ，如[计算色彩对比度](https://24ways.org/2010/calculating-color-contrast)中所述。在我们的用例中，我们需要一种可靠的方法来将文本颜色置于原色之上。

## 使用 CSS 变量

有了我们的助手函数，我们将动态原色从后端转换成可以在 CSS 变量中使用的 RGB 格式。然后我们得到`a11y`(可访问性)颜色。

我构造了我的函数来接收我声明的颜色类型的第二个参数。这允许我为我想要声明的 CSS 变量的任何组合重用该函数。向混合色中添加次要色、强调色或任何其他颜色都遵循完全相同的逻辑。

将格式转换为 RGB 是至关重要的一步。我们需要 RGB 格式的这些颜色的主要原因是，当我们通过 Tailwind CSS 合成新颜色时，我们可以为 RGBA 颜色添加一个 alpha 层。这确保了所有的`bg-opacity`和`text-opacity`类都可以工作，我们也可以通过处理不透明层来获得各种动态颜色的阴影。

对于 RGB 格式的`primaryColor`和`a11yColor`，我们需要声明一个作用于 HTML 文档根的 CSS 变量。在`:root`上添加这个意味着我们可以在 DOM 中任何使用 CSS 类的地方访问它。

```
// pages/index.js
const primaryColor = getRGBColor("#6231af", "primary")
const a11yColor = getRGBColor(getAccessibleColor("#6231af"), "a11y")
<!-- ... -->
<!-- ... -->
<Head>
  <style>:root {`{${primaryColor} ${a11yColor}}`}</style>
</Head>

```

现在，我们只是将它硬编码到`#6231af`，但是这很容易在以后替换为任何动态颜色的单一入口点。

## 了解顺风 CSS 中的 alpha 通道是如何工作的

Tailwind CSS 中的任何颜色都是通过立即在该类中声明一个`--tw-bg-opacity: 1;`实用程序来声明的。然后，声明的任何颜色都被转换为 RGBA 值，这里使用的是为 alpha 通道声明的`--tw-bg-opacity`值。

如果你只是声明一种颜色，那种颜色应该是纯色，但是当我们声明`bg-opacity-{value}`时，Tailwind CSS 重新声明了 alpha 通道，使我们能够用动态颜色实现各种级别的不透明度。

这实际上是 Tailwind CSS 的人非常聪明的做法，因为在 CSS 中没有办法声明或使用文本或只有背景的不透明度。实现这一点的唯一方法是使用 RGBA 的 alpha 通道，通过遵循他们设计的模式，我们可以充分利用他们颜色系统的潜力。

## 配置顺风 CSS

此时，我们在 HTML 中声明了一个 CSS 变量(它可以连接到我们的后端)。下一步是将 CSS 变量链接到一些要使用的 Tailwind CSS 类。

为了实现这一点，我们必须关注`tailwind.config.js`文件，这是所有神奇事情发生的地方。Tailwind 允许我们将颜色指定为函数，而不是字符串，以访问内部的 Tailwind 不透明度工具。这是我们将多次使用的东西，所以最好将其提取到我们的`tailwind.config.js`顶部的一个可重用函数中:

```
// tailwind.config.js
function withOpacity(variableName) {
  return ({ opacityValue }) => {
    if (opacityValue !== undefined) {
      return `rgba(var(${variableName}), ${opacityValue})`
    }
    return `rgb(var(${variableName}))`
  }
}

```

首先，我们从 Tailwind 接收颜色的不透明度值，我们可以使用 RGB 格式的颜色作为 alpha 通道。因为这可能没有被设置，我们执行了一个快速检查:如果它是`undefined`，我们返回它而没有任何 alpha。

在这两种情况下，我们只需将 CSS 变量的值以这种格式赋给`rgb`。这是现在准备使用时，组成我们的颜色。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

接下来，我们通过扩展主题来设置新的颜色:

```
// tailwind.config.js
theme: {
  extend: {
    textColor: {
      skin: {
        primary: withOpacity("--color-primary"),
        a11y: withOpacity("--color-a11y"),
      },
    },
    backgroundColor: {
      skin: {
        primary: withOpacity("--color-primary"),
        a11y: withOpacity("--color-a11y"),
      },
    },
    ringColor: {
      skin: {
        primary: withOpacity("--color-primary"),
      },
    },
    borderColor: {
      skin: {
        primary: withOpacity("--color-primary"),
        a11y: withOpacity("--color-a11y"),
      },
    },
  },
},

```

我喜欢用自己的前缀关键字`skin`来嵌套这些实用程序。这是可选的，但是当你输入类时，它会很方便，特别是当你使用类似于 [Tailwind VSCode](https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss) 扩展和 [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) 时，它会选择并显示你所有的新类。

我们按照计划用`textColor`和`backgroundColor`扩展了我们的主题，并添加了`ringColor`和`borderColor`变体，这样我们就可以在这些工具上使用我们的动态定制颜色。

## 观看动态色彩效果

有了这个，Tailwind 可以为我们生成一堆新的类。我们可以开始在我们的标记中使用这些新类。

现在，让我们将文本颜色更改为新的动态设置的原色。

```
<p className="... text-skin-primary">
  Jane Cooper
</p> 

```

在按钮上，让我们将背景设置为 30%不透明度的原色，这允许我们使用原色文本颜色。然后，我们可以在悬停时将背景设置为纯色，并在悬停时将图标切换为可访问性安全色。

我们也可以使用边框颜色和聚焦环颜色，它们都设置在我们的主要动态颜色中。

```
<a href={link.href} target="_blank" className="... bg-skin-primary bg-opacity-30 text-skin-primary hover:bg-opacity-100 hover:text-skin-a11y border-skin-primary focus:ring-skin-primary">
  <link.icon className="h-5 w-5" />
</a>

```

## 结论

回顾我们的需求，我们能够在不改变标记的情况下动态地设置我们的原色，获得我们的原色的不同色调，并且在我们的动态原色之上显示任何东西时保持良好的对比度。

你做过类似的事情吗？请在下面的评论中告诉我！

## 你的前端是否占用了用户的 CPU？

随着 web 前端变得越来越复杂，资源贪婪的特性对浏览器的要求越来越高。如果您对监控和跟踪生产环境中所有用户的客户端 CPU 使用、内存使用等感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/css-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/dacb06c713aec161ffeaffae5bd048cd.png)](https://lp.logrocket.com/blg/css-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/css-signup)

LogRocket 就像是网络和移动应用的 DVR，记录你的网络应用或网站上发生的一切。您可以汇总和报告关键的前端性能指标，重放用户会话和应用程序状态，记录网络请求，并自动显示所有错误，而不是猜测问题发生的原因。

现代化您调试 web 和移动应用的方式— [开始免费监控](https://lp.logrocket.com/blg/css-signup)。