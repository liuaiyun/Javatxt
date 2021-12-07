# 1、Spring测试介绍

测试是微软软件开发的一个组成部分。

重点介绍--IoC原则为单元测试增加的价值以及SpringFramework对集成测试的支持的好处

# 2.单元测试

与传统Java EE相比，依赖注入应该使代码对容器的依赖更少

组成应用程序的POJO应该可以在JUnit或TestNG测试中进行测试，对象使用new操作符实例化，无需Spring或任何其他容器，也可以使用模拟对象（结合其他有价值的测试技术）来单独测试代码。若遵循Spring的架构建议，那么由此产生的代码库的清晰分层和组件化将有助于更轻松的进行测试。如可通过存根或模拟DAO或存储库接口来测试服务层对象，无需在运行单元测试时访问持久数据。

真正的单元测试通常运行的非常快，因为没有要设置的运行时的基础。真正的单元测试作为开发方法的一部分可以提高效率，

## 2.1.模拟对象

Spring包括许多专门用于模拟的包：

- 环境
- JNDI
- 服务程序接口
- Spring Web响应式

### 2.1.1.环境

该org.springframework.mock.env包 包含Evironment和PropertySource抽象的模拟实现。

MockEnvironment和MockPropertySource对于为依赖环境特定属性的代码开发容器外测试很有用。

### 2.1.2.JNDI

该org.springframework.mock.jndi包 包含JNDI SPI的部分实现，可以使用它为测试套件或独立应用程序设置简单的JNDI环境。

若JDBC DataSource实例在测试代码中绑定到与它们在Java EE容器中相同的JNDI名称，则可以在测试场景中重用应用程序代码和配置且无需修改。

### 2.1.3.服务程序接口

该org.springframework.mock.web包 包含一组全面的Servlet API模拟对象，可用于测试Web上下文、控制器和过滤器。这些模拟对象的目标是与Spring的Web MVC框架一起使用，并且通常比动态模拟对象或替代的Serclet API模拟对象更方便使用。

Spring MVC测试框架建立在模拟Servlet API对象上，为Spring MVC提供集成测试框架。

### 2.1.4.Spring Web响应式

该org.springframework.mock.http.server.reactive包 包含WebFlux应用程序的模拟实现ServerHttpRequest和ServerHttpResponse用于WevFlux应用程序。该org.springframework.mock.web.server包 包含一个ServerWebExchange依赖于这些模拟请求和响应对象的模拟。

二者MockServerHttpRequest和MockServerHttpResponse从相同的抽象基类作为服务器具体实施方式及与它们共享行为延伸。例：模拟请求一经创建便不可改变，但可以使用mutate() from 方法ServerHttpRequest创建修改后的实例。

为了使模拟响应正确实现写入协定并返回写入完成句柄（即Mono<void>），它默认使用Flux with cache().then()，它缓冲数据并使其可用于测试中的断言，应用程序可以设置自定义写入函数
（如  测试无限流）

在WebTestClient建立在模拟的请求和响应，以用于不具有HTTP服务器测试WebFlux应用提供支持。客户端还可用于与正在运行的服务器进行端到端测试。

## 2.2.单元测试支持类

Spring包括许多可以帮助进行单元测试的类，它们分为两类：

- 一般测试工具
- Spring MVC 测试工具

### 2.2.1.一般测试工具

该org.springframework.test.util软件包 包含几个用于单元和集成测试的通用实用程序。

ReflectionTestUtils是基于反射的使用方法的集合。在测试用例的应用程序代码中，可以设置public字段更改常量值。调用非public Setter方法或调用非public配置或生命周期回调方法的测试。

- ORM 框架（例如 JPA 和 Hibernate）允许`private`或`protected`字段访问，而不是`public`域实体中属性的 setter 方法。
- Spring 对注解（例如`@Autowired`、`@Inject`、 和`@Resource`）的支持，这些注解为`private`或`protected`字段、setter 方法和配置方法提供依赖注入。
- 使用诸如`@PostConstruct`和之类的注释，`@PreDestroy`用于生命周期回调方法。

AopTestUtils是 AOP 相关实用方法的集合。您可以使用这些方法来获取对隐藏在一个或多个 Spring 代理后面的底层目标对象的引用。例如，如果您使用 EasyMock 或 Mockito 等库将 bean 配置为动态模拟，并且该模拟包装在 Spring 代理中，则您可能需要直接访问底层模拟以对其配置期望并执行验证.

### 2.2.2.Spring MVC测试工具

可以将ModelAndViewAssert与JUnit、TestNG或任何其他处理Spring MVC ModelAndView对象单元测试的测试框架结合使用。

# 3.集成测试

涵盖Spring应用程序的集成测试，包括：

- 概述
- 生成测试的目标
- JDBC测试支持
- 注释
- Spring TestContext框架
- 模拟Mvc

## 3.1.概述

能够执行一些集成测试 无需部署到应用程序服务器或连接到其他企业基础构架，测试以下内容：

- Spring IoC容器上下文的正确接线。
- 使用JDBC 或 ORM工具进行数据访问。可以包括SQL语句，Hibernate查询、JPA实体映射等的正确性。

## 3.2.集成测试的目标

Spring 的集成测试支持 有以下主要目标：

- 管理测试之间的 Spring IoC 容器缓存
- 提供测试夹具实例的依赖注入
- 提供适合集成工具的事务管理
- 提供特定于Spring 的基类，以帮助开发人员编写集成测试。

描述每个目标 提供 实现和 配置细节的衔接。

### 3.2.1.上下文管理和缓存

Spring TestContext Framework 提供一致的 Spring`ApplicationContext`实例和`WebApplicationContext`实例加载 以及这些上下文的缓存。支持缓存加载的上下文很重要，因为启动时间可能成为一个问题——不是因为 Spring 本身的开销，而是因为 Spring 容器实例化的对象需要时间来实例化。例如，具有 50 到 100 个 Hibernate 映射文件的项目可能需要 10 到 20 秒来加载映射文件，并且在每个测试装置中运行每个测试之前产生该成本会导致整体测试运行速度变慢，从而降低开发人员的生产力。

测试类通常声明 XML 或 Groovy 配置元数据的资源位置数组（通常在类路径中）或用于配置应用程序的组件类数组。这些位置或类与在`web.xml`生产部署的其他配置文件中指定的位置或类相同或相似。

默认情况下，一旦加载，配置`ApplicationContext`将重复用于每个测试。因此，每个测试套件只产生一次设置成本，后续的测试执行速度要快得多。在这种情况下，术语“测试套件”是指在同一个 JVM 中运行的所有测试——例如，所有测试都从 Ant、Maven 或 Gradle 构建为给定的项目或模块运行。在不太可能的情况下，测试破坏了应用程序上下文并需要重新加载（例如，通过修改 bean 定义或应用程序对象的状态），TestContext 框架可以配置为在执行下一个之前重新加载配置并重建应用程序上下文测试。

### 3.2.2.测试Fixtures的依赖注入

当TestContext框架加载应用程序上下文时，可以选择使用依赖注入来配置测试类的实例。这为通过使用应用程序上下文中的预配置bean来设置测试装置提供了方便。好处是可以在各种测试场景中重用应用程序上下文（如用于配置Spring管理的对象图、事务代理、DataSource实例等）从而避免需要为单个测试用例复制复杂的测试Fixtures设置。

有一个类HibernateTitleRepository 为Title 域实体实现数据访问逻辑，编写测试一下领域的集成测试：

- Spring配置 基本上 与HibernateTitleRepository bean配置相关的所有内容 是否正确存在？
- Hibernate映射文件配置，所有内容是否正确映射以及正确的延迟设置是否到位？
- 逻辑HibernateTitleRepository 此类的配置实例是否按预期执行？

### 3.2.3.Transaction Management

访问真实数据库的测试中的一个常见问题是它们对持久性存储状态的影响。即使开发数据库，对状态的更改也可能会影响未来的测试。许多操作无法在事务之外进行。

TestContext框架解决了这个问题。默认情况下，框架为每个测试创建并回滚一个事务。可以编写可以假设事务存在的代码。若在测试中调用事务代理对象，会根据其配置的事务语义正确运行。若测试方法在测试管理的事务中运行时删除了所选表的内容，则事务默认回滚，并且数据库返回到执行测试之前的状态。通过使用PlatfromTranSactionManager在测试的应用程序上下文中定义的bean为测试提供事务服务。

若希望提交事务（希望特定测试填充或修改数据库时偶尔有用），可以使用@Commit注释让TestContext框架提交事务而不回滚。

### 3.2.4.集成测试的支持类

Spring TestContext Framework 提供了几个`abstract`支持类来简化集成测试的编写。这些基本测试类为测试框架提供了定义良好的挂钩以及方便的实例变量和方法，让您可以访问：

- The `ApplicationContext`，用于执行bean的查找或者测试背景下作为一个整体的状态。
- A `JdbcTemplate`，用于执行 SQL 语句来查询数据库。您可以在执行与数据库相关的应用程序代码之前和之后使用此类查询来确认数据库状态，并且 Spring 确保此类查询与应用程序代码在同一事务范围内运行。当与 ORM 工具结合使用时，一定要避免误报。

此外，您可能希望使用特定于您的项目的实例变量和方法创建自己的自定义、应用程序范围的超类。

## 3.3.JDBC测试支持

org.springframework.test.jdbc包 包含JdbcTestUtils

是JDBC相关实用程序函数的集合，旨在简化标准数据库测试场景。提供了以下静态使用方法。

- `countRowsInTable(..)` 计算给定表中的行数
- `countRowsInTableWhere(..)` 使用提供的WHERE子句计算给定表中的行数
- `deleteromTables(..)` 删除指定表中的所有行
- `deleteFromTableWhere(..)` 使用提供的WHERE子句从给定表中删除行
- `dropTables(..)` 删除指定的表

## 3.4.注释

包括以下：

- Spring测试注解
- 标准注释支持
- Spring JUnit4 测试注解
- Spring JUnit JUpiter 测试注解
- 对测试的元注释支持

### 3.4.1.Spring测试注解

Spring Framework 提供了以下一组特定于Spring的注释，可以在单元和集成测试中与TestContext框架一起使用这些注释。

Spring的测试注解包括以下内容：

- `@BootstrapWith`

- `@ContextConfiguration`

- `@WebAppConfiguration`

- `@ContextHierarchy`

- `@Activepofiles`

- `@TestPropertySource`

- `@DynamicPropertySource`

- `@DirtiesContext`

- `@TestExecutionListeners`

- `@RecordApplicationEvents`

- `@Commit`

- `@Rollback`

- `@BeforeTransaction`

- `@AfterTransaction`

- `@Sql`

- `@SqlConfig`

- `@SqlMergeMode`

- `@SqlGroup`

  

#### `@BootstrapWith`

`@BootstrapWith` 是一个类级别 的注释，可以使用它来配置Spring TestContext Framework的引导方式。

可以使用`@BootstrapWith` 来指定自定义 TestContextBootstrapper

#### `@ContextConfiguration`

`@ContextConfiguration` 定义用于确定如何加载和配置ApplicationContext集成测试的类级元数据。具体来说，`@ContextConfiguration` 声明应用程序上下文资源 locations 或 classes 用于加载上下文的组件。

资源位置通常是位于类路径的XML配置文件或Groovy脚本，而组件类通常是@Configuration类。但是，资源位置也可以指定文件系统中的文件和脚本，组件类可以是@Component类、@Service类等。

`@ContextConfiguration`引用XML文件注释

```java
@ContextConfiguration("/test-config.xml")
class XmlApplicationContextTests{
    //class body...
}
```

`@ContextConfiguration` 引用类的注释

```java
@ContextConfiguration(calsses = TestConfig.class)
class ConfigClassApplicationContextTests{
    //class body...
}
```

可以选择使用@ContextConfiguration 来声明 ContextLoader。不需要显示配置加载器，默认加载其支持initializers和资源locations或组件classes。

```java
@ContextConfiguration(locations = "/test-context.xml",loader = CustomContextLoader.class)
class CustomLoaderXmlApplicationContextTests{
    //class body...
}
```

`@ContextConfiguration` 为继承资源位置或配置类以及由超类或封闭类声明的上下文设定项提供支持。



#### `@WebAppConfiguration`

`@WebAppConfiguration` 是一个类级别的注释，可以用来声明ApplicationContext集成测试的加载 --WebApplicationContext。`@WebAppConfiguration` 测试类上仅存在可确保`@WebAppConfiguration` 为测试加载，使用"file:src/main/webapp" Web应用程序路径的默认值。资源基础路径在幕后用于创建一个MockServletContext，用作ServletContext测试的WebApplicationContext

如何使用`@WebAppConfiguration` 注释：

```java
@ContextConfiguration
@WebAppConfiguration
class WebAppTests{
    //class body...
}
```

要覆盖默认值，可以使用隐式value属性指定不同的基本资源路径。无论`classpath:` 和 `file:` 资源前缀的支持。如果未提供资源前缀，则假定路径为文件系统资源。

如何指定类路径资源：

```java
@ContextConfiguration
@WebAppConfiguration("classpath:test-web-resource")
class WebAppTests{
    //class body...
}
```

`@WebAppConfiguration` 必须与结合使用 `@ContextConfiguration` ,无论是在单个测试类中还是在测试类层次结构中。



#### `@ContextHierarchy`

`@ContextHierarchy` 是一个类级别的注释，用于定义 @ApplicationContext 集成测试的实例层次结构。@ContextHierarchy应该使用一个或多个`@ContextConfiguration` 实例的列表来声明，每个实例定义了上下文层次结构中的一个级别。

```java
@ContextHierarchy{{
    @ContextConfiguration("/parent-config.xml")
	@ContextConfiguration("/child-config.xml")
}}
class WebIntegrationTests{
    //class body...
}
```

若需要为测试类层次结构中的上下文层次结构的给定级别合并或覆盖配置，则必须通过为类层次结构中每个相应级别的 name  属性提供相同的值来显示命名该级别 @ContextConfiguration。



#### @`Activepofiles`

@`Activepofiles` 是一个类级别的注释，用于声明在加载ApplicationContext 集成测试时 哪些bean 定义配置文件 应该是活动的。

以下 表命 dev  配置文件应处于活动状态：

```java
@ContextConfiguration
@ActiveProfiles("dev")
class DeveloperTests{
    //class body...
}
```

以下 表明配置文件 dev 和 integration 配置文件都应处于活动状态:

```java
@ContextConfiguration
@ActiveProfiles({"dev","integration"})
class DeveloperIntegrationTests{
    //class body...
}
```

@`Activepofiles` 默认情况下，支持继承由超类和封闭类声明的活动 bean 定义配置文件。还可以通过实现自定义 ActiveProfilesResolver 并 使用的resolver 属性注册它来编程方式解析活动bean定义配置文件@ActiveProfiles



#### `@TestPropertySource`

`@TestPropertySource` 是一个类级别的注释，可以用它来配置属性文件，内嵌特性的配置 将被添加到该组 PropertySources 中的 Environment 一个 ApplicationContext 加载的集成测试。

如何从类路径声明属性文件：

```java
@ContextConfiguration
@TestPropertySource("/test.properties")
class MyIntegrationTests{
    //class body...
}
```

如何什么声明内联属性：

```java
@ContextConfiguration
@testPropertySource(properties = {"timezone = GMT","port:4242"})
class MyIntegrationTests{
    //class body...
}
```



#### `@DynamicPropertySource`

`@DynamicPropertySource` 是一种方法级注释，可以使用以注册动态特性被添加到该组PropertySource中的Envirenment 一个 ApplicationContext 加载的一个继承测试。若不知道属性的值，动态属性很有用。如若属性由外部资源管理，  --Testcontainers项目管理的容器

如何注册动态属性:

```java
@ContextConfiguration
class MyIntegrationTests{
    static MyExternalServer server = //...
   	
    @DynamicPropertySource
    static void dynamicProperties(DynamicPropertyRegistry registry){
        registry.add("server.port",server:getPort);
    }
    
    //tests...
}
```

1. 用注释一个static方法@DynamicPropertySource
2. 接受uoge DynamicPropertyRegistry作为参数
3. 注册一个动态server.port属性以从服务器延迟检索



#### `@DirtiesContext`

`@DirtiesContext` 表示底层Spring ApplicationContext 在测试执行期间已被 dirty（即，测试以某种方式修改或破坏了它。如，通过更改单例bean的状态）应该关闭。当应用程序上下文被标记为  dirty，它会从测试框架的缓存中移除并关闭。因此，对于需要具有相同配置元数据的上下文的任何后续测试，都会重新构建底层Spring容器。

可以@DirtiesContext 在同一类或类层次结构中用作类级别和方法级别的注释，ApplicationContext根据配置的methodMode，在任何此类带注释的方法之前或之后以及在当前测试类之前或之后将 标记为 dirty classMode。

以下 解释上下文何时会因各种配置场景而变dirty

在当前测试类之前，当在类模式设置为`BEFORE_CLASS`

```java
@DirtiesContext(classMode = BEFORE_CLASS)
class FreshContextTests{
    //some tests that required a new Spring container
}
```

在当前测试类之前dirty上下文

在当前测试类之后，当在类模式设置为AFTER_CLASS（即默认类模式）的类上声明时。

```java
@DirtiesContext
class ContextDirtyingTests{
    //some tests that result in the Spring container being dirtied
}
```

在当前测试类之后dirty上下文

在当前测试类中的每个测试方法之前，当在类模式设置为BEFORE_EACH_TEST_METHOD

```java
@DirtiesContext(classMode = BEFOR_EACH_TEST_METHOD)
class FreshContextTests{
    //some sts that required a new Spring container
}
```

在每个测试方法之前dirty上下文

在当前测试类中的每个测试方法之后，当在类模式设置为AFTER_EACH_TEST_METHOD

```java
@DirtiesContext(classMod = AFTER_RACH_TEST_METHOD)
class ContextDirtyingTests {
    // some tests that result in the Spring container being dirtied
}
```

在当前测试之前，当在方法模式设置为 的方法上声明时 `BEFORE_METHOD`

```java
@DirtiesContext(methodMode = BEFORE_METHOD) 
@Test
void testProcessWhichRequiresFreshAppCtx() {
    // some logic that requires a new Spring container
}
```

在当前测试方法之前dirty上下文

在当前测试之后，当在方法模式设置为 `AFTER_METHOD`（即默认方法模式）的方法上声明时

```java
@DirtiesContext 
@Test
void testProcessWhichDirtiesAppCtx() {
    // some logic that results in the Spring container being dirtied
}
```

```java
@ContextHierarchy({
    @ContextConfiguration("/parent-config.xml"),
    @ContextConfiguration("/child-config.xml")
})
class BaseTests {
    // class body...
}

class ExtendedTests extends BaseTests {

    @Test
    @DirtiesContext(hierarchyMode = CURRENT_LEVEL) 
    void test() {
        // some logic that results in the child context being dirtied
    }
}
```



#### `@TestExecutionListeners`

`@TestExecutionListeners`定义类级别的元数据，用于配置 `TestExecutionListener`应该注册到 `TestContextManager`. 通常，`@TestExecutionListeners`与 结合使用 `@ContextConfiguration`。

以下示例显示了如何注册两个`TestExecutionListener`实现：

```java
@ContextConfiguration
@TestExecutionListeners({CustomTestExecutionListener.class, AnotherTestExecutionListener.class}) 
class CustomTestExecutionListenerTests {
    // class body...
}
```



#### `@RecordApplicationEvents`

`@RecordApplicationEvents`是一个类级别的注解，用于指示 *Spring TestContext Framework*记录`ApplicationContext`在单个测试执行期间发布的所有应用程序事件 。可以通过`ApplicationEvents`测试中的API访问记录的事件。



#### `@Commit`

`@Commit`指示事务测试方法的事务应在测试方法完成后提交。您可以将其`@Commit`用作直接替代，`@Rollback(false)`以更明确地传达代码的意图。类似于`@Rollback`,`@Commit`也可以声明为类级或方法级注解。

```java
@Commit 
@Test
void testProcessWithoutRollback() {
    // ...
}
```



#### `@Rollback`

`@Rollback`指示在测试方法完成后是否应回滚事务测试方法的事务。如果`true`，则事务回滚。否则，将提交事务。Spring TestContext Framework 中集成测试的回滚默认为`true`即使`@Rollback`未显式声明。

当声明为类级别注释时，`@Rollback`为测试类层次结构中的所有测试方法定义默认回滚语义。当声明为方法级注释时，`@Rollback`为特定测试方法定义回滚语义，可能会覆盖类级`@Rollback`或`@Commit`语义。

```java
@Rollback(false) 
@Test
void testProcessWithoutRollback() {
    // ...
}
//不会滚实例
```



#### `@BeforeTransaction`

`@BeforeTransaction`表示`void`应在事务启动之前运行带注释的方法，用于已配置为使用 Spring 的`@Transactional`注释在事务中运行的测试方法。`@BeforeTransaction`方法不需要`public`并且可以在基于 Java 8 的接口默认方法上声明。

以下示例显示了如何使用`@BeforeTransaction`注释：

```java
@BeforeTransaction 
void beforeTransaction() {
    // logic to be run before a transaction is started
}
```



#### `@AfterTransaction`

`@AfterTransaction`表示`void`应在事务结束后运行带注释的方法，用于已配置为使用 Spring 的`@Transactional`注释在事务中运行的测试方法。`@AfterTransaction`方法不需要`public`并且可以在基于 Java 8 的接口默认方法上声明。

```java
@AfterTransaction 
void afterTransaction() {
    // logic to be run after a transaction has ended
}
```



#### `@Sql`

`@Sql`用于注释测试类或测试方法，以配置 SQL 脚本以在集成测试期间针对给定数据库运行。以下示例显示了如何使用它：

```java
@Test
@Sql({"/test-schema.sql", "/test-user-data.sql"}) 
void userTest() {
    // run code that relies on the test schema and test data
}
```



#### `@SqlConfig`

`@SqlConfig`定义用于确定如何解析和运行使用`@Sql`注释配置的 SQL 脚本的元数据。以下示例显示了如何使用它

```java
@Test
@Sql(
    scripts = "/test-user-data.sql",
    config = @SqlConfig(commentPrefix = "`", separator = "@@") 
)
void userTest() {
    // run code that relies on the test data
}
```



#### `@SqlMergeMode`

`@SqlMergeMode`用于注释测试类或测试方法，以配置方法级`@Sql`声明是否与类级`@Sql`声明合并。如果 `@SqlMergeMode`未在测试类或测试方法上声明，则`OVERRIDE`默认使用合并模式。使用该`OVERRIDE`模式，方法级`@Sql`声明将有效地覆盖类级`@Sql`声明。

请注意，方法级`@SqlMergeMode`声明会覆盖类级声明。

类级别使用

```java
@SpringJUnitConfig(TestConfig.class)
@Sql("/test-schema.sql")
@SqlMergeMode(MERGE) 
class UserTests {

    @Test
    @Sql("/user-test-data-001.sql")
    void standardUserProfile() {
        // run code that relies on test data set 001
    }
}
```

方法级别使用

```java
@SpringJUnitConfig(TestConfig.class)
@Sql("/test-schema.sql")
class UserTests {

    @Test
    @Sql("/user-test-data-001.sql")
    @SqlMergeMode(MERGE) 
    void standardUserProfile() {
        // run code that relies on test data set 001
    }
}
```



#### `@SqlGroup`

`@SqlGroup`是一个容器注解，聚合了几个`@Sql`注解。您可以使用`@SqlGroup`本机来声明多个嵌套`@Sql`注释，也可以将其与 Java 8 对可重复注释的支持结合使用，其中`@Sql`可以在同一类或方法上多次声明，隐式生成此容器注释。以下示例显示了如何声明 SQLGroup：

```java
@Test
@SqlGroup({ 
    @Sql(scripts = "/test-schema.sql", config = @SqlConfig(commentPrefix = "`")),
    @Sql("/test-user-data.sql")
)}
void userTest() {
    // run code that uses the test schema and test data
}
```

### 3.4.2标准注释支持

Spring TestContext Framework 的所有配置的标准语义都支持以下注解。请注意，这些注释并非特定于测试，可以在 Spring 框架中的任何地方使用。

- `@Autowired`
- `@Qualifier`
- `@Value`
- `@Resource` (javax.annotation) 如果存在 JSR-250
- `@ManagedBean` (javax.annotation) 如果存在 JSR-250
- `@Inject` (javax.inject) 如果存在 JSR-330
- `@Named` (javax.inject) 如果存在 JSR-330
- `@PersistenceContext` (javax.persistence) 如果存在 JPA
- `@PersistenceUnit` (javax.persistence) 如果存在 JPA
- `@Required`
- `@Transactional`(org.springframework.transaction.annotation)

### 3.4.3.Spring JUnit 4 测试注解

仅当与SpringRunner、Spring 的 JUnit 4 规则或Spring 的 JUnit 4 支持类结合使用时，才支持以下注解 ：

- `@IfProfileValue`
- `@ProfileValueSourceConfiguration`
- `@Timed`
- `@Repeat`

#### `@IfProfileValue`

`@IfProfileValue`表示为特定测试环境启用了带注释的测试。如果配置`ProfileValueSource`返回`value`与提供的匹配`name`，则启用测试。否则，测试将被禁用，并且实际上被忽略。

您可以`@IfProfileValue`在类级别、方法级别或两者都申请。的类级的使用`@IfProfileValue`接管方法级的使用的优先级为类或其子类中的任何方法。具体来说，如果在类级别和方法级别都启用了测试，则它被启用。没有`@IfProfileValue` 意味着测试被隐式启用。这类似于 JUnit 4`@Ignore`注释的语义 ，不同之处在于`@Ignore`始终禁用测试。

```java
@IfProfileValue(name="java.vendor", value="Oracle Corporation") 
@Test
public void testProcessWhichRunsOnlyOnOracleJvm() {
    // some logic that should run only on Java VMs from Oracle Corporation
}
```

可以在`@IfProfileValue`使用`values`（带有`OR` 语义）列表进行配置，以在 JUnit 4 环境中实现对测试组的类似 TestNG 的支持。

```java
@IfProfileValue(name="test-groups", values={"unit-tests", "integration-tests"}) 
@Test
public void testProcessWhichRunsForUnitOrIntegrationTestGroups() {
    // some logic that should run only for unit and integration test groups
}
```

#### `@ProfileValueSourceConfiguration`

`@ProfileValueSourceConfiguration`是一个类级别的注释，它指定`ProfileValueSource`在检索通过`@IfProfileValue`注释配置的配置文件值时要使用的 类型。如果`@ProfileValueSourceConfiguration`未为测试声明，`SystemProfileValueSource`则默认使用。以下示例显示了如何使用`@ProfileValueSourceConfiguration`：

```java
@ProfileValueSourceConfiguration(CustomProfileValueSource.class) 
public class CustomProfileValueSourceTests {
    // class body...
}
```

#### `@Timed`

`@Timed`表示带注释的测试方法必须在指定的时间段内（以毫秒为单位）完成执行。如果文本执行时间超过指定时间段，则测试失败。

时间段包括运行测试方法本身、测试的任何重复以及测试夹具的任何设置或拆除。以下示例显示了如何使用它：

```java
@Timed(millis = 1000) 
public void testProcessWithOneSecondTimeout() {
    // some logic that should not take longer than 1 second to run
}
```

#### `@Repeat`

`@Repeat`表示必须重复运行带注释的测试方法。测试方法要运行的次数在注释中指定。

重复执行的范围包括测试方法本身的执行以及测试夹具的任何设置或拆除。以下示例显示了如何使用`@Repeat`注释：

```java
@Repeat(10) 
@Test
public void testProcessRepeatedly() {
    // ...
}
```

### 3.4.4.Spring JUnit Jupiter测试注解

`SpringExtension`与 JUnit Jupiter（即 JUnit 5 中的编程模型）结合使用时，支持以下注释 ：

- `@SpringJUnitConfig`
- `@SpringJUnitWebConfig`
- `@NestedTestConfiguration`
- `@DisabledIf`

#### `@SpringJUnitConfig`

`@SpringJUnitConfig`是一个组合注解，它结合 `@ExtendWith(SpringExtension.class)`了 JUnit Jupiter 和`@ContextConfiguration`Spring TestContext Framework。它可以在类级别用作`@ContextConfiguration`. 关于配置选项，`@ContextConfiguration`和之间的唯一区别`@SpringJUnitConfig`是可以使用 中的`value`属性声明组件类`@SpringJUnitConfig`。

指定一个配置类

```java
@SpringJUnitConfig(TestConfig.class) 
class ConfigurationClassJUnitJupiterSpringTests {
    // class body...
}
```

指定配置文件的位置

```java
@SpringJUnitConfig(locations = "/test-config.xml") 
class XmlJUnitJupiterSpringTests {
    // class body...
}
```

#### `@SpringJUnitWebConfig`

@SpringJUnitWebConfig`是一个组合注解，它结合 `@ExtendWith(SpringExtension.class)`了 JUnit Jupiter`@ContextConfiguration`和 `@WebAppConfiguration`Spring TestContext Framework。您可以在类级别的下拉更换为使用它`@ContextConfiguration`和`@WebAppConfiguration`。关于配置选项，`@ContextConfiguration` 和之间的唯一区别`@SpringJUnitWebConfig`是您可以使用 中的`value`属性声明组件类 `@SpringJUnitWebConfig`。此外，您可以仅通过使用中的属性 来覆盖 中的`value` 属性。`@WebAppConfiguration``resourcePath``@SpringJUnitWebConfig

指定配置类

```java
@SpringJUnitWebConfig(TestConfig.class) 
class ConfigurationClassJUnitJupiterSpringWebTests {
    // class body...
}
```

指定配置文件的位置

```java
@SpringJUnitWebConfig(locations = "/test-config.xml") 
class XmlJUnitJupiterSpringWebTests {
    // class body...
}
```

#### `@TestConstructor`

`@TestConstructor`是一个类型级别的注释，用于配置测试类构造函数的参数如何从测试的 `ApplicationContext`.

如果`@TestConstructor`测试类中不存在或元存在，则将使用默认的*测试构造函数自动装配模式*。有关如何更改默认模式的详细信息，请参阅下面的提示。但是请注意，`@Autowired`构造函数上的局部声明优先于`@TestConstructor`默认模式和两者。

#### `@NestedTestConfiguration`

`@NestedTestConfiguration` 是一个类型级别的注解，用于配置如何在内部测试类的封闭类层次结构中处理 Spring 测试配置注解。

如果`@NestedTestConfiguration`在测试类、其超类型层次结构或其封闭类层次结构中不存在或元存在，则将使用默认的*封闭配置继承模式*。

在 Spring TestContext框架表彰`@NestedTestConfiguration`了以下注释语义。

- `@BootstrapWith`
- `@ContextConfiguration`
- `@ContextHierarchy`
- `@ActiveProfiles`
- `@TestPropertySource`
- `@DynamicPropertySource`
- `@DirtiesContext`
- `@TestExecutionListeners`
- `@RecordApplicationEvents`
- `@Transactional`
- `@Commit`
- `@Rollback`
- `@Sql`
- `@SqlConfig`
- `@SqlMergeMode`
- `@TestConstructor`

#### `@EnabledIf`

`@EnabledIf`用于表示已启用带注释的 JUnit Jupiter 测试类或测试方法，如果提供的`expression`计算结果为，则应运行`true`。具体来说，如果表达式计算结果等于`Boolean.TRUE`或`String`等于`true` （忽略大小写），则启用测试。在类级别应用时，该类中的所有测试方法也会默认自动启用。

表达式可以是以下任何一种：

- Spring 表达式语言(SpEL) 表达式。例如： `@EnabledIf("#{systemProperties['os.name'].toLowerCase().contains('mac')}")`
- Spring 中可用的属性的占位符`Environment`。例如：`@EnabledIf("${smoke.tests.enabled}")`
- 文字文字。例如：`@EnabledIf("true")`

但是请注意，不是属性占位符的动态解析结果的文本文字的实际价值为零，因为`@EnabledIf("false")`等效于`@Disabled`并且`@EnabledIf("true")`在逻辑上没有意义。

#### `@DisabledIf`

`@DisabledIf`用于表示带注释的 JUnit Jupiter 测试类或测试方法已禁用，如果提供的`expression`计算结果为 ，则不应运行`true`。具体来说，如果表达式计算结果为`Boolean.TRUE`或`String`等于`true`（忽略大小写），则测试被禁用。在类级别应用时，该类中的所有测试方法也会自动禁用。

表达式可以是以下任何一种：

- Spring 表达式语言(SpEL) 表达式。例如： `@DisabledIf("#{systemProperties['os.name'].toLowerCase().contains('mac')}")`
- Spring 中可用的属性的占位符`Environment`。例如：`@DisabledIf("${smoke.tests.disabled}")`
- 文字文字。例如：`@DisabledIf("true")`

但是请注意，不是属性占位符的动态解析结果的文本文字的实际价值为零，因为`@DisabledIf("true")`等效于`@Disabled`并且`@DisabledIf("false")`在逻辑上没有意义。

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@DisabledIf(
    expression = "#{systemProperties['os.name'].toLowerCase().contains('mac')}",
    reason = "Disabled on Mac OS"
)
public @interface DisabledOnMac {}
```

### 3.4.5. 对测试的元注释支持

您可以使用大多数与测试相关的注释作为 元注释来创建自定义组合注释并减少测试套件中的配置重复。

您可以将以下各项用作与TestContext 框架结合使用的元注释 。

- `@BootstrapWith`
- `@ContextConfiguration`
- `@ContextHierarchy`
- `@ActiveProfiles`
- `@TestPropertySource`
- `@DirtiesContext`
- `@WebAppConfiguration`
- `@TestExecutionListeners`
- `@Transactional`
- `@BeforeTransaction`
- `@AfterTransaction`
- `@Commit`
- `@Rollback`
- `@Sql`
- `@SqlConfig`
- `@SqlMergeMode`
- `@SqlGroup`
- `@Repeat` *（仅在 JUnit 4 上支持）*
- `@Timed` *（仅在 JUnit 4 上支持）*
- `@IfProfileValue` *（仅在 JUnit 4 上支持）*
- `@ProfileValueSourceConfiguration` *（仅在 JUnit 4 上支持）*
- `@SpringJUnitConfig` *（仅在 JUnit Jupiter 上支持）*
- `@SpringJUnitWebConfig` *（仅在 JUnit Jupiter 上支持）*
- `@TestConstructor` *（仅在 JUnit Jupiter 上支持）*
- `@NestedTestConfiguration` *（仅在 JUnit Jupiter 上支持）*
- `@EnabledIf` *（仅在 JUnit Jupiter 上支持）*
- `@DisabledIf` *（仅在 JUnit Jupiter 上支持）*

```java
@RunWith(SpringRunner.class)
@ContextConfiguration({"/app-config.xml", "/test-data-access-config.xml"})
@ActiveProfiles("dev")
@Transactional
public class OrderRepositoryTests { }

@RunWith(SpringRunner.class)
@ContextConfiguration({"/app-config.xml", "/test-data-access-config.xml"})
@ActiveProfiles("dev")
@Transactional
public class UserRepositoryTests { }
```

如果发现在基于 JUnit 4 的测试套件中重复了前面的配置，我们可以通过引入一个自定义组合注释来减少重复，该注释集中了 Spring 的通用测试配置

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@ContextConfiguration({"/app-config.xml", "/test-data-access-config.xml"})
@ActiveProfiles("dev")
@Transactional
public @interface TransactionalDevTestConfig { }
```

然后可以使用我们的自定义`@TransactionalDevTestConfig`注解来简化各个基于 JUnit 4 的测试类的配置

```java
@RunWith(SpringRunner.class)
@TransactionalDevTestConfig
public class OrderRepositoryTests { }

@RunWith(SpringRunner.class)
@TransactionalDevTestConfig
public class UserRepositoryTests { }
```

如果编写使用 JUnit Jupiter 的测试，我们可以进一步减少代码重复，因为 JUnit 5 中的注释也可以用作元注释。

```java
@ExtendWith(SpringExtension.class)
@ContextConfiguration({"/app-config.xml", "/test-data-access-config.xml"})
@ActiveProfiles("dev")
@Transactional
class OrderRepositoryTests { }

@ExtendWith(SpringExtension.class)
@ContextConfiguration({"/app-config.xml", "/test-data-access-config.xml"})
@ActiveProfiles("dev")
@Transactional
class UserRepositoryTests { }
```

如果发现在基于 JUnit Jupiter 的测试套件中重复了前面的配置，我们可以通过引入一个自定义组合注释来减少重复，该注释集中了 Spring 和 JUnit Jupiter 的通用测试配置

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@ExtendWith(SpringExtension.class)
@ContextConfiguration({"/app-config.xml", "/test-data-access-config.xml"})
@ActiveProfiles("dev")
@Transactional
public @interface TransactionalDevTestConfig { }
```

然后可以使用自定义`@TransactionalDevTestConfig`注解来简化各个基于 JUnit Jupiter 的测试类的配置

```java
@TransactionalDevTestConfig
class OrderRepositoryTests { }

@TransactionalDevTestConfig
class UserRepositoryTests { }
```

由于 JUnit Jupiter 支持使用`@Test`、`@RepeatedTest`、`ParameterizedTest`等作为元注释，因此您还可以在测试方法级别创建自定义组合注释。例如，如果我们希望创建一个组合注解，将来自 JUnit Jupiter的`@Test`和`@Tag`注解与`@Transactional` 来自 Spring的注解相结合，我们可以创建一个`@TransactionalIntegrationTest`注解

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Transactional
@Tag("integration-test") // org.junit.jupiter.api.Tag
@Test // org.junit.jupiter.api.Test
public @interface TransactionalIntegrationTest { }
```

然后可以使用自定义`@TransactionalIntegrationTest`注解来简化各个基于 JUnit Jupiter 的测试方法的配置

```java
@TransactionalIntegrationTest
void saveOrder() { }

@TransactionalIntegrationTest
void deleteOrder() { }
```

### 3.5. Spring TestContext 框架

Spring TestContext Framework（位于`org.springframework.test.context` 包中）提供了通用的、注解驱动的单元和集成测试支持，这些支持与正在使用的测试框架无关。TestContext 框架还非常重视约定而不是配置，您可以通过基于注释的配置覆盖合理的默认值。

除了通用测试基础架构之外，TestContext 框架还为 JUnit 4、JUnit Jupiter (AKA JUnit 5) 和 TestNG 提供了明确的支持。对于 JUnit 4 和 TestNG，Spring 提供了`abstract`支持类。此外，Spring为 JUnit 4 和JUnit Jupiter提供了自定义 JUnit`Runner`和自定义 JUnit ，允许您编写所谓的 POJO 测试类。POJO 测试类不需要扩展特定的类层次结构，例如支持类。`Rules``Extension``abstract`

#### 3.5.1. 关键抽象

该框架的核心包括了中`TestContextManager`类和 `TestContext`，`TestExecutionListener`以及`SmartContextLoader`接口。阿 `TestContextManager`为每个测试类创建（例如，用于在JUnit的木星单个测试类中所有的测试方法的执行）。该`TestContextManager`反过来，管理一个`TestContext`保存当前测试的上下文。该 `TestContextManager`还更新的状态`TestContext`作为测试进展和代表到`TestExecutionListener`实现，这仪器测试实际的执行提供依赖注入，管理事务，等等。A `SmartContextLoader`负责为`ApplicationContext`给定的测试类加载一个。

#### `TestContext`

`TestContext`封装运行测试的上下文（与实际使用的测试框架无关），并为其负责的测试实例提供上下文管理和缓存支持。该`TestContext`还委托给 `SmartContextLoader`加载的`ApplicationContext`，如果要求。

#### `TestContextManager`

`TestContextManager`是 Spring TestContext Framework 的主要入口点，负责管理单个`TestContext`事件，并将事件发送到每个`TestExecutionListener`在明确定义的测试执行点注册的事件 ：

- 在特定测试框架的任何“类之前”或“所有之前”方法之前。
- 测试实例后处理。
- 在特定测试框架的任何“之前”或“每个”方法之前。
- 紧接在执行测试方法之前但在测试设置之后。
- 立即执行测试方法之后，但在测试拆除之前。
- 在特定测试框架的任何“之后”或“每个”方法之后。
- 在特定测试框架的任何“课后”或“毕竟”方法之后。

#### `TestExecutionListener`

`TestExecutionListener`定义用于对`TestContextManager`注册侦听器所发布的测试执行事件作出反应的 API 。

#### ContextLoader

`ContextLoader`是一个策略接口，用于加载`ApplicationContext`Spring TestContext Framework 管理的集成测试。应该实现 `SmartContextLoader`而不是这个接口来提供对组件类、活动 bean 定义配置文件、测试属性源、上下文层次结构和 `WebApplicationContext`支持的支持。

`SmartContextLoader`是`ContextLoader`接口的扩展，取代了原来的最小`ContextLoader`SPI。具体来说，a`SmartContextLoader`可以选择处理资源位置、组件类或上下文初始值设定项。此外，a `SmartContextLoader`可以在它加载的上下文中设置活动 bean 定义配置文件和测试属性源。

Spring 提供了以下实现：

- `DelegatingSmartContextLoader`：两个默认加载器之一，它在内部委托给 an `AnnotationConfigContextLoader`、 a`GenericXmlContextLoader`或 a `GenericGroovyXmlContextLoader`，具体取决于为测试类声明的配置或默认位置或默认配置类的存在。仅当 Groovy 在类路径上时才启用 Groovy 支持。
- `WebDelegatingSmartContextLoader`：两个默认加载器之一，它在内部委托给 an `AnnotationConfigWebContextLoader`、 a`GenericXmlWebContextLoader`或 a `GenericGroovyXmlWebContextLoader`，具体取决于为测试类声明的配置或默认位置或默认配置类的存在。`ContextLoader`仅当`@WebAppConfiguration`测试类中存在网络时才使用网络。仅当 Groovy 在类路径上时才启用 Groovy 支持。
- `AnnotationConfigContextLoader`：`ApplicationContext`从组件类加载标准。
- `AnnotationConfigWebContextLoader`:`WebApplicationContext`从组件类加载 a 。
- `GenericGroovyXmlContextLoader`：`ApplicationContext`从 Groovy 脚本或 XML 配置文件的资源位置加载标准。
- `GenericGroovyXmlWebContextLoader`：`WebApplicationContext`从资源位置加载一个Groovy 脚本或 XML 配置文件。
- `GenericXmlContextLoader`：`ApplicationContext`从 XML 资源位置加载标准。
- `GenericXmlWebContextLoader`:`WebApplicationContext`从 XML 资源位置加载 a 。

### 3.5.2. 引导 TestContext 框架

Spring TestContext Framework 内部的默认配置足以满足所有常见用例。但是，有时开发团队或第三方框架想要更改默认值`ContextLoader`、实现自定义`TestContext`或`ContextCache`、增加`ContextCustomizerFactory`和`TestExecutionListener`实现的默认集 ，等等。对于 TestContext 框架如何运行的这种低级控制，Spring 提供了引导策略。

`TestContextBootstrapper`定义用于引导 TestContext 框架的 SPI。A `TestContextBootstrapper`由`TestContextManager`用于加载`TestExecutionListener`当前测试的 实现并构建 `TestContext`它管理的 。您可以`@BootstrapWith`直接或作为元注释使用 为测试类（或测试类层次结构）配置自定义引导策略。如果引导程序未通过 using 显式配置，则使用 `@BootstrapWith`the`DefaultTestContextBootstrapper`或 the `WebTestContextBootstrapper`，具体取决于`@WebAppConfiguration`.

由于`TestContextBootstrapper`SPI 将来可能会发生变化（以适应新的需求），我们强烈建议实现者不要直接实现这个接口，而是扩展`AbstractTestContextBootstrapper`它或其具体子类之一。

### 3.5.3. `TestExecutionListener`配置

Spring 提供了以下`TestExecutionListener`默认注册的实现，完全按照以下顺序：

- `ServletTestExecutionListener`：为 `WebApplicationContext`.
- `DirtiesContextBeforeModesTestExecutionListener`：处理`@DirtiesContext` “before”模式的注释。
- `ApplicationEventsTestExecutionListener`: 提供支持 `ApplicationEvents`
- `DependencyInjectionTestExecutionListener`: 为测试实例提供依赖注入。
- `DirtiesContextTestExecutionListener`：处理`@DirtiesContext`“after”模式的注释。
- `TransactionalTestExecutionListener`：提供具有默认回滚语义的事务测试执行。
- `SqlScriptsTestExecutionListener`: 运行使用`@Sql` 注解配置的 SQL 脚本。
- `EventPublishingTestExecutionListener`：将测试执行事件发布到测试的 `ApplicationContext`

#### 注册`TestExecutionListener`实现

`TestExecutionListener`使用`@TestExecutionListeners`注释为测试类及其子类注册实现

#### 自动发现默认`TestExecutionListener`实现

`TestExecutionListener`通过 using注册实现`@TestExecutionListeners`适用于在有限测试场景中使用的自定义侦听器。但是，如果需要在整个测试套件中使用自定义侦听器，这会变得很麻烦。此问题已通过支持`TestExecutionListener`通过该`SpringFactoriesLoader`机制自动发现默认 实现来解决。

具体来说，该`spring-test`模块在其属性文件中`TestExecutionListener` 的`org.springframework.test.context.TestExecutionListener`键下声明了所有核心默认实现`META-INF/spring.factories`。第三方框架和开发人员可以`TestExecutionListener`通过他们自己的`META-INF/spring.factories`属性文件以相同的方式将他们自己的实现贡献给默认侦听器列表。

#### 排序`TestExecutionListener`实现

当 TestContext 框架`TestExecutionListener`通过上述 `SpringFactoriesLoader`机制发现默认实现时，实例化的侦听器将使用 Spring 的 进行排序`AnnotationAwareOrderComparator`，这遵循Spring 的`Ordered`接口和 `@Order`注解进行排序。Spring 提供的`AbstractTestExecutionListener`所有默认`TestExecutionListener`实现都 `Ordered`使用适当的值实现。

#### 合并`TestExecutionListener`实现

如果`TestExecutionListener`通过 注册自定义`@TestExecutionListeners`，则不会注册默认侦听器。在大多数常见的测试场景中，这有效地迫使开发人员手动声明除任何自定义侦听器之外的所有默认侦听器。

```java
@ContextConfiguration
@TestExecutionListeners({
    MyCustomTestExecutionListener.class,
    ServletTestExecutionListener.class,
    DirtiesContextBeforeModesTestExecutionListener.class,
    DependencyInjectionTestExecutionListener.class,
    DirtiesContextTestExecutionListener.class,
    TransactionalTestExecutionListener.class,
    SqlScriptsTestExecutionListener.class
})
class MyTest {
    // class body...
}
```

为了避免必须知道和重新声明所有默认侦听器，您可以将 的`mergeMode`属性设置 `@TestExecutionListeners`为`MergeMode.MERGE_WITH_DEFAULTS`。 `MERGE_WITH_DEFAULTS`指示本地声明的侦听器应与默认侦听器合并。合并算法确保从列表中删除重复项，并根据 的语义对合并的侦听器结果集进行排序`AnnotationAwareOrderComparator`，如排序`TestExecutionListener`实现中所述。如果侦听器实现`Ordered`或使用 注释`@Order`，则它可以影响它与默认值合并的位置。否则，本地声明的侦听器在合并时会附加到默认侦听器列表中。

例如，如果`MyCustomTestExecutionListener`前面示例中的类将其`order`值（例如，`500`）配置为小于`ServletTestExecutionListener`（恰好是`1000`） 的顺序， `MyCustomTestExecutionListener`则 可以自动与 前面的默认值列表合并`ServletTestExecutionListener`，并且前面的示例可以替换为以下内容：

```java
@ContextConfiguration
@TestExecutionListeners(
    listeners = MyCustomTestExecutionListener.class,
    mergeMode = MERGE_WITH_DEFAULTS
)
class MyTest {
    // class body...
}
```

### 3.5.4.Application Event

从 Spring Framework 5.3.3 开始，TestContext 框架支持记录 在 中发布的应用程序事件， `ApplicationContext`以便可以在测试中针对这些事件执行断言。在执行单个测试期间发布的所有事件都通过`ApplicationEvents`API提供，该API 允许您将事件作为 `java.util.Stream`.

要`ApplicationEvents`在您的测试中使用，请执行以下操作。

- 确保您的测试类使用 `@RecordApplicationEvents`
- 确保`ApplicationEventsTestExecutionListener`已注册。但是请注意，这`ApplicationEventsTestExecutionListener`是默认注册的，只有在您通过`@TestExecutionListeners`不包含默认侦听器的自定义配置时才需要手动注册 。
- 注释类型的字段`ApplicationEvents`与`@Autowired`和使用该实例 `ApplicationEvents`在您的测试和生命周期方法（如`@BeforeEach`和 `@AfterEach`JUnit中木星的方法）。
  - 使用SpringExtension for JUnit Jupiter 时，您可以`ApplicationEvents`在测试或生命周期方法中声明类型为方法参数的方法参数，以替代`@Autowired`测试类中的字段。

以下测试类使用`SpringExtension`for JUnit Jupiter 和 AssertJ来断言在调用 Spring 管理的组件中的方法时发布的应用程序事件的类型：

```java
@SpringJUnitConfig(/* ... */)
@RecordApplicationEvents 
class OrderServiceTests {

    @Autowired
    OrderService orderService;

    @Autowired
    ApplicationEvents events; 

    @Test
    void submitOrder() {
        // Invoke method in OrderService that publishes an event
        orderService.submitOrder(new Order(/* ... */));
        // Verify that an OrderSubmitted event was published
        int numEvents = events.stream(OrderSubmitted.class).count(); 
        assertThat(numEvents).isEqualTo(1);
    }
}
```

### 3.5.5. 测试执行事件

将`EventPublishingTestExecutionListener`在Spring框架介绍5.2提供了一种替代的方法来实现自定义`TestExecutionListener`。测试中的组件`ApplicationContext`可以监听 发布的以下事件`EventPublishingTestExecutionListener`，每个事件 对应`TestExecutionListener`API 中的一个方法 。

- `BeforeTestClassEvent`
- `PrepareTestInstanceEvent`
- `BeforeTestMethodEvent`
- `BeforeTestExecutionEvent`
- `AfterTestExecutionEvent`
- `AfterTestMethodEvent`
- `AfterTestClassEvent`

这些事件可能因各种原因而被消耗，例如重置模拟 bean 或跟踪测试执行。使用测试执行事件而不是实现自定义的一个优点`TestExecutionListener`是测试执行事件可以被测试中注册的任何 Spring bean 使用`ApplicationContext`，并且这些 bean 可以直接受益于依赖注入和`ApplicationContext`. 相反， a`TestExecutionListener`不是`ApplicationContext`.

为了监听测试执行事件，Spring bean 可以选择实现该 `org.springframework.context.ApplicationListener`接口。或者，侦听器方法可以使用注释`@EventListener`并配置为侦听上面列出的特定事件类型之一。由于这种方法的流行，Spring 提供了以下专用 `@EventListener`注解来简化测试执行事件侦听器的注册。这些注释驻留在`org.springframework.test.context.event.annotation` 包中。

- `@BeforeTestClass`
- `@PrepareTestInstance`
- `@BeforeTestMethod`
- `@BeforeTestExecution`
- `@AfterTestExecution`
- `@AfterTestMethod`
- `@AfterTestClass`

##### 异常处理

默认情况下，如果测试执行事件侦听器在使用事件时抛出异常，则该异常将传播到正在使用的底层测试框架（例如 JUnit 或 TestNG）。例如，如果消耗一个`BeforeTestMethodEvent`导致异常，则相应的测试方法将因异常而失败。相反，如果异步测试执行事件侦听器抛出异常，则该异常不会传播到底层测试框架。有关异步异常处理的更多详细信息，请参阅`@EventListener`.

##### 异步侦听器

如果您希望特定的测试执行事件侦听器异步处理事件，则可以使用 Spring 的常规 `@Async`支持。有关更多详细信息，请参阅 `@EventListener`.

### 3.5.6. Context Management

每个都`TestContext`为其负责的测试实例提供上下文管理和缓存支持。测试实例不会自动接收对配置的`ApplicationContext`. 但是，如果测试类实现了该 `ApplicationContextAware`接口，`ApplicationContext`则会向测试实例提供对 的引用。请注意，`AbstractJUnit4SpringContextTests`并 `AbstractTestNGSpringContextTests`实现`ApplicationContextAware`并因此提供对`ApplicationContext`自动的访问。

@Autowired应用程序上下文

作为实现`ApplicationContextAware`接口的替代方法，可以通过`@Autowired`字段或 setter 方法上的注释为您的测试类注入应用程序上下文，如以下示例所示：

```java
@SpringJUnitConfig
class MyTest {

    @Autowired 
    ApplicationContext applicationContext;

    // class body...
}
```

同样，如果测试配置为加载`WebApplicationContext`，您可以将 Web 应用程序上下文注入到您的测试中，如下所示：

```java
@SpringJUnitWebConfig 
class MyWebAppTest {

    @Autowired 
    WebApplicationContext wac;

    // class body...
}
```

依赖注入是由`@Autowired`提供的 `DependencyInjectionTestExecutionListener`，它是默认配置的

使用 TestContext 框架的测试类不需要扩展任何特定的类或实现特定的接口来配置它们的应用程序上下文。相反，配置是通过`@ContextConfiguration`在类级别声明注释来实现的。如果您的测试类未显式声明应用程序上下文资源位置或组件类，则配置`ContextLoader`确定如何从默认位置或默认配置类加载上下文。除了上下文资源位置和组件类之外，还可以通过应用程序上下文初始值设定项配置应用程序上下文。

以下部分解释了如何使用 Spring 的`@ContextConfiguration`注解`ApplicationContext`通过 XML 配置文件、Groovy 脚本、组件类（通常是`@Configuration`类）或上下文初始化器来配置测试。或者可以通过`SmartContextLoader`为高级用例实现和配置您自己的自定义。

#### 使用 XML 资源的上下文配置

要`ApplicationContext`使用 XML 配置文件加载测试，请使用包含 XML 配置元数据的资源位置的数组来注释测试类`@ContextConfiguration`并配置`locations`属性。普通路径或相对路径（例如，`context.xml`）被视为与定义测试类的包相关的类路径资源。以斜杠开头的路径被视为绝对类路径位置（例如，`/org/example/config.xml`）。表示资源URL的路径（即，路径前缀`classpath:`，`file:`， `http:`等等）使用*原样*。

```java
@ExtendWith(SpringExtension.class)
// ApplicationContext will be loaded from "/app-config.xml" and
// "/test-config.xml" in the root of the classpath
@ContextConfiguration(locations={"/app-config.xml", "/test-config.xml"}) 
class MyTest {
    // class body...
}
```

`@ContextConfiguration``locations`通过标准 Java`value`属性支持属性的别名。因此，如果不需要在 中声明其他属性`@ContextConfiguration`，则可以省略`locations` 属性名称的声明并使用以下示例中演示的速记格式声明资源位置：

```java
@ExtendWith(SpringExtension.class)
@ContextConfiguration({"/app-config.xml", "/test-config.xml"}) 
class MyTest {
    // class body...
}
```

如果从注释中省略 the`locations`和 the`value`属性 `@ContextConfiguration`，TestContext 框架会尝试检测默认 XML 资源位置。具体来说，`GenericXmlContextLoader`并 `GenericXmlWebContextLoader`根据测试类的名称检测默认位置。如果类已命名`com.example.MyTest`，`GenericXmlContextLoader`则从 加载应用程序上下文`"classpath:com/example/MyTest-context.xml"`。以下示例显示了如何执行此操作：

```java
@ExtendWith(SpringExtension.class)
// ApplicationContext will be loaded from
// "classpath:com/example/MyTest-context.xml"
@ContextConfiguration 
class MyTest {
    // class body...
}
```

#### 使用 Groovy 脚本进行上下文配置

要`ApplicationContext`使用使用Groovy Bean 定义 DSL 的Groovy 脚本加载测试 ，您可以使用 包含 Groovy 脚本资源位置的数组来注释测试类`@ContextConfiguration`并配置`locations`or`value`属性。Groovy 脚本的资源查找语义与XML 配置文件中描述的相同 。

- [x]  启用 Groovy 脚本支持

`ApplicationContext`如果 Groovy 位于类路径上，则会自动启用 对使用 Groovy 脚本加载Spring TestContext Framework 的支持。

指定Groovy配置文件

```java
@ExtendWith(SpringExtension.class)
// ApplicationContext will be loaded from "/AppConfig.groovy" and
// "/TestConfig.groovy" in the root of the classpath
@ContextConfiguration({"/AppConfig.groovy", "/TestConfig.Groovy"}) 
class MyTest {
    // class body...
}
```

如果从 注释中省略 the`locations`和`value`属性`@ContextConfiguration`，TestContext 框架会尝试检测默认的 Groovy 脚本。具体来说，`GenericGroovyXmlContextLoader`并`GenericGroovyXmlWebContextLoader` 根据测试类的名称检测默认位置。如果您的类已命名 `com.example.MyTest`，则 Groovy 上下文加载器将从 `"classpath:com/example/MyTestContext.groovy"`. 以下示例显示了如何使用默认值：

```java
@ExtendWith(SpringExtension.class)
// ApplicationContext will be loaded from
// "classpath:com/example/MyTestContext.groovy"
@ContextConfiguration 
class MyTest {
    // class body...
}
```

- [x] ##### 同时声明 XML 配置和 Groovy 脚本

可以使用 locations 或者 value`属性 同时声明 XML 配置文件和 Groovy 脚本`@ContextConfiguration`。如果配置资源位置的路径以 结尾`.xml`，则使用 `XmlBeanDefinitionReader`. 否则，将使用 `GroovyBeanDefinitionReader`.

以下清单显示了如何在集成测试中结合两者：

```java
@ExtendWith(SpringExtension.class)
// ApplicationContext will be loaded from AppConfig and TestConfig
@ContextConfiguration(classes = {AppConfig.class, TestConfig.class}) 
class MyTest {
    // class body...
}
```

##### 带有组件类的上下文配置

要`ApplicationContext`使用组件类加载测试，可以使用包含对组件类的引用的数组来注释测试类`@ContextConfiguration`并配置`classes`属性。

```java
@ExtendWith(SpringExtension.class)
// ApplicationContext will be loaded from AppConfig and TestConfig
@ContextConfiguration(classes = {AppConfig.class, TestConfig.class}) 
class MyTest {
    // class body...
}
```

- [ ] **组件类**

-  组件类术语“组件类”可以指以下任何一项：
- 用`@Configuration`.一个组件（即，具有注释的一类`@Component`，`@Service`，`@Repository`，或其他构造型注释）。
- 使用注释进行`javax.inject`注释的符合 JSR-330 的类。
- 任何包含`@Bean`-methods 的类。
- 任何其他打算注册为 Spring 组件（即 中的 Spring bean `ApplicationContext`）的类，可能会利用单个构造函数的自动自动装配而不使用 Spring 注释。

`OrderServiceTest`该类声明了一个`static`名为的嵌套配置类`Config`，该类将自动用于加载`ApplicationContext`测试类：

```java
@SpringJUnitConfig 
// ApplicationContext will be loaded from the
// static nested Config class
class OrderServiceTest {

    @Configuration
    static class Config {

        // this bean will be injected into the OrderServiceTest class
        @Bean
        OrderService orderService() {
            OrderService orderService = new OrderServiceImpl();
            // set properties, etc.
            return orderService;
        }
    }

    @Autowired
    OrderService orderService;

    @Test
    void testOrderService() {
        // test the orderService
    }

}
```

#### 混合 XML、Groovy 脚本和组件类

有时可能需要混合 XML 配置文件、Groovy 脚本和组件类（通常是`@Configuration`类）来`ApplicationContext`为您的测试配置一个 。例如，如果您在生产中使用 XML 配置，您可能决定使用`@Configuration`类为您的测试配置特定的 Spring 管理的组件，反之亦然。

此外，一些第三方框架（例如 Spring Boot）为`ApplicationContext`同时从不同类型的资源（例如 XML 配置文件、Groovy 脚本和 `@Configuration`类）加载一个提供了一流的支持。从历史上看，Spring 框架不支持标准部署。因此，`SmartContextLoader`Spring Framework 在`spring-test`模块中提供的大多数实现仅支持每个测试上下文的一种资源类型。但是，这并不意味着您不能同时使用两者。一般规则的一个例外是`GenericGroovyXmlContextLoader`和 同时 `GenericGroovyXmlWebContextLoader`支持 XML 配置文件和 Groovy 脚本。此外，第三方框架可以选择支持两者的声明`locations`并`classes`通过`@ContextConfiguration`，并且，借助 TestContext 框架中的标准测试支持，您有以下选择。

如果想使用资源位置（例如，XML 或 Groovy）和`@Configuration` 类来配置您的测试，您必须选择一个作为入口点，并且必须包含或导入另一个。例如，在 XML 或 Groovy 脚本中，可以`@Configuration`通过使用组件扫描或将类定义为普通 Spring bean来包含 类，而在`@Configuration`类中，您可以使用`@ImportResource`导入 XML 配置文件或 Groovy 脚本。请注意，此行为在语义上等同于您在生产中配置应用程序的方式：在生产配置中，定义一组 XML 或 Groovy 资源位置或一组`@Configuration` 从中`ApplicationContext`加载产品的类,仍然可以自由地包括或导入其他类型的配置。

#### 使用上下文初始值设定项的上下文配置

要配置`ApplicationContext`使用上下文初始化为你的测试，注释与你的测试类`@ContextConfiguration`和配置`initializers` 与包含对实现类的引用数组属性 `ApplicationContextInitializer`。然后使用声明的上下文初始化器来初始化`ConfigurableApplicationContext`为您的测试加载的 。请注意，`ConfigurableApplicationContext`每个声明的初始化程序支持的具体类型必须与使用中`ApplicationContext`创建 的类型`SmartContextLoader`（通常是 a `GenericApplicationContext`）兼容。此外，调用初始化器的顺序取决于它们是实现 Spring 的 `Ordered`接口还是使用 Spring 的`@Order`注解或标准 `@Priority`注解进行注解。以下示例显示了如何使用初始化程序：

```java
@ExtendWith(SpringExtension.class)
// ApplicationContext will be loaded from TestConfig
// and initialized by TestAppCtxInitializer
@ContextConfiguration(
    classes = TestConfig.class,
    initializers = TestAppCtxInitializer.class) 
class MyTest {
    // class body...
}
```

通过以编程方式从 XML 文件或配置类加载 bean 定义. 

```java
@ExtendWith(SpringExtension.class)
// ApplicationContext will be initialized by EntireAppInitializer
// which presumably registers beans in the context
@ContextConfiguration(initializers = EntireAppInitializer.class) 
class MyTest {
    // class body...
}
```

##### 上下文配置继承

`@ContextConfiguration`支持布尔值`inheritLocations`和`inheritInitializers` 属性，表示是否应继承资源位置或组件类和超类声明的上下文初始值设定项。两个标志的默认值都是`true`。这意味着测试类继承资源位置或组件类以及任何超类声明的上下文初始值设定项。具体来说，测试类的资源位置或组件类被附加到由超类声明的资源位置或带注释的类的列表中。类似地，给定测试类的初始值设定项添加到测试超类定义的初始值设定项集中。因此，子类可以选择扩展资源位置、组件类或上下文初始值设定项。

下面的例子展示了一个类如何扩展另一个类并使用它自己的配置文件和超类的配置文件：

```java
@ExtendWith(SpringExtension.class)
// ApplicationContext will be loaded from "/base-config.xml"
// in the root of the classpath
@ContextConfiguration("/base-config.xml") 
class BaseTest {
    // class body...
}

// ApplicationContext will be loaded from "/base-config.xml" and
// "/extended-config.xml" in the root of the classpath
@ContextConfiguration("/extended-config.xml") 
class ExtendedTest extends BaseTest {
    // class body...
}
```

下面的例子展示了一个类如何扩展另一个类并使用它自己的配置类和超类的配置类：

```java
// ApplicationContext will be loaded from BaseConfig
@SpringJUnitConfig(BaseConfig.class) 
class BaseTest {
    // class body...
}

// ApplicationContext will be loaded from BaseConfig and ExtendedConfig
@SpringJUnitConfig(ExtendedConfig.class) 
class ExtendedTest extends BaseTest {
    // class body...
}
```

下面的例子展示了一个类如何扩展另一个类并使用它自己的初始化器和超类的初始化器：

```java
// ApplicationContext will be initialized by BaseInitializer
@SpringJUnitConfig(initializers = BaseInitializer.class) 
class BaseTest {
    // class body...
}

// ApplicationContext will be initialized by BaseInitializer
// and ExtendedInitializer
@SpringJUnitConfig(initializers = ExtendedInitializer.class) 
class ExtendedTest extends BaseTest {
    // class body...
}
```

#### 使用环境配置文件的上下文配置

Spring 框架对环境和配置文件（又名“bean 定义配置文件”）的概念提供一流的支持，并且可以配置集成测试以激活用于各种测试场景的特定 bean 定义配置文件。这是通过使用注释对测试类进行`@ActiveProfiles`注释并提供在加载`ApplicationContext`测试时应激活的配置文件列表来实现的。

考虑两个带有 XML 配置和`@Configuration`类的示例：

```java
<!-- app-config.xml -->
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:jdbc="http://www.springframework.org/schema/jdbc"
    xmlns:jee="http://www.springframework.org/schema/jee"
    xsi:schemaLocation="...">

    <bean id="transferService"
            class="com.bank.service.internal.DefaultTransferService">
        <constructor-arg ref="accountRepository"/>
        <constructor-arg ref="feePolicy"/>
    </bean>

    <bean id="accountRepository"
            class="com.bank.repository.internal.JdbcAccountRepository">
        <constructor-arg ref="dataSource"/>
    </bean>

    <bean id="feePolicy"
        class="com.bank.service.internal.ZeroFeePolicy"/>

    <beans profile="dev">
        <jdbc:embedded-database id="dataSource">
            <jdbc:script
                location="classpath:com/bank/config/sql/schema.sql"/>
            <jdbc:script
                location="classpath:com/bank/config/sql/test-data.sql"/>
        </jdbc:embedded-database>
    </beans>

    <beans profile="production">
        <jee:jndi-lookup id="dataSource" jndi-name="java:comp/env/jdbc/datasource"/>
    </beans>

    <beans profile="default">
        <jdbc:embedded-database id="dataSource">
            <jdbc:script
                location="classpath:com/bank/config/sql/schema.sql"/>
        </jdbc:embedded-database>
    </beans>

</beans>
```

```java
@ExtendWith(SpringExtension.class)
// ApplicationContext will be loaded from "classpath:/app-config.xml"
@ContextConfiguration("/app-config.xml")
@ActiveProfiles("dev")
class TransferServiceTest {

    @Autowired
    TransferService transferService;

    @Test
    void testTransferService() {
        // test the transferService
    }
}
```

以下代码清单演示了如何使用`@Configuration`类而不是 XML实现相同的配置和集成测试：

```java
@Configuration
@Profile("dev")
public class StandaloneDataConfig {

    @Bean
    public DataSource dataSource() {
        return new EmbeddedDatabaseBuilder()
            .setType(EmbeddedDatabaseType.HSQL)
            .addScript("classpath:com/bank/config/sql/schema.sql")
            .addScript("classpath:com/bank/config/sql/test-data.sql")
            .build();
    }
}


@Configuration
@Profile("production")
public class JndiDataConfig {

    @Bean(destroyMethod="")
    public DataSource dataSource() throws Exception {
        Context ctx = new InitialContext();
        return (DataSource) ctx.lookup("java:comp/env/jdbc/datasource");
    }
}


@Configuration
@Profile("default")
public class DefaultDataConfig {

    @Bean
    public DataSource dataSource() {
        return new EmbeddedDatabaseBuilder()
            .setType(EmbeddedDatabaseType.HSQL)
            .addScript("classpath:com/bank/config/sql/schema.sql")
            .build();
    }
}


@Configuration
public class TransferServiceConfig {

    @Autowired DataSource dataSource;

    @Bean
    public TransferService transferService() {
        return new DefaultTransferService(accountRepository(), feePolicy());
    }

    @Bean
    public AccountRepository accountRepository() {
        return new JdbcAccountRepository(dataSource);
    }

    @Bean
    public FeePolicy feePolicy() {
        return new ZeroFeePolicy();
    }
}


@SpringJUnitConfig({
        TransferServiceConfig.class,
        StandaloneDataConfig.class,
        JndiDataConfig.class,
        DefaultDataConfig.class})
@ActiveProfiles("dev")
class TransferServiceTest {

    @Autowired
    TransferService transferService;

    @Test
    void testTransferService() {
        // test the transferService
    }
}



```

在此变体中，我们将 XML 配置拆分为四个独立的 `@Configuration`类：

- `TransferServiceConfig`：`dataSource`通过使用获得一个通过依赖注入 `@Autowired`。
- `StandaloneDataConfig`：`dataSource`为适合开发人员测试的嵌入式数据库定义 a 。
- `JndiDataConfig`：定义`dataSource`从生产环境中的 JNDI 检索的 。
- `DefaultDataConfig`：`dataSource`为默认嵌入式数据库定义 a ，以防配置文件处于活动状态。

在以下示例中，`@ActiveProfiles`（以及其他注释）的声明已移至抽象超类`AbstractIntegrationTest`：

```java
@SpringJUnitConfig({
        TransferServiceConfig.class,
        StandaloneDataConfig.class,
        JndiDataConfig.class,
        DefaultDataConfig.class})
@ActiveProfiles("dev")
abstract class AbstractIntegrationTest {
}


// "dev" profile inherited from superclass
class TransferServiceTest extends AbstractIntegrationTest {

    @Autowired
    TransferService transferService;

    @Test
    void testTransferService() {
        // test the transferService
    }
}


// "dev" profile overridden with "production"
@ActiveProfiles(profiles = "production", inheritProfiles = false)
class ProductionTransferServiceTest extends AbstractIntegrationTest {
    // test body
}



```

此外，有时需要以编程方式而不是声明方式解析测试的活动配置文件——例如，基于：

- 当前的操作系统。
- 测试是否正在持续集成构建服务器上运行。
- 某些环境变量的存在。
- 自定义类级别注释的存在。
- 其他问题。

以下示例演示了如何实现和注册自定义 `OperatingSystemActiveProfilesResolver`：

```java
// "dev" profile overridden programmatically via a custom resolver
@ActiveProfiles(
        resolver = OperatingSystemActiveProfilesResolver.class,
        inheritProfiles = false)
class TransferServiceTest extends AbstractIntegrationTest {
    // test body
}


public class OperatingSystemActiveProfilesResolver implements ActiveProfilesResolver {

    @Override
    public String[] resolve(Class<?> testClass) {
        String profile = ...;
        // determine the value of profile based on the operating system
        return new String[] {profile};
    }
}
```

#### 带有测试属性源的上下文配置

Spring Framework 对具有属性源层次结构的环境概念提供一流的支持，您可以使用特定于测试的属性源配置集成测试。与`@PropertySource`在`@Configuration`类上使用的注解 相反，您可以`@TestPropertySource`在测试类上声明注解来声明测试属性文件或内联属性的资源位置。这些试验性质源被添加到组`PropertySources`中 `Environment`的`ApplicationContext`加载注释集成测试。

##### 声明测试属性源

您可以使用 的`locations`or`value`属性 来配置测试属性文件`@TestPropertySource`。

支持传统和基于 XML 的属性文件格式 - 例如， `"classpath:/com/example/test.properties"`或`"file:///path/to/file.xml"`.

每个路径都被解释为一个 Spring `Resource`。普通路径（例如， `"test.properties"`）被视为与定义测试类的包相关的类路径资源。以斜杠开头的路径被视为绝对类路径资源（例如：）`"/org/example/test.xml"`。该引用的URL（例如，路径前缀的路径`classpath:`，`file:`或`http:`），通过使用指定的资源协议加载。`***/**.properties`不允许使用资源位置通配符（例如 ）：每个位置的计算结果必须恰好是一个 `.properties`或`.xml`资源。

测试属性文件:

```java
@ContextConfiguration
@TestPropertySource("/test.properties") 
class MyIntegrationTests {
    // class body...
}
```

键值对支持的语法与为 Java 属性文件中的条目定义的语法相同：

- `key=value`
- `key:value`
- `key value`

两个内联属性

```java
@ContextConfiguration
@TestPropertySource(properties = {"timezone = GMT", "port: 4242"}) 
class MyIntegrationTests {
    // class body...
}
```

以下示例显示了如何在文件和内联中指定属性：

```java
@ContextConfiguration
@TestPropertySource(
    locations = "/test.properties",
    properties = {"timezone = GMT", "port: 4242"}
)
class MyIntegrationTests {
    // class body...
}
```

以下示例显示如何使用文件在子类及其超类中定义属性：`BaseTest``base.properties``ApplicationContext``ExtendedTest``base.properties``extended.properties``properties`

```java
@TestPropertySource("base.properties")
@ContextConfiguration
class BaseTest {
    // ...
}

@TestPropertySource("extended.properties")
@ContextConfiguration
class ExtendedTest extends BaseTest {
    // ...
}
```

以下示例显示如何使用内联属性在子类及其超类中定义属性：`BaseTest``key1``ApplicationContext``ExtendedTest``key1``key2`

```java
@TestPropertySource(properties = "key1 = value1")
@ContextConfiguration
class BaseTest {
    // ...
}

@TestPropertySource(properties = "key2 = value2")
@ContextConfiguration
class ExtendedTest extends BaseTest {
    // ...
}
```

#### 具有动态属性源的上下文配置

与`@TestPropertySource`应用于类级别的注释相反，`@DynamicPropertySource`必须应用于`static`接受单个`DynamicPropertyRegistry`参数的方法，该参数用于将*名称-值*对添加到`Environment`. 值是动态的，并通过 a 提供，该 a`Supplier`仅在解析属性时调用。通常，方法引用用于提供值，如以下示例所示，该示例使用 Testcontainers 项目管理 Spring 之外的 Redis 容器 `ApplicationContext`。托管 Redis 容器的 IP 地址和端口可`ApplicationContext`通过`redis.host`和 `redis.port`属性供测试中的组件使用。这些属性可以通过 Spring 的`Environment` 抽象或直接注入到 Spring 管理的组件中——例如，分别通过 `@Value("${redis.host}")`和`@Value("${redis.port}")`。

```java
@SpringJUnitConfig(/* ... */)
@Testcontainers
class ExampleIntegrationTests {

    @Container
    static RedisContainer redis = new RedisContainer();

    @DynamicPropertySource
    static void redisProperties(DynamicPropertyRegistry registry) {
        registry.add("redis.host", redis::getContainerIpAddress);
        registry.add("redis.port", redis::getMappedPort);
    }

    // tests ...

}
```

#### 加载一个 `WebApplicationContext`

其余示例显示了用于加载`WebApplicationContext`. 以下示例显示了 TestContext 框架对约定优于配置的支持：

```java
@ExtendWith(SpringExtension.class)

// defaults to "file:src/main/webapp"
@WebAppConfiguration

// detects "WacTests-context.xml" in the same package
// or static nested @Configuration classes
@ContextConfiguration
class WacTests {
    //...
}
```

以下示例显示如何使用 显式声明资源基础路径 `@WebAppConfiguration`和 XML 资源位置`@ContextConfiguration`：

```java
@ExtendWith(SpringExtension.class)

// file system resource
@WebAppConfiguration("webapp")

// classpath resource
@ContextConfiguration("/spring/test-servlet-config.xml")
class WacTests {
    //...
}
```

以下示例显示我们可以通过指定 Spring 资源前缀来覆盖两个注释的默认资源语义：

```java
@ExtendWith(SpringExtension.class)

// classpath resource
@WebAppConfiguration("classpath:test-web-resources")

// file system resource
@ContextConfiguration("file:src/main/webapp/WEB-INF/servlet-config.xml")
class WacTests {
    //...
}
```

使用网络模拟

为了提供全面的 Web 测试支持，TestContext 框架 `ServletTestExecutionListener`默认启用了 。当针对 a 进行测试时 `WebApplicationContext`，这会在每个测试方法之前`TestExecutionListener` 使用 Spring Web 设置默认线程本地状态，并根据配置的基本资源路径 `RequestContextHolder`创建 a `MockHttpServletRequest`、 a`MockHttpServletResponse`和 a 。还确保可以将 和注入到测试实例中，并且一旦测试完成，它就会清除线程本地状态。`ServletWebRequest``@WebAppConfiguration``ServletTestExecutionListener``MockHttpServletResponse``ServletWebRequest`

一旦你`WebApplicationContext`为你的测试加载了一个，你可能会发现你需要与 web 模拟交互——例如，设置你的测试装置或在调用你的 web 组件后执行断言。以下示例显示了哪些模拟可以自动装配到您的测试实例中。请注意，`WebApplicationContext`和 `MockServletContext`都在整个测试套件中缓存，而其他模拟由`ServletTestExecutionListener`.

```java
@SpringJUnitWebConfig
class WacTests {

    @Autowired
    WebApplicationContext wac; // cached

    @Autowired
    MockServletContext servletContext; // cached

    @Autowired
    MockHttpSession session;

    @Autowired
    MockHttpServletRequest request;

    @Autowired
    MockHttpServletResponse response;

    @Autowired
    ServletWebRequest webRequest;

    //...
}
```

#### 上下文缓存

一旦 TestContext 框架为测试加载了`ApplicationContext`(或`WebApplicationContext`)，该上下文将被缓存并重用于所有后续测试，这些测试在同一测试套件中声明相同的唯一上下文配置。要了解缓存的工作原理，了解“唯一”和“测试套件”的含义很重要。

一个`ApplicationContext`可通过的是，用于加载配置参数的组合来唯一地标识。因此，配置参数的唯一组合用于生成缓存上下文的密钥。TestContext 框架使用以下配置参数来构建上下文缓存键：

- `locations`(来自`@ContextConfiguration`)
- `classes`(来自`@ContextConfiguration`)
- `contextInitializerClasses`(来自`@ContextConfiguration`)
- `contextCustomizers`(from `ContextCustomizerFactory`) – 这包括 `@DynamicPropertySource`Spring Boot 的测试支持中的方法和各种特性，例如`@MockBean`和`@SpyBean`. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
- `contextLoader`(来自`@ContextConfiguration`)
- `parent`(来自`@ContextHierarchy`)
- `activeProfiles`(来自`@ActiveProfiles`)
- `propertySourceLocations`(来自`@TestPropertySource`)
- `propertySourceProperties`(来自`@TestPropertySource`)
- `resourceBasePath`(来自`@WebAppConfiguration`)

上下文缓存的大小受默认最大大小 32 的限制。每当达到最大大小时，就会使用最近最少使用 (LRU) 驱逐策略来驱逐和关闭陈旧的上下文。

### 3.5.7. 测试bean的依赖注入

第一个代码清单显示了`@Autowired`用于字段注入的测试类的基于 JUnit Jupiter 的实现：

```java
@ExtendWith(SpringExtension.class)
// specifies the Spring configuration to load for this test fixture
@ContextConfiguration("repository-config.xml")
class HibernateTitleRepositoryTests {

    // this instance will be dependency injected by type
    @Autowired
    HibernateTitleRepository titleRepository;

    @Test
    void findById() {
        Title title = titleRepository.findById(new Long(10));
        assertNotNull(title);
    }
}
```

可以将类配置`@Autowired`为用于 setter 注入，如下所示：

```java
@ExtendWith(SpringExtension.class)
// specifies the Spring configuration to load for this test fixture
@ContextConfiguration("repository-config.xml")
class HibernateTitleRepositoryTests {

    // this instance will be dependency injected by type
    HibernateTitleRepository titleRepository;

    @Autowired
    void setTitleRepository(HibernateTitleRepository titleRepository) {
        this.titleRepository = titleRepository;
    }

    @Test
    void findById() {
        Title title = titleRepository.findById(new Long(10));
        assertNotNull(title);
    }
}
```

前面的代码清单使用`@ContextConfiguration`注释引用的相同 XML 上下文文件 （即`repository-config.xml`）。下面显示了此配置：

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- this bean will be injected into the HibernateTitleRepositoryTests class -->
    <bean id="titleRepository" class="com.foo.repository.hibernate.HibernateTitleRepository">
        <property name="sessionFactory" ref="sessionFactory"/>
    </bean>

    <bean id="sessionFactory" class="org.springframework.orm.hibernate5.LocalSessionFactoryBean">
        <!-- configuration elided for brevity -->
    </bean>

</beans>
```

### 3.5.8. 测试请求范围和会话范围的 Bean

- `WebApplicationContext`通过用 注释您的测试类，确保为您的测试加载了a `@WebAppConfiguration`。
- 将模拟请求或会话注入到您的测试实例中，并根据需要准备您的测试装置。
- 调用从配置中检索到的 Web 组件 `WebApplicationContext`（使用依赖注入）。
- 对模拟执行断言。

**请求范围的 bean 配置**

```java
<beans>

    <bean id="userService" class="com.example.SimpleUserService"
            c:loginAction-ref="loginAction"/>

    <bean id="loginAction" class="com.example.LoginAction"
            c:username="#{request.getParameter('user')}"
            c:password="#{request.getParameter('pswd')}"
            scope="request">
        <aop:scoped-proxy/>
    </bean>

</beans>
```

可以根据用户名和密码的已知输入对结果执行断言。以下清单显示了如何执行此操作：

```java
@SpringJUnitWebConfig
class RequestScopedBeanTests {

    @Autowired UserService userService;
    @Autowired MockHttpServletRequest request;

    @Test
    void requestScope() {
        request.setParameter("user", "enigma");
        request.setParameter("pswd", "$pr!ng");

        LoginResults results = userService.loginUser();
        // assert results
    }
}
```

需要在 TestContext 框架管理的模拟会话中配置一个主题。以下示例显示了如何执行此操作：

```java
<beans>

    <bean id="userService" class="com.example.SimpleUserService"
            c:userPreferences-ref="userPreferences" />

    <bean id="userPreferences" class="com.example.UserPreferences"
            c:theme="#{session.getAttribute('theme')}"
            scope="session">
        <aop:scoped-proxy/>
    </bean>

</beans>
```

可以根据配置的主题对结果执行断言。以下示例显示了如何执行此操作：

```java
@SpringJUnitWebConfig
class SessionScopedBeanTests {

    @Autowired UserService userService;
    @Autowired MockHttpSession session;

    @Test
    void sessionScope() throws Exception {
        session.setAttribute("theme", "blue");

        Results results = userService.processUserPreferences();
        // assert results
    }
}
```

### 3.5.9. Transaction Management

在 TestContext 框架中，事务`TransactionalTestExecutionListener`由默认配置的管理 ，即使您没有`@TestExecutionListeners`在测试类上显式声明。但是，要启用对事务的支持，您必须`PlatformTransactionManager`在`ApplicationContext`加载了`@ContextConfiguration`语义的bean 中配置一个bean 。此外，您必须`@Transactional` 在类或方法级别为您的测试声明 Spring 的注释。

#### 测试管理事务

测试管理的事务是通过 using`TransactionalTestExecutionListener`或以编程方式通过 using `TestTransaction` （稍后描述）以声明方式管理的事务 。不应将此类事务与 Spring 管理的事务（在`ApplicationContext`加载的测试中直接由 Spring管理的事务）或应用程序管理的事务（在由测试调用的应用程序代码中以编程方式管理的事务）混淆。Spring 管理的和应用程序管理的事务通常参与测试管理的事务。

#### 启用和禁用事务

注释测试方法`@Transactional`会导致测试在事务中运行，默认情况下，在测试完成后自动回滚。如果测试类用 注释`@Transactional`，则该类层次结构中的每个测试方法都在事务中运行。没有注释的测试方法 `@Transactional`（在类或方法级别）不会在事务中运行。请注意，`@Transactional`不支持在测试生命周期方法-例如，使用JUnit木星的注解的方法`@BeforeAll`，`@BeforeEach`等等。此外，被标注了测试`@Transactional`，但有`propagation`属性设置 `NOT_SUPPORTED`或者`NEVER`没有在事务中运行。

| 属性                                        | 支持测试管理的事务                                       |
| :------------------------------------------ | :------------------------------------------------------- |
| `value` 和 `transactionManager`             | 是的                                                     |
| `propagation`                               | 只有`Propagation.NOT_SUPPORTED`和`Propagation.NEVER`支持 |
| `isolation`                                 | 不                                                       |
| `timeout`                                   | 不                                                       |
| `readOnly`                                  | 不                                                       |
| `rollbackFor` 和 `rollbackForClassName`     | 否：`TestTransaction.flagForRollback()`改为使用          |
| `noRollbackFor` 和 `noRollbackForClassName` | 否：`TestTransaction.flagForCommit()`改为使用            |

以下示例演示了为基于 Hibernate 的 编写集成测试的常见场景`UserRepository`：

```java
@SpringJUnitConfig(TestConfig.class)
@Transactional
class HibernateUserRepositoryTests {

    @Autowired
    HibernateUserRepository repository;

    @Autowired
    SessionFactory sessionFactory;

    JdbcTemplate jdbcTemplate;

    @Autowired
    void setDataSource(DataSource dataSource) {
        this.jdbcTemplate = new JdbcTemplate(dataSource);
    }

    @Test
    void createUser() {
        // track initial state in test database:
        final int count = countRowsInTable("user");

        User user = new User(...);
        repository.save(user);

        // Manual flush is required to avoid false positive in test
        sessionFactory.getCurrentSession().flush();
        assertNumUsers(count + 1);
    }

    private int countRowsInTable(String tableName) {
        return JdbcTestUtils.countRowsInTable(this.jdbcTemplate, tableName);
    }

    private void assertNumUsers(int expected) {
        assertEquals("Number of rows in the [user] table.", expected, countRowsInTable("user"));
    }
}
```

#### 事务回滚和提交行为

默认情况下，测试事务会在测试完成后自动回滚；但是，事务提交和回滚行为可以通过`@Commit`和`@Rollback`注释以声明方式配置。

#### 程序化事务管理

您可以通过使用 中的静态方法以编程方式与测试管理的事务交互`TestTransaction`。例如，您可以使用`TestTransaction`测试方法内、之前方法和之后方法来启动或结束当前测试管理的事务，或者配置当前测试管理的事务以进行回滚或提交。`TestTransaction`只要`TransactionalTestExecutionListener`启用，就会自动提供对 的支持。

以下示例演示了`TestTransaction`

```java
@ContextConfiguration(classes = TestConfig.class)
public class ProgrammaticTransactionManagementTests extends
        AbstractTransactionalJUnit4SpringContextTests {

    @Test
    public void transactionalTest() {
        // assert initial state in test database:
        assertNumUsers(2);

        deleteFromTables("user");

        // changes to the database will be committed!
        TestTransaction.flagForCommit();
        TestTransaction.end();
        assertFalse(TestTransaction.isActive());
        assertNumUsers(0);

        TestTransaction.start();
        // perform other actions against the database that will
        // be automatically rolled back after the test completes...
    }

    protected void assertNumUsers(int expected) {
        assertEquals("Number of rows in the [user] table.", expected, countRowsInTable("user"));
    }
}
```

#### 在事务之外运行代码

有时，您可能需要在事务测试方法之前或之后但在事务上下文之外运行某些代码——例如，在运行测试之前验证初始数据库状态或在测试运行之后验证预期的事务提交行为（如果测试被配置为提交事务）。 `TransactionalTestExecutionListener`支持`@BeforeTransaction`和 `@AfterTransaction`注释恰好适用于此类场景。您可以使用这些注释之一来注释`void` 测试类中的任何`void`方法或测试接口中的任何默认方法，并`TransactionalTestExecutionListener`确保您的交易前方法或交易后方法在适当的时间运行。

#### 配置事务管理器

`TransactionalTestExecutionListener`期望`PlatformTransactionManager`在 Spring 中`ApplicationContext`为测试定义一个bean 。如果`PlatformTransactionManager`测试的 中有多个实例，则`ApplicationContext`可以使用`@Transactional("myTxMgr")`或声明限定符`@Transactional(transactionManager = "myTxMgr")`，`TransactionManagementConfigurer`也可以通过`@Configuration`类来实现 。

#### 展示所有相关的注解

该示例`@Sql`用于具有默认事务回滚语义的声明性 SQL 脚本执行。以下示例显示了相关的注释

```java
@SpringJUnitConfig
@Transactional(transactionManager = "txMgr")
@Commit
class FictitiousTransactionalTest {

    @BeforeTransaction
    void verifyInitialDatabaseState() {
        // logic to verify the initial state before a transaction is started
    }

    @BeforeEach
    void setUpTestDataWithinTransaction() {
        // set up test data within the transaction
    }

    @Test
    // overrides the class-level @Commit setting
    @Rollback
    void modifyDatabaseWithinTransaction() {
        // logic which uses the test data and modifies database state
    }

    @AfterEach
    void tearDownWithinTransaction() {
        // run "tear down" logic within the transaction
    }

    @AfterTransaction
    void verifyFinalDatabaseState() {
        // logic to verify the final state after transaction has rolled back
    }

}
```

- [x] 测试ORM代码时避免报错

当测试操作 Hibernate 会话或 JPA 持久性上下文状态的应用程序代码时，请确保在运行该代码的测试方法中刷新底层工作单元。未能刷新底层工作单元可能会产生误报：您的测试通过，但相同的代码在实时生产环境中引发异常。请注意，这适用于任何维护内存工作单元的 ORM 框架。在以下基于 Hibernate 的示例测试用例中，一种方法演示了误报，另一种方法正确公开了刷新会话的结果：

```java
// ...

@Autowired
SessionFactory sessionFactory;

@Transactional
@Test // no expected exception!
public void falsePositive() {
    updateEntityInHibernateSession();
    // False positive: an exception will be thrown once the Hibernate
    // Session is finally flushed (i.e., in production code)
}

@Transactional
@Test(expected = ...)
public void updateWithSessionFlush() {
    updateEntityInHibernateSession();
    // Manual flush is required to avoid false positive in test
    sessionFactory.getCurrentSession().flush();
}

// ...
```

以下示例显示了 JPA 的匹配方法：

```java
// ...

@PersistenceContext
EntityManager entityManager;

@Transactional
@Test // no expected exception!
public void falsePositive() {
    updateEntityInJpaPersistenceContext();
    // False positive: an exception will be thrown once the JPA
    // EntityManager is finally flushed (i.e., in production code)
}

@Transactional
@Test(expected = ...)
public void updateWithEntityManagerFlush() {
    updateEntityInJpaPersistenceContext();
    // Manual flush is required to avoid false positive in test
    entityManager.flush();
}

// ...
```

### 3.5.10. 执行 SQL 脚本

#### 以编程方式执行 SQL 脚本

- `org.springframework.jdbc.datasource.init.ScriptUtils`
- `org.springframework.jdbc.datasource.init.ResourceDatabasePopulator`
- `org.springframework.test.context.junit4.AbstractTransactionalJUnit4SpringContextTests`
- `org.springframework.test.context.testng.AbstractTransactionalTestNGSpringContextTests`

以下示例为测试模式和测试数据指定 SQL 脚本，将语句分隔符设置为 `@@`，并针对`DataSource`：

```java
@Test
void databaseTest() {
    ResourceDatabasePopulator populator = new ResourceDatabasePopulator();
    populator.addScripts(
            new ClassPathResource("test-schema.sql"),
            new ClassPathResource("test-data.sql"));
    populator.setSeparator("@@");
    populator.execute(this.dataSource);
    // run code that uses the test schema and data
}
```

#### 使用@Sql 以声明方式执行 SQL 脚本

除了上述以编程方式运行 SQL 脚本的机制之外，还可以在 Spring TestContext Framework 中以声明方式配置 SQL 脚本。具体来说，可以`@Sql`在测试类或测试方法上声明注释，以配置单个 SQL 语句或 SQL 脚本的资源路径，这些 SQL 脚本应该在集成测试方法之前或之后针对给定数据库运行。支持 `@Sql`由提供的是`SqlScriptsTestExecutionListener`，这是默认启用。

#### 路径资源语义

以下示例显示了如何`@Sql`在基于 JUnit Jupiter 的集成测试类中的类级别和方法级别使用：

```java
@SpringJUnitConfig
@Sql("/test-schema.sql")
class DatabaseTests {

    @Test
    void emptySchemaTest() {
        // run code that uses the test schema without any test data
    }

    @Test
    @Sql({"/test-schema.sql", "/test-user-data.sql"})
    void userTest() {
        // run code that uses the test schema and test data
    }
}
```

#### 声明多个`@Sql`集合

```java
@Test
@Sql(scripts = "/test-schema.sql", config = @SqlConfig(commentPrefix = "`"))
@Sql("/test-user-data.sql")
void userTest() {
    // run code that uses the test schema and test data
}
```

#### 脚本执行阶段

需要在测试方法之后运行一组特定的脚本

```java
@Test
@Sql(
    scripts = "create-test-data.sql",
    config = @SqlConfig(transactionMode = ISOLATED)
)
@Sql(
    scripts = "delete-test-data.sql",
    config = @SqlConfig(transactionMode = ISOLATED),
    executionPhase = AFTER_TEST_METHOD
)
void userTest() {
    // run code that needs the test data to be committed
    // to the database outside of the test's transaction
}
```

#### 脚本配置 `@SqlConfig`

您可以使用`@SqlConfig`注释配置脚本解析和错误处理。当在集成测试类上声明为类级注释时，`@SqlConfig` 用作测试类层次结构中所有 SQL 脚本的全局配置。当使用注解的`config`属性直接声明时`@Sql`，`@SqlConfig` 作为在封闭`@Sql` 注解中声明的 SQL 脚本的本地配置。中的每个属性`@SqlConfig`都有一个隐式默认值，该值记录在相应属性的 javadoc 中。由于 Java 语言规范中为注释属性定义的规则，不幸的是，无法分配值`null`到注释属性。因此，为了支持继承的全局配置的覆盖，`@SqlConfig`属性具有`""`（对于字符串）、`{}`（对于数组）或`DEFAULT`（对于枚举）的显式默认值。这种方法通过提供、、 或以外的值，允许局部声明有`@SqlConfig`选择地覆盖全局声明中的个别属性。只要局部属性不提供、 或 以外的显式值，就会继承全局属性。因此，显式本地配置会覆盖全局配置。`@SqlConfig``""``{}``DEFAULT``@SqlConfig``@SqlConfig``""``{}``DEFAULT`

`@Sql`和提供的配置选项`@SqlConfig`等同于和支持的配置选项`ScriptUtils`，`ResourceDatabasePopulator`但它们是`<jdbc:initialize-database/>`XML 命名空间元素提供的配置选项的超集。

#### 事务管理 `@Sql`

下面的示例显示了典型的测试场景，使用JUnit的木星和交易测试与`@Sql`：

```java
@SpringJUnitConfig(TestDatabaseConfig.class)
@Transactional
class TransactionalSqlScriptsTests {

    final JdbcTemplate jdbcTemplate;

    @Autowired
    TransactionalSqlScriptsTests(DataSource dataSource) {
        this.jdbcTemplate = new JdbcTemplate(dataSource);
    }

    @Test
    @Sql("/test-data.sql")
    void usersTest() {
        // verify state in test database:
        assertNumUsers(2);
        // run code that uses the test data...
    }

    int countRowsInTable(String tableName) {
        return JdbcTestUtils.countRowsInTable(this.jdbcTemplate, tableName);
    }

    void assertNumUsers(int expected) {
        assertEquals(expected, countRowsInTable("user"),
            "Number of rows in the [user] table.");
    }
}
```

## 3.6. 网络测试客户端

`WebTestClient`是为测试服务器应用程序而设计的 HTTP 客户端。它包装了 Spring 的WebClient]并使用它来执行请求，但公开了一个用于验证响应的测试外观。`WebTestClient`可用于执行端到端 HTTP 测试。它还可用于通过模拟服务器请求和响应对象在没有运行服务器的情况下测试 Spring MVC 和 Spring WebFlux 应用程序。

### 3.6.1. 设置

要设置一个，`WebTestClient`您需要选择要绑定到的服务器设置。这可以是多个模拟服务器设置选项之一，也可以是与实时服务器的连接。

#### 绑定到控制器

此设置允许您通过模拟请求和响应对象测试特定控制器，而无需运行服务器。

对于 WebFlux 应用程序，使用以下内容加载与WebFlux Java config等效的基础设施 ，注册给定的控制器，并创建一个WebHandler 链 来处理请求：

```java
WebTestClient client =
        WebTestClient.bindToController(new TestController()).build();
```

对于 Spring MVC，使用以下委托给 StandaloneMockMvcBuilder来加载与WebMvc Java config等效的基础设施，注册给定的控制器，并创建MockMvc的实例 来处理请求：

```java
WebTestClient client =
        MockMvcWebTestClient.bindToController(new TestController()).build();
```

##### 绑定到 `ApplicationContext`

此设置允许您使用 Spring MVC 或 Spring WebFlux 基础结构和控制器声明加载 Spring 配置，并使用它通过模拟请求和响应对象处理请求，而无需运行服务器。

对于 WebFlux，使用以下代码，其中 Spring`ApplicationContext`被传递给 WebHttpHandlerBuilder 来创建WebHandler 链来处理请求：

```java
@SpringJUnitConfig(WebConfig.class) 
class MyTests {

    WebTestClient client;

    @BeforeEach
    void setUp(ApplicationContext context) {  
        client = WebTestClient.bindToApplicationContext(context).build(); 
    }
}
```

对于 Spring MVC，使用以下代码，其中 Spring`ApplicationContext`被传递给 MockMvcBuilders.webAppContextSetup来创建一个MockMvc实例来处理请求：

```java
@ExtendWith(SpringExtension.class)
@WebAppConfiguration("classpath:META-INF/web-resources") 
@ContextHierarchy({
    @ContextConfiguration(classes = RootConfig.class),
    @ContextConfiguration(classes = WebConfig.class)
})
class MyTests {

    @Autowired
    WebApplicationContext wac; 

    WebTestClient client;

    @BeforeEach
    void setUp() {
        client = MockMvcWebTestClient.bindToApplicationContext(this.wac).build(); 
    }
}
```

##### 绑定到路由器功能

此设置允许您通过模拟请求和响应对象测试功能端点，而无需运行服务器。

对于 WebFlux，使用以下委托`RouterFunctions.toWebHandler`来创建服务器设置来处理请求：

```java
RouterFunction<?> route = ...
client = WebTestClient.bindToRouterFunction(route).build();
```

##### 绑定到服务器

此设置连接到正在运行的服务器以执行完整的端到端 HTTP 测试：

```java
client = WebTestClient.bindToServer().baseUrl("http://localhost:8080").build();
```

##### 客户端配置

除了前面描述的服务器设置选项，您还可以配置客户端选项，包括基本 URL、默认标头、客户端过滤器等。这些选项在下面很容易获得`bindToServer()`。对于所有其他配置选项，您需要使用`configureClient()`从服务器到客户端配置的转换，如下所示：

```java
client = WebTestClient.bindToController(new TestController())
        .configureClient()
        .baseUrl("/test")
        .build();
```

#### 3.6.2. 编写测试

`WebTestClient`提供与WebClient相同的 API， 直到使用`exchange()`. 有关如何准备包含表单数据、多部分数据等任何内容的请求的示例，

在调用 之后`exchange()`，`WebTestClient`与 不同`WebClient`，而是继续执行工作流以验证响应。

要断言响应状态和标头，请使用以下命令：

```java
client.get().uri("/persons/1")
            .accept(MediaType.APPLICATION_JSON)
            .exchange()
            .expectStatus().isOk()
            .expectHeader().contentType(MediaType.APPLICATION_JSON)
```

可以选择通过以下方式之一解码响应正文：

- `expectBody(Class<T>)`: 解码为单个对象。
- `expectBodyList(Class<T>)`：解码并收集对象到`List<T>`.
- `expectBody()`：解码`byte[]`为JSON 内容或空体。

并对生成的更高级别的对象执行断言：

```java
client.get().uri("/persons")
        .exchange()
        .expectStatus().isOk()
        .expectBodyList(Person.class).hasSize(3).contains(person);
```

如果内置断言不足，您可以使用该对象并执行任何其他断言：

```java
import org.springframework.test.web.reactive.server.expectBody

client.get().uri("/persons/1")
        .exchange()
        .expectStatus().isOk()
        .expectBody(Person.class)
        .consumeWith(result -> {
            // custom assertions (e.g. AssertJ)...
        });
```

或者可以退出工作流程并获得`EntityExchangeResult`：

```java
EntityExchangeResult<Person> result = client.get().uri("/persons/1")
        .exchange()
        .expectStatus().isOk()
        .expectBody(Person.class)
        .returnResult();
```

#### 无内容

如果预期响应没有内容，您可以如下断言：

```java
client.post().uri("/persons")
        .body(personMono, Person.class)
        .exchange()
        .expectStatus().isCreated()
        .expectBody().isEmpty();
```

如果你想忽略响应内容，以下释放内容，不做任何断言：

```java
client.get().uri("/persons/123")
        .exchange()
        .expectStatus().isNotFound()
        .expectBody(Void.class);
```

##### JSON 内容

可以在`expectBody()`没有目标类型的情况下使用对原始内容执行断言，而不是通过更高级别的对象。

要使用JSONAssert验证完整的 JSON 内容：

```java
client.get().uri("/persons/1")
        .exchange()
        .expectStatus().isOk()
        .expectBody()
        .json("{\"name\":\"Jane\"}")
```

使用JSONPath验证 JSON 内容：

```java
client.get().uri("/persons")
        .exchange()
        .expectStatus().isOk()
        .expectBody()
        .jsonPath("$[0].name").isEqualTo("Jane")
        .jsonPath("$[1].name").isEqualTo("Jason");
```

#### 流响应

要测试潜在的无限流，例如`"text/event-stream"`或 `"application/x-ndjson"`，请首先验证响应状态和标头，然后获取`FluxExchangeResult`：

```java
FluxExchangeResult<MyEvent> result = client.get().uri("/events")
        .accept(TEXT_EVENT_STREAM)
        .exchange()
        .expectStatus().isOk()
        .returnResult(MyEvent.class);
```

现在已准备好使用`StepVerifier`from使用响应流`reactor-test`：

```java
Flux<Event> eventFlux = result.getResponseBody();

StepVerifier.create(eventFlux)
        .expectNext(person)
        .expectNextCount(4)
        .consumeNextWith(p -> ...)
        .thenCancel()
        .verify();
```

#### MockMvc 断言

`WebTestClient` 是一个 HTTP 客户端，因此它只能验证客户端响应中的内容，包括状态、标头和正文。

在使用 MockMvc 服务器设置测试 Spring MVC 应用程序时，您有额外的选择来对服务器响应执行进一步的断言。要做到这一点，首先要`ExchangeResult`在断言主体之后获得一个：

```java
// For a response with a body
EntityExchangeResult<Person> result = client.get().uri("/persons/1")
        .exchange()
        .expectStatus().isOk()
        .expectBody(Person.class)
        .returnResult();

// For a response without a body
EntityExchangeResult<Void> result = client.get().uri("/path")
        .exchange()
        .expectBody().isEmpty();
```

然后切换到 MockMvc 服务器响应断言：

```java
MockMvcWebTestClient.resultActionsFor(result)
        .andExpect(model().attribute("integer", 3))
        .andExpect(model().attribute("string", "a string value"));
```

## 3.7. 模拟Mvc

Spring MVC 测试框架，也称为 MockMvc，为测试 Spring MVC 应用程序提供支持。它执行完整的 Spring MVC 请求处理，但通过模拟请求和响应对象而不是正在运行的服务器。

### 3.7.1. 概述

您可以通过实例化一个控制器、向其注入依赖项并调用其方法来为 Spring MVC 编写简单的单元测试。然而，这种测试不验证请求的映射，数据绑定，消息转换，类型转换，验证和它们也不涉及任何的支撑`@InitBinder`，`@ModelAttribute`或 `@ExceptionHandler`方法。

Spring MVC 测试框架，也称为`MockMvc`，旨在为 Spring MVC 控制器提供更完整的测试，而无需运行服务器。它通过从模块中调用`DispacherServlet`和传递 Servlet API 的“模拟”实现来实现这一点，该 `spring-test`模块复制了完整的 Spring MVC 请求处理，而无需运行服务器。

MockMvc 是一个服务器端测试框架，可让您使用轻量级和有针对性的测试来验证 Spring MVC 应用程序的大部分功能。您可以单独使用它来执行请求和验证响应，或者您也可以通过WebTestClientAPI使用它，并将 插入作为服务器来处理请求。

#### 静态导入

当直接使用 MockMvc 执行请求时，您需要静态导入：

- `MockMvcBuilders.*`
- `MockMvcRequestBuilders.*`
- `MockMvcResultMatchers.*`
- `MockMvcResultHandlers.*`

一个简单的记住方法是搜索`MockMvc*`. 如果使用 Eclipse，请务必在 Eclipse 首选项中将上述内容添加为“最喜欢的静态成员”。

通过WebTestClient)使用 MockMvc 时，您不需要静态导入。在`WebTestClient`无静态导入提供了流畅的API。

#### 设置选项

MockMvc 可以通过两种方式之一进行设置。一种是直接指向要测试的控制器并以编程方式配置 Spring MVC 基础设施。第二个是指向包含 Spring MVC 和控制器基础设施的 Spring 配置。

要设置 MockMvc 以测试特定控制器，请使用以下命令：

```java
class MyWebTests {

    MockMvc mockMvc;

    @BeforeEach
    void setup() {
        this.mockMvc = MockMvcBuilders.standaloneSetup(new AccountController()).build();
    }

    // ...

}
```

要通过 Spring 配置设置 MockMvc，请使用以下命令：

```java
@SpringJUnitWebConfig(locations = "my-servlet-context.xml")
class MyWebTests {

    MockMvc mockMvc;

    @BeforeEach
    void setup(WebApplicationContext wac) {
        this.mockMvc = MockMvcBuilders.webAppContextSetup(this.wac).build();
    }

    // ...

}
```

在`webAppContextSetup`加载您的实际Spring MVC的配置，带来了更完整的集成测试。由于 TestContext 框架缓存加载的 Spring 配置，它有助于保持测试快速运行，即使您在测试套件中引入了更多测试。此外，您可以通过 Spring 配置将模拟服务注入控制器，以保持专注于测试 web 层。以下示例使用 Mockito 声明了一个模拟服务：

```java
<bean id="accountService" class="org.mockito.Mockito" factory-method="mock">
    <constructor-arg value="org.example.AccountService"/>
</bean>
```

可以将模拟服务注入测试以设置和验证您的期望，如以下示例所示：

```java
@SpringJUnitWebConfig(locations = "test-servlet-context.xml")
class AccountTests {

    @Autowired
    AccountService accountService;

    MockMvc mockMvc;

    @BeforeEach
    void setup(WebApplicationContext wac) {
        this.mockMvc = MockMvcBuilders.webAppContextSetup(wac).build();
    }

    // ...

}
```

#### 设置功能

无论使用哪种 MockMvc 构建器，所有`MockMvcBuilder`实现都提供了一些通用且非常有用的功能。例如，可以`Accept`为所有请求声明一个标头，并期望状态为 200 以及`Content-Type`所有响应中的标头，如下所示：

```java
// static import of MockMvcBuilders.standaloneSetup

MockMvc mockMvc = standaloneSetup(new MusicController())
    .defaultRequest(get("/").accept(MediaType.APPLICATION_JSON))
    .alwaysExpect(status().isOk())
    .alwaysExpect(content().contentType("application/json;charset=UTF-8"))
    .build();
```

第三方框架（和应用程序）可以预先打包安装说明，例如`MockMvcConfigurer`. Spring Framework 有一个这样的内置实现，它有助于跨请求保存和重用 HTTP 会话。可以按如下方式使用它：

```java
// static import of SharedHttpSessionConfigurer.sharedHttpSession

MockMvc mockMvc = MockMvcBuilders.standaloneSetup(new TestController())
        .apply(sharedHttpSession())
        .build();

// Use mockMvc to perform requests...
```

#### 执行请求

要执行使用任何 HTTP 方法的请求，如以下示例所示：

```java
// static import of MockMvcRequestBuilders.*

mockMvc.perform(post("/hotels/{id}", 42).accept(MediaType.APPLICATION_JSON));
```

还可以执行内部使用的文件上传请求， `MockMultipartHttpServletRequest`这样就不会对多部分请求进行实际解析。相反，必须将其设置为类似于以下示例：

```java
mockMvc.perform(multipart("/doc").file("a1", "ABC".getBytes("UTF-8")));
```

可以在 URI 模板样式中指定查询参数，如以下示例所示：

```java
mockMvc.perform(get("/hotels?thing={thing}", "somewhere"));
```

还可以添加表示查询或表单参数的 Servlet 请求参数，如以下示例所示：

```java
mockMvc.perform(get("/hotels").param("thing", "somewhere"));
```

若必须使用完整的请求 URI 进行测试，请确保相应地设置`contextPath` 和`servletPath`以便请求映射工作，如以下示例所示：

```java
mockMvc.perform(get("/app/main/hotels/{id}").contextPath("/app").servletPath("/main"))
```

在前面的示例中，为每个执行的请求设置`contextPath`和 会很麻烦`servletPath`。相反，可以设置默认请求属性，如以下示例所示：

```java
class MyWebTests {

    MockMvc mockMvc;

    @BeforeEach
    void setup() {
        mockMvc = standaloneSetup(new AccountController())
            .defaultRequest(get("/")
            .contextPath("/app").servletPath("/main")
            .accept(MediaType.APPLICATION_JSON)).build();
    }
}
```

#### 定义期望

可以通过`.andExpect(..)`在执行请求后附加一个或多个调用来定义期望，如以下示例所示：

```java
// static import of MockMvcRequestBuilders.* and MockMvcResultMatchers.*

mockMvc.perform(get("/accounts/1")).andExpect(status().isOk());
```

以下测试断言绑定或验证失败：

```java
mockMvc.perform(post("/persons"))
    .andExpect(status().isOk())
    .andExpect(model().attributeHasErrors("person"));
```

很多时候，在编写测试时，转储执行请求的结果很有用。可以按如下方式进行，其中`print()`静态导入 from `MockMvcResultHandlers`

```java
mockMvc.perform(post("/persons"))
    .andDo(print())
    .andExpect(status().isOk())
    .andExpect(model().attributeHasErrors("person"));
```

## 3.8. 测试客户端应用程序

可以使用客户端测试来测试内部使用`RestTemplate`. 这个想法是声明预期的请求并提供“存根”响应，以便可以专注于隔离地测试代码（即，无需运行服务器）。以下示例显示了如何执行此操作：

```java
RestTemplate restTemplate = new RestTemplate();

MockRestServiceServer mockServer = MockRestServiceServer.bindTo(restTemplate).build();
mockServer.expect(requestTo("/greeting")).andRespond(withSuccess());

// Test code that uses the above RestTemplate ...

mockServer.verify();
```

默认情况下，请求按照声明期望的顺序进行。ignoreExpectOrder`在构建服务器时设置该选项，在这种情况下，将检查所有期望（按顺序）以找到给定请求的匹配项。这意味着允许请求以任何顺序出现。以下示例使用`ignoreExpectOrder`：

```java
server = MockRestServiceServer.bindTo(restTemplate).ignoreExpectOrder(true).build();
```

即使默认情况下是无序请求，每个请求也只允许运行一次。该`expect`方法提供了一个重载变体，该变体接受`ExpectedCount` 指定计数范围的参数（例如，`once`、`manyTimes`、`max`、`min`、 `between`等）。以下示例使用`times`：

```java
RestTemplate restTemplate = new RestTemplate();

MockRestServiceServer mockServer = MockRestServiceServer.bindTo(restTemplate).build();
mockServer.expect(times(2), requestTo("/something")).andRespond(withSuccess());
mockServer.expect(times(3), requestTo("/somewhere")).andRespond(withSuccess());

// ...

mockServer.verify();
```

客户端测试支持还提供了一个 `ClientHttpRequestFactory`实现，您可以`RestTemplate`将其配置到 中以将其绑定到`MockMvc`实例。这允许使用实际的服务器端逻辑处理请求，但无需运行服务器。以下示例显示了如何执行此操作：

```java
MockMvc mockMvc = MockMvcBuilders.webAppContextSetup(this.wac).build();
this.restTemplate = new RestTemplate(new MockMvcClientHttpRequestFactory(mockMvc));

// Test code that uses the above RestTemplate ...
```

### 3.8.1. 静态导入

与服务器端测试一样，用于客户端测试的 fluent API 需要一些静态导入。这些很容易通过搜索找到`MockRest*`。Eclipse 用户应在 Java → 编辑器 → 内容辅助 → 收藏夹下的 Eclipse 首选项中添加 `MockRestRequestMatchers.*`和`MockRestResponseCreators.*`作为“收藏的静态成员”。这允许在键入静态方法名称的第一个字符后使用内容辅助。其他 IDE（例如 IntelliJ）可能不需要任何额外的配置。检查对静态成员的代码完成支持。
