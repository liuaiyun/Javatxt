# 数据访问

## 1. Transaction Management

全面的事务支持是使用 Spring Framework 的最令人信服的理由之一。Spring Framework 为事务管理提供了一致的抽象，具有以下优点：

- 跨不同事务 API 的一致编程模型，例如 Java 事务 API (JTA)、JDBC、Hibernate 和 Java Persistence API (JPA)。
- 支持声明式事务管理。
- 比复杂的事务 API（例如 JTA）更简单的用于程序化事务管理的 API。
- 与 Spring 的数据访问抽象的完美集成。

以下部分描述了 Spring Framework 的事务特性和技术：

- Spring Framework 事务支持模型的优点描述了为什么要使用 Spring Framework 的事务抽象而不是 EJB 容器管理事务 (CMT) 或选择通过专有 API（例如 Hibernate）驱动本地事务。
- 理解 Spring Framework 事务抽象 概述了核心类并描述了如何`DataSource` 从各种来源配置和获取实例。
- 将资源与事务同步描述了应用程序代码如何确保正确创建、重用和清理资源。
- 声明式事务管理描述了对声明式事务管理的支持。
- 程序化事务管理涵盖了对程序化（即显式编码）事务管理的支持。
- 事务绑定事件描述了如何在事务中使用应用程序事件。

### 1.1. Spring Framework 的事务支持模型的优点

传统上，Java EE 开发人员对事务管理有两种选择：全局事务或本地事务，两者都有很大的局限性。在接下来的两节中回顾全局和本地事务管理，然后讨论 Spring Framework 的事务管理支持如何解决全局和本地事务模型的局限性。

#### 1.1.1. Global Transactions

全局事务让您可以使用多个事务资源，通常是关系数据库和消息队列。

#### 1.1.2. Local Transactions

本地事务是特定于资源的，例如与 JDBC 连接关联的事务。本地事务可能更易于使用，但有一个明显的缺点：它们不能跨多个事务资源工作。

#### 1.1.3. Spring Framework 的一致编程模型

Spring 解决了全局事务和本地事务的缺点。它允许应用程序开发人员在任何环境中使用一致的编程模型。

通过编程事务管理，开发人员可以使用 Spring Framework 事务抽象，它可以在任何底层事务基础架构上运行。使用首选声明式模型，开发人员通常很少编写或不编写与事务管理相关的代码，因此不依赖于 Spring Framework 事务 API 或任何其他事务 API。

### 1.2. 理解 Spring 框架事务抽象

Spring 事务抽象的关键是事务策略的概念。事务策略由 定义`TransactionManager`，特别是 `org.springframework.transaction.PlatformTransactionManager`命令式事务管理的`org.springframework.transaction.ReactiveTransactionManager`接口和反应式事务管理的 接口。以下清单显示了`PlatformTransactionManager`API的定义 ：

```java
public interface PlatformTransactionManager extends TransactionManager {

    TransactionStatus getTransaction(TransactionDefinition definition) throws TransactionException;

    void commit(TransactionStatus status) throws TransactionException;

    void rollback(TransactionStatus status) throws TransactionException;
}
```

以下清单显示了由 定义的事务策略 `org.springframework.transaction.ReactiveTransactionManager`：

```java
public interface ReactiveTransactionManager extends TransactionManager {

    Mono<ReactiveTransaction> getReactiveTransaction(TransactionDefinition definition) throws TransactionException;

    Mono<Void> commit(ReactiveTransaction status) throws TransactionException;

    Mono<Void> rollback(ReactiveTransaction status) throws TransactionException;
}
```

反应式事务管理器主要是一个服务提供者接口 (SPI)，尽管您可以从应用程序代码中以[编程](https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#transaction-programmatic-rtm)方式使用它。因为`ReactiveTransactionManager`是一个接口，所以可以根据需要轻松模拟或存根。

该`TransactionDefinition`接口指定：

- 传播：通常，事务范围内的所有代码都在该事务中运行。但是，如果在事务上下文已经存在时运行事务方法，您可以指定行为。例如，代码可以在现有事务中继续运行（常见情况），或者可以暂停现有事务并创建新事务。Spring 提供了 EJB CMT 中熟悉的所有事务传播选项。
- 隔离度：此事务与其他事务的工作隔离的程度。例如，这个事务可以看到来自其他事务的未提交的写入吗？
- 超时：此事务在超时和被底层事务基础设施自动回滚之前运行的时间。
- 只读状态：当您的代码读取但不修改数据时，您可以使用只读事务。在某些情况下，只读事务可能是一种有用的优化，例如当您使用 Hibernate 时。

该`TransactionStatus`接口为事务代码提供了一种简单的方式来控制事务执行和查询事务状态。这些概念应该很熟悉，因为它们对所有事务 API 都是通用的。以下清单显示了 `TransactionStatus`界面：

```java
public interface TransactionStatus extends TransactionExecution, SavepointManager, Flushable {

    @Override
    boolean isNewTransaction();

    boolean hasSavepoint();

    @Override
    void setRollbackOnly();

    @Override
    boolean isRollbackOnly();

    void flush();

    @Override
    boolean isCompleted();
}
```

以下示例展示了如何定义本地`PlatformTransactionManager`实现（在本例中，使用普通 JDBC。）

datasource

```java
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
    <property name="driverClassName" value="${jdbc.driverClassName}" />
    <property name="url" value="${jdbc.url}" />
    <property name="username" value="${jdbc.username}" />
    <property name="password" value="${jdbc.password}" />
</bean>
```

然后相关的`PlatformTransactionManager`bean 定义有一个对`DataSource`定义的引用 。它应该类似于以下示例：

```java
<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>
```

如果在 Java EE 容器中使用 JTA，那么将`DataSource`通过 JNDI 获得的容器与 Spring 的`JtaTransactionManager`. 以下示例显示了 JTA 和 JNDI 查找版本的样子：

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:jee="http://www.springframework.org/schema/jee"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/jee
        https://www.springframework.org/schema/jee/spring-jee.xsd">

    <jee:jndi-lookup id="dataSource" jndi-name="jdbc/jpetstore"/>

    <bean id="txManager" class="org.springframework.transaction.jta.JtaTransactionManager" />

    <!-- other <bean/> definitions here -->

</beans>
```

#### 1.2.1. 休眠事务设置

还可以轻松地使用 Hibernate 本地事务，

以下示例声明`sessionFactory`和`txManager`bean：

```java
<bean id="sessionFactory" class="org.springframework.orm.hibernate5.LocalSessionFactoryBean">
    <property name="dataSource" ref="dataSource"/>
    <property name="mappingResources">
        <list>
            <value>org/springframework/samples/petclinic/hibernate/petclinic.hbm.xml</value>
        </list>
    </property>
    <property name="hibernateProperties">
        <value>
            hibernate.dialect=${hibernate.dialect}
        </value>
    </property>
</bean>

<bean id="txManager" class="org.springframework.orm.hibernate5.HibernateTransactionManager">
    <property name="sessionFactory" ref="sessionFactory"/>
</bean>
```

如果使用 Hibernate 和 Java EE 容器管理的 JTA 事务，应该使用与`JtaTransactionManager`前面的 JTA 示例相同的 JDBC，如以下示例所示。此外，建议通过其事务协调器以及可能的连接释放模式配置使 Hibernate 了解 JTA：

```java
<bean id="sessionFactory" class="org.springframework.orm.hibernate5.LocalSessionFactoryBean">
    <property name="dataSource" ref="dataSource"/>
    <property name="mappingResources">
        <list>
            <value>org/springframework/samples/petclinic/hibernate/petclinic.hbm.xml</value>
        </list>
    </property>
    <property name="hibernateProperties">
        <value>
            hibernate.dialect=${hibernate.dialect}
            hibernate.transaction.coordinator_class=jta
            hibernate.connection.handling_mode=DELAYED_ACQUISITION_AND_RELEASE_AFTER_STATEMENT
        </value>
    </property>
</bean>

<bean id="txManager" class="org.springframework.transaction.jta.JtaTransactionManager"/>
```

或者，可以传递给`JtaTransactionManager` `LocalSessionFactoryBean` 以强制执行相同的默认值：

```java
<bean id="sessionFactory" class="org.springframework.orm.hibernate5.LocalSessionFactoryBean">
    <property name="dataSource" ref="dataSource"/>
    <property name="mappingResources">
        <list>
            <value>org/springframework/samples/petclinic/hibernate/petclinic.hbm.xml</value>
        </list>
    </property>
    <property name="hibernateProperties">
        <value>
            hibernate.dialect=${hibernate.dialect}
        </value>
    </property>
    <property name="jtaTransactionManager" ref="txManager"/>
</bean>

<bean id="txManager" class="org.springframework.transaction.jta.JtaTransactionManager"/>
```

### 1.3. 将资源与事务同步

如何创建不同的事务管理器以及它们如何链接到需要同步到事务的相关资源（例如`DataSourceTransactionManager` 到 JDBC `DataSource`、`HibernateTransactionManager`到 Hibernate`SessionFactory`等）现在应该很清楚了。

#### 1.3.1. 高级同步方法

首选方法是使用 Spring 的最高级别基于模板的持久性集成 API，或者使用带有事务感知工厂 bean 或代理的本机 ORM API 来管理本机资源工厂。

#### 1.3.2. 低级同步方法

诸如`DataSourceUtils`（对于JDBC）、`EntityManagerFactoryUtils`（对于JPA）、 `SessionFactoryUtils`（对于Hibernate）等类存在于较低级别。当您希望应用程序代码直接处理原生持久化 API 的资源类型时，您可以使用这些类来确保获得适当的 Spring Framework 管理的实例，事务（可选）同步，并且过程中发生的异常是正确映射到一致的 API。

例如，在JDBC的情况下，您可以使用Spring的类代替 传统的JDBC方式在 上调用`getConnection()`方法，如下所示：`DataSource``org.springframework.jdbc.datasource.DataSourceUtils`

```java
Connection conn = DataSourceUtils.getConnection(dataSource);
```

如果现有事务已经有一个与之同步（链接）的连接，则返回该实例。否则，该方法调用会触发新连接的创建，该连接（可选）同步到任何现有事务，并可供后续在同一事务中重用。如前所述， any `SQLException`包装在 Spring Framework 中`CannotGetJdbcConnectionException`，Spring Framework 的未检查`DataAccessException`类型层次结构之一。这种方法提供了比从 中轻松获取的更多信息，`SQLException`并确保了跨数据库甚至跨不同持久性技术的可移植性。

这种方法也可以在没有 Spring 事务管理的情况下工作（事务同步是可选的），因此无论是否使用 Spring 进行事务管理，都可以使用它。

当然，一旦使用了 Spring 的 JDBC 支持、JPA 支持或 Hibernate 支持通常不喜使用`DataSourceUtils`或其他辅助类，因为通过 Spring 抽象工作比直接使用相关 API 更快乐。例如，如果使用 Spring`JdbcTemplate`或 `jdbc.object`包来简化您对 JDBC 的使用，则正确的连接检索发生在幕后，无需编写任何特殊代码。

#### 1.3.3. `TransactionAwareDataSourceProxy`

在最底层存在`TransactionAwareDataSourceProxy`类。这是一个 target 的代理`DataSource`，它包装了 target`DataSource`以增加对 Spring 管理的事务的感知。在这方面，它类似于`DataSource`Java EE 服务器提供的事务性 JNDI 。

几乎不需要或不想使用此类，除非必须调用现有代码并传递标准 JDBC`DataSource`接口实现。在这种情况下，此代码可能可用但正在参与 Spring 管理的事务。可以使用前面提到的更高级别的抽象来编写新代码。

### 1.4. 声明式事务管理

Spring Framework 的声明式事务管理通过 Spring 面向方面的编程 (AOP) 成为可能。但是，由于事务方面代码随 Spring Framework 发行版一起提供并且可以样板方式使用，因此通常不必理解 AOP 概念即可有效使用此代码。

Spring Framework 的声明式事务管理类似于 EJB CMT，因为可以将事务行为（或缺少它）指定到单个方法级别。`setRollbackOnly()`如有必要，可以在事务上下文中进行调用。两种类型的事务管理之间的区别是：

- 与绑定到 JTA 的 EJB CMT 不同，Spring Framework 的声明式事务管理可在任何环境中工作。它可以通过调整配置文件使用 JDBC、JPA 或 Hibernate 来处理 JTA 事务或本地事务。
- 可以将 Spring Framework 声明式事务管理应用于任何类，而不仅仅是诸如 EJB 之类的特殊类。
- Spring Framework 提供了声明式 回滚规则，这是一项没有 EJB 等效项的特性。提供了对回滚规则的编程和声明性支持。
- Spring Framework 允许您使用 AOP 自定义事务行为。例如，可以在事务回滚的情况下插入自定义行为。还可以添加任意建议以及事务建议。使用 EJB CMT，不能影响容器的事务管理，除非使用 `setRollbackOnly()`.
- Spring Framework 不支持跨远程调用传播事务上下文，高端应用程序服务器支持。如果需要此功能，建议使用 EJB。但是，在使用此类功能之前请仔细考虑，因为通常情况下，人们不希望事务跨越远程调用。

回滚规则的概念很重要。它们指定哪些异常（和可抛出的）应该导致自动回滚。可以在配置中（而不是在 Java 代码中）以声明方式指定此项。所以，尽管仍然可以调用`setRollbackOnly()`上的`TransactionStatus`对象回滚当前事务回，经常可以指定一个规则，`MyApplicationException`必须始终导致回滚。此选项的显着优点是业务对象不依赖于事务基础结构。例如，他们通常不需要导入 Spring 事务 API 或其他 Spring API。

尽管 EJB 容器默认行为会在发生系统异常（通常是运行时异常）时自动回滚事务，但 EJB CMT 不会在发生应用程序异常（即除 之外的已检查异常`java.rmi.RemoteException`）时自动回滚事务。虽然声明性事务管理的 Spring 默认行为遵循 EJB 约定（仅在未检查的异常时自动回滚），但自定义此行为通常很有用。

#### 1.4.1. 理解 Spring 框架的声明式事务实现

使用注释对类进行`@Transactional`注释、添加`@EnableTransactionManagement`到您的配置中并期望了解它的工作原理是不够的 。

关于 Spring Framework 的声明式事务支持，需要掌握的最重要的概念是这种支持是通过 AOP 代理启用的 ，并且事务建议是由元数据（目前基于 XML 或注解）驱动的。AOP 与事务元数据的组合产生了一个 AOP 代理，该代理`TransactionInterceptor`结合适当的`TransactionManager`实现来驱动围绕方法调用的事务。

Spring Framework`TransactionInterceptor`为命令式和反应式编程模型提供事务管理。拦截器通过检查方法返回类型来检测所需的事务管理风格。返回响应式类型的方法，例如`Publisher`或 Kotlin `Flow`（或它们的子类型）有资格进行响应式事务管理。所有其他返回类型包括`void`使用代码路径进行命令式事务管理。	

#### 1.4.2. 声明式事务实现示例

考虑以下接口及其伴随的实现。此示例使用 `Foo`和`Bar`类作为占位符，以便可以专注于事务使用，而无需关注特定的域模型。就本示例而言，`DefaultFooService`类`UnsupportedOperationException` 在每个实现的方法的主体中抛出实例这一事实很好。该行为让您可以看到正在创建的事务，然后回滚以响应 `UnsupportedOperationException`实例。以下清单显示了`FooService` 界面：

```java
// the service interface that we want to make transactional

package x.y.service;

public interface FooService {

    Foo getFoo(String fooName);

    Foo getFoo(String fooName, String barName);

    void insertFoo(Foo foo);

    void updateFoo(Foo foo);

}
```

以下示例显示了上述接口的实现：

```java
package x.y.service;

public class DefaultFooService implements FooService {

    @Override
    public Foo getFoo(String fooName) {
        // ...
    }

    @Override
    public Foo getFoo(String fooName, String barName) {
        // ...
    }

    @Override
    public void insertFoo(Foo foo) {
        // ...
    }

    @Override
    public void updateFoo(Foo foo) {
        // ...
    }
}
```

假设`FooService`接口的前两个方法`getFoo(String)`and `getFoo(String, String)`必须在具有只读语义的事务上下文中运行，而其他方法`insertFoo(Foo)`and`updateFoo(Foo)`必须在具有读写语义的事务上下文中运行。下面几段详细解释下面的配置：

```java
<!-- from the file 'context.xml' -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:tx="http://www.springframework.org/schema/tx"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/tx
        https://www.springframework.org/schema/tx/spring-tx.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- this is the service object that we want to make transactional -->
    <bean id="fooService" class="x.y.service.DefaultFooService"/>

    <!-- the transactional advice (what 'happens'; see the <aop:advisor/> bean below) -->
    <tx:advice id="txAdvice" transaction-manager="txManager">
        <!-- the transactional semantics... -->
        <tx:attributes>
            <!-- all methods starting with 'get' are read-only -->
            <tx:method name="get*" read-only="true"/>
            <!-- other methods use the default transaction settings (see below) -->
            <tx:method name="*"/>
        </tx:attributes>
    </tx:advice>

    <!-- ensure that the above transactional advice runs for any execution
        of an operation defined by the FooService interface -->
    <aop:config>
        <aop:pointcut id="fooServiceOperation" expression="execution(* x.y.service.FooService.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="fooServiceOperation"/>
    </aop:config>

    <!-- don't forget the DataSource -->
    <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
        <property name="driverClassName" value="oracle.jdbc.driver.OracleDriver"/>
        <property name="url" value="jdbc:oracle:thin:@rj-t42:1521:elvis"/>
        <property name="username" value="scott"/>
        <property name="password" value="tiger"/>
    </bean>

    <!-- similarly, don't forget the TransactionManager -->
    <bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!-- other <bean/> definitions here -->

</beans>
```

一个常见的要求是使整个服务层具有事务性。最好的方法是更改切入点表达式以匹配服务层中的任何操作。以下示例显示了如何执行此操作：

```java
<aop:config>
    <aop:pointcut id="fooServiceMethods" expression="execution(* x.y.service.*.*(..))"/>
    <aop:advisor advice-ref="txAdvice" pointcut-ref="fooServiceMethods"/>
</aop:config>
```

前面显示的配置用于围绕从`fooService`bean 定义创建的对象创建事务代理。代理配置有事务建议，以便在代理上调用适当的方法时，根据与该方法关联的事务配置，启动、暂停、标记为只读等事务。考虑以下测试驱动前面显示的配置的程序：

```java
public final class Boot {

    public static void main(final String[] args) throws Exception {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("context.xml");
        FooService fooService = ctx.getBean(FooService.class);
        fooService.insertFoo(new Foo());
    }
}
```

运行上述程序的输出应类似于以下内容（为清晰起见，该类`UnsupportedOperationException`的`insertFoo(..)`方法抛出 的 Log4J 输出和堆栈跟踪`DefaultFooService`已被截断）：

```java
<!-- the Spring container is starting up... -->
[AspectJInvocationContextExposingAdvisorAutoProxyCreator] - Creating implicit proxy for bean 'fooService' with 0 common interceptors and 1 specific interceptors

<!-- the DefaultFooService is actually proxied -->
[JdkDynamicAopProxy] - Creating JDK dynamic proxy for [x.y.service.DefaultFooService]

<!-- ... the insertFoo(..) method is now being invoked on the proxy -->
[TransactionInterceptor] - Getting transaction for x.y.service.FooService.insertFoo

<!-- the transactional advice kicks in here... -->
[DataSourceTransactionManager] - Creating new transaction with name [x.y.service.FooService.insertFoo]
[DataSourceTransactionManager] - Acquired Connection [org.apache.commons.dbcp.PoolableConnection@a53de4] for JDBC transaction

<!-- the insertFoo(..) method from DefaultFooService throws an exception... -->
[RuleBasedTransactionAttribute] - Applying rules to determine whether transaction should rollback on java.lang.UnsupportedOperationException
[TransactionInterceptor] - Invoking rollback for transaction on x.y.service.FooService.insertFoo due to throwable [java.lang.UnsupportedOperationException]

<!-- and the transaction is rolled back (by default, RuntimeException instances cause rollback) -->
[DataSourceTransactionManager] - Rolling back JDBC transaction on Connection [org.apache.commons.dbcp.PoolableConnection@a53de4]
[DataSourceTransactionManager] - Releasing JDBC Connection after transaction
[DataSourceUtils] - Returning JDBC Connection to DataSource

Exception in thread "main" java.lang.UnsupportedOperationException at x.y.service.DefaultFooService.insertFoo(DefaultFooService.java:14)
<!-- AOP infrastructure stack trace elements removed for clarity -->
at $Proxy0.insertFoo(Unknown Source)
at Boot.main(Boot.java:11)
```

以下清单显示了之前使用的修改版本`FooService`，但这次代码使用了反应式类型：

```java
// the reactive service interface that we want to make transactional

package x.y.service;

public interface FooService {

    Flux<Foo> getFoo(String fooName);

    Publisher<Foo> getFoo(String fooName, String barName);

    Mono<Void> insertFoo(Foo foo);

    Mono<Void> updateFoo(Foo foo);

}
```

以下示例显示了上述接口的实现：

```java
package x.y.service;

public class DefaultFooService implements FooService {

    @Override
    public Flux<Foo> getFoo(String fooName) {
        // ...
    }

    @Override
    public Publisher<Foo> getFoo(String fooName, String barName) {
        // ...
    }

    @Override
    public Mono<Void> insertFoo(Foo foo) {
        // ...
    }

    @Override
    public Mono<Void> updateFoo(Foo foo) {
        // ...
    }
}
```

#### 1.4.3. 回滚声明性事务

向 Spring Framework 的事务基础结构指示要回滚事务的工作的推荐方法是抛出`Exception`当前在事务上下文中执行的from 代码。Spring Framework 的事务基础结构代码会`Exception`在调用堆栈冒泡时捕获任何未处理的内容，并确定是否将事务标记为回滚。

在其默认配置中，Spring Framework 的事务基础结构代码仅在运行时、未检查异常的情况下才将事务标记为回滚。也就是说，当抛出的异常是`RuntimeException`. （ `Error`默认情况下，实例也会导致回滚）。从事务方法抛出的已检查异常不会导致默认配置中的回滚。

您可以准确配置哪些`Exception`类型将事务标记为回滚，包括已检查的异常。以下 XML 片段演示了如何为已检查的特定于应用程序的`Exception`类型配置回滚：

```java
<tx:advice id="txAdvice" transaction-manager="txManager">
    <tx:attributes>
    <tx:method name="get*" read-only="true" rollback-for="NoProductInStockException"/>
    <tx:method name="*"/>
    </tx:attributes>
</tx:advice>
```

不希望在抛出异常时回滚事务，还可以指定“无回滚规则”。以下示例告诉 Spring Framework 的事务基础结构即使面对未处理的事务也要提交伴随的事务`InstrumentNotFoundException`：

```java
<tx:advice id="txAdvice">
    <tx:attributes>
    <tx:method name="updateStock" no-rollback-for="InstrumentNotFoundException"/>
    <tx:method name="*"/>
    </tx:attributes>
</tx:advice>
```

当 Spring Framework 的事务基础结构捕获异常并查询配置的回滚规则以确定是否将事务标记为回滚时，最强匹配规则获胜。因此，在以下配置的情况下，除`InstrumentNotFoundException`a之外的任何异常都会导致伴随事务回滚：

```java
<tx:advice id="txAdvice">
    <tx:attributes>
    <tx:method name="*" rollback-for="Throwable" no-rollback-for="InstrumentNotFoundException"/>
    </tx:attributes>
</tx:advice>
```

还可以以编程方式指示需要的回滚。尽管很简单，但这个过程非常具有侵入性，并且将代码与 Spring Framework 的事务基础结构紧密耦合。以下示例显示了如何以编程方式指示所需的回滚：

```java
public void resolvePosition() {
    try {
        // some business logic...
    } catch (NoProductInStockException ex) {
        // trigger rollback programmatically
        TransactionAspectSupport.currentTransactionStatus().setRollbackOnly();
    }
}
```

#### 1.4.4. 为不同的 Bean 配置不同的事务语义

考虑这样一个场景，您有多个服务层对象，并且您希望对每个对象应用完全不同的事务配置。您可以通过定义不同的做`<aop:advisor/>`用不同的元素`pointcut`和 `advice-ref`属性值。

作为比较点，首先假设您的所有服务层类都定义在一个根`x.y.service`包中。要使作为该包（或子包）中定义的类的实例的所有 bean`Service`具有默认事务配置，您可以编写以下内容：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:tx="http://www.springframework.org/schema/tx"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/tx
        https://www.springframework.org/schema/tx/spring-tx.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <aop:config>

        <aop:pointcut id="serviceOperation"
                expression="execution(* x.y.service..*Service.*(..))"/>

        <aop:advisor pointcut-ref="serviceOperation" advice-ref="txAdvice"/>

    </aop:config>

    <!-- these two beans will be transactional... -->
    <bean id="fooService" class="x.y.service.DefaultFooService"/>
    <bean id="barService" class="x.y.service.extras.SimpleBarService"/>

    <!-- ... and these two beans won't -->
    <bean id="anotherService" class="org.xyz.SomeService"/> <!-- (not in the right package) -->
    <bean id="barManager" class="x.y.service.SimpleBarManager"/> <!-- (doesn't end in 'Service') -->

    <tx:advice id="txAdvice">
        <tx:attributes>
            <tx:method name="get*" read-only="true"/>
            <tx:method name="*"/>
        </tx:attributes>
    </tx:advice>

    <!-- other transaction infrastructure beans such as a TransactionManager omitted... -->

</beans>
```

以下示例显示如何使用完全不同的事务设置配置两个不同的 bean：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:tx="http://www.springframework.org/schema/tx"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/tx
        https://www.springframework.org/schema/tx/spring-tx.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <aop:config>

        <aop:pointcut id="defaultServiceOperation"
                expression="execution(* x.y.service.*Service.*(..))"/>

        <aop:pointcut id="noTxServiceOperation"
                expression="execution(* x.y.service.ddl.DefaultDdlManager.*(..))"/>

        <aop:advisor pointcut-ref="defaultServiceOperation" advice-ref="defaultTxAdvice"/>

        <aop:advisor pointcut-ref="noTxServiceOperation" advice-ref="noTxAdvice"/>

    </aop:config>

    <!-- this bean will be transactional (see the 'defaultServiceOperation' pointcut) -->
    <bean id="fooService" class="x.y.service.DefaultFooService"/>

    <!-- this bean will also be transactional, but with totally different transactional settings -->
    <bean id="anotherFooService" class="x.y.service.ddl.DefaultDdlManager"/>

    <tx:advice id="defaultTxAdvice">
        <tx:attributes>
            <tx:method name="get*" read-only="true"/>
            <tx:method name="*"/>
        </tx:attributes>
    </tx:advice>

    <tx:advice id="noTxAdvice">
        <tx:attributes>
            <tx:method name="*" propagation="NEVER"/>
        </tx:attributes>
    </tx:advice>

    <!-- other transaction infrastructure beans such as a TransactionManager omitted... -->

</beans>
```

#### 1.4.5. <tx:advice/> 设置

本节总结了您可以使用`<tx:advice/>`标签指定的各种事务设置。默认`<tx:advice/>`设置为：

- 传播环境是`REQUIRED.`
- 隔离级别为 `DEFAULT.`
- 事务是读写的。
- 事务超时默认为底层事务系统的默认超时，如果不支持超时，则无。
- 任何`RuntimeException`触发器都会回滚，任何检查`Exception`都不会。

您可以更改这些默认设置。下表总结了`<tx:method/>`嵌套在`<tx:advice/>`和`<tx:attributes/>`标签中的标签的各种属性：

| 属性              | 必需的？ | 默认       | 描述                                                         |
| :---------------- | :------- | :--------- | :----------------------------------------------------------- |
| `name`            | 是的     |            | 与事务属性相关联的方法名称。通配符 (*) 可用于将相同的事务属性设置与多种方法（例如，`get*`、`handle*`、`on*Event`等）相关联。 |
| `propagation`     | 不       | `REQUIRED` | 事务传播行为。                                               |
| `isolation`       | 不       | `DEFAULT`  | 事务隔离级别。仅适用于`REQUIRED`或 的传播设置`REQUIRES_NEW`。 |
| `timeout`         | 不       | -1         | 事务超时（秒）。仅适用于传播`REQUIRED`或`REQUIRES_NEW`.      |
| `read-only`       | 不       | 错误的     | 读写与只读事务。仅适用于`REQUIRED`或`REQUIRES_NEW`。         |
| `rollback-for`    | 不       |            | `Exception`触发回滚的以逗号分隔的实例列表。例如， `com.foo.MyBusinessException,ServletException`。 |
| `no-rollback-for` | 不       |            | `Exception`不触发回滚的以逗号分隔的实例列表。例如， `com.foo.MyBusinessException,ServletException`。 |

#### 1.4.6. 使用`@Transactional`

除了用于事务配置的基于 XML 的声明性方法之外，您还可以使用基于注释的方法。直接在 Java 源代码中声明事务语义会使声明更接近受影响的代码。不存在过度耦合的危险，因为旨在以事务方式使用的代码几乎总是以这种方式部署的。

|      | 标准`javax.transaction.Transactional`注解也被支持作为 Spring 自己注解的替代品。有关更多详细信息，请参阅 JTA 1.2 文档。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

使用`@Transactional`注释提供的易用性最好用一个例子来说明，这在下面的文本中进行了解释。考虑以下类定义：

```java
// the service class that we want to make transactional
@Transactional
public class DefaultFooService implements FooService {

    @Override
    public Foo getFoo(String fooName) {
        // ...
    }

    @Override
    public Foo getFoo(String fooName, String barName) {
        // ...
    }

    @Override
    public void insertFoo(Foo foo) {
        // ...
    }

    @Override
    public void updateFoo(Foo foo) {
        // ...
    }
}
```

如上在类级别使用，注释指示声明类（及其子类）的所有方法的默认值。或者，每个方法都可以单独注释。请参阅方法可见性以及`@Transactional`有关 Spring 将哪些方法视为事务[性](https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#transaction-declarative-annotations-method-visibility)的更多详细信息。请注意，类级别的注释不适用于在类层次结构中向上的祖先类；在这种情况下，需要在本地重新声明继承的方法才能参与子类级别的注释。

当上面的 POJO 类在 Spring 上下文中定义为 bean 时，您可以通过类中的`@EnableTransactionManagement` annotation使 bean 实例具有事务性`@Configuration`。有关 完整详细信息，请参阅 [javadoc](https://docs.spring.io/spring-framework/docs/5.3.8/javadoc-api/org/springframework/transaction/annotation/EnableTransactionManagement.html)。

在 XML 配置中，`<tx:annotation-driven/>`标签提供了类似的便利：

```xml
<!-- from the file 'context.xml' -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:tx="http://www.springframework.org/schema/tx"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/tx
        https://www.springframework.org/schema/tx/spring-tx.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- this is the service object that we want to make transactional -->
    <bean id="fooService" class="x.y.service.DefaultFooService"/>

    <!-- enable the configuration of transactional behavior based on annotations -->
    <!-- a TransactionManager is still required -->
    <tx:annotation-driven transaction-manager="txManager"/> 

    <bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!-- (this dependency is defined somewhere else) -->
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!-- other <bean/> definitions here -->

</beans>
```

与命令式编程安排相比，反应式事务方法使用反应式返回类型，如下面的清单所示：

```java
// the reactive service class that we want to make transactional
@Transactional
public class DefaultFooService implements FooService {

    @Override
    public Publisher<Foo> getFoo(String fooName) {
        // ...
    }

    @Override
    public Mono<Foo> getFoo(String fooName, String barName) {
        // ...
    }

    @Override
    public Mono<Void> insertFoo(Foo foo) {
        // ...
    }

    @Override
    public Mono<Void> updateFoo(Foo foo) {
        // ...
    }
}
```

请注意，`Publisher`对于 Reactive Streams 取消信号的返回有特殊考虑。有关更多详细信息，请参阅“使用 TransactionOperator”下的取消信号)部分。

| XML 属性              | 注释属性                                                     | 默认                        | 描述                                                         |
| :-------------------- | :----------------------------------------------------------- | :-------------------------- | :----------------------------------------------------------- |
| `transaction-manager` | 不适用（请参阅[`TransactionManagementConfigurer`](https://docs.spring.io/spring-framework/docs/5.3.8/javadoc-api/org/springframework/transaction/annotation/TransactionManagementConfigurer.html)javadoc） | `transactionManager`        | 要使用的事务管理器的名称。仅当事务管理器的名称不是 时才需要`transactionManager`，如前面的示例所示。 |
| `mode`                | `mode`                                                       | `proxy`                     | 默认模式 ( `proxy`) 处理使用 Spring 的 AOP 框架代理的带注释的 bean（遵循代理语义，如前所述，仅适用于通过代理传入的方法调用）。替代模式 ( `aspectj`) 而是将受影响的类与 Spring 的 AspectJ 事务方面编织在一起，修改目标类字节码以应用于任何类型的方法调用。AspectJ 编织需要 `spring-aspects.jar`在类路径中以及启用加载时编织（或编译时编织）。（有关 如何设置加载时编织的详细信息，请参阅[Spring 配置](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop-aj-ltw-spring)。） |
| `proxy-target-class`  | `proxyTargetClass`                                           | `false`                     | `proxy`仅适用于模式。控制为用`@Transactional`注释注释的类创建什么类型的事务代理。如果该 `proxy-target-class`属性设置为`true`，则会创建基于类的代理。如果`proxy-target-class`是`false`或属性被省略，则创建标准的基于 JDK 接口的代理。（有关 不同代理类型的详细检查，请参阅[代理机制](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop-proxying)。） |
| `order`               | `order`                                                      | `Ordered.LOWEST_PRECEDENCE` | 定义应用于用 注释的 bean 的事务建议的顺序 `@Transactional`。（有关与 AOP 通知排序相关的规则的更多信息，请参阅[Advice Ordering](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop-ataspectj-advice-ordering)。）没有指定的排序意味着 AOP 子系统决定了通知的顺序。 |

在以下示例的情况下，`DefaultFooService`类在类级别使用只读事务的设置进行`@Transactional`注释，但`updateFoo(Foo)`同一类中方法的 注释优先于在类级别定义的事务设置。

```java
@Transactional(readOnly = true)
public class DefaultFooService implements FooService {

    public Foo getFoo(String fooName) {
        // ...
    }

    // these settings have precedence for this method
    @Transactional(readOnly = false, propagation = Propagation.REQUIRES_NEW)
    public void updateFoo(Foo foo) {
        // ...
    }
}
```

##### `@Transactional` 设置

的`@Transactional`注释是元数据，指定的接口，类或方法必须有事务语义（例如，“调用该方法时启动一个全新的只读事务，悬浮剂任何现有的交易”）。默认`@Transactional`设置如下：

- 传播设置是 `PROPAGATION_REQUIRED.`
- 隔离级别为 `ISOLATION_DEFAULT.`
- 事务是读写的。
- 事务超时默认为底层事务系统的默认超时，如果不支持超时，则为无。
- 任何`RuntimeException`触发器都会回滚，任何检查`Exception`都不会。

您可以更改这些默认设置。下表总结了`@Transactional`注释的各种属性：

| 财产                     | 类型                                         | 描述                                                         |
| :----------------------- | :------------------------------------------- | :----------------------------------------------------------- |
| value                    | `String`                                     | 指定要使用的事务管理器的可选限定符。                         |
| propagation              | `enum`： `Propagation`                       | 可选的传播设置。                                             |
| `isolation`              | `enum`： `Isolation`                         | 可选的隔离级别。仅适用于`REQUIRED`或 的传播值`REQUIRES_NEW`。 |
| `timeout`                | `int` （以秒为单位）                         | 可选的事务超时。仅适用于`REQUIRED`或 的传播值`REQUIRES_NEW`。 |
| `readOnly`               | `boolean`                                    | 读写与只读事务。仅适用于`REQUIRED`或 的值`REQUIRES_NEW`。    |
| `rollbackFor`            | `Class`对象数组，必须派生自`Throwable.`      | 必须导致回滚的异常类的可选数组。                             |
| `rollbackForClassName`   | 类名数组。类必须派生自`Throwable.`           | 必须导致回滚的异常类名称的可选数组。                         |
| `noRollbackFor`          | `Class`对象数组，必须派生自`Throwable.`      | 不得导致回滚的异常类的可选数组。                             |
| `noRollbackForClassName` | `String`类名数组，必须派生自`Throwable.`     | 不得导致回滚的异常类名称的可选数组。                         |
| `label`                  | `String`用于向交易添加表达性描述的标签数组。 | 标签可以由事务管理器评估以将特定于实现的行为与实际事务相关联。 |

##### 多个事务管理器 `@Transactional`

大多数 Spring 应用程序只需要一个事务管理器，但可能存在在单个应用程序中需要多个独立事务管理器的情况。您可以使用注释的`value`or`transactionManager`属性 `@Transactional`来选择性地指定`TransactionManager`要使用的标识 。这可以是事务管理器 bean 的 bean 名称或限定符值。例如，使用限定符表示法，您可以将以下 Java 代码与应用程序上下文中的以下事务管理器 bean 声明相结合：

```java
public class TransactionalService {

    @Transactional("order")
    public void setSomething(String name) { ... }

    @Transactional("account")
    public void doSomething() { ... }

    @Transactional("reactive-account")
    public Mono<Void> doSomethingReactive() { ... }
}
```

以下清单显示了 bean 声明：

```xml
<tx:annotation-driven/>

    <bean id="transactionManager1" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        ...
        <qualifier value="order"/>
    </bean>

    <bean id="transactionManager2" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        ...
        <qualifier value="account"/>
    </bean>

    <bean id="transactionManager3" class="org.springframework.data.r2dbc.connectionfactory.R2dbcTransactionManager">
        ...
        <qualifier value="reactive-account"/>
    </bean>
```

在这种情况下，在各个方法`TransactionalService`下单独的事务管理器运行，通过差异化`order`，`account`和`reactive-account` 预选赛。如果未找到特别限定的bean ，则仍使用默认`<tx:annotation-driven>`目标 bean 名称。`transactionManager``TransactionManager`

##### 自定义组合注释

如果您发现`@Transactional`在许多不同的方法上重复使用相同的属性，[Spring 的元注释支持](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-meta-annotations)允许您为特定用例定义自定义组合注释。例如，考虑以下注释定义：

```java
@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Transactional(transactionManager = "order", label = "causal-consistency")
public @interface OrderTx {
}

@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Transactional(transactionManager = "account", label = "retryable")
public @interface AccountTx {
}
```

前面的注释让我们将上一节中的示例编写如下：

```java
public class TransactionalService {

    @OrderTx
    public void setSomething(String name) {
        // ...
    }

    @AccountTx
    public void doSomething() {
        // ...
    }
}
```

#### 1.4.8. 为交易操作提供建议

调用该`updateFoo(Foo)`方法时，希望看到以下操作：

- 配置的分析方面开始。
- 事务性建议运行。
- 建议对象上的方法运行。
- 事务提交。
- 分析方面报告整个事务方法调用的确切持续时间。

以下代码显示了前面讨论的简单分析方面：

```java
package x.y;

import org.aspectj.lang.ProceedingJoinPoint;
import org.springframework.util.StopWatch;
import org.springframework.core.Ordered;

public class SimpleProfiler implements Ordered {

    private int order;

    // allows us to control the ordering of advice
    public int getOrder() {
        return this.order;
    }

    public void setOrder(int order) {
        this.order = order;
    }

    // this method is the around advice
    public Object profile(ProceedingJoinPoint call) throws Throwable {
        Object returnValue;
        StopWatch clock = new StopWatch(getClass().getName());
        try {
            clock.start(call.toShortString());
            returnValue = call.proceed();
        } finally {
            clock.stop();
            System.out.println(clock.prettyPrint());
        }
        return returnValue;
    }
}
```

建议的顺序是通过`Ordered`界面控制的。

以下配置创建了一个`fooService`bean，该bean 以所需的顺序应用了分析和事务方面：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:tx="http://www.springframework.org/schema/tx"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/tx
        https://www.springframework.org/schema/tx/spring-tx.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <bean id="fooService" class="x.y.service.DefaultFooService"/>

    <!-- this is the aspect -->
    <bean id="profiler" class="x.y.SimpleProfiler">
        <!-- run before the transactional advice (hence the lower order number) -->
        <property name="order" value="1"/>
    </bean>

    <tx:annotation-driven transaction-manager="txManager" order="200"/>

    <aop:config>
            <!-- this advice runs around the transactional advice -->
            <aop:aspect id="profilingAspect" ref="profiler">
                <aop:pointcut id="serviceMethodWithReturnValue"
                        expression="execution(!void x.y..*Service.*(..))"/>
                <aop:around method="profile" pointcut-ref="serviceMethodWithReturnValue"/>
            </aop:aspect>
    </aop:config>

    <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
        <property name="driverClassName" value="oracle.jdbc.driver.OracleDriver"/>
        <property name="url" value="jdbc:oracle:thin:@rj-t42:1521:elvis"/>
        <property name="username" value="scott"/>
        <property name="password" value="tiger"/>
    </bean>

    <bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>

</beans>
```

可以以类似的方式配置任意数量的附加方面。

以下示例创建与前两个示例相同的设置，但使用纯 XML 声明方法：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:tx="http://www.springframework.org/schema/tx"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/tx
        https://www.springframework.org/schema/tx/spring-tx.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <bean id="fooService" class="x.y.service.DefaultFooService"/>

    <!-- the profiling advice -->
    <bean id="profiler" class="x.y.SimpleProfiler">
        <!-- run before the transactional advice (hence the lower order number) -->
        <property name="order" value="1"/>
    </bean>

    <aop:config>
        <aop:pointcut id="entryPointMethod" expression="execution(* x.y..*Service.*(..))"/>
        <!-- runs after the profiling advice (c.f. the order attribute) -->

        <aop:advisor advice-ref="txAdvice" pointcut-ref="entryPointMethod" order="2"/>
        <!-- order value is higher than the profiling aspect -->

        <aop:aspect id="profilingAspect" ref="profiler">
            <aop:pointcut id="serviceMethodWithReturnValue"
                    expression="execution(!void x.y..*Service.*(..))"/>
            <aop:around method="profile" pointcut-ref="serviceMethodWithReturnValue"/>
        </aop:aspect>

    </aop:config>

    <tx:advice id="txAdvice" transaction-manager="txManager">
        <tx:attributes>
            <tx:method name="get*" read-only="true"/>
            <tx:method name="*"/>
        </tx:attributes>
    </tx:advice>

    <!-- other <bean/> definitions such as a DataSource and a TransactionManager here -->

</beans>
```

### 1.5. 程序化事务管理

Spring 框架提供了两种编程事务管理的方法，通过使用：

- 的`TransactionTemplate`或`TransactionalOperator`。
- 一个`TransactionManager`直接的实现。

#### 1.5.1. 使用`TransactionTemplate`

以下示例显示了如何执行此操作：

```java
public class SimpleService implements Service {

    // single TransactionTemplate shared amongst all methods in this instance
    private final TransactionTemplate transactionTemplate;

    // use constructor-injection to supply the PlatformTransactionManager
    public SimpleService(PlatformTransactionManager transactionManager) {
        this.transactionTemplate = new TransactionTemplate(transactionManager);
    }

    public Object someServiceMethod() {
        return transactionTemplate.execute(new TransactionCallback() {
            // the code in this method runs in a transactional context
            public Object doInTransaction(TransactionStatus status) {
                updateOperation1();
                return resultOfUpdateOperation2();
            }
        });
    }
}
```

如果没有返回值，可以使用方便的`TransactionCallbackWithoutResult`类和匿名类，如下：

```java
transactionTemplate.execute(new TransactionCallbackWithoutResult() {
    protected void doInTransactionWithoutResult(TransactionStatus status) {
        updateOperation1();
        updateOperation2();
    }
});
```

回调中的代码可以通过调用`setRollbackOnly()`提供的`TransactionStatus`对象上的方法来回滚事务 ，如下所示：

```java
transactionTemplate.execute(new TransactionCallbackWithoutResult() {

    protected void doInTransactionWithoutResult(TransactionStatus status) {
        try {
            updateOperation1();
            updateOperation2();
        } catch (SomeBusinessException ex) {
            status.setRollbackOnly();
        }
    }
});
```

##### 指定事务设置

您可以`TransactionTemplate`以编程方式或在配置中指定事务设置（例如传播模式、隔离级别、超时等）。默认情况下，`TransactionTemplate`实例具有 [默认事务设置](https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#transaction-declarative-txadvice-settings)。以下示例显示了特定交易设置的编程自定义`TransactionTemplate:`

```java
public class SimpleService implements Service {

    private final TransactionTemplate transactionTemplate;

    public SimpleService(PlatformTransactionManager transactionManager) {
        this.transactionTemplate = new TransactionTemplate(transactionManager);

        // the transaction settings can be set here explicitly if so desired
        this.transactionTemplate.setIsolationLevel(TransactionDefinition.ISOLATION_READ_UNCOMMITTED);
        this.transactionTemplate.setTimeout(30); // 30 seconds
        // and so forth...
    }
}
```

以下示例`TransactionTemplate`使用 Spring XML 配置定义了一些自定义事务设置：

```xml
<bean id="sharedTransactionTemplate"
        class="org.springframework.transaction.support.TransactionTemplate">
    <property name="isolationLevelName" value="ISOLATION_READ_UNCOMMITTED"/>
    <property name="timeout" value="30"/>
</bean>
```

然后，您可以`sharedTransactionTemplate` 根据需要将其注入到尽可能多的服务中。

最后，`TransactionTemplate`类的实例是线程安全的，因为实例不维护任何会话状态。`TransactionTemplate`但是，实例会维护配置状态。因此，虽然多个类可能共享 a 的单个实例`TransactionTemplate`，但如果一个类需要使用`TransactionTemplate`具有不同设置（例如，不同的隔离级别）的 ，则需要创建两个不同的`TransactionTemplate`实例。

#### 1.5.2. 使用`TransactionOperator`

在`TransactionOperator`如下操作者设计是类似于其它反应性运算符。它使用回调方法（使应用程序代码免于执行样板获取和释放事务资源）并产生意图驱动的代码，因为您的代码只关注您想要做什么。

|      | 正如下面的示例所示，使用`TransactionOperator`绝对会将您与 Spring 的事务基础结构和 API 结合起来。程序化事务管理是否适合您的开发需求是您必须自己做出的决定。 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

必须在事务上下文中运行并显式使用 的应用程序代码`TransactionOperator`类似于下一个示例

```java
public class SimpleService implements Service {

    // single TransactionOperator shared amongst all methods in this instance
    private final TransactionalOperator transactionalOperator;

    // use constructor-injection to supply the ReactiveTransactionManager
    public SimpleService(ReactiveTransactionManager transactionManager) {
        this.transactionOperator = TransactionalOperator.create(transactionManager);
    }

    public Mono<Object> someServiceMethod() {

        // the code in this method runs in a transactional context

        Mono<Object> update = updateOperation1();

        return update.then(resultOfUpdateOperation2).as(transactionalOperator::transactional);
    }
}
```

`TransactionalOperator` 可以通过两种方式使用：

- 操作员风格使用 Project Reactor 类型 ( `mono.as(transactionalOperator::transactional)`)
- 其他情况的回调样式 ( `transactionalOperator.execute(TransactionCallback<T>)`)

回调中的代码可以通过调用`setRollbackOnly()` 提供的`ReactiveTransaction`对象上的方法来回滚事务，如下所示：

```java
transactionalOperator.execute(new TransactionCallback<>() {

    public Mono<Object> doInTransaction(ReactiveTransaction status) {
        return updateOperation1().then(updateOperation2)
                    .doOnError(SomeBusinessException.class, e -> status.setRollbackOnly());
        }
    }
});
```

##### 指定事务设置

以下示例显示了针对特定事务的事务设置的自定义 `TransactionalOperator:`

```java
public class SimpleService implements Service {

    private final TransactionalOperator transactionalOperator;

    public SimpleService(ReactiveTransactionManager transactionManager) {
        DefaultTransactionDefinition definition = new DefaultTransactionDefinition();

        // the transaction settings can be set here explicitly if so desired
        definition.setIsolationLevel(TransactionDefinition.ISOLATION_READ_UNCOMMITTED);
        definition.setTimeout(30); // 30 seconds
        // and so forth...

        this.transactionalOperator = TransactionalOperator.create(transactionManager, definition);
    }
}
```

#### 1.5.3. 使用`TransactionManager`	

##### 使用 `PlatformTransactionManager`

对于命令式事务，`org.springframework.transaction.PlatformTransactionManager`直接使用 a 来管理您的事务。为此，`PlatformTransactionManager`请通过 bean 引用将您使用的实现传递给您的 bean。然后，通过使用`TransactionDefinition`和 `TransactionStatus`对象，您可以启动事务、回滚和提交。以下示例显示了如何执行此操作：

```java
DefaultTransactionDefinition def = new DefaultTransactionDefinition();
// explicitly setting the transaction name is something that can be done only programmatically
def.setName("SomeTxName");
def.setPropagationBehavior(TransactionDefinition.PROPAGATION_REQUIRED);

TransactionStatus status = txManager.getTransaction(def);
try {
    // put your business logic here
}
catch (MyException ex) {
    txManager.rollback(status);
    throw ex;
}
txManager.commit(status);
```

##### 使用 `ReactiveTransactionManager`

使用反应式事务时，org.springframework.transaction.ReactiveTransactionManager`直接使用 a 来管理您的事务。为此，`ReactiveTransactionManager`请通过 bean 引用将您使用的实现传递给您的 bean。然后，通过使用`TransactionDefinition`和 `ReactiveTransaction`对象，您可以启动事务、回滚和提交。以下示例显示了如何执行此操作：

```java
DefaultTransactionDefinition def = new DefaultTransactionDefinition();
// explicitly setting the transaction name is something that can be done only programmatically
def.setName("SomeTxName");
def.setPropagationBehavior(TransactionDefinition.PROPAGATION_REQUIRED);

Mono<ReactiveTransaction> reactiveTx = txManager.getReactiveTransaction(def);

reactiveTx.flatMap(status -> {

    Mono<Object> tx = ...; // put your business logic here

    return tx.then(txManager.commit(status))
            .onErrorResume(ex -> txManager.rollback(status).then(Mono.error(ex)));
```

## 2. DAO 支持

Spring 中的数据访问对象 (DAO) 支持旨在以一致的方式轻松使用数据访问技术（例如 JDBC、Hibernate 或 JPA）。这让您可以相当轻松地在上述持久性技术之间切换，并且还可以让您编写代码而不必担心捕获特定于每种技术的异常。

### 2.1. 一致的异常层次结构

pring 提供了从特定于技术的异常的方便转换，例如 `SQLException`到其自己的异常类层次结构，它具有`DataAccessException`作为根异常。这些异常包含了原始异常，因此永远不会有丢失有关可能出错的任何信息的风险。

除了 JDBC 异常之外，Spring 还可以包装 JPA 和 Hibernate 特定的异常，将它们转换为一组重点关注的运行时异常。可以仅在适当的层中处理大多数不可恢复的持久性异常，而无需在 DAO 中使用烦人的样板捕获和抛出块和异常声明。

### 2.2. 用于配置 DAO 或存储库类的注释

保证您的数据访问对象 (DAO) 或存储库提供异常转换的最佳方法是使用`@Repository`注释。此注释还允许组件扫描支持查找和配置您的 DAO 和存储库，而无需为它们提供 XML 配置条目。以下示例显示了如何使用`@Repository`注释：

爪哇

科特林

```java
@Repository 
public class SomeMovieFinder implements MovieFinder {
    // ...
}
```

|      | 该`@Repository`注解。 |
| ---- | --------------------- |
|      |                       |

任何 DAO 或存储库实现都需要访问持久性资源，具体取决于所使用的持久性技术。例如，基于 JDBC 的存储库需要访问 JDBC `DataSource`，而基于 JPA 的存储库需要访问 `EntityManager`. 做到这一点最简单的方法就是有这个资源依赖使用的一个注入`@Autowired`，`@Inject`，`@Resource`或`@PersistenceContext` 注解。以下示例适用于 JPA 存储库：

```java
@Repository
public class JpaMovieFinder implements MovieFinder {

    @PersistenceContext
    private EntityManager entityManager;

    // ...
}
```

```java
@Repository
public class HibernateMovieFinder implements MovieFinder {

    private SessionFactory sessionFactory;

    @Autowired
    public void setSessionFactory(SessionFactory sessionFactory) {
        this.sessionFactory = sessionFactory;
    }

    // ...
}
```

我们在这里展示的最后一个例子是典型的 JDBC 支持。可以将 `DataSource`注入到初始化方法或构造函数中，可以在其中使用 this创建 a `JdbcTemplate`和其他数据访问支持类（例如`SimpleJdbcCall`和其他类）`DataSource`。以下示例自动装配 a `DataSource`：

```java
@Repository
public class JdbcMovieFinder implements MovieFinder {

    private JdbcTemplate jdbcTemplate;

    @Autowired
    public void init(DataSource dataSource) {
        this.jdbcTemplate = new JdbcTemplate(dataSource);
    }

    // ...
}
```

|                                |      |      |
| :----------------------------- | :--- | :--- |
| 行动                           | 春天 | 你   |
| 定义连接参数。                 |      | X    |
| 打开连接。                     | X    |      |
| 指定 SQL 语句。                |      | X    |
| 声明参数并提供参数值           |      | X    |
| 准备并运行语句。               | X    |      |
| 设置循环以迭代结果（如果有）。 | X    |      |
| 为每次迭代做工作。             |      | X    |
| 处理任何异常。                 | X    |      |
| 处理交易。                     | X    |      |
| 关闭连接、语句和结果集。       | X    |      |