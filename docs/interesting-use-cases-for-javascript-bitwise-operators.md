# JavaScript 位操作符的有趣用例

> 原文：<https://blog.logrocket.com/interesting-use-cases-for-javascript-bitwise-operators/>

[JavaScript](https://blog.logrocket.com/javascript-date-libraries/) 提供了几种运算符，使得对简单值进行算术运算、赋值运算、逻辑运算、位运算等基本运算成为可能。

我们经常看到 JavaScript 代码混合了赋值操作符、算术操作符和逻辑操作符。然而，我们很少看到按位运算符被大量使用。

## JavaScript 按位运算符

1.  **`~`**——**按位非**
2.  **`&`**——**按位与**
3.  **`|`**——**按位或**
4.  **`^`**——**按位异或**
5.  **`<<`**——**左移**
6.  **`>>`**——**传号右移**
7.  **`>>>`**——**补零右移**

在本教程中，我们将看看所有的 JavaScript 位操作符，并试图理解它们是如何计算的。我们还将看看在编写简单的 JavaScript 程序时，位运算符的一些有趣的应用。这需要我们稍微了解一下 JavaScript 位操作符是如何将它们的操作数表示为*有符号的 32 位整数*的。来吧，让我们开始吧！

## 按位非(`~`)

`~`运算符是一个*一元运算符*；因此，它只需要一个操作数。`~`运算符对其操作数的每一位执行 NOT 运算。非运算的结果称为*补码*。整数的补码是通过反转整数的每一位形成的。

对于给定的整数，比如说`170`，可以使用`~`运算符计算补码，如下所示:

```
// 170 => 00000000000000000000000010101010
// --------------------------------------
//  ~ 00000000000000000000000010101010
// --------------------------------------
//  = 11111111111111111111111101010101
// --------------------------------------
//  = -171 (decimal)

console.log(~170); // -171
```

JavaScript 位操作符将它们的操作数转换成 32 位有符号整数，格式为*二进制补码*。因此，当对整数使用`~`运算符时，结果值是整数的二进制补码。整数`A`的二进制补码由`-(A + 1)`给出。

```
~170 => -(170 + 1) => -171
```

关于 JavaScript 按位运算符使用的 32 位有符号整数，有几点需要注意:

*   最高有效(最左边)位被称为*符号位*。正整数的符号位始终为`0`，负整数的符号位始终为`1`。
*   除了符号位之外，其余 31 位用于表示整数。所以能表示的最大 32 位整数是`(2^31 - 1)`，也就是`2147483647`，最小整数是`-(2^31)`，也就是`-2147483648`。
*   对于超出 32 位有符号整数范围的整数，最高有效位将被丢弃，直到该整数落入范围内。

以下是一些重要数字的 32 位序列表示:

```
           0 => 00000000000000000000000000000000
          -1 => 11111111111111111111111111111111
  2147483647 => 01111111111111111111111111111111
 -2147483648 => 10000000000000000000000000000000
```

从上面的表述可以明显看出:

```
          ~0 => -1
         ~-1 => 0
 ~2147483647 => -2147483648
~-2147483648 => 2147483647
```

### 找到索引

JavaScript 中的大多数内置对象，比如数组和字符串，都有一些有用的方法，可以用来检查数组中的某个项或字符串中的某个子串是否存在。以下是其中的一些方法:

*   `Array.indexOf()`
*   `Array.lastIndexOf()`
*   `Array.findIndex()`
*   `String.indexOf()`
*   `String.lastIndexOf()`
*   `String.search()`

这些方法都返回该项或子串的从零开始的*索引*，如果找到的话；否则，它们返回`-1`。例如:

```
const numbers = [1, 3, 5, 7, 9];

console.log(numbers.indexOf(5)); // 2
console.log(numbers.indexOf(8)); // -1
```

如果我们对找到的条目或子串的索引不感兴趣，我们可以选择使用布尔值来代替，这样对于没有找到的条目或子串，`-1`就变成了`false`，而其他所有的值都变成了`true`。这就是它的样子:

```
function foundIndex (index) {
  return Boolean(~index);
}

```

在上面的代码片段中，`~`操作符在用于`-1`时，计算结果为`0`，这是一个 falsy 值。因此，使用`Boolean()`将一个假值转换为布尔值将返回`false`。对于每隔一个索引值，返回`true`。因此，前面的代码片段可以修改如下:

```
const numbers = [1, 3, 5, 7, 9];

console.log(foundIndex(numbers.indexOf(5))); // true
console.log(foundIndex(numbers.indexOf(8))); // false
```

## 按位与(`&`)

`&`运算符对其操作数的每一对对应位执行 AND 运算。只有两位都为 1 时，`&`运算符才返回`1`；否则返回`0`。因此，AND 运算的结果相当于将每对对应的位相乘。

对于一对位，以下是 AND 运算的可能值。

```
(0 & 0) === 0     // 0 x 0 = 0
(0 & 1) === 0     // 0 x 1 = 0
(1 & 0) === 0     // 1 x 0 = 0
(1 & 1) === 1     // 1 x 1 = 1
```

### 关闭 bits

`&`运算符通常用于位屏蔽应用，以确保对于给定的位序列，某些位被关闭。这是基于以下事实:对于任何位`A`:

*   **`(A & 0 = 0)`**–该位总是被相应的`0`位关闭
*   **`(A & 1 = A)`**–该位与相应的`1`位配对时保持不变

例如，假设我们有一个 8 位整数，我们希望确保前 4 位关闭(设置为`0`)。`&`操作符可用于实现这一点，如下所示:

### 检查设置位

`&`操作符还有一些其他有用的位屏蔽应用。一个这样的应用是确定是否为给定的比特序列设置了一个或多个比特。例如，假设我们想检查第五位是否是为给定的十进制数设置的。下面是我们如何使用`&`操作符来实现这一点:

### 偶数还是奇数

在检查十进制数的设置位时使用的`&`操作符可以扩展到检查给定的十进制数是偶数还是奇数。为此，`1`被用作位屏蔽(以确定是第一位还是最右边的位被置位)。

对于整数，最低有效位(第一位或最右位)可用于确定数字是偶数还是奇数。如果最低有效位开启(设置为`1`)，则数字为奇数；否则，数字是偶数。

```
function isOdd (int) {
  return (int & 1) === 1;
}

function isEven (int) {
  return (int & 1) === 0;
}

console.log(isOdd(34)); // false
console.log(isOdd(-63)); // true
console.log(isEven(-12)); // true
console.log(isEven(199)); // false
```

### 有用的身份

在进入下一个操作符之前，这里有一些对`&`操作有用的恒等式(对于任何有符号的 32 位整数`A`):

```
(A & 0) === 0
(A & ~A) === 0
(A & A) === A
(A & -1) === A
```

## 按位或(`|`)

`|`运算符对其操作数的每一对对应位执行 or 运算。只有两位都为 0 时，`|`运算符才返回`0`；否则返回`1`。

对于一对位，以下是 or 运算的可能值:

```
(0 | 0) === 0
(0 | 1) === 1
(1 | 0) === 1
(1 | 1) === 1
```

### 打开 bits

在位屏蔽应用中，`|`操作符可用于确保位序列中的某些位开启(设为`1`)。这是基于以下事实:对于任何给定的位`A`:

*   **`(A | 0 = A)`** —该位与相应的`0`位配对时保持不变。
*   **`(A | 1 = 1)`** —该位总是由相应的`1`位开启。

例如，假设我们有一个 8 位整数，我们希望确保所有偶数位(第二、第四、第六、第八)都打开(设置为`1`)。`|`操作符可用于实现这一点，如下所示:

*   首先，创建一个位掩码，它的作用是打开一个 8 位整数的每个偶数位。该位掩码将为`0b10101010`。注意，位屏蔽的偶数位被设置为`1`，而每隔一位被设置为`0`。
*   接下来，使用 8 位整数和创建的位掩码执行`|`操作:

```
const mask = 0b10101010;

// 208 => 11010000

// (208 | mask)
// ------------
// 11010000
// | 10101010
// ------------
// = 11111010
// ------------
// = 250 (decimal)

console.log(208 | mask); // 250
```

### 有用的身份

在进入下一个操作符之前，这里有一些对`|`操作有用的恒等式(对于任何有符号的 32 位整数`A`):

```
(A | 0) === A
(A | ~A) === -1
(A | A) === A
(A | -1) === -1
```

## 按位异或(`^`)

`^`运算符对其操作数的每对对应位执行 XOR ( *异或*)运算。如果两位相同(0 或 1)，则`^`运算符返回`0`；否则返回`1`。

对于一对比特，下面是 XOR 运算的可能值。

```
(0 ^ 0) === 0
(0 ^ 1) === 1
(1 ^ 0) === 1
(1 ^ 1) === 0
```

### 切换位

在位屏蔽应用中，`^`运算符通常用于切换或翻转位序列中的某些位。这是基于以下事实:对于任何给定的位`A`:

*   当与相应的`0`位配对时，该位保持不变。
    T3`(A ^ 0 = A)`
*   当与相应的`1`位配对时，该位总是被切换。
    **`(A ^ 1 = 1)`** —如果`A`是`0`
    **`(A ^ 1 = 0)`** —如果`A`是`1`

例如，假设我们有一个 8 位整数，我们希望确保除了最低有效位(第一位)和最高有效位(第八位)之外的每一位都被切换。`^`操作符可用于实现这一点，如下所示:

*   首先，创建一个位掩码，其作用是切换 8 位整数中除最低有效位和最高有效位之外的每一位。该位掩码将为`0b01111110`。注意，要切换的位被设置为`1`，而每隔一位被设置为`0`。
*   接下来，使用 8 位整数和创建的位掩码执行`^`操作:

```
const mask = 0b01111110;

// 208 => 11010000

// (208 ^ mask)
// ------------
// 11010000
// ^ 01111110
// ------------
// = 10101110
// ------------
// = 174 (decimal)

console.log(208 ^ mask); // 174
```

### 有用的身份

在进入下一个操作符之前，这里有一些对`^`操作有用的恒等式(对于任何有符号的 32 位整数`A`):

```
(A ^ 0) === A
(A ^ ~A) === -1
(A ^ A) === 0
(A ^ -1) === ~A
```

从上面列出的恒等式可以明显看出，对`A`和`-1`的异或运算等同于对`A`的非运算。因此，以前的`foundIndex()`函数也可以写成这样:

```
function foundIndex (index) {
  return Boolean(index ^ -1);
}
```

## 左移(`<<`)

左移(`<<`)运算符接受两个操作数。第一个操作数是一个整数，而第二个操作数是第一个操作数要左移的位数。零(`0`)位从右侧移入，而移至左侧的多余位被丢弃。

例如，考虑整数`170`。假设我们想向左移动三位。我们可以如下使用`<<`运算符:

```
// 170 => 00000000000000000000000010101010

// 170 << 3
// --------------------------------------------
//    (000)00000000000000000000010101010(***)
// --------------------------------------------
//  = (***)00000000000000000000010101010(000)
// --------------------------------------------
//  = 00000000000000000000010101010000
// --------------------------------------------
//  = 1360 (decimal)

console.log(170 << 3); // 1360
```

可以使用以下 JavaScript 表达式定义左移位运算符(`<<`):

```
(A << B) => A * (2 ** B) => A * Math.pow(2, B)
```

因此，回头看前面的例子:

```
(170 << 3) => 170 * (2 ** 3) => 170 * 8 => 1360
```

### 颜色转换:RGB 到十六进制

左移(`<<`)运算符的一个非常有用的应用是将颜色从 RGB 表示转换成十六进制表示。

RGB 颜色的每个分量的颜色值在`0 - 255`之间。简单来说，每个颜色值都可以用 8 位完美表示。

```
  0 => 0b00000000 (binary) => 0x00 (hexadecimal)
255 => 0b11111111 (binary) => 0xff (hexadecimal)
```

因此，颜色本身可以由 24 位(红、绿和蓝分量各 8 位)完美地表示。从右边开始的前 8 位代表蓝色成分，接下来的 8 位代表绿色成分，之后的 8 位代表红色成分。

```
(binary) => 11111111 00100011 00010100

   (red) => 11111111 => ff => 255
 (green) => 00100011 => 23 => 35
  (blue) => 00010100 => 14 => 20

   (hex) => ff2314
```

既然我们已经了解了如何将颜色表示为 24 位序列，那么让我们看看如何从颜色的各个分量的值中合成 24 位颜色。假设我们有一个用`rgb(255, 35, 20)`表示的颜色。下面是我们如何组合这些位:

```
  (red) => 255 => 00000000 00000000 00000000 11111111
(green) =>  35 => 00000000 00000000 00000000 00100011
 (blue) =>  20 => 00000000 00000000 00000000 00010100

// Rearrange the component bits and pad with zeroes as necessary
// Use the left shift operator

  (red << 16) => 00000000 11111111 00000000 00000000
 (green << 8) => 00000000 00000000 00100011 00000000
       (blue) => 00000000 00000000 00000000 00010100

// Combine the component bits together using the OR (|) operator
// ( red << 16 | green << 8 | blue )

      00000000 11111111 00000000 00000000
    | 00000000 00000000 00100011 00000000
    | 00000000 00000000 00000000 00010100
// -----------------------------------------
      00000000 11111111 00100011 00010100
// -----------------------------------------
```

现在过程已经很清楚了，下面是一个简单的函数，它将颜色的 RGB 值作为输入数组，并根据上面的过程返回颜色的相应十六进制表示:

```
function rgbToHex ([red = 0, green = 0, blue = 0] = []) {
  return `#${(red << 16 | green << 8 | blue).toString(16)}`;
}
```

## 符号传播右移(`>>`)

符号传播右移(`>>`)运算符有两个操作数。第一个操作数是一个整数，而第二个操作数是第一个操作数要右移的位数。

已经移至右侧的多余位被丢弃，而符号位(最左侧位)的副本从左侧移入。结果，整数的符号总是被保留，因此得名*符号传播右移*。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

例如，考虑整数`170`和`-170`。假设我们想向右移动三位。我们可以如下使用`>>`操作符:

```
//  170 => 00000000000000000000000010101010
// -170 => 11111111111111111111111101010110

// 170 >> 3
// --------------------------------------------
//    (***)00000000000000000000000010101(010)
// --------------------------------------------
//  = (000)00000000000000000000000010101(***)
// --------------------------------------------
//  = 00000000000000000000000000010101
// --------------------------------------------
//  = 21 (decimal)

// -170 >> 3
// --------------------------------------------
//    (***)11111111111111111111111101010(110)
// --------------------------------------------
//  = (111)11111111111111111111111101010(***)
// --------------------------------------------
//  = 11111111111111111111111111101010
// --------------------------------------------
//  = -22 (decimal)

console.log(170 >> 3); // 21
console.log(-170 >> 3); // -22
```

符号传播右移位按位运算符(`>>`)可由以下 JavaScript 表达式描述:

```
(A >> B) => Math.floor(A / (2 ** B)) => Math.floor(A / Math.pow(2, B))
```

因此，回头看前面的例子:

```
(170 >> 3) => Math.floor(170 / (2 ** 3)) => Math.floor(170 / 8) => 21
(-170 >> 3) => Math.floor(-170 / (2 ** 3)) => Math.floor(-170 / 8) => -22
```

### 颜色提取

右移(`>>`)运算符的一个非常好的应用是从颜色中提取 RGB 颜色值。当用 RGB 表示颜色时，很容易区分红色、绿色和蓝色分量值。然而，对于用十六进制表示的颜色来说，需要花费更多的努力。

在上一节中，我们看到了从单个颜色成分(红色、绿色和蓝色)合成颜色成分的过程。如果我们把这个过程向后推，我们就能提取出颜色各个成分的值。让我们试一试。

假设我们有一种由十六进制符号`#ff2314`表示的颜色。以下是颜色的有符号 32 位表示:

```
(color) => ff2314 (hexadecimal) => 11111111 00100011 00010100 (binary)

// 32-bit representation of color
00000000 11111111 00100011 00010100
```

为了获得单独的分量，我们将根据需要将颜色比特右移 8 的倍数，直到我们获得目标分量比特，作为从右边开始的前 8 比特。由于颜色的 32 位中最高有效位是`0`，我们可以安全地使用符号传播右移(`>>`)操作符。

```
color => 00000000 11111111 00100011 00010100

// Right shift the color bits by multiples of 8
// Until the target component bits are the first 8 bits from the right

  red => color >> 16
      => 00000000 11111111 00100011 00010100 >> 16
      => 00000000 00000000 00000000 11111111

green => color >> 8
      => 00000000 11111111 00100011 00010100 >> 8
      => 00000000 00000000 11111111 00100011

 blue => color >> 0 => color
      => 00000000 11111111 00100011 00010100
```

既然我们已经将目标组件位作为从右开始的前 8 位，我们需要一种方法来屏蔽掉除前 8 位之外的所有其他位。这让我们回到 AND ( `&`)操作符。记住`&`操作符可以用来确保某些位被关闭。

让我们从创建所需的位掩码开始。看起来会像这样:

```
mask => 00000000 00000000 00000000 11111111
     => 0b11111111 (binary)
     => 0xff (hexadecimal)
```

准备好位掩码后，我们可以使用位掩码对之前右移操作的每个结果执行 AND ( `&`)操作，以提取目标组成位。

```
  red => color >> 16 & 0xff
      =>   00000000 00000000 00000000 11111111
      => & 00000000 00000000 00000000 11111111
      => = 00000000 00000000 00000000 11111111
      =>   255 (decimal)

green => color >> 8 & 0xff
      =>   00000000 00000000 11111111 00100011
      => & 00000000 00000000 00000000 11111111
      => = 00000000 00000000 00000000 00100011
      =>   35 (decimal)

 blue => color & 0xff
      =>   00000000 11111111 00100011 00010100
      => & 00000000 00000000 00000000 11111111
      => = 00000000 00000000 00000000 00010100
      =>   20 (decimal)
```

基于上面的过程，下面是一个简单的函数，它接受一个十六进制颜色字符串(有六个十六进制数字)作为输入，并返回相应的 RGB 颜色分量值数组。

```
function hexToRgb (hex) {
  hex = hex.replace(/^#?([0-9a-f]{6})$/i, '$1');
  hex = Number(`0x${hex}`);

  return [
    hex >> 16 & 0xff, // red
    hex >> 8 & 0xff,  // green
    hex & 0xff        // blue
  ];
}
```

## 补零右移(`>>>`)

零填充右移位(`>>>`)运算符的行为非常类似于符号传播右移位(`>>`)运算符。然而，关键的区别在于从左边移入的位。

顾名思义，`0`位总是从左边移入。结果，`>>>`操作符总是返回一个无符号的 32 位整数，因为得到的整数的符号位总是`0`。对于正整数，`>>`和`>>>`将总是返回相同的结果。

例如，考虑整数`170`和`-170`。假设我们想右移 3 位，我们可以如下使用`>>>`运算符:

```
//  170 => 00000000000000000000000010101010
// -170 => 11111111111111111111111101010110

// 170 >>> 3
// --------------------------------------------
//    (***)00000000000000000000000010101(010)
// --------------------------------------------
//  = (000)00000000000000000000000010101(***)
// --------------------------------------------
//  = 00000000000000000000000000010101
// --------------------------------------------
//  = 21 (decimal)

// -170 >>> 3
// --------------------------------------------
//    (***)11111111111111111111111101010(110)
// --------------------------------------------
//  = (000)11111111111111111111111101010(***)
// --------------------------------------------
//  = 00011111111111111111111111101010
// --------------------------------------------
//  = 536870890 (decimal)

console.log(170 >>> 3); // 21
console.log(-170 >>> 3); // 536870890
```

## 配置标志

在我们结束本教程之前，让我们考虑一下位操作符和位屏蔽的另一个非常常见的应用:配置标志。

假设我们有一个函数，它接受几个布尔选项，这些选项可以用来控制函数的运行方式或返回值的类型。创建该函数的一种可能方式是将所有选项作为参数传递给该函数，可能带有一些默认值，如下所示:

```
function doSomething (optA = true, optB = true, optC = false, optD = true, ...) {
  // something happens here...
}
```

当然，这不太方便。这种方法在两种情况下开始出现问题:

*   假设我们有 10 个以上的布尔选项。我们不能用那么多参数来定义函数。
*   假设我们只想为第五个和第九个选项指定不同的值，而让其他选项保留默认值。我们将需要调用函数，将默认值作为参数传递给所有其他选项，同时传递第五个和第九个选项的期望值。

解决上述问题的一种方法是为配置选项使用一个对象，如下所示:

```
const defaultOptions = {
  optA: true,
  optB: true,
  optC: false,
  optD: true,
  ...
};

function doSomething (options = defaultOptions) {
  // something happens here...
}
```

这种方法非常优雅，您很可能见过它被使用，甚至在某个时候自己也使用过。然而，使用这种方法，`options`参数将始终是一个对象，对于配置选项来说，这被认为是多余的。

如果所有选项都采用布尔值，我们可以用一个整数代替一个对象来表示选项。在这种情况下，整数的某些位将被映射到指定的选项。如果某一位开启(设置为`1`)，指定选项的值为`true`；否则，就是`false`。

我们可以用一个简单的例子来演示这种方法。假设我们有一个函数，它对包含数字的数组列表的项进行规范化，并返回规范化的数组。返回的数组可以由三个选项控制，即:

*   **Fraction:** 将数组中的每一项除以数组中的最大项
*   **Unique:** 从数组中删除重复项
*   **排序:**将数组中的项目从最低到最高排序

我们可以使用 3 位整数来表示这些选项，每一位映射到一个选项。以下代码片段显示了选项标志:

```
const LIST_FRACTION = 0x1; // (001)
const LIST_UNIQUE = 0x2;   // (010)
const LIST_SORTED = 0x4;   // (100)
```

要激活一个或多个选项，必要时可使用`|`操作符组合相应的标志。例如，我们可以创建一个激活所有选项的标志，如下所示:

```
const LIST_ALL = LIST_FRACTION | LIST_UNIQUE | LIST_SORTED; // (111)
```

同样，假设我们只希望默认激活**分数**和**排序**选项。我们可以再次使用`|`操作符，如下所示:

```
const LIST_DEFAULT = LIST_FRACTION | LIST_SORTED; // (101)
```

虽然只有三个选项看起来还不错，但当有这么多选项时，它往往会变得非常混乱，并且默认情况下需要激活其中的许多选项。在这种情况下，更好的方法是使用`^`操作符取消不需要的选项:

```
const LIST_DEFAULT = LIST_ALL ^ LIST_UNIQUE; // (101)
```

这里，我们有激活所有选项的`LIST_ALL`标志。然后，我们使用`^`操作符去激活唯一选项，根据需要激活其他选项。

现在我们已经准备好了选项标志，我们可以继续定义`normalizeList()`函数:

```
function normalizeList (list, flag = LIST_DEFAULT) {
  if (flag & LIST_FRACTION) {
    const max = Math.max(...list);
    list = list.map(value => Number((value / max).toFixed(2)));
  }
  if (flag & LIST_UNIQUE) {
    list = [...new Set(list)];
  }
  if (flag & LIST_SORTED) {
    list = list.sort((a, b) => a - b);
  }
  return list;
}
```

为了检查一个选项是否被激活，我们使用`&`操作符来检查该选项的相应位是否被打开(设置为`1`)。`&`操作是通过传递给函数的`flag`参数和选项的相应标志来执行的，如下面的代码片段所示:

```
// Checking if the unique option is activated
// (flag & LIST_UNIQUE) === LIST_UNIQUE (activated)
// (flag & LIST_UNIQUE) === 0 (deactivated)

flag & LIST_UNIQUE
```

## 实现新的 JS 特性？了解 JavaScript 错误如何影响用户

追踪生产 JavaScript 异常或错误的原因是耗时且令人沮丧的。如果您对监控 JavaScript 错误感兴趣，并想看看它们是如何影响用户的，[试试 LogRocket](https://logrocket.com/signup/) 。[![LogRocket Dashboard Free Trial Banner](img/e8a0ab42befa3b3b1ae08c1439527dc6.png)](https://logrocket.com/signup/)[https://logrocket.com/signup/](https://logrocket.com/signup/)

LogRocket 就像是网络应用的 DVR，记录下你网站上发生的每一件事。LogRocket 使您能够聚合和报告错误，以查看它们发生的频率以及它们影响了多少用户群。您可以轻松地重放发生错误的特定用户会话，以查看导致错误的用户操作。

log 火箭工具您的应用程序记录带有标题+正文以及用户上下文信息的请求/响应，以获得问题的全貌。它还会在页面上记录 HTML 和 CSS，即使是最复杂的单页应用程序也能重现像素级的完美视频。

增强您的 JavaScript 错误监控能力— [开始免费监控](https://logrocket.com/signup/)。

## 结论

嘿，我真的很高兴尽管读了很长时间，你还是读完了这篇文章，我强烈希望你在阅读的时候学到了一两件事。谢谢你的时间。

正如我们在本文中看到的，JavaScript 按位运算符虽然很少使用，但有一些非常有趣的用例。我强烈希望从现在开始，您在阅读本文的过程中获得的见解能够在您的日常编码中得到体现。

快乐编码…

## 通过理解上下文，更容易地调试 JavaScript 错误

调试代码总是一项单调乏味的任务。但是你越了解自己的错误，就越容易改正。

LogRocket 让你以新的独特的方式理解这些错误。我们的前端监控解决方案跟踪用户与您的 JavaScript 前端的互动，让您能够准确找出导致错误的用户行为。

[![LogRocket Dashboard Free Trial Banner](img/cbfed9be3defcb505e662574769a7636.png)](https://lp.logrocket.com/blg/javascript-signup)

LogRocket 记录控制台日志、页面加载时间、堆栈跟踪、慢速网络请求/响应(带有标题+正文)、浏览器元数据和自定义日志。理解您的 JavaScript 代码的影响从来没有这么简单过！

[Try it for free](https://lp.logrocket.com/blg/javascript-signup)

.