# SpringData MongoDB

## 1.Lea

Spring Data使用Spring框架的核心功能，包括：

IOC容器

类型转换系统

表达语言

JMX集成

DAO异常层次结构

MongoDB 支持的核心功能可以直接使用，无需调用Spring Contatiner的IoC服务。类似于JdbcTemplate，可以在没有Spring 容器的其他任务服务的情况下独立使用。

## 2.学习NoSQl和文档数据库

NoSQL存储席卷了存储世界。是一个广阔的领域，有大量的解决方案、术语和模式。有些原则是通用的

## 3.要求

Spring Data MongoDb3.x二进制文件需要JDK级别8.0 及更高版本和SpringFramework5.3.10及更高版本。

文档存储方面，至少需要MongoDB 3.6、

### 3.1 兼容性矩阵

总结了Spring Data版本到MongoDB驱动程序/数据库版本。数据库版本显示通过Spring Data测试套件的最高支持服务器版本。

| Spring Data Release Train | Spring Data MongoDB | Driver Version                   | Server Version |
| :------------------------ | :------------------ | :------------------------------- | :------------- |
| 2021.0                    | `3.2.x`             | `4.1.x`                          | `4.4.x`        |
| 2020.0                    | `3.1.x`             | `4.1.x`                          | `4.4.x`        |
| Neumann                   | `3.0.x`             | `4.0.x`                          | `4.4.x`        |
| Moore                     | `2.2.x`             | `3.11.x/Reactive Streams 1.12.x` | `4.2.x`        |
| Lovelace                  | `2.1.x`             | `3.8.x/Reactive Streams 1.9.x`   | `4.0.x`        |

### 3.1.1 MongoDB4.4 中相关的变化

当不存在 `$text`条件时，字段列表不得包含文本搜索分数属性。

运行 map reduce 时，Sort不能为空文档/

### 3.1.2 MongoDB 4.2 中的变化

Remove of `geoNear`command.

删除eval命令。

## 4.其他帮助

尝试提供我们认为易于遵循的Spring Data MongoDBmm模块入门指南。

## 5.后续开发

有关Spring Data Mongo源代码存储库、夜间构建和快照工件的信息，参考主页。

博客 Twitter

## 6. New & Noteworthy

### 6.1Spring Data  MongoDB 3.2的新功能

支持将嵌套对象解包到父对象中

支持表达式来定义场投影

### 6.2 Spring Data MongoDB 3.1 的新功能

无审计启用了通过@EnableReactiveMongoAuditing @EnableMongoAuditing 不在注册ReactiveAuditingEntityCallback。

Reactive SpEL支持@Query 和 @Aggregation query方法

聚合提示通过AggregationOptions.builder().hint(bson).build().

kProperty.asPath()将属性引用呈现为属性路径表示的扩展函数

服务器端JavaScript聚合表达式$function()和$accmulator通过ScriptOperators

### 6.3 Spring Data MongoDB3.0 的新功能

升级到MongoDB 驱动程序4.0

现在默认禁用自动索引创建

支持更新操作中的聚合管道

_id 使用MongoTemplate聚合时去除复合ID的扁平化

使用GridFS时应用分页find(Query)

分页密钥派生通过@Sharded

$merge 和 $addFields聚合管道阶段

@TextScore是null通过非全文检查检索查询实体

### 6.4 Spring Data MongoDB 2.2 中的新功能

与MongoDB的 4.2的兼容性 eval，group以及geoNear模板API方法

对MongoDB 3.4和MongoDB 4.0运算符的扩展SpEL聚合支持

Querydsl支持通过  ReactiveQuerydslPredicateExecutor

反应 GridFS 支持

通过存储库查询方法的 聚合框架 支持

使用  @Transactional 的声明式反应式事务

实体模板 API 删除会考虑删除查询中的版本属性。

OptimisticLockingFailureException 当版本化实体无法删除时，存储库删除现在会抛出

支持 Range<T> 在查询之间的存储库中

改变了 Reactive/MongoOperations#count 现在通过将偏移量和限制传递给服务器来限制范围以计算匹配项的行为

在Update操作中支持数组过滤器

从域类型生成JSON模式

SpEL支持 @Indexed

支持散列索引

通过@Document和支持基于注释的排序规则 @Query

Kotlin的类型安全查询

kClass现在不推荐使用Kotlin扩展方法来支持reifield方法

Kotlin协程支持

### 6.5 Spring Data MongoDB 2.1 版本中的新功能

基于游标的聚合

对命令式和反应式模板API的不同查询

通过反应式模板API支持Map/Reduce

validator支持

$jsonSchema支持查询和集合创建

Change Stream支持命令式和反应式驱动程序

命令式驱动程序的可尾游标

MongoDB 3.6会话支持命令式和反应式模板API

MongoDB 4.0事务支持和MongoDB 特定的事务管理器实现

默认排序规范库查询方法使用 @Query(sort=.)

findAndReplace通过命令式和反应式模板API提供支持

弃用dropDups的@Indexed和@CompoundIndex作为MongoDB 的服务器3.0和更新不支持dropDups

### 6.6 Spring Data MongoDB 2.0版本的新功能

升级到 Java8

使用Document API，而不是DBObject

反应式MongoDB 支持

Tailable Cursor查询

使用Java 8支持聚合结果流Stream

使用CRUD和聚合操作的Fluent Collention API

模板和聚合API的Kotlin扩展

用于集合和索引创建和查询操作的排序规则的集成

没有类型匹配的按示例查询支持

支持隔离Update操作

通过使用Spring @NonNullAPI和@Nullable注解为空安全提供工具支持

弃用了跨域支持并删除了Log4j附加程序

### 6.7 Spring Data MongoDB 1.10 中的新功能

与MongoDB Server 3.4和MongoDB Java Driver 3.4 兼容

对于新的注解@CountQuery，@DeleteQuery 和@ExistsQuery

对MongoDB 3.2和MongoDB 3.4聚合运算符的扩展支持

创建索引时支持部分过滤器表达式

在加载或转换 DBRef实例时发布生命周期事件

为Query By Example添加了任意匹配模式

支持 $caseSensitive 和 $diacriticSensitive 文本搜索

支持带空的 GeoJSon多边形

通过批量获取 DBRef实例提高性能

采取多方位的聚集 $feact,$bucket 及 $bucketAuto 用Aggregation

### 6.8 Spring Data MongoDB 1.9的新功能

以下注解已启用 @Document @Id @Field @Indexed @CompoundIndex @GeoSpatialIndexed @TextIndexed

@Query @Meta

支持存储库查询方法中的投影

支持Query by Example

对java.util.Current对象映射的开即用支持

支持 MongoDB 2.6 中引入的批量操作

升级到到Querydsl 4

断言与MongoDB 3.0和MongoDB Java Driver 3.2的兼容性

### 6.9 Spring Data MongoDB 1.8中的新功能

Criteria 提供创建 $geoIntersects

Support for [SpEL expressions](https://docs.spring.io/spring/docs/5.3.10/spring-framework-reference/core.html#expressions) in `@Query`.

MongoDBMappingEvents 公开发布它们的集合名称

改进了 `<mongo:mongo-client credentials="..."/>`

改进了索引创建失败错误消息

### 6.10 Spring Data MongoDB 1.7新功能

 与MongoDB 3.0 和 MongoDB Java Driver 3-beta3的兼容性

支持 JSR-310和ThreeTen后向端口日期/时间类型

允许 Stream 作为查询你方法返回类型

GeoJSON 支持域类型和查询

`QueryDslPredicateExcecutor`现在支持 `findAll(OrderSpecifier<?>... orders)`

支持使用  [Script Operations](https://docs.spring.io/spring-data/mongodb/docs/3.2.5/reference/html/#mongo.server-side-scripts)调用JavaScript函数

改进对 CONTAINS 类似集合属性的关键字的支持

支持 $bit $mul 和 $posotion运营商 Update

## 7.从2.x 升级到 3.x

使用 4.0驱动程序时：

1.IndexOperatorations.resetIndexCache() 不再支持

2.Any MapReduceOptions.extraOption 被忽略

3.WriteResult 不在保存错误信息，而是抛出一个Exception

4.MongoOperator.executeInsession() 不再调用requestStart和requestDone

5.索引名称生成已成为驱动程序内部操作。Spring Data MongoDB仍然使用2.x 模式来生成名称

6.某些Exception消息在第二代和第三代服务器之间以及MMap.v1 和 WiredTiger存储引擎之间有所不同

### 7.1 依赖变化

必需：org.mongodb:mongodb-driver-core

可选：org.mongodb:mongodb-driver-sync

可选：org.mongodb:mongodb-driver-reactivestreams

### 7.2 Java配置

Java API更改

| Type                                                         | Comment                                                      |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| `MongoClientFactoryBean`                                     | Creates `com.mongodb.client.MongoClient` instead of `com.mongodb.MongoClient` Uses `MongoClientSettings` instead of `MongoClientOptions`. |
| `MongoDataIntegrityViolationException`                       | Uses `WriteConcernResult` instead of `WriteResult`.          |
| `BulkOperationException`                                     | Uses `MongoBulkWriteException` and `com.mongodb.bulk.BulkWriteError` instead of `BulkWriteException` and `com.mongodb.BulkWriteError` |
| `ReactiveMongoClientFactoryBean`                             | Uses `com.mongodb.MongoClientSettings` instead of `com.mongodb.async.client.MongoClientSettings` |
| `ReactiveMongoClientSettingsFactoryBean`                     | Now produces `com.mongodb.MongoClientSettings` instead of `com.mongodb.async.client.MongoClientSettings` |
| `AbstractMongoClientConfiguration`, `AbstractReactiveMongoConfiguration` | Configuration methods use parameter injection instead of calling local methods to avoid the need for cglib proxies |

删除的 Java APi

| 2.x                                | Replacement in 3.x                                       | Comment                                                      |
| :--------------------------------- | :------------------------------------------------------- | :----------------------------------------------------------- |
| `MongoClientOptionsFactoryBean`    | `MongoClientSettingsFactoryBean`                         | Creating a `com.mongodb.MongoClientSettings`.                |
| `AbstractMongoConfiguration`       | `AbstractMongoClientConfiguration` (Available since 2.1) | Using `com.mongodb.client.MongoClient`.                      |
| `MongoDbFactory#getLegacyDb()`     | -                                                        | -                                                            |
| `SimpleMongoDbFactory`             | `SimpleMongoClientDbFactory` (Available since 2.1)       |                                                              |
| `MapReduceOptions#getOutputType()` | `MapReduceOptions#getMapReduceAction()`                  | Returns `MapReduceAction` instead of `MapReduceCommand.OutputType`. |
| `Meta|Query` maxScan & snapshot    |                                                          |                                                              |

### 7.3 XML命名空间

更改的XML命名空间元素和属性

| Element / Attribute                      | 2.x                                                          | 3.x                                                          |
| :--------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `<mongo:mongo-client />`                 | Used to create a `com.mongodb.MongoClient`                   | Now exposes a `com.mongodb.client.MongoClient`               |
| `<mongo:mongo-client replica-set="…" />` | Was a comma delimited list of replica set members (host/port) | Now defines the replica set name. Use `<mongo:client-settings cluster-hosts="…" />` instead |
| `<mongo:db-factory writeConcern="…" />`  | NONE, NORMAL, SAFE, FSYNC_SAFE, REPLICAS_SAFE, MAJORITY      | W1, W2, W3, UNAKNOWLEDGED, AKNOWLEDGED, JOURNALED, MAJORITY  |

删除的XML命名空间元素和属性

| Element / Attribute                      | Replacement in 3.x                          | Comment                                            |
| :--------------------------------------- | :------------------------------------------ | :------------------------------------------------- |
| `<mongo:db-factory mongo-ref="…" />`     | `<mongo:db-factory mongo-client-ref="…" />` | Referencing a `com.mongodb.client.MongoClient`.    |
| `<mongo:mongo-client credentials="…" />` | `<mongo:mongo-client credential="…" />`     | Single authentication data instead of list.        |
| `<mongo:client-options />`               | `<mongo:client-settings />`                 | See `com.mongodb.MongoClientSettings` for details. |

新的XML命名空间元素和属性

| Element                                        | Comment                                                  |
| :--------------------------------------------- | :------------------------------------------------------- |
| `<mongo:db-factory mongo-client-ref="…" />`    | Replacement for `<mongo:db-factory mongo-ref="…" />`     |
| `<mongo:db-factory connection-string="…" />`   | Replacement for `uri` and `client-uri`.                  |
| `<mongo:mongo-client connection-string="…" />` | Replacement for `uri` and `client-uri`.                  |
| `<mongo:client-settings />`                    | Namespace element for `com.mongodb.MongoClientSettings`. |

弃用

|                              |                                    |         |
| :--------------------------- | :--------------------------------- | :------ |
| 2.x                          | Replacement in 3.x                 | Comment |
| `MongoDbFactorySupport`      | `MongoDatabaseFactorySupport`      |         |
| `SimpleMongoClientDbFactory` | `SimpleMongoClientDatabaseFactory` |         |
| `MongoDbFactory`             | `MongoDatabaseFactory`             |         |

### 7.4 其他变化

#### 自动索引创建

基于注释的索引创建默认关闭，需要启用。

示例：启用自动索引创建

XML命名

```java
<mongo:mapping-converter auto-index-creation="true"/>
```

Java配置

```java
@Configuration
public class Config extends AbstractMongoClientConfiguration {
    @Override
    protected boolean autoIndexCreation(){
        return true;
    }
}
```

程序化

```java
 MongoDatabaseFactory dbFactory = new SimpleMongoClientDatabaseFactory(???);
        DefaultDbRefResolver defaultDbRefResolver = new DefaultDbRefResolver(dbFactory);
        MongoMappingContext mongoMappingContext = new MongoMappingContext();
        mongoMappingContext.setAutoIndexCreation(true);
        mongoMappingContext.afterPropertiesSet();
        MongoTemplate template = new MongoTemplate(dbFactory,new MappingMongoConverter(defaultDbRefResolver,mongoMappingContext));
```

使用XML命名空间属性 auto-index-creation 上mapping-converter

autoIndexCreation通过AbstractMongoClientConfiguration或覆盖AbstractReactiveMongoClientConfiguration

将标志设置为MongoMappingContext

#### UUID类型

MongoDB UUID表示现在可以配置为不同的格式 。这必须通过MongoClientSettings如下

示例：UUid编解码器配置

```java
@Configuration
public class Config extends AbstractMongoClientConfiguration {
    @Override
    public void configureClientSettings(MongoClientSettings.Builder builder){
        builder.uuidRepresentation(UuidRepresentation.STANDARD);
    }
}
```

延迟MongoDatabase 查找 ReactiveMongoDatabaseFactory

ReactveMongoDatabaseFactory 现在返回 Mono<MongoDatabase> 而不是MongoDataabase允许访问反应器上下文以启用特定于上下文的路由功能

会影响ReativeMongoTeplate.getMongoDatabase()，ReactiveMongoTemplate.getCollection() 两种方法必须遵循延迟检索

## 8 依赖

由于各个Spring Data模块的启动日期不同，它们中的大多数带有不同的主要和次要版本号，找到兼容版本的最简单方法是依赖我们随定义的兼容版本一起提供的Spring Data Release Train BOM

示例 使用Spring Data BOM

```
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.data</groupId>
            <artifactId>spring-data-bom</artifactId>
            <version>2021.0.5</version>
            <scope>import</scope>
            <type>pom</type>
        </dependency>
    </dependencies>
</dependencyManagement>
```

当前版本号2021.0.5，modifier可以是一下之一

SNAPSHOT：当前快照

M1，M2等等：里程碑

RC1，RC2，等等：发布候选

示例：声明对Spring Data模块的 依赖

```
<dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-jpa</artifactId>
</dependency>
```

### 8.1 使用Spring Boot进行依赖管理

Spring Boot会选择最新版本的Spring Data模块

### 8.2 框架

当前版本的 Spring Data模块需要 Spring Framework 5.3.10或更高版本。

## 9.使用Spring DataRepositories

Spring Data repositories抽象的目标是显著减少为各种持久性存储实现数据访问层所需的样板代码量

**本章介绍了Spring Data存储库的核心概念和接口。信息来自Spring Data Commons模块**

**使用Java Persistence API(JPA) 模块的配置和代码示例。应该将XML命名空间声明和要扩展的类型调整为使用的特定模块的等效项。**

**“命名空间参考”涵盖了所有支持存储库APi的Spring Data模块都支持的XML配置。**

**“存储库查询关键字”涵盖了存储库抽象支持的查询方法关键字。**

### 9.1 核心概念

Spring Data 存储库抽象中的中心接口时Repository 他需要域类来管理以及域类的ID类型作为类型参数。

此接口主要用作标记接口，以捕获要使用的类型并帮助发现扩展此接口的接口。

该 CrudRepository 接口正在管理的实体类提供复杂的CRUD功能。

示例：CrudRepository接口

```java
public interface CrudRepository<T,ID> extends Repository<T,ID>{
    <S extends T> S save(S entity);
    Optional<T> findById(ID primaryKey);
    Iterable<T> findAll();
    long count();
    void delete(T entity);
    boolean existsById(ID primaryKey);
}
```

保存给定的实体

返回由给定ID标识 的实体

返回所有实体

返回实体的数量

删除给定的实体

指示具有给定ID的实体是否存在

**还提供特定于持久性技术的抽象，如JpaRepository或MongoRepository。这些接口扩展CrudRepository。**

示例：PaginAndSortingRepository界面

```java
public interface PagingAndSortingRepository<T,ID> extends CrudRepository<T,ID>{
    Iterable<T> findAll(Sort sort);
    Page<T> findAll(Pageable pageable);
}
```

要访问User 页面大小为20的第二页，执行以下操作

```java
PagingAndSortingRepository<User,Long> repository = //..
    Page<User> users = repository.findAll(PageRequest.of(1,20));
```

除了车讯方法，计数和删除查询的查询派生也是可用的，如下:

示例：派生计数查询

```java
interface USerRepository extends CrudRepository<User,Long>{
    long countByLastname(String lastname);
}
```

以下显示了派生删除查询的接口定义

示例：派生删除查询

```java
interface USerRepository extends CrudRepository<User,Long>{
    long deleteByLastname(String lastname);
    List<User> removeByLastname(String lastname);
}
```

### 9.2 查询方法

标准CRUD功能存储库通常对底层数据存储进行查询。使用Sping Data，声明这些过程变成了一个四步过程

1.声明也给扩展Repository或其子接口之一的接口，并将其键入它应该处理的域类和Id类型，如下

```java
interface PersonRepository extends Repository<Person,Long>{
   ..
}
```

2.在接口上声明查询 方法

```java
interface PersonRepository extends Repository<Person,Long>{
    List<Person> findByLastname(String lastname);
}
```

3.设置Spring以使用JavaConfig或XML配置为这些接口创建代理实例

a.要使用Java配置，请创建一个类似于以下的内容

```
@EnableJpaRepositories
public class Config {
}
```

b.使用XML配置，定义如下内容bean

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xmlns:jpa="http://www.springframework.org/schema/data/jpa"
   xsi:schemaLocation="http://www.springframework.org/schema/beans
     https://www.springframework.org/schema/beans/spring-beans.xsd
     http://www.springframework.org/schema/data/jpa
     https://www.springframework.org/schema/data/jpa/spring-jpa.xsd">

   <jpa:repositories base-package="com.acme.repositories"/>

</beans>
```

本例中使用了JPA命名空间，若对任何其他使用存储库抽象，则需要将其更改为store 模块的适当命名空间。应该交换jpa支持，例如mongodb

另注意，JavaConfig变体并未显示配置包，因为默认情况下使用带注释的类的包，要定义要扫描的包，使用basePackage..特定于数据存储的存储库 @Enable${store}Repositroies 注释的属性之一

4.注入存储库实例并使用它，如下示例：

```
public class SomeClient {
    private final PersonRepository repository;
    
    SomeClient(PersonRepository repository){
        this.repository  = repository;
    }
    
    void doSomething(){
        List<Person> persons = repository.findByLastname("Matthews");
    }
}
```

### 9.3 （详解）定义存储库接口

定义存储库接口，首先需要定义特定于域类的存储库接口，接口必须扩展Repository并键入域类和ID类型。若要公开该域类型的CRUD方法，使用扩展CrudRepository而不是Repository。

#### 微调存储库定义

通常情况，接口扩展Repository，CrudRepository或PangingAndSortingRepository。或者，若不想扩展Sping Data接口，可以使用@RepositoryDefinition。扩展CrudRepository公开了一整套完整的方法来操作实体。

**这样可以在提供的Spring Data Respotories功能之上定义自己的抽象。**

示例：有选择性的公开CRUD方法

```java
@NoRepositoryBean
interface MybaseRepository<T,ID> extends Repository<T,ID>{
    Optional<T> findById(ID id);
    <S extends T> S save(S enetity);
}

interface UserRepository extends MyBaseRepository<User,Long>{
    User findByEmailAddress(EmailAddress emailaddress);
}
```

前面例子，定义为所有站点库一个共同的基础界面和暴露findById()以及save()，这些方法被发送刀基础信息库实现自己选择的由Spring提供的数据。它们匹配中的方法签名CurdRepostiroy。UserRepository可以保存用户，通过ID查找单个用户，触发查询以users通过电子邮件地址查找。

**中间存储库接口用@NoRepositoryBean，确保该注释添加到Spring Data不应在运行时为其创建实例的所有存储库接口。**

#### 使用具有多个Spring数据模块的存储库

在应用程序中使用唯一的Spring Data模块使变得简单。因为定义范围内的所有存储库接口都绑定到Spring Data模块。有时需要多个Spring  Data模块则存储库必须区分持久性技术。当在类路径上检测到多个存储库factory时，Spring Data进入严格的存储库配置模式。严格配置使用存储库或域类的详细信息来决定存储库定义的Spring Data模块绑定：

如果存储库定义扩展了特定于模块的存储库，则它时特定Spring Data模块的有效候选者

如果域类使用特定于模块的类型注释进行注释，则它是特定Spring Data模块的有效候选者。

Spring Data模块接受第三方注解(JPA @Enity)或提供自己的注解(@Document Soring Data MongoDB 和Spring Data Elasticsearch)

示例：使用模块特定接口的存储库定义

```java
interface MyRepository extends JpaRepository<User,Long>{
    @NoRepository
    interface MyBaseREpository<T,ID> extends JpaREpository<T,ID>{}
    interface USerRepository extends MyBaseRepository<T,ID>{}
}
```

示例：使用通过接口的存储库定义

```java
interface AmbiguousRepository extends Repository<User,Long>{
    
}
@NoRepository
interface MyBaseRepository<T,ID> extends CrudRepository<T,ID>{}

interface AmbiguousUserRepository extends MyBaseRepository<User,Long>{}
```

AmbiguousRepository 和 AmbiguousUserRepository  。CrudRepository在它们的类型层次。

虽然这在使用唯一的Spring Data模块时很好，但多个模块无法区分这些存储库应该绑定哪个特定的Spring Data

示例：使用带注释的域类的存储库定义

```java
interface PersonRepository extends Repository<Person,Long>{}

@Entity
class Person{}
interface UserRepository extends Repository<User,Long>{}
@Document
class User{}
```

PersonRepository继承Person，它用JPA @Entity注释进行注释，这个存储库属于Spring Data JPA。

UserRepository继承User，使用Spring Data MongoDB的@Document注解进行注解

示例：使用带有混合注释的域类的存储库定义

```java
interface JpaPersonRepository extends Repository<Person,Long>{}

interface MongoDBPersonRepository extends Repository<Person,Long>{}
@Entity
@Document
class Person{}
```

此示例显示使用JPA和Spring Data MongoDB注释的域类，定义了两个存储库，JpaRepository以及MongoDBPersonRepository。一个用于JPA，另一个用于MongoDB，Spring DAta不再能够区分存储库，会导致未定义的行为。

存储库类型详细信息和区分域类注释用于严格的存储库配置，以识别特定Spring Data模块的存储库候选者。在同一域类型上使用多个特定于持久性技术的注释是可能的，并且允许跨多个持久性计数重用域类型。但是，Spring Data无法再确定与存储库绑定的唯一模块。

区分存储库的最后一种方法是确定存储库基础包的范围，基础包定义了扫描存储库接口定义的地点，意味着存储库定义位于适当的包中。默认情况下，注解驱动的配置使用配置类的包，基于XML的配置中的基本包是必需的。

示例：基础包的注解驱动配置

```java
@EnableJpaRepository(basePackages="com.acme.repositories.jpa")
@EnableMongoRepositories(basePackages="com.acme.repositories.mongo")
class Configuration{}
```

### 9.4 定义查询方法

存储库代理有两种方法可以从方法名称派生特定于stop的查询：

通过直接从方法名称派生查询

通过使用手动定义的查询









## 10 介绍

介绍了 Spring Data MongoDB提供的核心功能

MongoDB支持   介绍了MongoDB模块功能集

MongoDB Repositories 介绍了对MongoDB的存储库支持

## 11 MongoDB支持

MongoDB支持包含广泛的功能：

Spring配置支持使用基于Java的@Configuration类或用于Mongo驱动程序实例和副本集的XML命名空间

MongoTemplate在执行常见的Mongo操作时提高生产力的助手类，包括文档和POJO之间的集成对象映射

异常转换为Spring的可移植数据访问异常层次结构

功能丰富的对象映射与Spring的转换服务集成

基于注释的映射元数据可扩展以支持其他元数据格式

持久性和映射生命周期时间

基于Java的查询，标准和更新DSL

存储库接口的自动实现，包括对自定义查找器方法的支持

QueryDSL集成以支持类型安全查询

对JPA实体的跨存储持久性支持，其字段透明的持久化并使用MongoDB检索

地理空间整合

大多数应该使用MongoTemplate或Repository支持。都利用了丰富的映射功能。MongoTemplate是寻找访问功能的地方，例如递增递增计数器或临时CRUD事件。MongoTemplate还提供了回调方法，以便可以轻松获取低级API工件，例如com.mongodb.client.MongoDatabase直接与MongoDB通信。各种API工件的命名约定的目标是复制基础MongoDB Java驱动程序中的命名约定，以便可以轻松将现有知识映射到Spring  APi

### 11.1入门

引导设置工作环境的一种简单方法是STS中创建一个基于Spring项目

首先需要设置一个正在运行的MongoDB服务器。安装后，启动MongoDB通常只需要运行以下命令：${MONGO_HOME}/bin/mongod

创建Spring项目

1.创建包

2.添加到dependencies元素中：

```java
<dependencies>

  <!-- other dependency elements omitted -->

  <dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-mongodb</artifactId>
    <version>3.2.5</version>
  </dependency>

</dependencies>
```

3.将pom.xml中spring版本改为

```java
<spring.framework.version>5.3.10</spring.framework.version>
```

4.将Maven的Spring Milestone存储库的以下位置添加到pom.xml的<dependencies/>元素中，

```java
<repositories>
  <repository>
    <id>spring-milestone</id>
    <name>Spring Maven MILESTONE Repository</name>
    <url>https://repo.spring.io/libs-milestone</url>
  </repository>
</repositories>
```

Person

```
public class Person {
    private String id;
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "id='" + id + '\'' +
                ", name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

主函数

```
public class MongoApp {
    private static final Log log = LogFactory.getLog(MongoApp.class);

    public static void main(String[] args) throws Exception{
        MongoOperations mongoOps = new MongoTemplate(MongoClients.create(),"database");
        mongoOps.insert(new Person("Joe",34));

        log.info(mongoOps.findOne(Query.query(where("name").is("Joe")),Person.class));

        mongoOps.dropCollection("person");
    }
}
```

可以MongoTemplate通过使用标准com.mongodb.client.MongoClient对象和要使用的数据库名称来实例化Spring Mongo的中央帮助器类

映射器针对标准POJO对象工作，无需任何额外的元数据

Conventions用于处理id字段，将其转换为ObjectId存储在数据库中的时间

映射约定可以使用字段访问，Person类只有getter

如果构造函数参数名称与存储文档的字段名称匹配，则用于实例化对象

### 11.2 示例库

包含多个示例的GitHub存储库

### 11.3 使用Spring连接到MongoDB

使用MongoDB和Spring时的首要任务之一时com.mongodb.client.MongoClient使用IoC容器创建对象，有两种主要方法能做到这一点，一种是使用基于Java的bean元数据，另一种是使用基于XML的bean元数据。

对于那些不熟悉如何配置使用基于java  bean的元数据，而不是基于XML的元数据Spring容器。

#### 使用基于Java的元数据注册Mongo示例

示例：com.mongodb.client.MongoClient使用基于Java的bean元数据注册对象

```
@Configuration
public class AppConfig {
    
    public @Bean MongoClient mongoClient(){
        return MongoClients.create("mongodb://localhost:27017");
    }
}
```

这种方法允许使用标准com.mongodb.client.MongoClient实例。容器使用Spring的MongoClientFactoryBean。与com.mongodb.client.MongoClient直接实例化实例相比，Factory具有额外的优势，即为容器提供了一个ExceptionTranslator实现，该实现将MongoDB异常转换为Spring的可移植DataAccessException层次结构中的异常，用于使用注释注释的数据访问类@Repository

示例：com.mongodb.client.MongoClient使用Spring注册对象MongoClientFactoryBean并启用Spring的异常转换支持

```
@Configuration
public class AppConfig {
 
    public @Bean MongoClientFactoryBean mongo(){
        MongoClientFactoryBean mongo = new MongoClientFactoryBean();
        mongo.setHost("localhost");
        return mongo;
    }
}
```

要访问com.mongodb.client.MongoClient由MongoClientFactoryBean或@Configuration其他类或自己类创建的对象，使用private @Autowired MongoClient mongoClient; 字段

#### 使用基于XML的元数据注册Mongo实例

示例：配置MongoDB的XML模式

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:mongo="http://www.springframework.org/schema/data/mongo"
          xsi:schemaLocation=
          "
          http://www.springframework.org/schema/data/mongo https://www.springframework.org/schema/data/mongo/spring-mongo.xsd
          http://www.springframework.org/schema/beans
          https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- Default bean name is 'mongo' -->
    <mongo:mongo-client host="localhost" port="27017"/>

</beans>
```

示例：使用XML模式设置com.mongodb.client.MongoClient对象MongoClientSettings

示例：com.mongodb.client.MongoClient使用副本集配置对象的XML模式

```java
        <mongo:mongo-client id="replicaSetMongo" replica-set="rs0">
            <mongo:client-settings cluster-hosts="127.0.0.1:27017,localhost:27018"/>
        </mongo:mongo-client>
```

#### MongoDatabaseFactory接口

com.mongodb.client.MongoClient是MongoDB驱动程序API的入口点，但连接到特定的MongoDB数据库实例需要其他信息，例如数据库名称和用户名 密码。使用该信息，可以获得一个com.mongodb.client.MongoDatabase对象并访问特定MongoDB数据库实例的所有功能。Spring提供了org.framework.data.mongodb.core.MongoDatabaseFactory如下所示的接口来引导连接到数据库

可以使用MongoDatabaseFactory实例来配置MongoTemplate

在标准Java中使用，而不是使用IoC容器来创建MongoTemplate实例

```java
public class MongoApp {

  private static final Log log = LogFactory.getLog(MongoApp.class);

  public static void main(String[] args) throws Exception {

    MongoOperations mongoOps = new MongoTemplate(new SimpleMongoClientDatabaseFactory(MongoClients.create(), "database"));

    mongoOps.insert(new Person("Joe", 34));

    log.info(mongoOps.findOne(new Query(where("name").is("Joe")), Person.class));

    mongoOps.dropCollection("person");
  }
}
```

SimpleMongoClientDbFactory在选择com.mongodb.client.MongoClient作为选择的入口点时使用

#### MongoDatabaseFactory使用基于Java的元数据注册实例

要向MongoDatabaseFactory容器注册实例，编写的代码与前面代码中突出显示的非常相似。

```java
@Configuration
public class MongoConfiguration {

  public @Bean MongoDatabaseFactory mongoDatabaseFactory() {
    return new SimpleMongoClientDatabaseFactory(MongoClients.create(), "database");
  }
}
```

MongoDB服务第三代连接到数据库时更改了身份验证模型

应该使用MongoClient-specific选项来设置MongoCredential提供身份验证数据

示例：

```
public class ApplicationContextEventTestsAppConfig extends AbstractMongoClientConfiguration {
    @Override
    protected void configureClientSettings(MongoClientSettings.Builder builder) {
        builder
                .credential(MongoCredential.createCredential("name","db","pwd".toCharArray()))
                .applyToClusterSettings(settings ->{
                    settings.hosts(Collections.singletonList(new ServerAddress("127.0.0.1",27017)));
                });
    }

    @Override
    protected String getDatabaseName() {
        return "database";
    }
}
```

验证身份 xml--credential上的<mongo-client>

在基于XML的配置中使用的用户名和密码凭证必须URL编码时这些包含保留的字符，如（： % @ ，）

以下示例显示了编码凭据： `m0ng0@dmin:mo_res:bw6},Qsdxx@admin@database`→`m0ng0%40dmin:mo_res%3Abw6%7D%2CQsdxx%40admin@database`

MongoDatabaseFactory使用基于XML的元数据注册实例

mongo命名空间提供了一个方便的方法来创建一个SimpleMongoClientDbFactory，相比于使用<beans/>名称空间，如下例子

```java
<mongo:db-factory dbname="database">
```

若需要在com.mongodb.client.MongoClient用于创建的实例上配置其他选项SimpleMongoClientDbDactory，可以使用mongo-ref以下示例中所示的属性来引用现有bean。为了显示另一种常见的使用模式，以下显示了占位符的使用，允许参数化配置和创建MongoTemplate：

```java
<context:property-placeholder location="classpath:/com/myapp/mongodb/config/mongo.properties"/>

<mongo:mongo-client host="${mongo.host}" port="${mongo.port}">
  <mongo:client-settings connection-pool-max-connection-life-time="${mongo.pool-max-life-time}"
    connection-pool-min-size="${mongo.pool-min-size}"
    connection-pool-max-size="${mongo.pool-max-size}"
	connection-pool-maintenance-frequency="10"
	connection-pool-maintenance-initial-delay="11"
	connection-pool-max-connection-idle-time="30"
	connection-pool-max-wait-time="15" />
</mongo:mongo-client>

<mongo:db-factory dbname="database" mongo-ref="mongoClient"/>

<bean id="anotherMongoTemplate" class="org.springframework.data.mongodb.core.MongoTemplate">
  <constructor-arg name="mongoDbFactory" ref="mongoDbFactory"/>
</bean>
```

### 11.4 简介MongoTemplate

是中央级的Spring的MongoDB支持，提供了与数据库交互的丰富的功能集。

该模板提供了创建/ 更新 /删除和查询 MongoDB文档的便捷操作，并提供了域对象和MongoDB文档之间的映射

配置后，MongoTemplate就是线程安全的，可以跨多个实例重复使用。

MongoDB文档和域类之间的映射是通过委托给MongoConverter接口的实现来完成的。Spring提供了MappingMongoConverter。

引用MongoTemplate实例操作的首选方法是通过其接口MongoOperations

使用的默认转换器实现MongoTemplate是MappingMongoConverter

核心功能 MongoTemplate是将MongoDB Java驱动程序抛出的异常转换为Spring的可移植数据访问异常层次结构

#### 实例化MongoTemplate

示例 注册一个com.mongodb.client.MongoClient对象并启用Spring的异常转换支持

```java
@Configuration
public class AppConfig{
    public @Bean MongoClient mongoClient(){
        return MongoClients.create("mongodb://localhost:27017");
    }
    public @Bean MongoTemplate mongoTemplate(){
        return new MongoTemplate(mongoClient(),"mydatabase");
    }
}
```

重载构造函数MongoTemplate

MongoTemplate(MongoClient mongo,String databaseName):采用MongoClient对象和默认数据库名称进行操作

MongoTemplate(MongoDatabaseFactory mongoDbFactory)采用封装了MongoClient对象，数据库名称、用户名和密码的MongoDbFactory对象

MongoTemplate(MongoDatabaseFactory mongoDbFactory,MongoConverter mongoConverter)：添加一个MongoConverter用于映射

可以使用Spring的XML <bean/>模式配置MongoTemplate

```java
<mongo:mongo-client host="localhost" port="27017"/>
    
<bean id="mongoTemplate" class="org.springframework.data.mongodb.core.MongoTemplate">
    <constructor-arg ref="mongoClient"/>
    <constructor-arg name="databaseName" value="gerspatial"/>
</bean>
```

引用MongoTemplate实例操作的首选方法是通过其接口MongoOperations

#### WirteResultChecking Policy

开发过程中，若com.mongodb.WriteResult任何MongoDB操作返回的包含错误，记录或抛出异常是很方便的。

可以将WriteResultChecking属性设置为MongoTemplate以下值其一：EXCEPTION或NONE，分别用于抛出EXCEPTION或不执行任何操作，默认值是none

#### Writeconcern

若未通过更高级别驱动程序指定，可以设置用于写操作的属性MongoTemplete。

若WriteConcern未设置该属性，则默认为MongoDB驱动程序DB或Collection设置中的设置。

#### WriteconcernResolver

更高级-->希望WriteConcerin再每个操作的基础上设置不同的值（删除 更新 插入 和保存操作），WriteConcernResolver可以在MongoTemplate。由于MongoTemplate用于持久化POJO，因此，WriteConcernResolver   -- 可以创建  一个--  将特定的POJO类映射到一个WriteConcern。

如下WriteConcernResolver界面：

```
public class WriteConcernResolver {
    WriteConcern resolve(MongoAction action);
}
```

可以使用MongoAction参数来确定WriteConcern值或使用模板本身的值作为默认值，MongoAction包含集合名称被写入时，java.lang.class所描述POJO转换后的Document，操作（REMOVE，UPDATE,INSERT,INSERT_LIST,SAVE)和其他一些上下文信息，

示例：显示两组获得不同WriteConcern设置的类：

```
public class MyAppWriteConcernResolver implements WriteConcernResolver {
    @Override
    public WriteConcern resolve(MongoAction action) {
        if(action.getEntityType().getSimpleName().contains("Audit")){
            return WriteConcern.NONE;
        }else if (action.getEntityType().getSimpleName().contains("Metadata")){
            return WriteConcern.JOURNALED;
        }
        return action.getDefaultWriteConcern();
    }
}
```

### 11.5 保存 更新和删除文档

类:

```
public class Person {
    private String id;
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "id='" + id + '\'' +
                ", name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

给定Person示例中的类，可以保存 更新和删除对象

MongoOperations是MongoTemplate实现的接口



```
import com.example.entity.Person;
import com.mongodb.client.MongoClients;
import com.mongodb.client.result.UpdateResult;
import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.data.mongodb.core.MongoOperations;
import org.springframework.data.mongodb.core.MongoTemplate;
import org.springframework.data.mongodb.core.SimpleMongoClientDbFactory;
import org.springframework.data.mongodb.core.query.Query;

import java.util.List;

public class MongoApp {
    private static final Log log = LogFactory.getLog(MongoApp.class);

    public static void main(String[] args) {
        MongoOperations mongoOperations = new MongoTemplate(new SimpleMongoClientDbFactory(MongoClients.create(),"database"));
        Person p = new Person("Joe",34);
        //Insert
        mongoOperations.insert(p);
        log.info("Insert:"+p);
        //find
        p = mongoOperations.findById(p.getId(),p.getClass());
        log.info("Found:"+p);
        //Update
        UpdateResult updateResult = mongoOperations.updateFirst(Query.query(where("name").is("Joe")), update("age", 35), Person.class);
        p = mongoOperations.findOne(Query.query(where("name").is("Joe")),Person.class);
        log.info("Update:"+p);
        //Delete
        mongoOperations.remove(p);
        //check
        List<Person> people = mongoOperations.findAll(Person.class);
        log.info("Number of people = :"+people.size());
        
        mongoOperations.dropCollection(Person.class);


    }
}
```

MongoConverter通过识别属性名称String，导致ObjectId存储在数据库中的a和an之间进行隐式转换Id

前面的示例旨在保存 更新  删除操作的使用   MongoTemplate不是展示复杂的映射功能

#### _id如何在映射层中处理字段

有一个_id包含所有文档的字段。若不提供，驱动程序将分配ObjectId一个生成的值，若使用MappingMongoConverter，某些会控制Java类中的属性映射到 _id

1.用@Id注释的属性或字段映射到_id

2.没有注释但已命名的属性或字段id映射到该_id

 _id在使用MappingMongoConverter时对映射到文档字段的属性进行的类型转换

1.若-------在Java类中id声明为a的属性或字段通过使用Spring  String转换为并存储为an，有效的转换规则给MongoDB Java驱动程序。若无法转换则该值作为字符串存储在数据库中。

ObjectId Conveter<String,ObjectId> ObjectId

2.使用Spring 将在Java类中id声明的属性或字段BigInteger转换为英寸厨卫ObjectId Converter<BigInteger,ObjectId>

若Java中不存在先前规则集中指定的字段或属性_id，则驱动程序会生成一个饮食文件，但不会映射到Java类的属性或字段。

查询和更新时，MongoTemplate使用与上述规则对应的转换器来保存文档，以便查询中使用的字段名称和类型可以匹配域类中的内容

示例：@MongoId映射

```
public class PlainStringId {
    @MongoId String id;//id被视为String没有进一步转换
    @MongoId ObjectId id1;//id被视为ObjectId
    @MongoId(FieldType.OBJECT_ID) String id2;//id被视为ObjectId给定的String的ObjectId十六进制有效，否则视为String，对@Id应用，
    
}
```

#### 类型映射

MongoDB集合可以包含表示各种类型实例的文档。若存储类的层次结构或具有type属性的类，则非常有用。

为实现这点，MapplingMongoConverter使用了一个MongoTypeMapper抽象DefaultMongoTypeMapper作为其主要实现。

示例：类型映射

```
public class Sample {
    Contact value;
}
public abstract class Contact{}
public class Person extends Contact{}
Sample sample = new Sample();
sample.value = new Person();

mongoTemplete.save(sample);
        {
            "value":{"_class":"com.acme.Person"},
        "_class":"com.acme.Sample"
        }
```

Spring  Data  MongoDB将类型信息存储为实际根类以及嵌套类型的最后一个字段。

若现在使用mongoTemplate.findAll(Object.class,"sample),会发现存储的文档是一个Sample实例，还可以发现value属性实际上是一个Person

##### 自定义类型映射

若不想把整个Java类名携程类型信息，而是想用一个key，可以@TypeAlias在实体类上使用注解。若需要自定义映射--TypeInfomationMapper接口

示例：为实体定义类型别名

```
@typeAlias("pers")
class Person{

}
```

生成的文档包含字段中pers的值_class

类型别名仅在映射上下文知道类型时有效。所需的实体元数据在第一次保存时确定，或者必须通过配置初始实体集提供。

```
@Configuration
public class AppConfig extends AbstractMongoClientConfiguration{
	@Override
	protected Set<class<?>> getInitialEntitySet(){
		return Collections.singleton(Person.classs);
	}
	//...
}
```

##### 配置自定义类型映射

说明如何配置自定义MongoTypeMapper的Mapping

示例：MongoTypeMapper使用Spring Java Config配置自定义

```
public class CustomMongoTypeMapper extends DefaultMongoTypeMapper {
    //
}
```

```
public class SampleMongoConfiguration extends AbstractMongoClientConfiguration {
    @Override
    protected String getDatabaseName() {
        return "database";
    }
    

    @Bean
    @Override
    public MappingMongoConverter mappingMongoConverter() throws Exception {
        MappingMongoConverter mmc = super.mappingMongoConverter();
        mmc.setTypeMapper(customTypeMapper());
        return mmc;
    }
    
    @Bean
    public MongoTypeMapper customTypeMapper(){
        return new CustomMongoTypeMapper();
    }
    
}
```

🐖：前面示例扩展了AbstractMongoClientConfiguration类并覆盖了MappingMongoConverter

配置自定义MongoTypeMapper

如何使用XML配置自定义MongoTypeMapper

```
<mongo:mapping-converter type-mapper-ref="customMongoTypeMapper"/>
<bean name="customMongoTypeMapper" class="com.bao.CustomMongoTypeMapper"/>
```

#### 保存和插入文档的方法

几种用于保存和插入对象MongoTemplate。要对转换过程进行更细的控制，可以使用MappingMongoConverter--例如Converter<person,Document>和注册Spring转换器Converter，Person

插入和保存至之间的区别在于，如果对象尚不存在，则保存操作会执行插入操作

使用保存操作的简单情况是保存一个POJO，在这种情况下，集合名称由类的名称确定。还可以使用特定的集合名称调用保存操作。可以使用映射元数据来覆盖存储对象的集合。

插入或保存时，若Id未设置该属性，则假设其将由数据库自动生成。因此，ObjectId要成功

生成an，类中的Id属性或字段类型必须是a String、an ObjectId或a BigInteger

示例：使用MongoTemplate插入和检索文档

```
Person p = new Person("Bob",33);
mongoTemplate.insert(p);

Person qp = mongoTemplate.findOne(query(where("age").is(33)),Person.class);

```

以下插入和保存操作可用：

void save(Object ObjectToSave)  将对象保存到默认集合

void save(Object ObjectToSave,String collectionName)   将对象保存到指定集合

也可以使用一组类似的插入操作

void insert(Object ObjectToSave)    将对象插入到默认集合中

void insert(Object ObjectToSave,String collectionName)   将对象插入到指定的集合中

##### 文档保存集合--> 有两种方法可以管理用于文档的集合名称。

1.@Document 注释提供不同的集合名称对此自定义

2.可以通过提供自己的集合名称作为所选MongoTemplate方法调用的最后一个参数来覆盖集合名称

##### 插入或保存单个对象

MongoDB 驱动程序支持在单个操作中插入文档集合。MongoOperations接口中的以下方法支持此功能

insert：插入一个对象。如果存在具有相同的现有文档，id则会生成错误

insertAll：将一个Collection对象作为第一个参数。

save：保存对象，覆盖任何可能具有相同id

##### 批量插入多个对象

MongoDB 驱动程序支持在一个操作中插入第一组文档，MongoOperations接口中的以下方法支持此功能：

插入方法：用一个Collection作为第一个参数。

#### 更新集合中的文档

更新----> 使用using 更新找到的第一个文档，也可以使用MongoOperation.updateFirst方法更新找到的与查询匹配的所有文档MongoOperation.updateMulti。以下示例显示了SAVINGS 我们使用$inc 运算向余额添加一次性50美元奖金的所有账户更新

示例：使用MongoTemplate

```
import static org.springframework.data.mongodb.core.query.Criteria.where;
import static org.springframework.data.mongodb.core.query.Query;
import static org.springframework.data.mongodb.core.query.Update;

WriteResult wr = mongoTemplate.updateMulti(new Query(where("accounts.accountType"),is(Account.Type.SAVINGS)),
new Update().inc("accounts.$.balance",50.00),Account.class);
```

除了Query前面讨论的之外，还能使用Update对象来提供更新定义，该Update有匹配供MongoDB的更新改进剂的方法。

大多数方法都会返回Update对象，为API提供流畅的样式

##### 运行文档更新的方法

updateFirst：用更新的文档更新与查询文档条件匹配的第一个文档。

updateMulti：使用更新的文档更新与查询文档条件匹配的所有对象

updateFirst不支持订。使用findAndModify申请Sort

##### Updata类中的方法

以下::

Update(String key,Objected value)使用$addToSet更新修饰的addToSet

Update(String key)使用 $currentDate update修饰符更新currentDate

Update currentTimestamp(String key)使用$currentDate更新修饰符更新$type timestamp

Update inc(String key,Number inc)使用$inc更新修饰符更新

Update (String key,Objected max)使用$max更新修饰符的最大更新

Update min(String key,Objected min)使用$min 更新修饰符进行更新

Update (String key,Number multiplier)使用$mul更新修饰符×更新

Update(String key,Update.Position pos)使用$pop更新修饰符弹出更新

Update(String key,Objected value)使用$pull 更新修饰符拉取更新

Update(String key,Object[] values) 使用$pullAll 更新修饰符pullAll更新

Update(String key,Objected value)使用$push更新修饰符推送更新

Update pushAll(String key,Object[] values)使用$pullAll更新修饰符

Update (String oldName,String newName)使用$rename更新修饰符重命名更新

Update(String key,Object value)使用$set更新修饰符设置更新

Update(String key,Object value)使用$setOnInsert更新修饰符setOnInsert更新

Update(String key)使用$unset更新修饰符取消设置更新

一些更新修饰符，如$push  $addToSet 允许嵌套额外的运算符

```
// { $push : { "category" : { "$each" : [ "spring" , "data" ] } } }
new Update().push("category").each("spring", "data")

// { $push : { "key" : { "$position" : 0 , "$each" : [ "Arya" , "Arry" , "Weasel" ] } } }
new Update().push("key").atPosition(Position.FIRST).each(Arrays.asList("Arya", "Arry", "Weasel"));

// { $push : { "key" : { "$slice" : 5 , "$each" : [ "Arya" , "Arry" , "Weasel" ] } } }
new Update().push("key").slice(5).each(Arrays.asList("Arya", "Arry", "Weasel"));

// { $addToSet : { "values" : { "$each" : [ "spring" , "data" , "mongodb" ] } } }
new Update().addToSet("values").each("spring", "data", "mongodb");
```

#### “更新”集合中的文档

与执行updateFirst操作相关，还可以执行"upsert"操作，若找不到与查询匹配的文档，将执行插入操作。插入的文档是查询文档和更新文档的组合，如何使用upsert方法：

```
template.update(Person,class)
.matching(query(where("ssn").is(1111).and("firstName").is("Joe").and("Fraizer").is("Update"))
.apply(update("address",addr))
.upsert();
```

#### 在集合中查找和更新文档

该findAndModify() 对方法MongoCollection 可以更新的文件，并在单个操作中返回上任或更新的文件。MongoTemplate提供了四个findAndModify重载方法，接受Query和Update 类并将from转换Document为POJO

```
<T> T findAndModify(Query query,Update update,Class<T> entityClass);
<T> T findAndModify(Query query,Update update,Class<T> entityClass,String collectionName);
<T> T findAndModify(Query query,Update update,FindAndModifyOptions options,Class<T> entityClass);
<T> T findAndModify(Query query,Update update,FindAndModifyOptions options,Class<T> entityClass);
```

示例：将一些Person对象插入容器并执行findAndUpdate操作

```java
temolate.insert(new Person("Tom",21));
template.insert(new Person("Dick",22));
template.insert(new Person("Harry",23));

Query query = new Query(Criteria.where("firstName").is("Harry"));
Update update = new Update().inc("age",1);

Person oldValue = template.update(Person.class)
    .matching(query)
    .apply(update)
    .findAndModifyValue();

assertThat(oldValue.getFirstName()).isEqualTo("Harry");
assertThat(oldValue.getAge()).isEqualTo(23);

Person newValue = template.query(Person.class)
    .matching(query)
    .findOneValue();

assertThat(newValue.getAge()).isEqualTo(24);

Person newestValue = template.update(Person.class)
    .matching(query).apply(update)
    .withOptions(FindAndModifyOptions.options().returnNew(true))
    .findAndModifyValue();
    
    assertTaht(newestValue.getAge()).isEqualTo(25);
```

FindAndModifyOptions 方法可以让你设置的选项returnNew,upsert以及remove 从前面的代码片段延伸一个例子  ：：

```Java
Person upserted = tempalte.update(Person.class)
    .match(new Query(Criteria.where("firstName").is("Mary")))
    .apply(update)
    .withOptions(FindAndModifyOptions.options().upsert(true).returnNew(true))
    .findAndModifyValue();

assertThat(upserted.getFirstName()).isEqualTo("Mary");
assertThat(upserted.getAge()).isOne();
```

#### 聚合管道更新

更新方法公开MongoOperations 并通过聚合管道 ReactiveMongoOperations 接受聚合管道AggregationUpdate

使用 AggregationUpdate 允许在更新操作中利用MongoDB 4.2 聚合

更新可以包括以下

AggregationUpdate.set().toValue()   --->  $set:{}

AggregationUpdate.unset()  ---->  $unset:[]

AggregationUpdate.replaceWith()  -----> $replaceWith:{}

示例：更新聚合

```java
AggregationUpdate update = Aggreation.newUpdate()
    .set("average").toValue(ArithmeticOperators.ValueOf("tests").avg())
    .set("grade").toValue(ConditionalOperators.swichCases(
    	when(valueOf("average").greaterThanEqualToValue(90)).then("A"),
    	when(valueOf("average").greaterThanEqualToValue(80)).then("B"),
    	when(valueOf("average").greaterThanEqualToValue(70)).then("C"),
    	when(valueOf("average").greaterThanEqualToValue(60)).then("D"))
    	.defaultTo("F")
);

template.update(Student,class)
    .apply(update)
    .all();
```

```java
db.students.update(
    {},
    [
        {$set:{average:{$avg:"$tests"}}},
        {$set:{grade:{$swith:{
            branches:[
                {case:{$gte:["$average",90]},then:"A"},
                {case:{$gte:["$average",80]},then:"B"},
                {case:{$gte:["$average",70]},then:"C"},
                {case:{$gte:["$average",60]},then:"D"}
            ],
            default:"F"
        }}}}
    ],
    {multi:true}
)
```

1.第一$set阶段根据测试字段的平均值计算新的字段平均值

2.第二$set阶段根据第一聚合阶段计算的平均字段计算新的字段等级

3.管道在学生集合上运行并Student用于聚合字段映射

4.将更新应用于集合中的所有匹配文档

#### 查找和替换文档

替换整个的最直接方法Document 是通过它 id save方法，这并不是总是可行的。

findAndReplace提供了一个替代方法，允许通过简单的查询来识别要替换的文档

查找贺替换文档

```java
Optional<User> result = template.update(Person.class)
    .matching(query(where("firstame").is("Tom")))
    .replaceWith(new Person("Dick"))
    .withOptions(FindAndReplaceOptions.options().upsert())
    .as(User.class)
    .findAndReplace();
```

1.使用具有给定域类型的fluent update API来映射查询和派生集合名称，或者仅使用MongoOperations#findAndReplace

2.映射到给定域类型的实际匹配查询。通过查询提供sort，fields 和colleciton设置

3.额外的可选以提供默认值意外的选项，如upsert

4.用于映射运算结果的可选投影类型，如果没有给出初始域类型，则使用。

5.触发实际处理，使用findAndReplaceValue 以获得可空的结果，而不是一个Optional

注意  ---------更换件不得保留其id本身，因为id现有的Document将被转移到更换件中。

还有  findAndReplace只会根据可能给定的排序替换与查询条件匹配的第一个文档

#### 删除文档的方法

可以使用五种重载方法之一从数据库中删除对象

```java
template.remove(tywin,"GOT");
template.remove(query(where("lastname").is("lannister")),"GOT");
template.remove(new Query().limit(3),"GOT");
tempalte.findAllRemove(query(where("lastname").is("lannister"),"GOT"));
template.findAllAmdRemove(new Query().limit(3),"GOT");
```

1._id从关联的集合中删除由他指定的单个实体

2.从GOT集合中删除与查询条件匹配的所有文档

3.删除GOT集合中的前三个文档。不同于<2>,文档，以除去由它们的标识_id,运行给定的查询，应用sort，limit和skip选择，然后再一次一个单独的步骤中除去素有

4.从GOT集合中删除与查询条件匹配的所有文档。与<3>不同的是，文档不会批量删除，而是逐个删除

5.删除GOT集合中的前三个文档，与<3>不同的是，文档不会批量删除，而是逐个删除。

#### Optimistic Locking

该`@Version`注释在 MongoDB 的上下文中提供类似于 JPA 的语法，并确保更新仅应用于具有匹配版本的文档。因此，version 属性的实际值被添加到更新查询中，如果另一个操作同时更改了文档，则更新不会产生任何影响。在这种情况下，`OptimisticLockingFailureException`会抛出an 。

```
@Document
class Person {

  @Id String id;
  String firstname;
  String lastname;
  @Version Long version;
}

Person daenerys = template.insert(new Person("Daenerys"));                            

Person tmp = template.findOne(query(where("id").is(daenerys.getId())), Person.class); 

daenerys.setLastname("Targaryen");
template.save(daenerys);                                                              

template.save(tmp); // throws OptimisticLockingFailureException  
```

| 1    | 最初插入文档。`version`设置为`0`。                           |
| ---- | ------------------------------------------------------------ |
| 2    | 加载刚刚插入的文档。`version`还在`0`。                       |
| 3    | 用 更新文档`version = 0`。将`lastname`和设置`version`为`1`。 |
| 4    | 尝试更新先前加载的文档，但仍有`version = 0`. 操作失败并显示`OptimisticLockingFailureException`，因为当前`version`是`1`。 |

### 11.6 查询文件

可以使用`Query`和`Criteria`类来表达queries. 有反映本地MongoDB的运营商名称方法的名称，如`lt`，`lte`，`is`，和others    Query`和 `Criteria `类遵循流畅API的风格，让你可以连续使用多个方法标准和查询同时具有易于理解的代码。为了提高可读性，静态导入让您避免使用 'new' 关键字来创建`Query`和`Criteria`实例。您还可以使用从纯 JSON 字符串`BasicQuery`创建`Query实例

示例:从纯JSON字符串创建Query实例

```
BasicQuery query = new BasicQuery("{age:{$It:50},accounts.balance:{$gt:1000.00}}");
List<Person> result = mongotemplate.find(query,Person.class);
```

#### 查询集合中的文档

若我们有许多Person带有名称和年龄的对象作为文档存储在一个集合中，并且每个人都有一个带入余额的嵌入式账户文档，

示例：使用MongoTemplate查询文档

```java
import static org.springframework.data.mongodb.core.query.Criteria.where;
import static org.springframework.data.mongodb.core.query.Query.query;


List<Person> result = template.query(Person.class)
    .matching(query(where("age").It(50).and("accounts.balance").gt(1000.00d)))
    .all();
```

所有find方法都有一个query对象作为参数。定义用于执行查询的条件和选项。这些条件是通过使用一个Criteria对象来指定的，具有一个where实例化新Criteria对象的静态工厂方法。

##### Criteria类的方法

Criteria all(Object o)  使用$all 运算符创建条件

Criteria and(String key)添加一个Criteria与指定的链接key到当前Criteria并返回新创建

Criteria andOperator(Criteria criteria)使用$and运算符为所有提供的条件创建和查询

Criteria andOperator(Collection<Criteria> criteria)使用$and运算符为所有提供的条件创建和查询

Criteria eleMatch(Criteria c)使用$elemMatch运算符创建标准

Criteria exists(boolean b)使用$exists运算符创建条件

Criteria gt(Object o)使用$gt运算符创建条件

Criteria gte(Object o)使用$gte运算符创建条件

Criteria in(Object o)使用$in varargs参数的运算符创建条件

Criteria in(Collection<?> collection)使用$in使用集合的运算符创建条件

Criteria is(Object o)使用字段匹配({key:value})创建标准，

Criteria It(Object o)使用$It运算符创建条件

Criteria Ite(Object o)使用$Ite运算符创建条件

Criteria mod(Number value,Number remainder)使用$mod运算符创建一个标准

Criteria ne(Object o)使用$ne运算符创建标准

Criteria nin(Object o)使用$nin运算符创建标准

Criteria norOperator(Criteria criteria)使用$nor运算符为所有提供的条件创建一个nor查询

Criteria norOperator(Collection<Criteria> criteria)使用$nor运算符为所有提供的条件创建一个nor查询

Criteria not() 使用$not影响紧随其后的子句的元运算符创建一个条件

Criteria orOperator(Criteria criteria)使用$or运算符为所有提供的条件创建or查询

Criteria orOperator(Collection<Criteria> criteria)使用$or运算符为所有提供的条件创建or查询

Criteria regex(String re)使用$regex

Criteria size(int s)使用$size运算符创建标准

Criteria type(int t)使用$type运算符创建条件

Criteria matchingDocumentStructure(MongoJsonSchema schema)创建使用的标准$jsonSchema 操作员JSON模式标准   $jsonSchema只能应用于查询的顶层，而不是特定于属性的。使用properties构架的属性来匹配嵌套字段

Criteria bits() 是MongoDB按位查询运算符

##### Query类的方法

Query addCriteria(Criteria criteria) 用于向查询添加附加条件

Fiela fields() 用于定义要包含在查询结果中的字段

Query limit(int limit)用于将返回结果大小限制为提供的限制

Query skip(int skip)用于跳过结果中提供的文档数

Query with(Sort sort)用于为结果提供排序定义

##### 选择字段

示例：选择结果字段

```
public class Person {
    @Id String id;
    String firstname;
    
    @Field("last_name")
    String lastname;
    
    Address address;
}

query.fields().include("lastname");//1
query.fields().exclude("id").include("lastname");//2
query.fields().include("address");//3
query.fields().include("address.city");//4
```

1.结果将同时包含_id 和 last_name {"last_name":1}

2.结果将仅包含last_name  {"_id":0,"last_name":1}

3.结果将通过包含_id整个address对象{"address":1}

4.结果将包含仅包含字段via的 _id和 address对象，city{"address.city":1}

示例：使用表达式计算结果字段

```java
query.fields()
    .project(MongoExpression.create("'$toUpper'L'$last_name"))1
    .as("last_name");2
query.fields()
    .project(StringOperators.valueOf("lastname").toUpper)3
    .as("last_name");
query.fields()
    .project(AggregationSpElExpression.expressionOf("toUpper(lastname)"))4
    .as("last_name");
```

1.使用本机表达式，使用的字段名称必须引用数据库文档中的字段名称

2.分配表达式结果投影到的字段名称，结果字段名称未映射到域模型

3.使用AggregationExpression。除了native MongoExoression 字段名称映射到域模型中使用的名称

4.使用SpEL和一个AggregationExpression来调用表达式函数。字段名称映射到域模型中使用的名称

#### 查询文档的方法

findAll：T从集合中查询类型对象的列表

findOne：将集合上的即席查询的结果映射到指定类型对象的单个实例

findById：返回给定ID和目标类的对象

find：将集合上的即席查询的结果映射到List指定类型的a

findAndRemove：将集合上的即席查询的结果映射到指定类型的对象的单个实例，与查询匹配的第一个文档被返回并从数据库中的集合中删除

#### 查询不同的值

示例：检索不同的值

```java
template.query(Person.class)1
    .distinct("lastname")2
    .all();3
```

1.查询Person集合

2.选择lastname字段的不同值。字段名称根据域类型属性声明进行映射，同时考虑了潜在的@Field注释

3.将所有不同的值作为List  of检查Object

示例：检索强类型的不同值

```java
template.query(Person.classs)1
    .distinct("lastname")2
    .as(String.class)3
    .all(0;)4
```

1.查询的集合Person

2.选择lastname字段的不同值。字段名根据域类型属性声明进行映射，同时考虑了潜在的@Field注释

3.检索到的值转换为所需的目标类型

4.检索所有不同的值作为一个 String的List。若类型无法转换为所需的目标类型，则此方法将抛出DataAccessException

#### GeoSpatial Queries

要了解如何执行GeoSpatial查询，考虑以下Venue类

```
@Document(collection = "newyork")
public class Venue {
    @Id
    private String id;
    private String name;
    private double[] location;

    @PersistenceConstructor
    Venue(String name,double[] location){
        super();
        this.name = name;
        this.location = location;
    }

    public Venue(String name, double x,double y) {
        super();
        this.name = name;
        this.location = new double[]{x,y};
    }

    public String getName() {
        return name;
    }

    public double[] getLocation() {
        return location;
    }

    @Override
    public String toString() {
        return "Venue{" +
                "id='" + id + '\'' +
                ", name='" + name + '\'' +
                ", location=" + Arrays.toString(location) +
                '}';
    }
}
```

查找位置Cirecle，可以使用以下查询

```
Circle circle = new Circle(-73.99171,40.738868,0.01);
List<Venue> venues = template.find(new Query(Criteria.where("location").within(circle)),Venue.class);
```

在Cirecle使用球坐标中查找场地，--

```
Circle circle = new Circle(-73.99171,40.738868,0.003712240453784);
List<Venue> venues = template.find(new Query(Criteria.where("location").withinSphere(circle)),Venue.class);
```

查找场所Box，--

```
Box box = new Box(new Point(-73.99756,40.73083),new Point(-73.988135,40.741404));
List<Venue> venues = template.find(new Query(Criteria.where("location").within(box)),Venue.class);
```

查找附近的场地Point

```
Point point = new Point(-73.99171,40.738868);
List<Venue> venues = template.find(new Query(Criteria.where("location").near(point).minDistance(0.01).maxDistance(100)),Venue.class);
```

要Point使用球坐标查找附近的场地

```
Point point = new Point(-73.99171,40.738868);
List<Venue> venues = 
	template.find(new Query(
		Criteria.where("location").nearSphere(point).maxDistance(0.003712240443784)),
		Venue.class);
```

dis 先前在包装器类型中返回的计算距离现在嵌入到生成的文档中，如果给定的域类型已经包含具有该名称的属性，则计算出的距离将calculated-distance使用一个潜在的随机后缀命名

目标类型可能包含一个以返回的距离命名的属性，以将其直接读回域类型

```
GeoResults<VenueWithDisField> = template.query(Venue.calss)1
	.as(VenueWtihDisField.class)
	.near(NearQuery.near(new GeoJsonPoint(-73.99,40.73),KILOMETERS))
	.all();
```

1.用于标识目标集合和潜在查询映射的域类型

2.包含dis type字段的目标类型Number、

MongoDB在数据库中查询地理位置并同时计算距给定原点的距离。使用geo-near查询，可以选择查找范围。

MongoOperations提供了geoNear 将 Near Query作为参数的方法--

```
Point location = new Point(-73.99171,40.738868);
NearQuery query = NearQuery.near(location).maxDistance(new Distance(10,Metrics.MILES));

GeoResults<Restaurant> = operations.geoNear(query,Restaurant.class);
```

#### GeoJSON支持

##### 域类中的GeoJSON类型

示例：使用了一个GeoJsonPoint

```
public class store{
	String id;
	GeoJsonPoint location;
}
```

##### 存储库查询方法中的GeoJSON类型

使用GeoJSON类型作为存储库查询参数会$geometry 在创建查询时强制使用运算符

示例：

```java
public interface StoreRepository extends CrudRepository<Store,String>{
    List<Store> findByLocationWithin(Pplygon polygon);
}
{
      "location":{
        "$geoWithin":{
            "$geometry":{
                "type":"Polygon",
                "coordinates":[
                    [
                        [-73.992514,40.758934],
                        [-73.992514,40.758934].
                        [-73.991658,40.730006],
                        [-73.992514,40.758934]
                    ]
                ]
            }
        }
    }
}
repo.findByLocationWithin(
    new GeoJsonPolygon(
     new Point(-73.992514, 40.758934),
   	 new Point(-73.961138, 40.760348),
  	 new Point(-73.991658, 40.730006),
  	 new Point(-73.992514, 40.758934)));
{
    "Location":{
        "$geoWithin":{
            "$polygon":[
                [-73.992514,40.758934].
                [-73.961134,40.760348],
                [-73.991658,40.730006]
        }
    }
}
repo.findByLocationWithin(
    new Polygon(
        new Point(-73.992514,40.758934),
        new Point(-73.961138,40.760348),
        new Potin(-73.991658,40.760006)
    )
);
```

1.使用commons类型的存储库方法定义允许使用GeoJSON和遗留格式调用它

2.使用GeoJSON类型来使用$geometry运算符

3.GeoJSON多边形需要定义一个封闭的环

4.使用旧格式$polygon运算符

##### 度量和距离计算

```
NearQuery.near(new Point(-73.99171,40.738868))
```

```
{
	"$geoNear":{
		"near":[-73.99171,]
	}
}
```

```
NearQuery.near(new GeoJsonPoint(-73.99171,40.738868))
```

```
{
	"$geoNear":{
	"near":{"type":"point","coordinates":[-73.99171,40.738868]}
	}
}
```

距离计算存在巨大差异，使用旧格式对地球上的弧度进行操作。如球体，而GeoJSON使用Meters

确保设置Meric所需的测量单位，以确保正确计算距离

若有5个文件

```java
{
	"_id":ObjectId("5c10f3735d38908db52796a5"),
    "name":"Penn Station",
    "location":{"type":"Point","coordinates":[-73.99408,40.75057]}
}
{
    "_id":ObejctId("5c10f3735d38908db52796a6"),
    "name":"10gen Offince",
    "location":{"type":"Point","coordinates":[-73.99171,40.738868]}
}
{
    "_id":ObjectId("5c10f3735d38908db52796a9"),
    "name":"City Bakery",
    "lcoation":{"type":"Point","coordinates":[-73.992491,40.738673]}
}
{
    "_id":ObjectId("5c10f3735d38908db52796aa"),
    "name":"Splash Bar",
    "location":{"type":"Point","coordinates":[-73.992491,40.738673]}
}
{
    "_id":ObjectId("5c10f3735d38908db52796aa"),
    "name":"Momoguku Milk Bar",
    "location":{"type":"Point","coordinates":[-73.985839,40.731698]}
}
```

[-73.99171,40.738868]使用GeoJSON获取400米半径内的所有文档如下所示

示例：GeoNear && GeoJSON

```
{
	"$geoNear":{
		"maxDistance":400,
		"num":10,
        "near":{type:"Point",coordinates:[-73.99171,40.738868]},
        "spherical":true,
        "key":"location",
        "distanceField":"distance"
	}
}
```

返回以下三个文件

```java
{
    "_id":ObjectId("5c10f3735d38908db52796a6"),
    "name":"10gen Office",
    "location":{"type":"Point","coordinates":[-73.99171,40.738868]}
 1   "distance":0.0
}
{
    "_id":ObjectId("5c10f3735d38908db52796a9"),
    "name":"City Bakery",
    "location":{"type":"Point","coordinates":[-73.992491,40.738673]}
2   "distance":69.3582262492474
}
{
    "_id":ObjectId("5c10f3735d38908db527796aa"),
    "name":"Splash Bar",
    "location":{"type":"Point","coordinates":[-73.992491,40.738673]}
3    "distance":69.3582262492474
}
```

1.到中心点的最大距离

2.GeoJSON总是在一个球体 上运行

3.到中心点的距离

当使用旧坐标时，对弧度进行操作。所以使用$geoNear命令。在Metric确保使乘数设置正确的距离

示例：带有传统坐标对的GeoNear

```java
{
    "$geoNear":{
    1    "maxDistance":0.0000627142377,
    2    "distanceMultiplier":6378.137,
        "num":10,
        "near":[-73.99171,40.738868],
    3    "spherical":true,
        "key":"location",
        "distanceField":"distance"
    }
}
```

像GeoJSON变体一样返回三个文档

```java
{
    "_id":ObjectId("5c10f3735d38908db52796a6"),
    "name":"10gen Office",
    "location":{"type":"Point","coordinates":[-73.99171,40.738868]}
 4   "distance":0.0
}
{
    "_id":ObjectId("5c10f3735d38908db52796a9"),
    "name":"City Bakery",
    "location":{"type":"Point","coordinates":[-73.992491,40.738673]}
  4  "distance":0.0693586286032982
}
{
    "_id":ObjectId("5c10f3735d38908db52796aa"),
    "name":"Splash Bar",
    "location":{"type":"Point","coordinates":[-73.992491,40.738671]}
  4  "distance":0.0693586286032982
}
```

1.到中心点的最大距离

2.距离乘数所以得到公里作为结果距离

3.确保对2d_sphere索引进行操作

4.以公里为单位和中心点的距离--乘以1000以匹配GeoJSON变体的类

##### GeoJSON Jackson Modules

要配备ObjectMapper一组对称的JsonSerializer S，需要手动配置 S ObjectMapper 或提供为Spring Bean SpringDataJacksonModules 公开的自定义配置GeoJsonModule.serializers()

```
class GeoJsonConfiguration implements SpringDataJacksonModules{
	@Bean
	public Module geoJsonSerializers(){
		return GeoJsonModule.serializers();
	}
}
```

#### 全文查询

全文检索

```java
db.foo.createIndex{
    {
        title:"text",
        content:"text"
    },
    {
        weights:{
            title:3
        }
    }
}
```

coffee cake可以按如下方法定义和运行查询搜索：

```java
Query query = TextQuery.queryText(new TextCriteria().matchingAny("coffee","cake"));
List<Document> page = template.find(query,Document.class);
```

根据weights用途按相关性对结果进行排序TextQuery.soryByScore

示例:按分数排序 全文查询

```java
Query query = TextQuery
    .queryText(new TextCriteria().matchingAny("coffee","cake"))
    .sortByScore()1
    .includeScore();2
List<Document> page = template.find(query,Document.class);
```

1.使用score属性按触发的相关性对结果进行排序.sot({'score':{'$meta':'textScore'}})

2.用于  TextQuery.includeScore() 在结果中包含计算出的相关性Document

在搜索词前加上 - 或使用来排除搜索词  notMatching如下图所示

```
TextQuery.queryText(new TextCriteria().matching("coffee").matching("-cake"));
TextQuery.queryText(new TextCriteria().matching("coffee").matching("cake"));
```

TextCriteria.matching按原样使用提供的术语

可以通过将短语放在双引号之间来定义短语（"\coffee cake\"或使用by TextCriteria.phrase

下面的示例显示了定义短语的两种方式

```
TextQuery.queryText(new TextCriteria().matching("\coffee cake\"));
TextQuery.queryText(new TextCriteria().phrase("coffee cake"));
```

#### 校对

MongoDB支持用于集合和索引创建以及各种查询操作的排序规则。排序规则根据ICU排序规则定义字符串比较规则。归类文档由封装在其中的各种属性组成Collation

```java
Collation collation = Collation.of("fr")1
    .strengh(ComparisonLevel.secondary()2
            .includeCase())3
    .numericOrderingEnabled()4
    .alternate(Alternate.shifted().punct())5
    .forwarDiacriticSort()6
    .normalizationEnabled();7
```

1.Collation 需要一个语言环境来创建。可以是区域设置的字符串形式  Locale或CollationLocale

2.整理强度定义了表示字符之间差异的比较级别。

3.指定时将数字字符串作为数字还是作为字符串进行比较

4.指定排序规则是否应将空格和标点符号视为基本字符以进行比较

5.指定带有变音符号的字符串是否从字符串的后面排序，

6.指定是否检查文本是否需要归一化以及是否进行归一化

与其他元数据一样，排序规则可以通过注释的collation属性从域类型派生@Document，并将在运行查询、创建集合或索引时直接应用

当Mongodb 在第一次交互自动创建集合时，将不会使用带注释的排序规则。将需要额外的shop交互，从而延迟整个过程。

```java
Collation french = Collation.of("fr");
Collation german = Collation.of("de");

template.createCollection(Person.class,CollectionOptions.just(collation));

template.indexOps(Person.class).ensureIndex(new Index("name",Direction.ASC).collation(german));
```

如果未指定排序规则（Collation.simple()) Mongodb 将使用简单的二进制比较

示例:使用排序规则 与find

```java
Collation collation = Collation.of("de");

Query query = new Query(Criteria.where("firstname").is("Amel")).collation(collation);

List<Person> results = template.find(query,Person.class);
```

示例：使用排序规则与aggregate

```java
Collation collation = Collation.of("de");
AggreationOptions options = AggregationOptions.builder().collation(collation).build();

Aggregation aggregation = newAggregation(
    project("tags"),
    unwind("tags"),
    group("tags")
    .count().as("count")
).withOptions(options);

AggregationResults<TagCount> results = template.aggregate(aggregation,"tags",TagCount.class);
```

MongoDb Repositories Collations 通过注解的 collation 属性支持@Query

示例：对存储库的整理支持

```java
public interface PersonRepository extends MongoRepository<Person,String>{
    @Query(collation = "en_US")
    List<Person> findByFirstname(String firstname);
    
    @Query(collation = "{'local':'en_US'}")
    List<Person> findPersonByFirstname(String firstname);
    
    @Query(collation = "?1")
    List<Person> findByFirstname(String firstname,Object collation);
    
    @Query(collation = "{'locale':'?1'}")
    List<Person> findByFirstname(String firstname,String collation);
    
    List<Person> findByFirstname(String firstname,Collation collation);
    
    @Query(collation = "{'locale':'en_US'}")
    List<Person> findByFirstname(String firstname,@Nullable Collation collation);
```

静态排序规则导致   {'locale':'en_US'}

动态排序规则取决于第二个方法参数

将Collation 方法参数应用于查询

将Collation方法的参数覆盖默认collation的@Query

##### JSON架构

示例 JSON模式

```java
{
    "type":"object",
    "required":["firstname","lastname"],
    
    "properties":{
        "firstname":{
            "type":"string",
            "enum":["luke","han"]
        },
        "address":{
            "type":"object"
                "properties":{
                    "postCode":{"type":"string","minLength":4,"maxLength":5}
                }
        }
    }
}
```

示例 创建JSON模式

```java
MongoJsonSchema.builder()
    .required("lastname")
    .properties(
    			required(sring("firstname").possibleValues("luke","han")),
    			object("address")
    				.properties(string("postCode").minLength(4).maxLength(5))
    .build();
)
```

示例  创建集合$jsonSchema

```java
MongoJsonSchema schema = MongoJsonSchema.builder().required("firstname","lastname").build();
template.createCollection(Person.class,CollectionOptions.empty().schema(schema));
```

示例： 从域类型生成Json  Schema

```java
public class Person {
    private final String firstname;
    private final int age;
    private Species species;
    private Address address;
    private @Field(fieldType=SCRIPT) String theForce;
    private @Transient Boolean useTheForce;

    public Person(String firstname, int age) {

        this.firstname = firstname;
        this.age = age;
    }

    public String getFirstname() {
        return firstname;
    }

    public int getAge() {
        return age;
    }

    public Species getSpecies() {
        return species;
    }

    public void setSpecies(Species species) {
        this.species = species;
    }

    public Address getAddress() {
        return address;
    }

    public void setAddress(Address address) {
        this.address = address;
    }

    public String getTheForce() {
        return theForce;
    }

    public void setTheForce(String theForce) {
        this.theForce = theForce;
    }

    public Boolean getUseTheForce() {
        return useTheForce;
    }

    public void setUseTheForce(Boolean useTheForce) {
        this.useTheForce = useTheForce;
    }
    MongoJsonSchema schema = MongoJsonSchemaCreator.create(mongoOperations
            .getConverter()).createSchemaFor(Person.class);
    template.createCollection(Person.class,CollectionOptions.empty().schema(schema));
}

```

示例  查询匹配文档  $jsonSchema

```java
MongoJsonSchema schema = MongoJsonSchema.builder().required("firstname","lastname").build();
template.find(query(matchingDocumentStructure(schema)),Person.class);
```

##### 加密字段

示例：通过Json Schema的客户端字段级加密

```java
MongoJsonSchema schema = MongoJsonSchema.builder()
    .properties(
    	encrypted(string("ssn"))
    		.algorithm("AEAD_AES_256_CBC_HMAC_SHA_512-Deterministic")
    		.keyId("key0_id")
).build();
```

流畅的模板API

MongoOperatopms  接口 --核心组件之一--涵盖从集合创建、索引创建和CRUD操作到更高级功能需求 提供更易读、更流畅的API

FluentMongoOperations为通用方法提供更窄的接口

```java
List<SWCharacter> all = ops.find(SWCharacter.class)
    .inCollection("star-wars")
    .all();
```

```java
List<Jedi> all = ops.find(SWCharacter.class)    //查询字段根据SWCharacter类型进行映射
    .as(Jedi.class)			//结果文档被映射到jedi
    .matching(query(where("jedi").is(true)))
    .all();
```

将实体作为GeoResult内获取GeoResults

```java
GeoResults<Jedi> results = mogoOps.query(SWCharacter.class)
    .as(Jedi.class)
    .near(alderaan)
    .all();
```

#### Kotlin类型安全查询

解释类型安全查询的示例

```java
mongoOperations.find<Book>(
Query(Book::title isEqualTo "Moby-Dick"));

mongoOperations.find<Book>(
    Query(titlePredicate = Book::title exists true));
mongoOperations.find<Book>(
	Query(
		Criteria().andOperator(
			Book::price gt 5,
			Book::price 1t 10
        )));
mongoOperations.find<BinaryMessage>(
	Query(BinaryMessage::payload bits{allClear(0b101)}));

mongoOperations.find<Book>(
Query(Book::author / Author::name regex "^H"));
   
```

isEqualTo() 是一个接收器类型的中缀扩展函数KProperty<T> 返回Criteria

#### 其他查询选项

```java
Query query = query(where("firstname").is("luke"))
    .comment("find luke")
    .barchSize(1100)
```

@Meta注释提供了以声明方式添加查询选项的方法

```java
@Meta(comment = "find luke",batchSize = 100,flags = {SLAVE_OK})
List<Person> findByFirstname(String firstname);
```

### 11.7 按示例查询

#### 介绍

#### 用法

Query by Example API由三部分组成

探针		ExampleMatcher		Example

Query by Example 适合以下用例

使用一组静态或动态约束查询数据存储

频繁重构对象不必担心破坏现有查询

独立于底层数据存储API工作

Query by Example 有几个限制

不支持嵌套分组的属性约束

金支持字符串的开始/包含/结束/正则表达式以及其他属性类型的精确匹配

示例：Person对象

```java
public class Person {
    @Id
    private String id;
    private String firstname;
    private String lastname;
    private Address address;

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getFirstname() {
        return firstname;
    }

    public void setFirstname(String firstname) {
        this.firstname = firstname;
    }

    public String getLastname() {
        return lastname;
    }

    public void setLastname(String lastname) {
        this.lastname = lastname;
    }

    public Address getAddress() {
        return address;
    }

    public void setAddress(Address address) {
        this.address = address;
    }
}
```

示例 :简单示例

```java
Person person = new Person();
person.setFirstname("Dave");

Example<Person> example = Example.of(Person);
```

示例：QueryByExampleExecutpr

```java
public interface QueryByExampleExecutor<T>{
    <S extends T> S findOne(Example<S> example);
    <S extends T> Iterable<S> findAll(Example<S> example);
}
```

示例匹配器

示例:具有自定义匹配的实力匹配器

```java
Person person = new Person();//创建域对象的新实例
person.setFirstname("Dave");//设置属性

ExampleMatcher matcher = ExampleMatcher.matching()//创建一个ExampleMatcher一起往所有值匹配
    .withIgnoerPaths("lastname")//创建一个新ExampleMatcher的忽略lastname属性路径
    .withIncludeNullValues()//构造一个新ExampleMatcher以忽略lastname属性路径并包含空值
    .withStringMatcher(StringMatcher.ENDING);//构造一个新ExampleMatcher以忽略lastname属性路径

Example<Person> example = Example.of(person,matcher);//创建一个新Example基于域对象和配置上ExampleMatcher
```

实例:匹配匹配器选项

```java
ExampleMatcher matcher = ExampleMatcher.matching()
    .withMatcher("firstname",endWith())
    .withMatcher("lastname",startsWith().ignoreCase());
```

配置匹配器选项的方法是使用lambdas，创建一个回调，要求实现者修改匹配器。

示例：使用lambda配置匹配器选项

```java
ExampleMathcer matcher = ExampleMatcher.matching()
    .withMatcher("firstname",match -> match.endsWith())
    .withMatcher("firstname",match -> match.startsWith());
```

#### 运行示例

示例:使用存储库按示例查询

```java
public interface PersonRepository extends QueryByExampleExecutor<Person>{
    
}
public class PersonService{
    @Autowired PersonRepository personRepository;
    public List<Person> findPeople(Person probe){
        return personRepository.findAll(Example.of(probe));
    }
}
```

#### 无类型示例

默认情况下Example是严格键入的，以为着映射的查询具有包括的类型匹配，将其限制为探测可分配的类型

示例：无类型示例查询

```java
class JustAnarbitaryClasswithMatchingFieldName{
    @Field("lastname") String value;
}
JustAnArbitaryClassWtihMatchingFieldNames probe = new JustAnArbitaryClassWithMatchingFieldNames();
probe.value="stark";
Example example = Example.of(probe,UntypedExampleMatcher.matching());

Query query = new Query(new Criteria().alike(example));
List<Person> result = template.find(query,Person.class);
```

### 11.8 计数文件

 MongoDBs 本地countDocuments方法和$match聚集，不支持$near和$nearSphere但需要$geoWithin连同`$center`或`$centerSphere`不支持`$minDistance`

Query重写给定的count操作，使用Reactive-/MongoTemplate绕如下：

```java
{location : {$near:[-73.99171,40.438868],$maxDistance:1.1}}
{location : {$geoWithin : {$center:[[-73.99171,40.738868],1.1]}}}
{location : {$near:[-73.99171,40.738868],$minDistance:0.1,$maxDistance:1.1}}
{$and:[$nor:[{location:{$geoWithin:{$center:[[-73.99171,40.738868],0.01]}}}]]}
```

### 11.9 Map-Reduce操作

Spring 通过提供方法MongoOperations来简化Map-Reduce操作的创建和运行，从而提供与MongoDB的Map-Reduce的集成

#### 示例用法

理解Map-Reduce操作，使用例子：创建三个具有[a,b],[b,c]的文档和[c,d]每个文档值与键"x"相关联

```java
{"_id":ObjectId("4e5ff893c0277826074ec533"),"x":["a","b"]}
{"_id":ObjectId("4e5ff893c0277826074ec534"),"x":["b","c"]}
{"_id":ObjectId("4e5ff893c0277826074ec535"),"x":["c","d"]}
```

map函数计算每个文档数据中每个字母的出现次数

```java
function(){
    for(var i = 0;i < this.x.length;i++){
        emit(this.x[i],i);
    }
}
```

reduce函数总结所有文档中每个字母的出现

```java
function(key,values){
    var sum = 0;
    for(var i = 0;i < values.length;i++){
        sum += values[i];
    }
    return sum;
}
```

运行上述函数产生以下集合

```java
{"_id":"a","value":1}
{"_id":"b","value":2}
{"_id":"c","value":2}
{"_id":"d","value":1}
```

若map和reduce函数位于map.js和reduce.js并捆绑在jar中，按如下运行Map-Reduce操作

```java
MapReduceResults<valueObject> results = mongoOperations.mapReduce("jmr1","classpath:map.js","classpath:reduce.js",ValueObject.class);
for(ValueObject valueObject:results){
    Ststem.out.println(valueObject);
}
```

输出

```java
ValueObject [id=a, value=1.0]
ValueObject [id=b, value=2.0]
ValueObject [id=c, value=2.0]
ValueObject [id=d, value=1.0]
```

MapReduceResults类实现Iterable并提供访问的原始输出和定时和下面列出计数statistics

```java
public class ValueObject{
    private String id;
    private float value;
    
    
  public String getId() {
    return id;
  }

  public float getValue() {
    return value;
  }

  public void setValue(float value) {
    this.value = value;
  }

  @Override
  public String toString() {
    return "ValueObject [id=" + id + ", value=" + value + "]";
  }
}
```

MapReduceOptions 类具有流畅的API，可以添加额外的选项以及紧凑的语法

以下示例将输出集合  jmr1_out设置为

```java
MapReduceResults<ValueObject> results = mongoOperations.mapReduce("jmr1","classpath:map.js","classpath:reduce.js",new MapReduceOptions().outputCollection("jmr1_out"),ValueObject.class);
```

还有一个静态导入稍微紧凑

```java
MapReduceResults<ValueObject> results = mongoOperations.mapReduce("jmr1", "classpath:map.js", "classpath:reduce.js",options().outputCollection("jmr1_out"), ValueObject.class);
```

还可以指定查询减少Map-Reduce操作的数据集

```java
Query query = new Query(where("x").ne(new String[]{"a","b"}));

MapReduceResults<ValueObject> results = mongoOperations.mapReduce(query,"jmr1","classpath:map.js","classpath:reduce.js",options().outputCollection("jmr1_out"),ValueObject.class);
```

### 11.10 脚本操作

MongoDB允许通过直接发送脚本或调用存储的脚本在服务器上运行JavaScript函数

ScriptOperations可以通过访问MongoTemplate并提供基本的JavaScript使用抽象

```java
ScriptOperations scriptOps = template.scriptOps();

ExecurableMongoScript echoScript = new ExecutableMongoScript("function(x){return x;}");
scriptOps.execute(echoScript,"directly execute script");//直接运行脚本，不需要再服务器上存储函数

scriptOps.register(new NamedMongoScript("echo",echoScript));//使用“echo”作为名称存储脚本，
scriptOps.call("echo","execute script via name");//使用提供的参数运行名为“echo”的脚本
```

### 11.11 Group Operations

可以使用SQL的group by查询的风格，使用group有限制，例如在共享环境中不受支持，返回单个BSON对象中的完整结果集。

Spring 通过在MongoOperations上提供方法来提供与MongoDB的组的操作的集成，简化操作的创建和运行，可以将分组操作的结果转换为POJO，并且集成Spring的Resource抽象

#### 示例用法

group_test_collection使用以下行创建的名为集合

```java
{"_id":ObjectId("4ex1d25d41421e5015da65f1"),"x":1}
++
```

按每行中唯一的字段进行分组，该x字段和聚合每个特定值x出现的次数。创建一个文档包含我们的count变量和一个reduce变量，每次遇到增加

```java
GroupByResults<XObjetc> results = mongoTemplate.group("group_test_collection",
                                                     GroupBy.key("x").initialDocument("{count:0}").reduceFunction("function(soc,prev){prev.count += 1}")),
													Xobject.class);
```

第一个参数是运行组操作的集合的名称，第二个参数是一个流利的API，通过一个GroupBy类指定组操作的属性

group操作的原始结果是一个JSON文档  如下

```java
{
    "retval":[{"x":1.0,"count":2.0},
              {"x":2.0,"count":1.0},
              {"x":3.0,"count":3.0}],
    "count":6.0,
    "keys":3,
    "ok":1.0
}
```

"retval"字段下的文档映射到group方法中的第三个参数 如下

```java
public class XObject{
    private float x;
    private float count;
    
  public float getX() {
    return x;
  }

  public void setX(float x) {
    this.x = x;
  }

  public float getCount() {
    return count;
  }

  public void setCount(float count) {
    this.count = count;
  }

  @Override
  public String toString() {
    return "XObject [x=" + x + " count = " + count + "]";
  }
}
```

group还有额外方法的重载，MongoOperations允许指定一个Criteria对象来选择行的子集。

如下一个使用Criteria对象的示例

```java
GroupByResults<XObject> results = mongoTemplate.group(where("x").gt(0),
                                                     "group_test_collection",
                                                     keyFunction("classpath:keyFunction.js").initialDocument("{count:0}").reduceFunction("classpath:froupReduce.js"),XObject.class);
```

### 11.12聚合框架支持

#### 基本

在Spring数据MongoDB中的聚合框架的支持是基于以下关键抽象：Aggregation，AggregationDefinition，

AggrefationResults

Aggregation：表示MongoDB  aggregate操作并保存聚合管道指令的描述。聚合是通过调用类的newAggregation()静态factory方法来创建的。Aggregation接受一个列表AggregateOperation和一个可选的收入类

TypeAggregation

像Aggregation  包含聚合管道的指令和对输入类型的引用，用于将域属性映射到实际文档字段

AggregationDefinition

表示MongoDB聚合管道操作并描述应在聚合步骤中执行的处理。建议使用Aggregate类提供的静态factory方法来构造AggregateOperation

AggregationResults

AggregationResults是聚合操作结果的容器  提供对原始聚合结果的访问，Document以映射对象和有关聚合的其他信息的形式

以下显示了规范示例

```java
Aggregation agg = newAggregation(
	pipelineOP1(),
    pipelineOP2(),
    pipelineOPn(0)
);
AggregationResults<OutputType> results = mongoTemplate.aggregate(agg,"INPUT_COLLECTION_NAME",OutputType.class);
List<OutputType> mappedResult = results.getMappedResults();
```

支持的聚合操作

提供以下类型聚合操作

- 管道聚合运算符
- 组/累加器聚合运算符
- 布尔聚合运算符
- 比较聚合运算符
- 算术聚合运算符
- 字符串聚合运算符
- 日期聚合运算符
- 数组聚合运算符
- 条件聚合运算符
- 查找聚合运算符
- 转换聚合运算符
- 对象聚合运算符
- 脚本聚合运算符

| 管道聚合运算符      | `bucket`, `bucketAuto`, `count`, `facet`, `geoNear`, `graphLookup`, `group`, `limit`, `lookup`, `match`, `project`, `replaceRoot`, `skip`, `sort`,`unwind` |
| ------------------- | ------------------------------------------------------------ |
| 设置聚合运算符      | `setEquals`, `setIntersection`, `setUnion`, `setDifference`, `setIsSubset`, `anyElementTrue`,`allElementsTrue` |
| 组/累加器聚合运算符 | `addToSet`, `first`, `last`, `max`, `min`, `avg`, `push`, `sum`, `count`(*), `stdDevPop`,`stdDevSamp` |
| 算术聚合运算符      | `abs`, `add`(* via `plus`), `ceil`, `divide`, `exp`, `floor`, `ln`, `log`, `log10`, `mod`, `multiply`, `pow`, `round`, `sqrt`, `subtract`(* via `minus`),`trunc` |
| 字符串聚合运算符    | `concat`, `substr`, `toLower`, `toUpper`, `strcasecmp`, `indexOfBytes`, `indexOfCP`, `split`, `strLenBytes`, `strLenCP`, `substrCP`, `trim`, `ltrim`,`rtim` |
| 比较聚合运算符      | `eq`(* 通过`is`), `gt`, `gte`, `lt`, `lte`,`ne`              |
| 数组聚合运算符      | `arrayElementAt`, `arrayToObject`, `concatArrays`, `filter`, `in`, `indexOfArray`, `isArray`, `range`, `reverseArray`, `reduce`, `size`, `slice`,`zip` |
| 文字运算符          | `literal`                                                    |
| 日期聚合运算符      | `dayOfYear`, `dayOfMonth`, `dayOfWeek`, `year`, `month`, `week`, `hour`, `minute`, `second`, `millisecond`, `dateToString`, `dateFromString`, `dateFromParts`, `dateToParts`, `isoDayOfWeek`, `isoWeek`,`isoWeekYear` |
| 变量运算符          | `map`                                                        |
| 条件聚合运算符      | `cond`, `ifNull`,`switch`                                    |
| 类型聚合运算符      | `type`                                                       |
| 转换聚合运算符      | `convert`, `toBool`, `toDate`, `toDecimal`, `toDouble`, `toInt`, `toLong`, `toObjectId`,`toString` |
| 对象聚合运算符      | `objectToArray`, `mergeObjects`                              |
| 脚本聚合运算符      | `function`, `accumulator`                                    |

#### 投影表达式

作为特定局和步骤结果的字段。

投影表达式示例

```java
// generates {$project: {name: 1, netPrice: 1}}
project("name", "netPrice")

// generates {$project: {thing1: $thing2}}
project().and("thing1").as("thing2")

// generates {$project: {a: 1, b: 1, thing2: $thing1}}
project("a","b").and("thing1").as("thing2")
```

示例：使用投影和排序的多阶段聚合

```java
// generates {$project: {name: 1, netPrice: 1}}, {$sort: {name: 1}}
project("name", "netPrice"), sort(ASC, "name")

// generates {$project: {name: $firstname}}, {$sort: {name: 1}}
project().and("firstname").as("name"), sort(ASC, "name")

// does not work
project().and("firstname").as("name"), sort(ASC, "firstname")
```

#### 分面分类

分面分类使用组合起来创建完整分类条目的语义类别。流经聚合管道的文档被分类到桶中。多面分类可以对同一组输入文档进行各种聚合，无需多次检索输入文档

##### 桶

存储桶操作根据指定的变道时和存储桶边界将传入文档分类为多个组，成为存储桶。

示例：存储桶操作

```java
// generates {$bucket: {groupBy: $price, boundaries: [0, 100, 400]}}
bucket("price").withBoundaries(0, 100, 400);

// generates {$bucket: {groupBy: $price, default: "Other" boundaries: [0, 100]}}
bucket("price").withBoundaries(0, 100).withDefault("Other");

// generates {$bucket: {groupBy: $price, boundaries: [0, 100], output: { count: { $sum: 1}}}}
bucket("price").withBoundaries(0, 100).andOutputCount().as("count");

// generates {$bucket: {groupBy: $price, boundaries: [0, 100], 5, output: { titles: { $push: "$title"}}}
bucket("price").withBoundaries(0, 100).andOutput("title").push().as("titles");
```

示例：存储桶操作示例：

```java
//generates{$bucketAuto:{groupBy:$price,buckets:5}}
bucketAuto("price",5);

//generates{$bucketAuto:{groupBy:$price,buckets:5,graularity:"E24"}}
bucketAuto("price",5).withGranularity(Granularities.E24).withDefault("Other");

//generates{$bucketAuto:{groupBy:$price,buckets:5,output:{titles:{$push:"$title"}}}}
bucketAuto("price",5).andOutput("title").push().as("titles");
```

##### 多面聚合

多个聚合管道可用于创建多方面聚合，在单个聚合阶段内表征跨多个维度的数据。多面聚合提供多个过滤器和分类来指导数据浏览和分析。

FacetOperation 使用类的facet()方法定义一个Aggregation

示例：分面操作实例

```java
facet(match(Criteria.where("price").exists(true)),bucketAuto("price",5)).as("categorizedByPrice"));
facet(match.where("country").exists(true)),soryByCount("country")).as("categorizedByCountry"));

facet(project("title").and("publiccationDate").extractYear().as("publiccationYear"),
     bucketAuto("publiccationYear",5).andOutput("title").push().as("titles")
      .as("categorizedByYear"));
```

##### 按计数排序

示例：按计数排序

```java
sortByCount("country");
```

按计数排序操作等效于以下JSON

```java
{$group:{_id:<表达式>,count:{$sum:1}}},
{$sort:{count:1}}
```

投影表达式中的Spring表达式支持

通过和类的andExpression方法支持在投影表达式中使用SpEL表达式

使用SpEL表达式进行复杂计算

```
1+(q+1)/(q-1)
```

前面的表达式被翻译成下面的投影表达式部分

```java
{
    "$add"[1,{
        "$divide":[{
            "$add":["$q",1]
        },{
            "$subtract":["$q",1]
        }]
    }]
}
```

| a == b   | { $eq : [$a, $b] }       |
| :------- | :----------------------- |
| a != b   | { $ne : [$a , $b] }      |
| a > b    | { $gt : [$a, $b] }       |
| a >= b   | { $gte : [$a, $b] }      |
| a < b    | { $lt : [$a, $b] }       |
| a ⇐ b    | { $lte : [$a, $b] }      |
| a + b    | { $add : [$a, $b] }      |
| a - b    | { $subtract : [$a, $b] } |
| a * b    | { $multiply : [$a, $b] } |
| a / b    | { $divide : [$a, $b] }   |
| a^b      | { $pow : [$a, $b] }      |
| a % b    | { $mod : [$a, $b] }      |
| a && b   | { $and : [$a, $b] }      |
| a \|\| b | { $or : [$a, $b] }       |
| !a       | { $not : [$a] }          |
|          |                          |

出上表中显示转换外   还可以使用标准SpEL操作，例如通过名称创建数据和引用表达式

```
//{$setEquals:[$a,[5,8,13]]}
.andExpression("setEquals(a,new int[]{5,8,13})");
```

##### 聚合框架示例

希望聚合一个标签列表，以MongoDB集合中获取特定标签的出现次数，并按出现次数降序排序

分组、排序‘、投影、展开的用法

```java
class TagCount{
    String tag;
    int n;
}
```

```java
Aggregation agg = new Aggregation(
	poject("tags"),
    unwind("tags"),
    group("tags").count().as("n"),
    project("n").and("tags").previousOperation(),
    sort(DESC,"n")
);
AggregationResults<TagCount> results = mongoTemplate.aggregate(agg,"tags",TagCount.class);
List<TagCount> tagCount = results.getMappedResults();
```



此例添加额外的排序

```java
class ZipInfo{
    String id;
    String city;
    String state;
    @Field("pop") int population;
    @Field("loc") double[] location;
}
class City{
    String name;
    int population;
   
}
class ZipInfoStats{
    String id;
    String state;
    City biggestCiy;
    City smallestCity;
}
```

```java
public class test {

    TypedAggregation<ZipInfo> aggregation = new Aggregation(ZipInfo.class,
            group("state","city")
            .sum("population").as("pop"),
    sort(Sort.Direction.ASC,"pop","state","city"),
            group("state")
                    .last("city").as("biggestCity")
                    .last("pop").as("biggestPop")
                    .last("city").as("smallestCity")
                    .last("pop").as("smallestPop"),
            project()
                    .and("state").previousOperation()
                    .and("biggestCity")
                    .nested(bind("name","biggestCity").and("population","biggestPop"))
                    .and("smallestCity")
                            .nested(bind("name","biggestCity").and("population","biggestPop"))
                    .and("smallestCity")
                    .nested(bind("name","biggestCity").and("population","biggestPop"))
                    .and("smallestCity")
                    .nested(bind("name","smallestCity").and("population","smallestPop")),
            sort(Sort.Direction.ASC,"state")
    );
    
    AggregationResults<ZipInfoStats> result = mongoTemplate.aggregate(aggregation,ZipInfoStats.class);
    ZipInfoStats firstZipInfoStats = result.getMappedResults().get(0);
}
```

示例3

```java
public class StateStats {
    @Id String id;
    String state;
    @Field("totalPop") int totalPopulation;
}

```

```java
public class test {

    TypedAggregation<ZipInfo> agg = new Aggregation(ZipInfo.class,
            group("state").sum("population").as("totalPop"),
            sort(Sort.Direction.ASC,previousOperation(),"totalPop"),
            match(where("totalPop").get(10*1000*1000))
    );
    AggregationResults<StateStats> result = mongoTemplate.aggregate(agg,StateStats.class);
    List<StateStats> stateStatsList = result.getMappedResults();
}
```

示例4

```java
public class Product {
    String id;
    String name;
    double netPrice;
    int SpaceUnits;
}
```

```java
public class test {
    TypedAggregation<Product> agg = Aggregation.newAggregation(Product.class, 
            project("name","netPrice")
            .and("netPrice").plus(1).as("netPricePlus1")
            .and("netPrice").minus(1).as("netPriceMinus1")
            .and("netPrice").multiply(1.19).as("grossPrice")
            .and("netPrice").divide(2).as("netPriceDiv2")
            .and("spaceUnits").mod(2).as("spaceUnitMod2")
        );
    AggregationResults<Document> result = mongoTemplate.aggregate(agg,Document.class);
    List<Document> resultList = result.getMappedResults();
}
```

示例5

```java
class Product {
    String id;
    String name;
    double netPrice;
    int spaceUnits;
}
```

```java
public class test1 {
    TypedAggregation<Product> agg = Aggregation.newAggregation(Product.class,
            Aggregation.project("name","netPrice")
                    .andExpression("newPrice+1").as("newPricePlus1")
                    .andExpression("netPrice-1").as("newPricePlus1")
                    .andExpression("netPrice/2").as("netPriceDiv2")
                    .andExpression("netPrice*1.19").as("grossPrice")
                    .andExpression("spaceUnits%2").as("spaceUnitsMod2")
                    .andExpression("(netPrice*0.8+1.2)*1.19)").as("grossPriceIncludingDiscountAndCharge")
                    
    );
    AggregationResults<Document> result = mongoTemplate.aggregate(agg,Document.class);
    List<Document> resultList = result.getMappedResults();
}
```

示例6

```java
class Product {
    String id;
    String name;
    double netPrice;
    int spaceUnits;
}
```

```java
public class test3 {
    double shippingCosts = 1.2;
    TypedAggregation<Product> agg = Aggregation.newAggregation(Product.class,
            Aggregation.project("name","netPrice")
                    .andExpression("(netPrice*(1-discountRate)+[0])*(1+taxRate)", shippingCosts).as("salesPrice")
    );
    AggregationResults<Document> result = mongoTemplate.aggregate(agg,Document.class);
    List<Document> resultList = result.getMappedResults();
}
```

示例7

```java
public class InventoryItem {

  @Id int id;
  String item;
  String description;
  int qty;
}
```

```java
public class InventoryItemProjection {

  @Id int id;
  String item;
  String description;
  int qty;
  int discount
}
```

```java
public class test {
    TypedAggregation<InventoryItem> agg = Aggregation.newAggregation(InventoryItem.class,
            Aggregation.project("item").and("discount")
                    .applyCondition(ConditionalOperators.when(Criteria.where("qty").gte(250))
                    .then(30)
                    .otherwise(20))
                    .and(ifNull("description","Unspecified")).as("description")
    );
    AggregationResults<InventoryItemProjection> result = mongoTempalte.aggregate(agg,"inventory",InventoryItem.class);
    List<InventoryItemProjection> stateStatsList = result.getMappedResults();
}

```

示例：条件聚合投影

```java
TypedAggregation<Book> agg = Aggregation.newAggregation(Book.class,
  project("title")
	.and(ConditionalOperators.when(ComparisonOperators.valueOf("author.middle")//如果字段的值author.middle
     	.equalToValue(""))//不包含值
        .then("$$REMOVE")//用于$$REMOVE排除该字段
        .otherwiseValueOf("author.middle")//添加的字段值author.middle
        )
         .as("author.middle"));
```

### 11.13 索引和馆藏管理

显示了IndexOperations界面

```java
public interface IndexOperations{
    void ensureIndex(IndexDefinition indexDefinition);
    void dropIndex(String name);
    void dropAllIndexes();
    void resetIndexCache();
    List<IndexInfo> getIndexInfo();
}
```

创建索引的方法

```java
mongoTemplate.indexOps(Person.class).ensureIndex(new Index().on("name",Order.ASCENDING));
```

ensureIndex确保为集合存在提供的IndexDefinition索引

给定Venue定义的类，声明一个地理空间查询如下L

```java
mongoTemplate.indexOps(Venue.class).ensureIndex(new GeospatialIndex("location"));
```

#### 访问索引信息

IndexOperations接口具有geiIndexInfo返回IndexInfo对象列表的方法此列表包含在集合上定义的所有索引。

Person具有age属性的类上定义索引：

```java
template.indexOps(Person.class).ensureIndex(new Index().on("age",Order.SESCENDING).unique());
List<IndexInfo> indexInfoList = template.indexOps(Person.class).getIndexInfo();
```

##### 处理集合的方法

示例：使用集合处理集合

```java
MongoCollection<Document> collection = null;
if(!mongoTemplate.getCollectionNames().contains("MyNewCollection")){
    collection = monfoTemplate.createCollection("MyNewCollection");
}
mongoTemplate.dropCollection("MyNewCollection");
```

getCollectionNames：返回一组集合名称

collectionExists：检查是否存在具有给定名称的集合

createCollection：创建一个无上限的集合

dropCollection：删除集合

getCollection：按名称获取一个集合，如果不存在就创建它

### 11.14 执行命令

MongoDatabase.runCommand()上的executeCommand()方法获取MongoDB驱动程序的方法MongoTemplate。还将异常转换为Spring的DataAccessException层次结构

#### 运行命令的方法

executeCommand(Document command)：运行MongoDB命令

executeCommand(Document command,ReadPreference readPreference)使用给定的可为空的MongoDB运行MongoDB命令

executeCommand(String jsonCommand)：运行以JSON字符串表示的MongoDB命令

### 11.15 声明周期事件

要在对象通过转换过程之前拦截对象，可以注册一个AbsrtactMongoEventListener覆盖该onBeforeConvert方法的子类

```java
public class BeforeConvertListener extends AbstractMongoEventListener<Person>{
    @Override
    public void onBeforeConvert(BeforeConvertEvent<Person> event){
        ==
    }
}
```

要在对象进入数据库之前对其进行拦截，可以诸葛一个AbstracrMongoEventListener覆盖该onBeforeSave方法的子类。当事件被调度时，监听器被调用并传递域对象和转换后的com.mongodb.Document

```java
public class BeforeSaveListener extends AbstractMongoEventListener<Person>{
    @Override
    public void onBeforeSave(BeforeSaveEvent<Person> event){
        ==
    }
}
```

### 11.16 实体回调

Spring  Data基础提供了在调用某些方法之前和之后修改实体的gouzi

#### 实现实体回调

示例：解剖entityCallback

```java
@FunctionalInterface
public interface BeforeSaveCallback<T> entends EntityCallback<T>{
    onBeforeSave(T entity<2>,String collection<3>);
}
```

示例：反应式的解剖

```java
@FunctionalInterface
public interface ReactiveBeforeSaveCallback<T> extends EntityCallback<T>{
    Publisher<T> onBeforeSave(T entity<2>,String collection<3>);
}
```

示例：BeforeSaveCallback

```java
class DefaultingEntityCallback implements BeforeSaveCallback<Person>,Ordered{
    @Override
    public Object onBeforeSave(Person entity,String collection){
        if(collection == "user"){
            return //
        }
        return //
    }
    @Override
    public int getOrder(){
        return 100;
    }
}
```

#### 注册实体回调

大多数API已经实现ApplicationContextAware，因此可以访问ApplicationContext

示例：EntityCallback  Bean注册示例

```java
@Order(1)
@Component
class First implements BeforeSaveCallback<Person>{
    @Override
    public Person onBeforeSave(Person person){
        return //
    }
}
@Compoent
class DefaultingEntityCallback implements BeforeSaveCallback<Person>,Ordered{
    @Override
    public Object onBeforeSave(Person entity,String cllention){
       	//
    }
    @Override
    public int getOrder(){
        return 100;
    }
}
@Configuration
public class EntityCallbackConfiguration{
    @Bean
    BeforeSaveCallback<Person> unorderedLambdaReceiverCallback(){
        return (BeforeSaveCallback<person>) it ->//
    }
}
@Component
class UserCallbacks implements BeforeConvertCallback<User>,BeforeSaveCallback<User>{
    @Override
    public Person onBeforeConvert(User user){
        return //
    }
    @Override
    public Person onBeforeSave(User user){
        return //
    }
}
```

BeforeSaveCallback从@Order注释中接收其命令

BeforeSaveCallback通过Ordered接口实现接手其订单

BeforeSaveCallback使用lambda表达式

在单个实现类中组合多个实体回调接口

存储特定的EntityCallbacks

| Callback                        | Method                                                       | Description                                                  | Order                       |
| :------------------------------ | :----------------------------------------------------------- | :----------------------------------------------------------- | :-------------------------- |
| Reactive/BeforeConvertCallback  | `onBeforeConvert(T entity, String collection)`               | Invoked before a domain object is converted to `org.bson.Document`. | `Ordered.LOWEST_PRECEDENCE` |
| Reactive/AfterConvertCallback   | `onAfterConvert(T entity, org.bson.Document target, String collection)` | Invoked after a domain object is loaded. Can modify the domain object after reading it from a `org.bson.Document`. | `Ordered.LOWEST_PRECEDENCE` |
| Reactive/AuditingEntityCallback | `onBeforeConvert(Object entity, String collection)`          | Marks an auditable entity *created* or *modified*            | 100                         |
| Reactive/BeforeSaveCallback     | `onBeforeSave(T entity, org.bson.Document target, String collection)` | Invoked before a domain object is saved. Can modify the target, to be persisted, `Document` containing all mapped entity information. | `Ordered.LOWEST_PRECEDENCE` |
| Reactive/AfterSaveCallback      | `onAfterSave(T entity, org.bson.Document target, String collection)` | Invoked before a domain object is saved. Can modify the domain object, to be returned after save, `Document` containing all mapped entity information. | `Ordered.LOWEST_PRECEDENCE` |

### 11.17 异常翻译

Spring框架为各种数据库和映射计数提供异常转换、

映射到Spring一致的数据访问异常层次结构背后的动机是，可以编写可移植和描述性的异常处理代码，无需针对MongoDB错误代码进行编码

### 11.18 执行回调

execute回调方法

<T> T execute(Class<?> entityClass,CollectionCallback<T> action):CollectionCallback：为指定类的实体集合运行给定

<T> T execute(String collectionName,CollectionCallback<T> action):在给CollectionCallback定名称的集合上运行给定

<T> T execute(DbCallback<T> action):运行DbCallback，根据需要翻译任何异常

<T> T execute(String collectionName,DbCallback<T> action):DbCallback在给定名称的集合上运行，根据需要翻译任何异常

<T> T executeInSession(DbCallback<T> action):DbCallback在与数据库的同一连接中运行给定的，以确保在写入繁重的环境中的一致性

示例:使用CollectionCallback返回有关索引的信息

```java
boolean hasIndex = tempalte.execute("geolocation",new CollectionCallbackBoolean>(){
    public Boolean doInCollection(Venue.class,DBCollection collection)throws MongoException,DataAccessException{
       	List<Document> indexes = collection.getIndexInfo();
        for(Document document:indexes){
            if("location_2d".equals(document.get("name"))){
                return true;
            }
        }
        return false;
    }
});
```

### 11.19 GridFS支持

MongoDB支持在其文件系统GridFS中存储二进制文件

示例：GridFSTemplate的JavaConfig设置

```java
class GridFsConfiguration extends AbstractMongoClientConfiguraion{
    @Bean
    public GridFsTemplate gridFsTempalte(){
        return new GridFsTemplate(mongoDbFactory(),mappingMongoConverter());
    }
}
```

对应的XML配置如下

示例:GridFsTemplate的XML配置

````java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:mongo="http://www.springframework.org/schema/data/mongo"
  xsi:schemaLocation="http://www.springframework.org/schema/data/mongo
                      https://www.springframework.org/schema/data/mongo/spring-mongo.xsd
                      http://www.springframework.org/schema/beans
                      https://www.springframework.org/schema/beans/spring-beans.xsd">

  <mongo:db-factory id="mongoDbFactory" dbname="database" />
  <mongo:mapping-converter id="converter" />

  <bean class="org.springframework.data.mongodb.gridfs.GridFsTemplate">
    <constructor-arg ref="mongoDbFactory" />
    <constructor-arg ref="converter" />
  </bean>

</beans>
````

示例：使用gridFsTemplate存储文件

```java
class GridFsClient{
    @Autowired
    GridFsOperations operations;
    @Test
    public void storeFileToGridFs(){
        FileMetadata metadata = new FileMetadata();
        Resource file = ;
        operations.store(file.getInputStream(),"filename.txt",metadata);
    }
}
```

示例：使用GridFsTemplate查询文件

```java
class GridFsClient{
    @Autowired
    GridFsOperations operations;
    @Test
    public void findFilesInGridFs(){
        GridFSFindIterable result = operations.find(query(whereFilename().is("filename.txt")))
    }
}
```

示例：使用GridFsTemplate读取文件

```java
class GridFsClient{
    @Autowired
    GridFsOperation operations;
    @Test
    public void readFilesFromGrids(){
        GridFsResources[] txtFiles = operations.getResources("*.txt");
    }
}
```

### 11.20带有可尾游标的无限流

默认情况下，当客户端耗尽游标提供的所有结果时，MongoDB会自动关闭游标。

可以使用MongoOperations.createCollection

Tailable游标可以与命令式和反应式MongoDB API一起使用。

#### tailable Cursors with MessageListener

示例:带有MessageListener实例的可尾游标

```java
MessageListenerContainer container = new DefaultMessageListenerContainer(template);
container.start();
MessageListener<Document,User> listener = System.ou::println;

TailableCursorRequest request = TailableCursorRequest.builder()
    .collection("orders")
    .filter(query(where("value").1t(100)))
    .publishTo(listener)
    .build();
container.register(request,User.class);
container.stop();
```

启动容器会初始化资源并Task为已注册的SubscriptionRequest实例启动实例

定义在Message接收到a时调用的侦听器。

设置要收听的集合

为要接收的文档提供可选过滤器

设置消息侦听器以将传入的Message发布

注册请求。返回的Subscription可用于检查当前Task状态并取消它以释放资源

一旦确定不需要容器，不要忘记停止容器。

#### 反应式可尾游标

示例：使用ReactiveMongoOperations的无限流查询

```java
Flux<Person> stream = template.tail(query(where("name").is("Joe")),Person.class);
Disposable subscription = stream.doOnnext(person -> System.out.println(person)).subscribe();
subscription.dispose();
```

示例：使用ReactiveMongoRepository进行无限流查询

```java
public interface PersonRespotory extends ReactiveMongoRepository<Person,String>{
    @Tailable
    Flux<Person> findByFirstname(String firstname);
}
Flux<Person> stream = repository.findByFirstname("Joe");
Disposable subscription = stream.doOnNext(System.out.println),subsribe();
subscription.dispose();
```

### 11.21 更改流

Change Stream支持仅适用于副本集或分片集群

#### 更改流MessageListener

示例：使用MessageListener实例更改流

```java
MessageListenerContainer container = new DefaultMessageListenerContainer(template);
container.start();

MessageListener<ChangeStreamDocument<Document>,User> listener = System.out::println;
ChangeStreamRequestOptions options = new ChangeStreamrequestOptions("user",ChangeStreamOptions.empty());

Subscription subscription = container.register(new ChangeStreamRequest<>(Listener,options),User.class);
container.stop();
```

#### 反应性变化流

示例：更改流发射ChangeStreamEvent

```java
Flux<ChangeStreamEvent<User>> flux = reactiveTemplate.changeStream(User.class)
    .watchCollection("people")
    .filter(where("age").gte(38))
    .listen();
```

#### 恢复更改流

示例：恢复变更流

```java
Flux<ChangeStreamEvent<User>> resumed = template.changeStream(User.class)
    .watchCollection("people")
    .resumeAt(Instant.now().minusSeconds(1))
    .listen();
```

## 12.MongoDB会话

会话的使用启用了MongoDB的因果一致性模型，该模型保证以尊重其因果关系的顺序运行操作

MongoOperations 和ReactiveMongoOperations 提供捆绑的网关方法ClientSession来操作。MongoCollection和MongoDatabase使用实现MongoDB的集合和数据库接口的会话代理对象

### 12.1 同步ClientSession支持

示例：ClientSession和MongoOperations

```java
ClientSessionOptions sessionOptions = ClientSessionOptions.builder()
    .causallyConsistent(true)
    .build);

ClientSession session = client.startSession(sessionOptions);
template.withSession(() -> session)
    .execute(action -> {
        Query query = query(where("name").is("Durzo Blint"));
        Person durzo = action.findOne(query,Person.class);
        
        Person azoth = new Person("Kylar Stern");
        azoth.setMaster(durzo);
        
        action.insert(azoth);
        
        return azoth;
    });
session.close()
```

从服务器获取新会话

MongoOperation像以前一样使用用法，在ClientSession被自动应用

确保关闭ClientSession

关闭会话

### 12.2 反应式ClientSession支持

示例 ClientSession与ReactiveMongoOperations

```java
ClientSessionOptions sessionOptions = ClientSessionOptions.builder()
    .causallyConsistent(true)
    .build();
Publisher<clientSession> session = client.startSession(sessionOptions);
template.withSession(session)
    .execute(action -> {
        Query query = query(where("name").is("Durzo Blint"));
        return action.findOne(query,Person.class)
            .flatMap(durzo -> {
                Person azoth = new Person("Kylar Stern");
                azoth.setMaster(durzo);
                
                return action.insert(azoth);
            });
    },ClientSession::close)
    .subscribe();
```

## 13.MongoDB事务

示例：程序化事务

```java
ClientSession session = client.startSession(options);
template.withSession(session)
    .execute(action -> {
        session.startTransaction();
        try{
            Step step = ;//
            action.insert(step);
            
            process(step);
            
            action.update(Step.class).apply(Update.set("state",**));
            
            session.commitTranSaction();
            
        }catch(RuntimeException e){
            session.abortRransaction();
        }
    },ClientSession::close)
```

### 13.1 TransactionTemplate

示例TransactionTemplate

```java
template.setSessionSynchronization(ALWAYS);

TransactionTemplate txTemplate = new TransactionTemplate(anyTxManager);
txTemplate.execute(new TransactionCallbackWithoutResult){
    @Override
    protected void doInTranSactonWithoutResult(TransactionStatus status){
        Step step = ;//
        template.insert(step);
        
        process(step);
        
        template.update(Step.class).apply(Update.set("state",**);//
        
    };
});
```

在模板API配置期间启用事务同步

创建TransactionTemplate使用所提供的PlatformTransactionManager

在回调中ClientSession，交易已经注册

### 13.2 MongoTransactionManager

MongoTransaction是通向众所周知的Spring事务支持的门户.允许应用程序使用Spring的托管事务功能。在MongoTransactionManager绑定ClientSession到线程

示例：如何创建和使用带有的事务MongoTransactionManager

```java
@Configuration
static class Config extends AbstractMongoClientConfiguration{
    @Bean
    MongoTransactionManager transactionManager(MongoDatabaseFactory dbFactory){
        return new MongoTransactionManager(dbFactory);
    }
}
@Component
public class StateService{
    @Transactional
    void someBusinessFunction(Step step){
        template.insert(step);
        process(step);
        template.update(Step.class).apply(Update.set("state",***)
    };
});
```

1.MongoTransactionManage在应用程序上下文中注册

2.将方法标记为事务性的

### 13.3 反应式事务

与反应式ClientSession支持相同，它ReactiveMongoTemplate提供了在事务内操作的专用方法，而不必担心根据操作结果或停止操作

示例：本机驱动程序支持

```java
Mono<DeleteResult> result = Mono.from(client.startSession())
    .flatMap(session -> {
        session.startTransaction();
        
        return Mono.from(collection.deleteMany(session,***))
            .onErrorResume(e -> Mono.from(session.abortTransaction().then(Mono.error(e))))
            .flatMap(val -> Mono.from(session.commitTransaction()).then(Mongo.just(val)))
            .doFinally(signal -> session.close());
    })
```

### 13.4 TransactionalOperator

示例：TransactionalOperator

```java
template.setSessionSynchronization(ALWAYS);
TransactionOperator rxtx = TransactionalOperator.create(anyTxManager,
                                                       new DefaultTransactionDefinition());

Step step = ;//
template.insert(step);
Mono<void> process(step)
    .then(template.update(Step.class).apply(Update.set("state",**)))
    .as(rxtx.transactional)
    .then();
```

### 13.5 ReactiveMongoTransactionManager

示例：ReactiveMongoTransactionManager

```java
@Configuration
static class Config extends AbstractMongoClientConfiguration{
    @Bean
    ReactiveMongoTransactionManager transactionManager(ReactiveDatabaseFactory factory){
        return new ReactiveMongoTransactionManager(factory);
    }
}
@Service
public class StateService{
    @Transactional
    Mono<UpdateResult> someBusinessFunction(Step step){
        return template.insert(step)
            .then(process(step))
            .then(template.update(Step.class).apply(Update.set("state",**));
    };
});
```

### 13.6 交易内部的特殊行为

连接设置

MongoDB驱动程序提供了一个专用的副本集名称配置选项，将驱动程序转换为自动检测模式

集合操作

MongoDB不支持收集操作

瞬态误差

MongoDB可以为事务操作期间引发的错误添加特殊标签。这些可能表示可能通过重试操作而消失的瞬时故障

数数

MongoDB   count根据收集统计信息进行操作，这些统计信息可能无法反映事务中的实际情况。

在聚合计数助手中使用地理命令时存在限制。不能用以下运算符，必须用不同的运算符替换

$where -> $expr

$near -> $geoWithin  /  $center

$nearSphere -> $geoWithin   /  $centerSphere

显示count会话绑定闭包中的用法

```java
session.startTransaction();
tempalte.withSession(session)
    ,execute(action -> {
        action.count(query(where("state").is("active")),Step.class);
    })
```

命令中具体化 如下

```java
db.collection.aggregate(
[
    {$match:{state:"active"}},
    {$count:"totalEntityCountq"}
])
```

代替

```java
db.colleciton.find({state:"active"}).count()
```

## 14.反应式MongoDB 支持

### 14.1 入门

安装后，启动 MongoDB 通常只需运行以下命令：`${MONGO_HOME}/bin/mongod`

添加到pom.xml部分

```java
<dependencies>

  <!-- other dependency elements omitted -->

  <dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-mongodb</artifactId>
    <version>3.2.5</version>
  </dependency>

  <dependency>
    <groupId>org.mongodb</groupId>
    <artifactId>mongodb-driver-reactivestreams</artifactId>
    <version>4.2.3</version>
  </dependency>

  <dependency>
    <groupId>io.projectreactor</groupId>
    <artifactId>reactor-core</artifactId>
    <version>2020.0.11</version>
  </dependency>

</dependencies>
```

Person类持久化

```java
public class Person {
    private String id;
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "id='" + id + '\'' +
                ", name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}

```

创建一个应用程序，如下：

```java
public class Demo1Application {
    private static final Logger log = LoggerFactory.getLogger(Demo1Application.class);

    public static void main(String[] args) throws Exception{
        CountDownLatch latch = new CountDownLatch(1);
        ReactiveMongoTemplate mongoOps = new ReactiveMongoTemplate(MongoClients.create(),"database");

        mongoOps.insert(new Person("Joe",34))
                .flatMap(p -> mongoOps.findOne(new Query(where("name")),Person.class))
                .doOnNext(person -> log.info(person.toString()))
                .flatMap(person -> mongoOps.dropCollection("person"))
                .subscribe();
        
        latch.await();
    }

}

```

### 14.2 使用Spring和Reactive Streams驱动程序连接到MongoDB

#### 14.2.1 使用基于Java的元数据注册MongoClient实例

示例：com.mongodb.reactivestreams.client.MongoClient使用基于Java的bean元数据注册对象

```java
@Configuration
public class AppConfig{
    public @Bean MongoClient reactiveMongoClient(){
        return MongoClients.create("mongodb://localhost");
    }
}
```

另一种方法是使用Spring的ReactiveMongoClientFactoryBean，与com.mongodb.reactivestreams.client.MongoClient直接实例化相比，Factory方法具有额外的优势，即还为容器提供一个ExceptionTranslation实现，该实现MongoDB异常转化为Spring的可移植DataAcccessException层次结构中的异常，用于使用注释注释的数据访问类@Respository

示例 :com.mongodb.reativestreams.client.MongoClient使用Spring的MongoClientFactroyBean注册对象启用Spring的异常转换支持

```java
@Configuration
public class AppConfig{
    public @Bean ReactiveMongoClientFactoryBean mongoClient(){
        ReactiveMongoClientFactoryBean clientFactory = new ReactiveMongoClientFactoryBean();
        clientFactory.setHost("localhost");
        
        return clientFactory;
    }
}
```

#### 14.2.2 ReactiveMongoDatabaseFactory接口

虽然com.mongodb.reativestreams.client.MongoClient是反应式MongoDB驱动程序API的入口点，但连接到特定的MongoDB数据库实例需要其他信息

实例:显示一个ReactiveMongoDatabaseFactory界面

```java
public interface ReactiveMongoDatabaseFactory{
    /*
    Creates a default {@Link MongoDatabase} instance,
    
    @return
    @throws DataAccessException
    */
    MongoDatabase getMongoDatabase() throws DataAccessException;
    /*
    Creates a {Link MongoDatabase} instance to access the database with the given name
    @param dbName must not be {@Literal null} or empty
    @return
    @throws DataAccessExcetpion
    */
    MongoDatabase getMongoDatabase(String dbName) throws DataAccessException;
    /*
    Exposes a shared {@Link MongoExceptionTranslator}
    @return will never be{@Literal null}
    */
    PersistenceExceptionTraslator getExceptionTranslator();
}
```

ReacitveMongoTemplate可以在标准Java代码中使用它们，而不是使用IoC容器来创建的实例，如下

```java
public class MongoApp {
    private static final Log log = LogFactory.getLog(MongoApp.class);

    public static void main(String[] args) throws Exception{
        ReactiveMongoTemplate mongoOps = new ReactiveMongoTemplate(new SimpleReactiveMongoDatabaseFactory(MongoClients.create(),"database"));
        
        mongoOps.insert(new Person("Joe",34))
                .flatMap(p -> mongoOps.findOne(new Query(where("name").is("Joe")),Person.class))
                .doOnNext(person -> log.info(person.toString()))
    
                .flatMap(person -> mongoOps.dropCollection("person"))
                .subscribe();
}

```

#### 14.2.3 使用基于Java元数据注册ReactiveMongoDatabaseFactory实例

向ReactiveMongoDatabaseFactory容器注册实例

```java
@Configuration
public class MongoConfiguration{
    public @Bean ReactiveMongoDatabaseFactory reactiveMongoDatabaseFactory(){
        return new SimpleReactiveMongoDatabaseFactory(MongoClients,create(),"database");
    }
}
```

要定义用户名和密码，请创建一个MongoDB连接字符串并将其传递给Factory方法，如下：

显示了如何使用ReactiveMongoDatabaseFactory向ReactiveMongoTemplate容器注册实例

```java
@Configuration
public class MongoConfiguration {
    public @Bean
    ReactiveMongoDatabaseFactory reactiveMongoDatabaseFactory(){
        return new SimpleReactiveMongoDatabaseFactory(MongoClients.create("mongodb://joe:secret@localhost"),"database");
        
    }
    public @Bean
    ReactiveMongoTemplate reactiveMongoTemplate(){
        return new ReactiveMongoTemplate(reactiveMongoDatabaseFactory());
    }
}

```

### 14.3 简介ReactiveMongoTemplate

ReactiveMongoTemplate  -> org.springframework.data.mongodb包，是中央级的Spring的反应MongoDB支持，提供了丰富的功能集和数据库进行交互。

该模板提供了创建、更新、删除和查询MongoDB文档的快捷操作，并提供了 域对象和MongoDb文档之间的映射

配置后，ReactiveMongoTemplate就是线程安全的，可以跨多个实例重复使用

MongoDB文档和域类之间的映射是通过委托给MongoConverter接口的实现来完成的

引用ReactiveMongoTemplate实例操作的首选方法是通过其ReactiveMongoOperations接口

#### 14.3.1 实例化ReactiveMongoTemplate

示例：创建一个com.mongodb.reactivestreams.client.MongoClient对象并启用Spring的异常转换支持

```java
@Configuration
public class AppConfig {
    public @Bean
    MongoClient reactiveMongoClient(){
        return MongoClients.create("mongodb://localhost");
    }
    
    public @Bean
    ReactiveMongoTemplate reactiveMongoTemplate(){
        return new ReactiveMongoTemplate(reactiveMongoClient(),"mydatabase");
    }
}
```

创建ReactiveMongoTemplate时，可以设置以下属性：

WriteResultCheckingPolicy

WriteConcern

ReadPreference

引用ReactiveMongoTempalate实例操作的首选方法时通过其ReactiveMongoOperations接口

#### 14.3.2 WriteResultChecking

属性--LOG、ExCEPTION、或者NONE记录错误、抛出和异常或不执行任何操作。默认值时使用的NONE

#### 14.3.3 WriteConcern

若WriteConcern未设置ReactiveMongoTemplate的属性，则默认为MongoDB驱动程序mongoDatabase或MongoCollection设置中设置的属性

#### 14.3.4 WriteConcernResolver

若希望在WriteConcern在每个操作的基础上设置不同的值，WriteConcernResolver可以在ReactiveMongoTemplate

由于ReactiveMongoTemplate用于持久化POJO，因此WriteConcernResolver可以创建一个策略，将特定的POJO类映射到一个WriteConcern值

```java
public interface WriteConcernResolver{
    WriteConcern resolver(MongoAction action);
    
}
```

以下示例显示如何创建一个WriteConcernResolver

```java
private class MyAppWriteConcernResolver implements WriteConcernResolver{
    public WriteConcern resolver(MongoAction action){
        if(action.getEntityClass().getSimpleName().contains("Audit")){
            return WriteConcern.NONE;
        }else if(action.getEntityClass().getSimpleName().contains("Metadata")){
            return WriteConcern.JOURNAL_SAFE;
        }
        return action.getDefaultWriteConcern();
    }
}
```

### 14.4 保存、更新和删除文档

```java
public class Person {
    private String id;
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "id='" + id + '\'' +
                ", name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

以下显示如何保存、更新和删除Person对象、

```java
public class ReactiveMongoApp {
    private static final Logger log = LoggerFactory.getLogger(ReactiveMongoApp.class);

    public static void main(String[] args) throws Exception{
        CountDownLatch latch = new CountDownLatch(1);
        ReactiveMongoTemplate mongoOps = new ReactiveMongoTemplate(MongoClients.create(),"database");

        mongoOps.insert(new Person("Joe",34))
                .doOnNext(person -> log.info("Insert: "+person))
                .flatMap(person -> mongoOps.findById(person.getId(),Person.class))
                .doOnNext(person -> log.info("Found: "+person))
                .zipWith(person -> mongoOps.updateFirst(Query.query(where("name").is("Joe")),update("age",35),Person.class))
                .flatMap(tuple -> mongoOps.remove(tuple.getT1()))
                .flatMap(deleteResult -> mongoOps.findAll(Person.class))
                .count()
                .doOnSuccess(count -> {
                    log.info("Number of people: "+count);
                    latch.countDown();
                })
                .subscribe();
        latch.await();
    }
}

```

### 14.5 执行回调

所有Spring模板类的一个共同设计特征是所有功能都路由到运行回调方法的模板之一。有助于确保异常和可能需要的任何资源管理执行的一致性。

示例：使用ReactiveCollectionCallback 返回有关索引的信息

```java
Flux<Boolean> hasIndex = operations.execute("geolocation,
                                            collection -> Flux.from(collection.listIndexes(Document.class))
                                            .filter(document->document.get("name").equals("fancy-index-name"))
                                            .flatMAp(document->Mono.just(true))
                                            .defaultIfEmpty(false)
                                            ");
```

### 14.6 GridFS 支持

MongoDB支持在其文件系统GridFS中存储二进制文件。Spring Data MongoDB 提供了一个ReactiveGridFsOperations接口以及相应的实现，ReactiveGridFsTemplate与文件系统进行交互

示例：ReactiveGridFsTemplate的JavaConfig设置

```java
class GridFsConfiguration extends AbstractReactiveMongoConfiguration{
    @Bean
    public ReactiveGridFsTemplate reactiveGridFsTemplate(){
        return new ReactiveGridFsTemplate(rectiveMongoDbFactory(),mappingMongoConverter());
    }
}
```

示例：使用ReactiveGridFsTemplate来存储文件

```java
class ReactiveGridFsClient{
    @Autowired
    ReactiveGridFsTemplate operations;
    @Test
    public Mono<ObjectId> storeFileToGridFs(){
        FileMetadata metadata = new FileMetadata();
        Publisher<DataBuffer> file = ;//
        return operations.store(file,"filename.txt",metadata);
    }
}
```

MongoDB的驱动程序使用AsyncInputStream和AsyncOutputStream接口来交换二进制流

示例：ReactiveGridFsTemplate查询文件

```java
class ReactiveGridFsClient{
	@Autowired
    ReactiveGridFsTemplate operations;
    
    @Test
    public Flux<GridFSFile> findFilesInGridFs(){
        return operations.find(query(whereFilename().is("filename.txt")))
    }
}
```

MongoDB不支持从GridFS检索文件时定义排序条件。

示例：使用ReactiveGridFsTemplate读取文件

```java
class ReactiveGridFsClient{
    @Autowired
    ReactiveGridFsOperations operations;
    @Test
    public void readFilesFromGridFs(){
        Flux<ReactiveGridFsResource> txtFiles = operations.getResource("*.txt");
    }
}
```

## 15 MongoDB存储库

### 15.1 介绍

### 15.2 用法

 要访问在MongoDB中的域实体，要使用复杂的存储库支持。

示例：Person实体

```java
public class Person {
    @Id
    private String id;
    private String firstname;
    private String lastname;
    private IiopUrl.Address address;

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getFirstname() {
        return firstname;
    }

    public void setFirstname(String firstname) {
        this.firstname = firstname;
    }

    public String getLastname() {
        return lastname;
    }

    public void setLastname(String lastname) {
        this.lastname = lastname;
    }

    public IiopUrl.Address getAddress() {
        return address;
    }

    public void setAddress(IiopUrl.Address address) {
        this.address = address;
    }
}

```

定义一个使用它的接口

示例：用于持久化Person实体的基本存储库接口

```java
public interface PersonRepository extends PagingAndSortingRepository<Person,String> {
    
}
```

要开始使用存储库，应该使用@EnableMongoRepositories注释

示例：存储库的Java配置

```java
public class ApplicationConfig extends AbstractMongoClientConfiguration {\

    @Override
    protected String getMappingBasePackage() {
        return "com.oreilly.springdata.mongodb";
    }

    @Override
    protected String getDatabaseName() {
        return "e-store";
    }
}
```

通过MongoDB存储库Spring XML配置

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mongo="http://www.springframework.org/schema/data/mongo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       https://www.springframework.org/schema/beans/spring-beans-3.0.xsd 
       http://www.springframework.org/schema/data/mongo 
       https://www.springframework.org/schema/data/mongo/spring-mongo-1.0.xsd">

    <mongo:mongo-client id="mongoClient"/>
    
    <bean id="mongoTemplate" class="org.springframework.data.mongodb.core.MongoTemplate">
        <constructor-arg ref="mongoClient"/>
        <constructor-arg value="databaseName"/>
    </bean>
    
    <mongo:repositories base-package="com.example.eneity"/>
</beans>
```

示例：对Person实体进行分页访问

```java
@RunWith(SpringRunner.class)
@ContextConfiguration
public class PersonRepositoryTests {
    @Autowired
    PersonRepository repository;
    
    @Test
    public void readsFirstPageCorrectly(){
        Page<Person> persons = repository.findAll(PageRequest.of(0,10));
        assert(persons.isFirst()).isTrue();
    }
}

```

### 15.3 查询方法

示例：带有查询方法的PersonRepository

```java
public interface PersonRepository extends PagingAndSortRepository<Person,String>{
    List<Person> findByLastname(String lastname);
    Page<Person> findByFirstname(String firstname,Pageable pageable);
    Person findByShippingAddresses(Address address);
    Person findFirstByLastname(String lastname);
    Stream<Person> findAllBy();
}
```

查询方法支持的关键字

| `After`                              | `findByBirthdateAfter(Date date)`                            | `{"birthdate" : {"$gt" : date}}`                             |
| ------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `GreaterThan`                        | `findByAgeGreaterThan(int age)`                              | `{"age" : {"$gt" : age}}`                                    |
| `GreaterThanEqual`                   | `findByAgeGreaterThanEqual(int age)`                         | `{"age" : {"$gte" : age}}`                                   |
| `Before`                             | `findByBirthdateBefore(Date date)`                           | `{"birthdate" : {"$lt" : date}}`                             |
| `LessThan`                           | `findByAgeLessThan(int age)`                                 | `{"age" : {"$lt" : age}}`                                    |
| `LessThanEqual`                      | `findByAgeLessThanEqual(int age)`                            | `{"age" : {"$lte" : age}}`                                   |
| `Between`                            | `findByAgeBetween(int from, int to)` `findByAgeBetween(Range<Integer> range)` | `{"age" : {"$gt" : from, "$lt" : to}}` 下/上界 ( `$gt`/ `$gte`& `$lt`/ `$lte`) 根据`Range` |
| `In`                                 | `findByAgeIn(Collection ages)`                               | `{"age" : {"$in" : [ages…]}}`                                |
| `NotIn`                              | `findByAgeNotIn(Collection ages)`                            | `{"age" : {"$nin" : [ages…]}}`                               |
| `IsNotNull`, `NotNull`               | `findByFirstnameNotNull()`                                   | `{"firstname" : {"$ne" : null}}`                             |
| `IsNull`, `Null`                     | `findByFirstnameNull()`                                      | `{"firstname" : null}`                                       |
| `Like`, `StartingWith`, `EndingWith` | `findByFirstnameLike(String name)`                           | `{"firstname" : name} (name as regex)`                       |
| `NotLike`, `IsNotLike`               | `findByFirstnameNotLike(String name)`                        | `{"firstname" : { "$not" : name }} (name as regex)`          |
| `Containing` 在字符串上              | `findByFirstnameContaining(String name)`                     | `{"firstname" : name} (name as regex)`                       |
| `NotContaining` 在字符串上           | `findByFirstnameNotContaining(String name)`                  | `{"firstname" : { "$not" : name}} (name as regex)`           |
| `Containing` 关于收藏                | `findByAddressesContaining(Address address)`                 | `{"addresses" : { "$in" : address}}`                         |
| `NotContaining` 关于收藏             | `findByAddressesNotContaining(Address address)`              | `{"addresses" : { "$not" : { "$in" : address}}}`             |
| `Regex`                              | `findByFirstnameRegex(String firstname)`                     | `{"firstname" : {"$regex" : firstname }}`                    |
| `(No keyword)`                       | `findByFirstname(String name)`                               | `{"firstname" : name}`                                       |
| `Not`                                | `findByFirstnameNot(String name)`                            | `{"firstname" : {"$ne" : name}}`                             |
| `Near`                               | `findByLocationNear(Point point)`                            | `{"location" : {"$near" : [x,y]}}`                           |
| `Near`                               | `findByLocationNear(Point point, Distance max)`              | `{"location" : {"$near" : [x,y], "$maxDistance" : max}}`     |
| `Near`                               | `findByLocationNear(Point point, Distance min, Distance max)` | `{"location" : {"$near" : [x,y], "$minDistance" : min, "$maxDistance" : max}}` |
| `Within`                             | `findByLocationWithin(Circle circle)`                        | `{"location" : {"$geoWithin" : {"$center" : [ [x, y], distance]}}}` |
| `Within`                             | `findByLocationWithin(Box box)`                              | `{"location" : {"$geoWithin" : {"$box" : [ [x1, y1], x2, y2]}}}` |
| `IsTrue`, `True`                     | `findByActiveIsTrue()`                                       | `{"active" : true}`                                          |
| `IsFalse`, `False`                   | `findByActiveIsFalse()`                                      | `{"active" : false}`                                         |
| `Exists`                             | `findByLocationExists(boolean exists)`                       | `{"location" : {"$exists" : exists }}`                       |

#### 15.3.1 存储库删除查询

 示例：Delete...by查询

```java
public interface PersonRepository extends MongoRepository<Person,String>{
    List<Person> deleteByLastname(String lastname);
    Long deletePersonByLastname(String lastname);
    @Nullable
    Person deleteSingByLastname(String lastname);
    Optional<Person> deleteByBirthdate(Date birthday);
}
```

使用返回类型List检索并在实际删除它们之前返回所有匹配的文档

数字返回类型直接删除匹配的文档，返回删除的文档总数

但是域类型结果检索并删除第一个匹配文档

#### 15.3.2 地理空间存储库查询

示例：高级Near查询

```java
public interface PersonRepository extends MongoRepository<Person,String>{
    List<Person> findByLocationNear(Point location,Distance distance);
}
```

示例：使用Distance with Metrics

```java
Point point = new Point(43.7,48.8);
Distance distance = new Distance(200,Metrics.KILOMETERS);
... = repository.findByLocationNear(point,location);
```

使用@GeoSpatialIndexed上的目标属性力量的使用$nearSphere操作

##### 近地查询

Spring Data MongoDb支持geo-near 查询

```java
public interface PersonRepository extends MongoRepository<Person,String>{
    GeoResults<Person> findByLocationNear(Point location);
    GeoResults<Person> findByLocationNear(Point location,Distance distance);
    GeoResults<Person> findByLocationNear(Point location,Distance min,Distance max);
    GeoResults<Person> findByLocationNear(Point location);
}
```

#### 15.3.3 MongoDB基于JSON的查询方法和字段限制

通过向org.springframework.data.mongodb.repository.Query存储库查询方法添加注释

指定要使用的MongoDB JSON查询字符串，而不是从方法名称派生查询

```java
public interface PersonRepository extends MongoRepository<Person,String>{
    @Query("{'firstname'：？0}")
    List<Person> findByThePersonsFirstname(String firstname);
}
```

String参数值在绑定过程中被转义，意味着不能通过参数添加MongoDB特定的运算符

还可以使用filter属性来限制映射到Java对象的属性集，

```java
public interface PersonRepository extends MongoRepository<Person,String>{
    @Query(value="{'firstname':?}",fields="{'firstname':1,'lastname':1}")
    List<Person> findByThePersonsFirstname(String firstname);
}
```

#### 15.3.4 排序查询方法结果

MongoDB存储库允许使用各种方法来定义排序顺序

```java
public interface PersonRepository entends MongoRepository<Person,String>{
    List<Person> findByFirstnameSortByAgeDesc(String firstname);
    List<Person> findByFirstname(String firstname,Sort sort);
    @Query(sort = "{age:-1}")
    List<Person> findByFirstname(String fitstname);
    @query(sort = "{age:-1}")
    List<Person> findByLastname(String lastname,Sort sort);
}
```

#### 15.3.5 带有SpELl表达式的基于JSON的查询

查询字符串和字段定义可与SpEL表达式一起使用，以在运行时创建动态查询

以下查询用于[0]声明

```java
public interface PersonRepository extends MongoRepository<Person,String>{
    @Query("{'lastname':?#{[0]}}")
    List<Person> findByQueryWithExpression(String param0);
}
```

表达式可用于调用函数、评估条件和构造值

```java
public interface PersonRepository extends MongoRepository<Person,String>{
    @query("{'id':?#{[0] ? {$exists:true}:[1]}}")
    List<Person> findByQueryWithExpressionAndNestedObject(boolean param0,String param1);
}
```

查询字符串中的SpEL可以成为增强查询的强大方法

表达式的支持是可扩展的通过查询SPI：org.springframework.data.repository.query.spi.EvaluationContextExtension。查询SPI可以提供属性和函数，并且可以自定义根对象，在构建查询时，在SpEL评估时从应用程序上下文中检索扩展

```java
public class SampleEvaluationContextExtension extends EvalutaionContextExtensionSupport{
    @Override
    public String getExtensionId(){
        return "security";
    }
    @Override
    public Map<String,Object> getProperties(){
        return Collections.singletonMap("principal",SecurityContextHolder.getCurrent().getPrincipal());
    }
}
```

MongoRepositoryFactory  引导不是应用程序上下文感知的，需要进一步配置以获取查询SPI扩展

#### 15.3.6 类型安全的查询方法

MongoDB存储库支持与Querydsl项目集成，该项目提供了一种执行类型安全查询的方法。

提供以下功能

IDE中的代码完成

**几乎不允许语法无效的查询**

可以安全引用域类型v和属性--不涉及字符串

更好的适应重构域类型的变化

增量查询定义更容易



QueryDSL允许编写如下查询

```java
QPerson person = new QPerson("person");
List<Person> result = repository.findAll(person.address.zipCode.eq("C0123"));

Page<Person> page = repository.findAll(person.lastname.contains("a"),
                                      PageRequest.of(0,2,Direction.ASC,"lastname"));
```

QPerso是Java注解后处理工具生成的一个类

可以Prediccate通过使用QuertdslPredicateExecutor接口来使用生成的类

```java
public interface QuerydslPerdicateExecutor<T>{
    T findOne(Predicate predicate);
    List<T> findAll(Predicate predicate);
    List<T> findAll(Predicate predicate,OrderSpecifier<?> ... orders);
    Page<T> findAll(Predicate predicate,Pageable pageable);
    Long count(Perdicate predicate);
}
```

#### 15.3.7   全文检索查询

MongoDB的全文搜索功能是存储特定的。

@TextScore注释可以按文档的分数进行排序

```java
@Document
class FullTextDocument{
    @Id String id;
    @TextIndexed String title;
    @TextIndexed String content;
    @TextScore Float score;
}
interface FullTextRepository extends Repository<FullTextDocument,String>{
    List<FullTextDocument> findAllBy(TextCriteria criteria,Sort sort);
    Page<FullTextDocument> findAllBy(TextCriteria criteria,Pageable pageable);
    List<FullTextDocument> findByTitleOrderByScoreSesc(String title,Textcriteria criteria);
}
Sort sort = Sort.by("score");
TextCriteria criteria = TextCriteria.forDefaultLanguage().matchingAny("spring","data");
List<FullTextDocument> result = repository.findAllBy(criteria,sort);

criteria = TextCriteria,forDefaulLanguage().matching("film");
Page<FullTextDocument> page = repository.findAllBy(criteria,PageRequest.of(1,1,sort));
List<<fullTextDocument> result = repository.findByTitltOrderByScoreDesc("mongodb",criteria);
```

#### 15.3.8 预测

Spring Data查询方法通常返回存储库管理的聚合根的一个或多个实例

示例：示例聚合和存储库

```java
class Person{
    @Id UUId id;
    String firstname,lastname;
    Address address;
    
    static class Address{
        String zipCode,city,street;
    }
}
interface PersonRepository extends Repository<Person,UUID>{
    Collection<Person> findByLastname(String lastname);
}
```

##### 基于界面的投影

将查询结果限制为仅名称属性的最简单方法是声明一个接口，该接口公开要读取的属性的访问器方法

示例：用于检索属性子集的投影接口

```java
interface NamesOnly{
    String getFirstname();
    String getLastname();
}
```

示例：使用基于接口的投影和查询方法的存储库

```java
interface PersonRepository extends REpository<Person,UUID>{
    Collection<NamesOnly> findByLastname(String lastname);
}
```

投影可以递归使用。

示例：用于检索属性子集的投影接口

```java
interface PersonSummary{
    String getFirstname();
    String getLastname();
    AddressSummary getAddress();
    
    interface AddressSummary{
        String getCity();
    }
}
```

##### 封闭式投影

其访问其方法都与目标聚合的属性匹配的投影接口被认为式封闭投影

示例：闭合投影

```java
interface NamesOnly{
    String getFirstname();
    String getLastname();
}
```

若使用封闭投影，Spring Data可以优化查询执行

##### 打开投影

投影接口中的访问其也可用于通过使用@Value注释计算新值

示例：一个开放的投影

```java
interface NamesOnly{
    @Value("#{target.firstname+' '+target.lastname}")
    String getFullName();
}
```

支持投影的聚合根在target变量中引用。使用的投影界面@value式开放式投影

示例:使用自定义逻辑的默认方法的投影界面

```java
interface NamesOnly{
    String getFirstname();
    String getLastname();
    
    default String getFullName(){
        return getFirstname().concat(" ").concat(getLastname());
    }
}
```

要求能纯粹基于投影接口上公开其他访问其方法来实现逻辑。第二个更灵活的选择式在Spring bean中实现自定义逻辑，然后从SpEL表达式调用

示例：示例Person对象

```java
@Component
class MyBean{
    String getFullName(Person person){
        ...........;
    }
}

interface NamesOnly{
    @Value("#{@myBean.getFullName(target)}")
    String getFullName();
}
```

显示如何从args数组中获取方法参数

示例：示例Person对象

```java
interface NamesOnly{
    @Value("#{args[0]+' '+target.firstname+'!'}")
    String getSalutation(String prefix);
}
```

##### 可空包装器

投影接口中的getter可以使用可为空的包装器来提高空安全性。支持的包装器类型：

java.util.Optional

com.google.common.base.Optional

scala.Option

io.vavr.control.Option

示例：使用可为空包装器的投影接口

```java
interface NamesOnly{
    Optional<String> getFirstname();
}
```

 如果基础投影值不是null，则使用包装器类型的当前表示返回值。如果支持值是null，则getter方法返回包装器类型的空表示

##### 基于类的预测

定义投影的另一种方法是使用值类型DTO，这些DTO包含应该检索的字段的属性。

如果怕存储通过限制要加载的字段来优化查询执行 ，则要加载的字段由公开的构造函数的参数名称确定。

示例：投影DTO

```java
class NamesOnly{
    private final String firstname,lastname;
    NamesOnly(String firstname,String lastname){
        this,firstname = firstname;
        this.lastname = lastname;
    }
    String getFirstname(){
        return this.firstname;
    }
    String getLastname(){
        return this.lastname;
    }
}
```

避免投影DTO的样板代码

可以使用Project Lombok显著简化DTO代码，提供了一个@Value注解。

如果使用Project Lombok的@Value注释，之前示例DTO变为以下内容

```java
@Value
class NamesOnly{
    String firstname,lastname;
}
```

##### 动态投影

要应用动态投影，使用如下实例所示的查询方法

实例：使用动态投影参数的存储库

```java
interface PersonRepository extends Repository<Person,UUID>{
    <T> Collection<T> findByLastname(String lastname,Class<t> type);
}
```

该方法可用于按原样或应用投影获取聚合

示例：使用具有动态投影的存储库

```java
void someMethod(PersonReposiroy people){
    Collection<Person> aggregates = perple.findByLastname("Matthews",Person.class);
    Collection<NamesOnly> aggregates = people.findByLastname("Matthews",NamesOnly.class);
}
```

#### 15.3.9 聚合存储库方法

存储库层提供了通过带注释的存储库查询方法与聚合框架交互的方法。

示例：聚合存储库方法

```java
public interface PersonRepository extends CrudReppsitory<Person,String>{
    List<PersonAggregate> groupByLastnameAndFirstname();
    List<PersonAggregate> groupByLastnameAndFirstnames(Sort sort);
    List<PersonAggregate> groupByLastnameAnd(String property);
    List<PersonAggregate> groupByLastnameAnd(String property,Pageable page);
    SumValue sumAgeUsingValueWrapper();
    Long sumAge();
    AggregationResults<SumValue> sumAgeRaw();
    List<String> findAllLastname();
}
```

```java
public class PersonAggregate{
    private @Id String lastname;
    private List<String> names;
    public PersonAggregate(String lastname,List<String> names){
        888888888;
    }
    //getter and setter
}
public  class SumValue{
    private final Long total;
    public SumValue(Long total){
        ...;
    }
    //Getter omitted
}
```

在某些情况下，聚合可能需要其他选项，使用@Meta注释通过maxExecutionTimeMs、comment或设置这些选项allowDiskUse

```java
public interface PersonRepository extends CrudReppsitory<Person,String>{
    @Meta(allowDiskUse = true)
    @Aggregation("{$group:{_id:$lastname,names:{$addToSet:$firstname}}}")
    List<PersonAggregate> groupByLastnameAndFirstnames();
}
```

或者@Meta创建自己的注释

```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.METHOD})
@Meta(allowDiskUse=true)
@interface AllowDiskUse{}
public interface PersonRepository extends CrudReppsitory<Person,String>{
    @alliowDiskUse
    @Aggregation("{$group:{_id:$lastname,names:{$addToSet:$firstname}}}")
    List<PersonAggregate> groupByLastnameAndFirstnames();
}
```

### 15.4 CDI集成

存储库接口的实例通常由容器创建，在使用Spring Data时，Spring是最自然的选择。

spring Data MongoDB附带了一个自定义CDI扩展，允许在CDI环境中使用存储库抽象。

可以通过为实现CDI Producer来设置基础结构MongoTemplate

```java
class MongoTemplateProducer{
    @Produces
    @ApplicationScoped
    public MongoOperations createMongoTemplate(){
        MongoDatabaseFactory factory = new SimpleMongoClientDatabaseFactory(MongoClients.create())
    }
}
```

MongoTemplate每当容器请求存储库类型的bean时，Spring Data MongoDB CDI扩展都会选择可用的CDI bean，并为Spring Data 存储库创建代理。因此，获取Spring Data存储库的实例是声明一个@inject-ed属性的问题

```java
class RepositoryClient{
    @Inject
    PersonRepository repository;
    public void businessMethod(){
        List<Person> people = repository.findAll();
    }
}
```

## 16 .反应式MongoDB存储库

### 16.1 反应性组合物库

反应空间提供各种反应组合库，最常见的就是RxJava     Project Reactor

### 16.2 用法

实例：示例Person实体

```java
public class Person{
    @Id
    private String id;
    private String firstname;
    private String lastname;
    private ADdress address;
    //getter and setter
}
```

示例：用于持久化Person实体的基本存储库接口

```java
public interface ReactivePersonRepository extends ReactiveSortingRepository<Person,String>{
    Flux<Person> findByFirstname(String firstname);
    Flux<Person> findByFirstname(Publisher<String> firstname);
    
}
```



