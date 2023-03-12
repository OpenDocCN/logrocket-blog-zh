# Rust - LogRocket 博客中日志记录和跟踪的比较

> 原文：<https://blog.logrocket.com/comparing-logging-tracing-rust/>

作为 web 开发人员，我们必须处理软件中出现的错误、警告、bug 和事件，因此通过监控和记录重要事件来了解软件的结构和行为以有效地收集足够的数据来帮助我们更快地找到根本原因是至关重要的。

有时，术语跟踪和日志记录可以互换使用，但它们并不完全相同。我们将了解如何在 Rust 应用程序中将日志记录与 Rust [日志箱](https://crates.io/crates/log)和跟踪与[跟踪箱](https://crates.io/crates/tracing)集成，它们之间的区别，以及为什么它们是每个软件不可或缺的一部分。

## Rust 中记录和跟踪的区别

日志记录和跟踪有相似的用例以及共同的目的——帮助您找到应用程序中问题的根本原因。

## 铁锈伐木

想象一下，你设计了一个被 50，000 多用户使用的应用程序，突然它在生产中停止工作了🙄。当然，您已经测试过了，但是几乎不可能有 100%的真实测试覆盖率，而且，即使您的测试覆盖率是 100%，也不意味着您的代码是完美的。现在，整个团队变成了消防队。

你怎么知道发生了什么？如果您在应用程序中放置了一段代码来记录发生的错误或警告，您会很容易地引用这些数据(日志文件)并找出在应用程序停止工作或经历停机之前记录的消息。这将有助于您更快地找到问题的原因。

在 Rust 生态系统中，集成日志的实际工具是[日志箱](https://crates.io/crates/log/)。它提供了一个 API，从其他库中抽象出实际的日志实现。它被设计成可以被其他库用来以他们想要的任何方式实现日志记录，使用在 Rust 中作为宏实现的标准日志记录级别。

以下是不同日志级别的标准宏:

```
use log::{ info, warn, error, debug, };

debug!("Something weird occured: {}", someDebugVariable);
error!("{}", "And error occured");
info!("{:?}", "Take note");
warn!("{:#?}", "This is important");

```

让我们使用它的一个实现(你可以在[文档](https://docs.rs/log/latest/log/)中找到更多)，即`[env_logger](https://docs.rs/env_logger/*/env_logger/)`。env 记录器是一个最小的配置记录器。它主要允许您使用环境变量配置日志，如下所示:

```
RUST_LOG=info cargo run

```

让我们探索一下如何将日志与 env_logger 一起使用。确保您已经安装了 [Rust 及其工具链](https://www.rust-lang.org/tools/install)。

首先，通过在你的终端上运行`cargo init`来初始化一个 Rust 应用程序，然后将`env_logger`和`log`作为依赖项添加到你的`Cargo.toml`文件中。

```
[dependencies]
env_logger = "0.9.0"
log = "0.4.16"

```

Log 要求所有实现它的库将 log 添加为依赖项。

接下来，将`log`及其`log`级宏导入到您想要添加日志的文件中。对于这个例子，我们将它放在`main.rs`文件中。

然后，在使用宏之前初始化`env_logger`，如下所示:

```
use log::{ info, error, debug, warn };
fn main() {
    env_logger::init();
    error!("{}", "And error occured");
    warn!("{:#?}", "This is important");
    info!("{:?}", "Take note");
    debug!("Something weird occured: {}", "Error");
}

```

如果在`env_logger`初始化之前使用任何日志级别，将不会有任何影响。需要注意的是，如果您使用`cargo run`运行这段代码，控制台上只会显示错误消息。

这是因为错误日志级别是默认的日志级别，并且是日志级别层次结构中的最高级别。您应该查看[文档](https://docs.rs/env_logger/latest/env_logger/)以获得更多配置选项和其他可能适合您的项目的实现选项。

## 铁锈色痕迹

> 在软件工程中，[跟踪](https://en.wikipedia.org/wiki/Tracing_(software))涉及日志记录的特殊用途，以记录关于程序执行的信息–维基百科

跟踪包括在执行过程中从头到尾监视代码逻辑流。很容易发现这对您有多重要，尤其是如果您正在设计一个大型应用程序，其中可能会出现太多问题，调试可能会很痛苦；跟踪为您提供了代码中活动的系统概述。

Rust [跟踪](https://crates.io/crates/tracing)库利用跟踪，并为开发人员提供一个全面的框架，允许您从 Rust 程序中收集结构化的、基于事件的诊断信息。

代码跟踪包括三个不同的阶段:

1.  **检测**:这是向应用程序源代码添加跟踪代码的地方
2.  **实际跟踪**:在执行过程中的这一点上，活动被写入目标平台进行分析
3.  **分析**:分析和评估跟踪系统收集的信息，以发现和理解应用程序中的问题的阶段。这也是你可以插入像 [LogRocket](https://logrocket.com/) 、 [Sentry](https://sentry.io/) 或 [Grafana](https://grafana.com/) 这样的工具的地方，让你可视化你的整个系统工作流程、性能、错误和你可以改进的地方

就像 log 一样，Rust tracing 库提供了一个健壮的 API，允许库开发人员实现必要的功能来收集所需的跟踪数据。

已经编写了几个库来处理跟踪。您可以在[跟踪文档](https://crates.io/crates/tracing)中找到它们。

让我们以[追踪订阅者](https://docs.rs/tracing-subscriber/)为例，看看如何将追踪整合到你的 Rust 应用中。将`tracing-subscriber`添加到您的依赖列表中。确保添加`tracing`作为依赖项。

您的`/Cargo.toml`文件应该如下所示:

```
[package]
name = "tracing_examples"
version = "0.1.0"
edition = "2021"
# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html
[dependencies]
tracing = "0.1.34"
tracing-subscriber = "0.3.11"

```

接下来，在您的`main.rs`文件中，导入跟踪及其日志级别。

```
use tracing::{info, error, Level};
use tracing_subscriber::FmtSubscriber;

fn main() {
    let subscriber = FmtSubscriber::builder()
        .with_max_level(Level::TRACE)
        .finish();
    tracing::subscriber::set_global_default(subscriber).expect("setting default subscriber failed");
    let number_of_teams: i32 = 3;
    info!(number_of_teams, "We've got {} teams!", number_of_teams);
}

```

在上面的代码中，我们构建了一个订阅者，它记录了`tracing`事件的格式化表示，并将级别设置为`TRACE`，这将捕获关于应用程序行为的所有细节，并启用错误、警告、信息和调试级别。

结果应该是这样的:

```
2022-05-04T23:30:02.401492Z  INFO tracing_examples: We've got 3 teams! number_of_teams=3

```

您可以使用这个[跟踪遥测箱](https://crates.io/crates/tracing-opentelemetry)轻松地与 OpenTelemetry 集成。使用跟踪还可以做更多的事情。查看文档以获取更多信息和示例。

## Rust 日志和跟踪库的替代方案

[Slog](https://crates.io/crates/slog) 和 log 一样，是 Rust 的一个可扩展的日志库。它旨在成为标准原木板条箱的超集。然而，与原木板条箱不同的是，它更难使用。下面是一个如何登录终端的例子；代码改编自文档。

```
#[macro_use]
extern crate slog;
extern crate slog_term;
extern crate slog_async;

use slog::Drain;

fn main() {
    let decorator = slog_term::TermDecorator::new().build();
    let drain = slog_term::FullFormat::new(decorator).build().fuse();
    let drain = slog_async::Async::new(drain).build().fuse();

    let _log = slog::Logger::root(drain, o!());
}

```

正如您所看到的，这并不像标准的原木板条箱那么简单，因为在 Rust 生态系统中没有公开可用的跟踪替代方案。如果您认识任何人，请在评论部分与我们分享。

## 结论

没有软件是完美的。没有人编写代码并让它运行的意图是一切都将永远工作，你应该预料到一些意想不到的事情会发生。您应该设置日志记录和跟踪来帮助您监控您的应用程序，跟踪 bug，并在您的用户对您大吼大叫之前修复它们。

## [log rocket](https://lp.logrocket.com/blg/rust-signup):Rust 应用的 web 前端的全面可见性

调试 Rust 应用程序可能很困难，尤其是当用户遇到难以重现的问题时。如果您对监控和跟踪 Rust 应用程序的性能、自动显示错误、跟踪缓慢的网络请求和加载时间感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/rust-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/rust-signup)

LogRocket 就像是网络和移动应用程序的 DVR，记录你的 Rust 应用程序上发生的一切。您可以汇总并报告问题发生时应用程序的状态，而不是猜测问题发生的原因。LogRocket 还可以监控应用的性能，报告客户端 CPU 负载、客户端内存使用等指标。

现代化调试 Rust 应用的方式— [开始免费监控](https://lp.logrocket.com/blg/rust-signup)。