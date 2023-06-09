# Node.js 与 Python:如何选择最好的技术来开发你的后端博客

> 原文：<https://blog.logrocket.com/node-js-vs-python-how-to-choose-the-best-technology-develop-backend/>

选择后端编程语言从来都不是一件容易的事情。毕竟，不同的语言各有利弊，您需要考虑这些因素，以确保它是您要构建的应用程序的正确工具。

Node.js 和 Python 是后端开发最受欢迎的选择。两者都有非常强大的包装生态系统和社区，在两者之间做出选择可能很困难。

在本文中，我们将分析 Node.js 和 Python 的优缺点，并了解其中一个优于另一个的场景，以便您可以为自己的后端做出最佳选择。

我们将涵盖以下主题:

## Node.js 是什么？

Node.js 是一个异步 JavaScript 运行时，运行在 Google 的 V8 引擎上。它通常用于构建实时应用程序、后端以及桌面和移动应用程序。

Node.js 是多范例的，支持以下范例:

*   事件驱动的
*   必要的
*   面向对象
*   函数式编程

Node 由 Ryan Dahl 开发，于 2009 年发布，因为它首次允许 JavaScript 开发人员在网络浏览器之外编写 JavaScript 代码，而一炮而红。多年来，它已经成长为 Python 等旧语言的有力竞争者，并提供了一系列后端开发工具，如 [Express.js](https://expressjs.com/en/5x/api.html) 、 [Fastify](https://www.fastify.io/) 和 [NestJS](https://nestjs.com/) 。

## Python 是什么？

Python 是一种解释性的通用编程语言，通常用于脚本编写、后端开发、机器学习和数据科学等。它支持多种范例，例如:

它是由吉多·范·罗苏姆设计和开发的，并于 1991 年发布，以主流的成功；Python 一直在 [TIOBE 编程社区指数](https://www.tiobe.com/tiobe-index/)中排名前 10。除此之外，谷歌、脸书、Dropbox 和 Instagram 等大公司都将其用于内部和外部工具——甚至 NASA 也发现了它的应用。

Python 在不断发展，它有成熟的 web 框架，如 [Django](https://www.djangoproject.com/) 、 [Flask](https://flask.palletsprojects.com/) 和 [FastAPI](https://fastapi.tiangolo.com/) ，你也可以在你的后端开发项目中使用它们。

## 比较架构

软件架构描述了系统中的主要组件是如何交互、关联和组织的。好的架构设计可以产生可伸缩、性能良好且可维护的系统。

在这一节中，我们将鸟瞰 Node.js 和 Python 架构。

### 节点. js

Node.js 是单线程、非阻塞的，实现了事件驱动的架构。它有一个单独的线程，你写的所有代码和你使用的库都在这个线程中执行。它还利用了 [libuv C 库](https://github.com/libuv/libuv)提供的其他线程来处理昂贵或长时间运行的任务。

Node.js 使用回调来通知长时间运行的任务的完成，一旦完成，它们将被添加到任务队列中，然后最终被添加回主线程。这种行为使得 Node.js 不阻塞，因为昂贵的任务不会阻塞主线程；相反，它们在单独的 libuv 线程中执行，Node.js 继续执行源代码的其他部分。

### 计算机编程语言

Python 也是一种单线程语言，很大程度上是因为它实现了一种[全局解释器锁(GIL)](https://wiki.python.org/moin/GlobalInterpreterLock) ，这种机制只允许一个线程在给定时间控制 Python 解释器并运行 Python 代码。即使 Python 程序使用多线程，GIL 也会定期在线程间切换，让每个线程都有机会执行代码——但默认情况下它们不能并行执行。这种行为使得 Python 成为单线程的。

与 Node.js 不同，Python 不是基于事件驱动的架构。然而，您仍然可以使用 [asyncio 包](https://docs.python.org/3/library/asyncio.html)来利用它，它允许您使用 async/await 语法编写异步代码，因为它实现了事件循环、未来等。

### 裁决

尽管这种语言的体系结构不同，但这两种语言都是不错的选择，并且都支持同步和异步编程。

## 并发性和并行性

选择后端的另一个重要方面是并发性和并行性。这些术语容易让人混淆，所以让我们来定义它们，这样我们就能达成共识:

*   **并发**:两个或多个任务在多线程中执行，但不是同时执行。相反，执行在任务之间切换，当计算机中断一个任务以切换到另一个任务时，它可以从中断点继续执行另一个任务
*   **并行性**:多个任务同时在不同的线程中执行

当您的应用任务受限于 CPU 时，并发性和并行性非常重要，例如在以下任务中:

如果你想看一些额外的 CPU 相关任务的例子，请看本文。

现在，如果您想要提高这些任务的性能，您可以将它们拆分到不同的线程中，并并行执行它们。

至此，让我们看看 Node 和 Python 是如何处理并发性和并行性的。

### 节点. js

即使 Node 是单线程的，你也可以使用 [`worker_threads`模块](https://nodejs.org/api/worker_threads.html)编写多线程程序。该模块创建轻量级线程工作器，允许您并行执行 CPU 密集型 JavaScript 代码。

工作线程与主线程(父线程)共享相同的内存和进程 ID，线程之间通过消息传递进行通信。你可以在博客的其他地方了解更多关于如何在 Node.js 中编写多线程程序的信息。

### 计算机编程语言

在 Python 中，您可以通过使用[线程模块](https://docs.python.org/3/library/threading.html)来实现并发，该模块创建线程来执行您的部分代码。然而，这并不意味着线程将并行执行。这是因为 GIL，它确保只有一个线程可以执行 Python 代码，并定期在它们之间切换。

虽然并发性对 I/O 相关的任务很有帮助，但是 CPU 相关的任务从并行性中获益匪浅。为了实现并行性，Python 提供了[多处理模块](https://docs.python.org/3/library/multiprocessing.html#module-multiprocessing)，它可以在每个内核上创建一个进程，并让您利用多核系统并行执行 Python 代码。

每个进程都有自己的解释器和 GIL，但也有一些注意事项。一方面，与工作线程相比，进程的通信是有限的，另一方面，启动一个进程往往比启动一个线程更昂贵。

### 裁决

Python 的线程模块与 Node.js `worker_thread`模块相比相形见绌，后者可以轻松实现并发和并行。Node.js 之所以胜出，是因为它支持并发性和并行性，而不像 Python 那样需要变通方法。

## 性能和速度

更快的后端可以减少服务器响应时间，从而提高页面速度。一个好的页面速度可以帮助你的 web 应用在 Google 上排名靠前，给你的用户一个好的体验。

编程语言的速度往往与源代码的执行方式相一致。让我们来探讨 Node.js 和 Python 在执行过程中的比较，以及它如何影响它们各自的执行速度。

### 节点. js

Node 以执行代码速度快而闻名，大部分原因可以归结为两个原因。

首先，如前所述，Node.js 编译成机器码，构建在 Google V8 引擎上，这是一个用 C++编写的高性能 JavaScript 引擎。V8 引擎将您的 JavaScript 编译成机器码，因此 CPU 会直接执行它，为您提供快速的性能。Node.js 还极大地受益于谷歌对 V8 引擎的频繁性能更新。

其次，Node.js 是非阻塞的，构建在事件驱动的架构之上。它对 Node.js 中几乎每个 I/O 方法操作都有异步方法，由于 Node.js 是单线程的，所以如果一个操作需要很长时间，它不会阻塞主线程。相反，它会并行执行它，为代码的其他部分留出执行空间。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

### 计算机编程语言

Python 的执行速度比 Node 慢很多。有几个因素会影响 Python 的速度。对于初学者来说，Python 自动将源代码编译成字节码，这是一种只有 Python 虚拟机(PVM)解释的低级格式。这对性能有影响，因为 CPU 不直接执行字节代码，而是由 PVM 解释代码，这会降低执行时间。

作为这个问题的解决方案，Python 有另外的实现，比如 [PyPy](https://www.pypy.org/) ，通过使用 [just-in-time (JIT)](https://en.wikipedia.org/wiki/Just-in-time_compilation) ，它声称比默认的 Python 实现快 4.5 倍。如果速度是您的 Python 应用程序迫切需要的，那么您应该考虑使用 PyPy。

话虽如此，尽管 Python 比 Node.js 慢，但它的速度对于很多项目来说仍然足够好，这也是它仍然受欢迎的原因。

### 裁决

Node.js 是赢家，因为它的执行速度与编译成机器代码一样快，而 Python 是用 PVM 解释的，这个过程往往会降低执行速度。

## 可量测性

当应用程序获得牵引力时，会发生以下情况:

*   由于用户数量增加，客户端请求增加
*   需要处理的数据量增加了
*   引入了新功能

应用程序在不损失性能的情况下随着需求的增长而增长和调整的能力称为可伸缩性。

### 节点. js

Node.js 提供了一个原生的[集群模块](https://nodejs.org/api/cluster.html),允许您轻松扩展应用程序。该模块在多核系统的每个内核上创建一个单独的进程或工作进程。每个 worker 都有一个应用程序实例，集群模块有一个内置的负载平衡器，它使用循环算法将传入的请求分配给所有 worker。

Node.js 的伸缩性也很好，因为它使用较少的线程来处理客户端请求。因此，它将大部分资源用于服务客户端，而不是处理可能非常昂贵的线程生命周期开销。

### 计算机编程语言

Python 没有 Node.js 的集群模块的本地等价物。最接近的是多处理模块，它可以在每个内核上创建进程，但它缺乏集群的一些功能。要进行集群计算，您可以使用第三方软件包，例如:

Python wiki 有一个 Python 集群计算包的完整列表。

### 裁决

与 Python 相比，Node.js 集群模块允许节点应用程序更容易扩展。然而，重要的是要承认，现在大多数人都在使用 Docker 进行缩放。

使用 Docker，您可以创建多个容器，每个容器包含一个应用程序实例。您可以创建与系统上可用的内核一样多的容器，并在每个容器中放置一个负载平衡器来分发请求。因此，无论您使用 Python 还是 Node.js，都可以使用 Docker 来简化缩放。

## 展开性

不是每一种编程语言都能有效地解决你遇到的每一个问题，有时你需要用另一种能胜任手头任务的语言来扩展一种编程语言。

我们来探讨一下 Node.js 和 Python 的扩展性。

### 节点. js

你可以用 C/C++通过使用[插件](https://nodejs.org/api/addons.html)来扩展 Node.js。例如，C++插件允许您编写一个 C++程序，然后使用`require`方法将其加载到 Node.js 程序中。有了这种能力，您就可以利用 C++库、速度或线程。

要实现插件，您可以使用:

也可以用 Rust 扩展 Node.js 查看[这篇教程](https://blog.logrocket.com/rust-and-node-js-a-match-made-in-heaven/)来学习如何做。

### 计算机编程语言

Python 也有很好的语言扩展能力。你可以用 C 或 C++来扩展它，这允许你在 Python 中调用 C/C++库，或者在 C/C++中调用 Python 代码。

您还可以使用其他 Python 实现来扩展 Python，如下所示:

*   Jython :使与 Java 的集成更加容易
*   [IronPython](https://ironpython.net/) :允许 Python 与微软的平滑集成。NET 框架

### 裁决

两者都很好地支持用其他语言扩展它们。

## 静态打字

Node.js 和 Python 都是动态类型语言，这使得您可以快速编程，而无需为您编写的代码定义类型。但是，随着代码库的增长，需要静态类型来帮助您尽早发现错误，并记录您的代码以供将来参考。尽管 Python 和 Node.js 是动态类型化的，但它们都提供了静态类型化工具，如果需要，您可以在代码库中利用这些工具。

### 节点. js

Node.js 作为 JavaScript 生态系统的一部分，拥有 [TypeScript](https://www.npmjs.com/package/typescript) ，这是微软在 2012 年开发的 JavaScript 的强类型超集。TypeScript 支持逐步键入，这意味着即使没有类型，您也可以使用 TypeScript，并在您认为合适的时候添加它们。

当您使用 TypeScript 时，您将源代码保存在一个`.ts`扩展名中，而不是保存在一个`.js`扩展名中，这涉及到一个将所有 TypeScript 文件编译成 JavaScript 的构建步骤。由于 TypeScript 是一种独立于 Node 的语言，所以它的发展速度要快得多，并且您可以使用所有较新的特性，因为它们总是被编译成 JavaScript。

TypeScript 近年来越来越受欢迎，客观地说，它在 npm 上每周有超过 2900 万次下载。根据 Stack Overflow 2021 开发者调查，它被列为第三大最受欢迎的编程语言，击败了 Python、Node.js 和 JavaScript 本身。要了解如何用 node 设置 TypeScript，请参见[这篇文章](https://github.com/nodejs/nan)。

### 计算机编程语言

与 Node.js 不同，Python 不需要单独的类型语言。相反，它附带了可以在项目中使用的[类型提示](https://docs.python.org/3/library/typing.html)。但是，Python 本身并不执行静态类型分析；相反，您可以使用类似于 [mypy](https://github.com/nodejs/nan) 的工具来进行静态类型检查。如果你想学习如何在 Python 中进行静态类型检查，请看[这篇文章](https://blog.logrocket.com/how-to-set-up-node-typescript-express/)。

Python 的类型提示方法的优点是，您不必为源代码使用不同的文件扩展名，并将其编译成 Python 文件扩展名。但是缺点是每一个新的 Python 版本都会引入新的类型提示，每一个都需要大约一年的时间。另一方面，TypeScript 的发布时间表是 3-4 个月。

### 裁决

Node.js 胜出是因为 TypeScript，比 Python 进化得快很多。但是，承认 Python 能够在不需要另一种语言的情况下添加类型也是件好事。

## 社区和图书馆

社区在软件开发中起着巨大的作用。拥有大型社区的编程语言往往具有:

*   更多用于开发的库和工具
*   更多学习内容
*   更容易找到的支持
*   更容易找到的招聘开发人员

Node.js 和 Python 同样拥有强大的社区，但是让我们更仔细地看看它们。

### 节点. js

Node.js 有一个强大而活跃的社区，已经构建了超过一百万个开源包，所有这些包都可以在 npm 上获得。

以下是您可能会遇到的一些软件包:

*   [Express](https://blog.logrocket.com/rust-and-node-js-a-match-made-in-heaven/) :构建 web 应用的 web 框架
*   [Axios](https://www.npmjs.com/package/axios) :用于发出 API 请求
*   Lodash :实用程序库

要发现更多的包，请查看 GitHub 上的管理过的 [awesome-nodejs](https://github.com/sindresorhus/awesome-nodejs) 库。

除了软件包之外，Node.js 还有大量高质量的书面内容和视频教程，它们遍布许多平台，包括这个博客。这使得学习 Node.js 变得更加容易，当你陷入一个任务时，很有可能有人已经在你之前在 Q &上问了这个问题，像 Stack Overflow 这样的平台。

此外，Node.js 也有很多国际会议，在那里你可以了解更多关于 Node.js 的信息并结识其他人，还有专注于 Node.js 的在线社区。

### 计算机编程语言

Python 也有一个活跃的社区，在 [Python 包索引](https://pypi.org/)上有超过 37 万个包和 340 万个版本。您可以使用 [pip](https://pypi.org/project/pip/) 将它们下载到您的项目中，这是一个从 Python 包索引中提取包的包安装程序。

以下是一些流行的软件包:

*   NumPy:一个使用数组的库
*   [熊猫](https://pypi.org/project/pandas/):用于分析数据
*   Django:一个网络框架

请参见[awesome-python](https://github.com/vinta/awesome-python)GitHub repo 获取完整列表。

像 Node.js 一样，Python 拥有大量与活跃的在线社区和会议相关的视频和书面内容，如在 40 多个国家举办的 [Python 大会(PyCon)](https://pycon.org/) 。

### 裁决

它们在这里都是赢家，因为 Node 和 Python 都有高质量的内容、活跃的社区和大量供开发使用的包。

## 常见使用案例

Python 和 Node.js 各有优缺点，我们在这里已经深入讨论过了。由于包和围绕它的社区，一些任务更适合 Python，而由于语言的架构和其他因素，一些任务更适合 Node.js。

### 节点. js

由于 Node.js 的非阻塞和事件驱动架构，它通常用于:

*   CPU 限制的操作:由于良好的多线程支持
*   I/O 操作:由于非阻塞和事件驱动的架构
*   实时应用:使用像 [socket.io](https://socket.io/) 这样的库

### 计算机编程语言

另一方面，科学界大量采用 Python，因此有许多用于机器学习、数据分析等的包，例如:

如果您的应用程序更侧重于数据分析或使用科学家使用的工具，Python 是一个极好的选择。

### 裁决

两个都好。这主要取决于你想用它们做什么。Node.js 适合实时应用，而 Python 适合需要数据分析和可视化的应用。

## 结论

我们已经到了这篇文章的结尾。我们已经研究了 Python 和 Node.js 之间的差异，我希望您已经了解到没有完美的工具。尽管如此，这些语言仍在努力修复它们的局限性，要么使用内置工具，要么使用第三方工具。

您对后端语言的选择在很大程度上取决于您想要构建的应用程序的类型，我希望这篇指南已经帮助您对后端做出了一个好的决定。

## 200 只![](img/61167b9d027ca73ed5aaf59a9ec31267.png)显示器出现故障，生产中网络请求缓慢

部署基于节点的 web 应用程序或网站是容易的部分。确保您的节点实例继续为您的应用程序提供资源是事情变得更加困难的地方。如果您对确保对后端或第三方服务的请求成功感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/node-signup)

.

[![LogRocket Network Request Monitoring](img/cae72fd2a54c5f02a6398c4867894844.png)](https://lp.logrocket.com/blg/node-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/node-signup)

LogRocket 就像是网络和移动应用程序的 DVR，记录下用户与你的应用程序交互时发生的一切。您可以汇总并报告有问题的网络请求，以快速了解根本原因，而不是猜测问题发生的原因。

LogRocket 检测您的应用程序以记录基线性能计时，如页面加载时间、到达第一个字节的时间、慢速网络请求，还记录 Redux、NgRx 和 Vuex 操作/状态。

[Start monitoring for free](https://lp.logrocket.com/blg/node-signup)

.