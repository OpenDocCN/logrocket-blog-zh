# 如何将字体 Awesome 添加到 Angular 项目

> 原文：<https://blog.logrocket.com/how-to-add-font-awesome-angular-project/>

在本文中，我们将学习如何在 Angular 应用程序中使用 Font Awesome，以及如何使用 Font Awesome 的图标动画和样式。

在我们进一步讨论之前，先说说什么是字体牛逼。

## 字体真棒

字体真棒是一个图标工具包，有超过 1500 个免费图标，非常容易使用。图标是使用可缩放的矢量创建的，并且在应用时继承 CSS 的大小和颜色。这使得它们成为高质量的图标，在任何屏幕尺寸下都能很好地工作。

在 Angular 5 发布之前，开发者必须安装 Font Awesome 包，并在 Angular 项目中引用它的 CSS 才能使用。

但是 Angular 5 的发布通过为字体 Awesome 创建 Angular 组件，使得在我们的项目中实现字体 Awesome 变得很容易。有了这个特性，Font Awesome 可以干净地集成到我们的项目中。

字体牛逼的图标由于其可伸缩性与文本很好地融合在一起。在这篇文章中，我们将探索如何使用动画，颜色和字体大小的图标。

## 创建演示角度应用程序

让我们为这个教程创建一个演示 Angular 应用程序。打开您的终端，CD 到项目目录，并运行下面的命令。

在运行该命令之前，请确保您的系统中安装了 Node.js，并且还安装了 [Angular CLI](https://blog.logrocket.com/6-useful-features-in-angular-cli-eb502bd95874/) :

```
ng new angular-fontawesome
```

## 安装字体 Awesome 依赖项

对于那些已经有项目的人，我们可以从这里继续跟进。一旦上面的命令完成，CD 到项目目录，并安装下面的字体真棒图标命令:

```
npm install @fortawesome/angular-fontawesome
npm install @fortawesome/fontawesome-svg-core
npm install @fortawesome/free-brands-svg-icons
npm install @fortawesome/free-regular-svg-icons
npm install @fortawesome/free-solid-svg-icons

# or

yarn add @fortawesome/angular-fontawesome
yarn add @fortawesome/fontawesome-svg-core
yarn add @fortawesome/free-brands-svg-icons
yarn add @fortawesome/free-regular-svg-icons
yarn add @fortawesome/free-solid-svg-icons

```

## 在角度应用中使用字体牛逼图标

在 Angular 项目中使用字体 Awesome 有两个步骤。我们要看看这两个:

1.  如何在组件级使用字体牛逼图标
2.  如何使用字体真棒图标库

### 如何在组件级使用字体牛逼图标

这一步必须在组件级别使用字体很棒的图标，这不是一个好方法，因为它需要我们将图标导入到每个需要图标的组件中，并且多次导入相同的图标。

我们仍然要看看如何在一个组件中使用图标，以防我们构建的应用程序要求我们只在一个组件中使用图标。

安装字体 Awesome 后，打开`app.module.ts`并导入`FontAwesomeModule`，如下图所示:

```
import { FontAwesomeModule } from '@fortawesome/angular-fontawesome'
imports: [
    BrowserModule,
    AppRoutingModule,
    FontAwesomeModule
  ],

```

之后，打开`app.component.ts`，导入想要使用的图标名称。假设我们想利用`faCoffee`:

```
import { faCoffee } from '@fortawesome/free-solid-svg-icons';

```

接下来，我们创建一个名为`faCoffee`的变量，并将导入的图标赋给该变量，这样就可以在`app.component.html`中使用它。如果我们不这样做，我们就不能使用它:

```
faCoffee = faCoffee;

```

现在，在`app.component.html`中，编写下面的代码:

```
<div>
    <fa-icon [icon]="faCoffee"></fa-icon>
</div>

```

运行命令为我们的应用提供服务，并检查我们的图标是否显示:

```
ng serve

```

如果我们看我们的网页，我们会看到`faCoffee`显示在屏幕上。这表明图标已成功安装和导入。

### 如何使用字体真棒图标库

这是在我们的应用程序中使用字体 Awesome 的最佳方法，特别是当我们想在所有组件中使用它而不需要重新导入图标或多次导入一个图标时。让我们来看看如何实现这一点。

打开`app.module.ts`并编写下面的代码:

```
import { FaIconLibrary } from '@fortawesome/angular-fontawesome';
import { faStar as farStar } from '@fortawesome/free-regular-svg-icons';
import { faStar as fasStar } from '@fortawesome/free-solid-svg-icons';

export class AppModule { 
  constructor(library: FaIconLibrary) {
    library.addIcons(fasStar, farStar);
  }
}

```

之后，我们可以在`app.component.html`中直接使用它，而无需在使用它之前声明一个变量并将其传递给该变量:

```
<div>
    <fa-icon [icon]="['fas', 'star']"></fa-icon>
    <fa-icon [icon]="['far', 'star']"></fa-icon>
</div>

```

如果我们现在加载网页，我们将看到显示的星形图标。

## 图标风格的字体真棒

字体 Awesome 有四种不同的风格，我们将看看免费图标——不包括 Pro light 图标，它们使用前缀`'fal'`和专业许可证:

*   实心图标使用前缀`'fas'`并从`@fortawesome/free-regular-svg-icons`导入
*   常规图标使用前缀`'far'`并从`@fortawesome/free-solid-svg-icons`导入
*   品牌图标使用前缀`'fab'`并从`@fortawesome/free-brands-svg-icons`导入

接下来，让我们看看我们还能对字体很棒的图标做些什么。

## 改变图标的颜色和大小，而不用编写 CSS 样式

让我们看看如何在不用 Angular 编写 CSS 样式的情况下改变字体和图标颜色。

这种方法有助于我们在组件级别使用图标。然而，当在所有组件中使用这种方法时，它没有帮助，因为它将改变我们项目组件中各处的图标颜色。对于多个组件，我们可以使用 CSS 类或样式属性只改变它一次。

但是当在组件级别工作时，我们可以使用它，因为我们将只在该组件中使用图标，而不是为它创建 CSS 属性并在 CSS 文件中进行样式化。所以让我们看看在一个角度项目中，我们将如何做这件事。默认情况下，下面的图标是`black`，我们想将其更改为`red`:

```
// from black
<fa-icon [icon]="['fab', 'angular']" ></fa-icon>

// to red
<fa-icon
      [icon]="['fab', 'angular']"
      [styles]="{ stroke: 'red', color: 'red' }"
></fa-icon>

```

当使用内嵌样式改变图标颜色和笔画时，我们必须利用`fa-icon`属性。

接下来，我们将在 Angular 中使用内联样式从小到大增加图标大小。为此，我们必须使用`fa-icon`的`size`属性:

```
    <fa-icon
      [icon]="['fab', 'angular']"
      [styles]="{ stroke: 'red', color: 'red' }"
      size="xs"
    ></fa-icon>

    <fa-icon
      [icon]="['fab', 'angular']"
      [styles]="{ stroke: 'red', color: 'red' }"
      size="sm"
    ></fa-icon>

    <fa-icon
      [icon]="['fab', 'angular']"
      [styles]="{ stroke: 'red', color: 'red' }"
      size="lg"
    ></fa-icon>

    <fa-icon
      [icon]="['fab', 'angular']"
      [styles]="{ stroke: 'red', color: 'red' }"
      size="5x"
    ></fa-icon>

    <fa-icon
      [icon]="['fab', 'angular']"
      [styles]="{ stroke: 'red', color: 'red' }"
      size="10x"
    ></fa-icon>

```

默认情况下，字体 Awesome 图标继承父容器的大小。它允许它们匹配我们可能使用的任何文本，但是如果我们不喜欢默认的大小，我们必须给它们我们想要的大小。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

我们使用了`xs`、`sm`、`lg`、`5x`和`10x`这些类。这些类根据我们的需要增加和减少图标的大小。

## 动画字体真棒图标

让我们也看看如何在 Angular 中不使用 CSS 或动画库的情况下动画字体牛逼的图标。

作为一名开发人员，当用户单击提交按钮或页面正在加载时，我们可能希望显示一个加载器或旋转器效果，告诉用户有东西正在加载。

我们可以使用字体很棒的图标来达到这个目的。我们不需要导入一个外部的 CSS 动画库，只需要在图标标签中添加字体 Awesome `spin`属性。

这样做可以让我们不用下载一个完整的 CSS 动画库，而只是为了使用微调效果或使用关键帧编写一个很长的 CSS 动画。

因此，让我们看看如何通过使用[反应图标](https://blog.logrocket.com/react-icons-comprehensive-tutorial-examples/)来实现这一点:

```
<fa-icon
      [icon]="['fab', 'react']"
      [styles]="{ stroke: 'blue', color: 'blue' }"
      size="10x"
></fa-icon>

```

我们刚刚导入了 React 图标，现在我们将为它制作动画。在 React 图标组件上，添加字体 Awesome `spin` loader 属性:

```
<fa-icon
      [icon]="['fab', 'react']"
      [styles]="{ stroke: 'blue', color: 'rgb(0, 11, 114)' }"
      size="10x"
      [spin]="true"
></fa-icon> 

```

当我们加载网页时，我们将看到 React 图标无限旋转。这是因为我们将`spin`属性设置为`true`。

## 结论

在本文中，我们可以看到如何在 Angular 项目中使用字体很棒的图标，如何添加一些基本的图标库样式，以及如何制作图标动画。

我们还可以对字体很棒的图标做更多的事情，比如[固定宽度图标](https://fontawesome.com/v5.15/how-to-use/on-the-web/styling/fixed-width-icons)、[旋转图标](https://fontawesome.com/v5.15/how-to-use/on-the-web/styling/rotating-icons)、[能量转换](https://fontawesome.com/v5.15/how-to-use/on-the-web/styling/power-transforms)，以及组合两个图标。Font Awesome 的教程可以教你如何在项目中使用这些工具。

如果你觉得这篇文章很有帮助，请与你的朋友分享。

## 像用户一样体验 Angular 应用程序

调试 Angular 应用程序可能很困难，尤其是当用户遇到难以重现的问题时。如果您对监视和跟踪生产中所有用户的角度状态和动作感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/angular-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/2794ac39244976f37c4941d9a910be23.png)](https://lp.logrocket.com/blg/angular-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/angular-signup)

LogRocket 就像是网络和移动应用程序的 DVR，记录你网站上发生的一切，包括网络请求、JavaScript 错误等等。您可以汇总并报告问题发生时应用程序的状态，而不是猜测问题发生的原因。

LogRocket NgRx 插件将角度状态和动作记录到 LogRocket 控制台，为您提供导致错误的环境，以及出现问题时应用程序的状态。

现代化调试 Angular 应用的方式- [开始免费监控](https://lp.logrocket.com/blg/angular-signup)。