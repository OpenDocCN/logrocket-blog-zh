# 如何在 Next.js - LogRocket 博客中使用代理

> 原文：<https://blog.logrocket.com/how-to-use-proxy-next-js/>

中间商无处不在。由于这些中间人的帮助，我们日常生活中使用的大部分东西都是可持续的和可用的。

例如，我们使用的商品和服务的零售商和批发商只不过是帮助我们以更有效的方式获得所需的中间商。我们不需要见制造商，也不需要知道原材料来自哪里。

从本质上说，这就是代理——它充当我们的数字中间人。

## 什么是代理？

代理或代理服务器充当您正在访问的网站和您的设备之间的中继。例如，如果您考虑到代理意味着您不必暴露您的真实身份，这可以为安全性提供巨大的好处。

就像中间人一样，代理服务器通过将客户端请求转发给资源来充当客户端和服务器之间的中间服务器——最好将其视为网关。代理服务器不应被误认为虚拟专用网络(VPN ),因为虽然它们的用途可能相似，但它们并不相同。

电脑上的 VPN 客户端与 VPN 服务器建立安全隧道，取代本地 ISP 路由。VPN 连接加密和保护您的所有网络流量；不仅仅是 HTTPS 像代理服务器一样从浏览器调用。

互联网上的任何东西要么在客户端，要么在服务器端。你的浏览器就是一个客户端的例子；当你在脸书或 Twitter 上注册时，你填写的表单域(流量)会被服务器发送和处理。

现在，当你试图在互联网上做任何事情时，你的计算机(客户机)直接与服务器通信。这是代理服务器可以帮助我们避免的。

## 代理的重要性

### 保护

在当今的网络安全环境下，不使用代理访问网站被认为是不安全的；它使您处于开放状态，容易受到任何能够访问服务器的人的攻击。使用代理有助于保护客户信息不被泄露。

### 防火墙

大多数公司或家庭设置了防火墙来限制网络用户访问某些网站。代理服务器可用于绕过被阻止的网站。对于组织来说，将被认为对员工数据安全有危险的网站屏蔽或列入黑名单是很常见的。

另外，很多网站都有地域限制。在这些情况下，如果访问网站是必要的，代理服务器可以帮助你。

### 匿名

因为你是通过网关发送流量，这就产生了一定程度的匿名性。

它将您的身份从客户端-服务器等式中移除，并通过仅将某些数据交给中间人来实现这一点，这是服务器接收到的关于您的唯一信息。通过使用代理，用户可以以这种方式匿名保存他们的个人信息和浏览习惯。

## 在 Next.js 应用程序中实现代理

Next.js 是构建在 Node.js 之上的开源开发框架，支持基于 React 的 web 应用功能，比如服务器端渲染和静态网站生成。

现在，我们将在一个简单的 Next.js 应用程序中试验我们已经讨论过的关于代理服务器的所有内容。Next.js 甚至提供了一种用它的[重写](https://nextjs.org/docs/api-reference/next.config.js/rewrites)数组来做这件事的方法。

确保您的计算机上至少安装了 Node.js 版本 12。在您的机器上安装 Node.js 带有 npm(节点包管理器)，它也带有 npx(节点包执行)，它将用于安装 Next.js。

一些开发人员更喜欢 Yarn，这是替代 npm 的另一个选项——如果您想使用它，请安装它。

### **安装 Next.js**

在您的机器上输入以下命令:

```
npx [email protected]
#OR
yarn create next-app

```

不管您安装了什么样的软件包管理器，您都会得到:

![Nextjs Proxy Example Screenshot Implementation](img/f33e151aa925c61a86cdbd6944226e31.png)

点击**进入**，之后会提示你输入项目所在文件夹的名称。如果你再次点击**输入**，这个项目的名字将会是“我的应用”。

安装完成后，进入您的项目目录。这是您的 Next.js 全新安装的样子:

![Proxy Nextjs Installation Project Directory](img/2a89ac75dd6acb0756161759b644e1aa.png)

要启动它，请运行:

```
npm run dev
#OR
yarn dev

```

会从 [http://localhost:3000](http://localhost:3000) 开始。

![Proxy And Nextjs Screenshot Example](img/1e53fb6de127a5a91c59946526146360.png)

你会得到类似这样的图像(我在这里做了一些小的编辑😏).

在这一点上，我假设您对 Next.js 有一定程度的熟悉，所以我将开门见山。我们准备用这个 app(注意默认运行在 3000 端口；您总是可以重新配置它)并让它连接到另一个完全不同端口上的后端服务器。

我将创建一个猫页面，在那里我将从[https://meowfacts.herokuapp.com](https://meowfacts.herokuapp.com)获得关于猫的随机事实(这是一个简单的 API，显示关于猫的随机事实)。

Next.js 提供了一种简单的方法。在`next.config.js` *，*中用这个替换里面的代码:

```
//js
module.exports = () => {
  const rewrites = () => {
    return [
      {
        source: "/cats",
        destination: "https://meowfacts.herokuapp.com",
      },
    ];
  };
  return {
    rewrites,
  };
};

```

[重写](https://nextjs.org/docs/api-reference/next.config.js/rewrites)允许您将传入请求路径映射到不同的目标路径。

重写充当 URL 代理并屏蔽目标路径，使其看起来好像用户没有改变他们在站点上的位置。相反，[重定向](https://nextjs.org/docs/api-reference/next.config.js/redirects)将重新路由到一个新的页面，并显示 URL 的变化。

我们在上面的代码中所做的只是简单地配置我们的 Next.js 应用程序，以便当我们转到`/cats`时，它从目的地获取数据而不泄露它。

我将继续添加另一条从[https://random-d.uk/api/random](https://random-d.uk/api/random)获取数据(随机鸭子图像)的路线。我只需要添加另一个对象，如下所示:

```
  {
        source: "/ducks",
        destination: "https://random-d.uk/api/random",
      },

```

现在，它看起来像这样:

```
module.exports = () => {
  const rewrites = () => {
    return [
      {
        source: "/cats",
        destination: "https://meowfacts.herokuapp.com",
      },
      {
        source: "/ducks",
        destination: "https://random-d.uk/api/random",
      },
    ];
  };
  return {
    rewrites,
  };
};

```

这样，当我们在 Next.js 应用程序上访问`/ducks`时，它会从[https://random-d.uk/api/random](https://random-d.uk/api/random)获取数据，而不会暴露其 URL。以下面的图片为例:

![Nextjs Data Retrieval Proxy Example Screenshot](img/d26f640fd523b287bcd6aff2b0e9b7be.png)

![Proxy Nextjs Retrieval Example Cat Website](img/ace88a1d48f03137718ee49939662e3f.png)

## 结论

恭喜你！您已经到达本教程的结尾。我们已经研究了什么是代理以及代理的效用——从它提供的保护到它带来的匿名性。

我们在 Next.js 中实现了一个代理，它提供了一种开箱即用的方式来实现这一点。通过重写，我们能够将传入的请求路径映射到不同的目标路径，并且能够使用一些外部 API 对此进行测试，并在我们的本地机器上纠正提要。

## [LogRocket](https://lp.logrocket.com/blg/nextjs-signup) :全面了解生产 Next.js 应用

调试下一个应用程序可能会很困难，尤其是当用户遇到难以重现的问题时。如果您对监视和跟踪状态、自动显示 JavaScript 错误、跟踪缓慢的网络请求和组件加载时间感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/nextjs-signup)

.

[![](img/f300c244a1a1cf916df8b4cb02bec6c6.png)](https://lp.logrocket.com/blg/nextjs-signup)[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/nextjs-signup)

LogRocket 就像是网络和移动应用的 DVR，记录下你的 Next.js 应用上发生的一切。您可以汇总并报告问题发生时应用程序的状态，而不是猜测问题发生的原因。LogRocket 还可以监控应用程序的性能，报告客户端 CPU 负载、客户端内存使用等指标。

LogRocket Redux 中间件包为您的用户会话增加了一层额外的可见性。LogRocket 记录 Redux 存储中的所有操作和状态。

让您调试 Next.js 应用的方式现代化— [开始免费监控](https://lp.logrocket.com/blg/nextjs-signup)。