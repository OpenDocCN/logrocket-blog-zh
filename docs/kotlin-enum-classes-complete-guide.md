# Kotlin 中的枚举类完整指南

> 原文：<https://blog.logrocket.com/kotlin-enum-classes-complete-guide/>

编程时一个有用的特性是能够指出一个变量只有有限的可能值。为了实现这一点，大多数编程语言都引入了枚举的概念。

虽然枚举通常只代表一个预定义常量值的命名列表，但 Kotlin 枚举远不止于此。事实上，它们是真正的类，而不是简单的类型或有限的数据结构。

这意味着它们可以拥有自定义属性和方法、实现接口、使用匿名类等等。因此，Kotlin 枚举类在语言中起着至关重要的作用。

此外，使用枚举使您的代码可读性更好，更不容易出错。这就是为什么每个 Kotlin 开发人员都应该知道如何使用它们。所以，让我们深入 enum 类，看看掌握它们需要学习的一切。

## Kotlin 枚举类与 Java 枚举类型

在 [Java](https://blog.logrocket.com/using-kotlin-data-classes-to-eliminate-java-pojo-boilerplates/) 中，枚举是类型。具体来说，[官方文档](https://docs.oracle.com/javase/tutorial/java/javaOO/enum.html)将 enum 类型定义为“一种特殊的数据类型，它使变量成为一组预定义的常量。”这意味着上述变量必须等于预定义值之一。这些值是常数，表示枚举类型的属性。

尽管是一个类型，Java `enum`声明实际上在幕后创建了一个类。因此，Java 枚举可以包含自定义方法和属性。除了编译器自动添加的缺省值之外。就是这样 Java 枚举类型不能再做什么了。

与 Java 中发生的情况不同， [Kotlin 枚举](https://kotlinlang.org/docs/enum-classes.html)本身就是类，而不仅仅是在幕后。这就是为什么它们被称为枚举类，而不是 Java 枚举类型。这使得开发人员不能像在 Java 中可能发生的那样，仅仅将它们视为常量的集合。

正如我们将要看到的，科特林枚举远不止这些。它们不仅可以使用匿名类，还可以实现接口，就像任何其他 Kotlin 类一样。所以，让我们忘记 Java 枚举类型，开始钻研 Kotlin 枚举类。

## 科特林枚举的基本特征

让我们开始探索 Kotlin 枚举提供的最常见的特性。

### 定义枚举

Kotlin 枚举类最基本的用例是将它们视为常量集合。在这种情况下，它们被称为类型安全枚举，可以定义如下:

```
enum class Day {   
    MONDAY, 
    TUESDAY,
    WEDNESDAY, 
    THURSDAY, 
    FRIDAY, 
    SATURDAY,
    SUNDAY
}

```

如您所见，`enum`关键字后面是`class`关键字。这应该可以防止你被愚弄，以为 Kotlin 枚举仅仅是类型。

然后是枚举类名。最后，在 enum 类的主体中，可能的逗号分隔选项称为 enum 常量。请注意，因为它们是常量，所以它们的名称应该总是大写字母。这就是最简单的 Kotlin 枚举类的组成。

### 初始化枚举

Kotlin 枚举是类，这意味着它们可以有一个或多个构造函数。因此，您可以通过将所需的值传递给一个有效的构造函数来初始化枚举常量。这是可能的，因为枚举常量只不过是枚举类本身的实例。
让我们通过一个例子来看看这是如何工作的:

```
enum class Day(val dayOfWeek: Int) {    
    MONDAY(1), 
    TUESDAY(2),
    WEDNESDAY(3), 
    THURSDAY(4), 
    FRIDAY(5), 
    SATURDAY(6),
    SUNDAY(7)
}

```

这样，每个枚举常量都与一周中某一天的相对数字相关联。

通常，基于构造函数的方法用于为枚举常量提供有用的信息或有意义的值。最常见的一种情况是为它们提供一个自定义的`printableName`属性。这在打印它们时非常有用，可以通过以下方式实现:

```
enum class Day(val printableName: String) {    
    MONDAY("Monday"), 
    TUESDAY("Tuesday"),
    WEDNESDAY("Wednesday"), 
    THURSDAY("Thursday"), 
    FRIDAY("Friday"), 
    SATURDAY("Saturday"),
    SUNDAY("Sunday")
}

```

### 内置属性

Kotlin 枚举类带有一些内置属性。就像 Java 中发生的一样，它们被编译器自动添加到每个枚举类中。因此，您可以在任何枚举类实例中访问它们。让我们看看他们:

1.  [`ordinal`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-enum/ordinal.html)
    `ordinal`允许您检索当前枚举常数在列表中出现的位置。它是一个从零开始的索引，这意味着选项列表中的第一个常量的值为`0`，第二个常量的值为`1`，依此类推。当实现`Comparable`接口时，该属性将用于排序逻辑。
2.  [`name`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-enum/name.html)
    `name`以字符串形式返回枚举常量的名称。

让我们通过下面的例子来看看这两者的作用:

```
enum class Day(val dayOfWeek: Int) {
    MONDAY(1), 
    TUESDAY(2),
    WEDNESDAY(3), 
    THURSDAY(4), 
    FRIDAY(5), 
    SATURDAY(6),
    SUNDAY(7)
}

fun main() {    
    for (day in DAY.values())
        println("[${day.ordinal}] -> ${day.name} (${day.dayOfWeek}^ day of the week)")
}

```

通过运行此代码，您将获得以下结果:

```
[0] -> MONDAY (1^ day of the week)
[1] -> TUESDAY (2^ day of the week)
[2] -> WEDNESDAY (3^ day of the week)
[3] -> THURSDAY (4^ day of the week)
[4] -> FRIDAY (5^ day of the week)
[5] -> SATURDAY (6^ day of the week)
[6] -> SUNDAY (7^ day of the week)

```

正如您所看到的，由`name`内置属性返回的字符串与常量本身一致。这使得它们不太容易打印，这就是为什么添加一个自定义的`printableName`属性可能是有用的。

此外，这个例子强调了为什么添加一个自定义索引也是必需的。这是因为`ordinal`是一个从零开始的索引，在编程时使用，而不是提供信息内容。

## Kotlin 枚举的高级功能

现在，是时候深入研究 Kotlin enum 类提供的最高级和最复杂的特性了。

### 添加自定义属性和方法

自定义属性和方法可以添加到 enum 类中，就像在任何其他 Kotlin 类中一样。改变的是语法，它必须遵循特定的规则。

特别是，方法和属性必须添加到枚举常量定义的下方，分号之后。就像 Kotlin 中的任何其他属性一样，您可以为它们提供一个默认值。另外，枚举类可以有实例和静态方法:

```
enum class Day {
    MONDAY(1, "Monday"),
    TUESDAY(2, "Tuesday"),
    WEDNESDAY(3, "Wednesday"),
    THURSDAY(4, "Thursday"),
    FRIDAY(5, "Friday"),
    SATURDAY(6, "Saturday"),
    SUNDAY(7, "Sunday"); // end of the constants

    // custom properties with default values
    var dayOfWeek: Int? = null
    var printableName : String? = null

    constructor()

    // custom constructors
    constructor(
        dayOfWeek: Int,
        printableName: String
    ) {
        this.dayOfWeek = dayOfWeek
        this.printableName = printableName
    }

    // custom method
    fun customToString(): String {
        return "[${dayOfWeek}] -> $printableName"
    }
}

```

在此示例中，向 enum 类添加了一个自定义构造函数、两个自定义属性和一个自定义实例方法。可以通过实例访问属性和方法，实例是枚举常量，语法如下:

```
// accessing the dayOfWeek property
Day.MONDAY.dayOfWeek

// accessing the customToString() method
Day.MONDAY.customToString()

```

请记住，Kotlin 没有静态方法的概念。然而，通过利用由 Kotlin 枚举类支持的[伴随对象](https://kotlinlang.org/docs/object-declarations.html#companion-objects)，可以获得相同的结果。在伴随对象中定义的方法不依赖于特定的实例，可以静态访问。您可以添加一个，如下所示:

```
enum class Day {
    MONDAY,
    TUESDAY,
    WEDNESDAY,
    THURSDAY,
    FRIDAY,
    SATURDAY,
    SUNDAY;

    companion object {
        fun getNumberOfDays() = values().size
    }
}

```

现在，您可以这样访问`getNumberOfDays()`方法:

```
Day.getNumberOfDays()

```

如您所见，该方法是在类上静态调用的，不依赖于任何实例。注意，在实现它时使用了合成静态方法`values()`。你很快就会看到它是什么以及如何使用它。

### 使用匿名类定义枚举常量

我们可以创建匿名类来定义特定的枚举常量。与 Java 相反，Kotlin 枚举类支持匿名类。

特别是，枚举常量可以由匿名类实例化。他们只需要给 enum 类本身的抽象方法一个实现。这可以通过以下语法实现:

```
enum class Day {
    MONDAY {
        override fun nextDay() = TUESDAY
    },
    TUESDAY {
        override fun nextDay() = WEDNESDAY
    },
    WEDNESDAY {
        override fun nextDay() = THURSDAY
    },
    THURSDAY {
        override fun nextDay() = FRIDAY
    },
    FRIDAY {
        override fun nextDay() = SATURDAY
    },
    SATURDAY {
        override fun nextDay() = SUNDAY
    },
    SUNDAY {
        override fun nextDay() = MONDAY
    };

    abstract fun nextDay(): Day
}

```

如此处所示，每个枚举常量都通过声明自己的匿名类来实例化，同时覆盖所需的抽象方法。这就像发生在任何其他 Kotlin 匿名类中一样。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

### 枚举可以实现接口

尽管 Kotlin 枚举类不能从类、枚举类或抽象类派生，但它们实际上可以实现一个或多个接口。

在这种情况下，每个枚举常量必须提供接口方法的实现。这可以通过一个常见的实现来实现，如下所示:

```
interface IDay {
    fun firstDay(): Day
}

enum class Day: IDay {
    MONDAY,
    TUESDAY,
    WEDNESDAY,
    THURSDAY,
    FRIDAY,
    SATURDAY,
    SUNDAY;

    override fun firstDay(): Day {
      return MONDAY  
    } 
}

```

或者使用匿名类，如前所示:

```
interface IDay {
    fun nextDay(): Day
}

enum class Day: IDay {
    MONDAY {
        override fun nextDay() = TUESDAY
    },
    TUESDAY {
        override fun nextDay() = WEDNESDAY
    },
    WEDNESDAY {
        override fun nextDay() = THURSDAY
    },
    THURSDAY {
        override fun nextDay() = FRIDAY
    },
    FRIDAY {
        override fun nextDay() = SATURDAY
    },
    SATURDAY {
        override fun nextDay() = SUNDAY
    },
    SUNDAY {
        override fun nextDay() = MONDAY
    };
}

```

在这两种情况下，每个枚举常量都实现了`IDay`接口方法。

## 枚举在行动

现在您已经看到了基本和高级特性，您已经具备了开始使用 Kotlin enum 类所需的一切。让我们通过三个最常见的用例来看看它们的实际应用。

### 枚举和`when`

当与 Kotlin 的`[when](https://kotlinlang.org/docs/control-flow.html#when-expression)`条件语句一起使用时，枚举类特别有用。这是因为`when`表达式必须考虑每个可能的条件。换句话说，它们必须详尽无遗。

由于枚举根据定义提供了一组有限的值，Kotlin 可以用它来判断是否考虑了所有条件。否则，编译时将抛出一个错误。让我们看看 enums 在使用`when`表达式时的表现:

```
enum class Day {
    MONDAY,
    TUESDAY,
    WEDNESDAY,
    THURSDAY,
    FRIDAY,
    SATURDAY,
    SUNDAY
}

fun main (currentDay: Day) {
    when (currentDay) {
        Day.MONDAY -> work()
        Day.TUESDAY -> work()
        Day.WEDNESDAY -> work()
        Day.THURSDAY -> work()
        Day.FRIDAY -> work()
        Day.SATURDAY -> rest()
        Day.SUNDAY -> rest()
    }
}

fun work() {
    println("Working")
}

fun rest() {
    println("Resting")
}

```

正如刚刚展示的，枚举允许您根据它们的值来区分逻辑。它们还使您的代码更具可读性，更不容易出错。这是因为它们确定了在`when`语句中要考虑的可能选项的最大数量。这样，你就不会忘记。

### Enums 和 Kotlin 合成方法

类似于前面提到的内置属性，每个枚举类也有合成方法。它们是 Kotlin 在编译时自动添加的，代表可以静态访问的实用函数。让我们看看最重要的几个以及如何使用它们:

*   `values()`
    返回枚举类中包含的所有枚举常量的列表。
*   `valueOf(value: String)`
    返回枚举常量，其`name`属性与作为参数传递的值字符串相匹配。如果没有找到，就会抛出一个`[IllegalArgumentException](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-illegal-argument-exception/)`。

让我们通过一个例子来看看它们的作用:

```
enum class Day(val printableName: String) {
    MONDAY("Monday"),
    TUESDAY("Tuesday"),
    WEDNESDAY("Wednesday"),
    THURSDAY("Thursday"),
    FRIDAY("Friday"),
    SATURDAY("Saturday"),
    SUNDAY("Sunday")
}

fun main () {
    for (day in Day.values())        
        println(day.printableName)

   println(Day.valueOf("MONDAY").printableName)
}

```

运行时，将打印以下结果:

```
Monday
Tuesday
Wednesday
Thursday
Friday
Saturday
Sunday
Monday

```

注意，通过使用以下两个 Kotlin 全局函数可以获得相同的结果:`[enumValues<T>()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/enum-values.html)`和 [`enumValueOf<T>()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/enum-value-of.html) 。它们允许你用基于泛型的方法[访问任何枚举类`T`。](https://kotlinlang.org/docs/generics.html)

### 遍历枚举

最后，由于使用了`values()`合成方法，两个用例可以组合起来遍历它们，并使用`when`表达式根据它们的值执行不同的动作。让我们看一个基于这种方法的例子:

```
enum class Day(val printableName: String) {
    MONDAY("Monday"),
    TUESDAY("Tuesday"),
    WEDNESDAY("Wednesday"),
    THURSDAY("Thursday"),
    FRIDAY("Friday"),
    SATURDAY("Saturday"),
    SUNDAY("Sunday")
}

fun main () {
    for (day in Day.values()) {
        // common behavior
        println(day.printableName)

        // action execute based on day value
        when (day) {
            Day.MONDAY -> work()
            Day.TUESDAY -> work()
            Day.WEDNESDAY -> work()
            Day.THURSDAY -> work()
            Day.FRIDAY -> work()
            Day.SATURDAY -> rest()
            Day.SUNDAY -> rest()
        }

        // common behavior
        println("---")
    }
}

fun work() {
    println("Working")
}

fun rest() {
    println("Resting")
}

```

这样，可以基于 enum 类包含的每个当前可能值来执行自定义逻辑。如果启动，该代码片段将返回以下内容:

```
Monday
Working
---
Tuesday
Working
---
Wednesday
Working
---
Thursday
Working
---
Friday
Working
---
Saturday
Resting
---
Sunday
Resting
---

```

## 结论

在本文中，我们研究了什么是 Kotlin enum 类，何时以及如何使用它们，以及为什么。如图所示，Kotlin 枚举具有许多功能，并为您提供了无限的可能性。因此，与许多其他编程语言中发生的情况相反，简单地将它们视为一组常数将是错误的。

由于 Kotlin 枚举是类，它们可以有自己的属性、方法和实现接口。此外，如果使用正确，它们可以使您的代码更清晰，更易读，更不容易出错。这就是为什么每个 Kotlin 开发人员都应该使用它们，并且教授正确使用它们所需的一切正是本文所要讨论的。

感谢阅读！我希望这篇文章对你有所帮助。如果有任何问题、意见或建议，请随时联系我。

## LogRocket :即时重现你的安卓应用中的问题。

[![](img/b5ae4bd0ecde7aa9d5288746416d5e18.png)](https://lp.logrocket.com/blg/kotlin-signup)

[LogRocket](https://lp.logrocket.com/blg/kotlin-signup) 是一款 Android 监控解决方案，可以帮助您即时重现问题，确定 bug 的优先级，并了解您的 Android 应用程序的性能。

LogRocket 还可以向你展示用户是如何与你的应用程序互动的，从而帮助你提高转化率和产品使用率。LogRocket 的产品分析功能揭示了用户不完成特定流程或不采用新功能的原因。

开始主动监控您的 Android 应用程序— [免费试用 LogRocket】。](hhttps://lp.logrocket.com/blg/kotlin-signup)