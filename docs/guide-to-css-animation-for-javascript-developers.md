# JavaScript 开发者 CSS 动画指南

> 原文：<https://blog.logrocket.com/guide-to-css-animation-for-javascript-developers/>

众所周知，人类的大脑天生就具有运动能力。人类更可能关注元素如何运动，而不是关注静态元素。

CSS 动画利用了这种人类行为。当动画被添加到一个网站上时，它会将用户的注意力吸引到产品的重要区域，产生一种持久的效果，并且通常会增强用户体验。

在这篇文章中，我们将回顾 CSS 动画的好处，不同的 CSS 动画属性，以及 JavaScript 开发人员可以使用 CSS 动画使网站更具交互性和用户友好性的不同示例。

每一个例子都将伴随着一个 Codepen 演示和一个详细的解释，以使这些例子更加现实，实用和丰富。

## CSS 动画概述

在深入了解作为 JavaScript 开发人员如何使用 CSS 动画之前，让我们快速回顾一下 CSS 动画到底是什么，为什么需要了解它，以及它对网站的外观和感觉有什么影响。

### 什么是 CSS 动画？

顾名思义， [CSS 动画允许用户通过遵循一个声明性的模式来动画化一些 CSS 属性](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_animated_properties)，在这个模式中，用户可以指定在一段时间内 CSS 属性的变化。

CSS 动画使得从一个 CSS 样式配置到另一个 CSS 样式配置的动画转换成为可能。

### 为什么你应该理解 CSS 动画

web 开发生态系统已经发展到一个地步，JavaScript 开发人员无法避免理解 CSS 并制作动画。一个没有动画的用户界面就像一个没有动作的视频游戏，没有人再喜欢玩纯文本的游戏了！

CSS 动画满足了建立更多互动网站的需求。

### CSS 动画对网站有什么积极影响？

你有没有看过一个网站，想知道网页是否加载或损坏？这种常见的体验会让遇到它的用户感到沮丧。

通过使用 CSS 动画，开发人员可以通过添加一个加载动画来减轻这种挫折感，该动画向用户发出信号，表明正在发生一些事情，使他们在页面上停留更长时间。

做得好的话，动画可以为网站界面增加有价值的互动、个性和吸引人的用户体验。

> 动画可以使 UI 元素类似于真实世界，使它们平滑地变化，同时给人以连续、动作和进度的感觉，而不是一眨眼就变了。— [帕特里西亚·席尔瓦《如何制作 CSS 动画》](https://www.imaginarycloud.com/blog/how-to-make-css-animations/)

## CSS 动画说明

动画由两部分组成:描述 CSS 动画的样式和指示动画样式序列的关键帧。

让我们分解这两个组件，以便有效地理解它们。

### 描述 CSS 动画的样式

对于您创建的每个动画，您必须描述动画的特征。这使您可以完全控制决定动画能做什么或不能做什么。

您可以配置的一些属性示例包括动画重复的持续时间、方向和次数。

要描述动画，您可以使用`animation`速记属性或`animation`子属性。

#### `Animation`速记属性

`animation`速记属性是八个`animation`子属性的速记。它可以防止您浪费时间键入子属性名称，并为需要所有八个子属性的元素制作动画:

```
/* Here’s the syntax of the animation shorthand property */
.element {
  animation: name duration timing-function delay iteration-count direction fill-mode play-state;
}

```

当您将这段代码应用到一个元素时，`animation`速记属性会在页面上用所有八个子属性将元素动画化:

参见 [CodePen](https://codepen.io) 上的 Pen [动画速记属性——CSS 动画教程](https://codepen.io/edyasikpo/pen/KKWyRyz)作者 Didicodes([@ edyasikpo](https://codepen.io/edyasikpo))
。

#### `Animation` **子属性**

八个子属性组成了实际的`animation`速记属性，并在 CSS 中配置元素的动画。当您不想同时使用所有子属性或者当您忘记动画属性中的排列顺序时，它会变得很有用:

```
/* Here’s the syntax of the animation sub-properties. */
.element {
  animation-name: name;
  animation-duration: duration;
  animation-timing-function: timing-function;
  animation-delay: delay;
  animation-iteration-count: count;
  animation-direction: direction;
  animation-fill-mode: fill-mode;
  animation-play-state: play-state;
}

```

同样，当您将代码应用于元素时，它会呈现一个动画正方形:

参见 [CodePen](https://codepen.io) 上 Didicodes([@ edyasikpo](https://codepen.io/edyasikpo))
的 Pen [动画子属性——CSS 动画教程](https://codepen.io/edyasikpo/pen/ZEeaoLp)。

请注意，您不能同时使用`animation`速记属性和`animation`子属性，因为它们产生相同的东西。它们应该根据你想要达到的目标单独使用。

您可以在 MDN Web 文档中了解关于[每个子属性及其值的更多信息。](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Animations/Using_CSS_animations)

当您进入关键帧时，必须知道如果没有关键帧来指示动画的序列，使用样式来描述 CSS 动画就无法工作。

例如，下面的演示包括应该产生心跳的`animation-name`、`animation-duration`和`animation-timing-function`子属性。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

但是，您看不到心脏上的任何动画，因为还没有配置`@keyframes` at-rule 属性:

参见 [CodePen](https://codepen.io) 上 Didicodes([@ edyasikpo](https://codepen.io/edyasikpo))
的钢笔 [一颗心——CSS 动画教程](https://codepen.io/edyasikpo/pen/GRWOEaB)。

## 使用`@keyframe`表示动画序列

关键帧描述动画元素在动画序列中的给定时间如何渲染。由于动画的计时是使用`animation`速记属性或其子属性在 CSS 样式中定义的，因此关键帧使用百分比来指示动画序列。

要使用关键帧，创建一个与传递给`animation-name`属性的名称相同的`@keyframes` at-rule。在心跳演示中，`animation-name`是`heartbeat`，所以您也必须将`@keyframes`命名为 at-rule `heartbeat`。

每个`@keyframes` at-rule 包含一个关键帧选择器的样式列表，指定关键帧出现时动画的百分比，以及一个包含该关键帧样式的块:

```
@keyframes heartbeat {
  0% {
    transform: scale(1) rotate(-45deg);
  }
  20% {
    transform: scale(1.25) rotate(-45deg);
  }
  40% {
    transform: scale(1.5) rotate(-45deg);
  }
}

```

`0%`表示动画序列的第一个瞬间，而`100%`表示动画的最终状态。

现在您已经理解了`@keyframes`，让我们将它加入到心脏演示中，看看是否有什么变化:

参见 [CodePen](https://codepen.io) 上 Didicodes([@ edyasikpo](https://codepen.io/edyasikpo))
的 Pen [心跳——CSS 动画教程](https://codepen.io/edyasikpo/pen/NWpwjrp)。

如你所见，心脏正在跳动！

当您添加一个 CSS `@keyframes` at-rule 来使心形的大小从`0%`缩放到`40%`时，您设置:

*   0%的时间没有转换
*   20%的时间通过`scale(1.25)`将心脏缩放至其初始大小的 125%
*   40%的时间通过`scale(1.5)`将心脏缩放到其初始大小的 150%

添加`rotate(-45deg)`是为了保持你用 CSS 创建的心脏的原始方向。

## JavaScript 开发人员的动画示例

在这一节中，我们将回顾两个例子，JavaScript 开发人员可以使用 CSS 动画使网站更具交互性，并改善用户体验。

### 填写表格

表单是几乎每个网站上都有的组件。通常情况下，填写在线表格会很繁琐。

在本例中，您将看到一个登录表单，并了解如何用 JavaScript 控制动画来使网站对用户更具交互性。当用户试图在下面的登录表单中添加他们的电子邮件地址和密码时，表单上没有应用动画:

参见 [CodePen](https://codepen.io) 上 Didicodes([@ edyasikpo](https://codepen.io/edyasikpo))
的 Pen [Form 1——CSS 动画教程](https://codepen.io/edyasikpo/pen/MWpOzNY)。

虽然拥有一个没有动画的表单完全没问题，但是它在视觉上对用户没有吸引力，并且很可能不会引起他们的注意。

但是在下面的动画登录页面中，当用户开始输入他们的信息时，**电子邮件**和**密码**字段中的字符同时上移:

参见 [CodePen](https://codepen.io) 上 Didicodes([@ edyasikpo](https://codepen.io/edyasikpo))
的 Pen [Form 2——CSS 动画教程](https://codepen.io/edyasikpo/pen/BaWmxXo)。

虽然这是一个微妙的动画，但它吸引了用户的注意力，并在以下方面改善了他们的体验:

*   它向用户表明他们点击了输入字段
*   用户现在意识到他们可以开始打字了

这可以创造一个更加人性化的环境，让人记忆深刻，引人注目。

### 滚动浏览页面

当用户浏览一个没有动画的网站时，他们经常会错过重要的信息。

让我们滚动浏览两个包含信息列表的页面，一个包含静态元素，另一个包含动画元素:

参见 [CodePen](https://codepen.io) 上 Didicodes([@ edyasikpo](https://codepen.io/edyasikpo))
的钢笔[卷轴 1-CSS 动画教程](https://codepen.io/edyasikpo/pen/rNypLBP)。

参见 [CodePen](https://codepen.io) 上 Didicodes([@ edyasikpo](https://codepen.io/edyasikpo))
的钢笔 [滚动 2- CSS 动画教程](https://codepen.io/edyasikpo/pen/PopOeMw)。

因为动画在第二个列表中从左边和右边带来内容，所以它可以减慢用户的速度，以确保他们阅读每个选项，不像没有动画的第一个页面。它还可以帮助用户滚动到最后，查看所有可用的信息。

我的朋友，这就是给网站添加动画的力量！

## 需要一些动画灵感？

这里列出了五家使用 CSS 动画为用户创造更好体验的公司。如果你浏览一下这些网站，就会发现它们的互动非常吸引人，让你不停地滚动到页面的末尾，或者与页面上的一个 CTA 互动:

当然，这并不是世界上唯一使用 CSS 动画的网站，但这五个网站很可能会给你所需的灵感。

## 结论

总之，作为一名 JavaScript 开发人员，CSS 动画是为用户创造难忘体验所需的工具。你可以在这里找到所有的动画 CSS 演示。

如果你有任何问题，请在下面的评论区分享，我会回复每一条评论。

## 你的前端是否占用了用户的 CPU？

随着 web 前端变得越来越复杂，资源贪婪的特性对浏览器的要求越来越高。如果您对监控和跟踪生产环境中所有用户的客户端 CPU 使用、内存使用等感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/css-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/dacb06c713aec161ffeaffae5bd048cd.png)](https://lp.logrocket.com/blg/css-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/css-signup)

LogRocket 就像是网络和移动应用的 DVR，记录你的网络应用或网站上发生的一切。您可以汇总和报告关键的前端性能指标，重放用户会话和应用程序状态，记录网络请求，并自动显示所有错误，而不是猜测问题发生的原因。

现代化您调试 web 和移动应用的方式— [开始免费监控](https://lp.logrocket.com/blg/css-signup)。