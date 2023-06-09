# Kotlin lambda 表达式完全指南

> 原文：<https://blog.logrocket.com/a-complete-guide-to-kotlin-lambda-expressions/>

科特林到处都是兰姆达。我们在代码中看到它们。他们在文档和博客文章中被提及。如果不快速接触 lambdas 的概念，很难写作、阅读或学习 Kotlin。

但是 lambdas 到底是什么？

如果你是这门语言的新手，或者没有仔细研究过 lambdas 本身，这个概念有时可能会令人困惑。

在本帖中，我们将深入探讨科特林的兰姆达斯。我们将探索它们是什么，它们是如何构成的，以及它们可以用在哪里。在这篇文章结束时，你应该对 Kotlin 中的 lambda 有一个完整的理解——以及如何在任何类型的 Kotlin 开发中实用地使用它们。

## 什么是科特林拉姆达？

先说形式定义。

Lambdas 是一种类型的*函数文字*，这意味着它们是一个没有使用`fun`关键字定义的函数，并且可以立即作为表达式的一部分使用。

因为 lambdas 没有使用`fun`关键字命名或声明，所以我们可以轻松地将它们赋给变量或作为函数参数传递。

## 科特林兰姆达斯的例子

让我们看几个例子来帮助说明这个定义。下面的代码片段演示了在变量赋值表达式中使用两种不同的 lambdas。

```
val lambda1 = { println("Hello Lambdas") }
val lambda2 : (String) -> Unit = { name: String -> 
    println("My name is $name") 
}
```

对于这两种情况，等号右边的一切都是λ。

让我们看另一个例子。这个片段演示了 lambda 作为函数参数的用法。

```
// create a filtered list of even values
val vals = listOf(1, 2, 3, 4, 5, 6).filter { num ->
    num.mod(2) == 0
}
```

在这种情况下，调用`.filter`之后的所有内容都是 lambda。

有时，lambda 可能会令人困惑，因为它们可以以不同的方式编写和使用，这使得很难理解某个东西是否是 lambda。这方面的一个例子可以在下面的代码片段中看到:

```
val vals = listOf(1, 2, 3, 4, 5, 6).filter({ it.mod(2) == 0 })
```

此示例显示了上一个示例的替代版本。在这两种情况下，lambda 都被传递给`filter()`函数。在这篇文章中，我们将讨论这些差异背后的原因。

## 什么是科特林拉姆达不是

既然我们已经看到了几个 lambdas *是*的例子，那么列举几个 lambdas *不是*的例子可能会有所帮助。

Lambdas 不是类或函数体。看看下面的类定义。

```
class Person(val firstName: String, val lastName: String) {
    private val fullName = "$firstName $lastName"

    fun printFullName() {
        println(fullName)
    }
}
```

在这段代码中，有两组看起来非常像 lambdas 的花括号。类体包含在一组`{ }`中，`printFullName()`方法的实现包含一组`{ }`中的方法体。

虽然这些看起来像兰姆达斯，但它们不是。我们将继续探索更详细的解释，但基本的解释是，这些实例中的花括号并不代表函数表达式；它们只是语言基本语法的一部分。

这是 lambda 不是什么的最后一个例子。

```
val greeting = if(name.isNullOrBlank()) {
    "Hello you!"
} else {
    "Hello $name"
}
```

在这段代码中，我们再次使用了两组花括号。但是，条件语句的主体不代表函数，所以它们不是 lambdas。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

现在我们已经看到了一些例子，让我们仔细看看 lambda 的正式语法。

## 理解基本 lambda 语法

我们已经看到 lambdas 可以用几种不同的方式来表达。然而，所有的 lambda 都遵循一组特定的规则，作为 Kotlin 的 [lambda 表达式语法](https://kotlinlang.org/docs/lambdas.html#lambda-expression-syntax)的一部分。

该语法包括以下规则:

*   Lambdas 总是用花括号括起来
*   如果 lambda 的返回类型不是`Unit`，那么 lambda 主体的最终表达式将被视为返回值
*   参数声明放在花括号内，可能有可选的类型注释
*   如果只有一个参数，可以使用隐式的`it`引用在 lambda 主体中访问它
*   参数声明和 lambda 主体必须用一个`->`隔开

虽然这些规则确实概述了如何编写和使用 lambda，但如果没有例子，它们本身就可能令人困惑。让我们看一些说明这个 lambda 表达式语法的代码。

## 声明简单的 lambdas

我们能定义的最简单的λ应该是这样的。

```
val simpleLambda : () -> Unit = { println("Hello") }
```

在这种情况下，`simpleLambda`是一个不接受参数并返回`Unit`的函数。因为没有要声明的参数类型，返回值可以从 lambda 主体中推断出来，所以我们可以进一步简化这个 lambda。

```
val simpleLambda = { println("Hello") }
```

现在我们依靠 Kotlin 的类型推理机来推断`simpleLambda`是一个不带参数并返回`Unit`的函数。通过 lambda 主体的最后一个表达式，即对`println()`的调用，返回`Unit`，可以推断出`Unit`的返回。

## 声明复杂的 lambdas

下面的代码片段定义了一个 lambda，它接受两个`String`参数并返回一个`String`。

```
val lambda : (String, String) -> String = { first: String, last: String -> 
    "My name is $first $last"
}
```

这个 lambda 是冗长的。它包括所有可选的类型信息。第一个*参数*和最后一个*参数*都包含它们的显式类型信息。该变量还明确定义了 lambda 所表达的函数的类型信息。

这个例子可以用几种不同的方式来简化。下面的代码显示了两种不同的方式，通过依赖类型推断，lambda 的类型信息可能变得不太明确。

```
val lambda2 = { first: String, last: String -> 
    "My name is $first $last"
}
val lambda3 : (String, String) -> String = { first, last -> 
    "My name is $first $last"
}
```

在`lambda2`的例子中，类型信息是从 lambda 本身推断出来的。参数值用`String`类型显式注释，而最终表达式可以推断为返回一个`String`。

对于`lambda3`，变量包含类型信息。因此，lambda 的参数声明可以省略显式类型注释；`first`和`last`都将被推断为`String`类型。

## 调用 lambda 表达式

一旦定义了 lambda 表达式，如何调用函数来实际运行 lambda 主体中定义的代码？

与 Kotlin 中的大多数东西一样，我们有多种方式来调用 lambda。看看下面的例子。

```
val lambda = { greeting: String, name: String -> 
    println("$greeting $name")
}

fun main() {
    lambda("Hello", "Kotlin")
    lambda.invoke("Hello", "Kotlin")
}

// output
Hello Kotlin
Hello Kotlin
```

在这个代码片段中，我们定义了一个 lambda，它将接受两个`Strings`并打印一个问候。我们能够以两种方式调用 lambda。

在第一个例子中，我们调用 lambda，就像调用一个命名函数一样。我们给变量`name`添加括号，并传递适当的参数。

在第二个例子中，我们使用了一个对函数类型`invoke()`可用的特殊方法。

在这两种情况下，我们得到相同的输出。虽然您可以使用任何一个选项来调用 lambda，但是直接调用 lambda 而不使用`invoke()`会产生更少的代码，并且更清楚地传达调用已定义函数的语义。

## 从 lambda 返回值

在上一节中，我们简要介绍了 lambda 表达式的返回值。我们演示了 lambda 的返回值是由 lambda 主体中的最后一个表达式提供的。无论是返回有意义的值还是返回`Unit`时都是如此。

但是如果你想在你的 lambda 表达式中有多个 return 语句呢？这在编写普通函数或方法时并不少见；lambdas 是否支持这种相同的多重回报概念？

是的，但是这不像在 lambda 中添加多个 return 语句那么简单。

让我们看看 lambda 表达式中多重返回的显而易见的实现。

```
val lambda = { greeting: String, name: String -> 
    if(greeting.length < 3) return // error: return not allowed here

    println("$greeting $name")
}
```

在一个普通的函数中，如果我们想提前返回，我们可以添加一个`return`在函数运行完成之前返回。然而，对于 lambda 表达式，以这种方式添加一个`return`会导致编译器错误。

为了达到预期的结果，我们必须使用所谓的限定回报。在下面的代码片段中，我们更新了前面的例子来利用这个概念。

```
val lambda = [email protected] { greeting: String, name: String -> 
    if(greeting.length < 3) [email protected]

    println("$greeting $name")
}
```

这段代码中有两个关键的变化。首先，我们通过在第一个花括号前添加`[[email protected]](/cdn-cgi/l/email-protection)`来标记我们的 lambda。其次，我们现在可以引用这个标签，并使用它从我们的 lambda 返回到外部的调用函数。现在，如果`greeting < 3`是`true`，我们将提前从 lambda 返回，并且不打印任何内容。

你可能已经注意到这个例子没有返回任何有意义的值。如果我们想返回一个`String`而不是打印一个`String`呢？这个有条件回报的概念还适用吗？

同样，答案是肯定的。当制作我们的标签`return`时，我们可以提供一个显式的返回值。

```
val lambda = [email protected] { greeting: String, name: String -> 
    if(greeting.length < 3) [email protected] ""

    "$greeting $name"
}
```

如果我们需要两个以上的回报，同样的概念也适用。

```
val lambda = [email protected] { greeting: String, name: String -> 
    if(greeting.length < 3) [email protected] ""
    if(greeting.length < 6) [email protected] "Welcome!"

    "$greeting $name"
}

```

注意，虽然我们现在有多个`return`语句，但是我们仍然没有使用一个明确的`return`作为最终值。这很重要。如果我们在 lambda 表达式体的最后一行添加了一个`return`，我们会得到一个编译器错误。最终返回值必须总是隐式返回。

## 使用 lambda 参数

我们现在已经看到了 lambda 表达式中参数的许多用法。lambdas 编写方式的灵活性很大程度上来自于使用参数的规则。

## 声明 lambda 参数

让我们从简单的案例开始。如果我们不需要向我们的 lambda 传递任何东西，那么我们就不需要为 lambda 定义任何参数，如下面的代码片段所示。

```
val lambda = { println("Hello") }
```

现在，假设我们要向这个 lambda 传递一个问候。我们需要定义一个单独的`String`参数:

```
val lambda = { greeting: String -> println("Hello") }
```

请注意，我们的 lambda 在几个方面发生了变化。我们现在已经在花括号中定义了一个`greeting`参数，并定义了一个`->`操作符来分隔参数声明和 lambda 的主体。

因为我们的变量包含参数的类型信息，所以我们的 lambda 表达式可以简化。

```
val lambda: (String) -> Unit = { greeting -> println("Hello") }
```

lambda 中的`greeting`参数不需要指定`String`的类型，因为它是从变量赋值的左边推断出来的。

您可能已经注意到，我们根本没有使用这个`greeting`参数。这种情况有时会发生。我们可能需要定义一个 lambda 来接受一个参数，但是因为我们不使用它，所以我们想忽略它，节省我们的代码并从我们的心理模型中移除一些复杂性。

要忽略或隐藏未使用的`greeting`参数，我们可以做几件事。在这里，我们通过完全移除来隐藏它。

```
val lambda: (String) -> Unit = { println("Hello") }
```

现在，仅仅因为 lambda 本身没有声明或命名参数，并不意味着它不是函数签名的一部分。要调用`lambda`，我们仍然需要向函数传递一个`String`。

```
fun main() {
    lambda("Hello")
}
```

如果我们想忽略这个参数，但仍然包含它，这样就可以更清楚地知道有信息传递给 lambda 调用，我们还有另一个选择。我们可以用下划线代替未使用的 lambda 参数的名称。

```
val lambda: (String) -> Unit = { _ -> println("Hello") }
```

虽然这在用于简单参数时看起来有点奇怪，但在有多个参数需要考虑时却非常有用。

## 访问 lambda 参数

我们如何访问和使用传递给 lambda 调用的参数值？让我们回到前面的一个例子。

```
val lambda: (String) -> Unit = { println("Hello") }
```

我们如何更新我们的 lambda 来使用将要传递给它的`String`?为此，我们可以声明一个命名的`String`参数并直接使用它。

```
val lambda: (String) -> Unit = { greeting -> println(greeting) }
```

现在，我们的 lambda 将打印传递给它的任何内容。

```
fun main() {
    lambda("Hello")
    lambda("Welcome!")
    lambda("Greetings")
}
```

虽然这个 lambda 非常容易阅读，但它可能比一些人想写的更冗长。因为 lambda 只有一个参数，并且可以推断出该参数的类型，所以我们可以使用名称`it`来引用传递的`String`值。

```
val lambda: (String) -> Unit = {  println(it) }
```

您可能已经看到 Kotlin 代码引用了一些没有明确声明的`it`参数。这在科特林是常见的做法。当非常清楚参数值代表什么时，使用`it`。在许多情况下，即使使用隐式`it`代码更少，最好还是命名 lambda 参数，这样代码更容易被读者理解。

## 使用多个 lambda 参数

到目前为止，我们的示例使用了传递给 lambda 的单个参数值。但是如果我们有多个参数呢？

谢天谢地，大部分相同的规则仍然适用。让我们更新一下我们的例子，让它同时包含一个`greeting`和一个`thingToGreet`。

```
val lambda: (String, String) -> Unit = { greeting, thingToGreet -> 
    println("$greeting $thingToGreet") 
}
```

我们可以命名两个参数并在 lambda 中访问它们，就像使用单个参数一样。

如果我们想忽略一个或两个参数，我们必须依赖下划线命名约定。对于多个参数，我们不能省略参数声明。

```
val lambda: (String, String) -> Unit = { _, _ -> 
    println("Hello there!")
}
```

如果我们只想忽略其中一个参数，我们可以自由地将命名参数与强调的命名约定混合使用。

```
val lambda: (String, String) -> Unit = { _, thingToGreet -> 
    println("Hello $thingToGreet") 
}
```

## 用 lambda 参数进行析构

析构让我们将一个对象分解成[个独立的变量，代表来自原始对象的数据片段](https://kotlinlang.org/docs/destructuring-declarations.html)。这在某些情况下非常有用，比如从一个`Map`条目中提取`key`和`value`。

对于 lambdas，当我们的参数类型支持时，我们使用杠杆析构。

```
val lambda: (Pair<String, Int>) -> Unit = { pair -> 
    println("key:${pair.first} - value:${pair.second}")
}

fun main() {
    lambda("id123" to 5)
}

// output
// key:id123 - value:5
```

我们将一个`Pair<String, Int>`作为参数传递给我们的 lambda，在这个 lambda 中，我们必须首先通过引用`Pair`来访问该对的`first`和`second`属性。

通过析构，我们可以定义两个参数:一个用于属性`first`,一个用于属性`second`,而不是声明一个参数来表示传递的`Pair<String, Int>`。

```
val lambda: (Pair<String, Int>) -> Unit = { (key, value) -> 
    println("key:$key - value:$value")
}

fun main() {
    lambda("id123" to 5)
}

// output
// key:id123 - value:5
```

这让我们可以直接访问`key`和`value`，这样可以节省代码，也可以减少一些心理上的复杂性。当我们只关心底层数据时，不必引用包含对象就少了一件需要考虑的事情。

要了解更多关于析构的规则，无论是变量还是 lambdas，请查看官方文档。

## 访问闭包数据

我们现在已经看到了如何处理直接传递给我们的 lambdas 的值。然而，lambda 也可以从其定义之外访问数据。

Lambdas 可以从其作用域之外访问数据和函数。这个来自外部作用域的信息是 lambda 的*闭包*。lambda 可以调用函数、更新变量，并根据需要使用这些信息。

在下面的例子中，lambda 访问一个顶级属性`currentStudentName`。

```
var currentStudentName: String? = null

val lambda = { 
    val nameToPrint = currentStudentName ?: "Our Favorite Student"
    println("Welcome $nameToPrint")
}

fun main() {
    lambda() // output: Welcome Our Favorite Student
    currentStudentName = "Nate"
    lambda() // output: Welcome Nate
}
```

在这种情况下，两次调用`lambda()`会产生不同的输出。这是因为每次调用都将使用`currentStudentName`的当前值。

## 将 lambdas 作为函数参数传递

到目前为止，我们一直将 lambdas 赋给变量，然后直接调用这些函数。但是如果我们需要将 lambda 作为另一个函数的参数传递呢？

在下面的例子中，我们定义了一个名为`processLangauges`的高阶函数。

```
fun processLanguages(languages: List<String>, action: (String) -> Unit) {
    languages.forEach(action)
}

fun main() {
    val languages = listOf("Kotlin", "Java", "Swift", "Dart", "Rust")
    val action = { language: String -> println("Hello $language") }

    processLanguages(languages, action)
}
```

`processLanguages`函数接受一个`List<String>`和一个函数参数，函数参数本身接受一个`String`并返回`Unit`。

我们已经为我们的`action`变量分配了一个 lambda，然后在调用`processLanguages`时将`action`作为参数传递。

这个例子演示了我们可以将一个存储 lambda 的变量传递给另一个函数。

但是如果我们不想先给变量赋值呢？我们能把一个 lambda 直接传递给另一个函数吗？是的，这是惯例。

下面的代码片段更新了我们之前的例子，将 lambda 直接传递给`processLanguages`函数。

```
fun processLanguages(languages: List<String>, action: (String) -> Unit) {
    languages.forEach(action)
}

fun main() {
    val languages = listOf("Kotlin", "Java", "Swift", "Dart", "Rust")
    processLanguages(languages, { language: String -> println("Hello $language") })
}
```

你会看到我们不再有`action`变量。我们在 lambda 作为参数传递给函数调用时定义了它。

这有一个问题。由此产生的对`processLanguages`的调用很难读懂。在函数调用的括号中定义一个 lambda 会给我们的大脑带来很多语法上的干扰，让我们在阅读代码时去分析。

为了帮助解决这个问题，Kotlin 支持一种特殊的语法，称为[尾随 lambda 语法](https://kotlinlang.org/docs/lambdas.html#passing-trailing-lambdas)。这个语法声明，如果一个函数的最后一个参数是另一个函数，那么 lambda 可以在函数调用括号的之外*传递。*

实际上是什么样的呢？这里有一个例子:

```
fun main() {
    val languages = listOf("Kotlin", "Java", "Swift", "Dart", "Rust")
    processLanguages(languages) { language -> 
        println("Hello $language") 
    }
}
```

注意到对`processLanguages`的调用现在只有一个值传递给括号，但是现在在括号后面有一个 lambda。

这种结尾 lambda 语法的使用在 Kotlin 标准库中非常常见。

看看下面的例子。

```
fun main() {
    val languages = listOf("Kotlin", "Java", "Swift", "Dart", "Rust")

    languages.forEach { println(it) }
    languages
        .filter { it.startsWith("K")}
        .map { it.capitalize() }
        .forEach { println(it) }
}
```

对`forEach`、`map`、*和* `filter`的每一个调用都利用了这个尾随的 lambda 语法，使我们能够在括号之外传递 lambda。

如果没有这个语法，这个例子看起来会更像这样。

```
fun main() {
    val languages = listOf("Kotlin", "Java", "Swift", "Dart", "Rust")

    languages.forEach({ println(it) })
    languages
        .filter({ it.startsWith("K")})
        .map({ it.capitalize() })
        .forEach({ println(it) })
}
```

虽然这段代码在功能上与前面的示例相同，但是随着括号和花括号的增加，它开始看起来复杂得多。因此，一般来说，将 lambdas 传递给函数括号外的函数可以提高 Kotlin 代码的可读性。

## 在 Kotlin 中使用 lambdas 进行 SAM 转换

我们一直在探索用 lambdas 来表达 Kotlin 中的函数类型。我们可以利用 lambdas 的另一种方式是在执行单一访问方法(或 SAM)转换时。

## 什么是 SAM 转换？

如果你需要用一个抽象方法提供一个接口的实例，SAM 转换让我们用一个 lambda 来表示这个接口，而不必实例化一个新的类实例来实现这个接口。

请考虑以下情况。

```
interface Greeter {
    fun greet(item: String)
}

fun greetLanguages(languages: List<String>, greeter: Greeter) {
    languages.forEach { greeter.greet(it) }
}

fun main() {
    val languages = listOf("Kotlin", "Java", "Swift", "Dart", "Rust")

    greetLanguages(languages, object : Greeter {
        override fun greet(item: String) {
            println("Hello $item")
        }
    })
}
```

`greetLanguages`函数接受一个`Greeter`接口的实例。为了满足需求，我们创建了一个匿名类来实现`Greeter`并定义我们的`greet`行为。

这很好，但是有一些缺点。它要求我们声明并实例化一个新类。语法冗长，很难理解函数调用。

借助 SAM 转换，我们可以简化这一过程。

```
fun interface Greeter {
    fun greet(item: String)
}

fun greetLanguages(languages: List<String>, greeter: Greeter) {
    languages.forEach { greeter.greet(it) }
}

fun main() {
    val languages = listOf("Kotlin", "Java", "Swift", "Dart", "Rust")

    greetLanguages(languages) { println("Hello $it") }
}
```

注意，现在对`greetLanguages`的调用更容易阅读了。没有冗长的语法，也没有匿名类。这里的 lambda 现在执行 SAM 转换来表示`Greeter`类型。

还要注意对`Greeter`界面的更改。我们在界面中添加了`fun`关键字。这将该接口标记为一个函数接口，如果您试图添加多个公共抽象方法，该函数接口将给出编译器错误。这就是为这些功能接口实现简单 SAM 转换的神奇之处。

如果你用一个公共的抽象方法创建一个接口，考虑把它变成一个函数接口，这样你就可以在处理这个类型的时候利用 lambdas。

## 结论

希望这些例子有助于阐明 lambdas 是什么，如何定义它们，以及如何使用它们来使您的 Kotlin 代码更具表现力和可理解性。

## LogRocket :即时重现你的安卓应用中的问题。

[![](img/b5ae4bd0ecde7aa9d5288746416d5e18.png)](https://lp.logrocket.com/blg/kotlin-signup)

[LogRocket](https://lp.logrocket.com/blg/kotlin-signup) 是一款 Android 监控解决方案，可以帮助您即时重现问题，确定 bug 的优先级，并了解您的 Android 应用程序的性能。

LogRocket 还可以向你展示用户是如何与你的应用程序互动的，从而帮助你提高转化率和产品使用率。LogRocket 的产品分析功能揭示了用户不完成特定流程或不采用新功能的原因。

开始主动监控您的 Android 应用程序— [免费试用 LogRocket】。](hhttps://lp.logrocket.com/blg/kotlin-signup)