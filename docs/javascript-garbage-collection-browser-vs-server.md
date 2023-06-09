# JavaScript 垃圾收集:浏览器与服务器

> 原文：<https://blog.logrocket.com/javascript-garbage-collection-browser-vs-server/>

我们都知道垃圾收集(GC)对现代应用程序开发有多重要。根据您的编程语言，您可能会自己完成这项工作，比如在 c 中。在其他语言中，它是如此的隐蔽，以至于许多开发人员几乎不知道它是如何完成的。

无论如何，[垃圾收集](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science))总是释放不再使用的内存。实现这一点的策略和算法因语言而异。例如，JavaScript 采用了一些有趣的路径，这取决于您是在浏览器还是 Node.js 服务器上。

但是你有没有考虑过这个过程是如何在幕后运作的？让我们花点时间来理解 JavaScript GC 是如何在浏览器和服务器上施展魔法的。

## 记忆的循环

我们需要 GC 的原因是因为在编程时会分配大量内存。你创建函数、对象等等。，所有这些都占用空间。

例如，与 C 语言相比，JavaScript 的最大优势在于它会自动为您分配内存。这个过程非常简单，只需要三个明确定义的步骤:

![JavaScript Memory Lifecycle Visual](img/e9e98cb74cc6afafb1e23e57cf7cc388.png)

JavaScript’s memory life cycle.

没错，但是 JavaScript 到底在哪里存储这些数据呢？JavaScript 将数据发送到两个目的地:第一个是内存堆，第二个是堆栈。

堆是另一个每个人都听说过的术语。它负责我们所说的动态内存分配。换句话说，这个空间是为 JavaScript 保留的，以便在需要时存储对象和函数等资源，而不会限制它可以使用的内存量。

这与 stack 有一点不同，stack 是一种数据结构，用于按字面意义堆叠元素，如原始数据和指向真实对象的引用。堆栈分配策略“更安全”,因为它知道分配了多少内存，因为它是固定的。

了解这些限制也因供应商而异是很重要的，所以在使用大量内存时要注意这一点。

以下面的代码清单为例:

```
// heap and stack
const task = {
  name: 'Laundry',
  description: 'Call Mary to go with you...',
};

// stack
let name = 'Walk the dogs'; // 1
name = 'Walk; Feed the dogs'; // 2
const firstTask = name.slice(0, 5); // 3

```

每次在 JavaScript 中创建新对象时，堆内存中的空间都是专门为它准备的。然而，它的内部值是原语，这意味着它们将被堆叠在堆栈中。这同样适用于`task`参考。

当涉及到使用不可变值(如 JavaScript 中的原语)等特殊情况时，该语言总是倾向于新的分配，而不是使用以前的内存槽。

以下是对上述代码示例中第 1–3 点注释的解释:

1.  简单地用一个字符串值创建一个新的原始变量
2.  用新值覆盖它的值。发生这种情况时，JavaScript 会在堆栈上分配一个新的位置，而不是用当前值替换它
3.  不管你这样做多少次，不管是通过直接赋值还是方法的结果，JavaScript 总是会这样做

## JavaScript 的垃圾收集算法

很好，现在我们知道了 JavaScript 如何处理内存分配，以及分配后内存的去向。但是它是如何释放事物的呢？

JavaScript 的垃圾收集器会处理它，这个过程听起来很简单:一旦一个对象不再被使用，垃圾收集器就会释放它的内存。

关于这一点不那么简单的是 JavaScript 如何知道哪些对象容易被收集。这就是算法进入场景的地方。

### 引用计数 GC

顾名思义，这种策略遍历内存中分配的资源，并搜索那些没有引用指向它们的资源。

让我们以前面的代码片段作为参考，以便更好地理解:

```
const task = {
  name: 'Laundry',
  description: 'Call Mary to go with you...',
};

task = 'Walk the dogs';

```

所以最初，`task`对象持有一堆内部属性。那么让我们假设另一个开发人员决定一个任务可以简单地表示为一个原语本身。所以现在，第一个任务对象不再有指向它的引用，这使得它可用于 GC。

等等，那不可能这么简单……的确，听起来很幼稚！的确如此。

然而，有一种特殊的边缘情况你必须知道:循环依赖。您可能以前从未想到过它们，因为 JavaScript 也知道如何处理它们。但通常情况下，它们是这样发生的:

```
function task(n, d) {
    // ...

    reporter = { ... };
    assignee = { ... };

    reporter.assignee = assignee;
    assignee.reporter = reporter;
};

myTask = task('Laundry', 'Call Mary to go with you...');

```

这可能不代表现实应用程序中的功能任务，但足以想象两个对象的内部属性相互引用的情况。

这就形成了一个循环。一旦函数完成，JavaScript 的引用计数 GC 将无法解释这两个对象可以被收集，因为它们仍然相互引用。

这是一个常见的场景，很容易导致现实应用中的[内存泄漏](https://blog.logrocket.com/understanding-memory-leaks-node-js-apps/)。为了避免这种情况，JavaScript 在战场上为我们提供了第二种策略。

### 标记和扫描算法

标记-清除算法因被许多编程语言用于垃圾收集而闻名。简而言之，它利用一种巧妙的方法来确定一个给定的对象是否可以从根对象到达。

在 JavaScript 中，如果您在 Node.js 应用程序上，根对象是`global`对象；如果你在浏览器上，就是`window`。

该算法从顶部开始，一次又一次地沿着层次结构向下，标记从根可以到达(即，仍然被引用)的每个对象，并且扫描不能到达的对象。

您现在可以看到 GC 将如何收集前面例子中的`reporter`和`assignee`了吗？

## Node.js 呢？

Node(以及 Chrome)由谷歌的开源 JavaScript 引擎 V8 驱动。重要的注释发生在节点的堆内存中。

让我们来看看下面的表示:

![Node New Space Old Space Comparison](img/203b35eb5ff1e7e873672467de4e46b5.png)

New space vs. old space.

节点的堆分为两个主要部分:新空间和旧空间。顾名思义，前者是新对象(称为年轻一代)被分配的地方，而后者是已经存活了很长时间的对象(老一代)的目的地。

因此，新空间中对象的垃圾收集比旧空间中的要快。平均来说，年轻一代中有多达 20%的对象存活下来，足以进入老一代。

由于所有这些特性，V8 使用了一个额外的 GC 策略:清道夫。

### 清道夫

正如我们所见，Node 在旧空间中释放东西的成本更高。当它必须这样做时，标记和清除算法运行以实现目标。

清道夫垃圾收集器专门收集年轻一代的垃圾。它的策略包括选择幸存的对象并将它们移动到所谓的新页面。为了实现这一步，V8 确保至少一半的年轻一代保持空闲；否则，它将面临内存不足的问题。

这个想法是跟踪年轻一代的所有引用，而不需要遍历整个老一代。此外，清道夫还保留一组旧空间的引用，这些引用指向新空间中的对象。

然后，这个过程将幸存的对象一个接一个地移动到新页面，直到整个 GC 完成。最后，它更新被移动的原始对象的指针。

## 结论

当然，这只是 JavaScript 世界中 GC 策略的概述。这个过程要复杂得多，值得进一步研究。我强烈推荐著名的 Mozilla GC[docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management)和 V8 的[talk](https://v8.dev/blog/trash-talk)将 Orinoco 垃圾收集器作为补充资源。

重要的是要记住，和许多其他语言一样，我们不能确定 GC 什么时候运行。从 2019 年开始，由 GC 来执行不定期的清理，不能自己触发。

除此之外，您的编码方式对 JavaScript 将分配多少内存有很大影响。这就是为什么了解垃圾收集器内存分配的特性以及释放内存的策略非常重要。有几个开源 lint 和提示工具可以帮助您识别和分析这些漏洞，以及代码中的其他陷阱。去找他们！

## 您是否添加了新的 JS 库来提高性能或构建新特性？如果他们反其道而行之呢？

毫无疑问，前端变得越来越复杂。当您向应用程序添加新的 JavaScript 库和其他依赖项时，您将需要更多的可见性，以确保您的用户不会遇到未知的问题。

LogRocket 是一个前端应用程序监控解决方案，可以让您回放 JavaScript 错误，就像它们发生在您自己的浏览器中一样，这样您就可以更有效地对错误做出反应。

[![LogRocket Dashboard Free Trial Banner](img/e8a0ab42befa3b3b1ae08c1439527dc6.png)](https://lp.logrocket.com/blg/javascript-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/javascript-signup)

[LogRocket](https://lp.logrocket.com/blg/javascript-signup) 可以与任何应用程序完美配合，不管是什么框架，并且有插件可以记录来自 Redux、Vuex 和@ngrx/store 的额外上下文。您可以汇总并报告问题发生时应用程序的状态，而不是猜测问题发生的原因。LogRocket 还可以监控应用的性能，报告客户端 CPU 负载、客户端内存使用等指标。

自信地构建— [开始免费监控](https://lp.logrocket.com/blg/javascript-signup)。