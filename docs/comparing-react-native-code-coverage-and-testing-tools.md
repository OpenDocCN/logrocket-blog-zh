# 比较 React 本机代码覆盖率和测试工具

> 原文：<https://blog.logrocket.com/comparing-react-native-code-coverage-and-testing-tools/>

React Native 是一个 JavaScript 框架，用于为 iOS 和 Android 编写本地呈现的移动应用程序。使用 React Native 可以显著减少构建移动应用程序所需的资源。

在本教程中，我们将回顾测试 React 本机应用的各种方法，并向您介绍 React 本机框架的一些有用的测试和代码覆盖工具。

我们将介绍一些自动化方法来帮助您更快地找到并解决 bug 和问题，从静态分析到端到端测试和测试覆盖报告。

## 什么是代码覆盖率？

代码覆盖率是一种衡量你的代码已经测试了多少的机制。代码覆盖率报告是一个重要的矩阵，用来衡量你的代码有多有效和写得多好。

覆盖率报告提供了关于测试实现的反馈，这使您能够验证代码库的质量和正确性。

## React 本地应用中的测试类型

在我们比较 React Native 的测试和代码覆盖工具之前，理解各种类型的测试范例之间的差异是很重要的:静态分析、单元测试、集成测试、组件测试和端到端(E2E)测试。

### 静态分析

提高代码质量的第一个方法是使用[静态分析](https://blog.logrocket.com/static-analysis-in-javascript-11-tools-to-help-you-catch-errors-before-users-do/)工具。

静态分析工具只是根据定义的规则解析和验证代码，并提供反馈，这样您就可以相应地调整代码。您可以为静态分析定义自己的规则，或者我们使用推荐的规则集。

用于验证 React 原生应用中代码的流行静态分析工具包括:

*   [ESLint](https://eslint.org/) ，一个强制某种代码风格的代码 linter。它分析代码并快速发现问题。您可以定义自己的规则集或扩展一些预定义的规则集
*   [更漂亮](https://blog.logrocket.com/automate-formatting-and-fixing-javascript-code-with-prettier-and-eslint/)，一个代码格式化工具。Pettier 确保了整个项目中一致的代码风格，并减少了团队成员提交冲突代码风格的可能性(例如，缩进、行长度、单引号或双引号等)。)到代码库
*   类型检查工具，如[流程和类型脚本](https://blog.logrocket.com/typescript-vs-flow/)

### 单元测试

单元测试覆盖了代码的最小部分，比如单独的函数和类。单元测试旨在确保应用程序的各个单元在隔离状态下按预期工作。

单元测试独立运行，易于编写和运行。当一个单元测试运行时，它会提供关于测试成功或失败的快速反馈，包括代码覆盖率。

### 集成测试

一旦您确定您的组件按预期工作，您应该验证它们是否与其他组件一起正常工作。在集成测试中，您将单元测试组合在一起进行测试，以确保它们按预期工作。

换句话说，集成测试同时评估应用程序的几个模块。

集成测试是 UI 测试中最重要的部分，它可以给你很大的信心，让你相信你的应用功能运行良好。

### 组件测试

组件测试是对单个 React 本地组件的测试，而不管它们在应用程序中的什么位置使用。

组件测试使您能够创建组件的一个实例，向它传递 props，与它交互，并观察它的行为。这可以更快、更容易地安排边缘案例，这些案例在端到端测试中设置起来会很慢、很困难。

### 端到端(E2E)测试

[端到端测试](https://blog.logrocket.com/end-to-end-testing-in-react-native-with-detox/)是在真实设备或模拟器上运行我们的应用程序，并像真实用户一样与之互动的实践。简单地说，这意味着一个系统正在检查你的应用程序。E2E 测试框架使你能够在应用程序的屏幕上找到并控制元素。

既然我们已经了解了测试代码的各种方法，那么让我们来关注一下在 React Native 中测试和生成代码覆盖报告的一些最流行和最有用的工具。

### 玩笑

Jest 是一个基于 JavaScript 的测试运行器，用于创建、运行和构建测试。您可以将 Jest 作为 npm 包安装在 React 本地项目中。

Jest 由脸书开发，内置于 React 中，这使得它成为测试 React 和 React 本地应用的默认选择。它还附带了默认测试设置`create-react-app`。除了 React 之外，Jest 与 Angular、Vue.js 和 TypeScript 配合得特别好。

Jest 内置了对代码覆盖的支持。这使它有别于其他顶级测试框架，其中许多框架需要您添加插件来生成代码覆盖报告。

您可以通过运行`npx jest --coverage`或`yarn --coverage`来运行 Jest 覆盖。要生成 HTML 或 JSON 格式的代码覆盖报告，您需要在`package.json`中启用`coverageReporters`:

```
"jest": {
     "collectCoverage": true,
     "coverageReporters": ["json", "html"],
 }

```

Jest 使您能够以最少的配置运行单元、集成和快照测试。Jest 测试是独立运行的，所以一个测试不会影响另一个测试的结果；所有测试并行运行。Jest 很容易与 CLI 工具集成。

Jest 对于 React Native 中的单元和集成测试特别有用。它很容易配置、运行和模仿。

Jest 的官方文档写得很好，并且是最新版本。它还享有广泛的社区支持。

### 戒瘾诊所

[Detox](https://github.com/wix/Detox) 是一个灰盒端到端测试和自动化库，使您能够在真实设备上模拟应用程序行为。它完全是为了支持 React 本地项目以及纯本地项目而构建的。Detox 还支持跨平台设备测试(真实设备和模拟器)。

您可以将 Detox 作为一个独立的测试运行程序，或者与另一个 JavaScript 测试运行程序(如 Jest、Mocha 或艾娃)一起使用。专为 CI 工具设计，在 Travis、Jenkins 等 CI 平台上很容易执行[排毒 E2E 测试](https://blog.logrocket.com/end-to-end-testing-in-react-native-with-detox/)。Detox 可在设备上运行，但 iOS 尚不支持。

开发人员选择 Detox 是因为在真实设备或模拟器上测试他们的应用程序可以让他们看到真实用户眼中的样子。这是一个很好的方法，可以让你在发布之前确信你的应用程序正在如你所愿地运行。

Detox 是 Wix 专门为 React Native 中的自动化测试开发的。这是一个以 JavaScript 为中心的测试工具，是 React 本地开发者的理想选择。因此，Detox 自动化测试与 Jest 单元测试配合得很好。

### Appium

Appium 是一个移动应用测试框架，支持基于 React 的现成应用。

如果 Detox 是一个灰箱测试框架，我们可以把 Appium 看作是黑箱版本。使用 React Native 时，Appium makes 很容易为 Android 和 iOS 平台编写测试用例。底层 web 驱动程序 Selenium 作为 Appium 和移动平台之间的桥梁来部署和执行测试。

Appium 用于 web 和移动应用的统一测试。它支持高级测试自动化，并具有完整的工具链集成。所有的测试结果都在一个地方，包括测试报告和分析。

Appium 比 Detox 有更高的剥落率。因为 Detox 是以 JavaScript 为中心的，所以在大多数情况下，对于希望对他们的 React 原生应用进行自动化测试的 JavaScript 开发人员来说，它是比 Appium 更合适的选择。也就是说，Appium 对于 React 本地开发人员的一个卖点是，你可以将它与任何其他测试框架结合使用。

### 茉莉

Jasmine 是一个为测试 JavaScript 代码而设计的测试框架。它不依赖于浏览器或 DOM。Jasmine 提供了一个小语法，使您能够测试整个应用程序的小单元，而不是将其作为一个整体来测试。

Jasmine 不依赖于任何其他 JavaScript 框架。Jasmine 框架中使用的所有语法都是清晰明了的。Jasmine 是一个开源框架，有多种形式，包括独立版本和 Ruby Gem、Node.js 等版本。

Jasmine 对于 React 本地开发人员来说是一个测试和评估代码覆盖率的很好的工具，因为它很快并且没有外部依赖性。它是专门为测试 JavaScript 代码而设计的，可以单独使用，也可以与 Karma 等其他库一起使用。此外，Jasmine 专注于单元测试，并不排斥行为驱动开发。

### 因果报应

Karma 是另一个基于 JavaScript 的应用程序测试工具。它是一个基于节点的工具，通过 npm 安装。

Karma 基于插件，这些插件使您能够用其他断言和测试框架编写测试。这使得快速配置和执行测试变得容易。测试工具运行在实际的浏览器环境中。

Karma 使用 [CodeCov](https://about.codecov.io/) 和[伊斯坦布尔](https://istanbul.js.org/)生成覆盖报告。在运行测试时，它将在一个新的覆盖率文件夹中生成测试覆盖率。您可以通过从覆盖率文件夹中打开`index.html`文件来访问覆盖率报告。

Karma 是一个测试运行器，所以对于测试和断言，我们可以使用 Jasmine、Mocha 和 QUnit。它在浏览器和真实设备上运行测试。由于是测试运行程序，Karma 需要插件来执行测试。它易于配置和设置。

## 摘要

代码覆盖率报告帮助您评估您的测试实现。在代码覆盖报告的帮助下，您可以轻松地找出未使用的代码和死代码，以改善开发人员的体验和应用程序的性能。此外，测试和代码覆盖率报告使您能够维护和跟踪应用程序的健康状况。

## [LogRocket](https://lp.logrocket.com/blg/react-native-signup) :即时重现 React 原生应用中的问题。

[![](img/110055665562c1e02069b3698e6cc671.png)](https://lp.logrocket.com/blg/react-native-signup)

[LogRocket](https://lp.logrocket.com/blg/react-native-signup) 是一款 React 原生监控解决方案，可帮助您即时重现问题、确定 bug 的优先级并了解 React 原生应用的性能。

LogRocket 还可以向你展示用户是如何与你的应用程序互动的，从而帮助你提高转化率和产品使用率。LogRocket 的产品分析功能揭示了用户不完成特定流程或不采用新功能的原因。

开始主动监控您的 React 原生应用— [免费试用 LogRocket】。](https://lp.logrocket.com/blg/react-native-signup)