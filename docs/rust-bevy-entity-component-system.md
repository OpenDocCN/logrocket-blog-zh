# Rust Bevy 实体组件系统

> 原文：<https://blog.logrocket.com/rust-bevy-entity-component-system/>

***编者按*** : *本帖包括来自《Bevy》撰稿人[爱丽丝·塞西尔](https://www.linkedin.com/in/alice-cecile-183116a3/?originalSubdomain=ca)的补充内容。* *帮助和评论上* [*对张子华的不和谐*](https://discord.com/invite/bevy) *所带来的喜悦和逻辑大为赞赏。*

Bevy 是一个用 Rust 编写的游戏引擎，以非常符合人体工程学的实体组件系统而闻名。

在 [ECS 模式](https://bevyengine.org/learn/book/getting-started/ecs/)中，实体是由组件组成的独特事物，就像游戏世界中的物体一样。系统处理这些实体并控制应用程序的行为。是什么让 [Bevy 的 API](https://bevyengine.org) 如此优雅，用户可以在 Rust 中编写常规函数，Bevy 将知道如何通过它们的类型签名调用它们，分派正确的数据。

关于如何使用 ECS 模式构建你自己的游戏，已经有大量的文档可用，比如非官方的 Bevy Cheat Book 中的[。相反，在本文中，我们将解释如何在 Bevy 本身中实现 ECS 模式。为此，我们将从头开始构建一个小型的、类似 Bevy 的 API，它接受任意的系统函数。](https://bevy-cheatbook.github.io/programming/ecs-intro.html)

这种模式非常通用，您可以将它应用到自己的 Rust 项目中。为了说明这一点，我们将在文章的最后一节更详细地讨论 Axum web framework 如何将这种模式用于其路由处理程序方法。

如果您熟悉 Rust，并且对类型系统技巧感兴趣，那么这篇文章适合您。在我们开始之前，我建议看看我以前的一篇关于 Bevy 标签实现的文章。我们开始吧！

## 目录

## Bevy 的系统就像一个面向用户的 API

首先，让我们学习如何使用 Bevy 的 API，这样我们就可以从它开始逆向工作，自己重新创建它。下面的代码展示了一个带有示例系统的小型应用程序:

```
use bevy::prelude::*;

fn main() {
    App::new()
        .add_plugins(DefaultPlugins) // includes rendering and keyboard input
        .add_system(move_player) // this is ours
        // in a real game you'd add more systems to e.g. spawn a player
        .run();
}

#[derive(Component)]
struct Player;

/// Move player when user presses space
fn move_player(
    // Fetches a resource registered with the `App`
    keyboard: Res<Input<KeyCode>>,
    // Queries the ECS for entities
    mut player: Query<(&mut Transform,), With<Player>>,
) {
    if !keyboard.just_pressed(KeyCode::Space) { return; }

    if let Ok(player) = player.get_single_mut() {
        // destructure the `(&mut Transform,)` type from above to access transform
        let (mut player_position,) = player;
        player_position.translation.x += 1.0;
    }
}

```

在上面的代码中，我们可以将一个常规的 Rust 函数传递给`add_system`，Bevy 知道如何处理它。更好的是，我们可以使用函数参数来告诉 Bevy 我们想要查询哪些组件。在我们的例子中，我们希望每个实体都有定制的`Player`组件。在幕后，Bevy 甚至根据函数签名来推断哪些系统可以并行运行。

## `add_system`方法

Bevy 有很多 API 面。毕竟，它是一个完整的游戏引擎，除了实体组件系统之外，还有调度系统、2D 和 3D 渲染器等等。在本文中，我们将忽略其中的大部分内容，而是专注于将函数作为系统添加并调用它们。

按照 Bevy 的例子，我们将调用我们添加系统的项目，`App`，并给它两个方法，`new`和`add_system`:

```
struct App {
    systems: Vec<System>,
}

impl App {
    fn new() -> App {
        App { systems: Vec::new() }
    }

    fn add_system(&mut self, system: System) {
        self.systems.push(system);
    }
}

struct System; // What is this?

```

但是，这就引出了第一个问题。什么是系统？在 Bevy 中，我们可以用一个有一些有用参数的函数来调用这个方法，但是我们如何在自己的代码中做到这一点呢？

## 将功能添加为系统

Rust 中的[主要抽象之一是特征](https://blog.logrocket.com/rust-traits-a-deep-dive/)，类似于其他语言中的接口或类型类。我们可以定义一个 trait，然后为任意类型实现它，这样 trait 的方法就可以在这些类型上使用。让我们创建一个允许我们运行任意系统的`System`特征:

```
trait System {
    fn run(&mut self);
}

```

现在，我们的系统有了一个特征，但是要在我们的函数中实现它，我们需要使用类型系统的一些附加特性。

Rust 使用特征来抽象行为，函数自动实现一些特征，比如 [`FnMut`](https://doc.rust-lang.org/1.62.1/std/ops/trait.FnMut.html) 。我们可以为满足约束的所有类型实现特征:

让我们使用下面的代码:

```
impl<F> System for F where F: Fn() -> () {
    fn run(&mut self) {
        self(); // Yup, we're calling ourselves here
    }
}

```

如果您不习惯 Rust，这段代码可能看起来很难读懂。没关系，这不是你在日常 Rust 代码库中看到的东西。

第一行实现了所有类型的系统特征，这些类型是带有返回某些内容的参数的函数。在下面的代码行中，`run`函数获取项目本身，因为这是一个函数，所以调用它。

这个虽然管用，但是相当没用。只能调用不带参数的函数。但是，在我们深入研究这个例子之前，让我们先对它进行修改，以便能够运行它。

## 插曲:运行一个例子

我们上面对`App`的定义只是一个草稿；为了让它使用我们新的`System`特征，我们需要让它更复杂一点。

由于`System`现在是特征而不是类型，我们不能再直接存储它了。我们甚至不知道`System`有多大，因为它可能是任何东西！相反，我们需要把它放在一个指针后面，或者如 Rust 所说，把它放在一个`Box`中。不用存储实现`System`的具体东西，只存储一个指针。

这是 Rust 类型系统的一个技巧:您可以使用 trait 对象来存储实现特定特征的任意项目。

首先，我们的应用程序需要存储一个盒子列表，其中包含的东西是一个`System`。实际上，它看起来像下面的代码:

```
struct App {
    systems: Vec<Box<dyn System>>,
}

```

现在，我们的`add_system`方法也需要接受任何实现`System`特征的东西，并将其放入列表中。现在，参数类型是通用的。我们使用`S`作为实现`System`的占位符，因为 Rust 希望我们确保它对整个程序都有效，所以我们也被要求添加`'static`。

现在，让我们添加一个方法来实际运行应用程序:

```
impl App {
    fn new() -> App { // same as before
        App { systems: Vec::new() }
    }

    fn add_system<S: System + 'static>(mut self, system: S) -> Self {
        self.systems.push(Box::new(system));
        self
    }

    fn run(&mut self) {
        for system in &mut self.systems {
            system.run();
        }
    }
}

```

有了这个，我们现在可以写一个如下的小例子:

```
fn main() {
    App::new()
        .add_system(example_system)
        .run();
}

fn example_system() {
    println!("foo");
}

```

到目前为止，你可以使用[的全部代码](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=3fe777f4a178aac4568c05dd621644b6)。现在，让我们回到更复杂的系统功能的问题上来。

## 带参数的系统功能

让我们让下面的函数成为一个有效的`System`:

```
fn another_example_system(q: Query<Position>) {}

// Use this to fetch entities
struct Query<T> { output: T }

// The position of an entity in 2D space
struct Position { x: f32, y: f32 }

```

看似简单的选择是为`System`添加另一个实现，以添加带有一个参数的函数。但是，可悲的是，Rust 编译器会告诉我们有两个问题:

1.  如果我们为一个具体的函数签名添加一个实现，[两个实现将会冲突](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=851bba8bbe9b29df018b2d30c8d9f838)。按下**运行**查看错误
2.  如果我们使接受的函数通用，它将是一个[无约束类型参数](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=ddc7b3af90e6af418bc99fd9b351c9ee)

我们需要用不同的方法来解决这个问题。让我们首先为我们接受的参数引入一个特征:

```
trait SystemParam {}

impl<T> SystemParam for Query<T> {}

```

为了区分不同的`System`实现，我们可以添加类型参数，这成为其签名的一部分:

```
trait System<Params> {
    fn run(&mut self);
}

impl<F> System<()> for F where F: Fn() -> () {
    //         ^^ this is "unit", a tuple with no items
    fn run(&mut self) {
        self();
    }
}

impl<F, P1: SystemParam> System<(P1,)> for F where F: Fn(P1) -> () {
    //                             ^ this comma makes this a tuple with one item
    fn run(&mut self) {
        eprintln!("totally calling a function here");
    }
}

```

但是现在，问题变成了在所有我们接受`System`的地方，我们需要添加这个类型参数。更糟糕的是，当我们试图存储`Box<dyn System>`时，我们也必须在那里添加一个:

```
error[E0107]: missing generics for trait `System`
  --> src/main.rs:23:26
   |
23 |     systems: Vec<Box<dyn System>>,
   |                          ^^^^^^ expected 1 generic argument
…
error[E0107]: missing generics for trait `System`
  --> src/main.rs:31:42
   |
31 |     fn add_system(mut self, system: impl System + 'static) -> Self {
   |                                          ^^^^^^ expected 1 generic argument
…

```

如果您创建所有实例`System<()>`并注释掉`.add_system(another_example_system)`，我们的代码将会编译。

## 存储通用系统

现在，我们的挑战是达到以下标准:

1.  我们需要一个知道其参数的通用特征
2.  我们需要在一个列表中存储通用系统
3.  我们需要能够在迭代时调用这些系统

这是研究 Bevy 代码的好地方。函数不实现`[System](https://docs.rs/bevy/0.8.0/bevy/ecs/system/trait.System.html)`，而是实现 [`SystemParamFunction`](https://docs.rs/bevy/0.8.0/bevy/ecs/system/trait.SystemParamFunction.html) 。另外， [`add_system`](https://docs.rs/bevy/0.8.0/bevy/app/struct.App.html#method.add_system) 不带一个`impl System`，而是一个`impl` [`IntoSystemDescriptor`](https://docs.rs/bevy/0.8.0/bevy/ecs/schedule/trait.IntoSystemDescriptor.html) ，进而使用一个 [`IntoSystem`](https://docs.rs/bevy/0.8.0/bevy/ecs/system/trait.IntoSystem.html) 性状。

[`FunctionSystem`](https://docs.rs/bevy/0.8.0/bevy/ecs/system/struct.FunctionSystem.html) ，一个 struct，会实现`System`。

让我们从中获得灵感，让我们的特性再次变得简单。我们之前的代码继续作为一个叫做`SystemParamFunction`的新特性。我们还将引入一个`IntoSystem`特征，我们的`add_system`函数将接受它:

```
trait IntoSystem<Params> {
    type Output: System;

    fn into_system(self) -> Self::Output;
}

```

我们使用一个[关联类型](https://doc.rust-lang.org/1.62.1/book/ch19-03-advanced-traits.html)来定义这个转换将输出什么类型的`System`类型。

这种转换特性仍然输出一个具体的系统，但那是什么呢？魔力来了。我们添加一个将实现`System`的`FunctionSystem`结构，并且我们将添加一个创建它的`IntoSystem`实现:

```
/// A wrapper around functions that are systems
struct FunctionSystem<F, Params: SystemParam> {
    /// The system function
    system: F,
    // TODO: Do stuff with params
    params: core::marker::PhantomData<Params>,
}

/// Convert any function with only system params into a system
impl<F, Params: SystemParam + 'static> IntoSystem<Params> for F
where
    F: SystemParamFunction<Params> + 'static,
{
    type System = FunctionSystem<F, Params>;

    fn into_system(self) -> Self::System {
        FunctionSystem {
            system: self,
            params: PhantomData,
        }
    }
}

/// Function with only system params
trait SystemParamFunction<Params: SystemParam>: 'static {
    fn run(&mut self);
}

```

`SystemParamFunction`是上一章我们称之为`System`的一般特质。如你所见，我们还没有对函数参数做任何事情。我们只是保留它们，这样一切都是通用的，然后将它们存储在 [`PhantomData`](https://doc.rust-lang.org/1.62.1/core/marker/struct.PhantomData.html) 类型中。

为了满足来自`IntoSystem`的约束，即它的输出必须是一个`System`，我们现在在我们的新类型上实现 trait:

```
/// Make our function wrapper be a System
impl<F, Params: SystemParam> System for FunctionSystem<F, Params>
where
    F: SystemParamFunction<Params> + 'static,
{
    fn run(&mut self) {
        SystemParamFunction::run(&mut self.system);
    }
}

```

这样，我们就差不多准备好了！让我们更新我们的`add_system`函数，然后我们可以看到这一切是如何工作的:

```
impl App {
    fn add_system<F: IntoSystem<Params>, Params: SystemParam>(mut self, function: F) -> Self {
        self.systems.push(Box::new(function.into_system()));
        self
    }
}

```

我们的函数现在接受所有实现类型参数为`SystemParam`的`IntoSystem`的东西。

为了接受具有多个参数的系统，我们可以在本身就是系统参数的元组上实现`SystemParam`:

```
impl SystemParam for () {} // sure, a tuple with no elements counts
impl<T1: SystemParam> SystemParam for (T1,) {} // remember the comma!
impl<T1: SystemParam, T2: SystemParam> SystemParam for (T1, T2) {} // A real two-ple

```

但是，我们现在储存什么呢？实际上，我们将做与之前相同的事情:

```
struct App {
    systems: Vec<Box<dyn System>>,
}

```

让我们探索一下为什么我们的代码能够工作。

## 包装我们的泛型

诀窍是我们现在将一个泛型`FunctionSystem`存储为一个[特征对象](https://doc.rust-lang.org/1.62.1/book/ch17-02-trait-objects.html)，这意味着我们的`Box<dyn System>`是一个胖指针。它既指向内存中的`FunctionSystem`,也指向与该类型实例的`System`特征相关的所有内容的查找表。

当使用通用函数和数据类型时，编译器将[单态化](https://rustc-dev-guide.rust-lang.org/backend/monomorph.html)它们来为实际使用的类型生成代码。因此，如果您将同一个泛型函数与三个不同的具体类型一起使用，它将被编译三次。

现在，我们已经满足了所有三个标准。我们已经为泛型函数实现了我们的 trait，我们存储了一个泛型`System`盒子，我们仍然在它上面调用`run`。

## 获取参数

遗憾的是，我们的代码还不能工作。我们无法获取参数并使用它们调用系统函数。不过没关系。在`run`的实现中，我们可以只打印一行而不调用函数。这样，我们可以证明它编译并运行了一些东西。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

结果看起来有点像下面的代码:

```
fn main() {
    App::new()
        .add_system(example_system)
        .add_system(another_example_system)
        .add_system(complex_example_system)
        .run();
}

fn example_system() {
    println!("foo");
}

fn another_example_system(_q: Query<&Position>) {
    println!("bar");
}

fn complex_example_system(_q: Query<&Position>, _r: ()) {
    println!("baz");
}

   Compiling playground v0.0.1 (/playground)
    Finished dev [unoptimized + debuginfo] target(s) in 0.64s
     Running `target/debug/playground`
foo
TODO: fetching params
TODO: fetching params

```

你可以在这里找到本教程的完整代码。按下**播放**，你会看到上面的输出和更多。请随意使用它，尝试一些系统的组合，也许还可以添加一些其他的东西！

我们现在已经看到 Bevy 是如何作为系统接受相当广泛的功能的。但是正如介绍中提到的，其他库和框架也使用这种模式。

Axum web 框架就是一个例子，它允许您为特定的路由定义处理函数。下面的代码显示了来自[他们的文档](https://docs.rs/axum/0.5.13/axum/extract/index.html)的一个例子:

```
async fn create_user(Json(payload): Json<CreateUser>) { todo!() }

let app = Router::new().route("/users", post(create_user));

```

有一个接受函数的`post`函数，甚至是`async`函数，其中所有参数都是提取器，就像这里的`Json`类型。正如你所看到的，这比我们目前看到的 Bevy 所做的要复杂一些。Axum 必须考虑返回类型和如何转换，以及支持异步函数，即返回期货的函数。

但是，总的原则是一样的。 [`Handler`特征](https://docs.rs/axum/0.5.13/axum/handler/trait.Handler.html)是为函数实现的:

特征被包装在路由器上的`HashMap`中存储的 [`MethodRouter`](https://docs.rs/axum/0.5.13/axum/routing/struct.MethodRouter.html) 结构中。调用时， [`FromRequest`用于](https://github.com/tokio-rs/axum/blob/329bd5f9b4e3747d6601773895a99899169e2ba5/axum/src/handler/mod.rs#L238-L252)提取参数值，这样就可以用它们调用底层函数。这也是一个关于 Bevy 如何工作的剧透！关于 Axum 提取器如何工作的更多信息，我推荐 David Pedersen 的演讲。

## 结论

在这篇文章中，我们看了看 Bevy，一个用 Rust 编写的游戏引擎。我们探索了它的 ECS 模式，熟悉了它的 API，并通过一个例子进行了运行。最后，我们简要地看了一下 Axum web 框架中的 ECS 模式，考虑了它与 Bevy 的不同之处。

如果你想了解更多关于 Bevy 的知识，我推荐你查看一下 [`SystemParamFetch`](https://docs.rs/bevy/0.8.0/bevy/ecs/system/trait.SystemParamFetch.html#) 特征，探索如何从`World`获取参数。我希望你喜欢这篇文章，如果你遇到任何问题，一定要留下评论。编码快乐！

## [log rocket](https://lp.logrocket.com/blg/rust-signup):Rust 应用的 web 前端的全面可见性

调试 Rust 应用程序可能很困难，尤其是当用户遇到难以重现的问题时。如果您对监控和跟踪 Rust 应用程序的性能、自动显示错误、跟踪缓慢的网络请求和加载时间感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/rust-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/rust-signup)

LogRocket 就像是网络和移动应用程序的 DVR，记录你的 Rust 应用程序上发生的一切。您可以汇总并报告问题发生时应用程序的状态，而不是猜测问题发生的原因。LogRocket 还可以监控应用的性能，报告客户端 CPU 负载、客户端内存使用等指标。

现代化调试 Rust 应用的方式— [开始免费监控](https://lp.logrocket.com/blg/rust-signup)。