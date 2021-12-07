# Final关键字

```
final关键字代表最终/不可改变的。
常见四种用法：
1.可以修饰一个类
2.可以修饰一个方法
3.还可以用来修饰一个局部变量
4.还可以用来修饰一个成员变量
```

## 1.修饰类

```java
/*
当final关键字用来修饰一个类的时候，格式：
public final class 类名称{
    //……
}
含义：当前这个类不能有任何的子类。（太监类）
注意：一个类如果是final的，那么其中的所有的成员方法都无法进行覆盖重写（因为没儿子）
 */
public final class MyClass /*extends Object*/{
    public void method(){
        System.out.println("方法执行");
    }
}

/*
不能使用final类来作为父类
 */
public class MySubClass extends MyClass{
}
```

## 2.修饰方法

```java
/*
当final关键字用来修饰一个方法的时候，这个方法就是最终方法，也就是不能被覆盖重写。
格式：
修饰符 final 返回值类型 方法名称（参数列表）{
    //……
}
注意事项：
对于类、方法来说，abstract关键字和final关键字不能同时使用，因为矛盾。（一个必重，一个必不重）
 */
public class Fu {
    public final void method(){
        System.out.println("FUggg");
    }
}

public class Zi extends Fu{
    //错误写法，不能覆盖重写父类当中final的方法。
//    @Override
//    public void method() {
//        System.out.println("ZZIZIZI");
//    }
}

```

## 3.修饰一个局部变量

```java
public class Demo02Final {
    public static void main(String[] args) {
        int num1 = 10;
        System.out.println(num1);
        num1 = 20;
        System.out.println(num1);
        //一旦使用final用来修饰局部变量，那么这么变量就不能进行更改。
        //“一次赋值，终生不变“
        final int num2 = 200;
        System.out.println(num2);
//        num2 = 300;错误写法
        //正确写法，只要保证有唯一一次赋值即可
        final int num3;
        num3 = 30;
        //对于基本类型来说，不可变说的是变量当中的数据不可改变
        //对于引用类型来说，不可变说的是变量当中的地址值不可改变
        Student stu1 = new Student("zzz");
        System.out.println(stu1);
        System.out.println(stu1.getName());
        stu1 = new Student("hhh");
        System.out.println(stu1);
        System.out.println(stu1.getName());
        System.out.println("================");
        final Student stu2 = new Student("ggg");
        System.out.println(stu2.getName());
        //错误写法，final的引用类型变量，其中的地址不可改变
//        stu2 = new Student("ghh");错误写法
        stu2.setName("gyyagyy");
        System.out.println(stu2.getName());
    }
}

public class Student {
    private String name;

    public Student() {
    }

    public Student(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

## 4.修饰一个成员变量

```java
/*
对于成员变量来说，如果使用final关键字修饰，那么这个变量也照样不可变。
1.由于成员变量具有默认值，所以用了final之后必须手动赋值，不会再给默认值了。
2.对于final的成员变量，要么使用直接赋值，要么通过构造方法赋值。二者选其一
3.必须保证类当中所有重载的构造方法，都最终会对final的成员变量进行赋值。
 */
public class Person {
    private final String name /*= "aaa"*/;

    public Person() {
        name = "guan";
    }

    public Person(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

//    public void setName(String name) {
//        this.name = name;
//    }
}
```