# SpringData

## 1.依赖

使用SpringData POM

<dependencyManagement>

   <dependencies>

​     <dependency>

​       <groupId>org.springframework.data</groupId>

​       <artifactId>spring-data-bom</artifactId>

​       <version>***（适应版本需求）</version>

​       <scope>import</scope>

​       <type>pom</type>

​     </dependency>

   </dependencies> </dependencyManagement>

声明对Spring Data模块的依赖

<dependencies>  <dependency>

​    <groupId>org.springframework.data</groupId>

​    <artifactId>spring-data-jpa</artifactId>

  </dependency> <dependencies>

### 1.1 使用Spring Boot进行依赖管理

SpringBoot会选择最新版本的Spring Data模块。

### 1.2 SpringFrame

强烈建议使用最新版本

### 1.3 Object Mapping Fundamentals

涵盖Spring Data对象映射、对象创建、字段和属性访问、可变性和不可变性的基础知识。

仅适用于不使用底层数据存储（such as JPA）的对象映射的Spring Data模块。

Spring Data 对象映射的核心职责是创建域对象的实例并将存储本机数据结构刀这些实力上

两个基本步骤：

​	1.使用公开的构造函数之一创建实例。

​	2.实例填充用来实现所有公开的属性。

## 2.对象映射基础

### 2.1对象创建

Spring Data 会自动尝试检测要用于具体化该类型对象的持久实体的构造函数。

​	 1.如果只有一个构造函数，使用。

​	2.如果有多个构造函数并且只有一个用注释@PersistenceConstructor，使用。

​	3.如果存在无参数构造函数，使用。其他构造函数被忽略。

可以通过使用@Value特定的SpEL表达式使用SpringFramework的值注释来自定义值解析。

对象创建内部：

Spring Data对象创建默认使用运行时生成的工厂类，会直接调用域类构造函数。

```java
class Person{

	Person(String firstname,String lastname){......}

}
```

在运行时创建一个语义上等同于这个的factory

```java
class PersonObjectInstantIator implements ObjectInstantiator{

	Object newInstantance(Object ... args){

		return new Person((String) args[0],(String) args[1]);		

	}

}
```

提高性能

{非private class

非静态内部类

不是CGLib代理类

Spring Data使用的构造函数不能是私有化的}

### 2.2 Property population

一旦创建了实体例，Spring Data就会填充该类的所有剩余持久属性。除非尸体的构造函数已经填充。

1.若属性是不可变的  公开了一个with…方法，我们使用改with...方法创建一个具有新属性值的新实体例。

2.如果定义了属性访问（通过getter和setter访问），我们将调用setter方法。

3.如果属性是可变的，我们直接设置字段。

4.如果属性是不可变的，我们将使用持久性操作使用的构造函数来创建实例的副本。

5.默认情况下，我们直接设置字段值。

#### Property population 内部类

```java
class Person {

  private final Long id;
  private String firstname;
  private @AccessType(Type.PROPERTY) String lastname;

  Person() {
    this.id = null;
  }

  Person(Long id, String firstname, String lastname) {
    // Field assignments
  }

  Person withId(Long id) {
    return new Person(id, this.firstname, this.lastame);
  }

  void setLastname(String lastname) {
    this.lastname = lastname;
  }
}
```

#### 生成的属性访问器

```java
class PersonPropertyAccessor implements PersistentPropertyAccessor{
    private static final MethodHandle firstname;
    private Person person;
    public void setProperty(PersistentProperty property,Object value){
        String name = property.getName();
        if("firstname".equals(name)){
            firstname.invoke(person,(String) value);
        }else if("id".equals(name)){
            this.person=person.withId((Long) value);
        }else if("lastname".equals(name)){
            this.person.setLastname((String) value);
        }
    }
}
```

PropertyAccessor持有底层对象的可变实例（为了启用其他不可变属性的突变）

默认情况Spring Data使用字段访问来读取和写入属性值。根据private字段的可见性规则，MethodHandles用于与字段交互。

该类公开了一个withId（）用于设置标识符的方法，例如：当一个实例插入到数据存储中并生成一个标识符。调用withId（）创建一个新Person对象。

使用属性访问允许直接方法调用而不是用MethodHandles

提高性能，遵守约束：

类型不得位于默认值或者java包下

类型及其构造函数必须是public

属于内部类的类型必须是static

使用的java运行时必须允许在原式ClassLoader

示例实体

```java
package com.entity;

import org.springframework.data.annotation.AccessType;
import org.springframework.data.annotation.Id;

import java.time.LocalDate;
import java.time.Period;

public class Person {
    private final @Id Long id;
    private final String firstname,lastname;
    private final LocalDate birthday;
    private final int age;
    private String comment;
    private @AccessType(AccessType.Type.PROPERTY) String remarks;

    static Person of(String firstname,String lastname,LocalDate birthday){
        return new Person(null,firstname,lastname,birthday, Period.between(birthday,LocalDate.now()).getYears());
    }
    Person(Long id,String firstname,String lastname,LocalDate birthday,int age){
        this.id=id;
        this.firstname=firstname;
        this.lastname=lastname;
        this.birthday=birthday;
        this.age=age;
    }
    Person withId(Long id){
        return new Person(id,this.firstname,this.lastname,this.birthday,this.age);
    }
    void setRemarks(String remarks){
        this.remarks=remarks;
    }
}
/*  **identifier属性是final但是在构造函数中设置为null。该类公开了一个withId()用于设置标识符的方法。
当一个实例插入到数据存储中并生成一个标识符时。Person创建新实例时，原始实例保持不变。
    **firstname和lastname是不可变属性。
    **age属性是不可变的，源自birthday属性。此设计，数据库值>默认值，由于Spring Data使用唯一声明的构造函数。
此构造函数也将age作为参数。
    **comment属性是可变的，可以直接通过设置字段来填充。
    **remarks属性是可变的，可以通过设置字段填充comment或通过调用用于setter方法。
    **该类公开了一个factory方法和一个用于创建对象的构造函数。。核心思想是使用factory方法而不是额外的构造函数
避免需要通过@PersistenceConstructor。属性的默认设置实在factory方法中处理。
*/
```

### 2.3 建议

1.尽量坚持不可变对象

不可变对象容易创建，因为具体化一个对象只是调用它的构造函数的问题。避免了域对象被允许客户端代码操作对象状态的setter方法困扰。

2.提供一个全参数构造函数

不想或不能将实体建模为不可变值，提供一个将实体的所有属性作为参数的构造函数仍然有用，允许对象映射跳过属性填充来获得最佳性能。

3.使用factory方法而不是重载的构造函数来避免@PersistenceConstructor

4.确保遵守允许使用生成的实例化器和属性访问器类的约束

5.对于要生成的标识符，仍然使用final字段和全参数持久性构造函数或with...方法相结合

6.对于Lombok避免样板代码

### 2.4 Kotlin支持

Spring Data调整了Kotlin的细节以允许对象创建和改变。

```java
package com.entity

import org.springframework.data.annotation.PersistenceConstructor

//data class Person(val id:String,val name:String) {
//具有显式构造函数的典型类。可以通过另一个构造函数来自定义这个类，并使用注释@PersistenceConstructor来指示构造函数首选项
//    @PersistenceConstructor
//    constructor(id: String):this(id,"unknown")
//}
//Kotlin通过允许在未提供参数的情况下使用默认值来支持参数可选性。当Spring Data检测到具有参数默认值的构造函数时，如果
//数据存储不提供值，它就会使这些参数不存在，因此Kotlin可以应用参数默认值
data class Person(var id:String,var name:String = "unknown")
```

## 3.使用Spring Data Repositories

### 3.1核心概念

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

### 3.2 查询方法

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

### 3.3 定义存储库接口

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

### 3.4 定义查询方法

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

### 3.5 创建存储库实例

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





