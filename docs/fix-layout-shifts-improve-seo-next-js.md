# 如何在 Next.js apps 中修复布局变化以改善 SEO

> 原文：<https://blog.logrocket.com/fix-layout-shifts-improve-seo-next-js/>

谷歌不断调整其核心搜索算法，很快，整体网络性能将在页面排名中发挥更大的作用。在确定排名时，谷歌将[评估从真实用户那里收集的以下三个指标](https://web.dev/vitals/):

*   最大含量涂料(LCP)
*   第一输入延迟(FID)
*   累积布局偏移(CLS)

这些指标共同构成了谷歌的网络生命倡议。在这篇文章中，我们将专门讨论 CLS，特别关注如何防止 Next.js 应用程序中的布局偏移。

## 什么是 CLS？

Google [将累积布局移位](https://web.dev/cls/#what-is-cls)定义如下:

> CLS 衡量在页面的整个生命周期中发生的每个意外布局变化的所有单个布局变化分数的总和。每当可见元素从一个呈现的帧到下一个呈现的帧改变其位置时，就会发生布局偏移。

用最简单的话来说，CLS 是页面加载时组件间移动的度量。它最终由布局偏移分数表示，计算如下:

```
layout shift score = impact fraction * distance fraction

```

其中，冲击分数是两个框架之间所有不稳定元素的冲击面积总和，距离分数是两个框架之间元素或组件移动的距离。

布局变化通常是由广告/嵌入、动态注入的内容和没有尺寸的图像引起的。让我们探讨一下如何处理每种情况下的 CLS。

## 如何修复 CLS

### 广告/嵌入

广告是许多网页的重要收入来源，不容忽视。然而，当一个新的广告容器被插入到 DOM 中时，它会导致严重的回流或布局偏移；它可能会移动整个网页的一部分。除了插入问题之外，如果广告容器由于其内容或广告库而调整大小，这也会产生额外的摩擦。

我们可以通过以下方法解决这个问题:

*   构建一个空容器作为广告的占位符，这样当插入广告时，它不会移动页面上的其他元素，并减少 CLS
*   指明广告的最大尺寸，这样它就不会调整大小和触发布局变化。如果广告是一个横幅或者在视窗的顶部，那就更要注意了
*   如果没有广告返回，避免删除占位符，因为这也将触发布局的变化

### 动态注入的内容

动态内容是布局变化的另一个主要原因。如果没有占位符，这样的内容或组件(例如，“接受 cookies”容器)会导致严重的布局偏移。即使是占位符容器，预期内容大小的增加或减少也会导致布局变化。

动态内容也可能是成功的网络调用/API 响应的结果。该数据最初将为空，并在成功的网络调用后加载，从而导致布局偏移。

除了优化的占位符容器之外，框架 UI 也是动态内容的绝佳解决方案。以电子商务产品页面为例。每个产品项目的框架组件将创建一个干净的布局，并且在成功的 API 响应之后，这些框架组件被更新，而没有重大的布局变化。

### 没有尺寸的图像

没有维度的图像是 CLS 最常见但容易修复的原因之一。没有预定义尺寸的图像在加载之前不会占用屏幕上的任何空间。因此，当它加载时，它会移动周围的所有元素，从而导致布局移动的多米诺骨牌效应。

我们可以通过向图像标签添加`height`和`width`属性来解决这个问题:

```
<img src="example.jpg" width="640" height="360" alt="Alt text">

```

这是一个固定的高度和宽度，但是，这仍然会导致响应设计中的布局变化。因此，在响应式设计中，固定尺寸没有太大帮助。此外，由于 CSS 已经成为一个更可取的方式来调整图像，我们需要一个更好的解决方案。

这就是 [CSS `aspect-ratio`属性](https://blog.logrocket.com/jank-free-page-loading-with-media-aspect-ratios/)的由来。我们仍然传递`height`和`width`属性，但是使用`aspect-ratio`来维护这些属性之间的比率。

```
<!-- set a 640:360 i.e a 16:9 - aspect ratio -->
<img src="example.jpg" width="640" height="360" alt="Alt text">

<!-- CSS -->
<style>
img {
    aspect-ratio: attr(width) / attr(height);
}
</style>

```

在这里，我们将纵横比保持为一个不变的值，而不考虑实际的图像大小，这有助于我们更好地对抗 CLS。不幸的是，开发人员经常忽略或忽视这种相对简单的优化。

## 修复 Next.js 中的 CLS

Next.js 是一个建立在 React 基础上的渐进式框架，旨在实现默认值和约束来解决我们上面讨论的问题。让我们看看如何在 Next.js 中利用上述技术，以及它们是如何被支持的。

### Images 和 Next.js

如上所述，没有尺寸的图像是布局偏移的主要原因。为了解决这个问题，Next.js 有一个`Image`组件，它是`img`元素的包装器。

这个组件到底是做什么的？

*   `height`和`width`属性在`img`标签中都是可选的。因此，许多开发者倾向于忽略它，导致一个糟糕的 CLS 分数。Next.js `Image`组件使得`height`和`width`成为必需的道具，保持开发者的诚实。它还保持`aspect-ratio`开箱即用
*   `Image`组件对`srcsets`有着神奇的支持，因此可以在不同大小的设备和视窗之间实现流畅的体验
*   它还支持在需要时预加载图像。但是，默认情况下，图像是延迟加载的，以提高性能

下面是一个语法示例:

```
<Image
  src="/example.png"
  alt="Alt text"
  width={500}
  height={500}
/>

```

### Ads 和 Next.js

尽管 Next.js 没有现成的广告支持，但我们可以利用`Layout`组件来更好地处理布局变化。Next.js 让我们创建一个`Layout`组件，它可以用作应用程序中所有页面的基本布局/结构。

对于给定的 web 应用程序，广告通常放置在所有页面的相同位置。我们可以创建一个带有广告占位符容器的`Layout`组件，并将其包含在`_app.js`文件中。这使得每个页面上都有广告容器，当广告加载时，应该很少或者没有布局变化。

根据 web 应用程序的类型，我们也可以将它用于框架 UI。请参见下面的语法:

```
# Layout.jsx
export const Layout = () => (
  <div>
    <AdContainer />
    <Header />
    <LeftMenu />
  </div>
)

# _app.js
import { Layout } from '../components/Layout'
export default function MyApp({ Component, pageProps }) {
  return (
    <Layout>
      <Component {...pageProps} />
    </Layout>
  )
}

```

这样，我们可以在单点处理整个应用程序的布局变化。

### 额外提示:CLS 和风格化组件

如果你[在 Next.js](https://blog.logrocket.com/theming-in-next-js-with-styled-components-and-usedarkmode/) 中使用 styled-components，并且没有足够的注意，布局会有很大的变化。这是因为 Next.js 在默认情况下是 SSR，因此，它使用基本样式将 JavaScript 加载到客户端。另一方面，样式化组件在运行时在浏览器中执行。

这会将新样式强加到网页上，导致重绘和重排。为了避免这种情况，使用`babel-plugin-styled-components`并利用`_document.js`中的样式组件`ServerStyleSheet`在服务器端应用样式，避免在浏览器上渲染后的回流。

## 结论

通过遵循上述技术，我们可以大大减少 Next.js 应用程序中的布局变化。这对于保持良好的 SEO 是必要的，但更重要的是，它在我们的 web 应用程序上创建了更流畅、更令人愉悦的用户体验。

## [LogRocket](https://lp.logrocket.com/blg/nextjs-signup) :全面了解生产 Next.js 应用

调试下一个应用程序可能会很困难，尤其是当用户遇到难以重现的问题时。如果您对监视和跟踪状态、自动显示 JavaScript 错误、跟踪缓慢的网络请求和组件加载时间感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/nextjs-signup)

.

[![](img/f300c244a1a1cf916df8b4cb02bec6c6.png)](https://lp.logrocket.com/blg/nextjs-signup)[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/nextjs-signup)

LogRocket 就像是网络和移动应用的 DVR，记录下你的 Next.js 应用上发生的一切。您可以汇总并报告问题发生时应用程序的状态，而不是猜测问题发生的原因。LogRocket 还可以监控应用程序的性能，报告客户端 CPU 负载、客户端内存使用等指标。

LogRocket Redux 中间件包为您的用户会话增加了一层额外的可见性。LogRocket 记录 Redux 存储中的所有操作和状态。

让您调试 Next.js 应用的方式现代化— [开始免费监控](https://lp.logrocket.com/blg/nextjs-signup)。