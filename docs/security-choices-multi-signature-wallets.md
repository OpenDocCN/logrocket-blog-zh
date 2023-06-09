# 安全选择和多重签名钱包

> 原文：<https://blog.logrocket.com/security-choices-multi-signature-wallets/>

多签名钱包是一种智能合约，需要多人同意才能执行一项操作。它们可用于保护资产(利用职责分离)或确保某些行动仅按照 multisig 所有者或大多数所有者的意愿进行。

本文重点介绍如何在设置 multisig 时做出最佳设计选择，以及如何避免常见错误。我们将演示几种不同的 multisig 配置。设置好多签名钱包后，您可以[将其添加到您的应用程序](https://blog.logrocket.com/build-treasury-wallet-multisignature-gnosis-safe/)中。

*向前跳转:*

## 为什么我们需要多西格？

在很多情况下，我们希望行动得到多人的批准。这里有几个例子:

### 分割所有权

如果一项资产由多人共同拥有，尤其是链上资产，智能合约可以验证它仅按照所有者的意愿使用。区块链还提供了审计线索，显示哪些所有者批准了任何行动，因此所有者不可能在以后假装他们没有批准。

### 职责分离

即使一项资产归一个实体所有，多 SIG 也有助于实现[职责分离](https://en.wikipedia.org/wiki/Separation_of_duties#Application_in_general_business_and_in_accounting)。当需要多人签署一项行动时，欺诈和无辜的错误都不太可能发生。在这些情况下，需要在安全性(更多的签名者意味着您更安全)和速度(更多的签名者意味着做任何事情需要更长的时间)之间进行权衡。

### 审查跟踪

有些情况下，允许多人执行一个动作，我们只想知道是谁执行了这个动作。通过使用只需要一个签名的多重签名，我们可以涵盖这个用例，而没有与共享帐户相关的安全风险。

## multisig 是如何工作的？

区块链上的实体，如多边合同，只能直接影响其他区块链实体。因此，multisig 可以控制的操作是那些可以通过调用智能合约来完成的操作，例如转移 ERC-20 令牌或 NFT。

多签名有多个签名地址，这些地址被授权单独执行某项操作，或者在特定规模的团体批准时执行。每个签名地址都是不同的以太坊地址，通常来自不同的恢复短语，由不同的人拥有。在本文的后面，我们将讨论您可能希望让一个人控制多个签名者地址的情况。

## 多重信号的类型

大多数 multisigs 实现 N 中的 M 要求。这意味着总共有 N 个签名者，其中 M 个必须在动作发生之前批准并签名。这被称为 M/N multi SIG；M 与 N 的比值称为法定商。例如，3/5 多签名将有五个签名人，其中三个需要同意或批准一项行动。

设置 multisig 参数的权衡可归结为安全性与易用性和可用性之间的权衡。

*   签名者越多(高 N)和需要的签名者越少(低 M)，就越容易找到必要的人来执行一项行动
*   如果你有更少的签名者(低 N ),一个错误或一个直接的黑客被批准的机会应该会减少
*   要求更多的签名者(高 M)可以转化为更多的监督和改进的安全性，但是如果 M 太高，你会得到责任的分散；关键人物可能认为其他人正在处理是否应该批准

## 演示:创建 multisig 钱包

要了解更多有关 multisig 法定商的信息并比较不同的案例，让我们为一家有四名经理的公司创建一个钱包。在我们的示例中，需要访问 multisig 来更改问候语。我们来看三种配置:无多信号、1/3 多信号和 2/4 多信号。

当然，这个例子的目的只是为了演示 multisig，而不是它所控制的契约。在现实世界的应用程序中，契约通常比改变问候语执行更有价值的功能，并且它们通常限制可以做出改变的个人数量。

### 无多重信号

在实际使用 multisig 之前，我们应该设置我们的实验室环境和目标合同(multisig 控制的合同)。实验室环境运行在 Goerli 测试网络之上。如果你需要格力测试 ETH，你可以在这个水龙头处[得到。](https://faucet.paradigm.xyz/)

对于我们的演示，我们将使用一个名为 `Greeter.sol`的[简单智能契约，它是我与](https://github.com/NomicFoundation/hardhat/blob/master/packages/hardhat-core/sample-projects/basic/contracts/Greeter.sol)[安全帽](https://hardhat.org/)一起部署的。这里可以看到[。](https://goerli.etherscan.io/address/0x8A470A36a1BDE8B18949599a061892f6B2c4fFAb)

要查看当前问候语，打开**合同>阅读合同**然后展开**问候语**。

要修改当前问候语，打开**合同>写合同**。然后，点击**连接网络 3** 连接钱包。从列出的选项中选择钱包后，点击**设置问候语**并输入新的问候语。然后，点击**写**并批准钱包中的合同。

请注意，由于缓存的原因，在您更改问候语之后，您可能需要重新加载合同几次，才能看到新的问候语。

### 1/4 多签名(需要一个签名)

演示 multisig 是用 [Gnosis Safe](https://gnosis-safe.io/) 创建的，这可能是最常见的 multisig 平台。

被授权使用 multisig 的地址都是从密码短语“哑车拉力赛入口铁羊群人灭亡记录月亮侵蚀绿色”中导出的

地址如下:

*   0x 3646468082813 b 33 BF 7 aab1 b 8333 aa 01 fee 8 a 386
*   0x8c 262 b 009 b 05 e 94d 3 fff 1 ce 4 CEA 8 da 0 ba 450 c 793
*   0x 126 Fe 1 acdb 5a 5101 b 80 DC 68 a 0 b 0 DC 882 bfeee 5a 6
*   0x 0c 48 DFB 3 faafbcecf 21 f 0 D1 F4 e 75 E1 Fe 6 e 731 ad 6
*   0x 934003 BC 77 b 9d 427 C4 a 441 ebef 2086 aa 089 ed 0 c 5
*   0x 9 D5 f 666 b 29d 0 DD 2397 fdbc 093 fdacaa 0e 6 e 7377

在现实世界中，当地址属于不同的人时，它们来自唯一的密码短语。但是，在这里这样做需要您(作为读者)不断地注销一个密码短语并进入另一个密码短语，或者使用多个设备。在本次培训中，我认为便利性比安全性更重要，因此我们将省略本演示中的独特密码。

现在，让我们看一个例子，其中只有所有者可以更改问候语。在本例中，只需要一个签名就可以进行更改。

我们将使用相同的`Greeter.sol`合同。在现实世界的应用程序中，我们可能会实现`[Ownable](https://docs.openzeppelin.com/contracts/2.x/api/ownership#Ownable)`并将所有者设置为 multisig，但这里的目的是使事情尽可能简单，而不是尽可能安全。

当需要单个签名者时，您需要提议，然后确认交易。

1.  [浏览此处](https://gnosis-safe.io/app/gor:0xbfE5a01f60396fd5F6a5350439058F35DDe67eA0/home)使用装有上述密码的钱包的浏览器，并使用上面列出的前四个地址之一进行连接
2.  点击**新建交易**和**合同交互**
3.  粘贴您试图与之互动的合同的地址:`0x8A470A36a1BDE8B18949599a061892f6B2c4fFAb`
4.  请注意，带有如何联系合同的定义的 ABI 是自动导入的；该合同的代码可以在 Etherscan 上获得，因此 Gnosis Safe 可以检索代码
5.  选择`setGreeting`方法并输入新的问候语
6.  点击**审核**和**提交**；接下来，批准钱包中的交易
7.  等待
8.  一旦交易被执行，[转到合同](https://goerli.etherscan.io/address/0x8A470A36a1BDE8B18949599a061892f6B2c4fFAb#readContract)并展开**问候语**以查看问候语已经改变

### 2/4 多签名(需要两个签名)

接下来，我们来看一个例子，其中四个所有者中的两个必须签名。在本演示中，我们需要假装是第二个经理，批准交易，以便获得交易发生所需的两个签名。

首先，按照前面例子中的步骤，但是[使用这个安全的](https://gnosis-safe.io/app/gor:0x8F760D2fD9999d407b3c4B67555bF037eD5eB832/home)。

1.  切换到钱包中的不同地址(其他三个批准者之一)
2.  [再次浏览此处](https://gnosis-safe.io/app/gor:0x8F760D2fD9999d407b3c4B67555bF037eD5eB832/home)；您可能需要断开连接，然后在应用程序中重新连接，才能显示正确的地址
3.  点击**交易队列**下的交易
4.  展开交易，点击**确认**批准交易，然后点击**提交**
5.  批准钱包中的交易

现在，查看事务，然后验证所请求的操作是否发生(问候语确实发生了变化):

1.  [浏览此处](https://goerli.etherscan.io/address/0x8A470A36a1BDE8B18949599a061892f6B2c4fFAb#readContract)并展开**问候语**以查看问候语是否真的发生了变化
2.  要查看交易，点击**内部交易**并找到 multi SIG(0x8f 760 D2 FD 9999d 407 B3 C4 b 67555 BF 037 ed 5 EB 832)和 greeter(0 x8a 470 a 36 a1 bde 8 b 18949599 a 061892 F6 B2 C4 ffab)之间的最新交易
3.  点击**父事务哈希**查看改变问候语的事务
4.  请注意，第二个签名者被列为来源

## 多西格潜在的问题

Multisig 钱包旨在提供额外的安全性，但问题仍然会出现。让我们看一些例子。

### 锁定的资产

区块链的最大优势是没有中央权威。在上面的示例中，除了这四个经理地址中的至少两个，没有人可以批准来自 multisig 的交易。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

区块链的最大缺点是没有一个中央机构可以在正当的时候推翻合同。例如，在 2/4 多重签名的三个签名人死亡的情况下，多重签名将无法释放其任何资产。钱包的资产将永远被锁定。

为这种情况提供备份的一个选择是让公司完全信任的人(例如，所有者)生成两个额外的地址，并将他们的密码保存在安全位置的防篡改信封中。异地位置，如公司律师或账户的保险箱，通常是一个不错的选择。

### 所有者覆盖

在多重签名中，所有签名者都是平等的。问题是，有时我们希望签名者比其他人更平等。例如，我们可能希望业务经理能够使用附加签名做一些事情，但是所有者能够做任何事情。

一种解决方案是允许所有者的地址直接访问目标合同，而不通过 multisig。这种解决方案具有最好的可用性，但这意味着我们不能完全依赖 multisig 进行审计。

第二种选择是所有者从密码短语中生成两个地址，并将这两个地址用作签名者。这种解决方案的可用性更有限，但如果 multisig 的部分目的是减少粗心错误的可能性，并且如果所有者覆盖被用作紧急措施，而不是日常处理的一部分，则可能是更好的选择。

## 演示:创建共享的 multisig 钱包

现在，让我们来看一个更复杂的场景，其中两家公司合作，钱包的功能需要得到每家公司至少一名经理的批准。

因为在多重签名中所有的签名者都是平等的，所以我们需要在合同中写入一些逻辑来实现这个目标。[点击此处查看担保合同](https://goerli.etherscan.io/address/0x3e55E2DBDE169Fbf91B17e337343D55a7E0D728e#code)。

让我们看看当公司 A 提出一个新的问候时会发生什么。

1.  [转到合同](https://goerli.etherscan.io/address/0x3e55E2DBDE169Fbf91B17e337343D55a7E0D728e#readContract)并检查当前问候语
2.  将钱包切换到 A 组地址之一:
    *   0x 3646468082813 b 33 BF 7 aab1 b 8333 aa 01 fee 8 a 386
    *   0x8c 262 b 009 b 05 e 94d 3 fff 1 ce 4 CEA 8 da 0 ba 450 c 793
    *   0x 126 Fe 1 acdb 5a 5101 b 80 DC 68 a 0 b 0 DC 882 bfeee 5a 6
3.  [浏览至 A 组 multisig](https://gnosis-safe.io/app/gor:0xb391FdE1f79F6CEaF08f8Ef34a7416a05F4646a4/home)
4.  点击**新增交易>合同交互**
5.  键入合同地址:`0x3e55E2DBDE169Fbf91B17e337343D55a7E0D728e`
6.  点击**提议问候**并提议问候
7.  点击**审核**然后**提交**
8.  确认钱包中的交易
9.  [再次转到合同](https://goerli.etherscan.io/address/0x3e55E2DBDE169Fbf91B17e337343D55a7E0D728e#readContract),看到问候语没有改变

接下来，让我们看看当 B 公司提出不同的问候方式时会发生什么。这一步是必要的，因为当人们遵循正确的过程时，仅仅看到智能合约行为正确是不够的。同样重要的是，当人们不遵循适当的程序时，要确保合同保持安全。

1.  将钱包切换到 B 组地址之一:
    *   0x 0c 48 DFB 3 faafbcecf 21 f 0 D1 F4 e 75 E1 Fe 6 e 731 ad 6
    *   0x 934003 BC 77 b 9d 427 C4 a 441 ebef 2086 aa 089 ed 0 c 5
    *   0x 9 D5 f 666 b 29d 0 DD 2397 fdbc 093 fdacaa 0e 6 e 7377
2.  [浏览至 B 组 multisig](https://gnosis-safe.io/app/gor:0x5109e87aeB1034380F1CA53F3fc20263b8d50521/home)
3.  点击**新增交易>合同交互**
4.  键入合同地址:`0x3e55E2DBDE169Fbf91B17e337343D55a7E0D728e`
5.  点击**提议问候**并提议问候
6.  看到评论告诉你交易会失败(因为你不是正确组的成员)；点击**返回**
7.  为您的当前地址选择合适的选项， **proposeGreetingB** ，并提议一个问候语(确保选择与公司 A 提议的问候语不同的问候语)
8.  点击**审核**然后**提交**
9.  确认钱包中的交易
10.  [再次转到合同](https://goerli.etherscan.io/address/0x3e55E2DBDE169Fbf91B17e337343D55a7E0D728e#readContract),看到问候语仍然没有改变

现在，让我们来看看当 B 公司提出与 a 公司相同的问候方式时会发生什么。

1.  再次尝试**提议问候 gB** ，这次使用您作为 A 组成员提议的相同问候
2.  最后一次回顾合同，看看问候语是否最终改变了

让我们看看[固体代码](https://docs.soliditylang.org/)来看看这是如何工作的:

```
/**
 *Submitted for verification at Etherscan.io on 2022-05-08
*/

//SPDX-License-Identifier: Unlicense
pragma solidity ^0.8.0;

contract AB_Greeter {
  string greeting;

```

以下是 multisigs 的地址:

```
  address multisigA;
  address multisigB;

```

这些变量保存了提议问候的[哈希值](https://en.wikipedia.org/wiki/Cryptographic_hash_function)。

使用散列有两个优点。

*   以太坊存储是一种昂贵的资源，这样我们就可以少用一些
*   当我们存储散列时，我们只需要为每个建议写一个 32 字节的字

如果我们要存储字符串，它们可能会更长和更昂贵。此外，Solidity 没有内置的表达式来比较字符串，所以比较两个字符串最简单的方法是比较它们的哈希值。通过使用散列，我们每次调用`proposeGreeting[AB]`时只计算一次散列。

```
  bytes32 proposedGreetingA = 0;
  bytes32 proposedGreetingB = 0;  

```

首先，我们需要问候语以及两个 multisigs 的地址:

```
  constructor(string memory _greeting, 
              address _multisigA, 
              address _multisigB) {
    greeting = _greeting;
    multisigA = _multisigA;
    multisigB = _multisigB;
  }

```

函数`greet`和`setGreeting`与我们之前使用的`Greeter.sol`契约中的相同。

```
  function greet() public view returns (string memory) {
    return greeting;
  }

  function setGreeting(string memory _greeting) internal {
    greeting = _greeting;
  }

```

这是提议新问候语的功能。

```
  function proposeGreetingA(string calldata _greeting) public {

```

只有`multisigA`可以作为 A 公司提出问候；任何其他来源将被拒绝。

```
    require(msg.sender == multisigA, "Only for use by multisig A");
    bytes32 _hashedProposal = keccak256(abi.encode(_greeting));

```

如果 B 公司已经提出了 A 公司现在提出的建议，我们就这样更新问候语:

```
    if(_hashedProposal == proposedGreetingB)    
      setGreeting(_greeting);

```

否则，我们将其注册为公司 A 提议的问候语:

```
    else
      proposedGreetingA = _hashedProposal;
  }

```

重要的是要认识到，这不是实现这一目标的理想方式，因为`multisigA`是 1/3，所以公司 A 的任何经理都可以切换 multisig，并剥夺其他两个签署者提议或批准任何事情的能力。

一个更明智的政策是，为这种类型的敏感操作提供另一个 multisig，可能是 2/3。然而，这个例子的目的是为了教学，所以我们选择简单而不是安全。

在下面的代码中，我们指定`multisigA`可以在需要时切换到新的 multisig。

```
  function changeMultisigA(address _newMultiA) public {
    require(msg.sender == multisigA, "Only for use by multisig A");    
    multisigA = _newMultiA;
  }

```

B 公司的职能是 a 公司职能的镜像。

```
 function proposeGreetingB(string calldata _greeting) public {
    .
    .
    .
  }

  function changeMultisigB(address _newMultiB) public {
    .
    .
    .
  }
}

```

## 关于智能合同开发的警告

智能合约开发相对容易，但是[安全智能合约开发](https://blog.logrocket.com/smart-contract-development-common-mistakes-avoid/)却不容易。除非您有丰富的安全专业知识，否则强烈建议您在将逻辑和代码用于任务关键型应用程序之前，让有经验的人检查您的逻辑和代码。

例如，当我编写`AB_Greeter`契约时，我首先只为提议的问候使用了一个变量，我的代码如下所示:

```
  function proposeGreetingA(string calldata _greeting) public {
    require(msg.sender == multisigA, "Only for use by multisig A");
    bytes32 _hashedProposal = keccak256(abi.encode(_greeting));

    if(_hashedProposal == proposedGreeting) {
      setGreeting(_greeting);
    } else {
      proposedGreeting = _hashedProposal;
    }
  }

```

你能发现问题吗？

更改问候语确实需要两次批准。但是，A 公司可以用同样的问候语给`proposeGreetingA`打两次电话。第一个调用将新问候语的散列作为建议。第二次调用发现新问候语的哈希与建议相同，并更新了问候语。

如果提案来自 B 公司，这应该没问题，但这里的提案来自 A 公司，所以这违反了条款。

为了解决这个问题，我决定使用两个独立的提议，一个由公司 A 控制，另一个由公司 B 控制。

我并不是说当前合同中的逻辑是 100%安全的。如果我要在生产中使用它，我会让其他人先看看。智能合同的存在是为了实现无信任的合作。当你写它们的时候，你必须假设它们会在一个敌对的环境中使用。只有在潜在的不利环境下，运行智能合同而不是更传统的程序的费用才是合理的。

## 结论(什么情况下 multisigs 是正确的解决方案？)

Multisigs 是一个简单问题的简单解决方案——当所有组成员都是平等的，并且组成员关系很少改变时，如何从组中获得权限。

在本文中，我们回顾了一些扩展该功能的机制，或者通过以不寻常的方式使用 multisig(所有者有两个签署者)，或者通过在单独的智能契约中添加我们自己的逻辑(两家公司的场景)。

如果您的签名者群体是动态的，或者如果您有许多不同的角色，每个角色都有自己的权限，那么多重签名可能不是理想的解决方案。相反，一个分散的自治组织可能是一个更好的选择。

但是，如果您需要实现的业务需求是 multisig 就足够了，那么这是一个比创建 DAO 简单得多的解决方案。注意，在我们的第一个例子中，我们不需要写任何代码。您还可以使用 SDK 将 multisigs 集成到您自己的应用[中。](https://docs.gnosis-safe.io/build/sdks/safe-apps/get-started)

## 加入像 Bitso 和 Coinsquare 这样的组织，他们使用 LogRocket 主动监控他们的 Web3 应用

影响用户在您的应用中激活和交易的能力的客户端问题会极大地影响您的底线。如果您对监控 UX 问题、自动显示 JavaScript 错误、跟踪缓慢的网络请求和组件加载时间感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/web3-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/dacb06c713aec161ffeaffae5bd048cd.png)](https://lp.logrocket.com/blg/web3-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/web3-signup)

LogRocket 就像是网络和移动应用的 DVR，记录你的网络应用或网站上发生的一切。您可以汇总和报告关键的前端性能指标，重放用户会话和应用程序状态，记录网络请求，并自动显示所有错误，而不是猜测问题发生的原因。

现代化您调试 web 和移动应用的方式— [开始免费监控](https://lp.logrocket.com/blg/web3-signup)。