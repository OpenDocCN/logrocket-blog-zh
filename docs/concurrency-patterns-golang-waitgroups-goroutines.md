# Golang 中的并发模式:WaitGroups 和 Goroutines

> 原文：<https://blog.logrocket.com/concurrency-patterns-golang-waitgroups-goroutines/>

并发性是指程序在重叠的时间段内独立运行多个任务的能力。在并发程序中，几个任务可以同时运行，没有特定的顺序，它们相互通信、共享资源、相互干扰。

随着多核 CPU 的兴起和并行执行线程的能力，开发人员现在可以构建真正的并发程序。

Golang 提供了 goroutines 来支持 Go 中的并发性。goroutine 是一个与程序中的其他 Go routine 同时执行的函数，是由 Go 管理的轻量级线程。

一个 goroutine 需要大约 2kB 的堆栈空间来初始化。相比之下，一个标准线程可以占用 1MB，这意味着创建一千个 goroutines 比创建一千个线程占用的资源少得多。

在本教程中，我们将探索 goroutines，goroutines 之间使用通道的通信，以及使用`WaitGroup` s 同步 goroutines。

## Goroutines 教程先决条件

要遵循并理解本教程，您需要以下内容:

您还可以克隆此[指南的存储库](https://github.com/Bamimore-Tomi/go-templates-guide)来访问完整的模板文件，或者在您的终端中运行以下内容:

```
git clone https://github.com/Bamimore-Tomi/goroutines-logrocket.git
```

## 在 Golang 创建 goroutines

在函数调用前添加关键字`go`会将 Go 运行时作为 goroutine 执行。

为了演示，让我们写一个函数，打印出随机数，然后休眠。第一个示例是顺序程序，第二个示例使用 goroutines:

```
go
package main

import (
    "fmt"
    "math/rand"
    "time"
)

// name is a string to identify the function call
// limit the number of numbers the function will print
// sleep is the number of seconds before the function prints the next value
func randSleep(name string, limit int, sleep int) {
    for i := 1; i <= limit; i++ {
        fmt.Println(name, rand.Intn(i))
        time.Sleep(time.Duration(sleep * int(time.Second)))

    }

}
func main() {
    randSleep("first:", 4, 3)
    randSleep("second:", 4, 3)

}

// OUTPUT
// first: 0
// first: 1
// first: 2
// first: 3
// second: 0
// second: 0
// second: 1
// second: 0

// git checkout 00
```

在这个顺序运行中，Go 按照函数调用的顺序打印数字。在下面的程序中，这些函数同时运行:

```
go
package main

import (
    "fmt"
    "math/rand"
    "time"
)

// name is a string to identify the function call
// limit the number of numbers the function will print
// sleep is the number of seconds before the function prints the next value
func randSleep(name string, limit int, sleep int) {
    for i := 1; i < limit; i++ {
        fmt.Println(name, rand.Intn(i))
        time.Sleep(time.Duration(sleep * int(time.Second)))

    }

}
func main() {
    go randSleep("first:", 4, 3)
    go randSleep("second:", 4, 3)

}

// git checkout 01
```

这个程序不会在终端中打印出任何东西，因为`main`函数在 goroutines 执行之前就完成了，这是一个问题；您不希望您的`main`在 goroutines 完成它们的执行之前完成并终止。

如果在 goroutine 之后有另一个顺序代码，它将并发运行，直到该顺序代码完成其执行。无论完成与否，程序都会终止。

```
go
package main

import (
    "fmt"
    "math/rand"
    "time"
)

// name is a string to identify the function call
// limit the amount of number the function will print
// sleep is the number of seconds before the function prints the next value
func randSleep(name string, limit int, sleep int) {
    for i := 1; i <= limit; i++ {
        fmt.Println(name, rand.Intn(i))
        time.Sleep(time.Duration(sleep * int(time.Second)))

    }

}
func main() {
    go randSleep("first:", 10, 2)
    randSleep("second:", 3, 2)

}

// second: 0
// first: 0
// second: 1
// first: 1
// first: 1
// second: 0

// git checkout 02
```

程序在 goroutine 下面的函数完成执行后终止，不管 goroutine 是否完成。

为了解决这个问题，Golang 提供了`WaitGroup` s。

### `WaitGroup` s 在戈朗

sync 包中提供的`WaitGroup`，允许程序等待指定的例程。Golang 中的这些同步机制会阻止程序的执行，直到`WaitGroup`中的 goroutines 完全执行，如下所示:

```
go
package main

import (
    "fmt"
    "math/rand"
    "sync"
    "time"
)

// wg is the pointer to a waitgroup
// name is a string to identify the function call
// limit the number of numbers the function will print
// sleep is the number of seconds before the function prints the next value
func randSleep(wg *sync.WaitGroup, name string, limit int, sleep int) {
    defer wg.Done()
    for i := 1; i <= limit; i++ {
        fmt.Println(name, rand.Intn(i))
        time.Sleep(time.Duration(sleep * int(time.Second)))

    }

}
func main() {
    wg := new(sync.WaitGroup)
    wg.Add(2)
    go randSleep(wg, "first:", 10, 2)
    go randSleep(wg, "second:", 3, 2)
    wg.Wait()

}

// OUTPUT

// second: 0
// first: 0
// first: 1
// second: 1
// second: 1
// first: 0
// first: 1
// first: 0
// first: 4
// first: 1
// first: 6
// first: 7
// first: 2

// git checkout 03
```

这里，`wg := new(sync.WaitGroup)`创建一个新的`WaitGroup`，而`wg.Add(2)`通知`WaitGroup`它必须等待两个 goroutines。

随后是`defer wg.Done()`在 goroutine 完成时提醒`WaitGroup`。

然后阻塞执行，直到 goroutines 的执行完成。

整个过程就像在`wg.Add()`里给计数器加，在`wg.Done()`里给计数器减，在`wg.Wait()`里等待计数器打到`0`。

## Goroutines 之间的通信

在编程中，并发任务可以相互通信，共享资源。Go 为两个 goroutines 之间通过通道进行双向通信提供了一种方式。

双向通信意味着任何一方都可以发送或接收消息，因此 [Go 提供通道作为在 goroutines](https://blog.logrocket.com/how-use-go-channels/) 之间发送或接收数据的机制。

您可以通过声明或使用`make`功能来创建通道:

```
go
package main

import (
    "fmt"
)

func main() {
    // creating a channel by declaring it
    var mychannel1 chan int
    fmt.Println(mychannel1)

    // creating a channel using make()

    mychannel2 := make(chan int)
    fmt.Println(mychannel2)

}

// git checkout 04
```

Go 中的双向通道是阻塞的，这意味着当向通道发送数据时，Go 会等待，直到从通道中读取数据后再继续执行:

```
go
package main

import (
    "fmt"
    "sync"
)

func writeChannel(wg *sync.WaitGroup, limitchannel chan int, stop int) {
    defer wg.Done()
    for i := 1; i <= stop; i++ {
        limitchannel <- i
    }

}

func readChannel(wg *sync.WaitGroup, limitchannel chan int, stop int) {
    defer wg.Done()
    for i := 1; i <= stop; i++ {
        fmt.Println(<-limitchannel)
    }
}

func main() {
    wg := new(sync.WaitGroup)
    wg.Add(2)
    limitchannel := make(chan int)
    defer close(limitchannel)
    go writeChannel(wg, limitchannel, 3)
    go readChannel(wg, limitchannel, 3)
    wg.Wait()

}

// OUTPUT

// 1
// 2
// 3

// git checkout 04
```

通过`limitchannel <- i`,`i`的值进入通道。`fmt.Println(<-limitchannel)`然后接收通道的值并打印出来。

但是，请注意，发送操作的数量必须等于接收操作的数量，因为如果您将数据发送到一个通道，而在其他地方没有接收到，您将获得一个`fatal error: all goroutines are asleep - deadlock!`。

### 缓冲通道

如果您想知道为什么在发送之后必须总是从通道接收，这是因为 Go 没有任何地方存储传递给通道的值。

但是，您可以创建一个存储多个值的通道，这意味着向该通道发送数据不会被阻塞，直到您超出容量:

```
go
limitchannel := make(chan int, 6)
```

该程序将数据发送到一个缓冲通道，并在 goroutine 执行之前不读取数据:

```
go
package main

import (
    "fmt"
    "sync"
)

func writeChannel(wg *sync.WaitGroup, limitchannel chan int, stop int) {
    defer wg.Done()
    for i := 1; i <= stop; i++ {
        limitchannel <- i
    }

}

func main() {
    wg := new(sync.WaitGroup)
    wg.Add(1)
    limitchannel := make(chan int, 2)
    defer close(limitchannel)
    go writeChannel(wg, limitchannel, 2)
    wg.Wait()
    fmt.Println(<-limitchannel)
    fmt.Println(<-limitchannel)

}

// OUTPUT

// 1
// 2

// git checkout 05
```

## 结论

如果不需要从 goroutine 返回任何数据，s 就足够了。然而，在构建并发应用程序时，您经常需要传递数据，通道对此非常有帮助。

理解何时使用通道对于避免死锁情况和[错误至关重要，因为跟踪](https://blog.logrocket.com/comparing-go-debugging-tools/)会非常困难。有时，[指针和`WaitGroups`可以达到一个通道](https://blog.logrocket.com/how-to-use-pointers-in-go/)的目的，但这不在本文讨论范围之内。

## 用[log 火箭](https://lp.logrocket.com/blg/signup)突破传统错误报告的噪音

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/signup)

[LogRocket](https://lp.logrocket.com/blg/signup) 是一个数字体验分析解决方案，它可以保护您免受数百个假阳性错误警报的影响，只针对几个真正重要的项目。LogRocket 会告诉您应用程序中实际影响用户的最具影响力的 bug 和 UX 问题。

然后，使用具有深层技术遥测的会话重放来确切地查看用户看到了什么以及是什么导致了问题，就像你在他们身后看一样。

LogRocket 自动聚合客户端错误、JS 异常、前端性能指标和用户交互。然后 LogRocket 使用机器学习来告诉你哪些问题正在影响大多数用户，并提供你需要修复它的上下文。

关注重要的 bug—[今天就试试 LogRocket】。](https://lp.logrocket.com/blg/signup-issue-free)