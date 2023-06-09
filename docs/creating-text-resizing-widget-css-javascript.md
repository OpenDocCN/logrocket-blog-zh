# 用 CSS 和 JavaScript 创建文本调整小部件

> 原文：<https://blog.logrocket.com/creating-text-resizing-widget-css-javascript/>

提高一般网站的可访问性是一个巨大且不断变化的过程，但重要的是要确保网站和应用程序对所有用户都是可访问的。

在本教程中，我们将关注用户体验的一个简单而重要的方面:文本大小。对于一些用户来说，“正常大小”的文本可能太小而无法舒适地阅读或者根本无法阅读，并且，根据字体大小和字体系列，即使视力良好的用户也可能难以舒适地阅读文本。

有许多方法来适应用户，其中之一是实现一个文本大小调整小部件。最简单的形式是，这个小部件是一对增加或减少文本大小的按钮或链接。

让我们学习如何实现一个文本大小调整小部件。

## 可调文本的可访问性标准

可调文本大小的要求来自 [WCAG 2.0 准则 1.4.4](https://www.w3.org/TR/2008/REC-WCAG20-20081211/#visual-audio-contrast-scale) :

> 调整文本大小:除了文本的标题和图像，文本可以在没有辅助技术的情况下调整 200%的大小，而不会丢失内容或功能。(AA 级)

这里没有太多的内容，但是 WCAG 2.0 在[理解章节 1.4.4](https://www.w3.org/TR/UNDERSTANDING-WCAG20/visual-audio-contrast-scale.html) 中提供了额外的信息。主要要点如下:

*   其目的是帮助视力低下的人阅读网页上的文字
*   文本的大小必须调整到至少 200%
*   调整文本大小时，内容不得被剪切、截断或遮挡

值得注意的是，文本大小调整小部件不是必需的。这是确保用户能够阅读你的网站的[建议技术](https://www.w3.org/TR/2016/NOTE-WCAG20-TECHS-20161007/G178)之一，但是也有其他可接受的方法来调整你的文本，比如简单地确保你的网站响应浏览器的文本大小设置。

我对需求的解释归结为:所有关键文本的大小必须至少是默认字体大小(16px)的 200%。不可否认，这是一个有点松散的解释，但我认为任何大于 32px 的字体都大于默认字体大小的 200%,所以它应该已经足够易读了。

此外，如果你有大标题，你把它们的大小调整到原来大小的 200%,这很可能会导致剪辑或其他布局问题，这不符合指南。

## 改装文本小部件的可行性

现在，让我们来谈谈实现文本小部件的可行性。具体来说，我将在现有站点上改造一个小部件的背景下讨论这一点——这个站点在设计或构建时不一定考虑到文本大小调整。

像这样的网站可能会面临许多挑战，但我将重点关注两个挑战:

*   一些字体大小可能以像素为单位，而不是像`em`或`rem`这样的相对单位。是的，这是一个大禁忌，但现实是，这样的网站仍然存在，原因有很多
*   如果网站上有非常大的文本，增加它的大小可能会破坏布局

我们的小部件有几种不同的工作方式。我们可以让它改变`<html>`或`<body>`的`font-size`。这很简单，但它不会影响以像素为单位设置的字体大小，而且它会针对网站上的所有文本，包括已经很大的文本。

经过多次反复之后，我决定采用这种方法:

*   使用 CSS 选择器来指定应该调整的内容
*   对于每个选择器，获取初始的`font-size`
*   调整文本尺寸时，根据初始尺寸更新每个选择器的`font-size`
*   将文本大小设置存储在浏览器内存中，以便在页面和会话中保持不变

这种方法有许多优点。首先，它非常灵活，几乎可以在任何网站上使用，无需更新标记或样式。字体大小最初可以是任何单位，因为脚本将读取初始大小，并将其用作计算新字体大小的基线。这种方法也可以用来创建一个系统，其中的类可以调整特定文本的大小。

## 创建文本大小调整小部件

这里是 Codepen 中的一个演示。让我们看一下剧本。

首先，为所有可调整大小的文本元素创建一个 CSS 选择器数组。您可以使用标记中已经存在的选择器，也可以专门为此创建一个类。

```
let selectorsToScale = [
  'p',
  'button',
  '#some .very .specific.selector',
  '.a-class-that-makes-text-resizable'
];

```

对于每个选择器，获取`font-size`并将选择器和字体大小作为键值对存储在一个数组中。

```
for (const selector of selectorsToScale) {
  let fontSize = $(selector).css('font-size');

  if (fontSize) {
    fontSize = fontSize.slice(0, fontSize.length - 2);
  } else {
    fontSize = '';
  }

  initialSizes[selector] = fontSize;
}

```

当用户使用小部件调整文本大小时，设置会保存在浏览器中。该设置是一个从 100 到 200 的简单数字，以百分比表示文本大小的比例因子。

在这里，我们检查是否有保存的设置，以便文本大小调整跨页面和浏览会话保持不变。

```
if (fontSizeSetting) {
  setFontSizes(fontSizeSetting);
} else {
  fontSizeSetting = 100.0;
}

```

单击调整大小按钮时，更新设置变量，将其存储在浏览器中，然后更新字体大小:

```
function changeFontSizes(direction) {
  if (direction === 'down') {
    fontSizeSetting -= 10;
  } else {
    fontSizeSetting += 10;
  }
  localStorage.setItem('codepenFontSizeSetting', fontSizeSetting);
  setFontSizes(fontSizeSetting);
}

```

最后，让我们把重点放在实际调整文本大小的部分。对于我们在开始时指定的每个选择器，我们基于初始值和新的文本大小设置计算新的`font-size` —以像素为单位，然后将该`font-size`应用于选择器。

```
function setFontSizes(setting) {
  const resizeFactor = setting / 100.0;

  indicator.html(parseInt(setting));

  for (const selector of selectorsToScale) {
    let initialSize = initialSizes[selector];

    initialSize = parseFloat(initialSize);

    let newSize = initialSize * resizeFactor;

    $(selector).css('font-size', newSize + 'px');
  }
}

```

最后但同样重要的是，我对小部件的外观有一些想法。G178 技术在细节上有些欠缺，但是让这个小工具对那些需要它的人来说容易使用是很重要的。文本应该相当大，对比度高，按钮应该易于点击。

小部件本身也应该很容易在页面上找到，它的目的应该很明确，所以避免使用时髦但模糊的图标。为了让它不碍事但仍然容易访问，我选择让我的文本调整小部件从屏幕的侧面或底部滑出，但这是可选的。静态小部件也是完全可以接受的。

## 结论

当为网络而建时，有一千种方法可以把事情做好。调整文本大小也不例外。这里概述的方法是一种相对低摩擦的方法，可以在现有网站上调整文本大小，但最终，适合您的方法将取决于您项目的平台、代码库、受众和预算。

## 你的前端是否占用了用户的 CPU？

随着 web 前端变得越来越复杂，资源贪婪的特性对浏览器的要求越来越高。如果您对监控和跟踪生产环境中所有用户的客户端 CPU 使用、内存使用等感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/css-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/dacb06c713aec161ffeaffae5bd048cd.png)](https://lp.logrocket.com/blg/css-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/css-signup)

LogRocket 就像是网络和移动应用的 DVR，记录你的网络应用或网站上发生的一切。您可以汇总和报告关键的前端性能指标，重放用户会话和应用程序状态，记录网络请求，并自动显示所有错误，而不是猜测问题发生的原因。

现代化您调试 web 和移动应用的方式— [开始免费监控](https://lp.logrocket.com/blg/css-signup)。