# 人工智能在更好、更快的网络开发中的进步

> 原文：<https://blog.logrocket.com/advances-in-ai-for-better-faster-web-development/>

只有少数技术发展像人工智能一样吸引了公众的关注。在过去的几年里，我们看到它以惊人的速度增长，为不久前我们认为仅仅是科幻小说的事情铺平了道路。

随着人工智能的发展，web 开发也在飞速前进。用于创建更可预测、可测试、可读和可伸缩的 web 应用程序的现代框架的出现，使得开发人员能够跟上不断增长的对更好用户体验的需求。随着许多原生 API 的出现，浏览器得到了更好的优化，SEO 每天都在增加新的需求。

像许多其他行业一样，网络开发正在拥抱人工智能的力量，以使网络应用程序更好、更强大。如今，标准要求更快地交付经得起未来考验的应用。网络开发人员正在想办法利用人工智能来帮助他们。在这里，我们分析了人工智能正在帮助 web 开发以更快的速度增长的几个领域。

## 智能代码完成

代码完成一直是开发人员生产力的关键因素。它通过减少打字错误和其他常见错误来加速应用程序的编码过程。今天，代码自动完成通常使用内存中的类、变量名和应用程序中定义的其他结构的数据库。当用户开始输入时，IDEs 搜索可能的匹配，并在弹出窗口中给出建议。

AI 现在将上下文预测添加到代码完成中。让我们考虑一个例子，用户开始键入一个变量名`now`。IDE 可以完成从 DateTime 接口获取当前时间的方法。或者，如果开发人员键入一个变量作为`color`，IDE 可以从定义应用程序主题的界面提供完成。

谷歌最近[宣布 Dart 2.5 SDK](https://medium.com/dartlang/announcing-dart-2-5-super-charged-development-328822024970) 具有 ML Complete——由机器学习驱动的代码完成。它使用一个 [TensorFlow](https://blog.logrocket.com/tensorflow-js-an-intro-and-analysis-with-use-cases-8e1f9a973183/) Lite 模型来预测开发者正在编辑的下一个符号。

![Code Completion With ML Complete](img/158cedbde179331a881750dbb1cbb85d.png)

![Code Completion Without ML Complete](img/0e95c6b91336ca4b03c779af00f2339f.png)

The code completion experience with ML Complete (above) and without (below).

## 智能预测

如今，Web 开发人员已经使用 webpack 和其他类似的库执行代码拆分。这些库中的开发使我们能够优化向最终用户交付代码的方式。

[Addy Osmani 分享了一个数据驱动的方法来预取用户接下来可能访问的页面的想法](https://github.com/addyosmani/predictive-fetching)。预测预取可以通过训练一个模型来预测用户可能会根据他们的旅程访问哪些页面来实现。

首先，这可以是一个简单的模型，它依赖于关于应用程序总体使用情况的数据。使用深度神经网络来分析特定用户可以取得进一步的进展。

除了用户的旅程之外，还有其他因素会影响页面下次被访问的概率。例如，在移动设备上靠近用户手的位置的链接比远离用户直接触及的链接更有可能被访问。

[Guess.js](https://github.com/guess-js/guess) 是目前为止为 web 应用添加预测预取的最佳方式。它有一个 webpack 插件，支持 Angular、Next.js、Nuxt.js 和 Gatsby。

## 自动化测试用例

图像识别正被用于将用户界面测试提升到一个新的水平。动态 UI 控件可以被识别，而不管它们的形状和大小，因此 AI 可以分析界面，以检查变化是有益的还是破坏系统。人工智能还可以帮助分析用户界面的某些部分是否符合产品所服务的受众的需求和愿望。

创建满足所有可能用例的单元测试有时会很麻烦。人工智能有一个自动化的测试用例生成。通过使用人工智能生成的单元测试，开发人员可以实现更高的代码覆盖率，同时将构建全面而有意义的单元测试套件所需的时间和精力减少一半。

另一种情况是通过检查当前数据和生成端到端测试流来预测用户旅程。这将允许 QA 工程师在保持现有功能不变的同时，更专注于测试新功能。

以下是一些利用人工智能改变软件测试的工具:

*   **[Test.ai](https://www.test.ai/) :** 一家由前谷歌和前微软测试领导的公司，它提供了一个人工智能驱动的测试自动化平台，以帮助移动应用分销商向他们的客户提供优质的用户体验
*   [**testim . io**](https://www.testim.io/)**:**一个创作、执行和维护自动化测试的机器学习工具
*   [**AISTA**](https://www.aitesting.org/) **:** 虽然它不完全是一个工具，但软件测试人工智能协会将利用人工智能进行 QA 的测试人员联系起来

## 更好的搜索引擎优化:更好的关键词和多语言图像标签

从技术审计、关键词研究和内容优化到内容分发、标签管理和内部链接，人工智能正在对当今的 SEO 产生巨大影响。除了从一个来源产生多语言内容，人工智能还帮助生成相关的元信息。

对于大型电子商务组织来说，针对他们展示的每个产品图像生成适当的关键字是一项昂贵的任务，并且查找多种语言的相关标签会大大增加成本。今天，复杂的图像识别技术可以从显示的图像中自动生成多语言标签。

此外，文本分析方面的进步正在帮助内容作者和营销人员针对页面上的大型文档和动态数据生成相关的标签和关键字。这也有助于作者轻松地将他们之前制作的内容与新鲜酿造的东西联系起来。

## 为每个人量身定制体验

人工智能已经准备好驱动下一代网站个性化，这可能会永远改变互联网的本质。我们正在走向一个时代，在这个时代，网站将自我调整，为每个用户提供量身定制的完美体验，而不是继续采用一刀切的方式。

这很可能通过人工智能工具的演变来实现，这些工具为今天的[人工设计智能(ADI)平台](https://smallbiztrends.com/2016/06/wix-artificial-design-intelligence.html)和分析系统提供动力。有了 Adobe 和 Wix 等公司的巨额投资，ADI 公司的未来一定会非常光明。

## 结论

从人工智能已经如何影响现代世界——以及它仍在前进的速度来看——很明显，我们只是看到了这项技术将在行业中发挥的破坏性力量的开端。

展望未来，几乎可以肯定的是，人工智能将在软件开发的每个方面发挥主要作用，并将为我们所认为的艺术水平设定新的基准。

## 使用 [LogRocket](https://lp.logrocket.com/blg/signup) 消除传统错误报告的干扰

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/signup)

[LogRocket](https://lp.logrocket.com/blg/signup) 是一个数字体验分析解决方案，它可以保护您免受数百个假阳性错误警报的影响，只针对几个真正重要的项目。LogRocket 会告诉您应用程序中实际影响用户的最具影响力的 bug 和 UX 问题。

然后，使用具有深层技术遥测的会话重放来确切地查看用户看到了什么以及是什么导致了问题，就像你在他们身后看一样。

LogRocket 自动聚合客户端错误、JS 异常、前端性能指标和用户交互。然后 LogRocket 使用机器学习来告诉你哪些问题正在影响大多数用户，并提供你需要修复它的上下文。

关注重要的 bug—[今天就试试 LogRocket】。](https://lp.logrocket.com/blg/signup-issue-free)