# 理解并发和 Rust 编程语言

> 原文：<https://blog.logrocket.com/deep-dive-concurrency-rust-programming-language/>

单个处理器可以处理来自许多不同作业的任务，处理速度之快让人觉得它们都是同时发生的。例如，我们每天在电脑上进行多任务处理。无论是 Chrome 选项卡还是 Node.js 应用程序，每个作业都被称为一个线程。处理速度的整体提升源于拥有多个内核，每个内核就像自己的处理器一样处理多个线程:

![Rust Concurrency Cores Diagram](img/f6c02cbc50df8e517b0c582122de17d0.png)

每个线程一次只能处理一个任务。如果一个任务花费了很长时间，它会冻结应用程序，造成糟糕的用户体验。有了并发性，我们可以避免这种情况。

### 目录

## 理解并发性

许多不同的并发模式在不同的编程语言中使用。例如，我们可以避免使用事件循环阻塞单线程上的任务，就像在 JavaScript 中一样。此外，您可以剥离并行线程或子进程，或者将这些线程分布在多个内核上。

然而，当我们以这种方式使用并发时，仍然有一些逻辑问题需要考虑。例如，如何协调这些独立的线程，使得一个依赖于另一个的线程不会出错？如果两个线程共享状态，那么这两个线程如何同时使用这个状态呢？

每种语言处理这些问题的方式都不同。在本文中，我们将研究如何在 Rust 中处理这些问题。

## Rust 中的并发性

Rust 以独特的方式处理这些问题，因此开发人员避免了在高级和低级语言之间的典型选择。

高级语言通常选择一种方法，需要权衡来提供单一的抽象，这在某些情况下比其他情况下更具性能。另一方面，低级语言通常缺乏抽象，旨在为不同的项目提供创建最佳解决方案的灵活性，显然需要更多的前期工作。

相反，Rust 为各种用例提供了不同的并发抽象，这提供了以更健壮的方式最大化性能和最小化错误的能力。

## 在 Rust 中使用多线程

在 Rust 中，您可以创建 1:1 的线程。Rust 本身没有捆绑线程的功能，这使得 Rust 的运行时间最小化。但是，您可以创建一个线程，并在该线程上运行一系列任务。让我们检查下面的代码:

```
use std::thread;
use std::time::Duration;

fn main() {
    thread::spawn(|| {
        for i in 1..5 {
            // print i, then sleep thread for 2 milliseconds
            println!("Secondary Thread Prints {}", i);
            thread::sleep(Duration::from_millis(2));
        }
    });

    for i in 1..5 {
        // print i, then sleep thread for 2 milliseconds
        println!("Main Thread Prints {}", i);
        thread::sleep(Duration::from_millis(1));
    }
}

```

当我们运行上面的程序时，会创建一个主线程来运行这段代码。当我们看到`thread::spawn`调用时，一个新的线程被创建，运行包含在其中的代码。之后的代码在主线程上并发运行，两者都要从`1`计数到`4`。您应该会看到如下结果:

```
Main Thread Prints 1
Secondary Thread Prints 1
Main Thread Prints 2
Secondary Thread Prints 2
Main Thread Prints 3
Main Thread Prints 4
Secondary Thread Prints 3

```

请注意，辅助线程从未完成计数。它是首先完成的主线程的子线程。我们可以告诉代码在继续下一步之前等待辅助线程完成。我们将把辅助线程保存在一个变量中，并使用它的 join 方法，我们将暂停主线程，直到子线程完成:

```
 use std::thread;
 use std::time::Duration;

 fn main() {
     let secondary_thread = thread::spawn(|| {
         for i in 1..5 {
             // print i, then sleep thread for 2 milliseconds
             println!("Secondary Thread Prints {}", i);
             thread::sleep(Duration::from_millis(2));
         }
     });

     for i in 1..5 {
         // print i, then sleep thread for 2 milliseconds
         println!("Main Thread Prints {}", i);
         thread::sleep(Duration::from_millis(1));
     }
     secondary_thread.join().unwrap();
 }

```

现在，您将看到以下代码:

```
Main Thread Prints 1
Secondary Thread Prints 1
Main Thread Prints 2
Secondary Thread Prints 2
Main Thread Prints 3
Main Thread Prints 4
Secondary Thread Prints 3
Secondary Thread Prints 4

```

你可以在 [Rust 文档](https://doc.rust-lang.org/book/ch16-01-threads.html)中阅读更多关于创建多线程的内容。

## 铁锈中的信息传递

Go 编程语言普及了通道的概念。您可以将通道视为一个线程向另一个线程发送数据以便共享数据的管道。主线程可以创建一个通道，然后子线程可以使用该通道来回发送数据:

```
use std::sync::mpsc;
use std::thread;
fn main() {
    // create a channel
    let (sender, receiver) = mpsc::channel();
    // create a new thread, create a value, send the value using the channel to main thread
    thread::spawn(move || {
        let val = String::from("I was created in the child thread, will be sent to main thread");
        sender.send(val).unwrap();
    });
    // receive and print the message from the child thread
    let received = receiver.recv().unwrap();
    println!("I have received this message from the child thread: {}", received);
}

```

输出应该如下所示:

```
I have received this message from the child thread: I was created in the child thread, will be sent to main thread

```

使用上面的代码，主线程在创建子线程之前创建一个通道。信道由两个对象组成，一个是发送数据的发送方，另一个是接收数据的接收方。

如果您的代码必须进行一些昂贵的计算，您可以将它偏移到另一个线程，然后使用通道将结果返回到主线程中，而不必阻塞它。您可以在 [Rust 文档](https://doc.rust-lang.org/book/ch16-02-message-passing.html)中了解更多关于消息传递的信息。

## 并发共享状态

虽然您可以使用通道来回传递值，但是存储这些值的内存是不共享的。但是，也许您希望在线程之间共享内存，以减少内存使用。在这个场景中，我们将使用 Rust 中的`mutex`对象。

一个`mutex`对象可以包含一个值，一个线程可以在任何时间点获得这个值的所有权。任何想要使用`mutex`的线程都必须声明对象，锁定它，使用数据，然后解锁。否则另一个线程无法认领。

协调并确保所有的锁定和解锁以有序的方式发生可能很棘手。但是，对于可以在许多线程中使用的数据，它可以在内存使用方面提供好处。

为了确保这个`mutex`对象可以在线程之间安全地共享，我们需要给它一些原子性的保证，要么全部改变，要么什么都不改变，这可以通过将它包装成一个`Arc`对象来实现:

```
use std::sync::{Arc, Mutex};
use std::thread;
fn main() {
    // create the mutex to have shared state
    // Wrap it in an Arc object to share safely
    let shared_state = Arc::new(Mutex::new(0));
    // create a vector to hold all the threads
    let mut threads = vec![];
    // loop 16 times, create a thread on each loop, that uses the mutex
    for _ in 0..16 {
        // create an atomic copy of the shared state
        let shared_state = Arc::clone(&shared_state);
        let child_thread = thread::spawn(move || {
            // lock the shared state in this thread
            let mut num = shared_state.lock().unwrap();
            // mutate the shared_state
            *num += 1;
        });
        // push thread into vector
        threads.push(child_thread);
    }
    // make sure to wait for all threads to complete
    for child_thread in threads {
        child_thread.join().unwrap();
    }
    // the lock shared state and print it in the main thread
    println!("Result: {}", *shared_state.lock().unwrap());
}

```

在上面的代码中，我们获取了`0`的值，并将其包装在一个`mutex`中以供共享。然后我们将它包装在一个`Arc`对象中，赋予它线程间的原子性。

我们创建一个数组来保存所有的子线程，并循环 16 次。在每个循环中，我们克隆`shared_state`，创建一个新的线程来锁定`shared_state`，并递增它。总共有 16 个线程将使用这个共享状态。

线程被推到向量中，所以我们可以在向量上循环，以确保主线程等待它们的完成。然后，我们打印共享状态的最终值。结果如下:

```
Result: 16

```

当每个循环运行时，它们在内存中变异相同的数据。你可以在 Rust 文档中读到更多关于线程间共享状态的信息。

## 扩展并发性

Rust 试图限制并发功能。注意，要使用我们所做的特性，我们必须从标准库中导入它们。它们是库，而不是本地语言特性，这意味着您可以使用相同的原语来创建自己的并发抽象。这主要是通过实现`std::marker`库中的`Send`和`Sync`特征来实现的。为了理解这两个特征和更多，查看一下 [Rust 文档](https://doc.rust-lang.org/book/ch16-04-extensible-concurrency-sync-and-send.html)。

## 结论

Rust 的目标是成为一种低级语言，仍然提供强类型的抽象，使开发人员的生活更容易。谈到 Rust 中的并发性，您可以生成子线程，使用 Go 等通道在线程之间传递数据，或者使用`Arc`和`mutex`在线程之间共享状态。这些都内置在标准库中，为您提供了进一步扩展的构件。

我希望你喜欢这个教程！如果你有任何问题，请留下评论。快乐编码。

## [log rocket](https://lp.logrocket.com/blg/rust-signup):Rust 应用的 web 前端的全面可见性

调试 Rust 应用程序可能很困难，尤其是当用户遇到难以重现的问题时。如果您对监控和跟踪 Rust 应用程序的性能、自动显示错误、跟踪缓慢的网络请求和加载时间感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/rust-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/rust-signup)

LogRocket 就像是网络和移动应用程序的 DVR，记录你的 Rust 应用程序上发生的一切。您可以汇总并报告问题发生时应用程序的状态，而不是猜测问题发生的原因。LogRocket 还可以监控应用的性能，报告客户端 CPU 负载、客户端内存使用等指标。

现代化调试 Rust 应用的方式— [开始免费监控](https://lp.logrocket.com/blg/rust-signup)。