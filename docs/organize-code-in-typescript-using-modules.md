# 使用模块在 TypeScript 中组织代码

> 原文：<https://blog.logrocket.com/organize-code-in-typescript-using-modules/>

我非常喜欢 TypeScript 的一点是它支持模块化编程架构的能力。在多年专注于设计模式和最佳实践构建 JavaScript 应用程序之后，我发现在考虑软件的结构和可重用性时，ES6 模块模式是必不可少的。

在本文中，我们将讨论模块，它们是什么，为什么以及如何使用它们来改进您的类型脚本代码的组织。

## 什么是模块？

模块是计算机编程中的一种设计模式，用于提高代码的可读性、结构和可测试性。包括 TypeScript 在内的许多编程语言都支持它们。简而言之，模块是具有独立程序功能的小段代码。

### 我们为什么需要模块？

一个典型的软件会随着新功能的引入而逐渐成长。构建和保存我们的代码库最初可能是一项单调乏味的任务，但是预先做好这项工作会使下次有人在代码库上工作时事情变得更容易。

如果没有结构化我们的代码，我们软件的各个部分会变得纠缠不清，这使得重用代码和孤立地看待任何一段代码变得更加困难。模块是一个非常有用的工具，可以让代码变得清晰，便于重用和测试。

模块作为代码及其相关功能的组织工具，允许我们:

*   封装我们的代码。
*   明确定义当前模块运行所需的模块列表。
*   在自包含的块中构造我们的代码
*   轻松测试我们的代码
*   定义公共 API
*   还有更多！

在下面几节中，我们将讨论如何使用 TypeScript 模块。

## 探索类型脚本模块

现在我们已经对软件开发中的模块有了共同的理解，我们可以更深入地研究 TypeScript 模块以及如何使用它们。

鉴于 TypeScript 是 JavaScript 的超集，它的大部分概念都来自 JavaScript，包括 ES6 中引入的 JavaScript 模块模式。它还使用与 JavaScript [ES6 模块](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)相同的`export`和`import`关键字。

### 使用 eexport**s**yn tax

在 TypeScript 中，一段代码保留在模块内部，在导出之前不能在模块外部访问。一旦导出，代码就公开了。

下面是一个使用`export`的例子:

```
// module-1.ts 

//Private variable
let myApiKey: string = "Secret"; 

//Public variable
export const myPublicKey: string = "Public"; 

export enum MutationType {
  CreateTask = 'CREATE_TASK',
  SetTasks = 'SET_TASKS',
  RemoveTask = 'REMOVE_TASK',
  EditTask = 'EDIT_TASK',
}

// exported interface 
export interface Mutation{ 
  content: string; 
  type: MutationType; 
}

// private function
function logToConsole(mutation: Mutation): void {
    switch (mutation.type) {
        case MutationType.CreateTask:
            console.log(mutation.content);
    ...
        default:
            console.error(mutation.content);
    }
    console.log(message);
}

// exported function
export function log(mutation: Mutation): void {
    logToConsole(mutation);
}

```

### 使用 Iimport**s**yn tax

在 TypeScript 中，可以使用`import`在模块外部访问一个模块中公开的代码。

这里有一个例子:

```
// my-module.ts
import { log, Mutation, MutationType, myPublicKey } from "./module-1";
console.log("Public key: ", myPublicKey);
const deleteTask: Mutation = {
    content: "Delete a task",
    type: MutationType.RemoveTask,
};
const createTask: Mutation = {
    content: "Create a task",
    type: MutationType.CreateTask,
};
log(deleteTask);
log(createTask);

```

使用 TypeScript 模块，我们可以导入和导出`classes`、`type`别名、`var`、`let`、`const`等符号。

### **将** **改名为** i **导入**

ES6 模块中一个非常常见的概念是重命名`import`。在 TypeScript 中，可以使用以下语法在导入时重命名公开的代码段:

```
// my-module.ts
import { publicKey as publicApiKey } from './module-1"

```

或者，您可以使用下面的语法来导入模块的所有内容，并给它一个您选择的名称:

```
// my-module.ts
import * as anyName from './module-1"

```

最后，通过将 tsconfig.json 中的`esModuleInterop`选项设置为`true`，可以使用下面的语法(符合 ECMAScript 规范)导入`CommonJS`模块:

```
// my-module.ts
import foo from "someCommonJsModule";

```

对于新的 TypeScript 3+项目，`esModuleInterop`设置会自动启用。

### **重命名** **带导出**

还可以使用以下语法，使用 export 语句来重命名我们公开的代码段:

```
interface Mutation{ 
  content: string; 
  type: MutationType; 
}

enum MutationType {
  CreateTask = 'CREATE_TASK',
  ...
}
// 1
export {
    Mutation
}
// Renaming export
export {
    Mutation as RenamedMutation
}

```

在上面的例子中，第一个导出只是用它的原始名称导出符号，而第二个导出使用不同的名称，使用`as`关键字。

### **默认** **e** 导出

正如 JavaScript ES6 模块一样，每个模块都可以使用以下语法进行默认导出。

```
// export default function
export default function log(mutation: Mutation): void {
    logToConsole(mutation);
}

```

默认导出语法可以与重命名的导出和`export`语法一起使用，如下所示:

```
// export default function
export default function log(mutation: Mutation): void {
    logToConsole(mutation);
}

function saveMutation(mutation: Mutation): void {
    saveToLocalStorage(mutation);
}

export {
    saveMutation
}

// Renaming export
export {
    saveMutation as RenameSaveMutation
}

```

最后，在模块中，可以使用`export = ...`语法导出单个对象或函数。例如:

```
//save-module.ts
function saveMutation(mutation: Mutation): void {
    saveToLocalStorage(mutation);
}
export = saveMutation;

```

下面是使用`export = ...`语法时对应的`import`语法:

```
//use-save-module.ts
import saveMutation = require("./save-module");
saveMutation(mutationToBeSave);

```

但是，请注意，这不是推荐的方法。只有在从模块中导入一段公开的代码时，才应该使用这种导入方式。

### **再出口**

还可以重新导出由其他模块导出的暴露的代码段:

```
// module-with-re-exports.ts 
export * from "./module-1";

```

在前面的代码片段中，除了重新导出`module-1.ts`中所有暴露的代码片段之外，我们没有在模块中做任何事情。

## 如何使用带进出口的桶

### **什么是桶？**

桶是一种技术，用于将不同模块的输出汇总到一个模块中，通常称为`index.ts`，以简化输入。因此，桶只是组合了一个或多个其他模块的输出。桶非常有用，因为它让你专注于你想使用的代码，而不是它们在哪里。

### **在上下文中理解桶**

桶可以被有策略地用来减轻特定的暴露代码的导入。

例如，下面的代码片段导出了`books`函数，该函数使用导出语法返回`bookList`(一个字符串数组):

```
//barrel/book-module.ts
const bookList: string = ["God of War", "Lord of the rings"]
export function books(): string[] {
  return bookList
}

```

类似地，下面的代码片段导出`cars`函数，该函数使用导出语法返回`carList`(一个字符串数组)。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

```
//barrel/car-module.ts
const carList: string = ["Ferrari", "BMW"]
export function cars(): string[] {
  return carList
}

```

最后，下面的代码片段演示了如何使用 barrel 将不同模块的导出汇总到一个单独的模块中，通常称为`index.ts`。在本例中，我们分别从`'./book-module'`和`'./car-module'`再出口所有出口:

```
//barrel/index.ts
export * from './book-module';
export * from './car-module';

```

### **为什么要用桶？**

如果没有桶，消费者将需要两份进口声明:

```
//barrel/usage.ts
import { books } from '@/barrel/book-module';
import { cars } from '@/barrel/car-module';

```

相反，桶允许我们简化进口，就像这样:

```
//barrel/usage.ts
import {book, car} from '@/barrel';
let allBooks = books(); //["God of War", "Lord of the rings"]
let allCars = cars(); //["Ferrari", "BMW"]

```

下面的代码片段也是一个有效的桶用法。

```
//barrel/usage.ts
import {book, car} from '@/barrel/index.ts';
let allBooks = books(); //["God of War", "Lord of the rings"]
let allCars = cars(); //["Ferrari", "BMW"]

```

## **结论**

模块化编程的重要性在一般的软件开发中怎么强调都不为过，尽管忽略它并让我们软件的各个部分深深地纠缠在一起是很诱人的。

为了构建可伸缩和可重用的 TypeScript 应用程序，我建议您利用 TypeScript 模块来改进应用程序的组织。这样做将增加代码的可重用性和可测试性，并有助于为您的构建创建一个更好的整体结构。

## [LogRocket](https://lp.logrocket.com/blg/typescript-signup) :全面了解您的网络和移动应用

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/typescript-signup)

LogRocket 是一个前端应用程序监控解决方案，可以让您回放问题，就像问题发生在您自己的浏览器中一样。LogRocket 不需要猜测错误发生的原因，也不需要向用户询问截图和日志转储，而是让您重放会话以快速了解哪里出错了。它可以与任何应用程序完美配合，不管是什么框架，并且有插件可以记录来自 Redux、Vuex 和@ngrx/store 的额外上下文。

除了记录 Redux 操作和状态，LogRocket 还记录控制台日志、JavaScript 错误、堆栈跟踪、带有头+正文的网络请求/响应、浏览器元数据和自定义日志。它还使用 DOM 来记录页面上的 HTML 和 CSS，甚至为最复杂的单页面和移动应用程序重新创建像素级完美视频。

[Try it for free](https://lp.logrocket.com/blg/typescript-signup)

.