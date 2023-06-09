# 创建一个离线第一反应原生应用-日志火箭博客

> 原文：<https://blog.logrocket.com/creating-offline-first-react-native-app/>

有很多理由可以考虑构建一个离线优先的 React 原生应用。你甚至需要在开发你的应用程序之前就考虑这样做，因为如果你等到你的应用程序因为速度慢或者没有反应而得到批评的时候，那就太晚了。

但是离线优先并不是一个万能的解决方案。有几种方法可以实现离线优先，根据你的应用程序的结构，有些方法可以在不改变应用程序架构的情况下工作，而有些则不行。在本文中，我们将介绍在 React Native 中实现离线优先应用程序的五种方法。

## 什么是离线优先？

离线优先意味着你要构建自己的 React 原生应用，这样它就可以在有或没有互联网连接的情况下工作。你可以通过各种方式做到这一点，但是让我们来看看为什么我们想要使用离线优先的应用程序。

最明显的原因是，您的应用程序可能会在断断续续或没有蜂窝服务的位置访问，这就是为什么我首先开始构建离线优先应用程序。由于连接中断，我的应用收到了负面评价。实现离线优先可以解决这个问题，但是使用它还有另一个原因:在本地保存数据更快。

无论您使用什么类型的移动连接，都会有影响用户体验的延迟。通过首先在本地保存数据，然后在后台将数据同步到服务，加载指示器在用户界面上花费的时间更少，用户可以更快地进入下一个任务。

在你的移动应用中使用离线优先意味着无论用户有 3G、4G、5G 还是没有连接，都可以获得相同的响应用户界面。

## 实现脱机优先方法

在现有应用中实现离线优先并不容易，但直到我开始深入研究它，我才意识到有这么多选项，甚至比我在这里列出的还要多，因为每个人创建的 React 原生应用略有不同。

本文从研究如何在现有的 React 原生应用程序中实现离线优先开始。如果你正在从头开始一个应用程序，这些选项中的任何一个都可能适合你。如果您使用的是现有的应用程序，您可能需要进行一些更改。

## 使用 react-native-offline 和 Redux 实现本地优先功能

我们将要介绍的前两种方法都使用 Redux 来处理本地优先的功能。如果你的应用程序的所有部分都使用 Redux，并且它连接到远程服务来保存数据，这是有意义的。您不必编写同步方法，也不必确保本地数据库与远程数据库的类型相同。

react-native-offline 包是一把专门为 react 原生应用设计的瑞士军刀。我们想要一个应用程序，它可以在离线时做在线可以做的一切，并在找到连接时同步更改。

从 react-native-offline 开始，将 react-native-offline 提供的网络缩减器添加到您的根缩减器中:

```
import { createStore, combineReducers } from 'redux';
import { reducer as network } from 'react-native-offline';

const rootReducer = combineReducers({
  // ... your other reducers here ...
  network,
});

const store = createStore(rootReducer);
export default store;

```

现在你有两个选择。您可以使用`ReduxNetworkProvider`作为 Redux provider 的后代，这样它就可以访问商店，如下所示:

```
import store from './reduxStore';
import React from 'react';
import { Provider } from 'react-redux';
import { ReduxNetworkProvider } from 'react-native-offline';

const Root = () => (
  <Provider store={store}>
    <ReduxNetworkProvider>
      <App />
    </ReduxNetworkProvider>
  </Provider>
);

```

另一个选择是，如果你的应用程序使用 Redux sagas，那么从你的根`saga`派生出`networkSaga`。这种方法不需要用额外的功能来包装组件。

```
import { all } from 'redux-saga/effects';
import saga1 from './saga1';
import { networkSaga } from 'react-native-offline';

export default function* rootSaga(): Generator<*, *, *> {
  yield all([
    fork(saga1),
    fork(networkSaga, { pingInterval: 30000 }),
  ]);
}

```

然后，通过使用 react-native-offline 提供的 Redux 中间件，您可以在进行 API 调用之前确定应用程序是否在线。如果它是在线的，一切都如你所料。

如果应用程序离线，调度的操作会存储在一个队列中，一旦应用程序恢复在线，它就可以重新调度，如果你的手机只在应用程序运行时离线，这就足够了。

为了在应用程序启动且没有连接时拥有离线优先功能，您需要在应用程序中添加 [redux-persist](https://github.com/rt2zz/redux-persist) 。这使它能够将应用程序状态的快照存储到设备的内存中，并在应用程序启动时恢复状态。

## 使用 redux-offline 启用离线优先

npm [redux-offline](https://github.com/redux-offline/redux-offline) 包类似于 react-native-offline，因为它使用 redux 来处理联机和脱机功能，但其方式略有不同。首先，将 redux-offline store enhancer 添加到 root reducer 中。

```
import { applyMiddleware, createStore, compose } from 'redux';
import { offline } from '@redux-offline/redux-offline';
import offlineConfig from '@redux-offline/redux-offline/lib/defaults';

const store = createStore(
  reducer,
  compose(
    applyMiddleware(middleware),
    offline(offlineConfig)
  )
);

```

接下来，用离线元数据修饰 Redux 操作:

```
const saveData = data => ({
  type: 'SAVE_DATA',
  payload: { data },
  meta: {
    offline: {
      // the network action to execute:
      effect: { url: '/api/save-data', method: 'POST', json: { data } },
      // action to dispatch when effect succeeds:
      commit: { type: 'SAVE_DATA_COMMIT', meta: { data } },
      // action to dispatch if network action fails permanently:
      rollback: { type: 'SAVE_DATA_ROLLBACK', meta: { data } }
    }
  }
});

```

如果 API 调用完成，将调用提交操作。如果应用程序不在线，redux-offline 将等待连接恢复，并重试保存任何尚未提交的数据。

如果该操作永久失败，则将调用回滚操作，这将把应用程序状态回滚到调用该操作之前的状态。您可以在这里指定发生这种情况时要调用的操作。例如，通知用户他们的更改尚未保存，一旦连接到网络，他们必须重试。

Redux-offline 默认使用 redux-persist，所以您不必像使用 react-native-offline 那样担心编写它的实现。但是 redux-offline 也使用不可靠的 [NetInfo API](https://github.com/react-native-netinfo/react-native-netinfo) 来检查连接。NetInfo API 假设，如果你有一个公共 IP 地址，你是连接到互联网的，这并不总是如此，因为应用程序可能会在收到 IP 地址后失去连接。您可能希望用 ping 您自己的后端来检查连接的方法来替换`reduxOfflineConfig`中的`detectNetwork`方法。

## 将 WatermelonDB 用于复杂的、离线优先的本地应用程序

如果您的 React Native 应用程序很简单，并且您使用远程服务将您的数据存储在数据库中并在整个应用程序中进行还原，那么以上两个 npm 包中的任何一个都将为您工作。

但是对于数据密集型的应用程序，这些方法可能会降低应用程序的速度。在不必要的地方使用 Redux 足以降低加载速度，而且这两个包都需要 Redux 来处理任何在线和离线数据。

对于由 SQL 数据库支持的更复杂的应用程序来说， [WatermelonDB 是一个很好的选择](https://github.com/Nozbe/WatermelonDB)。使用 WatermelonDB，所有数据都保存在 SQLite 数据库中，并使用单独的本地线程进行本地访问。西瓜也懒。它只在需要的时候加载数据，所以查询可以快速解决。

WatermelonDB 只是一个本地数据库，但是它还提供了同步原语和同步适配器，可以用来将本地数据同步到远程数据库。要使用 WatermelonDB 同步数据，需要在后端创建两个 API 端点——一个用于推送更改，一个用于拉取更改。您还必须创建自己的逻辑来确定何时同步这些数据。关于如何工作的更多信息，请查看如何使用 [WatermelonDB 进行离线数据同步](https://blog.logrocket.com/watermelondb-offline-data-sync/)。

## 将 MongoDB 领域用于数据密集型应用程序

如果您的数据密集型应用程序使用非关系数据，那么 WatermelonDB 可能不是最佳解决方案。MongoDB 领域可能是一个更好的解决方案。Realm database 是一个本地 NoSQL 数据库，可以在 React 本地应用程序中使用。它可以与 MongoDB Atlas 集成。

如果您选择使用 Realm，那么使用[MongoDB Realm React Native SDK](https://docs.mongodb.com/realm/sdk/react-native/)创建 React Native app 相对简单。Realm 具有内置的用户管理功能，允许使用各种身份验证提供商(包括电子邮件/密码、JWT、脸书、谷歌和苹果)跨设备进行用户创建和身份验证。如果您选择将数据同步到云中托管的 MongoDB，该特性也会内置到 SDK 中。

## SQLite 和云存储

这对于副业项目和业余爱好应用来说是一个很好的选择。这也是构建 React 原生应用原型的简单方法。这是一个非常基本的概念:简单地使用 SQLite 在本地存储数据，然后使用 Dropbox 这样的云服务将数据库同步到云。

对于拼图的 [SQLite](https://www.sqlite.org/index.html) 部分，您将想要使用[react-native-SQLite-storage](https://github.com/andpor/react-native-sqlite-storage)NPM 包。关于这方面的更多细节，请查看这篇关于使用 SQLite 和 React Native 的文章。

下一步是添加 DropBox 或另一个云存储提供商。你必须进入 [Dropbox 开发者页面](https://www.dropbox.com/developers)来创建你的应用。然后你将需要创建一个方法来授权 DropBox 和[一个方法来同步数据库文件](https://implementationdetails.dev/blog/2018/12/05/sync-react-native-sqlite-db-with-dropbox/)。

## 结论

离线优先可以改变用户对你的 react 原生应用的反应方式，尤其是当它被用于网络连接不稳定的领域时。但是没有简单的一刀切的解决方案。

在本文中，我甚至没有涵盖所有可能的解决方案。如果您是从零开始，您可以使用这些选项中的任何一个。然而，对于现有的应用程序，除非您已经使用 Redux 来处理所有的状态，否则您可能必须进行一些架构上的更改。编码快乐！

## [LogRocket](https://lp.logrocket.com/blg/react-native-signup) :即时重现 React 原生应用中的问题。

[![](img/110055665562c1e02069b3698e6cc671.png)](https://lp.logrocket.com/blg/react-native-signup)

[LogRocket](https://lp.logrocket.com/blg/react-native-signup) 是一款 React 原生监控解决方案，可帮助您即时重现问题、确定 bug 的优先级并了解 React 原生应用的性能。

LogRocket 还可以向你展示用户是如何与你的应用程序互动的，从而帮助你提高转化率和产品使用率。LogRocket 的产品分析功能揭示了用户不完成特定流程或不采用新功能的原因。

开始主动监控您的 React 原生应用— [免费试用 LogRocket】。](https://lp.logrocket.com/blg/react-native-signup)