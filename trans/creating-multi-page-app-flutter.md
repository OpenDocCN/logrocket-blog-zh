# 在 Flutter - LogRocket 博客中创建多页应用程序

> 原文：<https://blog.logrocket.com/creating-multi-page-app-flutter/>

Flutter 是由 Google 开发的开源 SDK，有助于从单一代码库为移动、web、桌面和嵌入式应用程序创建本机优化的应用程序。Flutter 的人气与日俱增；颤振应用现在无处不在。

说到做 app，不仅仅是开发漂亮的 UI；这也是通过将你的应用分成多个页面来提供更好的用户体验。在本文中，我们将学习使用 Flutter 的导航和多页面应用程序。

## 什么是飘动中的一页？

页面是在某个时间点可见的单个屏幕。单个页面或屏幕可以由许多小部件组成，这些小部件被组织在一起以创建所需的 UI。Flutter 中的页面/屏幕被称为路线，我们使用 Navigator 小部件在它们之间导航。

在 Flutter 中，所有东西——包括多页面应用——都是小部件。Flutter 使用方便的小部件(如 MaterialApp ),根据用户的导航和偏好显示不同的屏幕。

## 你如何浏览网页？

Navigator 小部件与 MaterialApp 捆绑在一起，管理一堆 [Route](https://api.flutter.dev/flutter/widgets/Route-class.html) 对象。您可以将 route 对象视为单个页面或屏幕的表示。这一叠最上面的路线对用户来说是可见的，当用户按下后退按钮时，最上面的路线就会弹出，露出下面的路线，就像一叠卡片一样。

## 我们走吧🚀

让我们首先创建一个 MaterialApp 小部件，它将为我们的应用程序配置顶级的[导航器](https://api.flutter.dev/flutter/widgets/Navigator-class.html)以及其他基本的东西:

```
class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Naviation Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const FirstPage(title: 'FirstPage'),
    );
  }
}

```

由于我们的应用程序有多个页面，我们将创建两个页面/屏幕，分别叫做**第一页**和**第二页**。

下面是**第一页**小部件，它由一个显示页面/屏幕标题的 AppBar 和一个显示导航到**第二页**的按钮的主体组成:

```
class FirstPage extends StatelessWidget {
  const FirstPage({Key? key, required this.title}) : super(key: key);
  final String title;
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(title),
      ),
      body: Center(
        child: TextButton(
          onPressed: () {},
          child: const Text('Next'),
        ),
      ),
    );
  }
}

```

现在，这是我们的第一页目前的样子。

![First Page](img/88148f618c7e5c270aadda9fb5a78017.png)

### 导航到第二页🚪

如果你尝试点击**下一个**按钮，什么也不会发生，因为我们还没有告诉 Flutter 当用户点击按钮时该做什么。

使用`Navigator.push()`方法切换到新路线。导航器的`push()`方法将一条路由添加到它管理的路由堆栈中。

为了导航到**第二页**，首先我们需要创建它，你不这样认为吗？🤠**第二页**将与**第一页**几乎相同，文本现在变为**返回**:

```
class SecondPage extends StatelessWidget {
  const SecondPage({Key? key, required this.title}) : super(key: key);
  final String title;
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(title),
      ),
      body: Center(
        child: TextButton(
          onPressed: () {},
          child: const Text('Go Back'),
        ),
      ),
    );
  }
}

```

既然我们的**第二页**已经构建好了，我们可以通过更新**第一页**中的`onPressed()`方法导航到它。用以下代码替换**首页**的`TextButton`中的`onPressed()`:

```
onPressed: () {
   Navigator.push(context, MaterialPageRoute(builder: (context) {
     return const SecondPage(title: 'SecondPage');
   }));
}

```

大多数时候，我们使用`MaterialPageRoute`在页面/屏幕之间导航，但有时当我们想要更多的控制来添加自定义过渡之类的东西时，我们可以使用`PageRouteBuilder`。

### 返回首页🔙

万岁！🥂:你已经成功地在 Flutter 中创建了你的第一个多页面应用程序。足够的庆祝活动；现在是时候导航回第**页第**页了。

为了往回导航，我们使用了`Navigator.pop()`方法。它从导航器的路线堆栈中删除当前路线。

在**第二页**中，用以下代码替换`onPressed()`:

```
onPressed: () {
  Navigator.pop(context);
}

```

这是我们到目前为止所做的所有努力的结果:

![Second Page](img/c27b77411e66892985a02309b30a2c82.png)

### 从第二页返回一些数据到第一页🚛

有时，您希望从导航器堆栈弹出的路线返回一些数据。假设在我们的例子中，当我们从**第二页**导航回**第一页**时，我们将返回一条消息说**从第二页**返回。

在`SecondPage`的`build()`方法中，更新`onPressed()`回调:

```
onPressed: () {
  Navigator.pop(context, "Returned from SecondPage");
}

```

现在在`FirstPage`的`build()`方法中，将`onPressed()`方法替换为:

```
onPressed: () async {
  var message = await Navigator.push(context,
   MaterialPageRoute(builder: (context) {
     return const SecondPage(title: 'SecondPage');
    }));
        // This message will be printed to the console
   print(message);
}

```

这里的`onPressed()`方法现在看起来像一个奇怪的方法，因为我们使用`async/await`关键字来等待方法`Navigator.push()`返回的`Future`。

理解它就像我们在等待在`push()`方法上使用关键字`await`，直到它被弹出并返回消息。为了使用`await`关键字，您必须使用关键字`async`使`onPressed()`方法异步。如果您需要帮助，请了解更多关于 Dart 中的[异步编程](https://dart.dev/codelabs/async-await)的信息。

运行 app，按下**第二页**上的**返回**按钮。检查控制台以查看返回的消息:

```
flutter: Returned from SecondPage

```

## 遗言

在本文中，您学习了如何使用 [Navigator 小部件](https://blog.logrocket.com/understanding-flutter-navigation-routing/)在 Flutter 中创建多页面应用程序，以及如何在弹出一条路线时返回数据。这不是结束，而是大量课程的开始，当你在学习扑动的旅程中前进时，这些课程会向你走来。我建议你去浏览一下[官方文件](https://docs.flutter.dev/development/ui/navigation)，了解更多关于导航和其他基础知识。

祝你好运！开心飘飘！👨‍💻

如果你有任何问题，请随时发表。👇

欢迎任何反馈。

## 使用 [LogRocket](https://lp.logrocket.com/blg/signup) 消除传统错误报告的干扰

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/signup)

[LogRocket](https://lp.logrocket.com/blg/signup) 是一个数字体验分析解决方案，它可以保护您免受数百个假阳性错误警报的影响，只针对几个真正重要的项目。LogRocket 会告诉您应用程序中实际影响用户的最具影响力的 bug 和 UX 问题。

然后，使用具有深层技术遥测的会话重放来确切地查看用户看到了什么以及是什么导致了问题，就像你在他们身后看一样。

LogRocket 自动聚合客户端错误、JS 异常、前端性能指标和用户交互。然后 LogRocket 使用机器学习来告诉你哪些问题正在影响大多数用户，并提供你需要修复它的上下文。

关注重要的 bug—[今天就试试 LogRocket】。](https://lp.logrocket.com/blg/signup-issue-free)