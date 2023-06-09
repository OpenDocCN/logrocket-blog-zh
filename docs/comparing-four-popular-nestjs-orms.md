# 比较 4 个流行的 NestJS ORM-log rocket 博客

> 原文：<https://blog.logrocket.com/comparing-four-popular-nestjs-orms/>

NestJS 是一个流行的 Node.js 服务器端框架。它是高度可定制的，带有丰富的生态系统，并且兼容大多数 Node.js 库，包括 ORM 库。

## NestJS 中的 ORM

对象关系映射(ORM)是一种将数据库表抽象为内存中的数据对象的技术。它允许您使用数据对象查询数据并将数据写入数据库。ORM 用于简化数据库访问，因为开发人员不需要编写原始查询。

ORM 有其局限性，比如复杂查询的性能问题，但是如果用在正确的地方，它们仍然可以让你的生活变得更轻松。

NestJS 是数据库无关的。为了方便起见，NestJS 通过@nestjs/typeorm 和@nestjs/sequelize 包提供了与 TypeORM 和 Sequelize 的紧密集成。您也可以直接使用任何通用 Node.js 数据库集成库或 ORM，但是 NestJS ORMs 的生态系统是如此庞大，为您的项目选择正确的生态系统可能会令人望而生畏。

在这篇文章中，我们将通过 NestJS 使用四种常见的 ORM。它们是:

我们的目的是总结它们的共同特征，比如流行度、特性、文档、成熟度和关于迁移的信息。对于每一个 ORM，都会提供一个代码片段来给你一个应用框架的最简单的例子。

## NestJS 和 Sequelize

大约在 2014 年推出， [Sequelize](https://github.com/sequelize/sequelize) 是 Node.js 的一个易于使用且基于承诺的 ORM。

它支持许多数据库，包括 PostgreSQL、MySQL、MariaDB、SQLite、DB2 和 MSSQL。它的局限性包括缺乏 NoSQL 支持，只支持活动记录模式。

### 特征

Sequelize 提供了一组丰富的特性:事务和迁移支持、模型验证、急切加载和延迟加载、读取复制等等。Sequelize 有合理的文档，包含丰富的信息和很好的例子，但有时我发现搜索特定的主题并不容易。

Sequelize 附带了一个 CLI，可以创建数据库、初始化配置和种子，或者管理迁移。它还使用了[活动记录](https://www.martinfowler.com/eaaCatalog/activeRecord.html)模式。在活动记录模式中，数据库行被映射到应用程序中的一个对象，数据库表由一个类表示。因此，当我们创建一个实体对象并调用`save`方法时，一个新行被添加到数据库表中。

活动记录模式的主要好处是它的简单性:您可以直接使用实体类来表示数据库表并与之交互。

### 设置

Sequelize 易于设置和使用。下面是基本数据库配置和操作的示例。

```
// First, we create a Sequelize instance with an options object
export const databaseProviders = [
  {
    provide: 'SEQUELIZE',
    useFactory: async () => {
      const sequelize = new Sequelize({
        dialect: 'postgres',
        host: 'localhost',
        port: 5432,
        username: 'postgres',
        password: 'postgres',
        database: 'postgres',
      });
      sequelize.addModels([Cat]); // Add all models
      await sequelize.sync(); // Sync database tables
      return sequelize;
    },
  },
];

// Then, export the provider to make it accessible
@Module({
  providers: [...databaseProviders],
  exports: [...databaseProviders],
})
export class DatabaseModule {}

// Define model entity, each represents a table in the database
@Table
export class Cat extends Model {
  @Column
  name: string;

  @Column
  age: number;

  @Column
  breed: string;
}
// Create a repository provider
export const catsProviders = [
  {
    provide: 'CATS_REPOSITORY',
    useValue: Cat,
  },
];
// In CatsService, we inject the repository
export class CatsService {
  constructor(
    @Inject('CATS_REPOSITORY')
    private catsRepository: typeof Cat,
  ) {}

// Then, we can perform database operations
this.catsRepository.findAll<Cat>();
```

在某些情况下，执行原始 SQL 查询更容易，您可以使用函数`sequelize.query`。

```
// SQL Script. Source https://sequelize.org/docs/v6/core-concepts/raw-queries/
const [results, metadata] = await sequelize.query("UPDATE users SET y = 42 WHERE x = 12");
// Results will be an empty array and metadata will contain the number of affected rows.
```

Sequelize 正在努力工作[使正式的类型脚本支持成为可能](https://sequelize.org/docs/v7/other-topics/typescript/)，但是现在，仍然推荐你在你的项目中使用`[sequelize-typescript package](https://www.npmjs.com/package/sequelize-typescript)`来处理类型脚本。

### 社区和受欢迎程度

红杉是一种非常成熟和稳定的 ORM。它有一个活跃的社区和大量必要的工具。作为最流行的 ORM 框架之一，Sequelize 在 GitHub 上有 26K stars 和 4K 福克斯。也是 NestJS 用`@nestjs/sequelize`直接支持的。

### 用例

Sequelize 是 Node.js 应用程序的通用 ORM。如果你正在寻找一个稳定的，易于使用的 ORM，它值得你考虑。

### 迁移到 Sequelize

要将现有应用程序迁移到 NestJS 中的 Sequelize，基本步骤如下:

*   设置 Sequelize 数据库提供程序
*   从现有数据库模式创建模型
*   为每个模型创建一个存储库提供者，并将其注入到相应的服务中

Sequelize 不提供从现有数据库生成模型的内置支持。当迁移现有的应用程序时，您可以选择手动创建模型，或者使用[sequelize-typescript-generator](https://github.com/spinlud/sequelize-typescript-generator)从现有的数据库中生成模型。

切换 ORM 是一个重大的变化，但这也是一个重构现有代码使其更易维护的机会。如果现有的应用程序没有清晰的结构，那么是时候在服务和数据库访问层之间实现清晰的分离了。

## NestJS 和 TypeORM

[TypeORM](https://typeorm.io/) 是 Node.js 的另一个成熟的 ORM，它有丰富的特性集，包括实体管理器、连接池、复制和查询缓存。NestJS 也用自己的包直接支持它，`@nestjs/typeorm`。

TypeORM 于 2016 年发布，支持方言 PostgreSQL、MySQL、MariaDB、SQLite、MSSQL 和 MongoDB。这意味着您可以通过 TypeORM 同时使用 NoSQL 和 SQL 数据库。

### 特征

TypeORM 提供了一个 CLI，可以创建实体、项目和订阅者或管理迁移。它支持[活动记录](https://www.martinfowler.com/eaaCatalog/activeRecord.html)和[数据映射器](https://martinfowler.com/eaaCatalog/dataMapper.html)模式。

数据映射器模式在应用程序的业务领域和数据库之间添加了一个数据访问层(DAL)。使用数据映射器模式提供了更大的灵活性和更好的性能，因为它比简单的活动记录实现更有效地利用了数据库。提供这两种方法可以让您选择适合您的应用程序的模式。

很像一些开源项目，它的文档有改进的空间。一个常见的问题是缺乏必要的 API 细节。

### 设置

下面是使用活动记录模式的 TypeORM 的基本数据库配置和操作。

```
// First, we set up the Database Connection
@Module({
  imports: [
    TypeOrmModule.forRoot({
      type: 'postgres',
      host: 'localhost',
      port: 5432,
      username: 'postgres',
      password: 'postgres',
      database: 'postgres',
      entities: [Cat],
      synchronize: true, // Sync the entities with the database every time the application runs
    }),
    CatsModule,
  ],
})

// Then you can define the data entity
@Entity()
export class Cat {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;

  @Column()
  breed: string;
}
// We can use the repository design pattern which means each entity has its own repository.
// Here, we inject the catRepository into your service
export class CatsService {
  constructor(
    @InjectRepository(Cat)
    private catsRepository: Repository<Cat>,
  ) {}

// Then you can use the injected repo to perform database operation
  this.catsRepository.find();
  this.catsRepository.save<Cat>(cat);
```

要运行一个原始的 SQL 查询，您可以使用`@InjectConnection` decorator 并使用`query`方法来运行一个定制的查询脚本。

```
// Inject the connection
constructor(@InjectConnection() private readonly connection: Connection) 
{} 
// Run the query
this.connection.query('SELECT * FROM [TableName];');
```

您可以将 JavaScript 或 TypeScript 用于 TypeORM。与其他 ORM 相比，在 TypeScript 中使用 TypeORM 更加自然，因为它是由 TypeScript 编写的。

### 社区和受欢迎程度

它是最受欢迎的 ORM 库之一，GitHub 上有 28.2K 的 stars 和 5.1K 的 forks。

凭借其丰富的特性集和灵活性，TypeORM 是 NestJS 的最佳 ORM 之一。

### 用例

TypeORM 可以在 Node.js 和许多其他平台上运行。如果您的应用程序需要以下一个或多个特性，这是一个很好的选择:

*   可扩展至大型企业级应用
*   支持多个数据库
*   支持 SQL 和 NoSQL 数据库
*   支持多种平台，包括 Node.js、Ionic、Cordova、React Native、NativeScript、Expo 或 Electron

### 迁移到 TypeORM

在迁移到 TypeORM 之前，有必要考虑以下几点:

*   为您的应用选择正确的模式。TypeORM 支持活动记录和数据映射模式。活动记录简单直观，但是与数据库紧密耦合。它适用于小型 CRUD 应用。对于复杂的应用程序，数据映射器将是更好的选择，因为它允许更灵活的设计和业务域与数据访问层之间更清晰的分离
*   利用 TypeORM 强大的类型支持。例如，为实体 id 实现名义类型将防止 id 被错误地分配给不同的实体(类型)

要在 NestJS 中迁移到 TypeORM，基本步骤包括:

*   配置数据库连接
*   创建实体模型类和相应的服务。实体模型可以手动创建，也可以使用 [typeorm-model-generator](https://github.com/Kononnable/typeorm-model-generator) 从数据库中生成。typeorm-model-generator 包支持 MSSQL、Postgres 和 SQLite

如果您是从 Sequelize 迁移过来的，[这篇文档](https://orkhan.gitbook.io/typeorm/docs/sequelize-migration)讨论了这两种 ORM 之间的相似和不同之处。从 Sequelize 转移相对容易，因为两种 ORM 都支持活动记录模式。

## NestJS 和 MikroORM

MikroORM 是 Node.js 的另一种类型脚本 ORM，它基于数据映射器、工作单元和身份映射模式。

于 2018 年推出，是一个相对年轻的 ORM。MikroORM 支持 SQL 和 NoSQL 数据库，包括 MongoDB、MySQL、PostgreSQL 和 SQLite 数据库。MikroORM 也有自己的 NestJS 支持，`[@mikro-orm/nestjs package](https://mikro-orm.io/docs/usage-with-nestjs/)`，这是一个第三方包，并不由 NestJS 团队管理。

### 特征

MikroORM 提供了一系列令人印象深刻的特性，包括事务、对身份映射模式的支持、级联持久化和移除、查询构建器等等。这是一个全功能的 ORM，具有所有主要的数据库选项，如果您想转换，可以很容易地从 TypeORM 迁移到它。

它附带了一个 CLI 工具，可以创建/更新/删除数据库模式、管理数据库迁移和生成实体。

MikroORM 支持数据映射模式。它也是基于[工作单元](https://martinfowler.com/eaaCatalog/unitOfWork.html)和[身份图](https://martinfowler.com/eaaCatalog/identityMap.html)模式构建的。实现工作单元允许我们自动处理事务。它还通过身份映射模式针对事务进行了优化，这使得避免不必要的数据库往返成为可能。这些模式也有助于 MikroORM 获得良好的性能:[最近的基准测试](https://mikro-orm.io/blog/mikro-orm-4-1-released)显示，使用 SQLite 插入 10K 实体只需要大约 70 毫秒。

### 设置

MikroORM 的语法非常简单明了。下面是一个在 NestJS 中使用 MikroORM 进行最简单的数据库操作的例子。

```
// Firstly, import MikroOrmModule in App.module
@Module({
  imports: [MikroOrmModule.forRoot(), CatsModule],
})
// The Database configuration is stored in mikro-orm.config.ts
const config: Options = {
  entities: [Cat],
  dbName: 'postgres',
  type: 'postgresql',
  port: 5432,
  host: 'localhost',
  debug: true,
  user: 'postgres',
  password: 'postgres',
} as Options;

// The config paths are defined in package.json
  "mikro-orm": {
    "useTsNode": true,
    "configPaths": [
      "./src/mikro-orm.config.ts",
      "./dist/mikro-orm.config.js"
    ]
  }
// To use repository pattern, we register entities via forFeature() in feature module
@Module({
  imports: [MikroOrmModule.forFeature({ entities: [Cat] })],
})
export class CatsModule {}

// We inject the repository into service
  constructor(
    @InjectRepository(Cat)
    private readonly catRepository: EntityRepository<Cat>,
  ) {}

 // Then, we can perform database operation
this.catRepository.findOne(findOneOptions);
```

### 社区和受欢迎程度

MikroORM 的文档得到积极维护，易于浏览。虽然它是最年轻的 ORM 之一，但它在 GitHub 中没有一个很长的开放问题列表。回购得到积极维护，问题通常会很快得到解决。

MikroORM 的构建是为了克服其他 Node.js 表单存在的一些问题，比如缺乏事务支持。它发展迅速，除了一系列优秀的特性之外，还提供了强大的类型安全性。

### 用例

MikroORM 因其独特的功能、强大的类型和良好的支持而脱颖而出。如果您正在为以下方面寻找 ORM，这是值得考虑的:

*   一个需要良好交易支持的绿地应用
*   需要定期批量数据更新和快速性能的应用程序

### 迁移到 MikroORM

迁移到新的 ORM 是一个昂贵的决定。在切换到 MikroORM 之前，确保您的应用程序可以利用它的许多特性。如果您的应用程序属于以下任何一类，您可能希望选择另一个 ORM:

*   只有少量表格的小型应用程序
*   不需要嵌套表或复杂的连接查询
*   用户不多，因此没有性能问题

使用内置工具可以帮助您节省时间，并提高迁移中的代码质量。MikroORM 附带了几个在开发过程中非常有用的命令行工具，比如[模式生成器](https://mikro-orm.io/docs/schema-generator)、[实体生成器](https://mikro-orm.io/docs/entity-generator)和[迁移](https://mikro-orm.io/docs/migrations)。

*   实体生成器:可用于从现有数据库模式生成实体
*   模式生成器:可用于从实体元数据中生成、更新或删除模式。这个工具只能在开发中使用
*   迁移:允许您生成具有当前模式差异的迁移

## 内斯特和普里斯马

[Prisma](https://github.com/prisma/prisma) 是 Node.js 的开源 ORM，目前支持 PostgreSQL、MySQL、SQL Server、SQLite、MongoDB、CockroachDB(目前仍只提供预览版)。

Prisma 与 NestJS 顺利融合。与其他 ORM 不同，NestJS-ORM 集成不需要单独的包。相反，Prisma CLI 包是我们在 NestJS 中使用 Prisma 唯一需要的东西。

### 特征

与本文中讨论的其他 ORM 相比，Prisma 是一个独特的库。它不像其他 ORM 那样使用实体模型，而是使用模式定义语言。开发人员可以使用 Prisma schema 定义数据模型，这些模型用于为所选数据库生成迁移文件。它还生成与数据库交互的强类型代码。

模式文件被用作数据库和应用程序的单一事实来源，基于它的代码生成有助于构建类型安全的查询，以便在编译时捕捉错误。

Prisma 提供了很好的工具，包括 Prisma CLI 和 Prisma Studio。您可以使用 CLI 创建迁移或使用 Studio 检查数据库，这与 Prisma 的实体结构配合得很好。它还为数据库托管提供了一个 Prisma 数据平台。

Prisma 的文档被很好地格式化并且被积极地维护。

### 设置

使用 Prisma，开发人员可以简单地定义他们的模式，而不用担心具体的 ORM 框架。下面是 PrismaORM 与 NestJS 的最基本用法。

```
// Firstly, we need a schema file to model the database
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL") // the DATABASE_URL is stored in .env file
}

model Cat {
  id            Int         @default(autoincrement()) @id
  name          String
  age           Int
  breed         String   
}

// To generate SQL files and also runs them on the configured database, run the following command
 npx prisma migrate dev --name init

// Create a Database service to interact with the Prisma Client API
@Injectable()
export class PrismaService extends PrismaClient implements OnModuleInit {
  async onModuleInit() {
    await this.$connect();
  }

  async enableShutdownHooks(app: INestApplication) {
    this.$on('beforeExit', async () => {
      await app.close();
    });
  }
}
// In the service layer, we inject the DatabaseService
constructor(private prisma: PrismaService) {}

// We can perform database operation via Prisma client api as below
// Please note that we use Prisma Client's generated types, like the "Cat"
  async cat(
    catWhereUniqueInput: Prisma.CatWhereUniqueInput,
  ): Promise<Cat | null> {
    return this.prisma.cat.findUnique({
      where: catWhereUniqueInput,
    });
  }
```

### 社区和受欢迎程度

Prisma 于 2019 年首次发布，是我们讨论的四种 ORM 中的最新一种。这需要时间来达到更成熟的状态。最近，版本 3 的发布引入了一些[突破性的变化](https://www.prisma.io/docs/guides/upgrade-guides/upgrading-versions/upgrading-to-prisma-3)。GitHub 中也注意到了一些现存的问题，比如它[不支持某些 Postgres 列类型](https://github.com/prisma/prisma1/issues/2466)。

拥有 23.3K GitHub stars 和 828 个 forks，人气迅速增长。

### 用例

总的来说，Prisma 是一个非常有前途的 ORM。它旨在缓解传统 ORM 中存在的问题，可以与 JavaScript 或 TypeScript 一起使用。使用 TypeScript 的优点是，您可以利用 Prisma 客户端生成的类型来实现更好的类型安全。

Prisma 的一些好的使用案例有:

*   新应用的快速原型开发
*   需要良好数据库工具和强大类型支持的应用程序

### 迁移到 Prisma

Prisma 支持增量采用。这意味着您可以逐步将数据库查询迁移到 Prisma，而不是一次性迁移整个应用程序。

向 Prisma 的迁移包括以下一般步骤，但迁移过程的详细信息可在[Prisma 文档](https://www.prisma.io/docs/guides/migrate-to-prisma/migrate-from-typeorm)中找到。

*   安装 Prisma CLI
*   自省当前数据库:用反映当前数据库模式的数据模型填充 Prisma 模式
*   安装 Prisma 客户端
*   逐步用 Prisma 客户端取代现有的数据库查询

## 摘要

有许多 ORM 可以和 NestJS 一起使用。因此，为您的 NestJS 应用程序选择合适的 ORM 可能是一个艰难的决定。在本文中，我们介绍了四种流行的 ORM，并讨论了它们的优缺点。

我还为每个 ORM 创建了一个 repo，其中包含了最基本的示例代码，因此如果您没有使用过它们，您将更容易有一个直接的印象。你可以在这里找到源代码[。](https://github.com/sunnyy02/nestjs-orms)

## 使用 [LogRocket](https://lp.logrocket.com/blg/signup) 消除传统错误报告的干扰

[![LogRocket Dashboard Free Trial Banner](img/d6f5a5dd739296c1dd7aab3d5e77eeb9.png)](https://lp.logrocket.com/blg/signup)

[LogRocket](https://lp.logrocket.com/blg/signup) 是一个数字体验分析解决方案，它可以保护您免受数百个假阳性错误警报的影响，只针对几个真正重要的项目。LogRocket 会告诉您应用程序中实际影响用户的最具影响力的 bug 和 UX 问题。

然后，使用具有深层技术遥测的会话重放来确切地查看用户看到了什么以及是什么导致了问题，就像你在他们身后看一样。

LogRocket 自动聚合客户端错误、JS 异常、前端性能指标和用户交互。然后 LogRocket 使用机器学习来告诉你哪些问题正在影响大多数用户，并提供你需要修复它的上下文。

关注重要的 bug—[今天就试试 LogRocket】。](https://lp.logrocket.com/blg/signup-issue-free)