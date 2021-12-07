# 1.Java Spring

## 1.1、简介

Srping：春天---------给软件行业带来了春天！

2002，首次推出了Spring框架的雏形，interface 21框架

Spring框架一interface21框架为基础，经过重新设计，并不断丰富其内涵，2001.3.24发布了1.0版本

Rod Johnson，Spring framework创始人，著名作者，很难想象Rod Johnson的学历，悉尼大学博士音乐学。

spring理念：使现有的技术更加容易使用，本身是一个大杂烩，整合了现有的技术框架。



SSH：Sruct2+Spring+Hibernate

SSM：SpringMvc+Spring+Mybatis



官网：https://spring.io/prokects/spring-framework#overview

官方下载地址：https://docs.spring.io/spring/docs/4.3.9.RELEASE/spring-framework-reference/html

Github：https://github.com/spring-protects/spring-framework

<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.7</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.3.7</version>
</dependency>

## 1.2、优点

Spring是一个开源的免费的框架（容器）！

Spring是一个轻量级的、非侵入式的框架

控制反转（IOC），面向切面编程（AOP）

支持事务的处理，对框架整合的支持



总结：Spring就是一个轻量级的控制反转（IOC）和面向切面编程（AOP）的框架

## 1.3组成

![image-20210524160609495](C:\Users\云\AppData\Roaming\Typora\typora-user-images\image-20210524160609495.png)

## 1.4拓展

在Spring官网的介绍：现代化的Java开发，说白了就是基于Spring的开发

![image-20210524160821402](C:\Users\云\AppData\Roaming\Typora\typora-user-images\image-20210524160821402.png)

Spring Boot

​	一个快速开发的猪手架。

​	基于SpringBoot可以快速的开发单个微服务

​	约定大于配置

Spring Cloud

​	SpringCloud是基于SpringBoot实现的

​	

因为现在大多数公司都在使用SpringBoot进行快速开发，学习SpringBoot的前提，需要掌握Spring以及SpringMVC，承上启下。

**弊端：发展太久以后，违背了原来的理念，配置十分繁琐，人称“配置地狱”**

# 2.IOC理论推导

1.UserDao接口

2.UserDaoImpl实现类

3.UserService业务接口

4.UserServiceImpl业务实现类

​	在之前的业务中，用户的需求可能会影响我们原来的代码，我们需要根据用户的需求去修改源代码，如果程序代码量十分大，修改一次的成本代价十分昂贵！

​	**之前，程序是主动创建对象，控制权在程序猿手上**

​	**使用set注入后，程序不再具有主动性，而是变成了被动的接受对象!**

这种思想，从本质上解决了问题，我们程序猿不用再去管理对象的创建了。系统的耦合性大大降低，可以更加专注的在业务的实现上，这是IOC的原型。

![image-20210525182020508](C:\Users\云\AppData\Roaming\Typora\typora-user-images\image-20210525182020508.png)

## IOC本质

控制反转IOC是一种设计思想， DI（依赖注入）是实现IOC的一种方法，也有人认为DI只是IOc的另一种说法，没有IOC的程序，我们使用面向对象编程，对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是：获得依赖对象的方式反转了。

采用XML方式配置Bean的时候，Bean的定义信息是和实现分离的，而采用注解的方式可以把两者合为一体，Bean的定义信心直接以注解的形式定义在实现类中，从而达到勒令配置的目的。

**控制反转是一种通过描述（XML或注解）并通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是IOC容器，其实现方法是依赖注入(DePendency Injection,DI)。**

# 3.HelloSpring

1.编写一个Hello实体类

```java
public class Hello {
    private String name;
    public String getName(){
        return name;
    }
    public void setName(String name){
        this.name = name;
    }
    public void show(){
        System.out.println("Hello"+name);
    }
}
```

2.再编写我们的spring文件，这里命名为beans.xml

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/util https://www.springframework.org/schema/util/spring-util.xsd">
     <!--bean就是java对象 , 由Spring创建和管理
     类型 变量名= new 类型（）；
     Hello hello = new Hello（）；
     id=变量名；
     class = new 的对象；
     property相当于给对象中的属性设置一个值！
     -->
      <bean id = "hello" class = "Hello">
              <property name="name" value="string"></property>
      </bean>
</beans>
```

3.测试

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Mytext {
    public static void main(String[] args) {
//        获取Spring的上下文对象
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
//        我们的对象现在都在Spring中的管理了，我们要使用，直接去取出来就可以。
        Hello hello = (Hello)context.getBean("hello");
        System.out.println(hello.toString());
    }
}
```

思考问题

Hello对象是谁创建的？Hello对象是由Spring创建的

Hello对象的属性是怎么设置的？Hello对象的属性是由Spring容器设置的

过程叫做反转:

控制：谁来控制对象的创建，传统应用程序的对象是由程序本身控制创建的，使用Spring后，对象是由Spring来创建的。

反转：程序本身不创建对象，而变成被动的接收对象。

依赖注入：就是利用set方法来进行注入的。

IOC是一种编程思想，由主动的编程编程被动的接收

可以通过newClassPathXmlApplicationContext去浏览底层源码

不在程序中改动，要实现不同的操作，只需要再xml配置文件中进行修改，所谓的IOC，一句话搞定：对象由Spring来创建，管理，装配！

# 4.IOc创建对象的方式

1.使用无参构造创建对象，默认

```
public User(){
    System.out.println("user的无参构造");
}
```

```
<bean id = "user" class = "User">
    <property name="name" value="秦奖"/>
</bean>
```

2.使用有参构造创建对象

​	1.下标赋值

```
<bean id = "user" class = "User">
<!--        <property name="name" value="秦奖"/>-->
        <constructor-arg index="0" value="lll"/>
    </bean>
```

​	2.通过类型创建（不建议使用）

```
<bean id="user" class="User">
    <constructor-arg type="User" value="qinjiang"/>
</bean>
```

​	3.直接通过参数名

```
<bean id="user" class="User">
    <constructor-arg name="name" value="qinjiang"/>
</bean>
```

总结：在配置文件加载的时候，容器中管理的对象就已经初始化了

# 5.Spring配置

1.别名

```java
<!--别名，如果添加了别人，我们也可以使用别名获取到这个对象-->
<alias name="user" alias="user2"/>
```

2.bean的配置

```java
<!--
   id:bean的唯一标识符，也就是相当于我们学的对象名
   class:bean对象所对应的全限定名+包名+类型
   name:也是别名,而且name可以同时去多个别名
-->
<bean id="user" class="User" name="user3,4,5,6">
    <property name="name" value="xxx"/>
</bean>
```

3.import

这个import，一般用于团队开发使用，可以将多个配置文件，导入合并为一个

假设，现在项目中有多个人开发，三个人复制不同的类开发，不同的类需要注册在不同的bean中，我们可以利用import将所有人的beans.xml合并在一个总的

张三、李四、王五

applicationContext.xml

```java
<import resource="beans.xml"/>
<import resource="beans2.xml"/>
<import resource="beans3.xml"/>
```

使用的时候直接使用总的配置就行

# 6.Groovy Bean定义DSL

```java
beans{
    dataSource(BasicDataSource){
        driverClassName="org.hsqldb.jdbcDriver"
        url="jdbc:hsqldb:mem:grailsDB"
        username="sa"
        password=""
        settings=[mynew:"setting"]
    }
    sessionFactory(SessionFactory){
        dataSource=dataSource
    }
    myservice(MyService){
        nestedBean={AnotherBean bean ->
            dataSource = dataSource
        }
    }
}
```

这种配置风格在很大程度上等同于XML bean定义，甚至支持Spring的XML配置命名空间，它还允许通过importBeans指令导入XML bean定义文件。

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.GenericGroovyApplicationContext;

public class jdbcDriver {
    public static void main(String[] args) {
        ApplicationContext context = new GenericGroovyApplicationContext("beans.groovy");
    }
}
```

最灵活的变体是GenericApplicationContext--

```java
GenericGroovyApplicationContext context = new GenericGroovyApplicationContext();
new XmlBeanDefinitionReader(context).loadBeanDefinitions("beans.groovy");
context.refresh();
```

还可以用GroovyBeanDefinitionReader 对于Groovy文件

```java
GenericGroovyApplicationContext context = new GenericGroovyApplicationContext();
new GroovyBeanDefinitionReader(context).loadBeanDefinitions("beans.groovy");
context.refresh();
```

可以在同一 个 项目 上  混合和此类读取器 ApplicationContext，从不同的配置源读取bean定义。

而后可用getBean来检索bean 的案例，Application接口有一些其他方法来检索bean，可能根本不需要调用该getBean方法，因此完全不依赖Spring API

Spring和Web框架的集成为各种Web框架组件（控制器的JSF和管理的bean）提供了依赖注入，可以用过元数据（如自动装配注释）声明对特定bean 的依赖

# 7.使用factory方法进行实例化

在定义使用static factoy方法创建的bean时，使用class 属性来指定包含static factory方法的类和命名factory-method为指定factory方法本身名称的属性。

以下bean指定通过调用factory方法来创建bean，定义中没有指定返回对象的类型。只包含factory方法的类

xml典型配置

```java
<bean id="clientService"
      class="examples.Clientservice"
      factory-method="createInstance"/>
```

类典型配置

```java
public class Clientservice {
    private static Clientservice clientservice = new Clientservice();
    private Clientservice(){}

    public static Clientservice createInstance(){
        return clientservice;
    }
}
```

与通过静态factory方法实例化类似，使用私立factory方法实例化从容器中调用现有的bean的非静态方法来创建新bean。用此，将class属性留空，并在factory-bean属性中指定当前容器中bean 的名称，该容器包含要调用以创建对象的实例方法。使用factory-method属性设置factory方法本身的名称

xml类

```java
<!-- the factory bean,which contains a method called createInstance() -->
<bean id="serviceLocator" class="examples.DefaultServiceLocator">
    <!-- inject any dependencies required by this locator bean -->
</bean>
<bean id="clientService"
      class="examples.DefaultServiceLocator"
      factory-method="createClientServiceInstance"/>
```

类代码

```java
public class DefaultServiceLocator {
    private static Clientservice clientservice = new ClientserviceImpl();
    public Clientservice createClientServiceInstance(){
        return clientservice;
    }
}
```

一个factory也可以包含多个factory方法

xml

```java
<bean id="serviceLocator" class="examples.DefaultServiceLocator">
    <!-- inject any dependencies required by this locator bean -->
</bean>

<bean id="clientService"
      factory-bean="serviceLocator"
      factory-method="createClientServiceInstance"/>

<bean id="accountService"
      factory-bean="serviceLocator"
      factory-method="createAccountServiceInstance"/>
```

类代码

```java
public class DefaultServiceLocator {
    private static Clientservice clientservice = new ClientserviceImpl();
    pirvate static AccountService accountService = new AccountServiceImpl();
    public Clientservice createClientServiceInstance(){
        return clientservice;
    }
    public AccountService createAccountServiceInstance(){
        return accountService;
    }
}
```

文档中的 “factory bean”是指在Spring容器中配置并通过实例或静态factory方法创建对象的bean/

而FactoryBean是指特定于Spring的FactoryBean实现类。

## 确定Bean的运行时类型

确定特定bean的运行时类型并非易事，bean元数据定义中的指定类只是一个初始类引用，可能与声明的factory方法结合，或者是FactoryBean可能导致bean的不同运行时类型的类，或者在实例的情况下根本没有设置-级别factory方法。此外AOP代理可以使用基于接口的代理包装bean实例，并限制暴露目标bean的实际类型。

找出特定bean 实际运行时类型的推荐方法是BeanFactory.getType调用指定的bean名称，

# 7、依赖注入

## 1.构造器注入

构造函数参数解析

构造函数参数解析匹配通过使用参数的类型发生。如果bean定义的构造函数参数中不存在潜在的歧义，那么bean定义中定义构造函数参数的顺序就是在实例化bean时将这些参数提供给适当的构造函数的顺序

ThingOne类

```java
package x.y;

public class ThingOne {
    public ThingOne(ThingTwo thingTwo, ThingThree thingThree){
        //...
    }
}
```

ThingTwo，ThingThree工具类

xml代码如下

```java
<bean id="beanOne" class="x.y.ThingOne">
    <constructor-arg ref="beanTwo"/>
    <constructor-arg ref="beanThree"/>
</bean>

<bean id="beanTwo" class="x.y.ThingTwo"/>
<bean id="beanThree" class="x.y.ThingThree"/>
```

当另一个bean被引用时，类型是已知的，并且可以发生匹配。当使用简单类型时，Sring无法确定值的类型，因此无法在没有帮助的情况下按类型进行匹配。

ExampleBean类如下

```java
public class ExampleBean {
    private final int years;
    private final String ultimateAnswer;

    public ExampleBean(int years, String ultimateAnswer) {
        this.years = years;
        this.ultimateAnswer = ultimateAnswer;
    }
}
```

xml如下

```java
<!-- 构造函数参数类型匹配 -->
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg type="int" value="150000"/>
    <constructor-arg type="java.lang.String" value="42"/>
</bean>
<!-- 构造函数参数索引 -->
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg index="0" value="45000"/>
    <constructor-arg index="1" value="32"/>
</bean>
```

## 2.**set方式注入**

依赖注入：set注入

​	依赖：bean对象的常见依赖于容器

​	注入：bean对象中的所有属性，由容器注入

【环境搭建】

1.复杂类型

```java
public class Address {
    private String address;

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }
}
```

2.真实测试对象

```java
public class Student {
    private String name;
    private Address address;
    private String[] books;
    private List<String> hobbys;
    private Map<String,String> card;
    private Set<String> games;
    private String wife;
    private Properties info;
}
```

3.beans.xml

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/util https://www.springframework.org/schema/util/spring-util.xsd">

    <bean id="student" class="com.kuang.pojo.Student">
        <!--第一种，普通注入value-->
        <property name="name" value="亲将"/>
    </bean>

</beans>
```

4.测试类

```java
public class MyText {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        Student student = (Student) context.getBean("student");
        System.out.println(student.getName());
    }
}
```

5.完善注入信息

```java
<bean id="address" class="com.kuang.pojo.Address"/>

    <bean id="student" class="com.kuang.pojo.Student">
        <!--第一种，普通注入value-->
        <property name="name" value="亲将"/>
        <!--第二种，bean注入，ref-->
        <property name="address" ref="address"/>
        <!--数组注入，ref-->
        <property name="books">
            <array>
                <value>红楼梦</value>
                <value>水浒传</value>
                <value>三国演义</value>
            </array>
        </property>
        <!--List-->
        <property name="hobbys">
            <list>
                <value>听歌</value>
                <value>6</value>
                <value>5</value>
            </list>
        </property>

        <!--map-->
        <property name="card">
            <map>
                <entry key="身份证" value="121312313"/>
                <entry key="银行卡" value="4654564564"/>
            </map>
        </property>

        <!--Set-->
        <property name="games">
            <set>
                <value>lol</value>
                <value>csgo</value>
                <value>dota2</value>
            </set>
        </property>

        <!--null-->
        <property name="wife">
            <null/>
        </property>

        <!--properties-->
        <property name="info">
            <props>
                <prop key="x">111</prop>
            </props>
        </property>
    </bean>
```

## 4.其他方式注入

我们可以使用p命名空间和c命名空间进行注入

官方解释

![image-20210602153519058](C:\Users\云\AppData\Roaming\Typora\typora-user-images\image-20210602153519058.png)

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- p命名空间注入，可以直接注入属性的值，property -->
    <bean id="user" class="com.kuang.pojo.User" p:name="qq" p:age="18"/>
    <!-- c命名空间注入，通过构造器注入，construct-args -->
    <bean id="user" class="com.kuang.pojo.User" c:age="18" c:name="wangc"/>


</beans>
```

测试

```java
    @Test
    public void test2(){
        ApplicationContext context =  new ClassPathXmlApplicationContext("userbeans.xml");
        User user = context.getBean("user",User.class);
        System.out.println(user);
    }
}

```

注意点：p命名和c命名空间不能直接使用，直接导入xml约束!

```java
    <!-- p命名空间注入，可以直接注入属性的值，property -->
    <bean id="user" class="com.kuang.pojo.User" p:name="qq" p:age="18"/>
    <!-- c命名空间注入，通过构造器注入，construct-args -->
    <bean id="user" class="com.kuang.pojo.User" c:age="18" c:name="wangc"/>
```

## 5.bean的作用域

![image-20210603083901275](C:\Users\云\AppData\Roaming\Typora\typora-user-images\image-20210603083901275.png)

| Class                    | [Instantiating Beans](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-class)（实例化bean） |
| ------------------------ | ------------------------------------------------------------ |
| Name                     | [Naming Beans（命名bean）](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-beanname)（命名bean） |
| Scope                    | [Bean Scopes](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-scopes)（bean作用域） |
| Constructor arguments    | [Dependency Injection](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-collaborators)（依赖注入） |
| Properties               | [Dependency Injection](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-collaborators)（依赖注入） |
| Autowiring mode          | [Autowiring Collaborators](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-autowire)（自动装配合作者） |
| Lazy initialization mode | [Lazy-initialized Beans](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-lazy-init)（延迟初始化的bean） |
| Initialization method    | [Initialization Callbacks](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-lifecycle-initializingbean)（初始化回调） |
| Destruction method       | [Destruction Callbacks](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-lifecycle-disposablebean)（销毁回调） |

**1.单例模式（Spring默认机制）**

```java
<bean id="accountService" class="com.something.DefaultAccountService"/>

<!-- the following is equivalent, though redundant (singleton scope is the default) -->
<bean id="accountService" class="com.something.DefaultAccountService" scope="singleton"/>
```

2.原型模式：每次从容其中get的时候，都会产生一个新对象

```java
<bean id="accountService" class="com.something.DefaultAccountService" scope="prototype"/>
```

3.其余的request、session、application、这些只能在web开发中使用

# 8、Bean的自动装配

​	自动装配是Spring满足bean依赖的一种方式

​	Spring会在上下文中自动寻找，并自动给bean装配属性

​	在Spring中有三种装配的方式

​	1.在xml中显示配置

​	2.在java显示配置

## 1.测试

1.环境搭建

​	一个人有两个宠物

## 2.ByName自动装配

```java
<!--
    byName:会自动在容器上下文中查找，和自己对象set方法后面的值对应的beanid
    -->
    <bean id="name" class="com.kk.Perople" autowire="byName">
        <property name="name" value="xxx"/>
     </bean>
```

## 3.byTape自动装配

```java
<bean id="cat" class="com.kk.Cat"/>
    <bean id="Dog" class="com.kk.Dog"/>

    <!--
    byName:会自动在容器上下文中查找，和自己对象set方法后面的值对应的beanid
    byTape:会自动在容器上下文中查找，和自己对象属性类型相同的bean
    -->
    <bean id="name" class="com.kk.Perople" autowire="byType">
        <property name="name" value="xxx"/>
     </bean>
```

byName的小结：

​	byName的时候，需要保证所有bean的id唯一，并且这个bean需要和自动注入的属性的set方法的值一致

​	byTape的时候，需要保证所有bean的id唯一，并且这个bean需要和自动注入的属性的类型一致

## 4.使用注解实现自动装配

jdk1.5支持的注解，Spring2.5就支持注解了

The introduction of annotation-based configuration raised the question of whether this approach is “better” than XML.

要使用注解须知:

​	1.导入约束：context约束

​	2.配置注解的支持：context:annotation-config

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

</beans>
```

@Autowired

直接在属性上使用即可，也可以在set方式上使用

使用Autowired我们可以不用编写Set方法，前提是你这个自动装配的属性在IOC（Spring）容器中存在，且符合名字byname

科普：

```java
@nullable	字段标记了这个注解，说明这个字段可以为null，
```

```java
public @interface Autowired{
    boolean required() default true;
}
```

```java
//测试代码
public class Perople {
    @Autowired(required = false)
    private Cat cat;
    @Autowired
    private Dog dog;
    private String name;
}
```

 如果@Autowired自动装配的环境比较复杂，自动装配无法通过一个注解【@Autowiresd】完成的时候，我们可以使用@Qualifier（value="xxx"）去配置@Autowired的使用，指定一个唯一的bean对象注入

```java
//测试代码
public class Perople {
    @Autowired
    @Qualifier(value="cat111")
    @Autowired(required = false)
    private Cat cat;
    @Autowired
    @Qualifier(value="dog222")
    private Dog dog;
    private String name;
}
```

@resource注解

```java
public class People{
    
    @Resource(name="cat2")
    private Cat cat;
    
    @Resource
    private Dog dog;
}
```

小结:

@Resource和@Autowired的区别：

​	都是用来自动装配的，都可以放在属性字段上

​	@Autowired通过bytape的方式实现，而且必须要求对象存在

​	@Resource默认通过byname的方式实现，如果找不到名字则通过bytape实现。如果两个都找不到的情况下，就报错

​	执行顺序不同：@Autowired通过bytape的方式实现。

5.嵌套类名

如果腰围嵌套类配置bean 定义

# 9、使用注解开发

在Spring 4之后，要使用注解开发，必须要保证 aop 的包导入了。

使用注解需要导入context约束，增加注解的支持

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          http://www.springframework.org/schema/context/spring-context.xsd">
    <context:annotation-config/>

</beans>
```



## 1.bean



## 2.属性如何注入

@Component：组件，放在类上，说明这个类被Spring管理了，就是Bean。1

```java
//等价于<bean id="user" class = "com.kuang.dao.User"/>
//@Component 组件
@Component
public class User {
    //相当于<property name = "name" value = "kuangshen"/>
    @Value("kuangshen")
    public String name;

}
```

## 3.衍生的注解

@Component有几个衍生注解，我们在web开发中，会按照mvc三层架构封分层

​	dao【@Repository】

​	service【@Service】

​	controller【@Controller】

​	这四个注解功能都是一样的，都是代表将某个类注册到Spring中， 装备Bean

## 4.自动装配置

@Autowired：自动装配通过类型，名字

​	如果Autowired不能唯一自动装配上属性，则需要通过@Qualifier（value="xxx"）

@Nullable	字段标记了这个注解，说明这个字段可以为null

@Resource：自动装配通过名字，类型

## 5.作用域

```java
@Component
@Scope("prototype")
public class User {
    //相当于<property name = "name" value = "kuangshen"/>
    @Value("kuangshen")
    public String name;

}
```



## 6.小结

xml与注解：

​	xml更加万能，适用于任何场合，维护简单方便

​	注解不是自己类使用不了，维护相对复杂

xml与注解最佳实践：

​	xml用来管理bean

​	注解只负责完成属性的注入

​	我们在使用的过程中，只需要注意一个问题：必须让注解生效，就需要开始注解的支持

```java
<!-- 指定要扫描的包，这个包下的注解就会生效 -->
<context:component-scan base-package="com.kuang"/>
<context:annotation-config/>
```

# 10.使用Java的方式配置Spring

我们现在要完全不适用Spring的xml配置了，全权交给Java来做

JavaConfig是Spring的一个子项目，在Spring 4之后，他成为了一个核心功能

注意：1.如果开启包扫描，加载配置类以后就可以通过反射拿到配置类中的对象了

​			2.@Bean只写在方法上，返回的是一个对象，但一般不获取已经在容器中的对象

​			3.@Bean可以用于通过方法获取数据库连接池Connection这种对象

User类上使用@Component注解，并在配置类上生命@ComponentScan("User类的路径")，这样会自动扫描@Component

方法1：在配置类中定义一个方法，并使用@Bean注解声明

使用@configuration声明配置类时有两种方法生成Bean



实体类

```java
//这里这个注解的意思，就是说明这个类被Spring接管了，注册到了容器中
//@Component
public class User {
    private String name;

    public void setName(String name) {
        this.name = name;
    }

    @Value("qin jiang")//注入值
    public String getName() {
        return name;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }
}

```

配置文件

```java
package com.kuang.config;

import com.kuang.pojo.User;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

@Configuration//这个也会Spring容器托管，注册到容器中。因为他本来就是一个@component，
// @Configuration代表这是一个配置类，就和我们之前看的beans.xml
@ComponentScan("com.kuang.pojo")
@Import(Kuangconfig2.class)
public class Kuangconfig {

    //注册一个Bean，就相当于我们之前写的一个bean标签
    //这个方法的名字，就相当于bean标签中的id属性
    //这个方法的返回值，就相当于bean标签中的class属性
    @Bean
    public User getUser(){
        return new User();//就是返回要注入到bean的对象
    }

}
```

测试类

```java
import com.kuang.config.Kuangconfig;
import com.kuang.pojo.User;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class MyText {
    public static void main(String[] args) {
        //如果完全使用了配置类方式去做，我们就只能通过AnnotationConfig上下文来获取容器，通过配置类的class对象加载
        //都用也不会报错，只是对象会实例化两次，但是是同一个对象，不是两个
        //两种都使用的话getBean("User")和getBean(getUser)获得的对象是同一个对象，打印了hashcode是一样的
        //使用@Bean的话，Bean的id是注入的方法名，可以在使用@Bean(name="")来设置id
        //两种方式的目的都是一样的，都是实现bean的注册，一般使用应该区别不大，但是还是有不少区别的
        ApplicationContext context = new AnnotationConfigApplicationContext(Kuangconfig.class);

        User getUser = (User) context.getBean("getUser");
        System.out.println(getUser.getName());

    }
```

这种纯Java的配置方式，在SpringBoot中随处可见

# 11、代理模式

学习代理模式--这是SpringAOP的底层【SpringAOP、SPringMVC】

代理模式分类：

​	静态代理

​	动态代理

![image-20210609102824703](C:\Users\云\AppData\Roaming\Typora\typora-user-images\image-20210609102824703.png)

## 1.静态代理

角色分析：

​	抽象角色：一般会使用接口或者抽象类来解决

​	真实角色：被代理的角色

​	代理角色：代理真实角色，代理真实角色后，我们一般会做一些附属操作

​	客户：访问代理对象的人



代码步骤

1.接口

```java
public interface Rent {
    public void rent();
}
```

2.真实角色

```java
public class Host implements Rent{
    @Override
    public void rent() {
        System.out.println("f d c z f z");
    }
}
```

3.代理角色

```java
public class Prosy implements Rent{
    private Host host;

    public Prosy() {
    }

    public Prosy(Host host) {
        this.host = host;
    }

    @Override
    public void rent() {
        seeHouse();
        host.rent();
        fare();
    }
    //看房
    public void seeHouse(){
        System.out.println("z j d n k f");

    }

    public void fare(){
        System.out.println(" s q");

    }

}
```

4.客户端访问代理角色

```java
public class Client {
    public static void main(String[] args) {
        //房东要租房子
        Host host = new Host();
        //代理，中介带房东租房子
        Prosy prosy = new Prosy(host);
        prosy.rent();
    }
}
```



代理模式的好处：

​	可以使真实角色的操作更加纯粹，不用去关于一些公共的业务

​	公共也就交给代理角色，实现了业务的分工

​	公共业务发生扩展的时候，方便集中管理

缺点：

​	一个真实角色就会产生代理角色，代码量会翻倍，开发效率会变低

## 2.加深理解

代码：对应spring-08-demo02

AOP

![image-20210609110322734](C:\Users\云\AppData\Roaming\Typora\typora-user-images\image-20210609110322734.png)

## 3.动态代理

动态代理和静态代理角色一样

动态代理类时动态生成的，不是我们直接写好的

动态代理分为两大类，基于接口的动态代理 ，基于类的动态代理

​	基于接口-JDK动态代理

​	基于类：cglib

​	java字节码实现：javassist

需要了解两个类：Proxy 代理，InvocationHandler：调用处理程序



InvocationHandler是由代理实例的调用处理程序实现的接口

动态代理的好处

​	可以使真实角色的操作更加纯粹，不用去关于一些公共的业务

​	公共也就交给代理角色，实现了业务的分工

​	公共业务发生扩展的时候，方便集中管理

​	一个动态代理类代理的是一个接口，一般对应的就是一个业务

​	一个动态代理类可以代理多个类，只要是实现了同一个接口即可

# 12、AOP

## 1.什么是AOP

AOP（Aspect Oriented Programming）意为：面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生规范，利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。

![image-20210609180932328](C:\Users\云\AppData\Roaming\Typora\typora-user-images\image-20210609180932328.png)

## 2.AOP在Spring中的作用

提供声明式事务；允许用户自定义切面

横切关注点：跨越应用程序多个模块的方法或功能。即是，与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点。如日志，安全，缓存，事务等等

切面（ASPECT）：横切关注点 被模块化 的特殊对象。即，它是一个类

通知（Advice）：切面必须要完成的工作。即，它是类中的一个方法

目标（Target）：被通知对象

代理（Proxy）：向目标对象应用通知之后创建的对象

切入点（PointCut）：切面通知执行的“地点”的定义

连接点（JointPoint）：与切入点相匹配的执行点

![image-20210609181326363](C:\Users\云\AppData\Roaming\Typora\typora-user-images\image-20210609181326363.png)

在SpringAOP中，通过Advice定义横切逻辑，Spring中支持5种类型的Advice

![image-20210609181543860](C:\Users\云\AppData\Roaming\Typora\typora-user-images\image-20210609181543860.png)

即AOP在不改变原有代码的情况下，去增加新的功能，

## 3.使用SpringAop

【重点】使用AOP织入，需要导入一个依赖包

```java
<dependencies>
	<dependency>
    	<groupId>aspectj</groupId>
    	<artifactId>aspectjweaver</artifactId>
    	<version>1.5.4</version>
	</dependency>
</dependencies>
```



方式一：使用Spring的API接口【主要是SpringAPI接口实现】

方式二：自定义类来实现AOP【主要是切面定义】

方式三：使用注解实现

代码见解<spring-09>



在使用spring框架配置AOP的时候，不管是通过哦xml配置文件还是注解的方式都需要定义pointcut切入点

例如切入点表达式execution(* com.sample.service.impl.*. *(..))

execution()是最常用的切点函数，其语法如下所示：

1、execution(): 表达式主体。

2、第一个*号：表示返回类型， *号表示所有的类型。

3、包名：表示需要拦截的包名，后面的两个句点表示当前包和当前包的所有子包，com.sample.service.impl包、子孙包下所有类的方法。

4、第二个*号：表示类名， *号表示所有的类。

5、*(..):最后这个星号表示方法名， *号表示所有的方法，后面括弧里面表示方法的参数，两个句点表示任何参数

# 13、整合Mybatis

步骤：

​	1.导入相关jar包

​		junit

​		mybatis

​		mysql数据库

​		spring相关的

​		aop织入

​		mybatis-spring【new】

​	2.编写配置文件

​	3.测试

```java
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="pojo">
    <select id="listStudent" resultType="Student">
        select * from  student
    </select>
</mapper>
```

#### 第一步：准备数据库

首先我们创建一个数据库【mybatis】，编码方式设置为 UTF-8，然后再创建一个名为【student】的表，插入几行数据：

```java
DROP DATABASE IF EXISTS mybatis;
CREATE DATABASE mybatis DEFAULT CHARACTER SET utf8;

use mybatis;
CREATE TABLE student(
  id int(11) NOT NULL AUTO_INCREMENT,
  studentID int(11) NOT NULL UNIQUE,
  name varchar(255) NOT NULL,
  PRIMARY KEY (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

INSERT INTO student VALUES(1,1,'我没有三颗心脏');
INSERT INTO student VALUES(2,2,'我没有三颗心脏');
INSERT INTO student VALUES(3,3,'我没有三颗心脏');
```

#### 第二步：创建工程

在 IDEA 中新建一个 Java 工程，并命名为【HelloMybatis】，然后导入必要的 jar 包：

- mybatis-3.4.6.jar
- mysql-connector-java-5.1.21-bin.jar

#### 第三步：创建实体类

在 Package【pojo】下新建实体类【Student】，用于映射表 student：



```java
package pojo;

public class Student {

    int id;
    int studentID;
    String name;

    /* getter and setter */
}
```

#### 第四步：配置文件 mybatis-config.xml

在【src】目录下创建 MyBaits 的主配置文件 `mybatis-config.xml` ，其主要作用是提供连接数据库用的驱动，数据名称，编码方式，账号密码等，我们在后面说明：



```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!-- 别名 -->
    <typeAliases>
        <package name="pojo"/>
    </typeAliases>
    <!-- 数据库环境 -->
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
    <!-- 映射文件 -->
    <mappers>
        <mapper resource="pojo/Student.xml"/>
    </mappers>

</configuration>
```

#### 第五步：配置文件 Student.xml

在 Package【pojo】下新建一个【Student.xml】文件：



```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="pojo">
    <select id="listStudent" resultType="Student">
        select * from  student
    </select>
</mapper>
```

- 由于上面配置了 `<typeAliases>` 别名，所以在这里的 `resultType` 可以直接写 Student，而不用写类的全限定名 pojo.Student
- `namespace` 属性其实就是对 SQL 进行分类管理，实现不同业务的 SQL 隔离
- SQL 语句的增删改查对应的标签有：

#### 第六步：编写测试类

在 Package【test】小创建测试类【TestMyBatis】：



```java
package test;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import pojo.Student;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

public class TestMyBatis {

    public static void main(String[] args) throws IOException {
        // 根据 mybatis-config.xml 配置的信息得到 sqlSessionFactory
        String resource = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        // 然后根据 sqlSessionFactory 得到 session
        SqlSession session = sqlSessionFactory.openSession();
        // 最后通过 session 的 selectList() 方法调用 sql 语句 listStudent
        List<Student> listStudent = session.selectList("listStudent");
        for (Student student : listStudent) {
            System.out.println("ID:" + student.getId() + ",NAME:" + student.getName());
        }

    }
}
```

