# 接口

**生活中的接口举例：电源插座当中的电源接口~**

​	接口就是一种**公共的规范标准**。

​	只要符合规范标准，就可以大家通用。

**计算机中接口：**

​	苹果电脑 -----（usb接口）---> （传数据）联想电脑

​							↓↓↓↓↓↓↓↓↓↓↓

​						只要符合usb接口的规范，

​						那么各种设备都可以使用。

​						U盘/打印机/夜读灯/usb小风扇……

## 接口定义

​	接口就是多个类的公共规范。

​	接口是一种引用数据类型，最重要的内容就是其中的抽象方法。

​	如何定义一个接口的格式：

​	public interface 接口名称{

​			//接口内容

​	}

​	备注：换成了关键字interface之后，编译生成的字节码文件依然是：.java--->.class。

​	如果是Java 7，那么接口可以包含的内容有：

​		1/常量

​		2/抽象方法

​	如果是Java 8，还可以额外包含有：

​		3/默认方法

​		4/静态方法

​	如果是Java 9，还可以额外包含有：

​		5/私有方法

​	

​	**在任何版本的Java中，接口都能定义抽象方法。**
**格式：**
​	public abstract 返回值类型 方法名称（参数列表）；

**注意事项：**

​	1.接口当中的抽象方法，修饰符必须是两个固定的关键字：public abstract

​	2.这两个关键字修饰符，可以选择性的省略（）

​	3.方法的三要素，可以随意定义。

```java
public interface MyInterAbstract {
    //这是一个抽象方法，不能写{}方法体~！！！！！
    public abstract void methodAbs1();
    //这也是抽象方法
    abstract void methodAbs2();
    //这也是抽象方法
    public void methodAbs3();
    //这也是抽象方法
    void methodAbs4();
}
```

## 接口使用步骤：

​	1.接口不能直接使用，必须有一个“实现类”来“实现”该接口。

​	格式：

​		public class 名称 implements 接口名称{

​			//……

​		}

```java
public class MyInterfaceAbstractImpl implements MyInterAbstract{
```

​	2.接口的实现类必须覆盖重写（实现）接口中所有的抽象方法。

​	去掉abstract关键字，加上方法体大括号。

```java
    @Override
    public void methodAbs1() {
        System.out.println("one");
    }
    @Override
    public void methodAbs2() {
        System.out.println("two");
    }
    @Override
    public void methodAbs3() {
        System.out.println("three");
    }
    @Override
    public void methodAbs4() {
        System.out.println("four");
    }
}
```

​	3.创建实现类的对象，进行使用。（非class类而是Interface）

```java
public interface MyInterAbstract {
    public abstract void methodAbs1();
    abstract void methodAbs2();
    public void methodAbs3();
    void methodAbs4();
}
```

​	**注意事项：**

​		如果实现类并没有覆盖重写接口中所有的抽象方法，那么这个实现类**自己就必须是抽象类**。

​	从Java 8开始，接口里允许定义默认方法。

​	**格式：**

​		public default 返回值类型 方法名称（参数列表）{

​			//方法体

​		}

​	**备注：**接口当中的默认方法，可以解决接口升级的问题。

​		1.接口的默认方法，可以通过接口实现类对象，直接调用。

​		2.接口的默认方法，也可以被接口实现类进行覆盖重写。

```java
public interface MyInterfaceDefault {//Interface类

    //抽象方法
    public abstract void methodAbs1();
    //新添加一个抽象方法
    // public abstract void methodAbs2();
    //新添加的方法，改成默认方法
    public default void methodDefault(){
        System.out.println("新添加");
    }
}
```

```java
public class MyInterfaceDefaultB implements MyInterfaceDefault{
    @Override
    public void methodAbs1() {
        System.out.println("yes2");
    }

    @Override
    public void methodDefault() {
        System.out.println("fugai");//实现类B覆盖重写了接口的默认方法
    }
}
```

```java
public class Demo02Interface {
    public static void main(String[] args) {
        //创建了实现类对象
        MyInterfaceDefaultA a = new MyInterfaceDefaultA();
        a.methodAbs1();//调用抽象方法，实际运行的是右侧实现类。
        //调用默认方法，如果实现类当中没有，会向上找接口。
    }
}
```

​	从java 8开始，接口当中允许定义静态方法。

​	**格式：**

​	public static 返回值类型 方法名称（参数列表）{

​			方法体

​	}

​	**提示：**就是将abstract或者default换成static即可，带上方法体。

## 静态接口：

​	不能通过接口实现类的对象来调用接口当中的静态方法。

​	正确方法：通过接口名称，直接调用其中的静态方法。

```java
public class Demo03Interface {
    public static void main(String[] args) {

        //MyInterfaceStaticImpl impl = new MyInterfaceStaticImpl();
        //错误写法
        //impl.methodStatic();
        MyInterfaceStatic.methodStatic();
    }
}
```

​	**格式：**

​		接口名称.静态方法名（参数）

**接口的私有方法定义：**

​	**问题描述：**

​		我们需要抽取一个公共方法，用来解决两个默认方法之间重复代码的问题。

​		但是这个公有方法不应该让实现类使用，应该是私有化的。

### 	**解决方案：**

​	从java 9开始，接口当中允许定义私有方法。

​	1.普通私有方法，解决多个默认方法之间重复代码问题。

​	private 返回值类型 方法名称（参数列表）{

​		//方法体

​	}

​	2.静态私有方法，解决多个静态方法之间重复代码问题。

​	private static 返回值类型 方法名称 （参数列表）{

​		//方法体

​	}

```java
public interface MyInterfacePrivateA {
    public default void methodDefault1(){
        System.out.println("mo1");
        methodCommon();
    }
    public default void methodDefault2(){
        System.out.println("mo2");
        methodCommon();
    }
    private void methodCommon(){
        System.out.println("A");
        System.out.println("B");
        System.out.println("C");
    }
}
```

```java
public interface MyInterfacePrivateB {
    public static void methodStatic1(){
        System.out.println("jing1");
        methodStaticCommon();
    }
    public static void methodStatic2(){
        System.out.println("jing2");
        methodStaticCommon();
    }
    private static void methodStaticCommon(){
        System.out.println("A");
        System.out.println("B");
        System.out.println("C");
    }
}
```

```java
public void methodAnother(){
    //直接访问到了接口中的默认方法，这样是错误的！
    //methodCommon();
}
```

```java
public static void main(String[] args) {
    MyInterfacePrivateB.methodStatic1();//√
    MyInterfacePrivateB.methodStatic2();//√
    //错误写法
    //MyInterfacePrivateB.methodStaticCommon();
```

## 接口的常量定义和使用

​	接口当中也可以定义“成员变量”，但是必须使用public static  final三个关键字进行修饰。

​	从效果上看，这其实就是接口的【常量】。

​	**格式：**

​		public static final 数据类型 常量名称 = 数值；

​	**备注：**

​		一旦使用**final关键字**进行修饰，说明不可改变。

```java
public class MyInterfaceConst {
    //这其实就是一个常量，一旦赋值，不可以修改。
    public static final int Num_OF_CLASS = 10;
}
```

​	**注意事项：**

​		1.接口当中的常量，可以省略public static final，注意：不写也照样是这样。

​		2.接口当中的常量，必须进行赋值，不能不赋值（不赋值默认为0）。

​		3.接口中常量的名称，使用完全大写的字母，用下划线进行分隔。（推荐命名规则）

```java
public class Demo05Interface {
    public static void main(String[] args) {
        //访问接口当中的常量
        System.out.println(MyInterfaceConst.Num_OF_CLASS);
    }
}
```

## 接口内容小结

​	**在Java 9+版本中，接口的内容可以有：**

​	1.**成员变量其实是常量，格式：**

​		[public] [static] [final] 数据类型 常量名称 = 数据值；

​		**注意：([ ]代表可省略)**

​		常量必须进行赋值，而且一旦赋值不能改变。

​		常量名称完全大写，用下划线进行分隔。

​	2.**接口中最重要的就是抽象方法，格式：**

​		[public] [abstract] 返回值类型 方法名称（参数列表）；

​		注意：实现类必须覆盖重写接口所有的抽象方法，除非实现类是抽象类。

​	3.**从Java 8开始，接口里允许定义默认方法，格式：**

​		[public] default 返回值类型 方法名称（参数列表）{方法体}

​	4.**从Java 8开始，接口里允许定义静态方法，格式：**

​		[public] static 返回值类型 方法名称（参数列表）{方法体}

​		注意：应该通过接口名称进行调用，不能通过实现类对象调用接口静态方法

​	5.**从Java 9开始，接口里允许定义私有方法，格式;**

​		普通私有方法：private 返回值类型 方法名称（参数列表）{方法体}

​		静态私有方法：private static 返回值类型 方法名称（参数列表）{方法体}

​		注意：private的方法只有接口自己才嗯那个调用，不能被实现类或别人使用。

#### **继承父类并实现多个接口**

​	**使用接口的时候，需要注意：**

​		1.接口是没有静态代码块或者构造方法的。

​		2.一个类的直接父类是唯一的，但是一个类可以同时实现多个接口。

​		**格式：**

​			public class MyInterfaceImpl implements MyInterfaceA，MyInterfaceB{

​			//覆盖重写所有抽象方法

​		}

​		**3.如果实现类所实现的多个接口当中，存在重复的抽象方法，那么只需要覆盖重写一次即可。**

```java
public interface MyInterfaceA {
	public abstract void methodAbs();
}
public interface MyInterfaceB {
	public abstract void methodAbs();
}
```

```java
@Override
public void methodAbs() {
    System.out.println("fu A+B");

}
```

​		**4.如果实现类没有覆盖重写所有接口当中的所有抽象方法， 那么实现类必须是一个抽象类。**

```java
public interface MyInterfaceA {
	public abstract void methodA();
	public abstract void methodAbs();
}
public interface MyInterfaceB {
	public abstract void methodB();
    public abstract void methodAbs();
}
```

```java
public abstract class MyInterfaceAbstract implements MyInterfaceA,MyInterfaceB{
    @Override
    public void methodA() {
    }

    @Override
    public void methodAbs() {
    }
    @Override
    public void methDefault() {
    }
}
```

​		**5.如果实现类所实现的多个接口当中，存在重复的默认方法，那么实现类一定要对冲突的默认方法进行覆盖重写。**

```java
public interface MyInterfaceA {
    public default void methDefault(){
        System.out.println("AAA");
    }
}
public interface MyInterfaceB {
    public default void methDefault(){
        System.out.println("BBB");
    }
}
```

```java
public class MyInterfaceImpl implements MyInterfaceA,MyInterfaceB{
    @Override
    public void methDefault() {
        System.out.println("duo ge fu gai");
    }
}

```

​		**6.一个类如果直接父类当中的方法，和接口当中的默认方法产生了冲突，优先用父类当中的方法。**

```java
public class Fu {
    public void method(){
        System.out.println("fufuf");
    }
}
```

```java
public class Zi extends Fu implements MyInterface{
}
```

```java
public interface MyInterface {
    public default  void method(){
        System.out.println("mo ren fang fa");
    }
}
```

```java
public class Demo01Interface {
    public static void main(String[] args) {
        Zi zi = new Zi();
        zi.method();
    }
}
```

#### 接口之间的多继承

​	1.类与类之间是单继承的，直接父类只有一个。

​	2.类与接口之间是多实现的，一个类可以实现多个接口。

​	3.接口与接口之间是多继承的。

​	**注意事项:**

​	1.多个父接口当中的抽象方法如果重复，没关系。

```java
public interface MyInterfaceA {
    public abstract void methodA();
    public abstract void methodCommon();
}
public interface MyInterfaceB {
    public abstract void methodB();
    public abstract void methodCommon();
}
```

```java
/*
这个子接口当中有几个方法？  4个
methodA 来源于A
methodB 来源于B
methodCommon 来源于A+B
method 来源于我自己
 */
public interface MyInterface extends MyInterfaceA,MyInterfaceB{
    public abstract void method();
}
```

​	2.多个父接口当中的默认方法如果重复，那么子接口必须继续进行默认方法的覆盖重写，【而且带着default关键字】。

```java
public interface MyInterfaceA {
    public default void methodDefault(){
        System.out.println("AAA");
    }
}
public interface MyInterfaceB {
    public default void methodDefault(){
        System.out.println("BBB");
    }
}
```

```java
public interface MyInterface extends MyInterfaceA,MyInterfaceB{
    @Override
    /*public*/default void methodDefault() {
    }
}
```