# 取代你的 UI 框架的(大部分)无痛指南

> 原文：<https://blog.logrocket.com/guide-replacing-ui-framework/>

更换你的 UI 框架是一项大工程，这并不奇怪。重写数百个视图需要时间、协调和决心。但是用正确的技术，你可以推翻这个巨大的任务。

在 [Retail Zipline](https://retail-zipline.breezy.hr) ，我们开始替换和整合我们现有的叛逆 UI 框架集合。使用这些流程，我们和一个三人核心团队在不到两个月的时间内迁移了 650 个视图。

## 1.)目标和版本

显然，如果与公司目标一致，替换 UI 框架只是开发人员时间的一种有价值的使用。你首先需要问的是:

1.  为什么要更换框架？
2.  这个项目为谁服务？
3.  是以团队为中心还是以客户为中心？

选择一个明确的目标将会塑造这个项目，并为可以削减的内容提供指导。你个人可能非常渴望提高你的上传器或所见即所得编辑器的 UX，但是如果框架不需要这样做，那就把它留给另一个项目吧。此外，如果框架需要的话，我建议单独升级那些较小的项目，这样框架项目就有重点了。

我们考虑了升级技术债务。开发团队是捐助者，所以我们尽可能保持相同的视觉设计。这意味着我们没有添加新的功能，没有修复现有的 UI 错误，也没有改变页面层次结构。我们的用户几乎不会注意到任何变化。

当时，我们有三个独立的 UI 框架:Bootstrap 3，来自应用程序最初创建的时候；几个定制的 BEM 风格的组件；以及自定义实用程序类，如 [Tailwind CSS](https://tailwindcss.com) 。

构建后端特性很简单，但是设计决策阻碍了进展。模式的缺乏隐含地鼓励我们为每个特性编写新的设计。替换框架不会解决不匹配的模式，但是它会将所有视图带到相同的基线，并提供模式选项。

没有所谓的*完美的 UI 框架*——相反，选择一个满足项目需求并适合你的团队如何完成工作的框架。你最不想做的事情就是浪费时间和一个框架战斗，因为团队中的每个人都觉得这很尴尬。

我们选择 [Bootstrap 4](https://getbootstrap.com) 是因为我们需要支持 IE 11 不想从头开始创建自定义组件；并且拥有一个不想成为设计瓶颈的小型前端团队。不管您选择什么样的框架，我们采用的方法都会有所帮助。

一旦目标到位，将工作分解成更小的项目。什么可以完全排除？哪些可以小批量发布？也许可以分阶段替换这个框架，这样你就可以更快地发布它。在 Rails 整体中，资产包可能会自然断裂；在微服务中，每个服务。特定于特定用户原型的区域可能是另一个切割。

我们的应用程序是一个庞大的整体，有几个支持服务，比如邮件程序和 iFrame 小部件，以及基于用户类型的四个不同的区域。我们完全排除了服务，因为它们使用单独的资产包。然后，按不同的部分拆分发布。我们还从初始范围中排除了我们的管理区域——它本身有 170 个视图。

我们细化了目标，将每个面向客户的区域升级为一个独立的版本，并在之后升级其余的视图。考虑正交工作，并根据依赖关系划分版本，有助于提高发布速度。

## 2.)方法和设置

主要框架经常与竞争对手甚至之前的版本不兼容。考虑你的应用程序的新 UI *版本 2* 而不是可以与旧 UI 共存的东西，让你的生活更轻松。我们的 UI 框架集合是在试图一点一点地取代以前的失败尝试中诞生的。不是做不到，但是工作进行中的状态很慢，让人士气低落。

我们创建了一个额外的`views_v2`文件夹，存放所有升级的视图，一个[视图解析器](https://github.com/retailzipline/bootstrap-tooling/blob/master/app/controllers/concerns/controller_view_v2_paths.rb)，呈现新的视图并退回到`views`，以及`v2` CSS 和 JavaScript 包。

如果你有几个包，像我们一样，只分离那些必要的。我们的`vendor`包导入了 Bootstrap 3，所以我们创建了`vendor_v2`导入 Bootstrap 4，同时在`views`和`views_v2`中导入了相同的`application`包。如果您的视图是用 JavaScript 编写和呈现的，您可能不需要单独的视图文件夹，但是您会希望将它们包含在单独的资产包中。

设置就绪后，我们现在可以开始在一个全新的环境中构建新的视图。从头开始构建更容易，对吗？也许不是。

## 3.)使用正确的工具

手动重写每一个视图是一个累人的、乏味的过程。很快，很明显，如果我们不开始自动化这个过程，我们将在这个升级上花费我们的余生，所以我们写了一个[小工具库](https://github.com/retailzipline/bootstrap-tooling)来加速它。

### 状态检查器

检查剩余工作的状态对于估计、计划和完成很重要。我们创建了`[bs4_migration_check](https://github.com/retailzipline/bootstrap-tooling/blob/master/bin/bs4_migration_check)`来按部分报告剩余的视图。这意味着我们可以更好地了解一个部分可能需要多长时间，并可以确保没有任何遗漏或遗忘。

### 自动升级

因为 UI 框架大多是用 CSS 类构建的，所以您可以自动替换名称。这将工作转变为编辑，而不是从头开始写，这要容易得多。我们创建了两个工具，`bs4_start`和`bs4_ugrade`，一起使用。

[首先](https://github.com/retailzipline/bootstrap-tooling/blob/master/bin/bs4_start)将一个给定的工作子文件夹复制到`views_v2`并提交文件。这为回顾`views`中的内容设置了一个基线。

第二个[用查找和替换来改变所有容易切换的类名，并提醒那些需要更多关注的类名。例如，我们之前使用了`.flex`来制作 flexbox 容器，而 Bootstrap 使用了`.d-flex`。从那时起，工作就是一个编辑过程，我们只需要修复错误。](https://github.com/retailzipline/bootstrap-tooling/blob/master/bin/bs4_upgrade)

### 功能标志

功能标志确保在准备就绪之前不会影响客户。我们标记了我们所有的工作，并且尽可能快地合并到 master 中，这样其他团队就可以处理新提交的视图，并且我们可以避免在项目结束时出现大的合并冲突。

[视图解析器](https://github.com/retailzipline/bootstrap-tooling/blob/master/app/controllers/concerns/controller_view_v2_paths.rb)在每个动作或每个控制器的基础上启用新视图，并通过[黑暗启动](https://launchdarkly.com)功能标志阻止客户使用。

在 JavaScript 中，我们创建了一个全局变量`window.CONFIG.bs4`，用于升级发生变化的库中的 API 调用。例如，Bootstrap 3 使用`destroy`来清理事件，而 Bootstrap 4 使用`dispose`。在特定的 JavaScript 文件中使用这个标志意味着我们可以在两个应用程序版本中使用相同的包。

### 自动化屏幕截图测试

虽然包含一些自动截图测试看起来很有用，但我们发现差异如此之大，最终不值得争论。相反，我们手动截图，这是 QA 的第一步。

### 记录参考资料和惯例

当处理像这样的大规模检修时，你会希望这个过程尽可能容易复制。在工具上写文档，这样任何人都可以使用它。保存出现的 UI 模式，以供以后参考和保持一致性。记录下你的设置，当项目完成时，它可以作为一个拆卸指南。

正确的工具将对项目的完成产生巨大的影响。我们估计，如果不是因为`bs4_upgrade`，我们的项目会多花 3 到 4 倍的时间。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

## 4.)团队工作惯例

工具只是拼图的一部分。你的团队工作惯例几乎同样重要。虽然工具很容易在代码中实施，但是工作约定需要团队一致同意。

让团队在同一个版本中一起工作。每周一次，设定团队焦点，制定本周目标。在周一设定目标意味着我们在大约 30 分钟内计划好我们的工作，然后开始行动。短暂的时间框架和关注意味着我们不需要每天站立，经理签到，或其他分心。

沟通谁拿走了什么，这样你就不会被冒犯，并确保它准确地反映在你的项目管理工具中。我们使用 [Basecamp](https://basecamp.com/welcome-back) ，给自己分配积极工作的任务，并在我们期望完成的周末添加截止日期。

定义一个“寻求帮助”的协议，这样当他们陷入困境时没有人会感到尴尬。我们决定在和某人配对之前，在一件特定的工作上花费不超过 20 分钟。这也有助于创建模式的一致性。

## 5.)通过它工作

现在你已经准备好克服它了。从一个独立的区域开始，熟悉这个框架。您将复制适用于每个领域的方法，并随着团队的继续前进而进行调整。

在我们的计划会议中，我们分解了谁负责什么，并在周末设定了一个 QA 会议的日期。我们将迁移视图作为一个优先事项，并有意选择推迟修复一些 bug，以防止上下文切换。

整个团队在每个版本上蜂拥而上，并在继续前进之前完全完成它。这对团队的士气很有帮助，并且让进步变得非常明显。

我们保持公关高度集中。通常，我们在单个 PR 中迁移模型的整个文件夹——索引、显示、新建、编辑。它减少了创建新分支的重复开销，同时保持了一个区域的隔离。公关部门根据直觉而非科学分裂——如果感觉太大，它会有自己的公关部门。

因为版本 2 是你的新代码库，抓住机会按照你认为合适的方式组织它。重命名、移动或删除文件夹和分部。重写每一个视图是一个大扫除的好时机，只要你记录下发生了什么变化，这样你就知道在最后一次扫描中没有遗漏任何东西。

预计框架约定会出现和变化，所以要考虑回过头来巩固它们。可能有几种方法来构建子导航，但是你的团队最喜欢的方法会在项目过程中出现。整合进行得很快，应该是在考虑发布之前要做的最后一件事。我们是在修复了 QA bash 的 bug 之后做的，所以它经常成为发布的最后一个 PR。

最后，一旦您完成了整个迁移，删除您的旧视图和工具。这应该与您的设置相反。

## 结论

在我的职业生涯中，我已经升级过几次 UI 框架，并且发现这些过程对于完成工作是最有效的。逐步升级是可行的，但你永远不会真正获得那种*新的、新鲜的*感觉，因为进展中的工作是如此长久。相比之下，版本 2 的方法意味着项目可以完全结束。没有比当一个项目在*完成*时感觉更好的了。

## 使用 [LogRocket](https://lp.logrocket.com/blg/signup) 消除传统错误报告的干扰

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/signup)

[LogRocket](https://lp.logrocket.com/blg/signup) 是一个数字体验分析解决方案，它可以保护您免受数百个假阳性错误警报的影响，只针对几个真正重要的项目。LogRocket 会告诉您应用程序中实际影响用户的最具影响力的 bug 和 UX 问题。

然后，使用具有深层技术遥测的会话重放来确切地查看用户看到了什么以及是什么导致了问题，就像你在他们身后看一样。

LogRocket 自动聚合客户端错误、JS 异常、前端性能指标和用户交互。然后 LogRocket 使用机器学习来告诉你哪些问题正在影响大多数用户，并提供你需要修复它的上下文。

关注重要的 bug—[今天就试试 LogRocket】。](https://lp.logrocket.com/blg/signup-issue-free)