# 最佳静态网站生成器比较

> 原文：<https://blog.logrocket.com/the-best-static-websites-generators-compared-5f1f9eeeaf1a/>

如果你没有跟上静态网站炒作的脚步，我会说你正在正确的时间阅读这篇文章。静态网站或所谓的 **JAMstack** 最近变得相当流行。

为什么？

为什么老派的 HTML + CSS + JS 以其重新设计和重命名的形式获得如此多的关注？在这篇文章中，我们将探究 JAMstack 到底是什么以及它有什么好大惊小怪的，然后我们将看看一些最好的静态网站生成器！

![](img/2de363edfc021e7941edf9b398977c2e.png)

JAMstack

JAMstack 这个术语最初是由 [**Netlify**](https://www.netlify.com) 使用作为旧的和不受欢迎的静态网站的新同义词。JAMstack 代表:

*   JavaScript — web 开发人员最好的朋友，一个执行逻辑的地方
*   **API—**JS 从中提取数据的提供者
*   **标记** —模板，在您的网站部署时处理

总而言之，JAMstack 背后的主要思想是消除客户机和服务器之间的任何紧密连接。接收数据的唯一方式是通过**API**。

### 论证

现在，是时候探索 JAMstack 的优势了。

#### 定价

**定价**大概是静态网站最重要的优势之一。要托管它们，您不需要服务器或 CMS(这意味着成本更低)。记住，你是以静态资产(HTML、JS、CSS、文本文件等)的形式处理/预构建你的网站。).这些服务可以便宜得离谱，除了标准的低价主机服务之外，没有更多的需求。

#### 表演

**性能**是静态网站的下一个关注点。这对你来说似乎很明显——静态资产很快——这是它们的本性。在这方面，没有什么能打败静态网站。使用 JAMstack 你可以很容易地达到高谷歌灯塔评分，从而在搜索结果中获得更高！除了速度，还有 cdn 的易用性。通过使用这些，你可以提高你的网站的交付速度甚至更多！

#### 稳定性

**稳定性**主要指对你的网站及其架构的高度信任。使用 JAMstack，事情真的很简单——除了静态前端之外，什么都不用做。你的数据来自 API。但是其他功能呢？

输入，无服务器提供商。主要思想是你使用第三方 API 和服务来为你的网站提供额外的功能。还有很多无服务器的提供者包括[**AWS**](https://aws.amazon.com)[**Google Cloud**](https://cloud.google.com/)和[**Netlify Functions**](https://www.netlify.com/features/functions/)！所有这些都适合静态网站。

#### 可量测性

基于 JAMstack 的网站很容易扩展。这主要是由于该解决方案背后的性能和架构，前面提到的**cdn**在这里发挥了重要作用。扩展静态资源交付无非意味着提供更好更快的 cdn。

#### 搜索引擎优化

SEO 是静态网站的另一个圣杯。当使用 JAMstack 时，拥有良好的 SEO 比使用 spa 要容易得多。静态网站得到更好的**灯塔**分数并且**可见**和**更容易被谷歌索引**。这是水疗真正缺乏的领域。对于这些，你必须使用 SSR 来获得和静态网站一样的 SEO 水平。

### 下降趋势

当然，静态网站也有一些缺点。可能最大的一个问题是它们是静态的，也就是说你没有任何真正的后台在运行。而且，由于不是每个功能都可以被无服务器服务取代，JAMstack 可能并不适合每个人。

更进一步说，静态网站不像动态网站那样可更新。要更新基于 JAMstack 的网站的内容，您需要对它进行预处理。而且，随着你的网站页面越来越多，这个过程会变得越来越慢。这就是为什么许多工具(我将在后面介绍)宣传的构建性能如此重要。

无论你为你的未来网站选择静态还是动态的道路，首先考虑两种解决方案的利弊总是好的。

### 工具作业

如果您决定使用 JAMstack，那么是时候探索这项工作的最佳工具了。我们将比较四种不同的发电机。其中一对基于 React，另外两个基于 Vue。这样你就可以选择一个最适合你个人喜好的。我们将比较一些基本因素，如开发经验、性能、支持、文档、社区等。

### 基于反应的

![](img/dce7588b077af6045cad92e2e101a6d7.png)

GatsbyJS landing page

GatsbyJS 可以说是最受欢迎的静态网站生成器之一。它允许您利用 Webpack，更重要的是，利用 React 创建令人惊叹的网站。除此之外，GatsbyJS 的构建速度足够快，可以处理超级复杂的东西。有很多插件可以让你从无数来源获取数据，并轻松地为你的网站添加额外的功能。

#### **入门**

GatsbyJS 在其官方网站上提供了大量的资源，用于启动其整个生态系统。有一个循序渐进的[教程](https://www.gatsbyjs.org/tutorial/)，面向 web 开发和 NodeJS、Git 之类东西的完全初学者(尽管我建议在直接使用 Gatsby 和任何其他列出的工具之前掌握更多的知识)。还有一个[快速入门](https://www.gatsbyjs.org/docs/quick-start)部分供更高级的用户使用。

当你对盖茨比感到更舒服，并想开始深入挖掘时，有大量的[文档](https://www.gatsbyjs.org/docs/)供你探索。涵盖环境设置、数据源、生态系统介绍等主题，一直到部署阶段，您会发现它非常有见地。除此之外，还有 API 参考、性能建议和其他更高级教程的链接。

#### **用途**

在选择工具时，开发经验是决定性因素之一。有了盖茨比，事情就简单多了。你应该在 React 生态系统中，意思是 React 本身、相关的库、JSX 和其他东西。如果反应不是你的事，那么盖茨比也不是。但是，如果您喜欢使用 React，那么 Gatsby 很可能是您静态网站的首选。在这里，一切都是用 React 构建的。整个页面都是 React 组件！您可以随意使用任何您喜欢的现代 JS 特性，例如，与 Babel 或 TypeScript 的集成很容易实现。事实上，这里有一整套的 [**盖茨比启动器**](https://www.gatsbyjs.org/starters/?v=2) ，你可以用它们来快速建立你自己的项目。

您将在 Gatsby 项目中使用的所有数据都来自于[](https://graphql.org/)**来源。如果你不知道的话，GraphQL 是一种由脸书开发的用于查询数据的查询语言。它与 React 集成在一起，总体来说非常健壮和出色。但是，如果你真的不喜欢 GraphQL，那么还有一个绕过 **it** 的 [**方法。当然，这需要省略盖茨比的数据层，从而使一些东西感觉不那么…精致。但是，无论如何，有一个选择是好的。**](https://www.gatsbyjs.org/docs/using-gatsby-without-graphql/)**

 **最后，《盖茨比》最重要的一个方面是它的生态系统。更具体地说，我说的是大约 700 个插件[](https://www.gatsbyjs.org/plugins/)**，它们都在那里，列在官网上，随时可以使用。而且，虽然主动维护的数量可能会少一些，但这使得 Gatsby 的开发容易得多。这些插件可以处理各种事情，如数据源(如 headless CMS 等)、自定义功能(如 PWA 功能、自定义网站搜索)和构建优化(如调整图像大小)。安装一个插件，然后继续你的工作，这种便利真是太棒了。**

 **#### **性能**

现在，当谈到任何类似生成器的工具的性能时，我们有两个不同的类别:生成器构建的性能和它的实际输出。而盖茨比输出网站的表现[简直令人咋舌](https://www.gatsbyjs.org/blog/2018-10-03-gatsby-perf/)！这很容易实现💯灯塔用这个得分。这不仅仅是因为它是一个静态网站。盖茨比在幕后做了大量的优化，让你的网站总是感觉流畅。

当谈到建立性能时，盖茨比有点落后。它在这里也做了一些优化，例如，每一次构建都会比第一次更快，但这似乎还不够。据报道，有一些问题与盖茨比的建设需要相当长的时间。不过，从积极的方面来看，这一点一直在努力。Gatsby v2 在这一领域做出了一些重大改进，显然，这同样适用于未来的 **v3** 。

#### **支持&社区**

盖茨比是一个伟大的、[积极开发的](https://www.npmjs.com/package/gatsby)和一个正在进行的[项目](https://isitmaintained.com/project/gatsbyjs/gatsby)。随着越来越多的人每天使用它，一些大玩家依赖它，它不会很快去任何地方。仅仅基于 React 和相关工具，你可以很容易地找到任何问题的帮助，不仅在 Gatsby 的，而且在 React 的巨大社区。

![](img/e4abd976a5af4ca78a40057d8e17a1e0.png)

Next.js landing page

Next.js 是另一个基于 React 的静态网站生成器。它由[时代](https://zeit.co)创建，在 React 社区广为人知。它不仅是一个生成器，而且内置了 SSR 支持。这让你可以选择你要创建的网站类型。Next.js 还简化了 React 开发的整个过程。也就是说，它可以作为所有与开发 React 网站有关的东西的一体化框架。

#### **入门**

Next 有一个完整的入门部分，涵盖了开始使用它所需要知道的一切。有了一个支持 SSR、静态网站和无服务器部署的框架，为所有这些东西创建一个好的指南并不是一项简单的任务。这就是为什么 Next 以一种有趣的方式对待它的教程——让它变得互动。每当你完成教程的一部分，你都会被一个简单的、与主题相关的问题所欢迎。如果你回答正确，你会得到一些分数。这是一个小的，但值得注意的细节。

当您需要更多信息时，您可以随时访问[文档](https://nextjs.org/docs)。他们的设计简单易读。每个特性都有很好的文档记录，包括适当的例子和代码片段。

#### **用途**

同样，Next 主要是为那些喜欢 React 的人准备的。它只是在它的基础上进一步改进。通过所有可能的输出(例如 SSR)和特性，这个框架的整体复杂性隐藏在 React 的简单性后面，只增加了一些组件和功能。这很大程度上保证了你更有效率，而不用关心到底发生了什么。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

很直观。你可以用提供的 Head 组件编辑你的网站的头部，用 link 组件链接到其他页面，用 *styled-jsx* 样式化你的组件，并以你喜欢的任何方式获取数据。但可能给我印象最深的是，您不必在每个页面或组件(页面自然是 react 组件)中从“React”导入*作为 React。这是一个小细节，但它使开发体验变得更好。

与盖茨比不同，Next 不仅仅是一个静态的网站生成器。在这里，您可以编写一次 web 应用程序，并以多个输出为目标。正如这个项目的首页所说——你可以很容易地用它来定位电子、PWA、静态网站、服务器渲染网站等等。它不仅仅是一个发电机。也许这就是为什么没有像盖茨比那样的生态系统或完善的插件系统。也就是说，与 React 的下一次集成是如此之好，以至于你可以使用任何你想使用的 React 相关的库来实现你的目标。

#### **性能**

Next 的性能可能是一件很难谈论的事情。这是因为有多少输出类型是可能的。Next 主要关注 SSR，这也是它做得最好的地方。但是，因为我们在这里谈论的是静态网站，所以事情有点不同。就像盖茨比一样，(很自然地)Next 生产快速优化的静态网站。至于生成网站本身的过程——已经优化了，但肯定还有改进的空间。

#### **支持&社区**

谈到[社区](https://github.com/zeit/next.js/stargazers)，Next 显然是赢家。大概就是**[**最流行的**](https://www.npmjs.com/package/next) **React 框架**。正因为如此，无论何时你需要帮助，你都可以很容易地找到。Next 也是由 [**Zeit**](https://zeit.co) 制造，这家公司也创造了 [**现在**](https://zeit.co/now) (无服务器部署平台)。这就是(以及上面提到的各种原因)为什么我认为 Next 总体上有很大的支持和后盾。**

 **### 基于 Vue

![](img/02abe7f6e0a8d1f161a2ab18b89bf3f4.png)

Gridsome landing page

从 Vue JAMstack 网站生成器开始，我们有 Gridsome。它就像盖茨比的对应物，但有 Vue 支持它！它有同样令人印象深刻的构建和页面浏览性能，PWA 和 SEO 支持，以及其他 JAMstack 优点！也许它没有 GatsbyJS 那么大的社区，但考虑到这个项目的年龄(2018 年 10 月出生)，它还是不错的。也就是说，它的[文档](https://gridsome.org/docs)和社区决心确实令人印象深刻。我个人对这个项目寄予厚望！

#### **入门**

Gridsome 的所有文档都放在一个位置。打开它时，你会看到一个小小的[入门](https://gridsome.org/docs)部分。它很短，但足以让经验很少的开发人员赶上。

对于这样一个年轻的项目来说，其余的文档非常详细，写得非常好。在创建你自己的 Gridsome 静态网站之前，它几乎有你需要知道的一切。无论是关于数据源、路由、页面转换、部署还是 API 参考——这些[文档](https://gridsome.org/docs)会为您提供帮助。

#### **用途**

Gridsome 的整个架构都是基于 [Vue](https://vuejs.org/) 的。页面、组件、链接——几乎所有东西都是 Vue 组件。只有 Head 和 HTML 属性必须从主 JS 文件中设置。

Gridsome 中的数据可以通过官方 **GraphQL 数据层** (hello Gatsby)接收，也可以通过其他方式接收(比如用 fetch 动态数据)。Gridsome 提供定制的块来保存 GraphQL 查询。这样，您的查询不必是字符串文字，这使得编写它们更加方便。

至于生态系统，Gridsome 也从 Gatsby 那里获得了一些想法。有一个现成的模板集合，这在这个时候不是太令人印象深刻(两个官方模板)。同样的道理也适用于(大约 20 个)插件的集合，这些插件允许你从不同的来源获取数据，例如，很容易地将谷歌分析添加到你的网站。但是，随着越来越多的人开始使用 Gridsome，这种情况很可能会随着时间的推移而改善。

#### **性能**

在建造性能方面，Gridsome 与 Gatsby 和 Next 并驾齐驱。当然，还有改进的空间。但是，比较基于两个不同库的生成器很有趣。这里你可以清楚地看到 Vue 和 React 都没有提供更好的换气次数。可以有把握地假设，JSX 和 Vue 组件语法在这一类别中匹配得很好。

#### **支持&社区**

社区是 Gridsome 缺乏的一个方面，主要是因为它太新了。尽管自其发布以来，关于它的资源和文章数量迅速增长，但它仍然无法与 **Nuxt** (我们稍后会谈到)相提并论。我只是建议任何 Vue 开发者尝试一下，或者至少关注一下它的发展。

![](img/608e3f577715b9b6db9b2985f7bd62df.png)

Nuxt.js landing page

Nuxt.js 是一个成熟的 Vue 框架。这是什么意思？嗯……它有多达三种渲染模式:SSR、静态网站和 SPA，这三种模式各有各的优势。作为一个一体化的解决方案，Nuxt.js 允许你选择最适合你的网站。考虑到这一点，这对于任何要求苛刻的 Vue 项目都是一个很好的解决方案。

#### **入门**

就像 Gridsome 是盖茨比的替代品一样， [Nuxt](https://nuxtjs.org/) 是 Next 的替代品。这是一个创建任何类型的 Vue 应用程序的很好的框架，在 Vue 社区中很有名。在其官方网站上，你可以找到一个 [**指南**](https://nuxtjs.org/guide) ，它会教你 Nuxt 提供的每一个功能。它非常详细，解释了基本配置、路由、数据源、状态管理和许多部署技术。

接下来，您将看到另外两个部分，涵盖了 API 参考和一些交互式示例。 [**API 文档**](https://nuxtjs.org/api) 涵盖了每一个方法、类、组件等。Nuxt 所提供的，使它成为你可以掌握的不可思议的知识来源。另一方面，示例向您展示了一些常用的模式和 Nuxt 的其他用例。

#### **用途**

Nuxt 极度依赖于 **Vue 生态系统**。它有工具和库，如用于 vue 组件的 vue-loader、用于状态管理的 Vuex 和集成到其核心的用于路由的 vue-router。这意味着如果你以前使用过这些工具，你很可能使用过(假设你是 Vue 开发人员)，那么 Nuxt 只会让你的开发过程更容易，在一个漂亮的单一包中提供所有这些工具。

就像 Next 一样，Nuxt，正如我前面提到的，允许您用相同的代码实现多种输出格式。这使得发展非常令人满意。当然，所有的输出都运行得非常平稳，因此您只需很少的额外工作就可以获得很好的性能。

#### **性能**

至于性能，与输出的情况没有什么不同。静态网站输出工作良好，具有最佳性能。

当谈到 Nuxt 作为发电机的速度时，可能会有一些问题。在过去，Nuxt 已经被认为是有点慢和有问题的。在 v1.x.x 中，构建花费了大量时间，Nuxt 的一些东西看起来不太好。但是，从 v2 开始，它发生了巨大的变化。最重要的变化之一是切换到 Webpack v4(和一些较小的变化),这带来了性能的显著提升。现在，Nuxt 是其他发电机的有力竞争对手——甚至是 Next！

#### **支持&社区**

Nuxt 可以说是最受欢迎的 Vue 框架。而且，在没有太多竞争的情况下，它在过去几年中被广泛采用。也就是说，它有一个非常非常大的 Vue 开发者社区，并且得到了积极的维护。所以，是的，它是稳定的和生产就绪！

### 值得吗？

所以，在快速浏览了上面的工具之后，你可能会开始思考它是否真的值得你花费时间和精力？

就像我前面说的，当选择使用上面的任何一个生成器(或者任何其他的生成器)时，你必须知道可能会有一个很大的学习曲线。这就是为什么基于你已经知道的工具(比如 Nuxt)的框架会给你带来优势——因为你对正在发生的事情知道得更多。

接下来，你有社区的方面。虽然对一些人来说这可能不是决定性的事情，但是使用一个有大量相关人员的项目，显然有一些优势。无论何时你需要帮助，你都有更大的机会找到它。此外，拥有更大社区的项目往往更稳定，更经得起未来考验。

最后，我们有开发工具。上面列出的所有生成器都有某种 CLI，甚至是浏览器扩展形式的真正的开发工具。这些较小的工具可以极大地提高您的生产力和开发体验。可以帮助你建立项目的初学者工具包和工具也非常有用。

一般来说，在选择工具时，有很多事情需要考虑。但是，如果你不想使用任何工具之类的东西，你总是可以使用 pure React、Vue 或 Angular，甚至更好——好的，旧的 HTML5、CSS 和 JS combo。

### 结论

在这篇文章中，我们已经看到了最好的静态网站生成器。没有什么神奇的方法可以找到最适合你的工具，除了试着使用它们。但是，尽管如此，我希望这篇文章至少提供了一点关于静态世界可以有多大和有多有趣的见解。

## [LogRocket](https://lp.logrocket.com/blg/nextjs-signup) :全面了解生产 Next.js 应用

调试下一个应用程序可能会很困难，尤其是当用户遇到难以重现的问题时。如果您对监视和跟踪状态、自动显示 JavaScript 错误、跟踪缓慢的网络请求和组件加载时间感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/nextjs-signup)

.

[![](img/f300c244a1a1cf916df8b4cb02bec6c6.png)](https://lp.logrocket.com/blg/nextjs-signup)[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/nextjs-signup)

LogRocket 就像是网络和移动应用的 DVR，记录下你的 Next.js 应用上发生的一切。您可以汇总并报告问题发生时应用程序的状态，而不是猜测问题发生的原因。LogRocket 还可以监控应用程序的性能，报告客户端 CPU 负载、客户端内存使用等指标。

LogRocket Redux 中间件包为您的用户会话增加了一层额外的可见性。LogRocket 记录 Redux 存储中的所有操作和状态。

让您调试 Next.js 应用的方式现代化— [开始免费监控](https://lp.logrocket.com/blg/nextjs-signup)。******