# 如何在 Go 中使用指针

> 原文：<https://blog.logrocket.com/how-to-use-pointers-in-go/>

近年来，围棋的受欢迎程度呈爆炸式增长。2020 年[黑客地球开发者调查](https://www.hackerearth.com/recruit/developer-survey/)发现，Go 是最受有经验的开发者和学生追捧的编程语言。 [2021 栈溢出开发者调查](https://insights.stackoverflow.com/survey/2021#most-loved-dreaded-and-wanted-language-want)报告了类似的结果，Go 是开发者最想使用的四种语言之一。

鉴于 Go 的流行，对于 web 开发人员来说，掌握 Go 非常重要，而 Go 最关键的组件之一可能就是它的指针。本文将解释创建指针的不同方式以及指针所解决的问题类型。

## 围棋是什么？

Go 是 Google 开发的一种静态类型的编译语言。Go 成为构建健壮、可靠和高效软件的流行选择有很多原因。最大的吸引力之一是 Go 编写软件的简单而简洁的方法，这在语言中指针的[实现中显而易见。](https://en.wikipedia.org/wiki/Pointer_(computer_programming))

## 在 Go 中传递参数

当用任何语言编写软件时，开发人员必须考虑他们的代码库中哪些代码可能会发生变异。

当您开始编写函数和方法并在代码中传递所有不同类型的数据结构时，您需要注意哪些应该通过值传递，哪些应该通过引用传递。

通过值传递参数就像传递某种东西的打印副本。如果副本的持有者在上面乱涂乱画或破坏它，你拥有的原始副本是不变的。

通过引用传递就像与某人共享原始副本一样。如果他们改变了什么，你可以看到——并且必须处理——他们所做的改变。

让我们从一段真正基础的代码开始，看看你是否能发现为什么它可能没有做我们期望它做的事情。

```
package main

import (
  "fmt"
)

func main() {
  number := 0
  add10(number)
  fmt.Println(number) // Logs 0
}

func add10(number int) {
  number = number + 10 
}

```

在上面的例子中，我试图让`add10()`函数增加`number` `10`，但是它似乎不起作用。它只是返回`0`。这正是指针要解决的问题。

## 在 Go 中使用指针

如果我们想让第一个代码片段工作，我们可以使用指针。

在 Go 中，每个函数参数都是通过值传递的，这意味着值是被复制和传递的，通过改变函数体中的参数值，底层变量不会发生任何变化。

此规则的唯一例外是切片和贴图。它们可以通过值传递，因为它们是引用类型，对它们传递位置的任何更改都会改变底层变量。

将参数传递给其他语言认为“通过引用”的函数的方法是利用指针。

让我们[修复我们的第一个例子](https://play.golang.org/p/jZs9CdNraAa)并解释发生了什么。

```
package main

import (
  "fmt"
)

func main() {
  number := 0
  add10(&number)
  fmt.Println(number) // 10! Aha! It worked!
}

func add10(number *int) {
  *number = *number + 10 
}

```

## 寻址指针语法

第一个代码片段和第二个代码片段之间唯一的主要区别是`*`和`&`的用法。这两个操作符执行称为[解引用/间接引用](https://tour.golang.org/moretypes/1) ( `*`)和[引用/内存地址检索](https://gobyexample.com/pointers) ( `&`)的操作。

## 使用`&`进行引用和存储器地址检索

如果您遵循从`main`函数开始的代码片段，我们更改的第一个操作符是在传递给`add10`函数的`number`参数前使用一个&符号`&`。

这将得到我们在 CPU 中存储变量的内存地址。如果您在第一个代码片段中添加一个日志，您会看到一个用十六进制表示的内存地址。大概是这样的:`0xc000018030`(每次登录都会改变)。

这个有点神秘的字符串实际上指向 CPU 上存储变量的一个地址。这就是 Go 共享变量引用的方式，因此所有其他可以访问指针或内存地址的地方都可以看到变化。

## 使用`*`取消内存引用

如果我们现在唯一拥有的是一个内存地址，那么将`10`加到`0xc000018030`可能不是我们所需要的。这就是解引用内存有用的地方。

我们可以使用指针，将内存地址转换成它所指向的变量，然后进行计算。我们可以在上面第 14 行的代码片段中看到这一点:

```
*number = *number + 10 

```

这里，我们将内存地址取消引用到`0`，然后将`10`添加到它。

现在，代码示例应该如最初预期的那样工作。我们共享一个反映变化的变量，而不是通过复制值。

我们创建的心智模型有一些扩展，这将有助于进一步理解指针。

## 在 Go 中使用`nil`指针

Go 中的所有东西在第一次初始化时都会被赋予一个`0`值。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

例如，当您创建一个字符串时，它默认为一个空字符串(`""`)，除非您给它赋值。

这里是[所有的零值](https://yourbasic.org/golang/default-zero-value/):

*   `0`对于所有 int 类型
*   `0.0`适用于 float32、float64、complex64 和 complex128
*   `false` for bool
*   `""`为字符串
*   `nil`用于界面、切片、通道、地图、指针和功能

指针也是如此。如果你创建了一个指针，但没有指向任何内存地址，那么它将是`nil`。

```
package main

import (
  "fmt"
)

func main() {
  var pointer *string
  fmt.Println(pointer) // <nil>
}

```

## 使用和取消引用指针

```
package main

import (
  "fmt"
)

func main() {
  var ageOfSon = 10
  var levelInGame = &ageOfSon
  var decade = &levelInGame

  ageOfSon = 11
  fmt.Println(ageOfSon)
  fmt.Println(*levelInGame)
  fmt.Println(**decade)
}

```

你可以在这里看到，我们试图在代码的很多地方重用`ageOfSon`变量，所以我们可以一直指向其他指针。

但是在第 15 行，我们必须取消引用一个指针，然后取消引用它所指向的下一个指针。

这利用了我们已经知道的操作符`*`，但是它也链接了下一个要被解引用的指针。

这可能看起来令人困惑，但是当您查看其他指针实现时，您以前见过这个`**`语法会有所帮助。

## 使用备用指针语法创建 Go 指针

创建指针最常见的方法是使用我们前面讨论过的语法。但是也有另一种语法可以用来使用`new()`函数创建指针。

让我们看一个[示例代码片段](https://play.golang.org/p/DAUovs2PP3P)。

```
package main

import (
  "fmt"
)

func main() {
  pointer := new(int) // This will initialize the int to its zero value of 0
  fmt.Println(pointer) // Aha! It's a pointer to: 0xc000018030
  fmt.Println(*pointer) // Or, if we dereference: 0
}

```

语法只是略有不同，但是我们已经讨论过的所有原则都是相同的。

## 常见的 Go 指针误解

回顾我们所学的一切，在使用指针时有一些经常重复的误解，这对讨论很有用。

每当讨论指针时，一个经常重复的短语是它们更具性能，这从直觉上讲是有意义的。

例如，如果您将一个大型结构传递到多个不同的函数调用中，您会发现将该结构多次复制到不同的函数中可能会降低程序的性能。

但是在 Go 中传递指针[往往比传递复制值](https://medium.com/@meeusdylan/when-to-use-pointers-in-go-44c15fe04eac)要慢。

这是因为当指针被传入函数时，Go 需要执行一个[转义分析](https://en.wikipedia.org/wiki/Escape_analysis)来计算出该值是需要存储在堆栈中还是堆中。

按值传递允许将所有变量存储在堆栈中，这意味着可以跳过对该变量的垃圾收集。

点击查看这个示例程序[:](https://blog.gopheracademy.com/advent-2018/avoid-gc-overhead-large-heaps/)

```
func main() {
  a := make([]*int, 1e9)

  for i := 0; i < 10; i++ {
    start := time.Now()
    runtime.GC()
    fmt.Printf("GC took %s\n", time.Since(start))
  }

  runtime.KeepAlive(a)
}

```

当分配十亿个指针时，垃圾收集器会占用半秒多的时间。这小于每个指针一纳秒。但是它可以累积起来，特别是当指针在一个巨大的代码库中大量使用，并且内存需求很大的时候。

如果不使用指针而使用上面相同的代码，垃圾收集器可以比快 1000 倍以上[。](https://blog.gopheracademy.com/advent-2018/avoid-gc-overhead-large-heaps/)

请测试您的用例的性能，因为没有硬性的规则。请记住这句口头禅，“指针总是更快”，并不是在所有情况下都是正确的。

## 结论

我希望这是一个有用的总结。在这篇文章中，我们讨论了什么是 Go 指针，创建它们的不同方法，它们解决什么问题，以及在它们的用例中需要注意的一些问题。

当我第一次学习指针时，我阅读了 GitHub 上大量编写良好的大型代码库(例如 [Docker](https://github.com/docker) ),试图理解何时以及何时不使用指针，我鼓励你也这样做。

这非常有助于巩固我的知识，并以实际操作的方式理解团队使用指针充分发挥潜力的不同方法。

有许多问题需要考虑，例如:

*   我们的性能测试表明了什么？
*   更广泛的代码库中的总体约定是什么？
*   这对这个特殊的用例有意义吗？
*   阅读和理解这里发生的事情简单吗？

决定何时以及如何使用指针是根据具体情况而定的，我希望您现在对何时在项目中最好地利用指针有一个透彻的理解。

## 使用 [LogRocket](https://lp.logrocket.com/blg/signup) 消除传统错误报告的干扰

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/signup)

[LogRocket](https://lp.logrocket.com/blg/signup) 是一个数字体验分析解决方案，它可以保护您免受数百个假阳性错误警报的影响，只针对几个真正重要的项目。LogRocket 会告诉您应用程序中实际影响用户的最具影响力的 bug 和 UX 问题。

然后，使用具有深层技术遥测的会话重放来确切地查看用户看到了什么以及是什么导致了问题，就像你在他们身后看一样。

LogRocket 自动聚合客户端错误、JS 异常、前端性能指标和用户交互。然后 LogRocket 使用机器学习来告诉你哪些问题正在影响大多数用户，并提供你需要修复它的上下文。

关注重要的 bug—[今天就试试 LogRocket】。](https://lp.logrocket.com/blg/signup-issue-free)