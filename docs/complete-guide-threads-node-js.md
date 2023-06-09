# Node.js 中的线程完整指南

> 原文：<https://blog.logrocket.com/complete-guide-threads-node-js/>

***编者按**:本文最后更新于 2023 年 2 月 10 日。查看 Node.js 中的[多线程指南了解更多信息。](https://blog.logrocket.com/node-js-multithreading-worker-threads-why-they-matter/)*

许多人想知道单线程 Node.js 后端如何与多线程后端竞争。考虑到 Node 被认为是单线程的，那么多大公司选择 Node 作为他们的后端，这似乎是违反直觉的。要知道为什么，我们必须理解当我们说节点是单线程的时候，我们真正的意思是什么。

JavaScript 被创造出来只是为了在网络上做一些简单的事情，比如验证一个表单，或者创建一个彩虹色的鼠标轨迹。直到 2009 年，Node 的创始人 Ryan Dahl 才让开发者有可能使用该语言编写后端代码。

后端语言通常支持多线程，有各种机制在线程和其他面向线程的特性之间同步值。要在 JavaScript 中增加对这些东西的支持，需要改变整个语言，这并不是 Dahl 的真正目标。为了让普通 JavaScript 支持多线程，他必须创建一个变通方法。让我们探索一下…

*向前跳转:*

## Node.js 的实际工作原理

Node.js 遵循单线程事件循环范式。要了解 Node 的完整工作原理，重要的是要了解 Node 中的线程是什么，组成节点的事件循环，并通过了解它是单线程还是多线程来了解节点的基本架构。

### Node.js 中的线程

Node.js 中的线程是单个进程中单独的执行上下文。它是一个轻量级的独立处理单元，可以与同一进程中的其他线程并行运行。它驻留在进程内存中，并且有一个执行指针。它有自己的堆栈，但有一个共享的进程堆。

Node.js 使用两种线程:一个由事件循环处理的主线程和工作池中的几个辅助线程。在 Node.js 的上下文中，辅助线程或线程可互换地用于工作线程。

在 Node.js 中，主线程是 Node.js 启动时启动的初始执行线程。它负责执行 JavaScript 代码和处理传入的请求。工作线程是一个独立的执行线程，与主线程并行运行。

### Node.js 是多线程的还是单*–*线程的？

单线程意味着一个程序只有一个执行线程，这使得它在给定时间只能执行一个任务。同时，术语“多线程”意味着一个程序有多个执行线程，这允许它同时执行多个任务。

每个线程独立运行，任务分配由操作系统处理。这两种方法都有各自的挑战。在单线程进程中，所有任务都按顺序执行，阻塞操作会延迟其他任务的执行。同时，在多线程进程中，出现的难点是多线程之间的同步和协调。

理解了这两个术语，我们现在可以回答这个问题了。

Node.js 是单线程的，因为它有一个处理 JavaScript 操作和所有 I/O 的主事件循环。但是，Node.js 为我们提供了额外的功能，如果使用得当，可以提供多线程所具有的优势。要详细了解是什么赋予了 Node 这种能力，以及如何应对这种方法带来的挑战，请查看[这篇文章](https://blog.logrocket.com/node-js-multithreading-worker-threads-why-they-matter/)。

单线程节点架构中的主要元素是事件循环，这使得节点如此强大，尽管是单线程运行时，但它正成为大多数后端开发人员的首选。我们之前解释过，一个节点中有两种线程。主线程使用事件循环。

事件循环是一种机制，它接受回调(函数)并注册它们以便在将来的某个时刻执行。它与正确的 JavaScript 代码在同一个线程中运行。当 JavaScript 操作阻塞线程时，事件循环也被阻塞。

工作池是一个执行模型，它产生并处理单独的线程，然后这些线程同步执行任务并将结果返回给事件循环。然后，事件循环用所述结果执行所提供的回调。

简而言之，它负责异步 I/O 操作——主要是与系统磁盘和网络的交互。主要由`fs` (I/O 重)或`crypto` (CPU 重)等模块使用。Worker pool 是在 [libuv](http://docs.libuv.org/en/v1.x/) 中实现的，这导致每当 Node 需要在 JavaScript 和 C++之间进行内部通信时都会有轻微的延迟，但这很难被注意到。

有了这两种机制，我们能够编写这样的代码:

```
fs.readFile(path.join(__dirname, './package.json'), (err, content) => {
 if (err) {
   return null;
 }
 console.log(content.toString());
});

```

前面提到的`fs`模块告诉工作池使用它的一个线程来读取文件的内容，并在完成后通知事件循环。然后，事件循环获取提供的回调函数，并用文件的内容执行它。

以上是非阻塞代码的例子；因此，我们不必同步等待某事发生。我们告诉工作池读取文件，并使用结果调用提供的函数。由于工作池有自己的线程，因此在读取文件时，事件循环可以继续正常执行。

这一切都很好，直到需要同步执行一些复杂的操作:任何运行时间过长的函数都会阻塞线程。如果一个应用程序有许多这样的功能，它可能会显著降低服务器的吞吐量或者完全冻结它。在这种情况下，没有办法将工作委托给工人池。

需要复杂计算的领域-如人工智能、机器学习或大数据-无法真正有效地使用 Node.js，因为这些操作会阻塞主(也是唯一的)线程，使服务器无响应。在 Node.js v10.5.0 出现之前一直是这样，它增加了对多线程的支持。

## 介绍`worker_threads`

`worker_threads`模块是一个允许我们创建全功能多线程 Node.js 应用程序的包。

线程工作器是在一个单独的线程中产生的一段代码(通常取自一个文件)。

请注意，术语线程工作器、工作器和线程经常互换使用；它们指的都是同一个东西。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

要开始使用线程工作器，我们必须导入`worker_threads`模块。让我们首先创建一个函数来帮助我们生成这些线程工作器，然后我们将稍微讨论一下它们的属性:

```
type WorkerCallback = (err: any, result?: any) => any;
export function runWorker(path: string, cb: WorkerCallback, workerData: object | null = null) {
 const worker = new Worker(path, { workerData });
 worker.on('message', cb.bind(null, null));
 worker.on('error', cb);
 worker.on('exit', (exitCode) => {
   if (exitCode === 0) {
     return null;
   }
   return cb(new Error(`Worker has stopped with code ${exitCode}`));
 });
 return worker;
}

```

要创建一个 worker，我们必须创建一个`Worker`类的实例。在第一个参数中，我们提供了包含工作者代码的文件的路径；在第二个例子中，我们提供了一个包含名为`workerData`的属性的对象。这是我们希望线程在开始运行时能够访问的数据。

请注意，无论您使用 JavaScript 本身还是将文件转换成 JavaScript 的东西(例如，TypeScript)，路径应该总是指向扩展名为`.js`或`.mjs`的文件。

我还想指出为什么我们使用回调方法，而不是返回一个在触发`message`事件时解决的承诺。这是因为工人可以调度许多`message`事件，而不仅仅是一个。

正如您在上面的例子中所看到的，线程之间的通信是基于事件的，这意味着我们正在设置一旦 worker 发送给定事件就调用的侦听器。

以下是最常见的事件:

```
worker.on('error', (error) => {});

```

每当 worker 内部出现未捕获的异常时，就会发出`error`事件。然后，该工作线程被终止，该错误可作为所提供的回调中的第一个参数:

```
worker.on('exit', (exitCode) => {});

```

每当工人离开时发出`exit`。如果`process.exit()`在 worker 内部被调用，`exitCode`将被提供给回调函数。如果工人以`worker.terminate()`终止，代码将为`1`:

```
worker.on('online', () => {});

```

每当工作人员停止解析 JavaScript 代码并开始执行时，就会发出`online`。它不常用，但在特定情况下可以提供信息:

```
worker.on('message', (data) => {});

```

每当工作线程向父线程发送数据时，就会发出`message`。

现在让我们看看数据是如何在线程间共享的。

## 使用工人的两种方式

有两种方法可以实现工作线程并获得工作线程提供的好处。

第一种方法是生成 worker，执行它的代码，并将结果发送给父级。使用这种方法，我们每次都必须从头开始设置一个新的工人。

在创建工作线程、启动线程、创建新的工作线程头的内存开销以及管理每个线程所需的额外资源时，需要大量的开销成本。虽然使用第一种方法可以实现任务，但这不是一种有效的方法——尤其是在实现基于大规模节点的系统时。为了解决这种方法带来的棘手问题，还有另一种方法，这也是标准的行业实践。

第二种方法是实现一个工人池。工作线程池通过创建一个工作线程工具来解决第一种方法的痛点，该工具可以在多个任务中重用。我们可以创建一个池并将任务分配给其中的工作线程，而不是每次都创建一个工作线程。

从技术角度来说，工作线程池可以被视为管理工作线程池的抽象数据类型。池中的每个工作线程都被分配了一个任务，该线程与其他线程并行运行该任务。

分配任务有多种方式。工作池还充当管理器，它将任务分配给工作线程，从它们那里收集结果，并支持工作池中的线程之间的通信。

工人池可以通过使用不同的数据结构和算法来实现，例如任务队列和消息传递系统。使用特定数据结构的选择取决于需求，即所需的工作线程数量、任务的确切性质以及线程之间需要多少通信。

### 实施工人池

在 Node 中，可以通过使用内置功能或使用第三方工具来实现工作池。节点中内置的`worker-threads`模块提供了对工作线程的支持，可以用来构建工作线程池。有几个库也可以用来补充工人池。

这些库为工作线程提供了高级 API，还提供了额外的支持，如自动调度任务和线程管理。为了让大家了解 worker pool 是如何实现的，下面是一个示例代码，它使用了 Node 的内置`worker-threads`特性:

```
const { Worker, isMainThread, parentPort } = require('worker_threads');

if (isMainThread) {
  // Main thread code
  // Create an array to store worker threads
  const workerThreads = [];
  // Create a number of worker threads and add them to the array
  for (let i = 0; i < 4; i++) {
    workerThreads.push(new Worker(__filename));
  }
  // Send a message to each worker thread with a task to perform
  workerThreads.forEach((worker, index) => {
    worker.postMessage({ task: index });
  });
} else {
  // Worker thread code
  // Listen for messages from the main thread
  parentPort.on('message', message => {
    console.log(`Worker ${process.pid}: Received task ${message.task}`);
    // Perform the task
    performTask(message.task);
  });
  function performTask(task) {
    // … operations to be performed to execute the task
  }
}

```

代码分为两部分，上面一部分是主线程，另一部分是工作线程。首先，我们从模块中导入必要的成员，然后，如果当前执行上下文在主线程中，我们将创建一个数组来存储四个工人。创建每个工作线程后，这段代码向每个工作线程发送一条新消息，其中包含要执行的任务。

在工作线程部分，我们通过使用`parentPort`属性的`on`方法监听来自主线程的消息。收到消息后，它将进程 id 与任务一起记录下来，并将其传递给执行任务的函数，该函数将对任务应用适当的方法。

## 使用线程的主要好处是什么？

本质上，线程是一种有价值的工具，可以显著影响程序的性能、响应能力和整体效率。当有效利用时，它们可以对程序的结果产生很大的影响，并帮助它跟上用户需求的步伐。

Node.js 中的线程对于开发者来说是一个强大的工具。它允许他们将一个进程分割成多个完全自主的执行流。如果正确使用，线程可以通过提高程序的速度、效率和响应能力来提高程序的质量。

线程的一些主要优势是:

1.  **性能提升**:线程促进了多个程序的并发运行，从而加快了整个程序的执行速度，而不是只运行一个任务
2.  **响应性**:如果一个任务计算量很大，它会阻塞或延迟其余操作的执行，从而延迟整个程序的执行。借助线程技术，如果一项计算繁重的任务耗费时间并延迟了一个线程的响应，它不会影响程序的响应，因为其他线程可以继续处理用户输入和其他任务
3.  **资源共享**:在 Node.js 中，由于进程级全局范围和进程间通信，多线程可以共享资源。资源共享有助于多线程访问和修改共享数据，即变量，从而允许并发处理，这导致程序的更快执行
4.  **易于编程**:随着线程化的引入，程序员不用担心 Node.js 单线程架构的局限性，使得编程变得高效且可扩展
5.  **改进的可伸缩性**:伸缩线程既简单又高效，因此可以更容易地构建高性能、可伸缩的 Node.js 应用程序，这些应用程序可以毫无困难地处理增加的负载

## 结论

为我们的应用添加多线程支持提供了一种相当简单的方法。通过将繁重的 CPU 计算委派给其他线程，我们可以显著提高服务器的吞吐量。有了官方线程的支持，我们可以期待更多来自人工智能、机器学习和大数据等领域的开发人员和工程师开始使用 Node.js。

## 200 只![](img/61167b9d027ca73ed5aaf59a9ec31267.png)显示器出现故障，生产中网络请求缓慢

部署基于节点的 web 应用程序或网站是容易的部分。确保您的节点实例继续为您的应用程序提供资源是事情变得更加困难的地方。如果您对确保对后端或第三方服务的请求成功感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/node-signup)

.

[![LogRocket Network Request Monitoring](img/cae72fd2a54c5f02a6398c4867894844.png)](https://lp.logrocket.com/blg/node-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/node-signup)

LogRocket 就像是网络和移动应用程序的 DVR，记录下用户与你的应用程序交互时发生的一切。您可以汇总并报告有问题的网络请求，以快速了解根本原因，而不是猜测问题发生的原因。

LogRocket 检测您的应用程序以记录基线性能计时，如页面加载时间、到达第一个字节的时间、慢速网络请求，还记录 Redux、NgRx 和 Vuex 操作/状态。

[Start monitoring for free](https://lp.logrocket.com/blg/node-signup)

.