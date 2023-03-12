# 如何使用 Go 频道- LogRocket 博客

> 原文：<https://blog.logrocket.com/how-use-go-channels/>

Go 通道是一种允许 Goroutines 交换数据的通信机制。当开发人员同时运行许多 Goroutines 时，通道是相互通信的最方便的方式。

开发人员通常使用这些通道来通知和管理应用程序中的并发性。

在这篇文章中，我们将讨论 Go 通道的一般用法，包括如何写入和读取通道，如何使用通道作为函数参数，以及如何使用 range 来迭代它们。

## 创建 Go 渠道结构

首先，让我们[使用`make`函数在 Go](https://blog.logrocket.com/getting-started-with-go-for-frontend-developers/) 中创建一个通道:

```
// for example if channel created using following : 
ch := make(chan string)

// this is the basic structure of channels 
type hchan struct {
  qcount uint   // total data in the queue
  dataqsiz uint  // size of the circular queue 
  buf  unsafe.Pointer // pointer to an array of dataqsiz elements
  elementSize uint16 
  closed uint32 
  sendx  uint // send index 
  recvx  uint // receive index 
  recvq waitq // list of receive queue 
  sendq  waitq // list of send queue 
  lock mutex // lock protects all fields in hchan, as well as several
}

```

## Go 频道用途

在本节中，我们将回顾 Go 渠道的用途以及它们如何有利于应用程序开发。

### 使用 Go 渠道作为未来和承诺

开发人员经常在 Go 中使用未来和承诺来请求和响应。例如，如果我们想要实现一个 async/await 模式，我们必须添加以下内容:

```
package main 

import (
  "fmt"
  "math/rand"
  "time"
)

func longTimedOperation() <-chan int32 {
  ch := make(chan int32)
  func run(){
    defer close(ch)
    time.Sleep(time.Second * 5)
    ch <- rand.Int31n(300)
  }
  go run()
  return ch
}

func main(){
  ch := longTimedOperation()
  fmt.Println(ch)
}

```

通过简单地模拟一个使用 5 秒延迟的长时间运行的进程，我们可以向通道发送一个随机整数值，等待该值，然后接收它。

### 使用 Go 渠道进行通知

通知是独一无二的返回值请求或响应。我们通常使用[空白结构类型](https://blog.logrocket.com/exploring-structs-and-interfaces-in-go/)作为通知通道元素类型，因为空白结构类型的大小为零，这意味着该结构的值不消耗内存。

例如，使用通道实现一对一通知会收到一个通知值:

```
package main 

import (
  "fmt"
  "time"
) 
type T = struct{}

func main() {
  completed := make(chan T)
  go func() {
    fmt.Println("ping")
    time.Sleep(time.Second * 5) // heavy process simulation
    <- completed // receive a value from completed channel
  }

  completed <- struct{}{} // blocked waiting for a notification 
  fmt.Println("pong")
}

```

这让我们可以使用从一个通道接收到的值来提醒另一个等待向同一个通道提交值的 Goroutine。

频道还可以安排通知:

```
package main

import (
  "fmt"
  "time"
) 

func scheduledNotification(t time.Duration) <- chan struct{} {
  ch := make(chan struct{}, 1) 
  go func() {
    time.Sleep(t)
    ch <- struct{}{}
  }()
  return ch
}

func main() {
    fmt.Println("send first")
    <- scheduledNotification(time.Second)
    fmt.Println("secondly send")
    <- scheduledNotification(time.Second)
    fmt.Println("lastly send")
}

```

### 使用 Go 通道作为计数信号量

为了施加最大数量的并发请求，开发人员经常使用计数信号量来锁定和解锁并发进程，以控制资源和应用互斥。例如，开发人员可以控制数据库中的读写操作。

有两种方法可以获得通道信号量所有权，类似于将通道用作互斥体:

1.  通过发送获取所有权，通过接收释放所有权
2.  通过接收获得所有权，通过发送释放所有权

然而，当拥有通道信号量时，有一些特定的规则。首先，每个通道都允许交换特定的数据类型，也称为通道的元素类型。

第二，要使通道正常运行，必须有人接收通过通道发送的内容。

例如，我们可以使用`chan`关键字声明一个新的通道，我们可以使用`close()`函数关闭一个通道。因此，如果我们使用`< -`通道语法阻止代码从通道中读取，一旦完成，我们就可以关闭它。

最后，当使用通道作为函数参数时，我们可以指定它的方向，这意味着指定通道将用于发送还是接收。

如果我们事先知道一个通道的用途，就使用这个功能，因为它使程序更加健壮和安全。这意味着我们不能意外地向只接收数据的通道发送数据，或者从只发送数据的通道接收数据。

因此，如果我们声明一个通道函数参数将只用于读取，而我们试图写入它，我们会得到一个错误消息，这很可能会将我们从讨厌的错误中解救出来。

## 写入 Go 频道

本小节中的代码教我们如何在 Go 中写通道。将值`x`写入通道`c`就像写入`c <- x`一样简单。

箭头显示值的方向；只要`x`和`c`具有相同的类型，我们对这个语句就没有问题。

在下面的代码中，`chan`关键字声明了`c`函数参数是一个通道，后面必须跟通道的类型，即`int`。然后，`c <- x`语句允许我们将值`x`写入通道`c`，`close()`函数关闭通道:

```
package main
import (
  "fmt"
  "time"
)

func writeToChannel(c chan int, x int) {
   fmt.Println(x)
   c <- x
   close(c)
   fmt.Println(x)
}

func main() {
    c := make(chan int)
    go writeToChannel(c, 10)
    time.Sleep(1 * time.Second)
}

```

最后，执行前面的代码会创建以下输出:

```
$ go run writeCh.go 
10

```

这里奇怪的是`writeToChannel()`函数只打印一次给定值，这是第二个`fmt.Println(x)`语句从不执行时造成的。

原因很简单:`c <- x`语句阻塞了其余`writeToChannel()`函数的执行，因为没有人在读取写入`c`通道的内容。

因此，当`time.Sleep(1 * time.Second)`语句结束时，程序不等待`writeToChannel()`就终止。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

下一节说明如何从通道读取数据。

## 从 Go 频道读取

我们可以通过执行`<-c`从名为`c`的通道中读取单个值。在这种情况下，方向是从通道到外部范围:

```
package main
import (
"fmt"
"time"
)

func writeToChannel(c chan int, x int) {
  fmt.Println("1", x)
  c <- x
  close(c)
  fmt.Println("2", x)
}

func main() {
  c := make(chan int)
  go writeToChannel(c, 10)
  time.Sleep(1 * time.Second)
  fmt.Println("Read:", <-c)
  time.Sleep(1 * time.Second)
  _, ok := <-c

  if ok {
    fmt.Println("Channel is open!")
  } else {
    fmt.Println("Channel is closed!")
  }
}

```

`writeToChannel()`功能的实现与之前相同。在前面的代码中，我们使用`<-c`符号从通道`c`读取。

第二个`time.Sleep(1 * time.Second)`语句给了我们从通道中读取的时间。

当通道关闭时，当前的 Go 代码工作正常；然而，如果通道是打开的，这里显示的 Go 代码将丢弃通道的读取值，因为我们在`_, ok := <-c`语句中使用了`_`字符。

如果我们还想在通道打开的情况下存储在通道中找到的值，请使用合适的变量名，而不是`_`。

执行`readCh.go`产生以下输出:

```
$ go run readCh.go
1 10
Read: 10
2 10
Channel is closed!
$ go run readCh.go
1 10
2 10
Read: 10
Channel is closed!

```

尽管输出仍然是不确定的，但是`writeToChannel()`函数的两个`fmt.Println(x)`语句都会执行，因为当我们从通道中读取时，通道会解除阻塞。

### 从封闭信道接收

在这一小节中，我们将回顾当我们试图使用`readClose.go`中的 Go 代码从一个封闭通道中读取时会发生什么。

在`readClose.go`程序的这一部分，我们必须创建一个名为`willClose`的新的`int`通道，向其写入数据，读取数据，并在接收到数据后关闭通道:

```
package main
import (
  "fmt"
)

func main() {
  willClose := make(chan int, 10)
  willClose <- -1
  willClose <- 0
  willClose <- 2
  <-willClose
  <-willClose
  <-willClose
  close(willClose)
  read := <-willClose
  fmt.Println(read)
}

```

执行之前的代码(保存在`readClose.go`文件中)会生成以下输出:

```
$ go run readClose.go
0

```

这意味着从一个封闭的通道中读取会返回其数据类型的零值，在本例中是`0`。

## 作为函数参数的通道

虽然我们在使用`readCh.go`或`writeCh.go`时没有使用函数参数，但 Go 确实允许我们在使用它作为函数参数时指定通道的方向，这意味着它是用于读取还是写入。

这两种类型的通道称为单向通道，而默认情况下通道是双向的。

检查以下两个函数的 Go 代码:

```
func f1(c chan int, x int) {
 fmt.Println(x)
 c <- x
}

func f2(c chan<- int, x int) {
 fmt.Println(x)
 c <- x
}

```

尽管这两个函数实现了相同的功能，但它们的定义略有不同。这种差异是由在`f2()`函数定义中的`chan`关键字右边的`<-`符号产生的。

这表示`c`通道只能写入。如果 Go 函数的代码试图从只写通道(也称为只发送通道)参数中读取，Go 编译器将生成以下错误信息:

```
# command-line-arguments
a.go:19:11: invalid operation: range in (receive from send-only type chan<-int)

```

类似地，我们可以有以下函数定义:

```
func f1(out chan<- int64, in <-chan int64) {
  fmt.Println(x)
  c <- x
}

func f2(out chan int64, in chan int64) {
  fmt.Println(x)
  c <- x
}

```

`f2()`的定义结合了一个名为 in 的只读通道和一个名为 out 的只写通道。如果我们无意中试图写入和关闭函数的只读通道(也称为只接收通道)参数，我们会得到以下错误信息:

```
# command-line-arguments
a.go:13:7: invalid operation: out <- i (send to receive-only type <-chan int)
a.go:15:7: invalid operation: close(out) (cannot close receive-only 
channel)

```

## Go 频道范围

我们可以在 Golang 中使用范围语法来遍历一个通道以读取其值。这里的迭代应用了先进先出(FIFO)的概念:只要我们向通道缓冲区添加数据，我们就可以像队列一样从缓冲区读取数据:

```
package main

import "fmt"

func main() {

    ch := make(chan string, 2)
    ch <- "one"
    ch <- "two"
    close(ch)

    for elem := range ch {
        fmt.Println(elem)
    }
}

```

如上所述，使用 range 从通道进行迭代应用了 FIFO 原理(从队列中读取)。因此，执行前面的代码会输出以下内容:

```
$ go run range-over-channels.go
one
two

```

## 结论

[Go](https://blog.logrocket.com/tag/go/) 通道用于通过发送和接收特定元素类型的数据，在并发运行的函数之间进行通信。当我们有许多 Goroutines 同时运行时，通道是它们相互通信的最方便的方式。

感谢阅读和快乐编码！🙂

## 使用 [LogRocket](https://lp.logrocket.com/blg/signup) 消除传统错误报告的干扰

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/signup)

[LogRocket](https://lp.logrocket.com/blg/signup) 是一个数字体验分析解决方案，它可以保护您免受数百个假阳性错误警报的影响，只针对几个真正重要的项目。LogRocket 会告诉您应用程序中实际影响用户的最具影响力的 bug 和 UX 问题。

然后，使用具有深层技术遥测的会话重放来确切地查看用户看到了什么以及是什么导致了问题，就像你在他们身后看一样。

LogRocket 自动聚合客户端错误、JS 异常、前端性能指标和用户交互。然后 LogRocket 使用机器学习来告诉你哪些问题正在影响大多数用户，并提供你需要修复它的上下文。

关注重要的 bug—[今天就试试 LogRocket】。](https://lp.logrocket.com/blg/signup-issue-free)