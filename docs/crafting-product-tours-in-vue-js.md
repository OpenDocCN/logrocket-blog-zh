# 在 Vue.js 中制作产品游览

> 原文：<https://blog.logrocket.com/crafting-product-tours-in-vue-js/>

## 介绍

产品导览用于让新用户和现有用户熟悉您的应用程序界面，并引导他们采取有意义的行动，帮助他们充分利用您的应用程序。

在本文中，我将带您了解如何设计产品之旅，帮助您将应用程序的新用户转化为长期用户或客户。

## 为什么要使用产品旅游？

所有的软件，尤其是同类中的新产品，通常都有一个学习曲线。作为产品所有者，你的目标是通过向用户展示如何有效地使用你的产品及其特性来帮助缩短这个曲线。这可以在产品参观的帮助下完成。

产品之旅也有助于用户保持和转化，因为大多数不能浏览你的应用程序的用户界面的用户会感到沮丧，转而使用你的竞争对手的产品。

请注意，产品参观不能替代优秀的用户界面设计，所以考虑投资高质量的设计也很重要。

虽然产品参观是一个很好的工具，但如果执行不好，它们会很快让新用户感到沮丧，阻止他们发现你产品的核心价值，使他们更有可能过早地放弃你的产品。关键是专注于你的产品的价值和用户目标，而不是在每一个功能上拖着用户。在这篇文章中，我们将涵盖设计有效的产品之旅的技巧。

## 创建产品游览的最佳实践

创建令人敬畏的产品之旅不是火箭科学，但它确实应用了科学原理，因为你必须在遵循基本设计原则的同时，对你的旅程进行多次测试和迭代，以找出什么对你的用户最好。让我们来看看一些有用的提示，好吗？

### 使其成为可选的

不是每个用户都喜欢被教导如何使用产品。他们中的一些人通过摆弄你的用户界面和自己解决问题来学习，强迫他们参观产品会降低他们的兴趣。为此，考虑在你的旅程中添加一个“跳过”按钮。请注意，在某些情况下，添加产品参观可能是至关重要的，但这取决于您的产品类型。

### 亲吻(保持简短)

在创建产品旅游时，不要试图将事情过于复杂。并不是每个新用户都渴望使用你的产品，所以尽量尊重他们的时间，保持简短。

你可以通过简化你的产品之旅来实现这一点，只包含用户需要的关键步骤，以便熟悉你的产品并充分利用它。

### 循序渐进

随意突出显示和工具提示并不是向用户展示你尊重他们的时间的最好方式，而且会让人感到困惑。尽量让你的产品之旅遵循一个特定的顺序。这会让你的用户渴望探索你的产品。

### 有暗示性

大多数情况下，你可以简单地向用户建议他们应该尝试的功能，或者在他们执行了某个特定的动作或设置后应该知道的功能，而不是一次向他们展示你的产品的所有功能。这鼓励他们[通过自我发现](https://en.wikipedia.org/wiki/Discovery_learning)来学习，这可以说是最好的学习方法之一。

### 提供价值

试着只关注你产品的核心内容，而不是让你的用户厌烦每一个小细节。80/20 原则也适用于这种情况，即大多数用户只会使用产品 20%的功能来满足他们 80%的需求。这意味着你应该找出你提供的那 20%的产品是什么，并在你的旅游中突出它。

## 使用`vue-tour`制作产品旅游

在 Vue.js 中，我们可以使用成吨的库来制作我们的产品之旅，但是我们将看看如何使用`vue-tour`库。`vue-tour`是使用最广泛的，每周下载 20，000 次，Github 星级 2，000 个。它也很容易启动和运行，并提供了大量的定制。

为了练习如何在 Vue.js 中制作产品导览，我们将使用`vue-tour`库为现有的仪表板应用程序添加产品导览功能。

你可以在这里找到 Github 为[制作产品旅游的知识库。](https://github.com/codiini/product-tour-demo)

请注意，存储库有两个分支:一个是最初的`main`分支，另一个是最终产品的`product-tour`分支。

### 入门指南

首先，让我们将存储库分支并克隆到我们的本地机器上。

然后，我们使用以下命令安装节点模块:

```
yarn install 
#OR if you prefer using npm
npm install

```

接下来，我们通过运行下面的命令来安装`vue-tour`库:

```
yarn add vue-tour
#OR
npm install vue-tour

```

不幸的是，`vue-tour`库还不兼容 Vue 3。这是正在进行的工作，将由项目维护者在即将到来的更新中发布，但现在，我们将在 Vue 2 中使用它。

在您的`main.js`文件中添加以下代码行。这将导入默认样式的`vue-tour`库，并指定我们希望在应用程序中全局使用该库:

```
import VueTour from 'vue-tour';
import 'vue-tour/dist/vue-tour.css';
Vue.use(VueTour)

```

### 使用`vue-tour`

首先，我们必须添加一个惟一的类名、ID 名或数据属性到我们希望我们的旅程指向的元素中。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

通过`src` > `views` > `dashboard` > `DashboardHome`导航至`DashboardHome.vue`:

```
<template>
  <v-main>
    <v-container>
      <div class="lg:flex">
        <v-container class="left-side">
          <v-row>
            <v-col cols="12">
              <h2 class="text-xl">Hello, Iniubong!</h2>
              <p class="text-sm mt-2 text-gray-400">
                Let's travel around the world again
              </p>
            </v-col>
            <v-col cols="6">
              <v-card
                id="v-step-0"
                height="150px"
                class="rounded-lg text-center cursor-pointer"
              >
                <router-link :to="'/dashboard/plan-trip'">
                  <div>
                    <v-avatar class="icon avatar-badge mt-6" size="50">
                      <v-icon color="primary">mdi-plus</v-icon>
                    </v-avatar>
                    <p class="mt-3">Add Trip</p>
                  </div>
                </router-link>
              </v-card>
            </v-col>
            <v-col cols="6">
              <v-card
                height="150px"
                class="v-step-1 rounded-lg text-center cursor-pointer">
                <router-link :to="'/dashboard/fund-wallet'">
                  <div>
                    <v-avatar class="icon avatar-badge mt-6" size="50">
                      <v-icon color="primary">mdi-plus</v-icon>
                    </v-avatar>
                    <p class="mt-3">Fund Wallet</p>
                  </div>
                </router-link>
              </v-card>
            </v-col>
            <v-col cols="12">
              <v-card height="300px" class="rounded-lg">
                <v-card-title>Popular Destinations</v-card-title>
                <v-container>
                  <PopularPlacesSlider />
                </v-container>
              </v-card>
            </v-col>
          </v-row>
        </v-container>
        <v-container class="right-side">
          <v-row>
            <v-flex class="flex-col">
              <v-col>
                <div class="flex justify-between items-center">
                  <p>Upcoming Trips</p>
                  <div>
                    <router-link
                      :to="'/dashboard/upcoming-trips'"
                      class="flex justify-end items-start">
                      <p>See all</p>
                      <v-icon>mdi-arrow-right</v-icon>
                    </router-link>
                  </div>
                </div>
              </v-col>
              <v-col cols="12">
                <UpcomingTrips>
                  <template v-slot:trip-name>Desert Surfing</template>
                  <template v-slot:trip-duration
                    >30th June 2021 - 20th July 2021</template
                  >
                </UpcomingTrips>
              </v-col>
              <v-col cols="12">
                <UpcomingTrips>
                  <template v-slot:trip-name>Flexing in Hawaii</template>
                  <template v-slot:trip-duration
                    >30th June 2021 - 20th July 2021</template
                  >
                </UpcomingTrips>
              </v-col>
              <v-col data-v-step="2">
                <VectorMap />
              </v-col>
            </v-flex>
          </v-row>
        </v-container>
      </div>
    </v-container>
  </v-main>
</template>

```

接下来，我们将步骤定义为组件的数据属性、Vuex 存储或任何其他地方的对象数组。

每个对象数组至少应该包含目标元素，以及您希望包含的内容。还可以进行一些其他定制，例如工具提示或标题的放置:

```
data() {
  return {
     steps: [
      {
        target: '#v-step-0', 
        header: {
          title: 'Plan Trip',
        },
        content: `Click here to plan a solo trip or with family and friends`
      },
      {
        target: '.v-step-1',
        content: 'Add funds to your wallet or setup up a schedule to save up for that big trip you\'ve always planned for.'

      },
      {
        target: '[data-tour-step="2"]',
        // You can use HTML tags. Even emojis too 😏
        content: 'Here\'s a map showing all of your travels as you go on amazing journies to amazing places.<br> Click on each point to relieve memories from each trip. &#127776;',
        params: {
           // This sets the placement of the popup
          placement: 'top'
        }
      }
    ]
  };
},

```

### 将产品参观添加到项目中

接下来，为了让我们的产品之旅显示在我们的页面上，我们添加了以下代码行:

```
    <v-tour name="myTour" :steps="steps"></v-tour>

```

最后一步是在我们的`mounted`钩子中添加下面一行代码，或者在我们希望旅程开始的任何时候添加:

```
this.$tours['myTour'].start()

```

在我们的例子中，我们希望在用户加载页面时开始浏览，所以我们将它加载到如下所示的`mounted`钩子中:

```
  mounted: function () {
    this.$tours['myTour'].start()
  }

```

好了，我们的产品之旅开始了。显然这不是什么新奇的东西，但我们已经学会了如何建造一个。

从这里，您可以调整和定制这个项目或您自己的产品之旅，以最适合您和您的用户。

更多关于如何使用和定制`vue-tour`的信息，请点击查看他们的[文档。](https://pulsardev.github.io/vue-tour/)

## 结论

在本教程中，我们回顾了产品之旅的定义和重要性。我们还讨论了创建真正好的产品之旅的一些最佳实践，有助于提高用户转化率和保留率。我们还练习了向我们的 Vue.js 应用程序添加产品游览功能。

## 像用户一样体验您的 Vue 应用

调试 Vue.js 应用程序可能会很困难，尤其是当用户会话期间有几十个(如果不是几百个)突变时。如果您对监视和跟踪生产中所有用户的 Vue 突变感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/vue-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/0d269845910c723dd7df26adab9289cb.png)](https://lp.logrocket.com/blg/vue-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/vue-signup)

LogRocket 就像是网络和移动应用程序的 DVR，记录你的 Vue 应用程序中发生的一切，包括网络请求、JavaScript 错误、性能问题等等。您可以汇总并报告问题发生时应用程序的状态，而不是猜测问题发生的原因。

LogRocket Vuex 插件将 Vuex 突变记录到 LogRocket 控制台，为您提供导致错误的环境，以及出现问题时应用程序的状态。

现代化您调试 Vue 应用的方式- [开始免费监控](https://lp.logrocket.com/blg/vue-signup)。