# 如何构建自定义的 GatsbyJS 主题- LogRocket 博客

> 原文：<https://blog.logrocket.com/how-to-build-a-custom-gatsbyjs-theme-e9b9529f217b/>

### 盖茨比简介

[Gatsby](https://www.gatsbyjs.org/) 是一个静态站点生成器，由 [React.js](https://reactjs.org/) 、 [Webpack](https://webpack.js.org/) 和 [GraphQL](https://graphql.org/) 构建而成。它用于构建速度非常快、内容相对静态的网站，例如博客、电子商务网站，甚至公司网站。

> 注意:static site generator 是一个平台，它允许您获得静态 HTML 中的所有请求，而不必担心每次用户发出请求时服务器和模板(代码)的交互。

> ![](img/a9dc26f7fc11e20fb3ffe997ce4d709f.png)
> 
> How Gatsby works.

#### **TL:DR**

以下是我们将在这篇文章中看到的内容的简要概述:

1.  创建主题目录并安装依赖项
2.  将网站连接到自定义主题
3.  导出站点要使用的一些组件
4.  通过实现组件隐藏来重写站点的一些自定义主题功能

先决条件

### 在我们进一步讨论之前，本文假设:

[Node.js ≥ v6](https://nodejs.org/en/) 安装在您的机器上

*   [NPM](https://www.npmjs.com/get-npm) 已安装在您的机器上
*   全球安装 Gatsby CLI
*   你对 [React.js](https://reactjs.org/) 有了基本的了解
*   文件夹设置

### 在完成以下所有步骤后，文件结构看起来会像这样:

简单-盖茨比主题>>套餐> >苏格兰主题

*   位置
*   目录是我们项目的文件夹。packages 目录包含一个文件夹，它的名字就是我们将要创建的主题，在这个文件夹中，我们有我们的主题文件。最后，站点目录包含了我们的 Gatsby 站点，当我们开发它时，它使用了我们的主题。

构建盖茨比主题

### 在某种意义上，主题不同于启动器，因为您可以将主题构建为库。然后，它变成一个可以更新的包，通过隐藏复杂性，可以让您更快地启动和运行。因此，通过构建主题，我们允许导入该主题的用户从主题中获得一些灵感，并更快地建立用户界面。

**设置项目配置**

首先，在本地设备上为 Gatsby 主题创建一个目录

1.  `$ mkdir simple-gatsby-theme`

然后运行`npm init`来设置`package.json`:

![](img/c6040e83cd4ae4c15dbbd156c7c0a58d.png)

2.在创建的`package.json `中，我们可以添加一个[工作区](https://yarnpkg.com/lang/en/docs/workspaces/)，这将允许我们从用户端管理主题的更新，我们还添加了一个“packages/*”来帮助我们将 Gatsby 主题链接到这个最终用户工作区主题管理包:

```
{ "private": true,  "name": "simple-gatsby-theme",  "version": "1.0.0",  "workspace": [ "site", "packages/*" ]  }
```

> 创建一个新目录，将其命名为 packages，在该目录中，我们将创建主题目录，并使用 `yarn init` bash 命令创建一个`package.json ` :

1.  ![](img/32b0ce635451555d855e1cb4fd03b397.png)

4.接下来，让我们去现场总监那里运行`$ yarn workspace simple-site add gatsby`。这使用 Yarn 的工作区特性将 Gatsby 添加为我们项目的依赖项。

注意:上面的命令指向站点目录中的 package.json 名称“simple-site”。

> 我们必须手动添加主题的配置作为项目的依赖项，这样它就可以访问主题:`"[THEME NAME]" : "*"`

```
{ 
 "name": "simple-site",
 "version": "0.0.1",
 "description": "A simple site built for a Gatsby Theme ", 
 "main": "gatsby-config.js", 
 "license": "MIT", 
 "private": true,
 "scripts": { "develop": "gastby develop", "build": "gatsby build" },
 "dependencies": { "gatsby": "^2.1.19", "simple-theme":"*" }
 }
```

设置主题依赖关系

### 在我们的主题目录中，我们要做的第一件事是通过运行`$ yarn workspace simple-site add gatsby -D`安装 Gatsby，作为一个开发依赖项，我们的`package.json `现在看起来像这样:

这里我们有一个 peer 依赖项和一个 dev 依赖项集，这样如果用户安装了主题，它会确保 Gatsby 被安装。如果他们还没有安装盖茨比，它会警告他们。我们还通过运行`$ yarn workspace simple-theme react react-dom -D`为主题安装了 React 和 react-dom。

```
{ 
 "name": "simple-theme", 
"version": "0.0.1", "description": "A Simple gatsby theme", 
"main": "index.js",
 "license": "MIT",
 "devDependencies": { 
 "gatsby": "^2.1.19",
 "react": "^16.8.3", 
"react-dom": "^16.8.3" 
}, 
"peerDependencies": { 
"gatsby": "^2.1.19", 
"react": "^16.8.3",
 "react-dom": "^16.8.3" 
 } 
}
```

构建主题

### 我们会使用。mdx 扩展，因为它允许您编写内嵌在 markdown 中的 JSX。这是一个很好的组合，因为它允许你对小东西使用 markdown 的语法(比如`# heading `),对更高级的组件使用 JSX。另外，MDX 没有运行时，所有编译都发生在构建阶段。

这在 Mac 上运行良好:

对于 windows 设备，运行以下命令(我将在下面的一节中解释原因):

```
$ yarn workspace simple-theme add gatsby-mdx @mdx-js/mdx @mdx-js/tag
```

由于 Gatsby 在默认情况下只会在 src/pages/index.mdx 目录中查找站点信息，所以我们的第一步是在我们的主题文件夹中创建这个目录。另外，记得在主题目录中创建一个 index.js。

```
$ yarn workspace simple-theme add [email protected]
```

为主题设置 Gatsby-config.js

### 我们将从在主题目录中创建一个`gatsby-config.js`文件开始，并输入以下内容:

我们安装了[gastby-content-page-creator](https://www.gatsbyjs.org/packages/gatsby-plugin-page-creator/)插件，这样当主题构建时，它也可以通过读取我们的包来查看主题的`src/pages/index.mdx`。这确保了当我们运行代码时，它从我们的包中读取，因为默认情况下，它查看站点目录中的页面。因此，如果我们希望它从主题中加载页面，我们将在 bash 中运行以下代码:

```
module.exports= { plugins : [ 
{ 
  resolve : 'gatsby-mdx', 
  options:{} 
},
 { 
  resolve : 'gatsby-plugin-page-creator',
  options:{ path: ${__dirname}/src/pages, 
      }
    }
  ]
}
```

为站点设置 gatsby-config.js

```
$ yarn workspace simple-theme add gatsby-plugin-page-creator
```

### 这里我们指定使用一个主题:

**注意:**一个快速检查你的站点是否真的有主题作为依赖项的方法是运行$ yarn workspace info

```
module.exports = { 
  __experimentalThemes : [ 'simple-theme' ] 
};
```

> ![](img/648a2e7e3e97b16d3522e25bcf3c435a.png)

从上面，我们可以看到，简单的网站有简单的主题作为依赖，这意味着他们是链接的，我们准备好了。

然后，我们通过运行`$ yarn workspace simple-site develop`来运行演示以进行测试，我们应该在屏幕上显示以下内容:

![](img/264ecf89b6c75093e0adc2db35c633df.png)

向简单主题添加布局

### 我们可以使用 [gatsby MDX layouts](https://gatsby-mdx.netlify.com/api-reference/options/default-layouts/) 来定位我们想要显示的实际布局。我们首先在站点的 src 中创建一个组件目录和一个 layout.js 组件，在其中我们有一个简单的 react 组件:

为了让上面的代码在简单主题中可用，我们必须告诉 MDX 解析这个布局:

```
import React from 'react' 

export default 
({children}) => ( 
  <section style = 
{{
   display: 'block', 
   maxWidth : '650px',
   margin: '2rem auto'
 }}>
     {children}
   </section>
 );
```

所以默认情况下，MDX 会寻找`layout.js`并对整个 MDX 实现样式。

```
module.exports = { 
plugins : [ 
  { 
    resolve : 'gatsby-mdx', 
    options:{ defaultLayouts:{
     default: require.resolve('./src/components/layout.js') 
         }
       }
     }
   ]
 }
```

将组件样式导入 index.mdx

### 我们可以写普通的。mdx，并导入 React 组件，对这些组件进行如下样式化:

首先，我们创建一个`info.js `文件并导入 React 和一些服装样式:

然后我们转到我们的`info.mdx `并导入组件，就像我们在常规反应中一样:

```
import React from 'react' 
const Info = ({children}) =>{ 
return( 
    <div 
      style = {{
                 padding: "1rem", 
                 backgroundColor: "blue" 
              }} > 
              {children} 
          </div>
           ); 
  }
   export default Info
```

我们应该有一个蓝色背景的显示，指示包裹在 info 标签周围的部分:

```
import Info from "./../components/info" 

<Info>
   My name is obinna  welcome to my simple theme
 </Info>

```

将组件从简单主题导入到站点

```
export {default as Info} from './src/components/info' 
export {default as color} from './src/tokens/color'
```

### 构建主题的整个目的是为了能够在站点中使用它的属性。现在，我们将在站点中创建一些页面，并按照以下步骤将主题的布局导入到样式中:

1.创建一个`src/pages/info.mdx`目录

2.从简单主题的`index.js`中导出信息组件

3.导入到`info.mdx`站点目录中

```
export {default as Info} from './src/components/info'
```

就这样，我们使用了主题中的组件。

```
import {Info} from 'simple-theme' 
#More Information about me 
<Info> 
  I like to write a lot of poetry
 </Info>
```

在你的主题中使用设计符号

### 假设我们有一些主题自定义样式，我们希望在默认情况下被我们的主题识别，我们还希望给用户覆盖这些样式的能力。我们可以在主题中创建一个令牌目录，在该目录下有我们的默认颜色配置:

为了将样式导入到`index.js `组件，我们重构了组件以实现导入:

```
export default {
primary : 'blue'
}
```

组件阴影

```
import React from 'react' 
import color from '../tokens/color';
 const Info = ({children}) =>{
   return( 
      <div 
          style = {{ 
              padding: "1rem", 
              backgroundColor: color.primary 
            }} > 
        {children} 
          </div> ); 
      }
   export default Info
```

### 这个概念允许我们根据自己的喜好修改部分主题，我们可以通过创建一个本地文件(以正在使用的主题命名)在我们的站点目录中进行渲染。这样，我们放入文件夹的任何文件都将优先于主题默认值。

在站点的`src `中创建一个简单主题文件夹

*   在新创建的文件夹中导出您喜欢的颜色
*   因此，我们现在有一个页面呈现文本，包裹在信息文本周围的部分为棕色。

```
export default {
primary: 'brown'
};

```

我们甚至可以覆盖网站上的组件。首先，我们必须在我们的站点目录中创建相同的组件文件夹。姑且称之为`simple-theme/components/info.js`。

更多来自 LogRocket 的精彩文章:

* * *

### *注意:要访问要修改的定制组件，我们必须从主题*的 index.js 导出

* * *

> 我们的`index.js `变成了:

```
export {default as Info} from './src/components/info'
export {default as color} from './src/tokens/color'
```

在同一个网站上使用两个主题

```
import React from 'react' 
import {color} from 'simple-theme'
 const Info = ({children}) =>{
  return( 
      <div style = {{ 
                padding: "10rem",
                 background : color.primary,
                 fontSize : '30px' 
        }} > 
            {children} 
        </div> ); 
 } 
 export default Info
```

### 之前，我提到过我们可以使用不止一个主题；因此，我们将建立一个子主题，它也依赖于简单主题。我将调用我的“bat-theme”，在这里我们通过运行`yarn init`初始化 yarn，并用所需的主题依赖设置`package.json`。

我们添加了 simple-theme 作为 bat-theme 的依赖项，因为我们希望它连接到父主题，这样即使用户安装了 bat-theme，他们也可以访问 simple-theme 的特性。

```
{ 
"name": "bat-theme", 
"version": "0.0.1", 
"main": "index.js", 
"dependencies": {
 "gatsby-plugin-page-creator": "^2.0.9", 
 "simple-theme": "*"
 },
 "devDependencies": { 
  "gatsby": "^2.1.19",
 "react": "^16.8.3",
 "react-dom": "^16.8.3"
 }, 
"peerDependencies": { 
  "gatsby": "^2.1.19", 
  "react": "^16.8.3", 
  "react-dom": "^16.8.3" 
}, 
"license": "MIT" 
}
```

然后，我们可以创建一个 gatsby-config.js 来将父主题导入到子主题中，就像我们连接到站点并解析 index.js 中的 package.json 一样:

我们可以通过修改站点的`gatsby-config.js`和`package.json`来测试子主题是否正常工作，以使用链接到简单主题的 bat-cave 主题。为了检查这一切是如何相互作用的，我们运行`yarn workspaces info `,我们应该有这样的显示:

```
module.exports = {
__experimentalThemes:[‘simple-theme’]
};

```

![](img/3f10c043b0b44816e5b67df003dbe600.png)

我们可以看到蝙蝠主题依赖于简单主题，而简单主题又是网站所依赖的。

创建主题的常见错误

### 在准备这篇文章时，我遇到了一些错误:

**错别字**:在陈述像“_ _ experimental themes”“plugins”这样的关键词时要小心。我联系了盖茨比团队，[在 GitHub 上发布了一个问题](https://github.com/gatsbyjs/gatsby/issues/12291)，以帮助在出现错别字时突出显示这些错误，所以我相信你会没事的

*   我还遇到了一个错误。mdx 文件没有显示在浏览器上，结果是 Gatsby 有一些特定于 windows 操作系统的错误。问题在于，由于 windows 和 unix 文件路径的差异，webpack 加载器规则并不适用。你可以在这里找到补丁
*   在这里找到 windows 错误[的修复方法](https://github.com/ChristopherBiscardi/gatsby-mdx/commit/c672ef5b9a80f69a81aed9672e187fefc461b5b5)
*   在不久的将来会有更多的 Gatsby 进展

### 从盖茨比主题中我们应该期待的事情:

不需要一直清理缓存

*   组件阴影可以与主题合并
*   结论

### 我真的很喜欢解释盖茨比主题是如何工作的。主题的众多优点之一是你可以一次安装多个主题到你的项目中。您还可以构建许多主题来帮助其他开发人员改进工作流程。

构建主题最令人兴奋的部分是编写 MDX，它确实名副其实。我希望和盖茨比建立一些主题，我也希望你这样做🙂如果你对此感兴趣，我建议你观看杰森和约翰的完整视频。[这里的](https://github.com/Ekwuno/Scotch-gastby-theme-demo)是本文回购的链接。编码快乐！

[LogRocket](https://lp.logrocket.com/blg/react-signup-general) :全面了解您的生产 React 应用

## 调试 React 应用程序可能很困难，尤其是当用户遇到难以重现的问题时。如果您对监视和跟踪 Redux 状态、自动显示 JavaScript 错误以及跟踪缓慢的网络请求和组件加载时间感兴趣，

.

[try LogRocket](https://lp.logrocket.com/blg/react-signup-general)

LogRocket 结合了会话回放、产品分析和错误跟踪，使软件团队能够创建理想的 web 和移动产品体验。这对你来说意味着什么？

[![](img/f300c244a1a1cf916df8b4cb02bec6c6.png) ](https://lp.logrocket.com/blg/react-signup-general) [![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/react-signup-general) 

LogRocket 不是猜测错误发生的原因，也不是要求用户提供截图和日志转储，而是让您回放问题，就像它们发生在您自己的浏览器中一样，以快速了解哪里出错了。

不再有嘈杂的警报。智能错误跟踪允许您对问题进行分类，然后从中学习。获得有影响的用户问题的通知，而不是误报。警报越少，有用的信号越多。

LogRocket Redux 中间件包为您的用户会话增加了一层额外的可见性。LogRocket 记录 Redux 存储中的所有操作和状态。

现代化您调试 React 应用的方式— [开始免费监控](https://lp.logrocket.com/blg/react-signup-general)。

Modernize how you debug your React apps — [start monitoring for free](https://lp.logrocket.com/blg/react-signup-general).