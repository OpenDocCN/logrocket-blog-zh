# 使用 Vue 测试工具测试 Vue.js 组件

> 原文：<https://blog.logrocket.com/testing-vue-js-components-vue-test-utils/>

我知道，对于许多开发人员来说，测试似乎是浪费时间。你讨厌它，对吗？但是，你应该吗？如果你想创建一个可靠的应用程序，你应该测试你的组件吗？

我将告诉你我的想法:测试你的组件(并且，重要的是，以正确的方式去做)是你能做的最好的投资之一，如果你正在构建一些长期的东西。但是为什么呢？

在本指南中，我将回答这些问题，并通过分享几个发生在我身上的故事来总结使用 [Vue 测试工具](https://vue-test-utils.vuejs.org/)测试你的 Vue.js 组件的好处。🤫

我们将讨论以下内容:

## 为什么要测试 Vue.js 组件？

当您将代码推向生产时，您不希望引入 bug。如果你是一个对你的代码库了如指掌的天才开发人员，你可能不会(也就是说，我已经看到很多优秀的、自信的工程师引入了他们没有预见到的罕见情况)。

但是，当你被大量的工作压得喘不过气来，因为你的公司在发展，你需要雇用一些初级员工来不断改进产品，会发生什么呢？他们会引入 bug 吗？可能比你想象的更频繁。

在理想情况下，当初级开发人员推出破坏您最重要的特性之一的东西时，您会希望在它影响到产品服务器之前得到通知。如果你的代码库被正确测试，其中一个测试将会失败，你将能够在任何损害发生之前修复这个问题。

这就是为什么如果你正在构建一个长期项目，你应该测试你的代码库的一个重要原因:开发人员在一个团队中工作，必须互相保护。一些公司甚至通过在他们的工作流程中引入诸如[测试驱动开发(TDD)](https://blog.logrocket.com/a-low-friction-way-to-do-tdd-with-react/) 这样的方法来改变他们的编码方式。简而言之，这意味着您在编码业务逻辑之前编写测试(即规范)。

您应该测试组件是否正常工作的另一个原因是，这样做可以为每个组件提供文档。通过阅读测试(我们将在接下来的部分中演示)，我们可以看到对于给定的输入(一个道具、一个事件等)，我们可以期望什么样的输出。).而且，您可能已经知道，优秀的文档会使调试变得更容易。😃📖

但是如果你问我我最喜欢测试的哪一点，那就是重构可以变得多产。几年前，当我开始走上成为一名 web 开发人员的独特道路时，我很快认识到代码库不是静态的，会随着时间的推移而发生很大变化。换句话说，你必须每周重构它的一部分。

我记得产品经理让我在一个最关键的界面中引入一个子功能。对我来说不幸的是，它需要对许多组件进行完整的重构才能工作。我害怕打破什么东西，但这种恐惧很快就消失了。在我完成重构之后，我非常高兴地看到所有的测试都通过了，没有触发任何错误。

自信是关键！事实上，这是测试 Vue.js 组件的另一个好处。当您确信您的代码工作正常时，您就可以确信您没有发布损坏的软件。😇

如果你仍然不相信，这里有更多的食物供你思考:解决问题通常比预防问题要昂贵得多。编写测试所花费的时间是值得的。

## 应该如何测试 Vue.js 组件？

讨论我们应该测试什么是必要的。对于 UI 组件，我不建议测试每一行代码。这可能会导致过于关注组件的内部实现(例如，达到 100%的测试覆盖率)。

相反，我们应该编写断言组件公共接口的测试，并将其视为内部黑盒。一个单独的测试用例将断言提供给组件的一些输入(用户动作、道具、存储)会产生预期的输出(组件渲染、vue 事件、函数调用等)。).

此外，去年，我在 Vue Amsterdam 观看了 Sarah Dayan 的精彩演讲，题目是“[用 Vue.js 进行测试驱动开发](https://www.youtube.com/watch?v=LXR1DRm-Gzo)”。在她的一张幻灯片中，她说要决定你是否应该测试你的一个组件(或者其中的一个特性)，你必须问你自己:如果它改变了，我会关心它吗？换句话说，如果有人破坏了这个特性，它是否会导致界面出现问题？如果是这样，您应该编写一个测试来增强您的代码。

## 什么是 Vue 测试工具？

现在我们来谈谈房间里的大象。什么是 Vue 测试工具？🤔

Vue Test Utils 是一个官方的帮助函数库，帮助用户测试他们的 Vue.js 组件。它提供了一些以隔离方式挂载 Vue.js 组件并与之交互的方法。我们称之为包装。但是什么是包装呢？

包装器是已安装组件的抽象。它提供了一些实用功能，让我们的生活变得更容易，比如当我们想要触发一次点击或一个事件时。我们将使用它来执行一些输入(用户动作、道具、商店更改等)。)这样我们就可以检查输出是否正确(组件渲染、Vue 事件、函数调用等)。).

值得注意的是，如果包装器上没有您需要的东西，您可以用`wrapper.vm`获取 Vue 实例。所以你有很大的灵活性。

您可以在官方的 [Vue Test Utils 文档](https://vue-test-utils.vuejs.org/api/wrapper/#properties)中找到包装器上可用的所有属性和方法。

Vue Test Utils 也允许用`shallowMount`或单独的存根来呈现模仿和存根组件，但是我们将在后面讨论。所以是的，这是一个非常完整可靠的库，你会喜欢的。😍

## 使用 Vue 测试工具测试 Vue.js 组件

现在是时候动手了，开始用 Vue 测试工具测试我们的组件。

### 建立基础设施

你可以在两位试跑者中进行选择: [Jest](https://blog.logrocket.com/testing-with-jest-from-zero-to-hero-85ce0e9cc953/) 或者[摩卡和柴](https://blog.logrocket.com/how-to-test-strapi-endpoints-with-mocha-and-chai/)。在本教程中，我们将使用 Jest，因为推荐使用带有 [Jest](https://jestjs.io/) 的 Vue 测试工具。

如果你不熟悉 Jest，这是一款由脸书开发的测试版。它旨在提供一个包含电池的单元测试解决方案。

如果您正在使用 Vue CLI 构建您的项目，以下是如何在您当前的 Vue 应用程序中设置 Vue 测试工具。

> 对于手动安装，请遵循文档中的[安装指南](https://vue-test-utils.vuejs.org/installation/#using-vue-test-utils-with-jest-recommended)。

```
vue add unit-jest
npm install --save-dev @vue/test-utils

```

您现在应该看到一个新的命令被添加到`package.json`中，我们将使用它来运行我们的测试。

```
{
  "scripts": {
    "test:unit": "vue-cli-service test:unit"
  }
}

```

### 测试我们的`HabitComponent`

现在是时候创建我们的第一套测试了。对于我们的例子，我们将创建一个习惯跟踪器。它将由一个单独的部分组成，我们将其命名为`Habit.vue`，我们将在每次完成习惯时勾选它。在组件文件夹中，复制/粘贴以下代码:

```
<template>
  <div class="habit">
    <span class="habit__name">{{ name }}</span>
    <span :class="{ 'habit__box--done': done }" class="habit__box" @click="onHabitDone">
      <span v-if="done">✔</span>
    </span>
  </div>
</template>
<script>
export default {
  name: "Habit",
  props: {
    name: {
      type: String,
      required: true,
    },
  },
  data: () => ({
    done: false,
  }),
  methods: {
    onHabitDone() {
      this.done = !this.done;
    },
  },
};
</script>
<style>
.habit {
  height: 100vh;
  width: 100%;
  display: flex;
  text-align: center;
  justify-content: center;
  align-items: center;
  text-transform: uppercase;
  font-family: ui-sans-serif, system-ui;
}
.habit__name {
  font-weight: bold;
  font-size: 64px;
  margin-right: 20px;
}
.habit__box {
  width: 56px;
  height: 56px;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 4px solid #cbd5e0;
  background-color: #ffffff;
  font-size: 40px;
  cursor: pointer;
  border-radius: 10px;
}
.habit__box--done {
  border-color: #22543d;
  background-color: #2f855a;
  color: white;
}
</style>

```

![Learn Something New Unchecked](img/219b3f8b128065c7962f62673cd91ee6.png)

![Learn Something New Checked](img/e028c6f784055b7ba99f17ad8f7452ac.png)

该组件接受一个单独的道具(习惯的标题)，并包含一个当我们点击它时变成绿色的框(即，习惯完成)。

在项目根目录下的`tests`文件夹中，创建一个`Habit.spec.js`。我们将在其中编写所有的测试。

让我们从创建包装对象开始，并编写我们的第一个测试。

```
import { mount } from "@vue/test-utils";
import Habit from "@/components/Habit";
describe("Habit", () => {
  it("makes sure the habit name is rendered", () => {
    const habitName = "Learn something new";
    const wrapper = mount(Habit, {
      propsData: {
        name: habitName,
      },
    });
    expect(wrapper.props().name).toBe(habitName);
    expect(wrapper.text()).toContain(habitName);
  });
});

```

如果您运行`npm run test:unit`，您应该看到所有测试都成功了。

```
> vue-cli-service test:unit
 PASS  tests/unit/Habit.spec.js
  Habit
    ✓ makes sure the habit name is rendered (11ms)
Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        1.135s
Ran all test suites.

```

现在，让我们确保当我们单击该框时，我们的习惯被选中。

```
it("marks the habit as completed", async () => {
  const wrapper = mount(Habit, {
    propsData: {
      name: "Learn something new",
    },
  });
  const box = wrapper.find(".habit__box");
  await box.trigger("click");
  expect(box.text()).toContain("✔");
});

```

注意测试必须是异步的，并且需要等待触发器。查看 Vue Test Utils 文档中的“[测试异步行为](https://vue-test-utils.vuejs.org/guides/#testing-asynchronous-behavior)”一文，了解为什么这是必要的，以及在测试异步场景时需要考虑的其他事项。

```
> vue-cli-service test:unit
 PASS  tests/unit/Habit.spec.js
  Habit
    ✓ makes sure the habit name is rendered (11ms)
    ✓ marks the habit as completed (10ms)
Test Suites: 1 passed, 1 total
Tests:       2 passed, 2 total
Snapshots:   0 total
Time:        1.135s
Ran all test suites.

```

我们还可以验证当我们点击它时是否调用了`onHabitDone`方法。

```
it("calls the onHabitDone method", async () => {
  const wrapper = mount(Habit, {
    propsData: {
      name: "Learn something new",
    },
  });
  wrapper.setMethods({
    onHabitDone: jest.fn(),
  });
  const box = wrapper.find(".habit__box");
  await box.trigger("click");
  expect(wrapper.vm.onHabitDone).toHaveBeenCalled();
});

```

运行`npm run test:unit`，一切都应该是绿色的。

下面是您应该在终端中看到的内容:

```
> vue-cli-service test:unit
 PASS  tests/unit/Habit.spec.js
  Habit
    ✓ makes sure the habit name is rendered (11ms)
    ✓ marks the habit as completed (10ms)
    ✓ calls the onHabitDone method (2ms)
Test Suites: 1 passed, 1 total
Tests:       3 passed, 3 total
Snapshots:   0 total
Time:        1.135s
Ran all test suites.

```

当我们改变一个道具时，我们甚至可以检查组件的行为是否符合预期。

```
it("updates the habit method", async () => {
  const wrapper = mount(Habit, {
    propsData: {
      name: "Learn something new",
    },
  });
  const newHabitName = "Brush my teeth";
  await wrapper.setProps({
    name: newHabitName,
  });
  expect(wrapper.props().name).toBe(newHabitName);
});

```

下面是您应该在终端中看到的内容:

```
> vue-cli-service test:unit
 PASS  tests/unit/Habit.spec.js
  Habit
    ✓ makes sure the habit name is rendered (11ms)
    ✓ marks the habit as completed (10ms)
    ✓ calls the onHabitDone method (2ms)
    ✓ updates the habit method (2ms)
Test Suites: 1 passed, 1 total
Tests:       4 passed, 4 total
Snapshots:   0 total
Time:        1.135s
Ran all test suites.

```

为了帮助你更快地编码，下面是我最常用的包装方法:

*   `wrapper.attributes()`:返回包装器 DOM 节点属性对象
*   `wrapper.classes()`:返回包装器 DOM 节点类
*   `wrapper.destroy()`:销毁一个 Vue 组件实例
*   `wrapper.emitted()`:返回一个包含包装器 vm 发出的自定义事件的对象
*   `wrapper.find()`:返回第一个 DOM 节点或 Vue 组件匹配选择器的包装器
*   `wrapper.findAll()`:返回一个 WrapperArray
*   `wrapper.html()`:以字符串形式返回包装器 DOM 节点的 HTML
*   `wrapper.isVisible()`:断言包装器可见
*   `wrapper.setData()`:设置包装虚拟机数据
*   `wrapper.setProps()`:设置包装虚拟机属性并强制更新
*   `wrapper.text()`:返回包装器的文本内容
*   `wrapper.trigger()`:在包装器 DOM 节点上异步触发事件

### 使用`fetch`

如果您在组件内部使用`fetch`方法来调用 API，您将会得到一个错误。以下是如何确保在测试过程中定义了`fetch`。

```
npm install -D isomorphic-fetch

```

然后更新你的`package.json`。

```
{
  "scripts": {
    "test:unit": "vue-cli-service test:unit --require isomorphic-fetch"
  }
}

```

### `mount`对`shallowMount`

你可能会发现有些人在用`shallowMount`而不是`mount`。原因是，像`mount`一样，它创建了一个包含已挂载和渲染的 Vue.js 组件的包装器，但是带有存根子组件。

这意味着组件的渲染速度会更快，因为它的所有子组件都不会被计算。不过要小心；如果您试图测试与儿童组件相关的东西，这种方法可能会带来一些麻烦。

## 我们将何去何从？

Vue Test Utils 文档是帮助您开始的很好的资源——尤其是每月更新的[指南](https://vue-test-utils.vuejs.org/guides/#testing-asynchronous-behavior)。包含所有[包装方法](https://vue-test-utils.vuejs.org/api/wrapper/)和[Jest API](https://jestjs.io/docs/api)的页面都是非常好的资源，你也应该把它们收藏起来。

记住，为你的项目练习和编写测试是开始学习的最好方式。我希望这个指南能帮助你理解你的组件测试有多健壮。这并不难。😃

我们将以著名计算机科学家唐纳德·克努特的一句话来结束本指南:“计算机擅长遵循指令，但不擅长阅读你的思想。”

我很乐意阅读你的评论和你的推特信息。如果你对我的作品感兴趣，你可以去 NadaRifki.com 的[看看。](https://www.nadarifki.com/)

## 像用户一样体验您的 Vue 应用

调试 Vue.js 应用程序可能会很困难，尤其是当用户会话期间有几十个(如果不是几百个)突变时。如果您对监视和跟踪生产中所有用户的 Vue 突变感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/vue-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/0d269845910c723dd7df26adab9289cb.png)](https://lp.logrocket.com/blg/vue-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/vue-signup)

LogRocket 就像是网络和移动应用程序的 DVR，记录你的 Vue 应用程序中发生的一切，包括网络请求、JavaScript 错误、性能问题等等。您可以汇总并报告问题发生时应用程序的状态，而不是猜测问题发生的原因。

LogRocket Vuex 插件将 Vuex 突变记录到 LogRocket 控制台，为您提供导致错误的环境，以及出现问题时应用程序的状态。

现代化您调试 Vue 应用的方式- [开始免费监控](https://lp.logrocket.com/blg/vue-signup)。