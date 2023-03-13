# 使用 TypeRunner 进行类型检查

> 原文：<https://blog.logrocket.com/type-checking-typerunner/>

[TypeRunner](https://github.com/marcj/TypeRunner) 是一个高性能的类型脚本编译器，支持类型检查，完全不需要`tsc`或 JavaScript 引擎。它通过将 TypeScript 源代码编译成字节码并在定制的虚拟机中运行，极大地加快了类型检查的速度。

通过这种方式，TypeRunner 使 TypeScript 语言在声明类型信息方面变得强大，然后使用这些定义在其他语言中执行类型检查——完全独立于 Node.js、Deno 或 JavaScript 引擎。TypeRunner 使以 TypeScript 编写的类型信息可以移植到其他环境中。

在撰写本文时，TypeRunner 只是一个概念验证，仅支持一些基本的类型表达式(即原语、对象文字、泛型函数类型、模板文字、类型别名、数组、元组、交集、联合和 rest 参数)。尚不支持接口或类。该包的作者 Marc J. Schmidt 在回购中建议，当社区提供足够的资金时，开发将会继续。

在本文中，我们将深入探讨 TypeRunner 背后的概念，演示如何使用 TypeRunner 对一个简单的项目进行类型检查，并将其功能与 Deno 和`tsc`进行比较。我们还将了解一项高级功能及其当前的局限性。

我们开始吧！

*向前跳转:*

## 了解性能问题

在过去的几年里，用 JavaScript 编写的被广泛采用的 web 开发工具受到了用更高性能语言编写的工具的激烈竞争，比如 Go 和 Rust。一些突出的例子是用 Go 编写的 esbuild，以及用 Rust 编写的 swc 或 [Turbopack](https://blog.logrocket.com/introducing-turbopack-rust-based-successor-webpack/) 。

现在，随着`tsc`在幕后运行 JavaScript 进行类型检查，开发大型代码库的开发人员在编辑代码时会遇到来自编译器的延迟反馈。

想象一下，改变一个函数参数的类型，然后等待几秒钟，红色的错误线就会出现！这不是我们要找的 DX！

由于 TypeScript 语言服务器在大型代码库中的低性能，公司和开发人员正在寻找解决方案。一名开发人员最近在关于 [TypeRunner repo](https://github.com/marcj/TypeRunner) 的 GitHub 问题中非常明确地表达了这一需求:

> “我愿意花钱买一个速度更快的 VS Code language 服务器，我相信成千上万的开发人员也会这样做，即使它不能与 TypeScript 100%对等。”

## 引入 TypeRunner

TypeScript 项目中定义的类型在开发过程中为开发人员提供帮助，但在运行前会被删除。然而，类型也在运行时为软件提供价值。

例如，TypeScript 接口可以用于 HTTP 端点的请求/响应验证。或者，假设我们用 TypeScript 接口定义了一个整体数据模型，并将这些类型声明为许多不同消费者(如微服务或前端应用程序)的单一真实来源。

使用 TypeRunner，在 TypeScript 中定义的数据模型的使用者不一定需要用 TypeScript 编写。TypeRunner 可以独立于语言添加类型检查，因为它不依赖于`tsc`或 JavaScript 引擎。

TypeRunner 提取 TypeScript 代码的类型信息，编译成字节码，在自定义虚拟机中执行，进行类型检查。

TypeRunner 旨在满足两个主要目标:

*   使 TypeScript 的类型信息在其他语言中可用
*   提高类型检查的速度

***注意，*** TypeRunner 在运行时不执行类型检查，但其作者也是 [Deepkit Type](https://deepkit.io/library/type) 的开发者，该库使类型在运行时可用，以便将 JSON 反序列化为类(反之亦然)，根据接口验证对象或基于接口编写类型保护。如果您正在寻找 TypeScript 的类型信息的运行时用法，我强烈建议您查看一下

### 类型的可移植性

数据模型通常用特定于领域的语言(DSL)来声明，通常只有很少的用例以及有限的工具。基于同一数据模型的服务必须根据模型中声明的类型和关系来验证它们的代码和特性。不同服务的 API 需要遵守一定的契约，以便一起正常工作。健壮的系统必须从运行时由于无效数据导致的错误中恢复。

现在，假设我们能够用 TypeScript 接口、类型以及它们之间的关系来声明一个系统的数据模型。嗯，我们可以从今天开始做！但是为了使用这些信息，我们必须依赖 JavaScript 和 TypeScript 编译器。毕竟，对于用完全不同的语言编写但依赖于相同数据模型的服务来说，这种类型的信息可能是有用的。

但是，使用 TypeRunner，可以将声明的 TypeScript 数据模型编译成字节码，移植到任何地方，并通过 CLI 或作为库(例如，在 CI/CD 管道中)用于类型检查。

## TypeRunner 的用例

TypeRunner 有几个真实的用例。以下是一些例子:

*   非 JavaScript 环境中的类型检查
*   用 TypeScript 接口和类型替换用于数据建模的 DSL
*   提高类型检查性能

## 运行中的 TypeRunner

截至本文撰写之时，TypeRunner 只支持 TypeScript 的类型系统的一个子集，但我们仍然可以很好地利用它！

为了实际演示 TypeRunner，让我们从定义一个简单的用户数据模型开始。

在构建 TypeRunner 之前，我们将首先定义模型。

这是因为 Docker 容器(我们将在其中看到 TypeRunner 执行类型检查)不提供基于终端的文本编辑器。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

模型(在这种情况下，我们的 TypeScript 源代码)将作为卷添加到容器中。

### 克隆 TypeRunner repo

[TypeRunner repo](https://github.com/marcj/TypeRunner) 包含构建 Docker 映像的 C++源代码和指令。作为提示，您需要递归地克隆 repo，如果您已经设置了 GitHub SSH 密钥，这只能在命令行上工作:

```
git clone [email protected]:marcj/TypeRunner.git
cd TypeRunner
git submodule update --init --recursive

```

### 建立码头工人形象

如果你还没有在你的系统上安装它，你可以[在这里](https://www.docker.com/get-started)得到 Docker。

我们将把 TypeRunner 构建到 Docker 映像中，并运行装载了 TypeScript 源文件的一次性容器，以便进行类型检查:

```
docker build -t typerunner .

```

这里我们标记图像，`typerunner,`,我们将在运行图像外的容器时引用它。

***注意，*** 如果我们在交互模式下运行容器，我们可以检查《TypeRunner》的作者 [Marc J. Schmidt](https://twitter.com/MarcJSchmidt) 用来做基准测试的测试文件

```
docker run -it typerunner
cat tests/model.ts

```

### 使用 TypeRunner 进行类型检查(没有`tsc)`的类型检查)

TypeRunner 允许您在没有 TypeScript 编译器(也称为`tsc`)的情况下进行类型检查。一旦 C++源代码在我们用 Docker 设置的环境中被编译，任何类型脚本代码都可以通过挂载到 Docker 容器中进行类型检查。

让我们创建一个文件`main.ts`，并用 TypeScript 编写一个基本的 HTTP 处理程序。然而，我们不会看到 VS 代码或任何其他内置于 TypeScript 语言服务器的文本编辑器。相反，我们将让 TypeRunner 来做繁重的工作！😉

```
// main.ts
type Phone = {
  country: "US" | "FR" | "UK" | "DE";
  phoneArea: number;
  phoneNumber: number;
};

type User = {
  id: string;
  name: string;
  age: number;
  contact: Phone;
};

const lindasPhone: Phone = {
  country: "FS",
  phoneArea: 123,
  phoneNumber: 4567,
};

const users: User[] = [
  {
    id: "abc",
    name: "Linda",
    age: 23,
    contact: lindasPhone,
  },
  {
    id: "def",
    name: "Wendy",
    age: 32,
    contact: {
      country: "UK",
      phoneArea: 123,
      phoneNumber: "4567",
    },
  },
];

```

这段代码显然有一些类型错误。这是国际性的，所以我们可以看到像 TypeRunner、Deno 和`tsc`这样的类型检查工具如何帮助检测和修复这些错误。

我鼓励您尝试使用 TypeRunner 并添加一些额外的类型错误，以便获得实践经验！

要使用 TypeRunner 对示例代码进行类型检查，请运行以下命令:

```
# Type check main.ts with TypeRunner
docker run -v $(pwd)/main.ts:/typerunner/main_test.ts typerunner build/typescript_main main_test.ts

```

***注意，*** `build/typescript_main`是执行类型检查的 TypeRunner 可执行文件；`main_test.ts`是我们如何命名我们挂载的源文件`main.ts`

现在，让我们用 Deno 检查我们的`main.ts`文件并比较输出。

### 使用 Deno 进行类型检查

Deno 可用于通过命令行对 TypeScript 文件进行类型检查:

```
deno check main.ts

# does the same as
npx tsc --noEmit main.ts

```

Deno 使用`tsc`进行类型检查，因此 TypeRunner 与 Deno 和`tsc`相比具有相同的性能优势。

在运行`deno check main.ts`时，Deno 在我们的代码中发现了两个类型错误，并返回一个用曲线和行号表示的错误的结构化概述。本质上，运行`deno check` 和运行`npx tsc –noEmit` 是一样的

***注意，*** 如果没有通过`--check`标志显式启用类型检查，Deno 将在没有类型检查的情况下运行您的源代码，而不管潜在的类型错误

```
# No type checking: This only compiles with tsc and runs in Deno
deno run main.ts

# Demand type checking
deno run --check main.ts

```

### 输出比较:TypeRunner vs. Deno 和`tsc`

TypeRunner 将类型检查的结果输出到`stdout`，所以您会在终端中看到红色的曲线(表示错误):

```
# [.... other logs ….]

main_test.ts:188:199 - error TS0000: Type '{"country": "FS""phoneArea": 123"phoneNumber": 4567}' is not assignable to type '{"country": "US" | "FR" | "UK" | "DE""phoneArea": number"phoneNumber": number}'

»const lindasPhone: Phone =
»      ~~~~~~~~~~~

main_test.ts:277:282 - error TS0000: Type '[{"id": "abc""name": "Linda""age": 23"contact": {"country": "US" | "FR" | "UK" | "DE""phoneArea": number"phoneNumber": number}}, {"id": "def""name": "Wendy""age": 32"contact": {"country": "UK""phoneArea": 123"phoneNumber": "4567"}}]' is not assignable to type 'Array<{"id": string"name": string"age": number"contact": {"country": "US" | "FR" | "UK" | "DE""phoneArea": number"phoneNumber": number}}>'

»const users: User[] = [ // no squiggly lines from Deno/tsc
»      ~~~~~

Found 2 errors in main_test.ts

```

为了比较，下面是用 Deno ( `tsc`)进行类型检查的输出:

```
# Output from Deno / tsc

error: TS2322 [ERROR]: Type '"FS"' is not assignable to type '"US" | "FR" | "UK" | "DE"'.
  country: "FS",
  ~~~~~~~
    at file:///home/christian/projects/typerunner-demo-app/main.ts:15:3

    The expected type comes from property 'country' which is declared here on type 'Phone'
      country: "US" | "FR" | "UK" | "DE";
      ~~~~~~~
        at file:///home/christian/projects/typerunner-demo-app/main.ts:2:3

TS2322 [ERROR]: Type 'string' is not assignable to type 'number'.
      phoneNumber: "4567", // no squiggly lines from TypeRunner
      ~~~~~~~~~~~
    at file:///home/christian/projects/typerunner-demo-app/main.ts:34:7

    The expected type comes from property 'phoneNumber' which is declared here on type 'Phone'
      phoneNumber: number;
      ~~~~~~~~~~~
        at file:///home/christian/projects/typerunner-demo-app/main.ts:4:3

Found 2 errors.

```

主要的区别是代码中的格式和曲线(表示错误)的位置。

例如，看看我们的用户群。它在第二个对象中有一个错误的类型，嵌套在`contact.phoneNumber`属性中。Deno 在设置了不正确的类型(它是一个字符串而不是一个数字)的地方标记这个类型错误，而 TypeRunner 将`users`数组标记为错误。

## 处理复杂类型

[TypeRunner 自述文件](https://github.com/marcj/TypeRunner)展示了 TypeRunner 能够检查的一些复杂类型:

```
type StringToNum<T extends string, A extends 0[] = []> = `${A['length']}` extends T ? A['length'] : StringToNum<T, [...A, 0]>;
const var1: StringToNum<'999'> = 999;

```

这个特殊的`StringToNum`类型可能实际上并不相关，但是它很好地演示了复杂类型是如何被 TypeRunner 构造和类型检查的。

## 当前的限制

在使用 TypeRunner 时，我偶然发现了一些与源代码中的表达式相关的当前限制，在编写时 TypeRunner 无法处理这些限制。

### 支持导入语句

TypeRunner 目前不支持处理 import 语句，所以我们不能使用 Deno 标准库中的 HTTP 处理程序和服务器，正如我所希望的:

```
// TypeRunner cannot process import statements at the moment
import { serve } from "https://deno.land/[email protected]/http/server.ts";
import { type Handler } from "https://deno.land/[email protected]/http/server.ts";

```

### Deno 全局名称空间的使用

目前无法使用 TypeRunner 从 Deno 全局名称空间`Deno`启动 HTTP 服务器:

```
// TypeRunner cannot process references to the Deno global namespace
const conn = Deno.listen({ port: 80 });

// TypeRunner's output
// terminate called after throwing an instance of 'std::runtime_error'
//  what():  Property access to Never not supported

```

## TypeRunner 的优势

与类似于`tsc`的工具相比，TypeRunner 提供了巨大的性能优势，因为它将 TypeScript 编译成字节码，并在一个定制的虚拟机中进行处理，而且是用 C++编写的。

作为一种静态类型的通用编译语言，C++提供了比 JavaScript 等解释型语言更好的性能。C++的性能与 Rust 等更年轻的编译语言不相上下。TypeRunner 作者出于个人偏好明确选择 C++而不是 Rust:

> [为什么 C++不会生锈？](https://github.com/marcj/TypeRunner#why-c-and-not-rust)因为我比 Rust 懂 C++多了。优秀 C++开发人员的市场要大得多。TypeScript 代码还可以很好地映射到 C++，因此移植扫描器、解析器和 AST 结构实际上相当容易，这使得将 TypeScript tsc 的特性回移植到 TypeRunner 更加容易。我也觉得铁锈很丑。”

TypeRunner 通过高效缓存解析后的 AST 和编译后的字节码(即热运行)进一步提高了性能。如果您有一个包含数百个文件的大型项目，并且在每次类型检查运行之间只有一两个更改，那么只有有更改的文件需要经过 TypeRunner 处理的所有三个阶段:

1.  解析 AST
2.  编译成字节码
3.  在定制虚拟机中执行字节码

## 结论

在本文中，我们探讨了 TypeRunner 的概念、特性、优点和局限性。我们还演示了它在实际中的使用，并将其性能与 Deno 和 `tsc`进行了比较。

在撰写本文时，TypeRunner 项目仅仅是一个概念验证，因此它缺乏流畅的用户体验，并且不支持 TypeScript 语言的每个方面。

我打赌开源社区未来会做出贡献来增强 TypeRunner 的可用性，所以在它的基础上构建并发布消息吧！我迫不及待地想看到 TypeRunner 获得牵引力，并看到它的采用飙升！

## [LogRocket](https://lp.logrocket.com/blg/typescript-signup) :全面了解您的网络和移动应用

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/typescript-signup)

LogRocket 是一个前端应用程序监控解决方案，可以让您回放问题，就像问题发生在您自己的浏览器中一样。LogRocket 不需要猜测错误发生的原因，也不需要向用户询问截图和日志转储，而是让您重放会话以快速了解哪里出错了。它可以与任何应用程序完美配合，不管是什么框架，并且有插件可以记录来自 Redux、Vuex 和@ngrx/store 的额外上下文。

除了记录 Redux 操作和状态，LogRocket 还记录控制台日志、JavaScript 错误、堆栈跟踪、带有头+正文的网络请求/响应、浏览器元数据和自定义日志。它还使用 DOM 来记录页面上的 HTML 和 CSS，甚至为最复杂的单页面和移动应用程序重新创建像素级完美视频。

[Try it for free](https://lp.logrocket.com/blg/typescript-signup)

.