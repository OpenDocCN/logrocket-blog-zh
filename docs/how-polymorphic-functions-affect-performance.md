# 多态 JavaScript 函数如何影响性能

> 原文：<https://blog.logrocket.com/how-polymorphic-functions-affect-performance/>

与任何关于性能的讨论一样，我们需要获得一些关于我们想要优化的 JavaScript 代码类型及其运行环境的共享上下文。所以，让我们从一些定义开始:

**性能。**首先，当我们在计算机程序的上下文中使用“性能”一词时，我们指的是程序执行的速度或效率。

**多态函数。**多态函数是根据传递给它的参数类型改变其行为的函数。

这里的关键词是类型，而不是值。(一个不根据参数的不同值改变输出的函数根本不是一个非常有用的函数。)

**JavaScript 引擎。为了有效地考虑性能，我们还需要知道我们的 JavaScript 将在哪里执行。对于我们的示例代码，我们将使用 [V8 引擎](https://v8.dev/)，因为它很受欢迎。**

V8 是支持 Chrome 浏览器、Node.js、Edge 浏览器等的引擎。注意，也有其他 JavaScript 引擎有自己的性能特点，比如 [SpiderMonkey](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/SpiderMonkey) (火狐用的)[JavaScript core](https://developer.apple.com/documentation/javascriptcore)(Safari 用的)等等。

## 在 JavaScript 中创建多态函数

假设我们正在构建一个 JavaScript 库，使其他工程师能够使用我们简单的 API 轻松地将消息存储到内存数据库中。为了使我们的库使用起来尽可能的简单和舒适，我们提供了一个单一的多态函数，它可以灵活地处理接收到的参数。

### 选项 1:使用完全独立的参数

我们函数的第一个签名将所需的数据作为三个独立的参数，可以这样调用:

```
saveMessage(author, contents, timestamp);

```

### 选项 2:通过`options`对象使用消息内容

这个签名将允许消费者把必需的数据(消息内容)和可选的数据(作者和时间戳)分成两个独立的参数。为了方便起见，我们接受任何顺序的论点。

```
saveMessage(contents, options);
saveMessage(options, contents);

```

### 选项 3:使用一个`options`对象

我们还将允许我们的 API 的用户调用函数，传递一个包含我们需要的所有数据的对象的单个参数:

```
saveMessage(options);

```

### 选项 4:仅使用消息内容

最后，我们将允许 API 的用户只提供消息内容，我们将为其余数据提供默认值:

```
saveMessage(contents);

```

## 实现多态函数

好了，定义了 API 之后，我们就可以构建多态函数的实现了。

```
// We'll utilize an array for a simple in-memory database.
const database = [];

function saveMessage(...args) {
  // Once we get our input into a unified format, we'll use this function to
  // store it on our database and calculate an identifier that represents the
  // data.
  function save(record) {
    database.push(record);
    let result = '';
    for (let i = 0; i < 5_000; i += 1) {
      result += record.author + record.contents;
    }
    return result.length;
  }
  // If the developer has passed us all the data individually, we'll package
  // it up into an object and store it in the database.
  if (args.length === 3) {
    const [author, contents, timestamp] = args;
    return save({author, contents, timestamp});
  }
  // Or, if the developer has provided a message string and an options object,
  // we'll figure out which order they came in and then save appropriately.
  if (args.length === 2) {
    if (typeof args[0] === 'string') {
      const [contents, options] = args;
      const record = {author: options.author, contents, timestamp: options.timestamp};
      return save(record);
    } else {
      const [options, contents] = args;
      const record = {author: options.author, contents, timestamp: options.timestamp};
      return save(record);
    }
  }
  // Otherwise, we've either gotten a string message or a complete set of
  // options.
  if (args.length === 1) {
    const [arg] = args;
    if (typeof arg === 'string') {
      // If the single argument is the string message, save it to the database
      // with some default values for author and timestamp.
      const record = {
        author: 'Anonymous',
        contents: arg,
        timestamp: new Date(),
      };
      return save(record);
    } else {
      // Otherwise, just save the options object in the database as-is.
      return save(arg);
    }
  }
}

```

好了，现在我们将编写一些代码，使用我们的函数存储大量消息——利用它的多态 API——并测量它的性能。

```
const { performance } = require('perf_hooks');

const start = performance.now();
for (let i = 0; i < 5_000; i++) {
  saveMessage(
    'Batman',
    'Why do we fall? So we can learn to pick ourselves back up.',
    new Date(),
  );
  saveMessage(
    'Life doesn\'t give us purpose. We give life purpose.',
    {
      author: 'The Flash',
      timestamp: new Date(),
    },
  );
  saveMessage(
    'No matter how bad things get, something good is out there, over the horizon.',
    {},
  );
  saveMessage(
    {
      author: 'Uncle Ben',
      timestamp: new Date(),
    },
    'With great power comes great responsibility.',
  );
  saveMessage({
    author: 'Ms. Marvel',
    contents: 'When you decide not to be afraid, you can find friends in super unexpected places.',
    timestamp: new Date(),
  });
  saveMessage(
    'Better late than never, but never late is better.'
  );
}
console.log(`Inserted ${database.length} records into the database.`);
console.log(`Duration: ${(performance.now() - start).toFixed(2)} milliseconds`);

```

现在让我们再次实现我们的函数，但是使用一个更简单的单态 API。

## 在 JavaScript 中创建单态函数

作为对更具限制性的 API 的交换，我们可以削减函数的复杂性，并使其单态化，这意味着函数的参数总是相同的类型和相同的顺序。

虽然它没有那么灵活，但是我们可以通过使用默认参数来保留以前实现的一些人体工程学。我们的新函数将如下所示:

```
// We'll again utilize an array for a simple in-memory database.
const database = [];

// Rather than a generic list of arguments, we'll take the message contents and
// optionally the author and timestamp.
function saveMessage(contents, author = 'Anonymous', timestamp = new Date()) {
  // First we'll save our record into our database array.
  database.push({author, contents, timestamp});
  // As before, we'll calculate and return an identifier that represents the
  // data, but we'll inline the contents of the function since there's no need
  // to re-use it.
  let result = '';
  for (let i = 0; i < 5_000; i += 1) {
    result += author + contents;
  }
  return result.length;
}

```

我们将更新前面示例中的性能测量代码，以使用我们新的统一 API。

```
const { performance } = require('perf_hooks');

const start = performance.now();
for (let i = 0; i < 5_000; i++) {
  saveMessage(
    'Why do we fall? So we can learn to pick ourselves back up.',
    'Batman',
    new Date(),
  );
  saveMessage(
    'Life doesn\'t give us purpose. We give life purpose.',
    'The Flash',
    new Date(),
  );
  saveMessage(
    'No matter how bad things get, something good is out there, over the horizon.',
  );
  saveMessage(
    'With great power comes great responsibility.',
    'Uncle Ben',
    new Date(),
  );
  saveMessage(
    'When you decide not to be afraid, you can find friends in super unexpected places.',
    'Ms. Marvel',
    new Date(),
  );
  saveMessage(
    'Better late than never, but never late is better.'
  );
}
console.log(`Inserted ${database.length} records into the database.`);
console.log(`Duration: ${(performance.now() - start).toFixed(2)} milliseconds`);

```

## 比较单态和多态结果

好，现在让我们运行我们的程序并比较结果。

```
$ node polymorphic.js 
Inserted 30000 records into the database.
Duration: 6565.41 milliseconds

$ node monomorphic.js 
Inserted 30000 records into the database.
Duration: 2955.01 milliseconds

```

我们函数的单态版本大约是多态版本的两倍，因为在单态版本中要执行的代码更少。但是因为多态版本中参数的类型和形状变化很大，所以 V8 对我们的代码进行优化更加困难。

简而言之，当 V8 可以识别(a)我们频繁调用的函数，以及(b)该函数使用相同类型的参数调用时，V8 可以为对象属性查找、算术、字符串操作等创建“快捷方式”。

为了更深入地了解这些“捷径”是如何工作的，我推荐这篇文章: *[单态性是怎么回事？](https://mrale.ph/blog/2015/01/11/whats-up-with-monomorphism.html)* 作者维亚切斯拉夫·叶戈罗夫。

## 多态函数与单态函数的利弊

在你开始优化你所有的单态代码之前，首先要考虑几个要点。

多态函数调用不太可能成为性能瓶颈。还有许多其他类型的操作更容易导致性能问题，例如潜在的网络调用、在内存中移动大量数据、磁盘 i/o、复杂的数据库查询等等。

只有当多态函数非常非常“热”(频繁运行)时，才会遇到性能问题。只有高度专业化的应用程序，类似于我们上面虚构的例子，才能从这一级别的优化中受益。如果你有一个只运行几次的多态函数，那么把它重写为单态是没有好处的。

比起试图优化 JavaScript 引擎，你更有可能更新你的代码以提高效率。在大多数情况下，应用良好的软件设计原则并关注代码的复杂性会比关注底层运行时更进一步。此外，V8 和其他引擎不断变得更快，因此一些今天有效的性能优化可能在该引擎的未来版本中变得无关紧要。

## 结论

多态 API 因其灵活性而便于使用。在某些情况下，它们的执行成本可能会更高，因为 JavaScript 引擎无法像更简单的单态函数那样积极地优化它们。

然而，在许多情况下，这种差别是微不足道的。API 模式应该基于其他因素，如易读性、一致性和可维护性，因为性能问题更有可能在其他领域突然出现。编码快乐！

## 通过理解上下文，更容易地调试 JavaScript 错误

调试代码总是一项单调乏味的任务。但是你越了解自己的错误，就越容易改正。

LogRocket 让你以新的独特的方式理解这些错误。我们的前端监控解决方案跟踪用户与您的 JavaScript 前端的互动，让您能够准确找出导致错误的用户行为。

[![LogRocket Dashboard Free Trial Banner](img/cbfed9be3defcb505e662574769a7636.png)](https://lp.logrocket.com/blg/javascript-signup)

LogRocket 记录控制台日志、页面加载时间、堆栈跟踪、慢速网络请求/响应(带有标题+正文)、浏览器元数据和自定义日志。理解您的 JavaScript 代码的影响从来没有这么简单过！

[Try it for free](https://lp.logrocket.com/blg/javascript-signup)

.