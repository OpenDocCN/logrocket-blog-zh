# 用多重签名 Gnosis Safe 构建一个国库钱包

> 原文：<https://blog.logrocket.com/build-treasury-wallet-multisignature-gnosis-safe/>

想象你和你的朋友正在建设一个 NFT 市场。你是首席执行官，你的朋友是一名撰写智能合同的可靠性工程师。NFT 市场变得受欢迎，你的收入来自每笔 NFT 交易的市场费。你将你的利润储存在一份智能合同中，并向媒体吹嘘你的公司有足够的钱购买一座私人岛屿。然后，固体工程师消失，并从国库中提取所有的资金。你惊恐地看着。

现在，你发誓不再落入同样的陷阱。从现在开始，智能合约中的每一项敏感交易都需要一定数量的人批准。例如，从你的国库中提取资金需要某些关键人物至少 60%的同意。如果有五个关键人员，至少需要三个批准。

幸运的是，您不需要从头开始构建这个机制；您可以使用 [Gnosis Safe](https://gnosis-safe.io/) 与您的 NFT 市场互动。您将资金放在 Gnosis Safe 智能合约中，现在提取资金至少需要一定数量的签名。一个流氓代理人不能再偷钱了，你又要为一个私人岛屿存钱了！

Gnosis Safe 是来自 [Gnosis](https://gnosis.io) 的项目。Gnosis 最初是一个[预测市场平台](https://whitepaper.io/document/116/gnosis-whitepaper)，人们可以在那里自由交易信息。作为项目的一部分，Gnosis 背后的团队创建了 Gnosis Safe 来确保多个参与者的资金。如今，它是以太坊上最受欢迎的 multisig 钱包智能合约。在谷歌上搜索“multisig wallet Ethereum”，你会在顶部结果中找到 Gnosis。

在本文中，您将学习如何使用 Gnosis Safe 设置国库钱包，从而保护您在以太坊区块链上的资金。

## 设置 Gnosis 安全智能合约

您可以从 GitHub repo 中克隆 Gnosis Safe 智能合约，如下所示:

```
$ git clone https://github.com/gnosis/safe-contracts/

```

使用特定版本，以便您可以遵循本教程:

```
$ cd safe-contracts
$ git checkout v1.3.0-libs.0

```

有了这个特定的版本，Gnosis Safe 的部署地址将是确定的。

让我们将 Gnosis Safe 部署到 [Hardhat](https://hardhat.org/) 开发网络中。但是首先，您必须在`safe-contracts`目录中安装 Hardhat:

```
$ yarn add hardhat

```

然后，运行 Hardhat 开发网络并部署 Gnosis 安全智能合约:

```
$ npx hardhat node
Nothing to compile
sending eth to create2 contract deployer address (0x3fab184622dc19b6109349b94811493bf2a45362) (tx: 0x076c3e6eb9678931c92e0322885f48ebdc064226483a9bae4866f99c7f8aa8bb)...
deploying create2 deployer contract (at 0x4e59b44847b379578588920ca78fbf26c0b4956c) using deterministic deployment (https://github.com/Arachnid/deterministic-deployment-proxy) (tx: 0xeddf9e61fb9d8f5111840daef55e5fde0041f5702856532cdbb5a02998033d26)...
deploying "SimulateTxAccessor" (tx: 0xfc6d7c491688840e79ed7d8f0fc73494be305250f0d5f62d04c41bc4467e8603)...: deployed at 0x59AD6735bCd8152B84860Cb256dD9e96b85F69Da with 237871 gas
deploying "GnosisSafeProxyFactory" (tx: 0x6fff529768b3c5660234fcd53d5d04918aadc935a90ec05aca1796649bf4f699)...: deployed at 0xa6B71E26C5e0845f74c812102Ca7114b6a896AB2 with 867594 gas
deploying "DefaultCallbackHandler" (tx: 0x406498f13d684b2db11ac78d1b06c2b38657f02e729ecebc677b7ea28a30e712)...: deployed at 0x1AC114C2099aFAf5261731655Dc6c306bFcd4Dbd with 542473 gas
deploying "CompatibilityFallbackHandler" (tx: 0xe7426790ce3fed5ba2083b5e5b911b561a306d3f26fbd5c0d0d6c0c1d5847e3f)...: deployed at 0xf48f2B2d2a534e402487b3ee7C18c33Aec0Fe5e4 with 1238095 gas
deploying "CreateCall" (tx: 0xa602d00962fa8de99f84dbefd62f831f179d12e549863bd305607bbb775f5c81)...: deployed at 0x7cbB62EaA69F79e6873cD1ecB2392971036cFAa4 with 294718 gas
deploying "MultiSend" (tx: 0x8790b4413d0b4336586897f0bf40a72cdcfcb8fd06aed8a164fac5ecf662e0f6)...: deployed at 0xA238CBeb142c10Ef7Ad8442C6D1f9E89e07e7761 with 190004 gas
deploying "MultiSendCallOnly" (tx: 0xb4ccc0ce8099412d505d0ab131ce9fffb1915a5053906875fc301528ebe79f1a)...: deployed at 0x40A2aCCbd92BCA938b02010E17A5b8929b49130D with 142122 gas
deploying "SignMessageLib" (tx: 0xdf0d113415ea15354de8e816b793ca89e5a9a7d4ad7b48e1344872d0f4aacdbf)...: deployed at 0xA65387F16B013cf2Af4605Ad8aA5ec25a2cbA3a2 with 262353 gas
deploying "GnosisSafeL2" (tx: 0x83b42dd66a2e282b3e76cb10fb4ab93da970b0454010faef142ab8c6a5c4233d)...: deployed at 0x3E5c63644E683549055b9Be8653de26E0B4CD36E with 5200241 gas
deploying "GnosisSafe" (tx: 0xea94214f16af5e66646518db2403a6e24b17973d6bbb0208fc40f01343b0225f)...: deployed at 0xd9Db270c1B5E3Bd161E8c8503c55cEABeE709552 with 5017833 gas
Started HTTP and WebSocket JSON-RPC server at http://127.0.0.1:8545/

```

Gnosis 安全智能合约不仅仅是一个智能合约；他们很多。但是你需要特别注意三个智能合约:`MultiSend`、`GnosisSafe`和`GnosisSafeProxyFactory`。当您以后使用 Gnosis Safe SDK 时，您需要他们的地址。

那么这三个智能合约是什么呢？

`[GnosisSafe](https://github.com/gnosis/safe-contracts/blob/1.3.0-libs.0/contracts/GnosisSafe.sol)`是核心的安全智能合约，每个人只需要一个。您的创业公司和您的商业竞争对手可以使用相同的部署`GnosisSafe`，所以如果它已经被部署，您不需要单独部署它。

你不直接和`GnosisSafe`互动。你使用一个代理，叫做`[GnosisSafeProxy](https://github.com/gnosis/safe-contracts/blob/1.3.0-libs.0/contracts/proxies/GnosisSafeProxy.sol)`。因此，你的初创公司和你的商业竞争对手需要使用不同的`GnosisSafeProxy`，要创建你自己的`GnosisSafeProxy`，你可以使用`[GnosisSafeProxyFactory](https://github.com/gnosis/safe-contracts/blob/1.3.0-libs.0/contracts/proxies/GnosisSafeProxyFactory.sol)`。

`[MultiSend](https://github.com/gnosis/safe-contracts/blob/1.3.0-libs.0/contracts/libraries/MultiSend.sol)`是一个助手智能合约，将多个交易批处理为一个。你可能想买一艘游艇作为你的创业办公室，给一个迷因艺术家发工资，给你的创业公司所在的国家交税。您可以使用这个助手智能契约将这些事务批处理成一个，然后一次性执行，而不是逐个执行。

作为 Gnosis Safe 的一部分，还有其他智能合约，但是你不能直接接触它们。您只处理前面提到的三个智能合同。这就是为什么 Gnosis Safe SDK 要求您提供他们的地址。

如果你在以太坊 mainnet 或 Rinkeby 等其他网络中使用 Gnosis Safe，你不需要部署 Gnosis Safe 智能合约，因为 Gnosis Safe 背后的团队已经部署了这些核心合约。你只需要找到他们的地址并写下来。但是既然你用的是 Hardhat 开发网络，就需要做这一步。

让这个过程和平地进行。您可以打开一个新终端，并在新终端中创建与这些智能合约交互的项目。

## 安装 Gnosis 安全 SDK 库

Gnosis 安全 SDK 库是 Node.js 库。要使用它们，您需要创建一个节点项目。让我们通过创建一个空目录来创建一个，并用 yarn 初始化它:

```
$ mkdir our-treasury
$ cd our-treasury
$ yarn init -y
yarn init v1.22.11
warning The yes flag has been set. This will automatically answer yes to all questions, which may have security implications.
success Saved package.json
Done in 0.02s.
$ ls
package.json

```

在 Node 中与以太坊交互，有两种选择:`[web3.js](https://web3js.readthedocs.io/)`和`[ethers.js](https://docs.ethers.io/)`。在本教程中，您将使用`ethers.js`库。像这样用纱线安装库:

```
$ yarn add ethers

```

您将把以太坊地址放在`.env`文件中，而不是硬编码它们，所以您需要`dotenv`库:

```
$ yarn add dotenv

```

最后，您需要 Gnosis 安全 SDK 库:

```
$ yarn add @gnosis.pm/safe-core-sdk @gnosis.pm/safe-ethers-lib

```

这些是核心库，您将在本教程中学习如何使用它们来与 Gnosis 安全智能契约进行交互。

## 设置。环境文件

您可以将以太坊地址存储为环境变量，而不是对其进行硬编码。但是在执行脚本之前在终端中设置环境变量是一件麻烦的事情:

```
$ export ACCOUNT_1=0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266
$ export ACCOUNT_2=0x70997970c51812dc3a010c7d01b50e0d17dc79c8
...
$ node index.js

```

如果用`.env`文件更好。基本上，您使用`dotenv`库从`.env`文件中加载环境变量。这样，您只需要设置一次环境变量。

创建包含以下内容的`.env`文件:

```
ACCOUNT_1="0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266"
ACCOUNT_2="0x70997970c51812dc3a010c7d01b50e0d17dc79c8"
ACCOUNT_3="0x3c44cdddb6a900fa2b585dd299e03d12fa4293bc"
ACCOUNT_4="0x90f79bf6eb2c4f870365e785982e1f101e93b906"
ACCOUNT_5="0x15d34aaf54267db7d7c367839aaf71a00a2c6a65"
ACCOUNT_6="0x9965507d1a55bcc2695c58ba16fb37d819b0a4dc"
ACCOUNT_7="0x976ea74026e726554db657fa54763abd0c3a0aa9"
MULTI_SEND_ADDRESS="0xA238CBeb142c10Ef7Ad8442C6D1f9E89e07e7761"
SAFE_MASTER_COPY_ADDRESS="0xd9Db270c1B5E3Bd161E8c8503c55cEABeE709552"
SAFE_PROXY_FACTORY_ADDRESS="0xa6B71E26C5e0845f74c812102Ca7114b6a896AB2"

```

在上面的代码中，您可以看到`MULTI_SEND_ADDRESS`、`SAFE_MASTER_COPY_ADDRESS`、`SAFE_PROXY_FACTORY_ADDRESS`的地址与第一个终端中的地址相同。提醒一下，第一个终端是您在 Hardhat 开发网络上部署 Gnosis Safe smart contracts 的终端。稍后，在您的客户端代码中，您将从`.env`文件中加载这些智能合约的地址，以便与 Hardhat 开发网络上部署的智能合约进行交互。

`ACCOUNT_1`和其他账户是由 Hardhat 开发节点提供的样本以太坊地址。如果你在一个新的 Hardhat 项目中运行`npx hardhat node`，你将获得 20 个样本以太坊地址，以及它们的私钥，用于开发。每个账户有 10，000ETH。在这个`.env`文件中，你只取了前七个地址。

## 创建财政部

让我们创建`index.js`文件。您将在这里编写代码来构建带有 Gnosis Safe 的金库:

```
$ edit index.js # replace edit with vim or code or your favorite editor

```

首先，您希望能够读取放在`.env`文件中的变量。所以加上这一行:

```
require('dotenv').config();

```

然后，导入 Gnosis Safe SDK 库:

```
const { SafeFactory, SafeAccountConfig, ContractNetworksConfig } = require('@gnosis.pm/safe-core-sdk');
const Safe = require('@gnosis.pm/safe-core-sdk')["default"];

```

为了让事情更清楚，想象一下你正在创建一个 web3 创业公司来颠覆传统银行。有五个人在创建这家初创公司，你担任首席执行官。团队中的其他人是首席技术官、可靠性工程师、迷因艺术家和顾问。

天使投资人给你的创业公司送钱。钱放在安全的智能合约里。您的团队需要五个签名中的三个来批准与此智能合同相关的任何交易。作为首席执行官，你决定为自己的创业办公室购买一艘游艇。您将需要您团队的另外两个签名来批准此交易。我们开始吧！

## 在 Gnosis 安全钱包中设置多重签名授权

为了保护公司的金库不被一个人掏空，你必须确保五个团队成员中有三个同意购买游艇。现在让我们添加这个功能。

仍然在同一个文件`index.js`中，在`const Safe = require…`行下面添加这些行:

```
const ceo = process.env.ACCOUNT_1;
const cto = process.env.ACCOUNT_2;
const meme_artist = process.env.ACCOUNT_3;
const solidity_engineer = process.env.ACCOUNT_4;
const advisor = process.env.ACCOUNT_5;
const investor = process.env.ACCOUNT_6;
const yacht_shop = process.env.ACCOUNT_7;

```

在这里，您将存储在`.env`文件中的地址加载到`index.js`中。这样，要更改地址，您不需要更改代码。

然后，从`ethers.js`库中设置一个提供者:

```
const { ethers } = require('ethers');
const provider = new ethers.providers.JsonRpcProvider();

```

一个[提供者](https://docs.ethers.io/v5/api/providers/provider/)是一个以太坊连接对象。这里，您创建了一个到以太坊节点的 JSON-RPC 连接的抽象。

在这个提供者中，您从三个地址创建了三个签名者。请记住，您只需要三个签名就可以批准保险箱中的交易:

```
const ceo_signer = provider.getSigner(ceo);
const cto_signer = provider.getSigner(cto);
const advisor_signer = provider.getSigner(advisor);

```

Gnosis Safe 智能合约与`ethers.js`库和`web3.js`库一起工作。在本教程中，您将使用`ethers.js`。因此，您需要与`ethers.js`配合使用的适配器:

```
const EthersAdapter = require('@gnosis.pm/safe-ethers-lib')["default"];
const ethAdapter_ceo = new EthersAdapter({ ethers, signer: ceo_signer });
const ethAdapter_cto = new EthersAdapter({ ethers, signer: cto_signer });
const ethAdapter_advisor = new EthersAdapter({ ethers, signer: advisor_signer });

```

在上面的代码中，您使用这些适配器与 Gnosis Safe SDK 进行了交互。请注意，每个地址都需要自己的适配器。

接下来，创建`main`，异步函数。您将使用[这个模式来处理](https://hardhat.org/getting-started/#deploying-your-contracts) `async/await`并正确处理代码中的错误:

```
async function main() {
  // FROM NOW ON, YOUR CODE IS PUT HERE
}

main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });

```

正如本教程开始时所解释的，创建一个安全的唯一方法是从与所有人共享的安全工厂创建。因此，首先，您需要创建一个连接到安全工厂智能契约的安全工厂对象，`GnosisSafeProxyFactory`:

```
  const id = await ethAdapter_ceo.getChainId();
  const contractNetworks = {
    [id]: {
      multiSendAddress: process.env.MULTI_SEND_ADDRESS,
      safeMasterCopyAddress: process.env.SAFE_MASTER_COPY_ADDRESS,
      safeProxyFactoryAddress: process.env.SAFE_PROXY_FACTORY_ADDRESS
    }
  }

  const safeFactory = await SafeFactory.create({ ethAdapter: ethAdapter_ceo, contractNetworks: contractNetworks });

```

在上面的代码中，您首先收到了链 ID。然后，您创建了一个包含三个智能契约的对象，您可以安全地与它们进行交互。最后，你创造了一个安全的工厂。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

接下来，从这个安全工厂创建一个安全，如下所示:

```
  const owners = [ceo, cto, meme_artist, solidity_engineer, advisor];
  const threshold = 3;
  const safeAccountConfig = { owners: owners, threshold: threshold};

  const safeSdk_ceo = await safeFactory.deploySafe({safeAccountConfig});

```

外管局需要会员的地址和批准该外管局交易所需的最低签名数量。在上面的代码中，你把 startup 的所有成员和`3`作为阈值。要部署一个保险箱，您可以使用保险箱工厂的`deploySafe`方法。

现在你已经有了一个保险箱，一个投资者给你的创业公司汇钱，看起来像这样:

```
  const treasury = safeSdk_ceo.getAddress();

  const ten_ethers = ethers.utils.parseUnits("10", 'ether').toHexString();

  const params = [{
        from: investor,
        to: treasury,
        value: ten_ethers
  }];

  await provider.send("eth_sendTransaction", params);
  console.log("Fundraising.");

```

保险箱保存着你的国库。投资人使用`eth_sendTransaction` RPC 方法发送了第 10 个。

为了确保它正常工作，您可以使用以下方法检查国库余额:

```
  const balance = await safeSdk_ceo.getBalance();
  console.log(`Initial balance of the treasury: ${ethers.utils.formatUnits(balance, "ether")} ETH`);

```

## 获得多重签名批准

一旦投资者的资金到位，你就可以快速行动。创建一个购买游艇的交易，如下所示:

```
  const three_ethers = ethers.utils.parseUnits("3", 'ether').toHexString();

  const transaction = {
    to: yacht_shop,
    data: '0x',
    value: three_ethers
  };

  const safeTransaction = await safeSdk_ceo.createTransaction(transaction);
  const hash = await safeSdk_ceo.getTransactionHash(safeTransaction);

```

交易将 3ETH 发送到游艇商店。由于交易正在传输 ETH，您可以在交易的数据字段中填写空数据`0x`。但是，如果您创建智能合约交易，如铸造 NFT 或出售令牌，您将需要填写数据字段。

之后，使用`createTransaction`方法创建安全事务。然后，用`getTransactionHash`方法获得散列。

最后，作为首席执行官，你的工作是批准交易:

```
  const txResponse = await safeSdk_ceo.approveTransactionHash(hash);
  await txResponse.transactionResponse?.wait();

```

但是你的工作还没有完成。你打电话给你的联合创始人，你的创业公司的首席技术官，说服她批准购买游艇的交易。“如果能在浩瀚的海洋中编码不是很好吗？”

您的首席技术官同意批准交易:

```
  const safeSdk_cto = await Safe.create({ ethAdapter: ethAdapter_cto,
                                          safeAddress: treasury,
                                          contractNetworks: contractNetworks });

  const safeTransactionCTO = await safeSdk_cto.createTransaction(transaction);
  const hashCTO = await safeSdk_ceo.getTransactionHash(safeTransaction);
  const txResponse_cto = await safeSdk_cto.approveTransactionHash(hashCTO);
  await txResponse_cto.transactionResponse?.wait();

```

CTO 需要一个不同的安全对象。但是你不需要用安全工厂来创建它；你可以用安全对象的`create`方法创建它。因为您的 safe smart 契约已经在区块链上运行，所以您在创建 safe 对象时只需传递财政部地址。

接下来，您通过聊天或电子邮件将交易传递给 CTO。在这个上下文中，事务的意思是代码中的`transaction`对象。请记住，您已经创建了这个对象:

```
  const transaction = {
    to: yacht_shop,
    data: '0x',
    value: three_ethers
  };

```

您的 CTO 从这个事务中创建了一个安全的事务，并获得了它的散列。然后，她使用接受散列参数的`approveTransactionHash`方法批准交易。

你的工作还没有完成；你需要另一个签名。但这一次，你不需要说服你的顾问，因为他会全力支持你买一艘游艇。他批准交易:

```
  const safeSdk_advisor = await Safe.create({ ethAdapter: ethAdapter_advisor,
                                              safeAddress: treasury,
                                              contractNetworks: contractNetworks });

  const safeTransactionAdvisor = await safeSdk_advisor.createTransaction(transaction);
  const txResponse_advisor = await safeSdk_advisor.executeTransaction(safeTransactionAdvisor);
  await txResponse_advisor.transactionResponse?.wait();
  console.log("Buying a yacht.");

```

代码与 CTO 的批准交易相同，但是顾问使用了`executeTransaction`方法，而不是`approveTransactionHash`方法。该方法也在幕后批准交易。但最重要的是，这个方法执行的是安全交易，也就是买游艇！

最后，让我们检查一下你的国库余额:

```
  const afterBalance = await safeSdk_ceo.getBalance();
  console.log(`The final balance of the treasury: ${ethers.utils.formatUnits(afterBalance, "ether")} ETH`);

```

剧本写完了。您可以像这样执行脚本:

```
$ node index.js
Fundraising.
Initial balance of the treasury: 10.0 ETH
Buying a yacht.
The final balance of the treasury: 7.0 ETH

```

现在，您可以在游艇上与您的团队一起建造 DAO 来扰乱银行！

## 结论

在本文中，您了解了如何创建一个 Gnosis Safe，它可以被配置为需要多个签名来批准事务。您在 Hardhat 开发网络中启动了 Gnosis Safe smart contracts，然后使用 Gnosis safe SDK 创建了一个保险箱来存放国库。使用多个地址，您创建并批准了发送 ETH 的交易。

本文只解释了与 Gnosis 安全智能契约交互的 SDK。如果你想了解智能合约本身的来龙去脉，可以查看[他们的 GitHub 库](https://github.com/gnosis/safe-contracts/)！SDK 也有其他方法，如离线签署交易。查看[的 GitHub 库](https://github.com/gnosis/safe-core-sdk/tree/main/packages/safe-core-sdk)以了解更多信息。这篇文章的代码可以在[的 GitHub 库](https://github.com/arjunaskykok/gnosis-safe-web3-startup)上找到。

## 加入像 Bitso 和 Coinsquare 这样的组织，他们使用 LogRocket 主动监控他们的 Web3 应用

影响用户在您的应用中激活和交易的能力的客户端问题会极大地影响您的底线。如果您对监控 UX 问题、自动显示 JavaScript 错误、跟踪缓慢的网络请求和组件加载时间感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/web3-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/dacb06c713aec161ffeaffae5bd048cd.png)](https://lp.logrocket.com/blg/web3-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/web3-signup)

LogRocket 就像是网络和移动应用的 DVR，记录你的网络应用或网站上发生的一切。您可以汇总和报告关键的前端性能指标，重放用户会话和应用程序状态，记录网络请求，并自动显示所有错误，而不是猜测问题发生的原因。

现代化您调试 web 和移动应用的方式— [开始免费监控](https://lp.logrocket.com/blg/web3-signup)。