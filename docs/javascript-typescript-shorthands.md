# 18 JavaScript 和 type script short hands to know-log rocket 博客

> 原文：<https://blog.logrocket.com/javascript-typescript-shorthands/>

***编者按:*** *这本最有用的 JavaScript 和 TypeScript 简写指南最近一次更新是在 2023 年 1 月 3 日，目的是解决代码中的错误，并包括关于 TypeScript v4.9 中引入的满足运算符的信息。*

JavaScript 和 TypeScript 共享了许多有用的常用代码概念的简写方法。简写代码替代方案可以帮助减少代码行数，这是我们通常努力追求的目标。

在本文中，我们将回顾 18 种常见的 [JavaScript](https://blog.logrocket.com/tag/vanilla-javascript/) 和[打字稿](https://blog.logrocket.com/tag/typescript/)和速记。我们还将探索如何使用这些速记的例子。

通读这些有用的 JavaScript 和 TypeScript 简写，或者在下面的列表中找到您想要的。

*向前跳转:*

## javascript 和打字稿速记

当[编写干净且可伸缩的代码](https://blog.logrocket.com/12-tips-for-writing-clean-and-scalable-javascript-3ffe30abfe20/)时，使用速记代码并不总是正确的决定。简洁的代码有时会更难阅读和更新。因此，重要的是你的代码清晰易读，并向其他开发人员传达意义和上下文。

我们使用速记的决定不能损害其他期望的代码特征。在 JavaScript 和 TypeScript 中对[表达式和操作符使用以下缩写时，请记住这一点。](https://blog.logrocket.com/a-comprehensive-guide-to-javascript-expressions/)

JavaScript 中所有可用的简写在 TypeScript 中都有相同的语法。唯一的细微差别是在 TypeScript 中指定类型，并且 TypeScript 构造函数速记是 TypeScript 独有的。

## 三元运算符

三元运算符是 JavaScript 和 TypeScript 中最流行的缩写之一。它取代了传统的`if…else`语句。其语法如下:

```
[condition] ? [true result] : [false result]

```

下面的例子演示了一个传统的`if…else`语句及其使用三元运算符的等效简写:

```
// Longhand

const mark = 80

if (mark >= 65) {
  return "Pass"
} else {
  return "Fail"
}

// Shorthand
const mark = 80

return mark >= 65 ? "Pass" : "Fail"

```

> 当有单行操作时，三元运算符非常有用，比如给变量赋值或基于两个可能的条件返回值。一旦你的情况有两种以上的结果，使用`if/else`块会更容易阅读。

## 短路评估

替换`if…else`语句的另一种方法是短路评估。当预期值为 falsy 时，这种简化使用逻辑 OR 运算符`||`为变量分配默认值。

以下示例演示了如何使用短路评估:

```
// Longhand
let str = ''
let finalStr

if (str !== null && str !== undefined && str != '') {
  finalStr = str
} else {
  finalStr = 'default string'
}

// Shorthand
let str = ''
let finalStr = str || 'default string' // 'default string

```

> 当您有单行操作，并且您的条件取决于值/语句的假或非假时，最好使用这种简写。

## 看涨凝聚算子

[无效合并操作符](https://blog.logrocket.com/optional-chaining-and-nullish-coalescing-in-javascript/) `??`类似于短路评估，它为变量分配一个默认值。但是，nullish 合并运算符仅在预期值也为 nullish 时使用默认值。

换句话说，如果想要的值是 falsy 而不是 nullish，它将不会使用默认值。

以下是零化合并运算符的两个示例:

### 例子一

```
// Longhand
let str = ''
let finalStr

if (str !== null && str !== undefined) {
  finalStr = 'default string'
} else {
  finalStr = str
}

// Shorthand
let str = ''
let finaStr = str ?? 'default string' // ''

```

### 例子二

```
// Longhand
let num = null
let actualNum

if (num !== null && num !== undefined) {
  actualNum = num
} else {
  actualNum = 0
}

// Shorthand
let num = null
let actualNum = num ?? 0 // 0

```

## 逻辑零赋值运算符

这类似于 nullish 合并运算符，通过检查值是否为 null，并增加了在 null 检查后赋值的功能。

下面的例子演示了我们如何使用逻辑 nullish 赋值进行手写和速记检查和赋值:

```
// Longhand
let num = null

if (num === null) {
  num = 0
}

// shorthand
let num = null

num ??= 0

```

> JavaScript 还有其他几种赋值简写，比如加法赋值`+=`，乘法赋值`*=`，除法赋值`/=`，余数赋值`%=`，以及其他几种。你可以在这里找到赋值操作符[的完整列表](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators)。

## 模板文字

模板文字是作为 JavaScript 强大的 ES6 特性的一部分引入的，可以用来代替`+`在一个字符串中连接多个变量。要使用模板文字，将字符串包装在````中，将变量包装在这些字符串内的`${}`中。

下面的示例演示了如何使用模板文字来执行字符串插值:

```
// Longhand
const name = 'Iby'
const hobby = 'to read'

const fullStr = name + ' loves ' + hobby // 'Iby loves to read'

// Shorthand
const name = 'Iby'
const hobby = 'to read'

const fullStr = `${name} loves ${hobby}` // 'Iby loves to read'

```

不使用`\n`，也可以使用模板文字构建多行字符串。例如:

```
// Shorthand
const name = 'Iby'
const hobby = 'to read'
const fullStr = `${name} loves ${hobby}.
She also loves to write!` 

```

使用模板文字有助于添加其值可能会变成更大字符串的字符串，如 HTML 模板。它们对于创建多行字符串也很有用，因为包装在模板文字中字符串保留了所有空白和缩进。

## 对象属性分配简写

在 JavaScript 和 TypeScript 中，您可以通过在对象文字中提及变量来以速记法将属性分配给对象。要做到这一点，变量必须用想要的键来命名。

请参阅下面的对象属性分配简写示例:

```
// Longhand
const obj = {
  x: 1,
  y: 2,
  z: 3
}

```

```
// Shorthand
const x = 8
const y = 10
const obj = { x, y }

```

## 可选链接

点符号允许我们访问对象的键或值。使用[可选链接](https://blog.logrocket.com/optional-chaining-and-nullish-coalescing-in-javascript/)，我们可以更进一步，读取键或值，即使我们不确定它们是否存在或已设置。

当密钥不存在时，可选链接的值为`undefined`。这有助于我们避免从对象中读取值时不必要的`if/else`检查条件，以及处理试图访问不存在的对象键时抛出的错误的不必要的`try/catch`。

请参见下面的可选链接示例:

```
// Longhand
const obj = {
  x: {
    y: 1,
    z: 2
  },
  others: [
    'test',
    'tested'
  ] 
}

if (obj.hasProperty('others') && others.length >= 2) {
  console.log('2nd value in others: ', obj.others[1])
}

```

```
// Shorthand
const obj = {
  x: {
    y: 1,
    z: 2
  },
  others: [
    'test',
    'tested'
  ] 
}

console.log('2nd value in others: ', obj.others?.[1]) // 'tested'
console.log('3rd value in others: ', obj.others?.[2]) // undefined

```

## 对象析构

除了传统的点标记法，另一种读取对象值的方法是将对象的值析构为它们自己的变量。

下面的示例演示了如何使用传统的点标记法读取对象的值，并与使用对象析构的简写方法进行了比较:

```
// Longhand
const obj = {
  x: {
    y: 1,
    z: 2
  },
  other: 'test string'
}

console.log('Value of z in x: ', obj.x.z)
console.log('Value of other: ', obj.other)

// Shorthand
const obj = {
  x: {
    y: 1,
    z: 2
  },
  other: 'test string'
}

const {x, other} = obj
const {z} = x

console.log('Value of z in x: ', z)
console.log('Value of other: ', other)

You can also rename the variables you destructure from the object. Here's an example:
const obj = {x: 1, y: 2}
const {x: myVar} = object

console.log('My renamed variable: ', myVar) // My renamed variable: 1

```

## 传播算子

扩展操作符`…`用于访问数组和对象的内容。你可以使用 spread 操作符来替换[数组函数](https://blog.logrocket.com/javascript-array-methods/)，比如`concat`，以及对象函数，比如`object.assign`。

查看以下示例，了解 spread 运算符如何替换手写数组和对象函数:

```
// Longhand
const arr = [1, 2, 3]
const biggerArr = [4,5,6].concat(arr)

const smallObj = {x: 1}
const otherObj = object.assign(smallObj, {y: 2})

// Shorthand
const arr = [1, 2, 3]
const biggerArr = [...arr, 4, 5, 6]

const smallObj = {x: 1}
const otherObj = {...smallObj, y: 2}

```

## 对象循环速记

传统的 JavaScript `for`循环语法如下:

```
for (let i = 0; i < x; i++) { … }

```

我们可以使用这个循环语法，通过引用迭代器的数组长度来遍历数组。有三种`for`循环快捷方式提供了遍历数组对象的不同方法:

*   `for…of`:访问数组条目
*   `for…in`:当用于对象文字时，访问数组的索引和键
*   `Array.forEach`:使用回调函数对数组元素及其索引进行操作

请注意，`Array.forEach`回调有三个可能的参数，按以下顺序调用:

*   正在进行的迭代的数组元素
*   元素的索引
*   数组的完整副本

下面的例子演示了这些对象循环的实际操作:

```
// Longhand
const arr = ['Yes', 'No', 'Maybe']

for (let i = 0; i < arr.length; i++) {
  console.log('Here is item: ', arr[i])
}

// Shorthand
const arr = ['Yes', 'No', 'Maybe']

for (let str of arr) {
  console.log('Here is item: ', str)
}

arr.forEach((str) => {
  console.log('Here is item: ', str)
})

for (let index in arr) {
  console.log(`Item at index ${index} is ${arr[index]}`)
}

// For object literals
const obj = {a: 1, b: 2, c: 3}

for (let key in obj) {
  console.log(`Value at key ${key} is ${obj[key]}`)
}

```

## `Array.indexOf`使用按位运算符的速记

我们可以使用`Array.indexOf`方法在数组中查找一个项目的存在。如果该项在数组中存在，该方法返回该项的索引位置，如果不存在，则返回`-1`。

在 JavaScript 中，`0`是一个假值，而小于或大于`0`的数字被视为真值。通常，这意味着我们需要使用一个`if…else`语句，通过返回的索引来确定该项是否存在。

[使用按位运算符](https://blog.logrocket.com/interesting-use-cases-for-javascript-bitwise-operators/) `~`而不是`if…else`语句允许我们获得大于或等于`0`的真值。

下面的例子演示了使用按位运算符而不是`if…else`语句的`Array.indexOf`简写:

```
const arr = [10, 12, 14, 16]

const realNum = 10
const fakeNum = 20

const realNumIndex = arr.indexOf(realNum)
const noneNumIndex = arr.indexOf(fakeNum)

// Longhand
if (realNumIndex > -1) {
  console.log(realNum, ' exists!')
} else if (realNumIndex === -1) {
  console.log(realNum, ' does not exist!')
}

if (noneNumIndex > -1) {
  console.log(fakeNum, ' exists!')
} else if (noneNumIndex === -1) {
  console.log(fakeNum, ' does not exist!')
}

// Shorthand
const arr = [10, 12, 14, 16]

const realNum = 10
const fakeNum = 20

const realNumIndex = arr.indexOf(realNum)
const noneNumIndex = arr.indexOf(fakeNum)

console.log(realNum + (~realNumIndex ? ' exists!' : ' does not exist!')
console.log(fakeNum + (~noneNumIndex ? ' exists!' : ' does not exist!')

```

## 用`!!`将值转换为布尔值

在 JavaScript 中，我们可以使用`!![variable]`简写将任何类型的变量转换为布尔值。

查看使用`!! [variable]`简写将值转换为`Boolean`的示例:

```
// Longhand
const simpleInt = 3
const intAsBool = Boolean(simpleInt)

// Shorthand
const simpleInt = 3
const intAsBool = !!simpleInt

```

## 箭头/λ函数表达式

JavaScript 中的函数可以使用箭头函数语法来编写[，而不是显式使用`function`关键字的传统表达式。箭头函数类似于其他语言](https://blog.logrocket.com/anomalies-in-javascript-arrow-functions/)中的 [lambda 函数。](https://blog.logrocket.com/a-complete-guide-to-kotlin-lambda-expressions/)

看看这个使用箭头函数表达式用速记法编写函数的例子:

```
// Longhand
function printStr(str) {
  console.log('This is a string: ', str)
}
printStr('Girl!')

// Shorthand
const printStr = (str) => {
  console.log('This is a string: ', str)
}
printStr('Girl!')

// Shorthand TypeScript (specifying variable type)
const printStr = (str: string) => {
  console.log('This is a string: ', str)
}
printStr('Girl!')

```

## 使用箭头函数表达式的隐式返回

在 JavaScript 中，我们通常使用`return`关键字从函数中返回值。当我们使用 arrow 函数语法定义函数时，我们可以通过排除括号`{}`来隐式返回值。

对于多行语句，比如表达式，我们可以将返回表达式放在括号`()`中。下面的示例演示了使用箭头函数表达式从函数中隐式返回值的简写代码:

```
// Longhand
function capitalize(name) {
  return name.toUpperCase()
}

function add(numA, numB) {
  return numA + numB
}

// Shorthand
const capitalize = (name) => name.toUpperCase()

const add = (numA, numB) => (numA + numB)

// Shorthand TypeScript (specifying variable type)
const capitalize = (name: string) => name.toUpperCase()

const add = (numA: number, numB: number) => (numA + numB)

```

## 双位非运算符

在 JavaScript 中，我们通常使用内置的`Math`对象来访问数学函数和常数。这些功能中的一些是`Math.floor()`、`Math.round()`、`Math.trunc()`以及许多其他功能。

`Math.trunc()`(在 ES6 中可用)返回整数部分。例如，给定数字的小数之前的数字使用双位 NOT 运算符`~~`获得相同的结果。

查看以下示例，了解如何使用双位 NOT 运算符作为`Math.trunc()`简写:

```
// Longhand
const num = 4.5
const floorNum = Math.trunc(num) // 4

// Shorthand
const num = 4.5
const floorNum = ~~num // 4

```

> 值得注意的是，双位 NOT 运算符`~~`不是`Math.trunc`的正式缩写，因为一些边缘情况不会返回相同的结果。更多细节请点击[这里](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/trunc#using_bitwise_no-ops_to_truncate_numbers)。

## 指数幂速记

另一个有用的数学函数是`Math.pow()`函数。使用内置的`Math`对象的替代方法是`**`简写。

以下示例演示了这种指数幂速记的实际应用:

```
// Longhand
const num = Math.pow(3, 4) // 81

// Shorthand
const num = 3 ** 4 // 81

```

## TypeScript 构造函数速记

通过 TypeScript 中的[构造函数创建一个类并为类属性赋值有一个简单的方法。使用该方法时，TypeScript 会自动创建并设置](https://blog.logrocket.com/writing-constructor-typescript/)[类属性](https://blog.logrocket.com/when-how-use-interfaces-classes-typescript/)。这种简写是 TypeScript 独有的，在 JavaScript 类定义中不可用。

看看下面的例子，看看 TypeScript 构造函数速记是如何工作的:

```
// Longhand
class Person {
  private name: string
  public age: int
  protected hobbies: string[]

  constructor(name: string, age: int, hobbies: string[]) {
    this.name = name
    this.age = age
    this.hobbies = hobbies
  }
}

// Shorthand
class Person {
  constructor(
    private name: string,
    public age: int,
    protected hobbies: string[]
  ) {}
}

```

## TypeScript 满足运算符

满足运算符提供了一定的灵活性，不受使用具有显式类型的错误处理覆盖来设置类型的约束。

当一个值有多种可能的类型时，最好使用它。比如可以是字符串，也可以是数组；有了这个操作符，我们不需要添加任何检查。这里有一个例子:

```
// Longhand
type Colors = "red" | "green" | "blue";
type RGB = [red: number, green: number, blue: number];
const palette: Record<Colors, string | RGB> = {
    red: [255, 0, 0],
    green: "#00ff00",
    blue: [0, 0, 255]
};

if (typeof palette.red !== 'string') {
    console.log(palette.red.at(0))
}

// shorthand
type Colors = "red" | "green" | "blue";
type RGB = [red: number, green: number, blue: number];
const palette = {
    red: [255, 0, 0],
    green: "#00ff00",
    blue: [0, 0, 255]
} satisfies Record<Colors, string | RGB>;

console.log(palette.red.at(0))

```

在上面例子的手写版本中，我们必须做一个`typeof`检查来确保`palette.red`属于`type RGB`，并且我们可以用`at`读取它的第一个属性。

虽然在我们的简写版本中，使用`satisfies`，我们没有`palette.red`是`string`的类型限制，但是我们仍然可以告诉编译器确保`palette`及其属性具有正确的形状。

> `Array.property.at()`即`at()`方法接受一个整数并返回该索引处的项目。`Array.at`需要`ES2022`目标，从 TypeScript v4.6 开始提供。更多信息请点击查看[。](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/at)

## 结论

这些只是一些最常用的 JavaScript 和 TypeScript 简写。

JavaScript 和 TypeScript 手写和速记代码通常在幕后以相同的方式工作，所以选择速记通常意味着编写更少的代码行。记住，使用简写代码并不总是最好的选择。最重要的是编写清晰易懂的代码，让其他开发人员也能轻松阅读。

你最喜欢的 JavaScript 或 TypeScript 速记是什么？在评论中与我们分享吧！

## 通过理解上下文，更容易地调试 JavaScript 错误

调试代码总是一项单调乏味的任务。但是你越了解自己的错误，就越容易改正。

LogRocket 让你以新的独特的方式理解这些错误。我们的前端监控解决方案跟踪用户与您的 JavaScript 前端的互动，让您能够准确找出导致错误的用户行为。

[![LogRocket Dashboard Free Trial Banner](img/cbfed9be3defcb505e662574769a7636.png)](https://lp.logrocket.com/blg/javascript-signup)

LogRocket 记录控制台日志、页面加载时间、堆栈跟踪、慢速网络请求/响应(带有标题+正文)、浏览器元数据和自定义日志。理解您的 JavaScript 代码的影响从来没有这么简单过！

[Try it for free](https://lp.logrocket.com/blg/javascript-signup)

.