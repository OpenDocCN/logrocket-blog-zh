# Swift 中的并发性:使用新的 async/await 语法

> 原文：<https://blog.logrocket.com/concurrency-swift-async-await-syntax/>

当苹果在 2014 年首次推出 Swift 时，它旨在满足软件工程师对现代编程语言的所有需求。在苹果公司设计 Swift 的克里斯·拉特纳的目标是开发一种既能用于编程教学又能为操作系统构建软件的语言。

从那以后，苹果公司开源了这种语言，结果，它继续发展。尽管对 Swift 进行了改进，但仍然缺少的一个关键特性是用于并发和并行的原语。

在过去，你可以在 Swift 中使用像[大中央调度(GCD)](https://blog.logrocket.com/grand-central-dispatch-tutorial/) 和 libdispatch 这样的库来模仿原语。如今，我们可以使用`async`和`await`关键字来实施并发原语。

在本教程中，我们将讨论什么是并发以及它为什么有用。然后，我们将学习使用`async`和`await`关键字来加强并发性。

我们开始吧！

## 并发和 CPU 内核

由于过去十年处理器的变化，[并发性](https://blog.logrocket.com/parallelism-concurrency-and-async-programming-in-node-js/#concurrencymeansthat)已经成为计算机编程中一个更相关的话题。尽管新型处理器中的晶体管数量有所增加，但时钟速度并没有显著提高。

然而，处理器的一个值得注意的改进是每个芯片上有更多的 CPU 内核。苹果较新的处理器，如 iPhone 12 中的 A14，有六个 CPU 内核。用于 MAC 电脑和 iPad 的 M1 处理器有八个 CPU 核心[。然而，A14 的时钟速度仍然在 3.1 GHz 左右。](https://www.apple.com/newsroom/2020/11/apple-unleashes-m1/)

CPU 设计的真正进步来自于现代芯片内核数量的改变。为了利用这些更新的处理器，我们需要提高并发编程的能力。

## 长期运行的任务

在大多数现代计算机系统中，主线程用于呈现和处理用户界面和用户交互。iOS 开发者经常被强调永远不要阻塞主线程。

像发出网络请求、与文件系统交互或查询数据库这样的长时间运行的任务会阻塞主线程，导致应用程序的 UI 冻结。令人欣慰的是，苹果提供了许多不同的工具，我们可以用它们来防止应用程序的用户界面被阻塞。

## Swift 中的并发选项

对 GCD 和 libdispatch 等框架的改进使得并发编程变得更加容易。

iOS 设备当前的最佳实践是将任何会阻塞主线程的任务卸载到后台线程或队列中。一旦任务完成，结果通常在一个块或尾随闭包中处理。

在 GCD 发布之前，苹果提供了使用委托来卸载任务的 API。首先，开发人员必须为委托对象运行一个单独的线程，该线程调用调用类上的方法来处理任务的完成。

尽管卸载任务是可行的，但是阅读这种类型的代码可能是困难的，任何错误都可能引入新类型的错误。因此，2017 年，克里斯·拉特纳撰写了他的 [Swift 并发宣言](https://gist.github.com/lattner/31ed37682ef1576b16bca1432ea9f782)，表达了他对如何使用 async/await 为 Swift 添加并发的想法。

## 中央调度中心

GCD 于 2009 年首次推出，是苹果公司通过苹果操作系统上的托管线程池来管理任务并行性的方法。

GCD 的实现起源于一个 C 库，允许开发人员将它与 C、C++和 Objective-C 一起使用。在 Swift 推出后，为使用苹果较新语言的开发人员创建了 GCD 的 Swift 包装器。

GCD 还被移植到了 libdispatch 上，其他开源软件也在使用它。Apache Web 服务器已经合并了这个库来进行多处理。

### 中央车站`DispatchQueue`

让我们看看 GCD 的运行情况！我们将使用 GCD 将工作分配给另一个调度队列。在下面的代码片段中，一个函数将其部分工作分配给一个异步任务:

```
swift
func doSomethinginTheBackground() {
    DispatchQueue.global(qos: .background).async {
        // Do some long running work here
        ...
    }
}

```

`DispatchQueue`类提供了允许开发人员在尾随闭包中运行代码的方法和属性。一个常见的场景是在产生某种结果的尾随闭包中运行一个长时间运行的任务，然后将结果返回给主线程。

在下面的代码片段中，`DispatchQueue`在将结果返回给主线程之前正在做一些工作:

```
swift
DispatchQueue.global(qos: .background).async {
    // Do some work here
    DispatchQueue.main.async {
        // return to the main thread.
        print("Work completed and back on the main thread!")
    }
}

```

更常见的场景是使用`NSURLSession`进行网络调用，在尾随闭包中处理结果，然后返回到主线程:

```
swift
func goGrabSomething(completion: @escaping (MyJsonModel?, Error?) -> Void) {
    let ourl = URL(string: "https://mydomain.com/api/v1/getsomejsondata")
    if let url = ourl {
        let req = URLRequest(url: url)
        URLSession.shared.dataTask(with: req) { data, _, err in
            guard let data = data, err == nil else {
                return
            }
            do {
                let model = try JSONDecoder().decode(MyJsonModel.self, from: data)
                DispatchQueue.main.async {
                    completion(model, nil)
                }
            } catch {
                completion(nil, error)
            }
        }.resume()
    }
}

```

虽然上面的例子可以编译和运行，但是有几个错误。首先，我们没有在函数可以退出的任何地方使用完成处理程序。同步编写代码时也更难阅读。

为了改进上面的代码，我们将使用`async`和`await`。

## 在代码中使用 async/await

当 iOS 15 和 macOS 12 在 2021 年秋季发布时，开发者将能够使用新的 async/await 语法。您已经可以在 JavaScript 和 C#等语言中使用 async/await。

这两个关键字正成为开发人员用现代编程语言编写并发代码的最佳实践。让我们来看看前面的函数`goGrabSomething`，使用新的 async/await 语法重写:

```
swift
func goGrabSomething() async throws -> MyJsonModel? {
    var model: MyJsonModel? = nil
    let ourl = URL(string: "https://mydomain.com/api/v1/getsomejsondata")
    if let url = ourl {
        let req = URLRequest(url: url)
        let (data, _) = try await URLSession.shared.data(for: req)
        model = try JSONDecoder().decode(MyJsonModel.self, from: data)
    }
    return model
}

```

在上面的例子中，我们在`throws`之前和函数名之后添加了`async`关键字。如果我们的函数没有抛出，`async`会在`->`之前。

我能够改变函数签名，使它不再需要完成。现在，我们可以返回已经从我们的 API 调用中解码的对象。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

在我们的函数中，我在我的`URLSession.shared.data(for: URLRequest)`前面使用了关键字`await`。由于`URLSession`数据函数会抛出错误，所以我在`await`关键字前面加了一个`try`。

每次我们在函数体中使用`await`时，它都会创建一个延续。如果系统在处理我们的函数时必须等待，它可以挂起我们的函数，直到它准备好从挂起状态返回。

如果我们试图从同步代码中调用`goGrabSomething`函数，将会失败。Swift 为该用例提供了一个很好的解决方案！我们可以在同步代码中使用一个`async`闭包来调用我们的`async`函数:

```
swift
async {
    var myModel = try await goGrabSomething() 
    print("Name: \(myModel.name)")
}

```

现在，Swift 拥有自己的系统来管理并发性和并行性。通过利用这些新的关键字，我们可以利用系统中新的并发特性。

最终的结果是，我们能够编写一个更容易阅读并且包含更少代码的函数。

## 结论

Swift 中的 Async/await 大大简化了我们在 iOS 应用中编写并发代码的方式。你可以通过下载 Xcode 13 并在 iOS 15 和 macOS 12 的测试版上运行这些例子来体验这些新功能。

这篇文章仅仅触及了这些新特性的皮毛。例如，Swift 还增加了一个`actor`对象类型，允许开发人员创建包含共享`mutable`状态的`write`对象，这些对象可以跨线程使用，而没有竞争条件。

我希望你喜欢这篇文章。如果你有兴趣了解更多关于 Swift 中的 async/await，请观看[苹果的 WWDC21 演示](https://developer.apple.com/videos/play/wwdc2021/10132/)。

## 使用 [LogRocket](https://lp.logrocket.com/blg/signup) 消除传统错误报告的干扰

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/signup)

[LogRocket](https://lp.logrocket.com/blg/signup) 是一个数字体验分析解决方案，它可以保护您免受数百个假阳性错误警报的影响，只针对几个真正重要的项目。LogRocket 会告诉您应用程序中实际影响用户的最具影响力的 bug 和 UX 问题。

然后，使用具有深层技术遥测的会话重放来确切地查看用户看到了什么以及是什么导致了问题，就像你在他们身后看一样。

LogRocket 自动聚合客户端错误、JS 异常、前端性能指标和用户交互。然后 LogRocket 使用机器学习来告诉你哪些问题正在影响大多数用户，并提供你需要修复它的上下文。

关注重要的 bug—[今天就试试 LogRocket】。](https://lp.logrocket.com/blg/signup-issue-free)