# 如何建立一个区块链慈善或众包平台——log rocket 博客

> 原文：<https://blog.logrocket.com/build-blockchain-charity-crowdsourcing-platform/>

许多人可能会同意，公众对非营利组织的信任最近有所下降。事实上，[美国人向慈善机构捐款的比例在过去二十年里稳步下降](https://www.philanthropy.com/article/the-trust-crisis/)，从 2000 年的 66%下降到 2016 年的 53%。这似乎是对政府、企业、非政府组织、媒体等失去信任的更广泛现象的影响。

这里有一个有趣的(在某种程度上也是可悲的)结论是，公众信任度的下降与对其他机构信任度的上升并不相关。这个问题的解决方案不能仅仅是技术性的，还需要一种新的方法来看待这些机构。

区块链当然是一种技术架构，但它仍然包含了处理用户之间信任关系的新方式。总的来说，区块链并没有为各种形式的中央集权提供一个平台——一切都是公开的。

数据，更重要的是，处理这些数据的算法，都收集在一个公共注册表中。任何人都可以自由查阅副本。有了这个，一个区块链的慈善组织可以在捐赠者中培养新的信任。

在本文中，我们将构建一个简单的、功能性的慈善组织，它将在 crypto 中收集资金，并在接收者之间分发。

## 项目概述

我们的方法将基于智能合同，该合同将托管我们想要使用的所有功能。我们将从流行的众包网站如 [GoFundMe](https://www.gofundme.com/) 和 [Kickstarter](https://www.kickstarter.com/) 中获取灵感。

对于我们的项目，任何用户都可以创建一个收集捐款的活动。一个活动将有一个标题，标志，简短的描述，和截止日期，捐款将在 ETH。捐赠类型是一个严格的要求，尽管基于加密的经济中一个有趣的部分与 NFTs 有关，但为了简单起见，我们将使用 ETH。

像往常一样，我们将提供一个现成的存储库。[你可以在我的 GitHub repo 这里找到代码](https://github.com/rosdec/charity)。

为了保持存储库(和本文)的简单，我们将更多地关注智能契约逻辑，而不是实现完整的 web 应用程序。如果您真的渴望完整的 web 应用程序体验，您可以轻松地从测试套件中提取代码，并在您选择的框架中使用它。

这个项目使用 [Hardhat](https://hardhat.org/) 来支持开发。您可以运行测试并使用这些方法，而不需要真正的以太坊节点。

## 在区块链创建、捐赠和撤回

我们系统的总体思路是允许任何人创建一个活动。一旦活动开始，人们可以在某个截止日期前捐款。

捐赠的 ETH 只有在活动停止时才能收回。这可以通过两种方式之一实现:明确请求终止活动或在设定的截止日期到来时。

值得注意的一个细节是，这两个方法都生成一个适当的事件。Solidity 中的事件是跟踪正在发生的事情的最佳解决方案，但也实现了区块链和 UI 之间的一些异步交互。

### 创建活动

活动的创建通过调用方法`startCampaign`来实现:

```
    function startCampaign(
        string calldata title,
        string calldata description,
        string calldata imgUrl,
        uint256 deadline
    )

```

该方法采用四个参数来构成活动的元数据:标题、描述、图像的 URL 和以 UTC 表示的截止日期。我们可以看到一个如何在测试套件中执行它的例子:

```
        const tx = await contract.startCampaign(
            "Test campaign",
            "This is the description",
            "http://localhost:3000/conference-64.png",
            Math.round(new Date().getTime() / 1000) + 3600);
        await tx.wait();

```

在示例中，您可以看到我们通过在调用方法的时间上增加 3600 秒(相当于一个小时)来计算活动的截止时间。

一旦活动被正确创建，即在交易被实际收集到一个块中之后，活动将被激活，并将由三元组`[owner account, title, description]`标识。如果我们查看`/contracts`目录中的合同代码，您会注意到以下方法:

```
    function generateCampaignId(
        address initiator,
        string calldata title,
        string calldata description
    ) public pure returns (bytes32)

```

该方法将上述三元组作为参数，并生成唯一的活动`id`。

通过查看参数，您可能会注意到来自不同发起者的两个活动可能具有完全相同的标题和描述，而同一发起者不能两次发起同一活动。当然，在开始竞选时，在处理更复杂的政策和条件方面还有改进的空间。

### 向一场运动捐款

一旦活动开始，它就可以接受捐赠。每个活动都有一个`balance`字段。一旦满足许多条件，就通过增加字段来跟踪每一笔捐赠。

方法`donateToCampaign`负责接收捐赠和更新计数器。

```
    function donateToCampaign(bytes32 campaignId) public payable

```

从方法签名可以看出，该方法是可支付的。这意味着发送给它的交易将带来可以转移到智能合约的资金。

`donate`方法将使用上述方法计算的`campaignId`作为参数。转移到活动的金额是交易中的`value`参数的内容。接下来是一个调用的例子，再次取自`/test`目录中的测试套件。

```
     await contract.connect(accounts[1]).donateToCampaign(
           campaignId, { value: ethers.utils.parseEther('0.5') });

```

如您所见，该方法的调用与通常不同。这是因为它包含了代表我们将要捐献的 ETH 数量的值。

我们在这里更新的一个重要数据结构是注册表`userCampaignDonation`,它将跟踪每个活动、支持者以及他们捐赠的金额。如果一个赞助者不止一次地向同一个活动捐款，捐款将被加到一个总数中。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

### 结束或撤回活动

正如我们之前提到的，活动要么在截止日期到来时结束，要么在发起者显式调用`endCampaign()`方法时结束:

```
     function endCampaign(bytes32 campaignId) public 

```

该方法在检查调用的合法性后做两件简单的事情。它将`.isLive`标志设置为假，并将`.deadline`字段设置为当前块时间戳。这确保了该运动不会接受更多的捐赠。

在方法中检查两种机制:

```
     function withdrawCampaignFunds(bytes32 campaignId) public

```

当一个活动不再有效时，`withdrawal`方法会将收集的资金转移到发起者的账户。

```
     uint256 amountToWithdraw = campaign.balance;

     campaign.balance = 0;
     payable(campaign.initiator).transfer(amountToWithdraw);

```

`payable()`函数只是告诉 Solidity 编译器这个地址可以接收 ETH 传输的语法糖。

这正是函数`transfer()`所做的。它将活动收集的特定数量的 ETH(`campaign.balance`字段)传送给发起者。

## 附加功能

上述功能通过创建、收集捐款、设置截止日期和提取资金来结束活动的生命周期。

我们将简要讨论一些额外的功能，以便更舒适地创建一个完整的系统。

下面的方法将分批返回五个项目中的`campaignId`。这有助于实现一个 UI，我们可以在其中显示可用活动的分页列表:

```
     function getCampaignsInBatch(uint256 _batchNumber)
          public view returns(bytes32[] memory)

```

最后一个方法是`getCampaign`，它返回活动的元数据，如标题、描述和余额，并将`campaignId`作为参数。

```
     function getCampaign(bytes32 campaignId) public view

```

那么，我们该何去何从呢？一旦合同启动并运行，您就可以开始考虑您打算向潜在用户群提供的用户体验，并由此开始为各种功能设计和实现最合适的前端！

您还可以实现一种机制，让智能合约的所有者在资金转移时收取少量费用。这种机制可以在`donateToCampaign`或`withdrawCampaignFunds`方法中实现。

此外，您还可以重点加强智能合同。这种智能合约处理柜台和基金，但即使是简单的数学也可能容易出现弱点。你可以考虑使用 [OpenZeppelin 计数器](https://docs.openzeppelin.com/contracts/3.x/api/utils#Counters)来处理`_campaignCount`。

## 结论

总而言之，这篇文章开始谈论信任和实施筹款系统的巨大成果。

使用区块链意味着没有处理捐赠和资金转移的秘密机制。其中的一切，包括捐款的分类账和用于操作它们的算法，都写在一份智能合同中，可以很容易地进行检查。

区块链的活动可能是解决公众对筹款组织信任度下降的一个办法。它们将是这些系统设计方式的一个巨大的、颠覆性的范式转变！

## 加入像 Bitso 和 Coinsquare 这样的组织，他们使用 LogRocket 主动监控他们的 Web3 应用

影响用户在您的应用中激活和交易的能力的客户端问题会极大地影响您的底线。如果您对监控 UX 问题、自动显示 JavaScript 错误、跟踪缓慢的网络请求和组件加载时间感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/web3-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/dacb06c713aec161ffeaffae5bd048cd.png)](https://lp.logrocket.com/blg/web3-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/web3-signup)

LogRocket 就像是网络和移动应用的 DVR，记录你的网络应用或网站上发生的一切。您可以汇总和报告关键的前端性能指标，重放用户会话和应用程序状态，记录网络请求，并自动显示所有错误，而不是猜测问题发生的原因。

现代化您调试 web 和移动应用的方式— [开始免费监控](https://lp.logrocket.com/blg/web3-signup)。

## 参考

1.  [用区块链管理慈善 4.0:新冠肺炎时代的案例研究| SpringerLink](https://link.springer.com/article/10.1007/s12208-021-00281-8)
2.  [慈善区块链:追踪捐赠的 Luxarity 案例研究| ConsenSys](https://consensys.net/blockchain-use-cases/social-impact/luxarity-case-study/)
3.  [在区块链上创建一个慈善/捐赠平台(第一部分)| DEV 社区](https://dev.to/emkay860/create-a-charitydonation-platform-on-the-blockchain-part-1-6nb)
4.  [2020 年爱德曼信托晴雨表|爱德曼](https://edubirdie.com/blog/trust-barometer)