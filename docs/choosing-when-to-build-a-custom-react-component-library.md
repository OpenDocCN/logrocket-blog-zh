# 选择何时构建自定义 React 组件库

> 原文：<https://blog.logrocket.com/choosing-when-to-build-a-custom-react-component-library/>

React 组件库变得越来越流行，这是有原因的。它们是很好的工具，保证了数字产品内部和跨数字产品的 UI 元素的一致性和可重用性，同时具有显著减少开发时间和工作的潜力。

在本教程中，我们将概述构建自定义组件库的不同方法。但首先，我有一个问题要问你:你真的需要自己的组件库吗？先说定制一个的好处和取舍。

## 构建组件库的好处

### 代码内部的一致性

使用组件库的最大好处之一是，它保证了我们的组件在整个应用程序中外观和行为的一致性。UI 不一致会严重损害用户体验，并成为挫折感的来源。

### 复用性

根据定义，组件库中的组件是可重用的。这在 web 应用程序内部和之间都是如此。如果需要更改，我们只需在组件库中更改一次，然后就可以在任何地方反映出来。

### 易接近

确保我们的网络应用程序尽可能易于访问是非常重要的。通过使用组件库，可以(也应该)将基本的可访问性直接构建到我们的组件中。

### 程序调试时间

组件库可以节省大量时间。拥有一个现成的组件库意味着构建新特性的时间更少。此外，因为 UI 行为是预定义的，所以设计人员和开发人员之间的通信开销更少。

### 证明文件

组件库是创建有用且全面的 UI 组件文档的好方法。关于这方面的更多细节，请查看我关于[自记录 React 组件](https://blog.whereisthemouse.com/5-ways-to-create-a-self-documenting-react-component)的文章。

### 社区

一个构建良好的组件库可能对其他人有用。它可以开源并与 web dev 社区共享。

构建自定义组件库听起来很棒，不是吗？但是总有不好的一面。

## 构建自定义 React 组件库的权衡

### 资源

构建组件库需要涉及设计、产品和开发团队的大量跨职能的前期工作。这是一项巨大的事业，需要适当水平的资源和专业知识来获得所有的好处。

### 维护

维护定制组件库是一项持续的工作。它需要不断的关注以保持更新。包含一个开源社区会产生更多的开销。

### 设置

组件库的使用增加了项目设置的复杂性。通常，为了在项目间可重用，组件库独立于我们的应用程序，并作为一个依赖项安装。这意味着每次有变化时，组件库都需要重新发布，所有依赖的应用程序都需要更新。

### 用户化

组件库在设计上对我们的组件有很多限制。这通常是一件好事。但是，在某些情况下，定制需求是不可避免的。在组件 API 的灵活性和简单性之间取得恰当的平衡是极具挑战性的。

## 构建还是不构建自定义 React 组件库？

综上所述，似乎对于大多数项目来说，从头开始构建自己的组件库根本不值得花费时间和精力。好消息是，你可能不需要这么做！

有许多方法可以平衡创建组件库的好处和缺点，React 生态系统可以帮助您。

## 构建 React 组件库的方法

### 1.从头做起

顾名思义，使用这种方法，我们自己创建一切——从 UI 规则和模式到组件本身。我们使用 React，也许和我们最喜欢的 CSS-in-JS 库一起使用(如果你喜欢那种东西的话😉)，但其他的一切都要靠我们来构建。

**请说**

*   几乎无限自由地根据需要进行创建、构建和定制
*   对第三方的依赖最小
*   对源代码的完全控制

❌ **缺点:**

*   巨大的跨职能前期工作
*   持续维护成本

从头开始构建 React 组件库主要适用于复杂且有些特殊的数字产品，跨越多个 web 应用程序，并且有足够大的开发团队来承受成本。

### 2.使用顺风 CSS、样式化组件和样式化系统引入约束

构建我们自己的结构和约束当谈到我们的组件库的基本构件时——颜色、排版、间距、阴影等。—既困难又在大多数情况下不必要。伟大的解决方案已经存在！

在这种情况下，立即想到的是 [Tailwind CSS](https://tailwindcss.com/) 。它允许我们用一组有限的、基于约束的样式工具来构建复杂的 ui。开发人员很容易采用和熟悉 Tailwind，并且它还具有极强的可定制性和灵活性。

如果我们选择 CSS-in-JS 解决方案，特别是[样式组件](https://styled-components.com/)，引入一些有用约束的另一种方法是使用[样式系统](https://styled-system.com/)。有了它，我们可以用非常有用的方式构建我们的主题，并利用许多实用程序，同时在构建组件时保持重要的控制。

**请说**

*   根据需要自由创建、构建和定制
*   基于约束的基本构建块提供了结构

❌ **缺点:**

*   我们在组件库的核心引入了一个依赖——从它开始迁移可能是不可能的
*   维护成本(虽然有所降低)仍然是我们的责任

这种方法适合于具有 UI 组件的项目，这些组件需要很大的灵活性和自由来定制。在决定解决方案之前，确保引入的约束适合项目的特定用例，并且符合设计和开发需求。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

### 3.使用选定的组件，如 React 日期选择器、react-tippy 和 Reach UI

众所周知，有些 React 组件很难从头开始创建。设想一个 select 组件，它需要支持搜索、多选、创建自己的选项不存在、键盘控制和响应等特性。

一个带有日历、键盘控件、屏蔽输入和可访问的验证的日期选择器怎么样？一个动画的，可定制的，并且总是正确定位的工具提示怎么样？即使是广泛使用的组件，如 modal 和 tabs，从头开始创建也很有挑战性。

创建组件库时节省时间和精力的一个好方法是为这些更复杂的组件利用 React 的生态系统。回到上面的列表，这里有一些例子。

当涉及到选择组件时， [react-select](https://react-select.com/home) 提供了所有已经提到的特性以及更多开箱即用的特性。对于日期挑选者来说，也有一些很棒的选项，取决于用例，包括[反应日期挑选者](https://projects.wojtekmaj.pl/react-date-picker/)、[反应日期挑选者](https://www.npmjs.com/package/react-datepicker)，以及 AirBnB 的[反应日期](https://www.npmjs.com/package/react-dates)。

当谈到工具提示时，反应-提比已经涵盖了我们。最后， [Reach UI](https://reach.tech/) 是一个库的例子，它提供了真正可访问和可定制的组件，比如模态、选项卡、菜单等等。它们都可以单独安装，所以选择全部或部分添加到您的项目中。

另一方面，将第三方组件添加到您的库中会带来一些权衡。

**请说**

*   利用久经考验的用户界面节省时间和资源，同时降低我们自己代码库的复杂性
*   有选择地使用外部组件可以最小化依赖的影响
*   组件库的维护成本有所降低

❌ **缺点:**

*   对我们的依赖组件的行为和特性设置了重要的约束
*   引入了许多我们无法控制的第三方依赖项
*   外部组件有时很难完全定制

使用这种方法可以减轻组件库开发团队的负担。尽管如此，当决定集成第三方组件时，您必须考虑长期影响，并尽可能确保这些组件符合项目需求。

### 4.高度可定制的 React 组件库，如 Chakra UI

通常情况下，我们愿意为项目投资于组件库的时间和资源与实现上述方法所需的时间和资源并不相等。如果我们的项目范围有限，或者，即使是更大的项目，如果我们需要的 UI 组件是相对标准的，就可能出现这种情况。

我们仍然希望能够设计和定制我们的组件，但是我们很乐意依赖由外部基础组件库决定的敏感组件 API。幸运的是，在 React 生态系统中有几个选项可供选择。虽然 React 组件库的完整列表可以在其他地方找到，但我认为在大多数情况下， [Chakra UI](https://chakra-ui.com/) 是这种方法的合适选择。

**请说**

*   显著减少实现组件所需的时间和精力
*   组件样式很容易定制
*   减轻了大部分维护成本

❌ **缺点:**

*   我们完全依赖于我们选择的库，所以从它迁移出去可能是不可能的
*   随着时间的推移，存在项目需求偏离外部库中可用内容的风险

如果我们重视开发速度，同时确信未来的需求不会与我们的选择发生冲突，那么为我们的项目使用现有的组件库是一条可行之路。

### 5.现成的组件库

最后，我们可以用一个现成的组件库来构建我们的项目。这些解决方案虽然有些可定制性，但直接为我们提供了所需的一切，从颜色模式和主题到全面的组件集合。

一个最好的例子就是谷歌的[素材 UI](https://material-ui.com/) 。这个库非常广泛，如果你想用最少的设置开始构建你的 React 应用程序，它是完美的。因此，毫无疑问，在构建原型和概念证明时，材料 UI 是我的首选工具。在时间至关重要的面试任务中，这也很方便。

然而，将现实生活中的项目与固执己见的组件库紧密耦合也有明显的缺点。我们来分解一下。

**请说**

*   易于上手，意味着只需最少的前期工作
*   最低维护成本

❌ **缺点:**

*   定制从一开始就很难
*   大多数功能和样式都是预定义的。这可以使我们的用户界面感觉通用，与使用相同库的其他用户界面相似
*   设计师和开发人员都必须处理他们无法控制的约束和设计原则

## 结论

我们在构建(或不构建)自定义组件库时选择的方法是一个战略性决策，它将从头到尾影响我们的 React 项目。理解这一选择的含义并能够充分评估好处和权衡是至关重要的。我希望以上概述有助于我们更好地迎接挑战。

如果您觉得这篇文章很有用，请在 Twitter 上关注我，了解更多技术内容！

## [LogRocket](https://lp.logrocket.com/blg/react-signup-general) :全面了解您的生产 React 应用

调试 React 应用程序可能很困难，尤其是当用户遇到难以重现的问题时。如果您对监视和跟踪 Redux 状态、自动显示 JavaScript 错误以及跟踪缓慢的网络请求和组件加载时间感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/react-signup-general)

.

[![](img/f300c244a1a1cf916df8b4cb02bec6c6.png) ](https://lp.logrocket.com/blg/react-signup-general) [![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/react-signup-general) 

LogRocket 结合了会话回放、产品分析和错误跟踪，使软件团队能够创建理想的 web 和移动产品体验。这对你来说意味着什么？

LogRocket 不是猜测错误发生的原因，也不是要求用户提供截图和日志转储，而是让您回放问题，就像它们发生在您自己的浏览器中一样，以快速了解哪里出错了。

不再有嘈杂的警报。智能错误跟踪允许您对问题进行分类，然后从中学习。获得有影响的用户问题的通知，而不是误报。警报越少，有用的信号越多。

LogRocket Redux 中间件包为您的用户会话增加了一层额外的可见性。LogRocket 记录 Redux 存储中的所有操作和状态。

现代化您调试 React 应用的方式— [开始免费监控](https://lp.logrocket.com/blg/react-signup-general)。