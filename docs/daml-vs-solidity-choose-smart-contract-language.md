# Daml vs. Solidity:如何选择智能合约语言

> 原文：<https://blog.logrocket.com/daml-vs-solidity-choose-smart-contract-language/>

自从引入 Web3 和区块链以来，智能合同已经成为安全执行交易或绑定协议的流行解决方案。

区块链的智能合约是使用 Daml、Solidity 和其他特殊编程语言创建的。选择正确的编程语言来构建您的智能合约可能是一项挑战。

例如，Solidity 是最流行的智能编程语言之一，客观上比 Daml 等语言拥有更多优势。然而，Daml 提供了许多优势，可能使它成为某些企业主观上更好的选择。

让我们看看 Daml 和 Solidity 的一些相似性、区别和用例，以帮助您决定哪一个可能更适合您的特定需求。

以下是我们将在本文中涉及的内容:

## 什么是 Daml？

数字资产建模语言(Daml)是来自数字资产的[多方应用平台。](https://www.digitalasset.com/developers)[最近在 2022 年 3 月发布的 Daml 2.0](https://www.businesswire.com/news/home/20220302005237/en/Digital-Asset-Announces-General-Availability-of-Daml-2.0)据说具有隐私和互操作性的“突破性”能力。

这种开源智能契约语言用于创建基于抽象 Daml 分类帐模型的可组合应用程序。它还有助于协议建模，并在几个区块链系统上运行。

您可以将 Daml 应用程序部署到集中式数据库和区块链网络中。这些应用可以与任何基础设施顺利集成，无论是为了内部流程优化还是组建联盟。

由于其简单的语法，Daml 帮助开发人员专注于业务逻辑，并更快地进入市场。如果您想以最小的努力快速开发安全的智能契约，Daml 是一个不错的选择。

Daml 中的智能合同存在于 Daml 分类帐中。Daml 智能合约可能是单边的，也可能是多边的。这两种情况下的智能契约架构都可以存储事实、权利和义务，同时在需要知道的基础上施加可见性限制。

您可以用 VS 代码和 Daml SDK 或 webIDE 创建和运行 Daml 项目。

下面是一个 Daml 代码示例:

```
choice NameOfChoice :
      () -- replace () with the actual return type
    with
    party : Party -- parameters here
  controller party
    do
      return () -- replace this line with the choice body

```

## 什么是扎实？

Solidity 是一种用于编写智能合同的高级面向对象语言。它是静态类型的，在以太坊虚拟机(EVM)中使用。在以太坊网络上，Solidity 也可以用来构造去中心化应用(DApps)。

以太坊的 Solidity 中的所有代码都是使用 Remix IDE 编写、编译、部署和测试的。还可以[使用 Remix IDE 对以太坊交易](https://blog.logrocket.com/debugging-ethereum-transactions-remix-ide/)进行调试。

Solidity 代码很容易识别，因为它总是以`pragma`开头，后跟一个指定编译器版本的数字。例如，`pragma solidity ^0.8.10`表示使用编译器版本 0.8.10 的 Solidity 文件。

下面是一个 Solidity 代码示例:

```
// SPDX-License-Identifier: MIT
// compiler version must be greater than or equal to 0.8.10 and less than 0.9.0
pragma solidity ^0.8.10;

contract HelloWorld {
    string public greet  = "Hello World!";
}

```

## Daml 和 Solidity 的异同

Daml 和 Solidity 都是静态类型的函数式编程语言，用于创建智能契约。Daml 和 Solidity 之间的一些差异包括它们的工作流、契约概念和实现的灵活性。

当涉及到合同时，Daml 使用未用完的事务输出(UTXO)模型。这意味着在 Daml 工作流中，契约在被触发后完成。然后，一个新的合同与新的财产和控股形成。

另一方面，Solidity 使用基于帐户的范例。这意味着在 Solidity 工作流中，对合同的操作会影响基础账户的属性和持有量，同时保持合同有效。

最后，Daml 可以在越来越多的平台上实现，包括 Amazon Aurora、VMware Concord、Hyperledger Fabric 等等。

相比之下，你通常会使用 [Hardhat 来开发和部署 Solidity smart contracts](https://blog.logrocket.com/develop-solidity-smart-contracts-hardhat/) ，而[的另一个流行工具是 Truffle](https://blog.logrocket.com/truffle-suite-tutorial-develop-ethereum-smart-contracts/) 。这两者都需要创建一个部署脚本。

### 用 Daml 编写的智能合同示例

下面的代码块演示了一个简单的 Daml NFT 契约，它展示了用户如何使用 NFT 获得其他好处，比如音乐会门票或商店优惠券。

```
template RockBandNFT
  with
    uniqueNFTId: Text
    imageUrl: Text
    band    : Party
    fan     : Party
    -- benefits  : Benefits
    issuedDate: Date
  where
    signatory band, fan

```

### 用 Solidity 编写的智能合同示例

下面的代码块演示了一个简单的 Solidity 存储契约，用户可以存储一个值，并通过调用契约中的函数来检索它。

```
// SPDX-License-Identifier: GPL-3.0

pragma solidity >=0.7.0 <0.9.0;

/**
 * @title Storage
 * @dev Store & retrieve value in a variable
 * @custom:dev-run-script ./scripts/deploy_with_ethers.ts
 */
contract Storage {

    uint256 number;

    /**
     * @dev Store value in variable
     * @param num value to store
     */
    function store(uint256 num) public {
        number = num;
    }

    /**
     * @dev Return value
     * @return value of 'number'
     */
    function retrieve() public view returns (uint256){
        return number;
    }
}

```

如您所见，两种智能合约语言的代码格式有很大不同。

只有有编码经验的人一看就能理解 Solidity 示例代码。然而，在 Daml 中，诸如“where”、“with”和“do”这样的关键字有助于向没有编码或技术知识的人解释代码。

## 比较 Daml 和 Solidity 时要考虑的因素

在比较 Daml 和 Solidity 时，您应该考虑许多不同的因素。了解这些因素对于为您的项目选择最佳方案至关重要。通读下面的一些因素。

### 流行

人气方面，踏实取胜。自从固体在 EVM 运行以来，它就和以太坊一起越来越受欢迎。它被用在大多数流行的项目中，并被认为是智能合约的顶级编程语言之一。

此外，Solidity 于 2015 年 8 月发布，而 Daml 的首次发布是在 2019 年 4 月 4 日。在这额外的几年里， [Solidity 建立了大量的文档和广泛的社区支持，使得新手很容易获得帮助。](https://docs.soliditylang.org/en/latest/)

### 表演

虽然两种语言都可以用于智能合约，但是决定使用哪一种还是取决于您正在进行的具体项目。

Daml 允许您在处理逻辑的同时专注于项目。数据建模、细粒度权限、业务逻辑和基于场景的测试是典型 Daml 代码的四个组成部分。

Daml 还包括内置函数，您必须在 Solidity 中硬编码这些函数。虽然这只需要几行代码，但它仍然给了 Daml 更简单的优势。

然而，Solidity 包含了使开发人员的工作更容易的库——而不是必须自己实现这些库来实现你想要的功能，例如克隆智能合同。这些库包括社区验证的代码和安全的即用型合同。

### 测试自动化

[Daml 脚本允许您快速测试 Daml 模型](https://docs.daml.com/daml-script/index.html)并在 Daml Studio 中接收反馈。您可以使用 Daml Studio 在虚拟分类帐或实际分类帐中运行 Daml 脚本，用于应用程序脚本编写、自动化逻辑测试和分类帐启动。

您必须将 Daml 脚本库添加到您的`daml.yaml`文件中的 dependencies 字段，以访问用于实现 Daml 脚本的 API。

要运行一个脚本，首先必须用`daml build`构建它。然后，您可以通过指定 DAR、脚本的名称以及分类帐运行的主机和端口来运行它。实现 Daml 脚本所需的 API 是由 Daml 脚本库定义的。

在 Solidity 中，没有测试自动化特性。您只能手动编写和运行单元测试。

### 错误处理

这是任何开发项目的重要部分。任何程序，不管多小，都可能有大量的问题。

Daml 中的错误管理很容易。Daml 有一个“异常”特性，管理解释过程中出现的特定错误，而不是中止事务并回滚导致错误的状态更改。

用户定义的异常，`throw`和`try…catch`块是[Daml](https://docs.daml.com/daml/reference/exceptions.html)中的一些异常。值得注意的是，异常并不能处理所有的错误。

可靠性错误可能发生在编译时或运行时。编译时错误主要是语法错误，而运行时错误发生在执行过程中。

在 Solidity 中，`assert`函数、`require`函数、`revert`语句和`try…catch`语句等异常用于处理错误。

Daml 和 Solidity 处理错误的方式相似。因此，当涉及到错误管理时，您可以决定在项目中使用哪种语言。

### 容易理解

Solidity 代码一开始可能很难理解，可能需要开发人员的帮助。Daml 代码就不是这样了，即使对于那些以前从未和代码打过交道的人来说，它也很容易理解。

当向你的老板或潜在客户解释你的代码库时，你可能会发现他们会发现 Daml 代码库比 Solidity 代码库更容易理解。这可以帮助您在没有太多技术专长的情况下保持不同团队之间的清晰性。

虽然 Daml 很容易理解，但 Solidity 仍然被更广泛地使用和流行。在撰写本文时，Daml 的资源和社区支持比 Solidity 少。这在使用坚固性时非常有用，因为已经有了解决问题的方法。

### 灵活性

Daml 代码被认为是智能合约的区块链无关语言。它兼容各种区块链，包括 Hyperledger Fabric、Besu、Corda、VMware 和 Daml Hub。

使用 Solidity 的区块链或分布式分类帐的示例包括 Tendermint、币安智能链、以太坊经典、Tron、Avalanche、交易对手、Hedera 和 IOTA。Solidity 代码在以太网上完美运行。

## Daml 的用例

使用 Daml 的开发人员可以在很短的时间内设计出干净、安全的数据模型，并且不会出现 DIY 定制开发带来的错误。在下列情况下，您可能希望使用 Daml:

*   您希望节省时间并让开发人员专注于应用程序开发
*   您需要安装和集成的灵活性
*   您计划将 Daml 应用程序整合到现有系统中
*   您运行多方工作流，需要一个适用于许多区块链的智能合约

值得注意的是，每个运行多方工作流的公司必须在相同的区块链上运行，并使用与该区块链兼容的智能合同。

Daml 抽象出底层的区块链基础设施，允许开发人员专注于业务逻辑和应用程序开发，而不是交织技术代码。

下面的用例可能有助于展示 Daml 更适合您的业务的一些方式。

### 使用 Daml 解决不断变化的需求

Daml 既支持[集中式数据库，也支持](https://blog.logrocket.com/top-7-blockchain-based-databases/)分布式分类帐技术，这赋予了它使用任何现有基础设施的灵活性。使用 Daml 的公司可以购买底层分类帐或数据库的单一许可证，然后随着需求的发展迁移到任何区块链或数据库平台。

您还可以在任何类型的存储系统上安装 Daml 应用程序。该应用程序将与底层持久层无缝集成。

此外，Daml 支持内部、混合和公共云 IT 基础设施。这使得将新应用集成到现有业务流程和数据工作流中变得更加容易。

Daml 还与其他分类帐和基于 Daml 的应用程序通信，确保业务保持连接，不会形成新的孤岛。您可以使用流行的语言(如 JavaScript 和 Java)轻松地将 Daml 应用程序集成到现有系统中。

另一方面，可靠性合同一旦部署就不可改变。不能对协定进行任何更改，但它可以由需要协定功能的多个 UI 使用。

### 使用 Daml 的令牌化和发布

改变整个金融系统程序的关键是符号化。

任何资产类别都可以使用 Daml 进行标记化，包括 NFT。您可以通过将资产的权利和义务直接集成到令牌中来实现这一点，同时对工作流进行建模，以在整个金融生态系统中完全自动化资产的生命周期。

在 Solidity 中，这可以作为以太坊注释请求 721 (ERC721)令牌来完成。然而，ERC721 令牌不同于 Daml NFT；它不能在以太坊区块链上运行，因为它不是以太坊中的令牌标准。

### 基于 Daml 的金融工具生命周期管理

金融工具生命周期管理通常是手动的、不协调的，导致错误和延迟。

有了 Daml 的内置功能，现代企业可以利用自动化和跨单一数据源的实时事务。这有助于降低成本和实现运营现代化，同时维护交易方隐私。

Daml 很容易建立并精确执行基于生活事件的事务。通过这样做，它可以跨任何技术基础架构为任何类型的金融工具提供端到端的生命周期管理。

这也可以在 Solidity 中实现，但是你必须硬编码所有的东西。

### 使用 Daml 处理支付

公司通常依靠过时的系统来处理他们之间的支付。如果没有权威的数据和交易画面，转账很容易出现延迟和错误，而跨国企业则穿越复杂的支付系统网络。

通过其工作流，Daml 智能合同消除了手动流程，并促进了资产的无缝转移。

另一方面，稳健性对于银行和金融行业的智能合约可能更理想。在下面了解更多关于这个可靠性用例的信息。

## 可靠性的用例

如果你担心你的企业的安全性，可靠是一个很好的选择。

虽然智能合约是安全的，但它们可能会被黑客攻击，导致公司亏损。大多数企业对 Solidity 感到满意，因为它有支持系统、安全工具和安全测试环境。

因为 Solidity 是一种广泛使用的智能合同语言，所以很可能有专家提供了许多类型的安全问题的解决方案供其他人采用。

下面的用例可能有助于展示可靠性可能更适合您的业务的一些方式。

### 使用 Solidity 处理支付

如前所述，Solidity 智能合约在银行和金融行业非常有用。例如，您可以使用 Solidity 智能合约来表示和执行金融产品或服务限制。

此外，Solidity 智能合约可以使自动支付、交换、结算和索赔更加方便。他们还可以建立抵押贷款、债券、止赎和基于区块链技术的贷款。

### 稳固的分散自治组织

分散自治组织为众多类型的组织提供了分散治理的机会。这些团体的领导人可能由匿名人士组成。这可能也适用于私营公司和企业。

这可以在 Daml 中完成，尽管技术可能不同且不可靠。

### 建造不可信的道

实道是完全不可信的；一旦部署，就永远无法更改。相比之下，Daml 本身并不完全支持 DAO。Daml 合同有可以编辑合同的签署者，这使得他们不是完全不可信的。

如果你想设计一个完全不可信的刀，坚固是必由之路。虽然 Daml 可以实现无信任的 DAO，但目前它更难构建。

### 游戏开发

由于这些游戏可以通过 NFT 和代币实现盈利，越来越多的开发者正在使用 Solidity 进行游戏制作。Solidity 被用来创建大多数流行的区块链游戏，例如 Crypto Kitties。

在撰写本文时，Daml 还不知道它在游戏开发中的用途。

## 结论

Daml 和 Solidity 各有利弊。它们都是出色的选择，具有适合特定应用的特性。一个不能替代另一个，但每一个都是可行的选择。

我鼓励你探索和比较 Daml 和 Solidity，以便更好地了解你更喜欢哪一个。

## 加入像 Bitso 和 Coinsquare 这样的组织，他们使用 LogRocket 主动监控他们的 Web3 应用

影响用户在您的应用中激活和交易的能力的客户端问题会极大地影响您的底线。如果您对监控 UX 问题、自动显示 JavaScript 错误、跟踪缓慢的网络请求和组件加载时间感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/web3-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/dacb06c713aec161ffeaffae5bd048cd.png)](https://lp.logrocket.com/blg/web3-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/web3-signup)

LogRocket 就像是网络和移动应用的 DVR，记录你的网络应用或网站上发生的一切。您可以汇总和报告关键的前端性能指标，重放用户会话和应用程序状态，记录网络请求，并自动显示所有错误，而不是猜测问题发生的原因。

现代化您调试 web 和移动应用的方式— [开始免费监控](https://lp.logrocket.com/blg/web3-signup)。