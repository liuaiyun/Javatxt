# JDBC

## 一、JDBC入门

客户端操作MySQL数据库的方式：

1.使用第三方客户端来访问。

2.使用MySQL自带的命令行方式。

3.通过Java来访问MySQL数据库（本篇重点）

### 1.什么是JDBC？（了解即可）

Java和数据库之间的桥梁，

是一个规范而不是实现，能执行SQL语句。

JDBC规范定义接口。是Java访问数据库的标准规范。

### 2.使用JDBC的好处

1.若要开发访问数据库的程序，只需会调用JDBC接口中的方法即可。

2.使用同一套Java代码，进行少量修改就能访问其他JDBC支持的数据库。

### 3.使用JDBC开始使用到的包

1.java.sql				所有与JDBC访问数据库相关的接口和类。

2.javax.sql			数据库扩展包，提供数据库额外的功能。如：连接池。

3.数据库的驱动		由各大数据库厂商提供，需要额外去下载，是对JDBC接口实现的类。

4.JDBC的核心API

接口或类		作用

1.DriverManager类					管理和注册数据库驱动；得到数据库连接对象。

2.Connection接口						一个连接对象，可用于创建Statement和PreparedStatement对象。

3.Statement接口							一个SQL语句对象，用于将SQL语句发送给数据库服务器。

4.PrepareedStatement接口		一个SQL语句对象，是Statement的子接口。

5.ResultSet接口								用于表示一个数据库查询的结果集。

### 4.导入驱动jar包（自总）

下载地址：https://dev.mysql.com/downloads/connector/j/

Select Operating System：Platform Independent

选择后缀为 .ZIP下载压缩到自己设定好的地址

包中会出现mysql-connector-java-8.0.24.jar

下面以IDEA为例：

在项目上右键--new--Directory--复制mysql-connector-java-8.0.24.jar该压缩包--直接在新创建的Directory上粘贴--右键粘贴的jar--Add as Library--完成！

### 5.加载和注册驱动

方法：Class.forName（数据库驱动实现类）

注：从JDBC3开始，可以不用注册驱动直接使用。Class.forName可以省略。

## 二、DriverManager类

### 1.DriverManager作用：

1.管理和注册驱动。

2.创建数据库的连接。

### 2.类中的方法（简单了解）

Drivermanager类中的静态方法：

1.Drivermanager.getConnection（Sring url，String user，String password）

描述：通过连接字符串，用户名，密码来得到数据库的连接对象。

2.Drivermanager.getConnection（String url，Properties info）

描述：通过连接字符串，属性对象来得到连接对象。

### 3.使用JDBC连接数据库的四个参数：

四个参数：

1.用户名	2.密码

3.连接字符串url	（不容的数据库URL是不同的）

4.驱动类的字符串名	（import.mysql.jdbc.Driver）

### 4.连接数据库的URL地址格式

协议名:子协议://服务器名或IP地址:端口号/数据库名?参数=参数值

#### 1.MySQL写法

URL用于标识数据库的位置，程序员通过URL地址告诉JDBC程序连接哪个数据库，URL写法为;

jdbc:mysql://localhost:3306/test?参数名=参数值（与上面相对应）

#### 2.MySQL中可以简写

前提：必须是本地服务器，端口号是3306

jdbc:mysql:///jdbcDatabase（数据库名）

#### 3.乱码的处理

若数据库中出现乱码，可以指定参数：?characterEncoding=utf8，表示让数据库以UTF-8编码来处理数据。

语法:

jdbc:mysql://Localhost:3306/数据库?characterEncoding=utf8

### 5.案例：得到MySQL的数据库连接对象

#### 1.使用用户名/密码/URL得到连接对象(内容需理解，可复制)

package (自己建的包);
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
/**

得到连接对象
*/
public class Demo2 {
public static void main(String[] args) throws SQLException {
String url = "jdbc:mysql://localhost:3306/day24";
//1) 使用用户名、密码、URL 得到连接对象
Connection connection = DriverManager.getConnection(url, "root", "root");
//com.mysql.jdbc.JDBC4Connection@68de145
System.out.println(connection);
  }
}

#### 2.使用属性文件和url得到连接对象

```java
package lqg;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.Properties;
public class Demo3 {
    public static void main(String[] args) throws SQLException {
//url 连接字符串
        String url = "jdbc:mysql://localhost:3306/day24";
//属性对象
        Properties info = new Properties();
//把用户名和密码放在 info 对象中
        5 / 21info.setProperty("user","root");
        info.setProperty("password","root");
        Connection connection = DriverManager.getConnection(url, info);
//com.mysql.jdbc.JDBC4Connection@68de145
        System.out.println(connection);
    }
}
```

## 三、Connection接口

### 1.Connection作用

代表一个连接对象。

### 2.Connection方法

Statement createStatement() ---------->>创建一条SQL语句对象

## 四、Statement接口

### 1.JDBC访问数据库的步骤

1.注册和加载驱动（可省略）

2.获取连接

3.Connection获取Statement对象

4.使用Statement对象执行SQL语句

5.返回结果集

6.释放资源

### 2.Statement作用

代表一条语句对象，用于发送SQL语句给服务器，用于执行静态SQL语句并返回它所生成结果的对象。

### 3.Statement中的方法

1.int executeUpdate（String sql）--->用于发送DML语句，增删改的操作，insert、update、delete；

​	参数：SQL语句

​	返回值：返回对数据库影响的行数

2.ResultSet executeQuery（String sql）--->>用于发送DQL语句，执行查询的操作（select）

​	参数：SQL语句

​	返回值：查询的结果集

### 4.释放资源

1.需要释放的对象：Result结果集，Statement语句，Connection连接

2.释放原则：先开后关，后开先关。

3.放在哪个代码块？：finally块

### 5.执行DDL操作

1.需求：使用JDBC在MySQL的数据库中创建一张学生表

```java
package lqg;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;
/*
创建一张学生表  数据库
 */
public class IDEA2 {
    public static void main(String[] args) {
//        创建连接
        Connection conn = null;
        Statement statement = null;
        try {
            conn = DriverManager.getConnection("jdbc:mysql:///mysql", "root", "root");
//通过连接对象得到语句对象
            statement = conn.createStatement();
//            通过语句对象发送SQL语句给服务器
//            执行SQL
            statement.executeUpdate("create table student (id int PRIMARY key auto_increment," + "name varchar(20) not null,gender boolean,birthday date)");
//            返回影响行数（DDL 没有返回值）
            System.out.println("创建表成功");
        }catch(SQLException e){
            e.printStackTrace();
        }
//        释放资源
        finally{
//            关闭之前要先判断
            if(statement != null){
                try{
                    statement.close();
                }catch(SQLException e){
                    e.printStackTrace();
                }
            }
            if(conn != null){
                try{
                    conn.close();
                }catch (SQLException e){
                    e.printStackTrace();
                }
            }
        }
    }
}
```

### 6.执行DML操作

需求：向学生表中添加四条记录，主键是自动增长

步骤：

1.创建链接对象

2.创建Statement语句对象

3.执行SQL语句：executeUpdate(sql)

4.返回影响的行数

5.释放资源

```java
package lqg;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;
/*
想学生表中添加4条记录，主键是自动增长
 */

public class IDEA3 {
    public static void main(String[] args) throws SQLException {
//        创建连接对象
        Connection connection = DriverManager.getConnection("jdbc:mysql:///mysql", "root", "root");
//        创建Statement语句对象
        Statement statement = connection.createStatement();
//        执行SQL语句：executeUpdate(sql)
        int count = 0;
//        返回影响的行数
        count += statement.executeUpdate("insert into student values(null, '孙悟空', 1, '1993-03-24')");
        count += statement.executeUpdate("insert into student values(null, '白骨精', 0, '1995-03-24')");
        count += statement.executeUpdate("insert into student values(null, '猪八戒', 1, '1903-03-8 / 2124')");
        count += statement.executeUpdate("insert into student values(null, '嫦娥', 0, '1993-03-11')");
        System.out.println("插入了"+count+"条记录");
//        释放资源
        statement.close();
        connection.close();
    }
}
```

### 7.执行DQL操作

#### 1.ResultSet接口

作用：封装数据库查询的结果集，对结果结进行遍历，取出每一条记录。

**接口中的方法：**

1.boolean next()	-->游标向下移动一行；返回Boolean类型，若还有下一条记录，返回true，否则返回false

2.数据类型getXxx()	-->通过字段名，参数是String类型，返回不同类型；通过列号，参数是整数，从1开始，返回不同类型。

![2](https://img-blog.csdnimg.cn/20190709143916439.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNjc0MTMy,size_16,color_FFFFFF,t_70)

#### 2.常用数据类型转换

SQL 					 						JDBC												返回类型

BIT(1)bit(n)								getByte()											boolean	

TINYINT										getByte()										byte

SMALLINT								getShort()											short

INT												getInt()												int

BIGINT										getLong()											long

CHAR,VARCHAR						getString()											Sting

Text(Clob)Blob							GetClob getBlob()							Clob Blob

DATE											getDate()											java.sql/Date	只代表日期

TIME											getTime()												java.sql.Time	只代表时间

TIMESTAMP								getTimestamp()									java.sql.Timestamp	同时有日期和时间

java.sql.Date/Time/Timestamp(时间戳)		共同父类--java.util.Date

#### 3.需求：确保数据库中有3条以上的记录，查询所有的学员信息

步骤：

1.得到连接对象

2.得到语句对象

3.执行SQL语句得到结果集ResultSet对象

4.循环遍历取出每一条记录

5.输出的控制台上

6.释放资源

```java
package lqg;
import javax.swing.plaf.nimbus.State;
import java.sql.*;


public class IDEA4 {
    public static void main(String[] args) throws SQLException{
//        得到连接对象
        Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/sql","root","root");
//        得到语句对象
        Statement statement = connection.createStatement();
//        执行SQL语句得到结果集Result对象
        ResultSet rs = statement.executeQuery("select * from student");
//        循环遍历取出每一条记录
        while(rs.next()){
            int id = rs.getInt("id");
            String name = rs.getString("name");
            boolean gender = rs.getBoolean("gender");
            Date birthday = rs.getDate("birthday");
//            输出的控制台上
            System.out.println("编号：" + id + "，姓名" + name + "，性别" + gender + "，生日" + birthday);
        }
//        释放资源
        rs.close();
        statement.close();
        connection.close();
    }
}
```

#### 4.关于ResultSet接口中的注意事项：

1.如果光标在第一行之前，使用rs.getXX()获取列值，报错：Before start of result set

2.如果光标在最后一行之后，使用rs.getXX()获取列值，报错：After end of result set

3.如果完毕以后要关闭结果集ResultSet，再关闭Statement，再关闭Connection

## 五、数据库工具类JDBC Utils

创建工具类---> 一个功能经常用到，建议把这个功能做成一个工具类，可以在不同的地方重用。

### 1.需求

#### 1.需求：

1.有一张用户表

2.添加几条用户记录

```java
create table user(
                     id int primary key auto_increment,
                     name varchar(20),
                     password varchar(20)
);
insert into user values(null,'jack','123'),(null,'rose','456');
# 登录，SQL中大小写不敏感
select * from user where name = 'JACK' and password = '123';
# 登陆失败
select * from user where name = 'JACK' and password = '333';
```

3.使用Statement字符串拼接的方式实现用户的登录，用户在控制台上输入用户名和密码

#### 2.步骤

1.得到用户从控制台上输入的用户名和密码来查询数据库

2.写一个登录的方法

​	①通过工具类得到连接

​	②创建语句对象，使用拼接字符

​	③查询数据库，如果有记录则表示登录成功，否则失败

​	④释放资源

3.SQL注入问题

​	当我们输入密码时，会发现我们账号和密码不对但登陆成功

​	问题分析:

​		我们让用户输入的密码和SQL语句进行字符串拼接，用户输入的内容作为了SQL语句语法的一部分，改变了SQL真正的意义，（SQL注入问题），要解决则不能进行简单的字符串拼接。

## 六、PreparedStatement

### 1.继承结构与作用

PreparedStatement时Statement接口的子接口，继承于父接口中所有的方法。它是一个预编译的SQL语句。

### 2.PreparedStatement的执行原理

Statement对象每执行一条SQL语句都会先将SQL语句发送给数据库，数据库先编译SQL，在执行。

PrepareStatement会先将SQL语句发送给数据库预编译。

Presparedtatement会引用预编译后的结果。

可以多次传入不同的参数给PreparedStatement对象并执行。

​	**1.因为有预先编译的功能，提高SQL的执行效率。**

​	**2.可以有效的防止SQL注入的问题，安全性更高。**

### 3.Connection创建PreparedStatement对象

Connection接口中的方法：

PreparedStatement								指定预编译的SQL语句，SQL语句中使用占位符

PrepareStatement（String sql）			创建一个语句对象

### 4.PreparedStatement接口中的方法

int executeUpdate()				执行DML，增删改的操作，返回影响的行数。

ResultSet executeQuery()		执行DQL，查询的操作，返回结果集。

### 5.PreparedStatement的好处

1.PrepareStatement会先将SQL语句发送给数据库预编译；Presparedtatement会引用预编译后的结果。

可以多次传入不同的参数给PreparedStatement对象并执行。

2.安全性更高，没有SQL注入的隐患。

3.提高了程序的可读性。

### 6.使用PreparedStatement的步骤

1.编写SQL语句，位置内容使用?占位	“SELECT * FROM USER WHERE name=? AND password=?”；

2.获得PreparedStatement对象

3.设置实际参数：setXxx（占位符的位置，真实的值）

4.执行参数化SQL语句

5.关闭资源

#### Prepared Statement设置参数的方法

void setDouble(int parameterIndex,double x)			将指定参数设置为给定Java double值

void setFloat(int parameterIndex,flaot x)					将指定参数设置为给定Java REAL值

void setInt(int parameterIndex,int x)							将指定参数设置为给定Java int值

void setLong(int parameterIndex,long x)					将指定参数设置为给定Java long值

void setObject(int parameterIndex,Object x)				使用给定对象设置指定参数的值

void setString(int parameterIndex,String x)					将指定参数设置为给定Java String值

**注：使用PreparedStatement改写上面的登陆程序，看有没有SQL注入的情况**

```java
package lqg;
import utils.JdbcUtils;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.Scanner;
/**
 * 使用 PreparedStatement
 */
public class Demo8Login {
    //从控制台上输入的用户名和密码
    public static void main(String[] args) throws SQLException {
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入用户名");
        String name = sc.nextLine();
        System.out.println("请输入密码：");
        String password = sc.nextLine();
        login(name, password);
    }
    /**
     * 登录的方法
     * @param name
    16 / 21
     * @param password
     */
    private static void login(String name, String password) throws SQLException {
        Connection connection = JdbcUtils.getConnection();
        //写成登录 SQL 语句，没有单引号
        String sql = "select * from user where name=? and password=?";
        //得到语句对象
        PreparedStatement ps = connection.prepareStatement(sql);
        //设置参数
        ps.setString(1, name);
        ps.setString(2,password);
        ResultSet resultSet = ps.executeQuery();
        if (resultSet.next()) {
            System.out.println("登录成功：" + name);
        }
        else {
            System.out.println("登录失败");
        }
        //释放资源,子接口直接给父接口
        JdbcUtils.close(connection,ps,resultSet);
    }
}
```

### 7.表与类的关系

整个表可以看作为一个类

表的一行称为一条记录，类的实例

表中一列代表具体类实例的数据

#### 1.例：使用PreparedStatement查询一条数据，封装成一个学生Student对象

```java
package lqg;
import lqg.entity.Student;
import lqg.utils.JdbcUtils;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
public class Demo9Student {
    public static void main(String[] args) throws SQLException {
        //创建学生对象
        Student student = new Student();
        17 / 21
        Connection connection = JdbcUtils.getConnection();
        PreparedStatement ps = connection.prepareStatement("select * from student where id=?");
        //设置参数
        ps.setInt(1,2);
        ResultSet resultSet = ps.executeQuery();
        if (resultSet.next()) {
            //封装成一个学生对象
            student.setId(resultSet.getInt("id"));
            student.setName(resultSet.getString("name"));
            student.setGender(resultSet.getBoolean("gender"));
            student.setBirthday(resultSet.getDate("birthday"));
        }
        //释放资源
        JdbcUtils.close(connection,ps,resultSet);
        //可以数据
        System.out.println(student);
    }
}
```

#### 2.例：将多条记录封装成集合List<Student>，集合中每个元素是一个JavaBean实体类

```java
package lqg;
import lqg.entity.Student;
import lqg.utils.JdbcUtils;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;
public class Demo10List {
    public static void main(String[] args) throws SQLException {
        //创建一个集合
        List<Student> students = new ArrayList<>();
        Connection connection = JdbcUtils.getConnection();
        PreparedStatement ps = connection.prepareStatement("select * from student");
        //没有参数替换
        ResultSet resultSet = ps.executeQuery();
        while(resultSet.next()) {
            //每次循环是一个学生对象
            Student student = new Student();
            //封装成一个学生对象
            student.setId(resultSet.getInt("id"));
            student.setName(resultSet.getString("name"));
            student.setGender(resultSet.getBoolean("gender"));
            student.setBirthday(resultSet.getDate("birthday"));
            //把数据放到集合中
            students.add(student);
        }
        //关闭连接
        JdbcUtils.close(connection,ps,resultSet);
        //使用数据
        for (Student stu: students) {
            System.out.println(stu);
        }
    }
}
```

### 8.PreparedStatement执行DML操作

```java
package lqg;
import lqg.utils.JdbcUtils;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;
public class Demo11DML {
    public static void main(String[] args) throws SQLException {
        //insert();
        //update();
        delete();
    }
    //插入记录
    private static void insert() throws SQLException {
        Connection connection = JdbcUtils.getConnection();
        PreparedStatement ps = connection.prepareStatement("insert into student
                values(null,?,?,?)");
        ps.setString(1,"小白龙");
        ps.setBoolean(2, true);
        ps.setDate(3,java.sql.Date.valueOf("1999-11-11"));
        int row = ps.executeUpdate();
        System.out.println("插入了" + row + "条记录");
        JdbcUtils.close(connection,ps);
    }
    //更新记录: 换名字和生日
    private static void update() throws SQLException {
        Connection connection = JdbcUtils.getConnection();
        PreparedStatement ps = connection.prepareStatement("update student set name=?,         birthday=?
                where id=?");
        ps.setString(1,"黑熊怪");
        ps.setDate(2,java.sql.Date.valueOf("1999-03-23"));
        ps.setInt(3,5);
        int row = ps.executeUpdate();
        System.out.println("更新" + row + "条记录");
        JdbcUtils.close(connection,ps);
    }
    19 / 21
    //删除记录: 删除第 5 条记录
    private static void delete() throws SQLException {
        Connection connection = JdbcUtils.getConnection();
        PreparedStatement ps = connection.prepareStatement("delete from student where id=?");
        ps.setInt(1,5);
        int row = ps.executeUpdate();
        System.out.println("删除了" + row + "条记录");
        JdbcUtils.close(connection,ps);
    }
}
```

## 七、JDBC的事务处理

### 1.准备数据

```java
CREATE TABLE account(
    id INT PRIMARY KEY AUTO_INCREMENT,
    NAME VARCHAR(10),
    balance DOUBLE
);
# 添加数据
INSERT INTO account(NAME,balance) VALUES ('JACK',1000),('ROSE',1000);
```

### 2.API介绍

Connection接口中与事务有关的方法

void setAutoCommit(boolean autoCommit)				参数是true或false

​									如果设置为false，表示关闭自动提交，相当于开启事务

void commit()									提交事务

void rollback()									回滚事务

### 3.开发步骤

1.获取连接

2.开启事务

3.获取到PrepareStatement

4.使用PrepareStatement

5.正常情况下提交事务

6.出现异常回滚事务

7.最后关闭资源

```java
package lqg;
import lqg.utils.JdbcUtils;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;
public class Demo12Transaction {
    //没有异常，提交事务，出现异常回滚事务
    public static void main(String[] args) {
        20 / 21
        //1) 注册驱动
        Connection connection = null;
        PreparedStatement ps = null;
        try {
            //2) 获取连接
            connection = JdbcUtils.getConnection();
            //3) 开启事务
            connection.setAutoCommit(false);
            //4) 获取到 PreparedStatement
            //从 jack 扣钱
            ps = connection.prepareStatement("update account set balance = balance - ? where
                    name=?");
            ps.setInt(1, 500);
            ps.setString(2,"Jack");
            ps.executeUpdate();
            //出现异常
            System.out.println(100 / 0);
            //给 rose 加钱
            ps = connection.prepareStatement("update account set balance = balance + ? where
                    name=?");
            ps.setInt(1, 500);
            ps.setString(2,"Rose");
            ps.executeUpdate();
            //提交事务
            connection.commit();
            System.out.println("转账成功");
        } catch (Exception e) {
            e.printStackTrace();
            try {
                //事务的回滚
                connection.rollback();
            } catch (SQLException e1) {
                e1.printStackTrace();
            }
            System.out.println("转账失败");
        }
        finally {
            //7) 关闭资源
            JdbcUtils.close(connection,ps);
        }
    }
}
```

# 内容

1.数据库连接池

2.Spring JDBC：JDBC Template

## 数据库连接池

1.概念：其实就是一个容器（集合），存放数据库连接的容器。

​				当系统初始化后，容器被创建，容器中会申请一些连接对象，当用户来访问数据库时，从容器中获取连接对象，用户访问完之				后，会将连接对象还给容器。

2.好处：

​	1.节约资源

​	2.用户访问高效

3.实现：

​	1.标准接口：DataSource javax.sql包下的

​		1.方法：

​			**获取链接：getConnection()**

​			**归还连接：如果连接对象Connection是从连接池中获取的，那么调用Connection.close()方法，则不会关闭连接了，而是归还			连接**

​	2.一般我们不去实现它，有数据库厂商来实现

​		1.C3P0：数据库连接池技术

​		2.Druid：数据库连接池实现技术，由阿里巴巴提供的

4.C3P0：数据库连接池技术

​	步骤

​		1.导入jar包（两个）c3p0-0.9.5.5.jar	mchange-commons-java-0.2.19.jar

​		2.定义配置文件	c3p0.properties or c3p0-config.xml

​			1.名称：c3p0.properties 或者 c3p0-config.xml

​			2.路径：直接将文件放在src目录下即可。

​			3.创建核心对象	数据库连接池对象	CombopooledDataSource

​			4.获取连接：getConnection

5.Druid：数据库连接池实现技术，由阿里巴巴提供的

​	步骤：

​		1.导入jar包	druid-1.0.0.jar

​		2.定义配置文件：

​			是properties形式的

​			可以叫任意名称，可以放在 任意目录下

​		3.加载配置文件	properties

​		4.获取数据库连接池对象：通过工厂来获取	DruidDataSourceFactory

​		5.获取连接：getConnection

## Spring JDBC



