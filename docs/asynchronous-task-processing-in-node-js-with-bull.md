# 使用 Bull - LogRocket Blog 在 Node.js 中进行异步任务处理

> 原文：<https://blog.logrocket.com/asynchronous-task-processing-in-node-js-with-bull/>

当处理来自 API 客户端的请求时，您可能会遇到这样的情况:一个请求启动了一个 CPU 密集型操作，这可能会阻塞其他请求。您可以通过在称为队列的处理器中添加有关任务的信息，而不是立即处理这样的任务并阻塞其他请求，而是将它推迟到将来处理。然后，任务消费者将从队列中选取任务并处理它。

队列有助于以优雅的方式解决常见的应用程序扩展和性能挑战。根据 NestJS 文档，队列可以帮助解决的问题包括:

*   消除处理峰值
*   分解可能阻塞 Node.js 事件循环的单一任务
*   提供跨各种服务的可靠通信渠道

[Bull](https://optimalbits.github.io/bull/) 是一个节点库，基于 [Redis](https://redis.io/) 实现了一个快速健壮的队列系统。尽管可以使用 Redis 命令直接实现队列，但 Bull 是 Redis 之上的抽象/包装器。它提供了一个处理所有底层细节的 API，并丰富了 Redis 的基本功能，以便更复杂的用例可以轻松处理。

## 装置

在我们开始使用 Bull 之前，我们需要安装 Redis。按照 [Redis Labs](https://redislabs.com/get-started-with-redis/) 指南上的指南安装 Redis，然后使用 npm 或 yarn 安装 Bull。

```
npm install bull --save

```

或者:

```
yarn add bull

```

## 创建队列

通过实例化一个新的 Bull 实例来创建一个队列。

### 句法

```
Queue(queueName: string, url?: string, opts?: QueueOptions): Queue

```

可选的`url`参数用于指定 Redis 连接字符串。如果没有指定`url`，bull 将尝试连接到运行在`localhost:6379`上的默认 Redis 服务器

### `QueueOptions`界面

```
interface QueueOptions {
  limiter?: RateLimiter;
  redis?: RedisOpts;
  prefix?: string = 'bull'; // prefix for all queue keys.
  defaultJobOptions?: JobOpts;
  settings?: AdvancedSettings;
}

```

### `RateLimiter`

`limiter:RateLimiter`是`QueueOptions`中的可选字段，用于配置一次可以处理的作业的最大数量和持续时间。更多信息见[速率限制器](https://github.com/OptimalBits/bull/blob/master/REFERENCE.md#queue)。

### `RedisOption`

`redis: RedisOpts`也是`QueueOptions`中的可选字段。这是 Redis `url`字符串的替代。详见 [`RedisOpts`](https://github.com/OptimalBits/bull/blob/master/REFERENCE.md#queue) 。

### `AdvancedSettings`

`settings: AdvancedSettings`是一个高级队列配置设置。它是可选的，Bull 警告说，除非您对队列的内部有很好的理解，否则不应该覆盖默认的高级设置。更多信息见[高级设置](https://github.com/OptimalBits/bull/blob/master/REFERENCE.md#queue)。

基本队列如下所示:

```
const Queue = require(bull);

const videoQueue - new Queue('video');

```

### 使用`QueueOptions`创建队列

```
// limit the queue to a maximum of 100 jobs per 10 seconds
const Queue = require(bull);

const videoQueue - new Queue('video', {
  limiter: {
  max: 100,
  duration: 10000
  }
});

```

每个队列实例可以执行三种不同的角色:作业生产者、作业消费者和/或事件监听器。每个队列可以有一个或多个生产者、消费者和侦听器。

#### 生产者

作业生成器创建任务并将其添加到队列实例中。Redis 只存储序列化的数据，所以任务应该作为 JavaScript 对象添加到队列中，这是一种可序列化的数据格式。

```
add(name?: string, data: object, opts?: JobOpts): Promise<Job>

```

如果队列为空，任务将被立即执行。否则，一旦处理器空闲或基于任务优先级，任务将被添加到队列中并执行。

您可以添加可选的 name 参数，以确保只有使用特定名称定义的处理器才能执行任务。命名作业必须有相应的命名使用者。否则，队列会抱怨您缺少给定作业的处理器。

#### **工作选项**

作业可以有与之关联的附加选项。在`add()`方法的数据参数后传递一个选项对象。

[作业选项属性](https://github.com/OptimalBits/bull/blob/master/REFERENCE.md#queueadd)包括:

```
interface JobOpts {
  priority: number; // Optional priority value. ranges from 1 (highest priority) to MAX_INT  (lowest priority). Note that
  // using priorities has a slight impact on performance, so do not use it if not required.

  delay: number; // An amount of miliseconds to wait until this job can be processed. Note that for accurate delays, both
  // server and clients should have their clocks synchronized. [optional].

  attempts: number; // The total number of attempts to try the job until it completes.

  repeat: RepeatOpts; // Repeat job according to a cron specification.

  backoff: number | BackoffOpts; // Backoff setting for automatic retries if the job fails

  lifo: boolean; // if true, adds the job to the right of the queue instead of the left (default false)
  timeout: number; // The number of milliseconds after which the job should be fail with a timeout error [optional]

  jobId: number | string; // Override the job ID - by default, the job ID is a unique
  // integer, but you can use this setting to override it.
  // If you use this option, it is up to you to ensure the
  // jobId is unique. If you attempt to add a job with an id that
  // already exists, it will not be added.

  removeOnComplete: boolean | number; // If true, removes the job when it successfully
  // completes. A number specified the amount of jobs to keep. Default behavior is to keep the job in the completed set.

  removeOnFail: boolean | number; // If true, removes the job when it fails after all attempts. A number specified the amount of jobs to keep
  // Default behavior is to keep the job in the failed set.
  stackTraceLimit: number; // Limits the amount of stack trace lines that will be recorded in the stacktrace.
}

interface RepeatOpts {
  cron?: string; // Cron string
  tz?: string; // Timezone
  startDate?: Date | string | number; // Start date when the repeat job should start repeating (only with cron).
  endDate?: Date | string | number; // End date when the repeat job should stop repeating.
  limit?: number; // Number of times the job should repeat at max.
  every?: number; // Repeat every millis (cron setting cannot be used together with this setting.)
  count?: number; // The start value for the repeat iteration count.
}

interface BackoffOpts {
  type: string; // Backoff type, which can be either `fixed` or `exponential`. A custom backoff strategy can also be specified in `backoffStrategies` on the queue settings.
  delay: number; // Backoff delay, in milliseconds.
}

```

一个基本的生产者应该是这样的:

```
const videoQueue - new Queue('video')

videoQueue.add({video: 'video.mp4'})

```

命名作业可以这样定义:

```
videoQueue.add('video'. {input: 'video.mp4'})

```

以下是使用作业选项自定义作业的示例。

```
videoQueue.add('video'. {input: 'video.mp4'}, {delay: 3000, attempts: 5, lifo: true, timeout: 10000 })

```

## 顾客

作业消费者，也称为工作者，定义流程功能(处理器)。process 函数负责处理队列中的每个作业。

```
process(processor: ((job, done?) => Promise<any>) | string)

```

如果队列是空的，一旦一个作业被添加到队列中，进程函数将被调用。否则，每当 worker 空闲并且队列中有要处理的作业时，就会调用它。

将作业的一个实例作为第一个参数传递给流程函数。作业包括流程功能处理任务所需的所有相关数据。数据包含在作业对象的`data`属性中。一个作业还包含一些方法，例如用于报告作业进度的`progress(progress?: number)`、用于向这个特定于作业的作业添加一个日志行的`log(row: string)`、`moveToCompleted`、`moveToFailed`等。

Bull 按照作业添加到队列的顺序处理作业。如果希望并行处理作业，请指定一个`concurrency`参数。然后，Bull 将并行调用工人，考虑`RateLimiter`的最大值。

```
process(concurrency: number, processor: ((job, done?) => Promise<any>) | string)

```

如上图所示，可以命名一个作业。命名作业只能由命名处理器处理。通过在 process 函数中指定 name 参数来定义命名处理器。

```
process(name: string, concurrency: number, processor: ((job, done?) => Promise<any>) | string)

```

### 事件监听器

在队列和/或作业的整个生命周期中，Bull 会发出有用的事件，您可以使用事件侦听器来侦听这些事件。事件可以是给定队列实例(worker)的本地事件。本地事件的侦听器将只接收给定队列实例中产生的通知。

下面是一个本地进度事件。

```
queue.on('progress', function(job, progress){
  console.log(`${jod.id} is in progress`)
})

```

其他可能的事件类型包括`error`、`waiting`、`active`、`stalled`、`completed`、`failed`、`paused`、`resumed`、`cleaned`、`drained`和`removed`。

通过将`global:`作为本地事件名称的前缀，您可以监听给定队列中所有工作线程产生的所有事件。

下面是一个全球进展事件。

```
queue.on('global:progress', function(jobId){
  console.log(`${jobId} is in progress`)
})

```

注意，对于全局事件，传递的是`jobId`而不是作业对象。

## 实际例子

假设一家电子商务公司希望鼓励客户在其市场上购买新产品。该公司决定增加一个选项，让用户选择接收关于新产品的电子邮件。

* * *

### 更多来自 LogRocket 的精彩文章:

* * *

因为外发电子邮件是那些可能有很高延迟和失败的互联网服务之一，我们需要将为新市场到达者发送电子邮件的行为排除在这些操作的典型代码流之外。为此，我们将使用任务队列来记录需要向谁发送电子邮件。

```
const Queue = require('bull');
const sgMail = require('@sendgrid/mail');
sgMail.setApiKey(process.env.SENDGRID_API_KEY);

export class EmailQueue{
  constructor(){
    // initialize queue
    this.queue = new Queue('marketplaceArrival');
    // add a worker
    this.queue.process('email', job => {
      this.sendEmail(job)
    })
  }
  addEmailToQueue(data){
    this.queue.add('email', data)
  }
  async sendEmail(job){
    const { to, from, subject, text, html} = job.data;
    const msg = {
      to,
      from,
      subject,
      text,
      html
    };
    try {
      await sgMail.send(msg)
      job.moveToCompleted('done', true)
    } catch (error) {
      if (error.response) {
        job.moveToFailed({message: 'job failed'})
      }
    }
  }
}

```

## 结论

到目前为止，您应该对 Bull 做什么以及如何使用它有了坚实的基础理解。

要了解更多关于用 Bull 实现任务队列的信息，请查看 GitHub 上的一些常见模式。

## 200 只![](img/61167b9d027ca73ed5aaf59a9ec31267.png)显示器出现故障，生产中网络请求缓慢

部署基于节点的 web 应用程序或网站是容易的部分。确保您的节点实例继续为您的应用程序提供资源是事情变得更加困难的地方。如果您对确保对后端或第三方服务的请求成功感兴趣，

[try LogRocket](https://lp.logrocket.com/blg/node-signup)

.

[![LogRocket Network Request Monitoring](img/cae72fd2a54c5f02a6398c4867894844.png)](https://lp.logrocket.com/blg/node-signup)[https://logrocket.com/signup/](https://lp.logrocket.com/blg/node-signup)

LogRocket 就像是网络和移动应用程序的 DVR，记录下用户与你的应用程序交互时发生的一切。您可以汇总并报告有问题的网络请求，以快速了解根本原因，而不是猜测问题发生的原因。

LogRocket 检测您的应用程序以记录基线性能计时，如页面加载时间、到达第一个字节的时间、慢速网络请求，还记录 Redux、NgRx 和 Vuex 操作/状态。

[Start monitoring for free](https://lp.logrocket.com/blg/node-signup)

.