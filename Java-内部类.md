# 内部类

## 内部类的概念与分类

```java
/*
如果一个事物的内部包含另一个事物，那么这就是一个类内部包含另一个类。
例如：身体和心脏的关系，汽车和发动机的关系。

分类：
1.成员内部类
2.局部内部类（包含匿名内部类）
 */
```

## 成员内部类的定义

```java
//外部类：Body.class
//内部类：Body$Heart.class
/*
成员内部类的定义格式：
修饰符 class 外部类名称{
    修饰符 class 内部类名称{
        //……
    }
    //……
}
注意：内用外，随意访问；外用内，需要内部类对象。
 */
 public class Body {//外部类
    public class Heart{//成员内部类
        //内部类方法
        public void beat(){
            System.out.println("beng");
            System.out.println("我叫: " + name);
        }
    }
    private String name;
    //外部类方法
    public void methodBody(){
        System.out.println("外部类的方法");
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

## 成员内部类的使用

```java
/*
如何使用成员内部类？有两种方式：
1.间接方式：在外部类的方法当中，使用内部类，然后main只是调用外部类的方法。
2.直接方式：公式：
类名称 对象名 = new 类名称（）[普通方式]
外部类名称.内部类名称 对象名 = new 外部类名称（）.new 内部类名称（）
 */
public class Demo01InnerClass {
    public static void main(String[] args) {
        Body body = new Body();//外部类的对象
        //通过外部类的对象，调用外部类的方法，立面间接再使用内部类Heart
        body.methodBody();
        //按照公式来
        Body.Heart heart = new Body().new Heart();
        heart.beat();
    }
}

 public void methodBody(){
        System.out.println("外部类的方法");
        new Heart().beat();
    }
}//（同上）
```

## 内部类的同名变量访问

```java
/*
如果出现了重名现象，那么格式是：外部类名称.this.外部类成员变量名
 */
public class Outer {
    int num = 10;//外部类的成员变量
    public class Inner{
        int num = 20;//内部类的成员变量
        public void methodInner(){
            int num = 30;//内部类方法的局部变量
            System.out.println(num);//局部变量，就近原则
            System.out.println(this.num);//内部类的成员变量
            System.out.println(Outer.this.num);//外部类的成员变量
        }
    }
}

public class Demo02InnerClass {
    public static void main(String[] args) {
        Outer.Inner inner = new Outer().new Inner();
        inner.methodInner();
    }
}
```

## 局部内部类定义

```java
/*
如果一个类是定义在一个方法内部的，那么这就是一个局部内部类。
”局部“：只有当前所属的方法才能使用它，出了这个方法外面就不能用了。

定义格式：
修饰符 class 外部类名称{
    修饰符 返回值类型 外部类方法名称（参数列表）{
        class 局部内部类名称{
            //……
         }
    }
}
小节一下类的权限修饰符：
public > protected > (default) > private
定义一个类的时候，权限修饰符规则：
1.外部类：public/(default)
2.成员内部类：public / protected / (default) / private
3.局部内部类：什么都不能写，也并不是（default）含义。
 */
public class Outer1 {
    public void methodOuter1(){
        class Inner{//局部内部类
            int num  = 10;
            public void methodInner(){
                System.out.println(num);
            }
        }
        Inner inner = new Inner();
        inner.methodInner();
    }
}

public class DemoMain {
    public static void main(String[] args) {
        Outer1 obj = new Outer1();
        obj.methodOuter1();
    }
}
```

## 局部内部类的final问题

```java
/*
局部内部类：如果希望访问所在方法的局部变量，那么这个局部变量必须是【有效final的】。

从Java 8+开始，只要局部变量事实不变，那么final关键字可以省略。

原因：
1.new出来的对象再堆内存当中。
2.局部变量是跟着方法走的，再栈内存当中。
3，方法运行结束之后，立刻出栈，局部变量就会立刻消失。
4.new出来的对象会在堆当中持续存在，直到垃圾回收消失。
    为了解决这个问题，就将局部变量复制了一份作为内部类的成员变量，这样当局部变量死亡后，内部类仍可以访问它，实际访问的是局部变量的”copy“。
 */
public class MyOuter {
    public void methodOuter(){
        int num = 10;//所在方法的局部变量【就赋值了一次，为有效final】
//        num = 20；//错误

        class MyInner{//若上面的局部变量num死亡，则用copy
            public void methodInner(){
                System.out.println(num);
            }
        }
    }
}
```

## 匿名内部类

```java
/*
如果接口的实现类（或者是父类的子类）只需要使用唯一的一次，
那么这种情况下就可以省略掉该类的定义，而改为【匿名内部类】。

匿名内部类的定义格式:
接口名称 对象名 = new 接口名称(){
    //覆盖重写所有抽象方法
};
 */
public class DemoMain1 {
    public static void main(String[] args) {
        MyInterface obj = new MyInterfaceImpl();
        obj.method();
        MyInterface obj1 = new MyInterface() {
            @Override
            public void method(){
                System.out.println("匿名内部类实现了");
            }
        };
        obj1.method();
    }
}
```

## 匿名内部类的注意事项

```java
/*
对格式“new 接口名称（）{……}”进行解析：
1.new代表创建对象的动作
2.接口名称就是匿名内部类需要实现哪个接口
3.{……}这才是匿名内部类的内容
另外还要注意几点问题：
1.匿名内部类，在【创建对象】的时候，只能使用唯一一次。
如果希望多次创建对象，而且类的内容一样的话，那么就必须使用单独定义的实现类了。
2.匿名对象，在【调用方法】的时候，只能调用一次。
如果希望同一个对象，调用多次方法，那么必须给对象起个名字。
3.匿名内部类是省略了【实现类/子类】匿名对象是省略了【对象名称】
强调：匿名内部类和匿名对象不是一回事！！！
 */
 //使用了匿名内部类，而且省略了对象名称，也是匿名对象。
        /*MyInterface obj2 = */new MyInterface() {
            @Override
            public void method(){
                System.out.println("匿名内部类实现了");
            }

            @Override
            public void method1() {
                System.out.println("ninini222");
            }
        }.method1();
        //因为匿名对象无法调用第二次方法，所以需要再创建一个匿名内部类的匿名对象。
//        obj2.method();
//        obj2.method1();
```

## 类作为成员变量类型

创建一个**Weapon**类，里面含有：

```java
private String code;//武器代号
```

进行构造，并setter、getter

创建一个**Hero**类，里面含有：

```java
private String name;//英雄名字
private Weapon weapon;//武器
private int age;//年龄
```

并进行构造+setter、getter

**主函数：**

```java
public class DemoMain2 {
    public static void main(String[] args) {
        //创建一个英雄角色
        Hero hero = new Hero();
        //为英雄起一个名字，并且设置年龄
        hero.setName("盖伦");
        hero.setAge(20);

        //创建一个武器对象
        Weapon weapon = new Weapon("多兰剑");
        //为英雄配备武器
        hero.setWeapon(weapon);

        hero.attack();
    }
}
```

## 接口作为成员变量类型

创建一个**Skill接口**：

```java
public interface Skill {
    void use();//释放技能的抽象方法
}
```

创建一个**Hero类**

```java
private String name;//英雄的名称
private Skill skill;//英雄的技能
```

并构造，setter、getter

创建一个**SkillImpl实现类**

```java
public class SkillImpl implements Skill{
    @Override
    public void use() {
        System.out.println("Biu~biu~biu");
    }
}
```

**主函数**

```java
public class DemoGame {
    public static void main(String[] args) {
        Hero1 hero = new Hero1();
        hero.setName("艾希");//设置英雄名称
        //设置英雄技能
//        hero.setSkill(new SkillImpl());
        //还可以改成使用匿名内部类
       /* Skill skill = new Skill() {
            @Override
            public void use() {
                System.out.println("Pia~pia~pia");
            }
        };
        hero.setSkill(skill);*/
        //进一步简化,同时使用匿名内部类和匿名对象
        hero.setSkill(new Skill(){
            @Override
            public void use() {
                System.out.println("Biu~pia~biu~");
            }
        });
        hero.attack();
    }
}
```

## 接口作为方法的参数和返回值

```java
import java.util.ArrayList;
import java.util.List;
/*
Java.util.List正是ArrayList所实现的接口。
 */
public class DemoInterface {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();

        List<String> result = addName(list);
        for (int i = 0; i < list.size(); i++) {
            System.out.println(list.get(i));
        }
    }
    public static List<String> addName(List<String> list){
        list.add("di");
        list.add("ma");
        list.add("gu");
        return list;
    }
}
```