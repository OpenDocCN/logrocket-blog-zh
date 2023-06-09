# 基于区块链的 7 大数据库

> 原文：<https://blog.logrocket.com/top-7-blockchain-based-databases/>

区块链从一开始就和加密货币绑在了一起。区块链是建立加密货币的技术。它旨在提高透明度，满足集中管理的需求。从技术上来说，你可以不使用区块链来建立一种加密货币，但这是另一天的讨论。

但是区块链不仅仅是加密货币。你可以使用区块链技术交换任何东西，从虚拟货币到数字资产，如不可替代的代币。由于其多功能性，区块链正被越来越广泛的行业和学科所采用。

从技术上讲，区块链是一个数据库，但数据库不同于基于区块链的数据库。在本教程中，我们将向您介绍基于区块链的数据库的概念，并评估区块链开发人员目前可用的一些顶级数据库解决方案。

以下是我们将要介绍的内容:

## 什么是基于区块链的数据库？

区块链是一个分布式数据库，记录或公开网络上进行的交易。

为了描绘一幅图画，让我们说四个商业伙伴，爱丽丝，鲍勃，汤姆和哈代，正在开始一个连锁店。他们每个人拥有一家商店，他们总共有四家商店。合作伙伴决定将每个商店的利润和销售额存储在一个数据库中(例如 MySQL)。

数据库容易受到许多潜在问题的影响，包括但不限于以下问题。

*   数据库可能会被恶意行为者破坏
*   由于其集中的性质，数据库的崩溃或故障会影响所有记录
*   恶意或不知情的参与者可能会更改数据库中的记录
*   授权方可以在不验证其真实性的情况下将记录输入数据库
*   一个合作伙伴可能会意外更改或删除另一个合作伙伴输入的数据

考虑到无数的安全风险，Alice、Bob、Tom 和 Hardy 明智地选择在他们的数据库中使用区块链技术。

现在每个合伙人都有记录或数据库的副本。如果一个记录被输入到一个数据库中，它将被广播到所有其他数据库，这些数据库必须在记录被输入到用户的记录之前对其进行审查。一旦它被所有的参与者审查，那么记录就被输入到用户的数据库中，并且新的副本被发送给节点中的所有人。

利用区块链取得了团队的工作成果:

1.  透明的
2.  安全的
3.  不变的
4.  分散的

## 区块链与传统数据库

基于区块链的数据库和传统数据库相似，都存储信息，但功能不同。基于区块链的数据库补充了传统数据库的功能和特征。

简单地说，每个区块链都是一个数据库，但是[每个数据库都不是区块链](https://intellipaat.com/blog/tutorial/blockchain-tutorial/blockchain-vs-database/)。分散的本质——当然，还有底层的区块链理工大学——是区块链的数据库区别于传统数据库的地方。

区块链是一种以块为单位存储数据的数字分类账。这些块是分散的，并在网络中的所有节点上广播。传统的数据库，无论是 RDMS 还是 NoSQL 的数据库，也被用来存储数据。

这里明显的相似之处是，基于区块链的数据库和传统的数据库都用来存储和保存信息。它们都可以存储任何类型的数据，无论是二进制文件、媒体文件、文本文件等等。

传统的数据库是集中式的，这意味着有一个中央管理员控制着数据库。我们每天使用的许多网站和应用程序都使用传统的数据库。例如，Twitter 控制着存储我们推文的数据库。作为数据库的管理员，Twitter 控制着我们看到的内容；如果有一天 Twitter 决定关闭，我们可以和我们的推文和文件说再见了。

使用区块链，数据库不是中央的，也没有管理员。这是一个点对点的网络，就像 Napster 一样。每个人都连接到网络，网络中的每个节点都有当前数据库的副本。

当网络中的一个节点想要在数据库中写入或创建新记录时，该节点首先创建该记录，并将其广播给网络中的所有节点。所有这些节点然后使用一个[一致算法](https://blog.logrocket.com/guide-blockchain-consensus-protocols/)来审查新记录。

如果所有节点的审查过程都成功，则该节点将记录写入其数据库并广播它。然后，网络中的每个节点将记录写入各自的数据库，这样状态和记录是一致的和最新的。

这使得存储在区块链中的数据很难被篡改或复制。它是透明的，因为所有节点都要检查要输入数据库的每条记录。

概括来说，基于区块链的数据库和传统数据库都用于存储信息，但它们在功能上有所不同，如下表所示:

| **区块链** | **数据库** |
| --- | --- |
| 分散 | 中央集权的 |
| 未经许可 | 许可的 |

## 如何为您的项目选择正确的基于区块链的数据库

你如何知道什么时候使用基于区块链的数据库，什么时候在你的项目中使用传统的数据库？让我们回顾一下一些主要的考虑因素。

### 成本支持

在决定为您的项目使用哪种类型的数据库时，成本是要考虑的最重要的事情之一。

因为老式的数据存储方式仍然非常流行，所以使用普通数据库的成本要比使用区块链系统的成本低得多。构建一个区块链并将其集成到您的项目中的费用与普通软件开发的费用相当。

### 容错

如果您决定使用区块链，您将体验到您所能想象的最健壮和容错的数据库。因为传统的数据库是集中式的，所以它可以被黑客攻击和篡改。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

另一方面，区块链很难妥协，如果不是几乎不可能的话。因此，如果您有敏感数据要存储，并需要一个具有高容错能力的数据库，区块链是您的最佳选择。

### 表演

大多数现代数据库的设计都是为了实现高性能。例如，SQL 和 MongoDB 是非常快速的现成产品。无需管理员进行任何优化，读取和写入都非常高效。

区块链则完全相反。在区块链中写入记录相对较慢，因为在将记录写入数据库之前，必须在区块链核心中进行多次检查和往返。

如果高速性能是优先考虑的，那么您应该使用传统的数据库而不是区块链。

### 安全性

在任何业务中，安全性都是一个重要的考虑因素。任何人都可以查看区块链中的数据。但是你可能有充分的理由不希望你的数据公开。

幸运的是，区块链已经发展到你可以在你选择的节点内私下使用区块链网络的程度。传统数据库也可以公开或私下制作和使用。

区块链对其区块中的交易进行加密哈希，每个区块形成一个相互链接的链。这赋予了它高度的透明性，因为没有节点或客户端可以对记录提出错误或争议。

## 位于区块链的 7 大数据库

现在我们已经了解了基于区块链的数据库和传统数据库之间的区别，让我们来看看目前使用的一些最流行的基于区块链的数据库。

### 1.BigchainDB

BigchainDB 是一个基于区块链的数据库，由 MongoDB 提供支持，使您能够将分散和区块链技术添加到您的应用程序中。

BigchainDB v0.1 于 2016 年 2 月首次向世界公布，最初是一个传统的数据库，直到开发团队后来添加了区块链功能。起初它有一些问题。也就是说，它有一个主节点来完成所有的写操作和向其他节点的广播。其他节点只是从这个主节点读取数据。

这个主节点是数据库的单点控制，这违反了区块链的黄金法则。当数据库被更改时，所有其他节点都会看到未更新的更改。

2.0 版本修补了所有这些漏洞，使 BigchainDB 成为世界上最受欢迎的区块链数据库。BigchainDB 的卓越特性包括:

#### 不变

存储在 BigchainDB 中的记录是防篡改的。记录是不可变的，这意味着一旦记录被验证并存储在数据库中，它就不能被修改或更改。

#### 分散

数据库在 P2P 网络中是分散的。没有单一的指挥点。Bigchain 网络中的每个节点都有一个 MongoDB 数据库的本地副本，它将 [Tendermint](https://tendermint.com/) 用于网络和共识协议。

使用 Tendermint 的一个优势是它使用[拜占庭容错(BFT)](https://en.wikipedia.org/wiki/Byzantine_fault) ，这使得区块链能够就下一个块的内容达成一致，即使网络中多达一半的节点有故障。因此，如果黑客获得了某个节点的 MongoDB 数据库的访问权限，网络可以删除该特定数据库，但仍然可以运行。

#### 支持多种资产

各种类型的资产可以存储在数据库中。节点中的用户可以发布大链网络中的任何资产。

根据 [BlockchainDB](https://www.bigchaindb.com/developers/guide/key-concepts-of-bigchaindb/#about-our-transaction-model) 的说法，资产可以表征你能想到的任何物理或数字对象，比如汽车、数据集、知识产权等。

#### 高性能

BigchainDB 是在考虑性能的基础上构建的。Tendermint 的使用使 BigchainDB 实现高性能成为可能。

Tendermint 只需要几秒钟就可以处理大型事务并将它们提交给新的块。这与在区块链中提交事务需要大量时间的观点背道而驰。

BigchainDB 在许多场景中工作得非常好，特别是在供应链商店中，那里需要组织数据并提供不变性和透明性。

| **数据库** | **共识** |
| --- | --- |
| MongoDB | Raft 算法 |

### 2.卡桑德拉

Apache Cassandra 是一个开源的 NoSQL 分布式数据库，在不牺牲性能的情况下提供线性可伸缩性和高可用性。

Cassandra 于 2008 年首次发布，用 Java 编写，可以跨许多商用服务器处理大量数据，提供无单点故障的高可用性。

Cassandra 对行进行分区，每一行都包含具有所需主键的表。通过这种划分，Cassandra 可以将这些行分布在多个网络和设备上。当删除行和分区并将其添加到网络中时，它会在整个网络中进行调整。

Cassandra 有许多显著的特点，使其成为一家独特的区块链数据库。

#### 分布的

在 Cassandra 中没有中心节点或单点控制。行和分区分布在整个集群中。没有主集群，因为每个集群既是客户端又是服务器，并且完全相同。

#### 容错的

因为它是分布式的，没有单点控制，所以 Cassandra 是容错的。这是因为集群中的每个节点都有数据库的副本，所以如果集群中的一个节点受到攻击，整个系统不会宕机。折叠节点中的数据仍在集群中的其他节点中，因此数据是安全的。

#### 查询语言

Cassandra 的结构与 SQL 非常相似，它有行、表和列。然而，Cassandra 并不使用 SQL 语言来查询数据。相反，它有自己的查询语言，卡珊德拉查询语言(CQL)。

| **数据库** | **共识** |
| --- | --- |
| NoSQL | 帕克斯 |

### 3.ChainifyDB

ChainifyDB 是一个针对数据库的区块链解决方案。它提供了一个层，在这个层中，数据库可以插入到 ChainifyDB 网络中，并在插入的数据库网络中同步数据库的记录。

在一个数据库中输入一条记录后，ChainifyDB 会将添加的内容传递给所有其他数据库节点。他们达成某种共识，记录被写入数据库，因此记录是分散的、不可变的和透明的。

ChainifyDB 与其他基于区块链的数据库的区别在于，每个块都有自己的数据库/存储区域。ChainfyDB 没有自己的数据库；它使用提供给它的数据库并在其中插入一个区块链层。换句话说，ChainifyDB 为已经存在的数据库提供了一个[区块链层。](https://arxiv.org/pdf/1912.04820.pdf)

让我们放大一下 ChainifyDB 的独特之处。

#### 端到端加密

ChainifyDB 网络中插入的数据库之间的通信是高度加密的。

#### Web 前端

chainifyDB 的核心要素、组件和维护设置都可以从 web 前端运行。与其他数据库解决方案不同，它不需要很多工具来设置。

#### 无缝侵入式

ChainifyDB 可以无缝连接或插入任何数据存储或数据库，而不会影响数据库上运行的应用程序。

| **数据库** | **共识** |
| --- | --- |
| Postgre，MySQL | 投票 |

### 4.契约 SQL

正如传说中的那样，Auxten Wang 的联合创始人于 2017 年“一个寒风凛冽的日子，当 Jing Mi 和我在一家烧烤店吃饭时”第一次想到了 [CovenantSQL](https://github.com/CovenantSQL/CovenantSQL) 。他给我带来了一个有趣的想法，在区块链上建立一个 SQL 数据库。我对这个想法感到兴奋，并立即决定辞去工作，开始这个项目。”

CovenantSQL 是一个区块链 SQL 数据库。根据[官方网站](https://covenantsql.io/)的消息，CovenantSQL 利用共识协议连接闲置的存储资源，旨在通过全面的 SQL 支持更好地促进 DApp 开发。

CovenantSQL 提供了一个基础设施，在此基础上[构建去中心化的应用，就像以太坊](https://blog.logrocket.com/ethereum-blockchain-development-using-web3-js/)一样。在其众多用例中，CovenantSQL 可用于资产管理，并集成到物联网解决方案中。

CovenantSQL 有很多很棒的特性。下面重点介绍几个。

#### 分散

就像它实现的区块链技术一样，CovenantSQL 在很大程度上分散在 P2P 网络中。这使得它具有容错性，并且无法由单个实体进行治理。

#### 结构化查询语言

SQL 是世界上使用最广泛、最流行的数据库查询语言。CovenantSQL 之所以使用它，是因为它很受欢迎，并且有可能增加额外的区块链杠杆。SQL 支持使得 CovenantSQL 成为一个基于区块链的数据库。

#### 不变

CovenantSQL 的区块链使数据库成为不可变的。所有进入的记录在提交到数据库之前都必须经过网络中所有节点的审查。

CovenantSQL 提供了一个基础设施，开发人员可以在其上构建分散的应用程序。这就像我们在以太坊能做的一样。CovenantSQL 可用于资产管理，也可集成到物联网解决方案中。

| **数据库** | **共识** |
| --- | --- |
| 结构化查询语言 | 大量 |

### 5.莫德克斯·BCDB

[Modex 区块链数据库(BCDB)](https://modex.tech/blockchain-database-product/) 是一款中间件软件产品，为希望开发区块链软件的组织提供即插即用的方法。

Modex BCDB 位于客户端应用程序和数据库之间。这种方法不同于其他区块链区议会。它插入 DBs 并修改它们的连接器，[在它们之间提供一个区块链层](https://modex.tech/wp-content/uploads/2020/10/Modex_Whitepaper.pdf)。

Modex BCDB 有各种各样的[功能](https://modex.tech/product-features/)，我们将在下面进行分解。

#### 多区块链支持

Modex BCDB 是灵活的，因为它可以使用其他区块链框架。它目前使用 Hyperledger 锯齿框架，并在其网络和共识协议中使用 Tendermint 协议。也可以使用其他框架以太坊和 Hyperledger 结构。

#### 多数据库支持

Modex BCDB 支持多个数据库。一个节点可以使用 MongoDB，而另一个节点可以使用 MySQL，Modex BCDB 可以无缝地工作，并与它们同步数据，无需任何配置，也无需移植到支持的数据库。

#### 数据管理

Modex BCDB 可以完美地管理数据，而不影响安全性。Modex BCDB 知道何时以及向哪个节点暴露部分或全部数据。Modex BCDB 网络中的完整节点暴露于全部数据，部分节点仅暴露于其 API 所请求的数据，而私有节点仅暴露于其私有的数据，而不暴露于其他节点。

| **数据库** | **共识** |
| --- | --- |
| SQL，NoSQL | 利害关系证明 |

### 6.邮政链

[Postchain](https://postchain-docs.readthedocs.io/en/latest/) 是瑞典 Chromeby 开发的区块链平台。Postchain 是一个区块链框架，就像以太坊或者超账本一样；它有一个节点网络，这些节点通过一个权威证明共识算法来维护一组数据。

Postchain 将这些数据存储在 SQL 数据库中，这与所有其他区块链框架不同。同样，Postchain 的[事务逻辑](https://postchain-docs.readthedocs.io/en/latest/overview.html)可以在 SQL 代码中定义。这就是 Postchain 成为区块链数据库的原因。

事务不是通过 SQL 代码写入数据库的。Postchain 具有在 Postchain 网络中的每个节点上工作的验证器。交易通过高度加密和签名的消息提交。验证器拾取消息并同步运行，以验证消息的证据和来源。进行这种同步是为了使网络中的所有节点在其数据库中具有相同的状态。

### 7.ProvenDB

根据 CTO Guy Harrison 的说法，ProvenDB 是一个基于 MongoDB 的区块链数据库服务。

ProvenDB 是一个结合了 MongoDB 数据库和区块链特性的数据库服务。通过使用 ProvenDB，您可以将 MongoDB 数据库与区块链一起使用。

在其最重要的特性中， [ProvenDB](https://compliancevault.readme.io/docs) 提供了:

*   防篡改数字数据存储器
*   不可变的数据存储；记录一旦被输入 ProvenDB 数据库，就不能被修改、编辑或删除。它在数据库中保持“固定”
*   一个高度安全的区块链数据库，可用于存储各种敏感数据，如不易被篡改的财务记录、知识产权、法律文档、公共记录等

如果您想使用 MongoDB 并在应用程序中利用区块链，ProvenDB 是一个不错的选择。ProvenDB 提供了一个 REST APIs，您可以使用它来进行防篡改事务和存储文档。

[ProvenDB](https://www.provendb.com/litepaper) 为数据工程师提供一个加密防篡改的安全数据库。难怪它得到了众多大公司的认可，包括多巴资本、微软、瑞格科技、CRN 等等。

| **数据库** | **共识** |
| --- | --- |
| MongoDB | 没有人 |

## 结论

区块链正在快速进化。它始于比特币热潮，现在全世界都开始看到区块链在各种各样的行业中拥有令人敬畏的力量。有这么多基于区块链的数据库，但上面提到的七个因其受欢迎程度和广泛的特性而与众不同。

在本指南中，我们向您简要介绍了区块链，并重点介绍了基于区块链的数据库的概念，包括传统数据库和基于区块链的数据库之间的异同。

然后，我们就如何为您的项目选择最好的基于区块链的数据库提供了一些提示。最后，我们评估了七个位于区块链的顶级数据库，并详细讨论了它们的功能、用例以及它们的超能力。

## 加入像 Bitso 和 Coinsquare 这样的组织，他们使用 LogRocket 主动监控他们的 Web3 应用

影响用户在您的应用中激活和交易的能力的客户端问题会极大地影响您的底线。如果您对监控 UX 问题、自动显示 JavaScript 错误、跟踪缓慢的网络请求和组件加载时间感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/web3-signup)

.

[![LogRocket Dashboard Free Trial Banner](img/dacb06c713aec161ffeaffae5bd048cd.png)](https://lp.logrocket.com/blg/web3-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/web3-signup)

LogRocket 就像是网络和移动应用的 DVR，记录你的网络应用或网站上发生的一切。您可以汇总和报告关键的前端性能指标，重放用户会话和应用程序状态，记录网络请求，并自动显示所有错误，而不是猜测问题发生的原因。

现代化您调试 web 和移动应用的方式— [开始免费监控](https://lp.logrocket.com/blg/web3-signup)。