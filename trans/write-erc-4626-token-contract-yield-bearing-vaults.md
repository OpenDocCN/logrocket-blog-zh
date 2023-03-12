# 如何写一个 ERC-4626 代币合同的收益率承载金库

> 原文：<https://blog.logrocket.com/write-erc-4626-token-contract-yield-bearing-vaults/>

2022 年 4 月，流行的 DEFI 协议“渴望金融”宣布支持新推出的以太坊征求意见稿(ERC)-4626 令牌标准，声明“渴望 V3 + ERC-4626 =不可避免”

> 5/向往 V3 + ERC-4626 =不可避免贡献者已经在努力实现向往 V3 保险库的标准，所以开发者在@AlchemixFi、@balancer、@RariCapital、@feiprotocol、@OpenZeppelin 和其他地方，也许有一天，我们甚至会在@etherscan 上看到 Erc4626 标签

对于基于以太坊的应用程序，ERC(以太坊请求注释)是包括名称注册、包格式、库和令牌需求的标准。为了使其他开发人员能够预测一组功能是如何实现的，即使无法访问符合这些标准的应用程序的源代码，这组标准描述了这些功能将如何在应用程序和智能合约中实现或交互。

鉴于以太坊社区的规模以及开发者技能水平和实现方法的多样性，ERC 对于以太坊生态系统至关重要。这些准则作为一个框架，保证应用程序和智能合约以一致的方式构建，以便其他人可以为它们做出贡献。

在 ERC-4626 之前，已经成功部署了大约 25 个 ERC 标准，包括 ERC-20(可替换令牌)、ERC-721(不可替换令牌，或 NFTs)和 ERC-1155(单一智能合约多令牌)。

没有比 ERC-4626 更简单的方法来实现产量保险库了。通过提供一个单一的 API，允许在一个共享的接口上从多个链铸造令牌，ERC-4626 使不同的协议具有互操作性。

本文将解释什么是 ERC-4626，收益率保险库如何工作，以及如何创建收益率保险库 ERC-4626 令牌智能合约。

### 内容

## 什么是屈服拱顶？

创建智能合同是为了确保各种区块链交易之间的协议得到履行。收益率保险库是一种智能合约，允许用户将不同的 ERC-20 令牌存入令牌池，以换取 vTokens(保险库令牌)。

用户将 vTokens 兑换成初始资本以及任何利润。存放的令牌与其他类似的加密资产交换(汇集)并分布在许多协议上，能够在最短的时间内产生收益。

收益金库根据收益最高的协议对不同的协议进行排序，然后将令牌池的一部分分配给收益最高的协议，同时为想要提取资金的用户保留令牌。在收获利润后，归属程序重复进行。然后，保管库将它们转换回最初存放的令牌。

下图描述了高产拱顶的工作原理:

![vault smart contract diagram](img/d6ffdc3752d3c8eb589722ebc4ce2470.png)

上图可以分解为如下过程:

首先，跳马参与者必须投入代币。保管库将相似的 ERC 代币分组到一个池中。向保管库中的参与者分配保管库令牌，这些令牌反映了他们对池中令牌的要求。

为了优化产量，保险库采用了预编程策略。这种策略寻找收益率最高的机会，并重新分配一定比例的代币，以优化池利润，同时保留一些代币。

当用户提取令牌时，首先从保管库储备中提取令牌，然后从产出池中提取令牌。当参与者退出时，退出费用计算为天然气费、策略费和国库费的总和。

### 屈服拱顶的用例

既然你知道了收益率拱顶的工作原理，你可能想知道如何使用它们。以下是承载屈服的拱顶应用的一些例子:

*   融资:道组织和政府在没有众包的情况下，利用收益率高的金库来筹集资金
*   加密贷款:像 Yearn Finance 这样的公司提供协议，允许用户通过贷款和出售加密资产来最大化他们的利润
*   DCA(美元平均成本)金库:DCA 金库采用收益率策略来优化利润

## 什么是 ERC-4626？

“令牌化金库标准”，也称为 ERC-4626，是令牌化金库的标准协议，代表收益率令牌的份额，建立在 ERC-20 令牌标准的基础上。

换句话说，ERC-4626 是 ERC-20 的扩展，增加了新的功能，允许用户从他们的股份中获利。以前，使用 ERC-20 标准，用户只能提取少于或等于他们存入帐户的代币金额。ERC-4626 允许用户根据金库产生的利润，在一段时间内提取超过初始付款的金额。

作为 ERC-20 的扩展，ERC-4626 实现了以下功能:

*   存款和取款
*   金库余额
*   接口
*   事件

## ERC 历史-4626

2021 年 12 月 22 日，费金融的创始人乔伊·桑托罗和其他五位作家提出了 4626 作为[以太坊改进提案](https://eips.ethereum.org/EIPS/eip-4626#:~:text=Reference%20Implementations-,Abstract,withdrawing%20tokens%20and%20reading%20balances.) (EIP)，以解决收益率代币缺乏标准化这一分散金融中的问题。

如果你想知道“4626”这个数字是怎么来的，ERC-4626 的合著者 T11s 在一条推特上透露，乔伊在一次锻炼中向他建议了这个数字，这个名字听起来比最初提议的“4700”更好

> 随机 4626 有趣的事实:最初我们想等一个干净的 EIP 号码，比如 4700，但乔伊在一次通话中随机提到了当前的号码是 4626，我们都意识到它有一个病态的押韵，因为它是在跑步中，但还是跑回家来确保我们得到了它哈哈

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

## 如何撰写 ERC-4626 保险库合同

建立您的许可证标识符和版本后，构建我们的 vault 的第一步是创建您的 ERC20 和 ERC4626 接口，也称为 IERC20 和 IERC4626。记住，你的契约必须使用你界面中的每一个函数；Solidity 的接口有一个定义好的功能，与逻辑一起实现。它们是继承的，原始契约利用了它们的功能。

下面是一个 IERC20 界面:

```
// SPDX-License-Identifier: MIT
pragma solidity 0.8.7;

interface IERC20{

    function transferFrom(address A, address B, uint C) external view returns(bool);

    function approve() external view returns(uint256);

    function decimals() external view returns(uint256);

    function totalSupply() external view returns(uint256);

    function balanceOf(address account) external view returns(uint256);

    function transfer(address D, uint amount) external ;
}

```

下面是一个 IERC4626 界面:

```
//SPDX-License-Identifier: MIT
pragma solidity 0.8.7;

interface IERC4626 { 
    function totalAssets() external view returns (uint256); 
}

```

创建接口后，使用 ERC20 令牌标准将它们导入到您的合同中。

下一步是写你的合同。为您的合同命名，并导入必要的文件和库。

```
//SPDX-License-Identifier: MIT
pragma solidity 0.8.7;

// Import the necessary files and lib
import "./Interfaces/IERC4626.sol";
import "./Interfaces/IERC20.sol";
import "https://github.com/Rari-Capital/solmate/blob/main/src/tokens/ERC20.sol";

// create your contract and inherit the your imports
contract TokenizedVault is IERC4626, ERC20 {

}

```

然后，创建事件。这里将有两个事件:一个是供用户存放令牌的存款事件，另一个是供用户请求取款的取款事件:

```
// create an event that will the withdraw and deposit function
    event Deposit(address caller, uint256 amt);
    event Withdraw(address caller, address receiver, uint256 amt, uint256 shares);

```

您导入的 ERC-20 令牌将是一个不可变的资产，用作构造函数。为此，您将创建一个名为`asset`的不可变变量来引用您的 ERC20。

之后，制作一个映射来展示用户在存款后作为股东的状态:

```
// create your variables and immutables
    ERC20 public immutable asset;

// a mapping that checks if a user has deposited
    mapping(address => uint256) shareHolder;

```

接下来，创建一个构造函数，将资产变量分配给底层令牌，该令牌的地址与 ERC20 令牌的地址、名称和符号相关联:

```
constructor(ERC20 _underlying, string memory _name, string memory _symbol )
     ERC20(_name, _symbol, 18) {
        asset = _underlying;
    }

```

现在，在配置合同的资产和事件之后，为您的保险库编写函数。用户应该能够存放代币以使契约起到保险库的作用；用户收到存款凭证后存入，称为“股金”。用户将在提款时使用此信息来要求其股份的所有权:

```
// a deposit function that receives assets from users
    function deposit(uint256 assets) public{
        // checks that the deposit is higher than 0
        require (assets > 0, "Deposit less than Zero");

        asset.transferFrom(msg.sender, address(this), assets);
// checks the value of assets the holder has
        shareHolder[msg.sender] += assets;
// mints the reciept(shares)
        _mint(msg.sender, assets);

        emit Deposit(msg.sender, assets);

    }

```

在该功能将代币转移或接受到保管库之前，执行检查以确保用户存放的是实际代币(而不是零代币)。用户成为股东，并且每个股东拥有与他们存放在保管库代币中的数量相等的股份；所以，如果他们存了 50 埃特，他们现在拥有的股份就等于 50 埃特。

以下函数给出存放在此保险库中的资产总额:

```
// returns total number of assets
    function totalAssets() public view override returns(uint256) {
        return asset.balanceOf(address(this));
    } 

```

接下来将实现一个`redeem`函数。此`internal`函数将检查用户是否拥有股份，以及股份数量是否大于零。如果用户是股东，他可以从分配给他的股份中赎回任意数量的股份。

在执行将股份转换为最初存放的相同资产令牌的 burn 函数之前，我们为用户拥有的股份数量分配 10%的固定利息。

请注意，合同所有者可以根据需要调整股份的利率。

最后，它发出取款事件来授予用户对资产的访问权:

```
// users to return shares and get thier token back before they can withdraw, and requiers that the user has a deposit
function redeem(uint256 shares, address receiver ) internal returns (uint256 assets) {
        require(shareHolder[msg.sender] > 0, "Not a share holder");
        shareHolder[msg.sender] -= shares;

        uint256 per = (10 * shares) / 100;

        _burn(msg.sender, shares);

        assets = shares + per;

        emit Withdraw(receiver, assets, per);
        return assets;
    }

```

最后是`withdraw`功能。允许存款但不允许取款的保险库被认为是恶意的。

为了撤销，由`withdraw`函数调用`redeem`函数，该函数将股票转换成资产令牌并计算资产的利息。然后，用户可以使用以下方法提取她的资产和利息:

```
// allow msg.sender to withdraw his deposit plus interest

    function withdraw(uint256 shares, address receiver) public {
        uint256 payout = redeem(shares, receiver);
        asset.transfer(receiver, payout);
    }

}

```

整个合同代码可以在这个 [GitHub gist](https://gist.github.com/wolz-CODElife/0acdee6ac30b85521377be81a0af19ac) 中找到。

## 结论

阅读完本文后，您应该有望更好地了解什么是 ERC、ERC-4626 代币和收益率金库，以及如何为 ERC-4626 代币编写收益率金库智能合约。

你也可以看看我在以太坊官网上为 ERC-4626 写的[文档](https://ethereum.org/en/developers/docs/standards/tokens/erc-4626/)。欢迎在下面的评论区留下你的评论！🥂

## 加入像 Bitso 和 Coinsquare 这样的组织，他们使用 LogRocket 主动监控他们的 Web3 应用

影响用户在您的应用中激活和交易的能力的客户端问题会极大地影响您的底线。如果您对监控 UX 问题、自动显示 JavaScript 错误、跟踪缓慢的网络请求和组件加载时间感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/web3-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/dacb06c713aec161ffeaffae5bd048cd.png)](https://lp.logrocket.com/blg/web3-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/web3-signup)

LogRocket 就像是网络和移动应用的 DVR，记录你的网络应用或网站上发生的一切。您可以汇总和报告关键的前端性能指标，重放用户会话和应用程序状态，记录网络请求，并自动显示所有错误，而不是猜测问题发生的原因。

现代化您调试 web 和移动应用的方式— [开始免费监控](https://lp.logrocket.com/blg/web3-signup)。