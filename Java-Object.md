# Object类

## Object概述

java.land.Object类是Java语言中的根类，即所有类的父类。它中描述的所有方法子类都可以使用。在对象案例化的时候，最终找的父类就是Object。

如果一个类没有特别指定父类，那么默认则继承自Object类。

## ToString方法

**Person类：**

```java
private String name;
private int age;
```

构造+setter、getter

```
@Override
public String toString() {
    return "Person{" +
            "name='" + name + '\'' +
            ", age=" + age +
            '}';
}
```

**主函数：**

```java
/*
    java.lang.Object类
    类Object是类层次结构的根（最顶层）类。每个类都是用Object作为超（父）类。
    所有对象（包括数组）都实现这个类的方法。
 */
public class Demo01ToString {
    public static void main(String[] args) {
        /*
            Person类默认继承Object类，所有可以使用Object类中的ToString方法。
            String toString() 返回该对象的字符串表示。
         */
        Person p = new Person("z",2);
        String s = p.toString();
        System.out.println(s);//Object.Person@27d6c5e0

        //直接打印对象的名字，其实就是调用对象的toString方法
        System.out.println(p);//Object.Person@27d6c5e0

        /*
            看一个类是否重写了toString方法，直接打印这个类对应对象的名字即可
            如果没有重写toString发放，那么打印的对象就是对象的地址值（默认）
            如果重写了toString方法，那么就按照重写的方式打印
        */
        Random r = new Random();
        System.out.println(r);//java.util.Random@5b6f7412       没重写
        Scanner sc = new Scanner(System.in);
        System.out.println(sc);//java.util.Scanner[delimiters=\p{javaWhitespace}+……     重写
        ArrayList<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(2);
        System.out.println(list);//重写
    }
}
```

## equals方法

```java
public class Demo02Equals {
    public static void main(String[] args) {
        /*
            Person默认继承了Object类，所以可以使用Object类的equals方法。
            boolean equals（Object obj）指示其他某个对象是否与此对象“相等”。
            Object类equals方法的源码：
                public boolean equals(Object obj){
                    return (this == obj);
                }
                参数：
                    Object obj：可以传递任意的对象
                方法体：
                    ==：比较运算符，返回的就是一个布尔值 true/false
                    基本数据类型：比较的是值
                    引用数据类型：比较的是两个对象的地址值
                this是谁？哪个对象调用的方法，方法中的this就是那个对象；p1调用的equals方法，所以this就是p1
                obj是谁？传递过来的参数p2
                this==obj-->p1==p2
         */
        Person p1 = new Person("dd",2);
        Person p2 = new Person("gu",3);
        p1 = p2;
        System.out.println(p1);//Object.Person@27d6c5e0
        System.out.println(p2);//Object.Person@4f3f5b24


        boolean b = p1.equals(p2);
        System.out.println(b);//false
    }
}
```

## 重写Object类的equals方法

```java
/*
        直接打印对象的地址值没有意义，需要重写Object类的toString方法
        打印对象的属性（name，age）
     */
    /*@Override
    public String toString(){
        //return "abc";//返回啥打印啥
        return "Person(name = "+name+",age = "+age+")";
    }
*/

    /*@Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }*/
    /*
            Object类的equals方法默认比较的是两个对象的地址值，没有意义
            所以我们需要重写equals方法，比较两个对象的属性（name，age）
                对象的属性值一样，返回true；否则返回false
            问题：
                隐含着一个多态
                Object obj = p2 = new Person("gu",3);
                多态弊端：无法使用子类特有的内容（属性，方法）
                解决：可以使用向下转型（强转）把Object类型转换为Person
         */

    /*@Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        Person person = (Person) o;

        if (age != person.age) return false;
        return name != null ? name.equals(person.name) : person.name == null;
    }*/

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return age == person.age && Objects.equals(name, person.name);
    }

    /*    @Override
    public boolean equals(Object obj) {
        //增加一个判断，传递的参数obj是this本身，直接返回true，提高程序效率
        if(obj == this){
            return true;
        }
        //增加一个判断，传递的参数obj是null，直接返回false，提高程序的效率
        if(obj == null){
            return false;
        }
        *//*
            增加一个判断，是Person类型再转换，防止类型转换异常ClassCastException
         *//*
        if(obj instanceof Person){
            //使用向下转型（强转）把Object类型转换为Person
            Person p = (Person)obj;
            //比较两个对象的属性;一个是调用方法的this(p1)，一个就是p(obj=p2)
            boolean b = this.name.equals(p.name) && this.age==p.age;
            return b;
        }
        //不是Person类型直接让它返回false
        return false;
    }*/

 Person p1 = new Person("dd",2);
        Person p2 = new Person("dd",2);
//        p1 = p2;
        System.out.println(p1);//Object.Person@27d6c5e0
        System.out.println(p2);//Object.Person@4f3f5b24

        Random r = new Random();

        //boolean b = p1.equals(r);
//        boolean b = p1.equals(null);
        boolean b = p1.equals(null);
        System.out.println(b);//false
```