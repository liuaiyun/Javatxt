# Spring  Data JPA

## 1.使用Spring Data Repositories

### 1.1核心概念

Spring Data存储库抽象中的中心接口时Repository，需要域类来管理以及域类的ID类型作为类型参数。此接口主要用作标记接口，以捕获要使用的类型并帮助扩展此接口的接口。该接口(CrudRepository)正在管理的实体类提供复杂的CRUD功能。

实例

```java
public interface CrudRepository<T,ID> extends Repository<T,ID>{
    <S extends T> S save(S entity);//保存给定的实体
    Optional<T> findById(ID primaryKey);//返回由给定ID标识的实体
    Iterable<T> findAll();//返回所有实体
    long count();//返回实体的数量
    void delete(T entity);//删除给定的实体
    boolean existsById(ID primaryKey);//指定具有给定ID的实体是否存在
    // … more functionality omitted.
}
```

提供特定于持久性技术的抽象，（JpaRepository、MongoRepository）这些接口扩展CrudRepository

在CrudRepository之上有一个PagingAndSortingRepository抽象，添加了额外的方法来简化对实体的分页访问

实例：PagingAndSortingRepository界面

```java
public interface PagingAndSortingRepository<T,ID> extends CrudRepository<T,ID>{
    Iterable<T> findAll(Sort sort);
    Page<T> findAll(Pageable pageable);
}
```

若要访问User页面大小为20的第二页，可以执行一下操作

```java
PagingAndSortingRepository<User,Long> repository=//...get access to a bean
Page<User> users = repository.findAll(PageRequest.of(1,20));
```

示例：派生计数查询

```java
interface UserRepository extends CrudRepository<User,Long>{
    long countByLastname(String lastname);
}
```

示例：派生删除查询

```java
interface UserRepository extends CrudRepository<User,Long>{
   long deleteByLastname(String lasename);
    List<User> removeByLastname(String lastname);
}
```

### 1.2 查询方法

标准CRUD功能存储库通常对底层数据存储进行查询，使用Spring Data,声明这些查询变成了一个四步过程：

1.声明一个扩展Repository或其子接口之一的接口，并将其键入他应该处理的域类和ID类型，示例;

```java
interface PersonRepository extends Repository<Person,Long>{...}
```

2.在接口上声明查询方法

```java
interface PersonRepository extends Repository<Person,Long>{
    List<Person> findByLastname(String lastname);
}
```

3.设置Spring以使用JavaConfig或XML配置为这些接口创建代理实例。

要使用Java配置，创建一个类似于以下内容的类

```java
import org.springframework.data.jpa.repository.config.EnableJpaRepositories;

@EnableJpaRepositories
class Config{...}
```

要使用XML配置，请定义一个类似一下内容bean

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns"http://www.springframework.org/schema/beans"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xmlns:jpa="http://www.springframework.org/schema/data/jpa"
   xsi:schemaLocation="http://www.springframework.org/schema/beans
     https://www.springframework.org/schema/beans/spring-beans.xsd
     http://www.springframework.org/schema/data/jpa
     https://www.springframework.org/schema/data/jpa/spring-jpa.xsd">

   <jpa:repositories base-package="com.acme.repositories"/>

</beans>
```

使用了Jpa命名空间

JavaConfig变体并未显式配置包，因为默认情况下使用带注释的类的包

注入存储库实例并使用，示例：

```java
class SomeClient{
    private final PersonRepository repository;
    SomeClient(PersonRepository repository){
        this.repository=repository;
    }
    
    void doSomething(){
        List<Person> persons = repository.findByLastname("Matthews");
    }
}
```

### 1.3 定义存储库接口

首先需要定义特定于域类的存储库接口。接口必须扩展 Repository 并键入域类和ID类型。若要公开该域类型的CRUD方法，需要扩展 CrudRepository 而不是 Repository

#### 微调存储库定义

通常情况下 接口扩展Repository，CrudRepository 或者PagingAndSortingRespository

若不想扩展Spring Data接口，可以使用 @RepositoryDefinition，扩展CrudRepository公开了完整的方法来操作实体。若愿意选择公开的方法，可以将想要公开的方法复制 CrudRepository 到域存储库中。

** 可以在提供的 Spring Data Repository 功能之上定义自己的抽象。

如何以选择性的公开CRUD方法（findById和save）

示例：

```java
@NoRepositoryBean
public interface MyBaseRepository<T,ID> extends Repository<T,ID>{
    Option<T> findById(ID id);

    <S extends T> S save(S entity);
}

public interface UserRepository {
    User findByIdEmailAddress(EmailAddress emailAddress);
}
```

中间存储库接口使用@NoRepositoryBean，确保该注释添加到Spring Data  不应在运行时为其创建实例的所有存储库接口

#### 使用具有多个Spring数据模块的存储库

定义范围内的所有存储库接口都绑定到Spring Data模块。当应用程序需要使用多个Spring Data模块时，存储库定义必须区分持久性技术。

当在类路径上检测到多个存储库factory时，Spring Data进入严格的存储库配置模式。

严格配置使用存储库或域类的详细信息来决定存储库定义的Spring Data模块绑定。

1.如果存储库定义扩展了特定于模块的存储库，则是特定的Spring Data模块的有效候选。

2.如果域类使用特定于模块的类型注释进行注释，则是特定Spring Data模块的有效候选。Spring Data接受第三方注解（如JPA @Entity）或自己的注解（@Document Spring Data MongoDB和Spring Data Elasticsearch）

示例：使用模块特定接口的存储库定义

```
public interface MyRepository extends JpaRepository<User,Long> {
    @NoRepositoryBean
    interface MyBeanRepository<T,ID> extends JpaRepository<T,ID>{}
    
    interface UserRepository extends MyBeanRepository<User,Long>{}
}
```

示例：使用通用接口的存储库定义

```
public interface AmbiguousRepository extends Repository<User,Long> {
    
    @NoRepositoryBean
    interface MyBaseRepository<T,ID> extends CrudRepository<T,ID>{}
    
    interface AmbiguousUserRepository extends MyBaseRepository<User,Long>{}
}
```

AmbiguousRepository 和 AmbiguousRepository 仅延伸Repository，并在它们的类型层次CrudRepository。

虽然这在使用唯一的Spring Data模块时good，但多个模块无法区分这些存储库应该绑定到哪个特定Spring Data

示例:使用带注释的域类的存储库定义

```
public interface PersonRepository extends Repository<Person,Long> {
    @Entity
    class Person{}
    interface UserRepository extends Repository<User,Long>{}

    @Document
    class User{}
}
```

使用了JPA @entity进行了注释，所以这个存储库显然属于Spring Data JPA

UserRepository依赖User，使用Spring Data MongoDB的@Document进行注解。

示例:使用带有混合注释的域类的存储库定义

```
public interface JpaPersonRepository extends Repository<Person,Long> {
    
    interface MongoDBPersonRepository extends Repository<Person, Long>{}
    @Entity
    @Document
    class Person{}
}
```

显示使用JPA和Spring Data MongoDB注释的域类。

定义了两个存储库 --JpaPersonRepository和MongoDBPersonRepository。

一个用于JPA 一个用于MongoDB。Spring Data不再区分存储库。

示例：基础包的注解驱动配置

```java
@EnableJpaRepositories(basePackages="com.acme.repositories.jpa")
@EnableMongoRepositories(basePackages="com.acme.repositories.jpa")
class Configuration{}
```

### 1.4 定义查询方法

1.通过直接从方法名称派生查询

2.通过使用手动定义的查询

#### 查询查找策略

可以使用注解的queryLookupStrategy属性Enable${store}Repositories

1.CREATE尝试从查询方法名称构造特定于查询。

2.USE_DECLARED_QUERY尝试查找已声明的查询，如果找不到则抛出异常。

3.CREATE_IF_NOT_FOUND 结合CREATE和USE_DECLARED_QUERY。首先查找声明的查询，如果没有找到声明的查询，他会创建一个自定义的基于方法名称的查询。默认查询策略。

按方法名称快速定义查询，还通过根据需要引入声明的查询来自定义这些查询。

#### 查询创建

示例:从方法名称创建查询

```java
public interface PersonRepository extends Repository<Person,Long> {
    List<Person> findByEmailAddressAndLastname(EmailAddress emailAddress,String lastname);
    
    //Enables the distinct flag for the query
    List<Person> findDistinctPeopleByLastnameOrFirstname(String lastname,String firstname);
    List<Person> findPersonDistinctByLastnameOrFirstname(String lastname,String firstname);
    
    //Enabling ignoring case for an individual property
    List<Person> findByLastnameIgnoreCase(String lastname);
    
    //Enabling ignoring case for all suitable properties
    List<Person> findByLastnameAndFirstnameAllIgnoreCase(String lastname,String firstname);
    
    //Enabling static ORDER BY for a query
    List<Person> findByLastnameOrderByFirstnameAsc(String lastname);
    List<Person> findByLastnameOrderByFirstnameDesc(String lastname);
}
```

解析查询方法名称分为主语和谓语。

1.（find by和exists by）定义查询主题。

2.构成谓词。介绍从句可以包含进一步的表达。

解析方法的实际结果取决于创建查询的持久性存储。注意：

1.表达式通常是与可以连接的运算符相结合的属性遍历。

2.方法解析器支持IgnoreCase为单个属性或支持忽略大小写的类型的所有属性设置标志。

3.可以通过将OrderBy子句附加到引用属性的查询方法并提供排序方法（Asc或Desc）来应用静态排序。

#### 属性表达式

属性表达是只能引用托管实体的直接属性。在创建查询时，可以确保解析的属性时托管域类的属性。

```java
List<Person> findByAddessZipCode(ZipCode,zipcode);
```

假设Person 有一个 AddessZipCode。这种情况下，该方法慧创建x.address.zipCode属性进行遍历。解析算法首先将整个部分解释为属性并检查具有该名称的属性的域类。

若成功，则使用该属性。若不成功，则算法将来自右侧的驼峰式部分的源分成头部和尾部，并尝试找到相应的属性。

尽管这应该适用于大多数情况，但算法可能会选择错误的属性。若Person类也有一个addressZip属性，则该算法将在第一个分割轮中匹配，选择错误的属性，并失败。

要解决这种歧义，可以在方法名称中使用手动定义遍历点。

```java
List<Person> findByAddress_ZipCode(ZipCode,zipcode);
```

因为我们将下户线字符视为保留字符，所以强烈建议遵循标准的Java命名约定。

#### 特殊参数处理

要处理查询中的参数，要定义方法参数。

~~~java
Page<User> findByLastname(String lastname,String firstname);
Slice<User> findByLastname(String lastname,String firstname);
List<User> findByLastname(String lastname,Sort sort);
List<USer> findByLastname(String lastname,Pageable pageable);
~~~

API接受Sort并切Pageable期望将非null值传递给方法。以将分页动态添加到静态定义的查询中。

Page  --可用的元素和页面的总数。

通过基础结构出发计数查询来计算总数。

排序选项也通过Pageable实例处理。若只需要排序，在方法重添加一个参数。返回List也是可能的

Page不会创建实际实例所需的额外元数据。

要了解整个查询获得了多少页，必须得到额外的计数查询。默认情况下，查询源自实际触发的查询。

##### 分页和排序

可以使用属性名称定义简单的排序表达式。可以连接表达式将多个条件收集到一个表达式中。

示例：定义排序表达式

```java
Sort sort = Sort.by("firstname").accending().and(Sort.by("lastname").descending());
```

对于定义排序表达式的更1类型安全的方法，从定义排序表达式的类型开始，并开始方法引用来定义排序的属性。

##### 示例：使用类型安全API定义排序表达式

```java
TypedSort<Person> person = Sort.sort(Person.class);
Sort sort = person.by(Person::getFirstname).ascending().and(person.by(Person::getLastname).descending());
```

TypedSort.by()通过使用CGlib来使用运行时代理，可能会在使用Graal VM Native等工具时干扰本机影响编译。

若实现支持Querydsl，可以使用生成的元模型来定义排序表达式

示例：使用Querydsl API定义排序表达式

```java
QSort sort = QSort.by(QPerson.firstname.asc()).and(QSort.by(QPerson.lastname.desc()));
```

#### 限制查询结果

可以使用first 或 top关键字来限制查询方法的结果，这两个关键字可以互换使用。可以将一个可选的数值附加到top或first指定要返回的最大结果大小。若忽略该数字，则假定结果大小为1.如何限制查询大小。

##### 示例：使用Top和First限制查询的结果大小

```java
User findFirstByOrderByLastnameAsc();
User findTopByOrderByAgeDesc();
Page<USer> queryFirst10ByLastname(String lastname,Pageable pageable);
Slice<User> findTop3ByLastname(String lastname,Pageable pageable);
List<User> findFirst10Bylastname(String lastname,Sort sort);
List<User> findTop10ByLastname(String lastname,Pageable pageable);
```

限制表达式还支持Distinct支持不同查询的数据存储的关键字。对于结果集限制为一个实例的查询，Optional支持将结果用关键字包装。

如果分页或切片应用于限制查询分页，则在受限结果内应用。

通过使用Sort参数限制结果与动态排序相结合，可以表达‘K’最小元素与‘K'最大元素的查询方法。

#### 返回集合或可迭代对象的存储库方法

查询方法，返回多个结果可以使用标准的Java Iterable，List和Set。除此之外，我们支持返回Spring Data的Streamable 的自定义扩展Iterable以及Var提供的集合类型。

##### 使用Streamable作为查询方法返回类型

可以使用任何集合类型的Streamable替代品Iterable，提供了访问并Stream的便捷方法。以及直接filter和map覆盖元素并将其连接Streamable到其他元素的能力。

实例：使用Streamable组合查询方法结果

```java
interface PersonRepository extends Repository<Person,Long>{
    Streamable<Person> findByFirstnameContaining(String firstname);
    Streamable<Person> findByLastnameContaining(String lastname);
}
Streamable<Person> resilt = repository,findByFirstnameContaining("av").and(repository.findByLastnameContaining("ea"));
```

##### 返回自定义流包装器类型

为集合提供专用包装器类型是一种常用模式，用于为返回多个元素的查询结果提供API。通常，通过调用存储库方法返回类集合类型并手动创建包装器类型得 实例来使用这些类型。可以避免该额外步骤，因为Spring Data允许将这些包装器类型用作查询方法返回类型。满足：

1.类型实现Streamable

2.类型公开任一个构造或命名静态factory 方法of或valueOf该取Streamable作为参数。

示例：

```java
class Product{
    MonetaryAmount getPrice(){}
}
@RequiredArgsContructor(staticName="of")
class Products implements Streamable<Product>{
    private final Streamable<Product> streamable;
    public MonetaryAmount getTotal(){
        return streamable.stream()
            ,map(Priced::getPrice)
            .reduce(Money.of(0),MonetaryAmount::add);
    }
    @Override
    public Iterator<Product> iterator(){
        return streamable.iterator();
    }
    interface ProductRepository implements Repository<Producy,Long>{
        Products findAllByDescriptionContaining(String text);
    }
}
```

1.一个Product暴露的API来访问产品的价格实体。

2.Streamable<Product>可以通过使用Products.of()（使用Lombok注释创建的factory方法）构造的包装器类型。采用Streamable<Product>will的标准构造函数也可以。

3.包装器类型公开了一个额外的API，在Streamable<Product>

4.实现Streamable接口并委托给实际结果。

5.该包装器类型Products可以直接用作查询方法返回类型。不需要Streamable<Product>在存储库客户端中的查询之后返回并手动包装它。

##### 支持Vavr集合

Vavr是一个包含Java函数式编程概念的库。附带一组自定义集合类型。可以将其用作查询方法返回类型。

| Vavr 集合类型            | 使用的 Vavr 实现类型               | 有效的 Java 源类型   |
| :----------------------- | :--------------------------------- | :------------------- |
| `io.vavr.collection.Seq` | `io.vavr.collection.List`          | `java.util.Iterable` |
| `io.vavr.collection.Set` | `io.vavr.collection.LinkedHashSet` | `java.util.Iterable` |
| `io.vavr.collection.Map` | `io.vavr.collection.LinkedHashMap` | `java.util.Map`      |

可以使用第一列中的类型作为查询返回类型。并根据实际查询结果的Java类型获取第二列中的类型作为实现类型。

或者可以声明Traversable(Iterable相当于Vavr)，然后从实际返回值派生实现类。

#### 存储库方法的空处理

Spring Data2.0开始，返回单个聚合示例的存储库CRUD方法使用Java 8Optional来指示可能缺少值。

SpringData支持在查询方法上返回一下包装器类型：

- `com.google.common.base.Optional`
- `scala.Option`
- `io.vavr.control.Option`

或者查询方法可以选择根本不实用包装器类型。然后通过返回来指示不存在查询结果null。返回集合 集合替代 包装器和流的存储库方法保证永远不会返回null，而是返回相应的空表示。

##### 可空性注释

可以使用SpringFramework的可空性注释来表达存储库方法的可空性约束。null在运行时提供了一种工具有好的方法和选择加入检查。

1.@NonNullApi：在包装级别上用于声明参数和返回值的默认行为分别时既不接受也不产生null值。

2.@NonNull：用于不得为的参数或返回值null（在@NonNullApi适用的参数和返回值上不需要）。

3.@Nullable：用于可以是的参数或返回值null

Spring 注释使用JSR 305注释进行元注释。

JSR 305元注释让工具供应商以通过方式提供空安全支持，而无需对Spring注释进行硬编码支持。要为查询方法启用可空性约束的运行时检查，需要使用Spring的@NonNullApi在包级别激活非可空性package-info.java

示例：在package-info.java

```java
@org.springframework.lang.NonNullApi
    package com.acme;
```

一旦非空默认设置到位，存储库查询方法调用将在运行时验证为可空性约束。如果查询结果违反了定义的约束，则抛出异常。当发i方法将返回null但被声明不可为空时，就会发生此情况。

若想再次选择可空结果。青有选择性地使用@Nullable单个方法。

示例：使用不同的可空性约束

```java
import java.util.Optional;
import com.mongodb.lang.Nullable;

public interface UserRepository extends Repository<User,Long> {
    User getByEmailAddess(EmailAdddress emailAdddress);
    
    @Nullable
    User findByEmailAddress(@Nullable EmailAddress emailAddress);
    
    Optional<User> findOptionalByEmailAddress(EmailAddress emailAddress);
}
```

1.存储库驻留在我们定义了非空行为的包中

2.EmptyResultDataAccessException当查询未产生结果时抛出。

IllegalArgumentException当emailAddress传递给方法时抛出null

3.null当查询未产生结果时返回，也接受null作为值emailAddress。

4.Optional.empty() 当查询未产生结果返回。IllegaargumentException当emailAddress传递给方法时抛出null。

##### 基于Kotlin的存储库中的可空性

Kotlin在语言中定义了可空性约束。Kotlin代码编译为字节码，他不通过方法签名而是通过编译元数据来表达可空性约束。确保kotlin-reflect在项目中包含JAR以启用对Kotlin的可空性约束的内省。SpringData存储库使用语言机制来定义这些约束以应用相同的运行时检查。

示例：在Kotlin存储库上使用可空性约束

```java
interface UserRepository:Repository<User,Long>{
    fun findByUsername(usernmae:Sring):User
        fun findByFirstname(firstname:String?):User?
}
```

1.该方法将参数和结果都定义为不可为空。Kotlin编译器拒绝传递null给方法的方法调用。如果查询产生空结果，EmptyResultDataAccessException将抛出an

2.该方法接受null的firstname参数，并返回null，如果查询不产生结果。

#### Streaming Query Results

可以使用Java 8 stream<T> 作为返回类型以增量方式处理查询方法的结果。不是将查询结果包装在Stream中，而是使用数据存储特定的方法来执行传输

示例：使用Java 8流式传输查询结果Stream<T>

```java
@Query("select u from User u")
Stream<User> findAllByCustomQueryAndStream();
Stream<User> readAllByFirstnameNotNull();

@Query("select u from User u")
Stream<User> streamAllPaged(Pageable pageable);
```

Stream潜在包装了底层数据存储特定的资源，因此必须在使用后关闭，可以使用 Stream close()方法或使用Java 7 try-with-resources手动关闭

示例：Stream<T>在try-with-resources块中处理结果

```java
try(Stram<User> stream = repository.findAllByCustomQueryAndStream()){
    stream.forEach();
}
```

并非所有Spring Data模块当前都支持Stream<T>作为返回类型。

#### 异步查询结果

可以使用Spring的异步方法运行能力异步运行存储库查询。这意味着该方法在调用时立即返回，而实力查询发生已提交给Spring的任务中TaskExecutor。异步查询不同于反应式查询，不应混合使用。

```java
@Async
Fucture<User> findByFirstname(String firstname);

@Async
CompltableFucture<User> findOneByFirstname(String firstname);

@Async
ListenableFucture<User> findOneByLastname(String lastname);
```

1.使用java.util.concurrent.Fucture作为返回类型

2.使用Java 8 java.util.concurrent.CompletableFucture作为返回类型

3.使用 org.springframework.util.concurrent.ListenableFucture作为返回类型。

### 1.5 创建存储库实例

如何为定义的存储库接口创建实例和bean定义。一种方法是使用支持存储库机制的每个Spring Data模块附带的Spring命名空间，尽管通常建议使用Java配置。

#### XML配置

每个Spring Data模块都包含一个repository元素，可以定义Spring扫描的比本报

实例：通过XML启用Spring Data存储库

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns:beans="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns="http://www.springframework.org/schema/data/jpa"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
    https://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/data/jpa
    https://www.springframework.org/schema/data/jpa/spring-jpa.xsd">

  <repositories base-package="com.acme.repositories" />

</beans:beans>
```

在前面的示例中，指示Spring扫描com.acme.repositories及其所有子包以查找扩展Respository的接口或其子接口之一。对于找到的每个接口，基础结构注册特定FactoryBean于持久性技术异常黄健处理查询方法调用的适当代理。每个bean都在从接口名称派生的bean名称下注册。嵌套存储库接口的Bean名称以其封闭的类型名称为前缀。该base-package属性允许使用通配符，以便可以定义扫描包的模式。

##### 使用过滤器

要将某些接口从实例化中排除为存储库bean，使用以下配置

示例：使用exclude-filter元素

```java
<repositories base-package="com.acme.repositories">
    <context:exclude-filter type="regex" expression=".*SomeRepository"/>
</repositories>
```

#### Java 配置

启用Spring Data存储库的示例配置类似以下内容

示例：基于注解的存储库配置示例

```java
@Configuration
@EnableJpaRepositories("com.acme.repositories")
class ApplicationConfiguraion{
    @Bean
    EntityManagerFactory entityManagerFactory(){
        
    }
}
```

#### 独立使用

还可以在Spring容器之外使用存储库基础设施--在CDI环境中。

示例 存储库factory的独立使用

```java
RepositoryFactorySupport factory = ;//
UserRepository repository = factory.getRepository(UserRepository.class);
```

### 1.6 Spring Data Repositories自定义实现

#### 自定义单个存储库

要定义功能丰富的存储库，首先定义片段接口和自定义功能的实现

示例 自定义存储库功能的接口

```java
interface CustomizedUserRepository{
    void someCustomMethod(User user);
}
```

示例：自定义存储库功能的实现

```java
class CustomizedUserRepositoryImpl implements CustomizedUserRepository{
    public void someCustomMethod(User user){
        
    }
}
```

实现本身不依赖Spring  Data 

可以让repository接口扩展fragment接口

示例：对存储库界面的更改

```java
interface UserRepository extends CrudRepository<User,Long>,CustomizedUserRepository{
    
}
```

Spring Data存储库是通过使用形成存储库组合的片段来实现的

示例：片段及其实现

```java
interface HumanRepository{
    void someHumanMethod(User user);
}
class HumanRepositoryImpl implements HumanRepository{
    publiv void someHumanMethod(User user){
        
    }
}
interface ContactRepository{
    void someContactMethod(User user);
    USer anotherContactMethod(User user);
}
class ContactRepositoryImpl implements ContactRepository{
    public void someContactMethod(User user){
        
    }
    public User anotherContactMethod(User user){
        
    }
}
```

示例：对存储库界面的更改

```java
interface UserRepository extends CrudRepository<Uuser,Long>.HumanRepository,ContactRepository{
    
}
```

存储库可能由按声明顺序导入的多个自定义实现组成。

示例:片段覆盖save

```java
interface CustomizedSave<T>{
    <S extends T> S save(S entity);
}
class CustomizedSaveImpl<T> implements CustomizedSave<T>{
    public <S extends T> S save(S entity){
        
    }
}
```

示例：自定义存储库接口

```java
interface UserRepository extends CrudRepository<User,Long>,CustomizedSave<User>{
    
}
interface PersonRepository extends CrudRepository<Person,Long>,CustomizedSave<Person>{}
```

##### 配置

若使用命名空间配置，存储库基础结构会尝试通过扫描它在其中找到存储库的包下的类来自动检测自定义实现片段。

示例：配置实例

```java
<repositories base-package="com.acme.repository"/>
<repositories base-package="com,acme.repository" repository-impl-postfix="MyPostfix"/>
```

解决歧义

若在不同的包中找到多个具有匹配类名的实现，Spring Data使用bean名称来标识使用的

示例：解决不明确的实现

```java
package com.acme.impl.one;
class CustomizedUserRepositoryImpl implements CustomizedUserRepository{
    
}
```

```java
package com.acme.impl.two;
@Component("specialCustomImpl")
class CustomizedUserRepositoryImpl implements CustomizedUserRepository{
    
}
```

##### 手动接线

若自定义实现仅使用基于注解的配置和自动装配，则前面显示方法效果很好，因为被视为任何其他Spring bean。

示例：自定义实现的手动接线

```java
<repositories base-package="com.acme.repository"/>
<beans:bean id="userRepositoryImpl" class="">
</beans:bean>
```

#### 自定义基础存储库

若要 更改所有存储库的行为，可以创建一个扩展持久性技术特定存储库基类的实现。

示例：自定义存储库基类

```java
class MyRepositoryImpl<T,ID>
    extends SimpleJpaRepository<T,ID>{
    private final EntityManager entityManager;
    MyRepositoryImpl(JpaEntityInfotmation entityInfomation,
                    EntityManager entityManager){
        super(entityInformation,entityManager);
        this.entityManager = entityManager;
    }
    
    @Transactional
    public <S extends T> S save(S entity){
        
    }
}
```

该类需要具有特定于stop的存储库factory实现使用的超类的构造函数

最后一步时让Spring Data基础设施知道定制的存储库基类

```java
@Configuration
@EnableJpaRepositories(repositoryBasrClass = MyRepositoryImpl.class)
class ApplicationConfiguration{}
```

示例：使用XML配置自定义存储库基类

```java
<repositories base-package="com.acme.repository"
    base-class="99.MyRepositoryImpl"/>
```

### 1.7 从聚合根发布事件

存储库管理的实体是聚合根

示例：从聚合根公开域事件

```java
class AnAggregateRoot{
    @DomainEvents//可以返回单个事件实例或事件集合
    Collection<Object> domainEvents(){
        
    }
    @AfterDomainEventPublication//可以使用它来潜在的清理要发布的事件列表
    void callbackMethod(){
        
    }
}
```

### 1.8 Spring数据扩展

#### Querydsl扩展

Querydsl是一个框架，可以通过其流畅的API构建静态类型的SQL类查询

几个Spring Data模块通过提供与Querydsl的集成QuerydslPredicateExecutor

示例：QuerydslPredicateExecutor接口

```java
public interface QuerydslPredicateExecutor<T>{
    Optional<T> findById(Perdicate predicate);
    Iterable<T> findAll(Predicate predicate);
    long count(Predicate predicate);
    boolean exists(Predicate predicate);
}
```

要使用Querydsl支持，扩展QuerydslPredicateExecutor存储库界面

示例：存储库上的Querydsl集成

```java
interface UserRepository extends CrudRepository<User,Long>,QuerydslPredicateExecutor<User>{}
```

#### 网络支持

支持存储库编程模型的Spring Data模块附带了各种Web支持

示例：启用Spring Data Web支持

```java
@Configuraion
@EnableWebMvc
@EnableSpringDataWebSupport
class WebConfiguration{}
```

该@EnableSpringDataWebSupport批注注册几个组件

示例：在XML中启用Spring Data Web支持

```java
<bean class="org.springframework.data.web.config.SpringDataWebConfiguration" />

<!-- If you use Spring HATEOAS, register this one *instead* of the former -->
<bean class="org.springframework.data.web.config.HateoasAwareSpringDataWebConfiguration" />
```

##### 基本网络支持

基本组件：

DomainClassConverter：让Spring MVC从请求参数或路径变量解析存储库管理的域类的实例

HandlerMethodArgumentResolver 让Spring MVC从请求参数解析Pageable和Sort实例的实现

Jackson Modules用于反序列化Point和等类型Distance，或存储特定类型，具体取决于所使用的Spring数据模块

**使用DomainClassConverter类**

示例：在方法签名中使用域类型的Spring MVC控制器

```java
@Controller
@RequestMapping("/users")
class UserController{
    @ReuqestMapping("/{id}")
    String showUserForm(@PathVariable("id") User user,Model model){
        model.addAttribute("user",user);
        return "userForm";
    }
}
```

**用于可分页和排序的HandlerMethodArgumentResolvers**

示例：使用Pageable作为控制器方法参数

```java
@controller
@RequestMapping("/users")
class UserController{
    private final UserRepository repository;
    UserController(UserRepository repository){
        this.repository = repository;
    }
    @RequestMapping
    String showUsers(Model model,Pageable pageable){
        model.addAttribute("users",repository.findAll(pageable));
        return "users";
    }
}
```

为Pageable实例评估的请求参数

| `page` | 您要检索的页面。0 索引，默认为 0。                           |
| ------ | ------------------------------------------------------------ |
| `size` | 要检索的页面大小。默认为 20。                                |
| `sort` | 应按格式排序的属性`property,property(,ASC|DESC)(,IgnoreCase)`。默认排序方向是区分大小写的升序。`sort`如果要切换方向或区分大小写，请使用多个参数 - 例如，`?sort=firstname&sort=lastname,asc&sort=city,ignorecase`. |

要自定义则要分别注册一个实现PageableHandlerMethodArgumentResolverCustomizer接口或SortHandlerMethodArgumentResolverCustonizer接口的bean

其customize()方法已被调用，更改设置如下

```java
@Bean SortHandlerMethodArgumentResolverCustomizer sortCustomizer(){
    return s-> s.setPropertyDelimiter("<-->");
}
```

若需要从请求中解析多个Pageable或多个Sort实例，可以使用Spring的@Quailfier注解来区分两个。请求参数必须以${qualifier}为前缀

```java
String showUsers(Model model,
                @Quailfier("thing1") Pageable first,
                @Quailfier("thing2") Pageable second){}
```

##### 对可分页的超媒体支持

Spring HATEOAS附带了一个表示模型类，允许Page使用必要的Page元数据和链接来丰富实例内容，让客户端轻松导航页面。

实例：使用PagedResourcesAssembler作为控制器方法参数

```java
@Controller
class PersonController{
    @Autowired PersonRepository repository;
    @RequestMapping(value = "/persons",method = RequestMethod.GET)
    HttpEntity<PagedResources<Person>> persons(Pageable pageable,
                                              PagedReourcesAssembler assembler){
        Page<Person> persons = repository.findAll(pageble);
        return new ResponseEntity<>(assembler.toResources(persons),HttpStatus.OK);
    }
}
```

启用配置可以PagedReourcesAssembler将用作控制器方法参数

##### Spring Data Jackson模块

核心模块和一些特定于shop的模块附带一组jackson模块，用于Spring Data域使用的类型

如org.springframework.data.geo.Distance和org.springframework.data.geo.Poing一旦启用Web支持并可用，com.fasterxml.jackson.databind.ObjectMapper就会导入这些模块

单个模块可能会提供额外的SpringDataJacksonModules

##### 网页数据绑定支持

可以使用Spring Data投影通过使用JSONPath表达式来绑定传入的请求有效负载

示例：使用JSONPath或XPath表达式的HTTP负载绑定

```java
@ProjectedPayload
public interface UserPayload{
    @XBRead("//firstname")
    @JsonPath("$,,firstname")
    String getFirstname();
    
    @XBRead("/lastname")
    @JsonPath({"$.lastname","$.user.lastname"})
    String getLastname();
}
```

可以使用在前面例子中为一个Spring MVC处理程序方法参数或通过使用所示类型ParameterizedTypeReference上的方法之一RestTemplate。

对于Spring MVC，必要的转换器一旦处于活动状态就会自动注册@EnableSpringDataWebSupport，并且所需的依赖项在类路径上可用。对于使用RestTemplate，注册ProjectingJackson2HttpMessageConverter或XmlBeamHttpMessageConverter手动

##### Querydsl网络支持

对于具有QueryDSL集成的stores，可以从Request查询字符串包含的属性派生查询

```java
?firstname=Dave&lastname=Matthews
```

给定User示例中的对象，可以使用将查询字符串解析为以下值QuerydslPredicateArgumentResolver

```java
QUser.user.firstname.eq("Dave").and(QUser.user.lastname.eq("Matthews"))
```

@EnableSpringDataWebSupport当在类路径上找到Querydsl时，会自动启用该功能

类型信息通常从方法的返回类型解析。由于该信息不一定与域类型匹配，因此使用的root属性可能时一个QuerydslPredicate

@QuerydslPredicate在方法签名中使用

```java
@Controller
class UserController{
    @Autowired USerRepository repository;
    @RequestMapping(value = "/",method=RequestMethod.GET)
    String index(Model model,@querydslPredicate(root=User.class) Predicate predicate,
                Pageable pageable,@RequestParam MultiValueMap<String,String> parameters){
        model.addAttribute("users",repository.findAll(predicate,pageable));
        return "index";
    }
}
```

默认绑定如下：

Object在简单的属性eq上

Object在像contains属性一样的集合上

Collection在简单的属性in上

可以通过bindings属性@QuerydslPredicate或通过使用default methods并将QuerydslBinderCustomizer方法添加到存储库接口来自定义这些绑定

```java
interface UserRepository extends CrudRepository<User,String>,QuerydslPredicateExecutor<user>,QuerydslBinderCustomizer<QUser>{
    @Override
    default void customize(QuerydslBindings bindings,QUser user){
        bindings.bind(user.username).first((path,value) -> path.contains(value));
        bindings.bind(String.class)
            .first((StringPath path,String value) -> path.comtainsIgnoreCase(value));
        bindings,excluding(user.password);
    }
}
```

可以注册一个QuerydslBinderCustomizerDefaylts从资源库或应用特定的绑定之前bean保持默认Querydsl绑定@QuerydslPredicate

#### 4.8.3 存储库填充器

若使用Spring JDBC模块，可能熟悉DataSource使用SQL 脚本填充支持

示例：JSON中定义的数据

```java
[{"_class":"com.acme.Preson",
 "firstname":"Dave",
 "lastname":"Matthews",}
 {"_class":"com.acme.Person",
 "firstname":"Carter",
 "lastname":"Beauford"}]
```

可以使用Spring Data Commons中提供的存储库命名空间populator元素来填充存储库。要将前面数据填充到PersonRepository，声明一个类似于以下内容的填充器

示例：声明一个Jackson存储库填充器

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:repository="http://www.springframework.org/schema/data/repository"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
    https://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/data/repository
    https://www.springframework.org/schema/data/repository/spring-repository.xsd">

  <repository:jackson2-populator locations="classpath:data.json" />

</beans>
```

JSON对象解组的类型是通过检查_class JSON文档的属性来确定的。

要改为使用XML定义应填充存储库的数据，可以使用该unmarshaller-populator元素。

将其配置为使用Spring OXM中可用的XML marshaller可选项之一。

示例：声明解组 存储库填充其（使用JAXB

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:repository="http://www.springframework.org/schema/data/repository"
  xmlns:oxm="http://www.springframework.org/schema/oxm"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
    https://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/data/repository
    https://www.springframework.org/schema/data/repository/spring-repository.xsd
    http://www.springframework.org/schema/oxm
    https://www.springframework.org/schema/oxm/spring-oxm.xsd">

  <repository:unmarshaller-populator locations="classpath:data.json"
    unmarshaller-ref="unmarshaller" />

  <oxm:jaxb2-marshaller contextPath="com.acme" />

</beans>
```

## 2 JPA

### 2.1 JPA存储库

建立在使用Spring数据库存储库中解释的核心存储库支持之上

#### 2.11 介绍

配置JPA

Spring Namespce （XML配置

基于注解的配置 （JAva配置

##### Spring命名空间

Spring Data的JPA模块包含一个允许定义存储库bean的自定义命名空间

示例:使用命名空间设置JPA存储库

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:jpa="http://www.springframework.org/schema/data/jpa"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
    https://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/data/jpa
    https://www.springframework.org/schema/data/jpa/spring-jpa.xsd">

  <jpa:repositories base-package="com.acme.repositories" />

</beans>
```

使用repositories元素查找Spring Data存储库

自定义命名空间属性

repositories元素的自定义JPA特定属性

| `entity-manager-factory-ref` | 显式`EntityManagerFactory`连接要与`repositories`元素检测到的存储库一起使用的。通常`EntityManagerFactory`在应用程序中使用多个bean 时使用。如果未配置，春数据自动查找了`EntityManagerFactory`名为豆`entityManagerFactory`中`ApplicationContext`。 |
| ---------------------------- | ------------------------------------------------------------ |
| `transaction-manager-ref`    | 显式`PlatformTransactionManager`连接要与`repositories`元素检测到的存储库一起使用的。通常只有`EntityManagerFactory`在配置了多个事务管理器或bean时才需要。默认为`PlatformTransactionManager`在 current 中定义的单个`ApplicationContext`。 |

##### 基于注解的配置

Spring Data JPA存储库支持不仅可以通过XML命名空间激活，还可以通过JacaConfig使用注释激活

示例：使用JavaConfig的Spring Data JPA存储库

```java
@Configuration
@EnableJpoaRepositories
@EnableTransactionManagement
class ApplicationConfig{
    @Bean
    public DataSource dataSource(){
        EmbeddedDatabaseBuilder builder = new EmbeddedDatabaseBuilder();
        return builder.setType(EmbeddedDatabaseType.HSQL).build();
    }
    
    @Bean
    public LocalContainerEntityManagerFactoryBean entityManagerFactory(){
        HibernateJpaVendorAdapter vendorAdapter = new HibernateJpaVendorAdapter();
        vendorAdapter.setGenerateDdl(true);
        
        LocalcontainerEntityManagerFactoryBean factory = new LocalContacinerEntityManagerFactoryBean();
        factory.setJpaVendorAdapter(vendorAdaptor);
        factory.setPackagesToScan("com.acme.domain");
        factory.setDataSource(dataSource());
        return factory;
    }
    
    @Baen
    public PlatformTransactionManager transactionManger(EntityManagerFactory entityManagerFactory){
        JpaTransactionManager txManager = new JpaTransactionManager();
        txManager.setEntityManagerFactory(enetityManagerFactory);
        return txManager;
    }
}
```

必须创建LocalContainerEntityManagerFactoryBean而不是EntityManagerFactory直接创建，

##### 引导模式

默认情况下，Spring Data JPA存储库是默认的Spring bean。是单例范围的并且初始化

在启动期间，已经与JPA交互以EntityManager进行验证和元数据分析。

SpringFramework支持EntityManagerFacroy在后台线程中初始化JPA，因为该进程通常会在Spring应用程序中占用大量启动时间。

可以配置一个BootstrapMpde

DEFAULT 默认

LAZY 隐式声明所有存储库bean为惰性，并导致创建惰性初始化代理以将其出入客户端bean

DEGERRED 与基本相同的操作模式LAZY，但触发存储库初始化以相应，ContextRefreshedEvent以便在应用程序完全启动之前验证存储库

**建议**

若异步引导JPA，DEFERED是一个合理的默认值，可以确保Spring Data JPA引导程序仅在EntityManagerFactory设置本身比初始化所有其他应用程序组件花费的时间更长的情况下等待设置。

LAZY是测试场景和本地开发的不错选择

#### 2.1.2 持久实体

##### 保存实体

可以使用CrudRepository.save()方法执行保存实体。通过使用底层JPa来持久化或合并给定的的实体EntityManager

##### 实体状态检测策略

Version-Property和Id-Property检查：默认情况下，Spring Data JPA首先做检查是否妇女在非原始类型的Version-property。若存在则该实体被视为新实体null，若没有这样的检查给定尸体的inentifier属性。如果标识符是null，则假定实体是新的否则不是

Persistable：若实施Persistable，Spring Data JPa将检测委托给isNew()实体的方法。

实现EntityInformation：可以通过创建子类并相应的覆盖方法来自定义实现EntityInformation使用的抽象

示例：具有手动分配标识符的实体的基类

```java
@MappedSuperclass
public abstract class AbstractEntity<ID> implements Persistable<ID>{
    @Transient
    private boolean isNew = true;
    @Override
    public boolean isNew(){
        return isNew;
    }
    @PrePersist
    @Postload
    void narkNotNew(){
        this.isNew = false;
    }
}
```

#### 2.1.3 查询方法

##### 查询朝朝策略

JPA模块支持将查询手动定义为字符串或从方法名称诞生

**声明的查询**

可以通过命名约定使用JPA命名查询或者使用注释查询方法@Query

##### 查询创建

示例：从方法名称创建查询

```java
公共接口 UserRepository extends Repository<User,Long>{
    List<User> findByEmailAddressAndLastname(String emailAddress,String lastname);
}
```

使用JPA标准API从中创建一个查询，本质上转换为select u from User u where u.emailAddress = ?1 and u.lastname = ?2

方法名称中支持的关键字

| Keyword                | Sample                                                       | JPQL snippet                                                 |
| :--------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `Distinct`             | `findDistinctByLastnameAndFirstname`                         | `select distinct … where x.lastname = ?1 and x.firstname = ?2` |
| `And`                  | `findByLastnameAndFirstname`                                 | `… where x.lastname = ?1 and x.firstname = ?2`               |
| `Or`                   | `findByLastnameOrFirstname`                                  | `… where x.lastname = ?1 or x.firstname = ?2`                |
| `Is`, `Equals`         | `findByFirstname`,`findByFirstnameIs`,`findByFirstnameEquals` | `… where x.firstname = ?1`                                   |
| `Between`              | `findByStartDateBetween`                                     | `… where x.startDate between ?1 and ?2`                      |
| `LessThan`             | `findByAgeLessThan`                                          | `… where x.age < ?1`                                         |
| `LessThanEqual`        | `findByAgeLessThanEqual`                                     | `… where x.age <= ?1`                                        |
| `GreaterThan`          | `findByAgeGreaterThan`                                       | `… where x.age > ?1`                                         |
| `GreaterThanEqual`     | `findByAgeGreaterThanEqual`                                  | `… where x.age >= ?1`                                        |
| `After`                | `findByStartDateAfter`                                       | `… where x.startDate > ?1`                                   |
| `Before`               | `findByStartDateBefore`                                      | `… where x.startDate < ?1`                                   |
| `IsNull`, `Null`       | `findByAge(Is)Null`                                          | `… where x.age is null`                                      |
| `IsNotNull`, `NotNull` | `findByAge(Is)NotNull`                                       | `… where x.age not null`                                     |
| `Like`                 | `findByFirstnameLike`                                        | `… where x.firstname like ?1`                                |
| `NotLike`              | `findByFirstnameNotLike`                                     | `… where x.firstname not like ?1`                            |
| `StartingWith`         | `findByFirstnameStartingWith`                                | `… where x.firstname like ?1` (parameter bound with appended `%`) |
| `EndingWith`           | `findByFirstnameEndingWith`                                  | `… where x.firstname like ?1` (parameter bound with prepended `%`) |
| `Containing`           | `findByFirstnameContaining`                                  | `… where x.firstname like ?1` (parameter bound wrapped in `%`) |
| `OrderBy`              | `findByAgeOrderByLastnameDesc`                               | `… where x.age = ?1 order by x.lastname desc`                |
| `Not`                  | `findByLastnameNot`                                          | `… where x.lastname <> ?1`                                   |
| `In`                   | `findByAgeIn(Collection<Age> ages)`                          | `… where x.age in ?1`                                        |
| `NotIn`                | `findByAgeNotIn(Collection<Age> ages)`                       | `… where x.age not in ?1`                                    |
| `True`                 | `findByActiveTrue()`                                         | `… where x.active = true`                                    |
| `False`                | `findByActiveFalse()`                                        | `… where x.active = false`                                   |
| `IgnoreCase`           | `findByFirstnameIgnoreCase`                                  | `… where UPPER(x.firstname) = UPPER(?1)`                     |

##### 使用JPA命名查询

使用<named-qiery/>元素和@NamedQuery注释。配置元素的查询必须在JPA查询语言中定义

**XML命名查询定义**

要使用XML配置，将<named-query/>元素添加到orm.xml位于META-INF类路径文件夹中的JPA配置文件中

示例：XML命名查询配置

```java
<named-query name=" User.findByLastname">
    <query>select u from u where u.lastname = ?1</query>
</named-query>
```

**基于注解的配置**

基于注解的配置优点是不需要编辑另一个配置文件，减少维护工作

示例：基于注解的命名查询配置

```java
@Entity
@NamedQuery(name = "User.findByEmailAddress",
           query = "select u from User u where u.emailAddress = ?1")
public class User{
    
}
```

**声明接口**

示例：UserRepository中的查询方法声明

```java
public interface UserRepository extends JpaRepository<User,Long>{
    List<User> findByLastname(String lastname);
    User findByEmailAddress(String emailAddress);
    
}
```

##### 使用@Query

使用命名查询来声明实体查询是一种很有效的方法，并且适用于少量查询。由于查询本身与运行他们的java方法相关联，因此可以通过使用Spring Data  JPA @Query注释直接绑定它们，而不是将他们注释到域类，

注释到查询方法的查询优先于使用中定义的@NamedQuery查询或声明的命名查询orm.xml

示例：在查询方法重声明查询使用@Query

```java
public interface UserRepository extends JpaRepository<User,Long>{
    @Query("select u from User u where u.emailAddress = ?1")
    User findByEmailAddress(String emailAddress);
}
```

**使用高级LIKE表达式**

示例：like  @Query 中的高级表达式

```java
public interface UserRepository extends JpaRepository<User,Long>{
    @Query("select u from User u where u.firstname like %?1")
    List<User> findByFirstnameEndsWith(String firstname);
}
```

Spring Data JPA目前不支持对原生查询进行动态排序，因为它必须操作声明的实际查询，而对于原生SQL，不能可靠做这一点

示例：在查询方法中声明用于分页的原生计数查询

```java
public interface UserRepository extends JpaRepository<User,Long>{
    @Query(value = "SELECT * FROM USERS WHERE LASTNAME = ?1",
           countQuery = "SELECT count(*) FROM USERS WHERE LASTNAME = ?1",
           nativeQuery = true)
    Page<USer> findByLastname(String lastname,Pageable pageable);
}
```

##### 使用排序

排序可以通过提供PageRequest或Sort直接使用来完成

使用任何不可引用的路径表达式会导致Exeption

若Sort和with一起使用@Query可以让Order包含ORDER BY子句中函数的非路径检查实例

示例：使用Sort和JpaSort

```java
public interface UserRepository extends JpaRepository<User,Long>{
    @Query("select u from User u where u.lastname like ?1%")
    List<User> findByAndSort(String lastname,Sort sort);
    
    @Query("select u.id,LENGTH(u.firstname) as fn_len from User u where u.lastname like ?1%")
    List<Object[]> findByArrayAndSort(String lastname,Sort sort);
}

repo.findByAndSort("lannister",Sort.by("firstname"));
repo.findByAndSort("stark",Sort.by("LENGTH(firstname)"));
repo.findByAndSort("targaryen",JpaSort.unsafe("LENGTH(firstname)"));
repo.findByAsArrayAndSort("bolton",Sort.by("fn_len"));
```

##### 使用命名参数

默认情况下，Spring Data JPA使用基于位置的参数绑定。

示例：使用命名参数

```java
public interface UserRepository extends JpaRepository<User,Long>{
    @Query("select u from User u where u.firstname = :firstname or u.lastname = :lastname")
    User findByLastnameOrFirstname(@Param("lastname") String lastname.
                                  @Param("firstname") String firstname);
}
```

方法参数根据它们在定义的查询中的顺序进行切换

##### 使用SpEL表达式

示例：在存储库查询方法中使用SpEL表达式-enetityName

```java
@Entity
public class User{
    @Id
    @GeneratedValue
    Long id;
    String lastname;
}

public interface UserRepository extends JpaRepository<User,Long>{
    @Query("select u from #(entityName) u where u.lastname = ?1")
    List<User> findByLastname(String lastname);
}
```

该entityName可以通过使用定制@Entity的注释

示例：在存储库查询方法中使用SpEL表达式--entityName与继承

```java
@MappedSuperclass
public abstract class AbstractMappedType{
    String attribute
}
@Entity
public class ConcrateType extends AbstractMappedType{}

@NoRepositoryBean
public interface MappedTypeRepository<T extends AbstractMappedType> extends Repository<T,Long>{
    
    @Query("select t from ${$entityName} t where t.attribute = ?1")
    List<T> findAllByAttribute(String attribute);
}
public interface ConcrateRepository extends MappedTypeRepository<ConcreteType>{}
```

操作参数的SpEL表达式可用于操作方法参数。在这些SpEL表达式中，实体名称不同，但参数可用。

示例：在存储库查询方法中使用SpEL表达式-访问参数

```java
@Query("select u from User u where u.firstname = ?1 and u.firstname = ?#{[0]} and u.emailAddress = ?#{principal.emailAddress}")
List<User> findByFirstnameAndCurrentUserWithCustomQuery(String firstname);
```

对于like-conditions，通常希望附加%到字符串值参数的开头或结尾

示例：在存储库查询方法中使用SpEL表达式-通配符快捷方式

```java
@Query("select u from User where u.lastname like %:{[0]}% and u.lastname like %:lastname%")
List<User> findByLastnameWithSpelExpression(@Param("lastname") String lastname);
```

当ilke对来子不安全来源的值使用-conditions时，应该对值进行清理，这样就布恩那个包含任何通配符，从而允许攻击者人选择比他们应该能选择的更多的数据

示例：在存储库查询方法中使用SpEL表达式-清理输入值

```java
@Query("select u from User u where u.firstname like %?${escape([0])} excape ?#{excapeCharacter()}")
List<User> findContainingEscaped(String namePart);
```

##### 修改查询

前面描述了如何查明查询以访问给定的实体或实体集合

示例：声明操作查询

```java
@Modifying
@Query("update User u set u.firstname = ?1 where u.lastname = ?2")
int setFixedFirstnameFor(String firstname,String lastname);
```

这样会触发注释到方法的查询作为更新查询而不是选择查询

**派生删除查询**

示例：使用派生的删除查询

```java
interface UserRepository extends Repository<User,Long>{
    void deleteByRoleId(long roleId);
    
    @Modifying
    @Query("delete from User u where u.role.id = ?1")
    void deleteInBulkByRoleId(long roleId);
}
```

##### 应用查询提示

要将JPA查询提示应用于存储库接口中声明的查询，可以使用@QueryHints注释。它需要一组JPA @QueryHint注释加一个布尔标志来潜在的禁用应用于应用分页时触发的附加计数查询的提示

示例：将QueryHints与存储库方法一起使用

```java
public interface UserRepository extends Repository<User,Long>{
    @QueryHints(value = {@QueryHint(name = "name",value = "value")},forCounting = false)
    Page<User> findByLastname(String lastname,Pageable pageable);
}
```

##### 配置Fetch-和LoadGraphs

示例：在实体上定义命名实体图

```java
@Entity
@NamedEntityGraph(name = "GroupInfo.detail",
attributeNodes = @NamedAttributeNode("members"))
public class GroupInfo{
    @ManyToMany
    List<GroupMember> members = new ArrayList<GroupMember>();
}
```

示例：在存储库查询方法上引用命名实体图定义

```java
public interface GroupRepository entends CrudRepository<GroupInfo,String>{
    @EntityGraph(value = "GroupInfo.detail",type = EntityGraphType.LOAD)
    GroupInfo.getByGroupName(String name);
}
```



ye可以使用@EntityGraph来定义临时实体图

示例：在存储库查询方法上使用AD-HOC实体图定义、

```java
public interface GroupRepository extends CrudRepository<GroupInfo,String>{
    @EntityGraph(attributePaths = {"members"})
    GroupInfo getByGroupName(String name);
}
```

##### 预测

Spring Data查询方法通常返回存储库管理的聚合根的一个或多个实例。但是，有时可能需要根据这些类型的某些属性创建投影。Spring Data 允许对专用返回类型进行建模，以更有选择的检索托管聚合的部分视图

示例：示例聚合和存储库

```java
class Person{
    @Id UUID id;
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

**基于界面的投影**

将查询结果限制为仅名称属性的最简单方法是声明一个接口，该接口公开要读取的属性的访问其方法

示例：用于检索属性子集的投影接口

```java
interface NamesOnly{
    String getFirstname();
    String getLastname();
}
```

示例：使用基于接口的投影和查询方法的存储库

```java
interface PersonRepository extends Repository<Person,UUID>{
    Collection<NamesOnly> findByLastname(String lastname);
}
```

可以递归的使用投影，若想包含一些Address信息，要为其创建一个投影接口

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

方法调用时，address获取目标示例的属性并一次包装到投影代理中

示例：闭合投影

```java
interface NamesOnly{
    String getFirstname();
    String getLastname();
}
```

若使用封闭投影，Spring Data可以优化查询执行，因为支持投影代理所需的所有属性

打开投影

投影接口中的访问器方法也可以用过使用@Value注释计算新值

示例：一个开放的投影

```java
interface NamesOnly{
    @Value("#{target.firstname + ' ' + target.lastname}")
    String getFullName();
}
```

支持投影的聚合根在target变量中可用。使用的投影界面@Value是开放式投影

示例：使用自定义逻辑的默认方法的投影界面

```java
interface NamesOnly{
    String getFirstname();
    String getLastname();
    
    default String getFullName(){
        return getFirstname().concat(" ").concat(getLastname());
    }
}
```

示例：Person对象

```java
@Component
class MyBean{
    String getFullName(Person person){
        
    }
    
}
interface NamesOnly{
    @Value("#{@myBean.getFullName(target)}")
    String getFullName();
}
```

从args数组中获取方法参数

示例：Person

```java
interface NamesOnly{
    @Value("#{args[0] + ' ' + target.firstname+'!'}")
    String getSalutation(String prefix);
}
```

可空包装器

投影接口中的getter可以使用可为空的包装器来提高空安全性

示例：使用可为空包装器的投影接口

```java
interface NamesOnly{
    Optional<String> getFirstname();
}
```

********基于类的预测(DTO********

定义投影的另一种方法是使用值类型DTO，DTO包含应该检索的字段的属性。

示例：一个投影DTO

```java
class NamesOnly{
    private final String firstname,lastname;
    NamesOnly(String firstname,String lastname){
        this.firstname = firstname;
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

可以使用Project Lombok显著简化DTO代码，提供一个@Value注解

```java
@Value
class NamesOnly{
    String firstname,lastname;
}
```

字段是private final默认的，该类公开了一个构造函数，该构造函数接受所有字段并自动获取equals和hashCode实现方法

基于类的投影不适用于本机查询

**动态投影**

示例：使用动态投影参数的存储库

```java
interface PersonRepository extends Repository<Person,UUID>{
    <T> Collection<T> findByLastname(String lastname,Class<T> type);
}
```

示例：使用具有动态投影的存储库

```java
void someMethod(PersonRepository people){
    Collection<Person> aggregates = people.findByLastname("Matthews",Person.class);
    
    Collection<NamesOnly> aggregates = people.findByLastname("Matthews",NamesOnly.class);
}
```

#### 2.1.4 存储过程

示例：plus1inout HSQL DB中过程的定义

```
/;
DROP procedure IF EXISTS plus1inout
/;
CREATE procedure plus1inout(IN arg int,OUT res int)
BEGIN ATOMIC
 set res = arg + 1;
END
/;
```

可以通过使用NamedStoredProcedureQuery实体类型上的注释来配置存储过程的元数据

示例：实体上的StoredProcedure元数据定义

```java
@Entity
@NamedStoredProcedureQuery(name = "User.plus1",procedureName = "plus1inout",paramters={
    @StoredProcedureParameter(mode = ParameterMode.IN,name="arg",type=Integer.class),
    @StoredProcedureParameter(mode = ParameterMode.OUT,name="res",type="Integer.classs")
})
public class User{}
```

示例：引用数据库 库名称为“plus1inout”的显示映射过程

```java
@Procedure("plus1inout")
Integer explicitlyNamedPlus1inout(Integer arg);
```

示例：通过procedureName别名在数据库中引用名为“plus1inout”的隐式映射过程

```java
@Procedure(procedure = "plus1inout")
Integer callPlus1InOut(Integer arg);
```

示例：EntityManager使用方法名称引用隐式映射的命名存储过程“User.plus1”

```java
@Procedure
Integer plus1inout(@Param("arg") Integer arg);
```

示例：在EntityManager

```java
@Procedure(name = "User.plus1IO")
Integer entityAnnottatedCustomNamedProcedurePlus1IO(@Param("arg") Integer arg);
```

#### 2.1.5规格

可以用JpaSecificationExecutor接口扩展存储库接口

```java
public interface CustomerRepository extends CrudRepository<Customer,Long>,JpaSpecificationExecutor<Customer>{
                                                                                                            }
```

附加接口的方法可以以多种方式运行规范。该findAll方法返回与规范匹配的所有实体

```java
List<T> findAll(Specification<T> spec);
```

Specification接口定义如下

```java
public interface Specification<T>{
    Predicate toPredicate(Root<T> root,CirteriaQuery<?> query,CriteriaBuilder builder);
}
```

规范可以很容易用于在实体之上构建一组可扩展的谓词，然后组合和使用这些谓词，JpaRepository无需为每个需要的组合声明查询

示例：客户规格（specification）

```java
public class CustomerSpecs{
    public static Specification<Customer> isLongTermCustomer(){
        return (root,query,builder) -> {
            LocalDate date = LocalDate.now().miunsYears(2);
            return builder.lessThan(root.get(Customer_.createdAt),date);
        };
    }
    
    public static Specification<Customer> hasSalesOfMoreThan(MonetaryAmount value){
        return (root,query,builder) -> {
            
        };
    }
}
```

Customer_类型是使用JPA元模型生成器生成的元模型类型。因此表达式，Customer_.createdAt假定Customer具有createdAt类型的属性Date

示例：使用简单的规范

```java
List<Customer> customers = customerRepository,findAll(isLongTermCustomer());
```

示例:组合规格

```java
MonetaryAmount amount = new MonetaryAmount(200.0,Currencies.DOLLAR);
List<Customer> customers = customerRepository.findAll(
    isLongTermCustomer().or(hasSalesOfMoreThan(amount));
)
```

#### 2.1.6按示例查询

##### 介绍

示例查询（QBE）是一种用户友好的查询技术，具有简单的界面。允许动态创建查询，不需要编写包含字段名称的查询，

##### 用法

Query by example API由三部分组成

探针：具有填充字段的域对象的实际示例

ExampleMatcher：ExampleMatcher包含有关如何匹配特定字段的详细信息。

Example：Example由探针和ExampleMatcher，用于创建查询

Query by Example适合以下几个用例：

使用一组静态或动态约束查询数据存储

频繁重构对象而不必担心破坏现有查询

独立于底层数据存储API工作

Query by Example的限制：

不支持嵌套或分组的属性约束

仅支持字符串的开始/包含/结束/正则表达式匹配以及其他属性类型的精确匹配

示例：示例person对象

```java
public class Person{
    @Id
    private String id;
    private String firstname;
    private String lastname;
    private Address address;
    //getters and setters omitted
}
```

可以使用of factory方法或使用ExampleMathcer，Example是不可变的

示例：简单示例

```java
Person person = new Person();//创建域对象的新实例
person.setFirstname("Dave");//设置要查询的属性

Example<Person> example = Example.of(person);//创建Example
```

可以使用存储库运行示例查询。

为此 存储库接口扩展了QueryByExampleExecutor<T>

示例：QueryByExampleExecutory界面的摘录

```java
public interface QueryByExampleExecutor<T>{
    <S extends T> S findOne(Example<S> example);
    <S extends T> Iterable<S> findAll(Example<S> example)
}
```

##### 示例匹配器

示例不限于默认设置

示例:具有自定义匹配的示例匹配器

```java
Person person = new Person();
person.setFirstname("Dave");

ExampleMathcer matcher = ExampleMatcher.matching()//创建一个ExmpaleMathcer以期望所有值匹配.
    .withIgnorePaths("lastname")//构造一个新ExampleMatcher的忽略lastname属性路径
    .withIncludeNullValues()//构造一个new ExampleMathcer以忽略lastname属性路径并包含空值
    .withStringMathcer(StringMathcer.ENDING);//构造一个新的ExampleMathcer来忽略lastname属性路径,包含空值,并执行后缀字符串匹配

EXample<Person> example = Example.of(person,matcher);//创建一个新Example基于域对象和配置上ExmpaleMatcher
```

默认情况下,ExampleMatcher期望在探测器上设置的所有值都匹配.若要获得与任何隐式定义的谓词匹配的结果,要使用ExampleMatcher.matchingAny()

可以为单个属性指定行为.可以使用匹配选项和区分大小写来调整

示例:配置匹配器选项

```java
ExampleMathcer matcher = ExampleMatcher.matching()
    .withMathcer("firstname",endWith())
    .withMathcer("lastname",startsWith().ignoreCase());
```

另一种配置匹配器选项的方法是使用lambdas.这种方法创建了一个回调,要求实现者修改匹配器.

示例:使用lambda配置匹配器选项

```java
ExampleMathcer matcher = EXampleMatcher.matching()
    .withMathcer("firstname",match -> match.endsWith())
    .withMathcer("lastname",mathch -> match.startsWith());
```

Example使用配置的合并视图创建的查询.默认匹配设置可以在ExampleMathcer级别设置,而单独的设置可以应用于特定的属性路径.

| Setting              | Scope                              |
| :------------------- | :--------------------------------- |
| Null-handling        | `ExampleMatcher`                   |
| String matching      | `ExampleMatcher` and property path |
| Ignoring properties  | Property path                      |
| Case sensitivity     | `ExampleMatcher` and property path |
| Value transformation | Property path                      |

运行实例

在Spring Data JPA中,可以将Query by Example与Repositories一起使用

实例:使用存储库按实例查询

```java
public interface PersonRepository extends JpaRepository<Person,String>{}
public class PersonService{
    @Autowired PersonRepository personRepository;
    
    public List<Person> findPeople(Person probe){
        return personRepository.findAll(Example.of(probe));
    }
}
```

属性说明符接受属性名称

下表显示了StringMacher可以使用的各种选项以及在名为firstname的字段上使用他们的结果

| 匹配                          | 逻辑结果                                      |
| :---------------------------- | :-------------------------------------------- |
| `DEFAULT` （区分大小写）      | `firstname = ?0`                              |
| `DEFAULT` （不区分大小写）    | `LOWER(firstname) = LOWER(?0)`                |
| `EXACT` （区分大小写）        | `firstname = ?0`                              |
| `EXACT` （不区分大小写）      | `LOWER(firstname) = LOWER(?0)`                |
| `STARTING` （区分大小写）     | `firstname like ?0 + '%'`                     |
| `STARTING` （不区分大小写）   | `LOWER(firstname) like LOWER(?0) + '%'`       |
| `ENDING` （区分大小写）       | `firstname like '%' + ?0`                     |
| `ENDING` （不区分大小写）     | `LOWER(firstname) like '%' + LOWER(?0)`       |
| `CONTAINING` （区分大小写）   | `firstname like '%' + ?0 + '%'`               |
| `CONTAINING` （不区分大小写） | `LOWER(firstname) like '%' + LOWER(?0) + '%'` |

#### 2.1.7 交易性

默认情况下,从存储库实例继承的CRUD方法 SImpleJpaRepository是事务性的.

对于读取操作,事务配置readOnly标志设置为true.

@Transactional以便应用默认事务配置,由事务存储库片段支持的存储库方法从实际片段方法继承事务属性

实例:CRUD的自定义事务配置

```java
public interface UserRepository extends CrudRepository<User,Long>{
    @Override
    @Transactional(timeout = 10)
    public List<Uers> findAll();
}
```

改变事务行为的另一种方法是使用覆盖多个存储库的外观或服务实现.目的是为非CRUD操作定义事务边界.

实例:使用Facade为多个存储库调用定义事务

```java
@Service
public class UserManageMentImpl implements UserManagement{
    private final UserRepository userRepository;
    private final RoleRepository roleRepository;
    
    public UserManagementImpl(UserRepository userRepository,
                             RoleRepository roleRepository){
        this.userRepository = userRepository;
        this.roleRepository = roleRepository;
    }
    
    @Transactional
    public void addRoleToAllUsers(String roleName){
        Role role = roleRepository.findByName(roleName);
        
        for(User user:userRepository.findAll()){
            user,addRole(role);
            userRepository.save(user);
        }
    }
}
```

##### 事务查询方法

若要查询方法是事务性的,要在定义的存储库接口处使用@Transactional

实例:在查询方法中使用@Transactional

```java
@Transactional(readOnly = true)
interface UserRepository extends JpaRepository<User,Long>{
    List<User> findByLastname(String lastname);
    
    @Modifying
    @Transactional
    @Query("delete from User u where u.active = false")
    void deleteInactiveUsers();
}
```

#### 2.1.8 锁定

实例:在查询方法上定义锁元数据

```java
interface UserRepository extends Repository<User,Long>{
    @Lock(LockModeType.READ)
    List<User> findByName(String lastname);
}
```

此方法声明导致被触发的查询配备一个LockModeType of READ

示例:在CRUD方法上定义锁元数据

```java
interface UserRepository extends Repository<User,Long>{
    @Lock(LockModeType.READ)
    List<User> findAll();
}
```

#### 2.1.9 审计

##### 基本

Spring Data提供了复杂的支持,以透明的跟踪创建或更改了实体以及更改发生的事件

##### 基于注释的审计元数据

提供@CreatedBy和@LastModifieBy捕获创建或修改实体的用户,

@CreatedDate和@LastModifiedDate捕获更改发生的时间

示例:一个被审计的实体

```java
class Customer{
    @CreatedBy
    private User user;
    
    @CreatedDate
    private Instant createdDate;
}
```

示例:审计嵌入实体中 的元数据

```java
class Customer{
    private AuditMetadata auditingMetadata;
}
class AuditMetadata{
    @CreatedBy
    private User user;
    @CreatedDate
    private Instant createdDate;
}
```

**基于接口的审计元数据**

AuditorAware

若使用@CreatedBy或LastModifiedBy,审计基础结构需要以某种方式了解当前主体.

示例:AuditorAware基于Spring Security的实体

```java
class SpringSecurityAuditorAware implements AuditorAware<User>{
    
    @Override
    public Optional<User> getCurrentAuditor(){
        return Optional.ofNullable(SecuriyContextHolder.getContext())
            .map(SecurityContext::getAuthentication)
            .filter(Authentication::isAuthenticated)
            .map(Authentication::getPrincipal)
            .map(User.class::cast);
    }
}
```

该实现访问Authentication Spring Security提供的对象并查找userDetails在UserDetailsService实现中创建的自定义实例

ReactiveAuditorAware

使用反应式基础架构时,可能希望使用上下文信息来提供@CreatedBy或提供@LastModifiedBy信息.提供一个ReactiveAuditorAware<T> SPI接口,必须实现该接口才能告诉基础设施当前与应用程序监护的用户或系统是谁

实例:ReactiveAuditorAware基于Spring Security的实现

```java
class SpringSecurityAuditorAware implements ReactiveAuditorAware<User>{
    @Override
    public Mono<User> getCurrentAuditor(){
        return ReactiveSecurityContextHolder.getContext()
            .map(SecurityContext::getAuthentication)
            .filter(Authentication::isAuthenticated)
            .map(Authentication::getPrincipal)
            .map(User.class::cast);
    }
}
```

该实现访问Authentication Spring Security提供的对象并查找UserDetails 在UserDetailsService实现中创建的自定义实例

#### 2.1.10 JPA审计

##### 常规审计配置

Spring Data JPA附带一个实体侦听器,可用于触发审计信息的捕获.首先必选在orm.xml文件内注册要勇于所有实体的持久性上下文

示例:审计配置orm.xml

```xml
<persistence-unit-metadata>
  <persistence-unit-defaults>
    <entity-listeners>
      <entity-listener class="….data.jpa.domain.support.AuditingEntityListener" />
    </entity-listeners>
  </persistence-unit-defaults>
</persistence-unit-metadata>
```

可以在AuditingEntityListener使用@EntityListeners注释在每个实体的基础上启用

```java
@Entity
@EntityListeners(AuditingEntityListener.class)
public class MyEntity{
    
}
```

审计功能需要在spring-aspects.jar类路径上

示例：使用XML配置激活Auditing

```xml
<jpa:auditing auditor-arare-ref="yourAuditorAwareBean"/>
```

也可以通过使用注释对配置类进行@EnableJpaAuditing注释来启用Auditing

示例：使用Java配置激活Auditing

```java
@Configuraion
@EnableJpaAuditing
class Config{
    @Bean
    public AuditorAware<AuditableUser> auditorProvider(){
        return new AuditorAwareImpl();
    }
}
```

若向公开类型AuditorAware为bean ApplicaitonContext，审计基础结构会自动选取它并使用它来确定要在与类型上设置的当前用户。

### 2.2 其他注意事项

#### 2.2.1 使用JpaContext在自定义实现

当使用多个EntityManager实例 和自定义存储库实现时，需要将正确的连接EntityManager到存储库实现类始终。

可以通过EntityManager在@PersistenceContext注释中显示命名来实现或者若EntityManager是@Autowired，则使用@Qualifier

Spring Data Jpa包含一个类，假设仅由应用程序中的一个实例管理，JpaContext，EntityManager可以通过该类获取受管理的域类EntityManager

示例 ：JpaContext在自定义存储库实现中使用

```java
class UserRepositoryImpl implements UserRepositoryCustom{
    private final EntityManager em;
    
    @Autowired
    public UserRepositoryImpl(JpaContext context){
        this.em = context.getEntityManagerByManagedType(User.class);
    }
}
```

优点--若域类型被分配给不同的持久性单元，则不必接触存储库来更改对持久性单元的引用

#### 5.2.2 合并持久化单元

Spring 支持拥有多个持久化单元。有时可能希望对应用程序进行模块化，但仍要确保所有这些模块都在单个持久性单元中运行。为了实现这种行为，Spring Data JPA提供了一个PersistenceUnitManger实现，根据名称合并持久性单元

示例：使用MergingPersistenceUnitManger

```xml
<bean class = "^.LocalContainerEntityManagerFactoryBean">
    <property name = "persistenceUnitManager">
        <bean class  = "^.MergingPersistenceUnitManager"/>
    </property>
</bean>
```

##### @Entity类和JPA映射文件的类路径扫描

一个普通的JPA设置需要在rom.xml，同样适用于XML映射文件，Spring Data JPA提供了一个可以配置基本包并可选的采用映射文件名模式的党法ClassPathScanningPersistenceUnitPostProcessor。然后扫描给定包中用@Entity或注释的类@MappedSuperclass，加载与文件名模式匹配的配置文件，并将它们交给JPA配置

示例：使用ClasspathScanningPersistenceUnitPostprocessor

```xml
<bean class = "^.LocalContainerEntityManagerFactoryBean">
    <property name = "persistenceUnitPostProcessor">
        <list>
        	<bean class = "org.psringframework.data.jpa.support.ClasspathScanningPersistenceUnitPostProcessor">
                <constructor-arg value="com.acme.domain"/>
                <property name = "mappingFileNamePattern" value="**.*Mapping.xml"/>
            </bean>
        </list>
    </property>
</bean>
```

#### 2.2.3 CDI集成

存储库接口的实例通常由容器创建，因此在使用Spring Data时，Spring是最自然的选择。Spring为创建bean实例提供了复杂的支持。

Spring Data JPA附带了一个自定义CDI扩展，允许在CDI环境中使用存储库抽象，该扩展时JAR的一部分。若要激活，则在类路径中包含Spring Data JPA JAR

示例：通过为EntityManagerFactory和实现CDI Producer来设置基础结构EntityManager

```java
class EntityManagerFactoryProducer{
    @Produces
    @ApplicationScoped
    public EntityManagerFactory createEntityManagerFactory(){
        return Persistence.createEntityManagerFactory("my-persistence.unit");
    }
    
    public void close(@Disposes EntityManagerFactory enetityManagerFactory){
        enetityManagerFactory.close();
    }
    
    @Produces
    @RequestScoped
    public EntityManager createEntityManager(EntityManagerFactory entityManagerFactory){
        return entityManagerFactory.createEntityManager();
    }
    
    public void close(@Disposes EntityManager entityManager){
        entityManager.close();
    }
}
```

必要的设置可能因JavaEE环境不同，只需要重新生命EntityManager 为CDI bean

```java
class CidConfig{
    @Produces
    @RequestScoped
    @PersistenceContext
    public EntityManager entityManager;
}
```

容器必须能够EntityManager自己创建JPA

Spring Data JPA CDI扩展将所有可用EntityManager实例作为CDI bean选取，并在容器请求存储库类型的bean为Spring Data 存储库擦黄健代理。获取Spring  Data存储库的实例是声明@Injected属性的问题

```java
class RepositoryClient{
    @Inject
    PersonRepository repository;
    
    public void businessMethod(){
        List<Person> people = repository.findAll();
    }
}
```





