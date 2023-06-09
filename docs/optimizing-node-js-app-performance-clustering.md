# 使用集群优化 Node.js 应用程序的性能

> 原文：<https://blog.logrocket.com/optimizing-node-js-app-performance-clustering/>

***编者按**:这篇文章于 2022 年 9 月 12 日更新，包含了 Node.js 中的集群是什么、Node.js 中集群的优势以及其他一般性更新和修订的信息。*

在过去的几年中，Node.js 获得了很大的人气。LinkedIn、易贝和网飞等知名企业都在使用它，这证明它已经经受住了考验。

集群是优化 Node.js 应用程序整体性能的好方法。在本教程中，我们将学习如何通过使用所有可用的 CPU，在 Node.js 中使用集群来实现这些性能优势。我们走吧。

## Node.js 中的集群是什么？

默认情况下，Node.js 不会利用所有的 CPU 内核，即使机器有多个。

另一方面，Node.js 附带了一个原生的[集群](https://nodejs.org/api/cluster.html)模块。集群模块支持创建子进程(工作进程),这些子进程在共享同一个服务器端口的同时运行。
每个子进程都有自己的事件循环、内存和 V8 实例。

子进程使用进程间通信与主父节点进行通信。

## Node.js 中对集群的需求

Node.js 的一个实例运行在一个线程上(你可以在 Node.js 的这里阅读更多关于[线程的内容)。官方](https://blog.logrocket.com/a-complete-guide-to-threads-in-node-js-4fa3898fe74f/) [Node.js“关于”页面](https://nodejs.org/en/about/)声明:“Node.js 被设计成没有线程并不意味着你不能在你的环境中利用多核。”这就是它指向集群模块的地方。

[集群模块 doc](https://nodejs.org/api/cluster.html) 补充道:“为了利用多核系统，用户有时会希望启动 Node.js 进程集群来处理负载。”因此，为了利用运行 Node.js 的系统上的多个处理器，我们应该使用集群模块。

利用可用的内核在它们之间分配负载可以提升 Node.js 应用程序的性能。因为大多数现代系统都有多个内核，所以我们应该使用 Node.js 中的集群模块来从这些新机器中获得最大的性能。

## Node.js 集群模块是如何工作的？

简而言之，Node.js 集群模块充当负载平衡器。它将负载分配给在共享端口上同时运行的子进程。Node.js 不擅长阻塞代码，所以如果只有一个处理器，并且它被一个繁重的 CPU 密集型操作阻塞，其他请求就在队列中等待这个操作完成。

对于多个进程，如果一个进程忙于相对 CPU 密集型的操作，其他进程可以接收其他传入的请求，并利用其他可用的 CPU/内核。这就是集群模块的强大之处——工作人员分担负载，应用程序不会停止。

主进程可以通过两种方式将负载分配给子进程。第一种(也是默认的)是循环方式。第二种方式是主进程监听套接字，并将工作发送给感兴趣的工人。然后，工人处理传入的请求。

然而，第二种方法不像基本的循环法那样非常清晰和容易理解。

## 在 Node.js 中使用集群的优势

在 Node.js 中使用集群有一些明显的优势。由于 Node.js 应用程序能够利用所有可用的 CPU 资源(鉴于目前大多数计算机都有多核 CPU)，它会将计算负载分配给这些核心。由于负载是分布式的，并且所有 CPU 内核都得到了充分利用，这将导致多个线程提高吞吐量(以每秒请求数衡量)

这是可能的，因为当多个进程准备好处理传入的请求时，这些请求可以被同时处理。即使有阻塞或长时间运行的任务，也只有一个工人受到影响，其他工人可以继续处理其他请求。在阻塞操作完成之前，Node.js 应用程序不会像以前那样暂停。

拥有多名员工的另一个优势是，应用程序可以在最短或完全不停机的情况下进行更新。由于有多个工作线程，因此可以一次回收/重启一个。这意味着一个子进程可以优雅地替换另一个子进程，并且不会有任何工作进程不活动的时候。如您所见，这很容易优化更新的速度和效率。

理论讲得够多了，让我们在深入研究代码之前先看看一些先决条件。

## 先决条件

要遵循关于 Node.js 集群的指南，您应该具备以下条件:

*   Node.js 运行在你的机器上，最新的 LTS 是可取的。写的时候是 Node.js 18。
*   Node.js 和 Express 的工作知识
*   关于进程和线程如何工作的基本知识
*   Git 和 GitHub 的工作知识

现在让我们进入本教程的代码。

## 构建不带集群的简单快速服务器

我们将从创建一个简单的 Express 服务器开始。该服务器将执行相对繁重的计算任务，这将故意阻塞[事件循环](https://blog.logrocket.com/a-complete-guide-to-the-node-js-event-loop/)。我们的第一个例子将没有任何聚类。

要在新项目中设置 Express，我们可以在 CLI 上运行以下命令:

```
mkdir nodejs-cluster
cd nodejs-cluster
npm init -y
npm install --save express

```

然后，我们将在项目的根目录下创建一个名为`no-cluster.js`的文件，如下所示:

![No Cluster Js File](img/7d693c777df7a9c1b539c66a5880eefa.png)

`no-cluster.js`文件的内容如下:

```
const express = require('express');
const port = 3001;

const app = express();
console.log(`Worker ${process.pid} started`);

app.get('/', (req, res) => {
  res.send('Hello World!');
})

app.get('/api/slow', function (req, res) {
  console.time('slowApi');
  const baseNumber = 7;
  let result = 0;    
  for (let i = Math.pow(baseNumber, 7); i >= 0; i--) {        
    result += Math.atan(i) * Math.tan(i);
  };
  console.timeEnd('slowApi');

  console.log(`Result number is ${result} - on process ${process.pid}`);
  res.send(`Result number is ${result}`);
});

app.listen(port, () => {
  console.log(`App listening on port ${port}`);
});

```

让我们看看代码在做什么。我们从一个简单的 Express 服务器开始，它将在端口`3001`上运行。它有两个 URIs ( `/`)显示`Hello World!`和另一个路径`/api/slow`。

慢速 API GET 方法有一个长循环，循环 77 次，也就是 823，543 次。在每个循环中，它执行一个`math.atan()`，或者一个数字的反正切(以弧度为单位)，以及一个`math.tan()`，一个数字的正切。它将这些数字添加到结果变量中。之后，它记录并返回这个数字作为响应。

是的，它被故意做得非常耗时和耗费处理器资源，以便稍后看到它对集群的影响。我们可以使用`node no-cluser.js`快速测试它，然后点击`[http://localhost:3001/api/slow](http://localhost:3001/api/slow)`，这将给出以下输出:

![Localhost Api Slow First Output](img/780ee538b6ab62b1eca85a3c81bfa079.png)

运行 Node.js 进程的 CLI 如下图所示:

![Slow Api First Results](img/958488fced17a4302cfb1c41fb9985fe.png)

如上所示，根据我们添加了`console.time`和`console.timeEnd`调用的分析，API 用了 37.432ms 完成了 823，543 个循环。

到目前为止的代码可以作为一个 [pull 请求](https://github.com/geshan/nodejs-cluster/pull/1)供您参考。接下来，我们将创建另一个服务器，它看起来很相似，但是其中有集群模块。

## 将 Node.js 集群添加到 Express 服务器

我们将添加一个类似于上面的`no-cluster.js`文件的`index.js`文件，但是在这个例子中它将使用集群模块。`index.js`文件的代码如下所示:

```
const express = require('express');
const port = 3000;
const cluster = require('node:cluster');
const totalCPUs = require('node:os').cpus().length;
const process = require('node:process');

if (cluster.isMaster) {
  console.log(`Number of CPUs is ${totalCPUs}`);
  console.log(`Master ${process.pid} is running`);

  // Fork workers.
  for (let i = 0; i < totalCPUs; i++) {
    cluster.fork();
  }

  cluster.on('exit', (worker, code, signal) => {
    console.log(`worker ${worker.process.pid} died`);
    console.log("Let's fork another worker!");
    cluster.fork();
  });

} else {
  startExpress();
}

function startExpress() {
  const app = express();
  console.log(`Worker ${process.pid} started`);

  app.get('/', (req, res) => {
    res.send('Hello World!');
  });

  app.get('/api/slow', function (req, res) {
    console.time('slowApi');
    const baseNumber = 7;
    let result = 0;    
    for (let i = Math.pow(baseNumber, 7); i >= 0; i--) {        
      result += Math.atan(i) * Math.tan(i);
    };
    console.timeEnd('slowApi');

    console.log(`Result number is ${result} - on process ${process.pid}`);
    res.send(`Result number is ${result}`);
  });

  app.listen(port, () => {
    console.log(`App listening on port ${port}`);
  });
}

```

让我们看看这段代码在做什么。我们首先需要`express`模块，然后我们需要`cluster`模块。之后，我们通过`require('os').cpus().length`得到可用的 CPU 数量。我在运行 Node.js 18 的 Macbook Pro 上是 8 次。

因此，我们检查集群是否是主集群。在几个`console.logs`之后，我们根据可用 CPU 的数量来分配工作线程。我们只要抓住一个工人的出口，我们记录并叉另一个。

如果不是主进程，就是子进程，在那里我们调用`startExpress`函数。该功能与前面示例中不带集群的 Express 服务器相同。

当我们用`node index.js`运行上面的`index.js`文件时，我们会看到下面的输出:

![Node Index Js Results](img/2fc31d0d08200101e100dd15404de6a1.png)

正如我们所看到的，所有八个 CPU 都有八个相关的工作器在运行，准备接受任何传入的请求。如果我们点击`[http://localhost:3000/api/slow](http://localhost:3000/api/slow)`，我们将看到以下输出，与之前非集群服务器的输出相同:

![Results With Clustering](img/643b886560c93e84b3fc82a7d2c3383b.png)

带有集群模块的服务器的代码在这个[拉请求](https://github.com/geshan/nodejs-cluster/pull/2)中。接下来，我们将对有集群和没有集群的 Express server 进行负载测试，以评估响应时间的差异以及它可以处理的每秒请求数(RPS)。

## 有集群和没有集群的负载测试服务器

为了在有集群和没有集群的情况下对 Node.js 服务器进行负载测试，我们将使用[贝吉塔负载测试](https://geshan.com.np/blog/2020/09/vegeta-load-testing-primer-with-examples/)工具。其他选项可以是 [loadtest](https://www.npmjs.com/package/loadtest) npm 包或者 [Apache benchmark](https://httpd.apache.org/docs/2.4/programs/ab.html) 工具。我发现贝吉塔更容易安装和使用，因为它是一个 Go 二进制文件，预编译的可执行文件可以无缝安装和开始使用。

在我们的机器上运行贝吉塔之后，我们可以运行以下命令来启动 Node.js 服务器，而不启用任何集群:

```
node no-cluster.js

```

在另一个 CLI 选项卡中，我们可以运行以下命令，使用贝吉塔发送 50 个 RPS，持续 30 秒:

```
echo "GET http://localhost:3001/api/slow" | vegeta attack -duration=30s -rate=50 | vegeta report --type=text

```

大约 30 秒后，它会产生如下输出。如果您在 Node.js 运行的情况下检查另一个选项卡，您将看到许多日志在流动:

![50 Rps For 30s With Vegeta](img/1e599f03246bfefd1225dc5a1baf9af2.png)

从上面的负载测试中可以快速了解一些情况。总共发送了 1，500 (50*30)个请求，服务器的最大良好响应为 27.04 RPS。最快的响应时间为 96.998μs，最慢的为 21.745s。同样，只有 1104 个请求返回了`200`响应代码，这意味着在没有集群模块的情况下成功率为 73.60%。

让我们停止该服务器，并使用集群模块运行另一个服务器:

```
node index.js

```

如果我们在 30 秒内运行 50 RPS 的相同测试，在第二台服务器上我们可以看到不同之处。我们可以通过运行以下命令来运行负载测试:

```
echo "GET http://localhost:3000/api/slow" | vegeta attack -duration=30s -rate=50 | vegeta report --type=text

```

30 秒后，输出将如下所示:

![50 Rps For 30s With Vegeta With Clustering](img/5eab0b9937de7bb1a21a4cb0cfc2a63c.png)

我们可以清楚地看到一个很大的区别，因为服务器可以利用所有可用的 CPU，而不是一个。所有 1500 个请求都成功了，并返回了一个`200`响应代码。最快的响应时间是 31.608 毫秒，最慢的只有 42.883 毫秒，而没有集群模块的响应时间是 21.745 秒。

吞吐量也是 50，所以这次服务器在 30 秒内处理 50 个 RPS 没有问题。由于有全部八个内核可供处理，它可以轻松处理比以前的 27 个 RPS 更高的负载。

如果您查看带有集群的 Node.js 服务器的 CLI 选项卡，它应该显示如下内容:

![50 Rps For 30s With Vegeta And Clustering Results](img/18a7b5cf4e943a87a1c69db49318e779.png)

这告诉我们，至少使用了两个处理器来处理请求。如果我们尝试使用 100 个 RPS，它会根据需要使用更多的 CPU 和进程。你当然可以用 30 秒 100 个 RPS 试试，看看效果如何。它在我的机器上以大约 102 RPS 达到最大值。

从没有集群的 27 个 RPS 到有集群的 102 个 RPS，集群模块的响应成功率提高了近四倍。这就是使用集群模块来使用所有可用 CPU 资源的优势。

## 关于 Node.js 集群的最终想法

如上所述，我们自己使用集群有利于提高性能。对于生产级系统来说，最好使用久经考验的软件，比如 PM2。它有内置的集群模式，并包括其他伟大的功能，如进程管理和日志。

类似地，对于在 kubernetes 上的容器中运行的生产级 Node.js 应用程序，资源管理部分可能由 kubernetes 更好地处理。即使使用 kubernetes，如果应用程序分配的 CPU 内核少于一个，有没有集群也没有什么区别。一般来说，由专用的负载平衡器来完成负载平衡比在应用程序中编写代码更好。

这些是您和您的软件工程团队需要做出的决定和权衡，以使 Node.js 应用程序在生产环境中运行时具有更好的可伸缩性、性能和弹性。

## 结论

在本文中，我们学习了如何利用 Node.js 集群模块来充分利用可用的 CPU 内核，从而提高 Node.js 应用程序的性能。

除此之外，集群还是 Node.js 武库中的另一个有用工具，可以获得更好的吞吐量并优化应用程序的性能。

## 200 只![](img/61167b9d027ca73ed5aaf59a9ec31267.png)显示器出现故障，生产中网络请求缓慢

部署基于节点的 web 应用程序或网站是容易的部分。确保您的节点实例继续为您的应用程序提供资源是事情变得更加困难的地方。如果您对确保对后端或第三方服务的请求成功感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/node-signup)

.

[![LogRocket Network Request Monitoring](img/cae72fd2a54c5f02a6398c4867894844.png)](https://lp.logrocket.com/blg/node-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/node-signup)

LogRocket 就像是网络和移动应用程序的 DVR，记录下用户与你的应用程序交互时发生的一切。您可以汇总并报告有问题的网络请求，以快速了解根本原因，而不是猜测问题发生的原因。

LogRocket 检测您的应用程序以记录基线性能计时，如页面加载时间、到达第一个字节的时间、慢速网络请求，还记录 Redux、NgRx 和 Vuex 操作/状态。

[Start monitoring for free](https://lp.logrocket.com/blg/node-signup)

.