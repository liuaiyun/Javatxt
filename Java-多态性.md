# 多态性

​	面向对象三大特性：封装性，继承性，多态性。

​	extends继承或者implements实现，是多态性的前提。

​	父类：人类->学生 					员工

​	小明同学是一个学生（学生形态），但同时也是一个人（人类形象）。

​	小明是一个对象，这个对象既有学生形态，也有人类形态。

​	一个对象拥有多种形态，这就是**对象的多态性**。

#### 	体现多态性：

```
代码当中体现多态性，其实就是一句话，父类引用指向子类对象。
格式：
父类名称 对象名 = new 子类名称();
接口名称 对象名 = new 实现类名称();
```

​	体现多态性：左父右子

```java
public class Demo01Multi {
    public static void main(String[] args) {
        //使用多态的写法
        //左侧父类的引用，指向了右侧子类的对象。
        Fu fu = new Zi();
        fu.method();//new谁运行谁
        fu.methodFu();
    }
}
public class Fu {
    public void method(){
        System.out.println("fufufu");
    }
    public void methodFu(){
        System.out.println("fufufziziz");
    }
}
public class Zi extends Fu{
    @Override
    public void method() {
        System.out.println("ZZZ");
    }
}
```

#### 成员变量使用特点

​	访问成员变量的两种方式：

​		1.直接通过对象名称访问：看等号左边是谁，优先用谁，没有则向上找。（成员变量不能进行覆盖重写，方法能覆盖重写。）

​		2.间接通过成员方法访问：看该方法属于谁，优先用谁，没有则向上找。

#### 成员方法规则

​	在多态的代码当中，成员方法的访问规则是：

​	看new的是谁，就优先用谁，没有则向上找。

​	口诀：编译看左边，运行看右边。

#### 对比

成员变量，编译看左边，运行还看左边。

成员方法，编译看左边，运行看右边。

```java
//Zi函数：：
public void methodZi1(){
    System.out.println("teyouzi");
}
//主函数：：fu1.methodZi1();//错误}}编译看左，左边是Fu，中没有zi方法。
```

#### 使用多态的好处：

​	父类：员工//方法 work() //抽象

​	子类1：讲师类//work(){讲课}

​	子类2：助教类//work(){辅导}

###### 	如果不用多态，只用子类，那么写法是：

​		**Teacher** one = new Teacher();

​		one.work();//讲课

​		**Assistant** two = new Assistant();

​		two.work();//辅导

​	我现在唯一要做的事，就是调用work方法，其他的功能不关心。

###### 	如果使用多态写法，对比一下：

​		**Employ** one = new Teacher();

​		one.work();//讲课

​		**Employ** two = new Assistant();

​		two.work();//辅导

**好处：无论右边new的时候换成哪个子类对象，等号左边调用方法都不会变化。**

#### 对象的向上转型：

​	父类：动物类//方法eat();//抽象

​	子类：猫//eat(){🐟}

​	1.对象的向上转型，其实就是多态写法：

​	父亲名称 对象名 = new 子类名称();																Animal animal = new Cat();

​	含义：右侧创建一个子类对象，把它当作父类来看待使用。						创建了一只猫，当作动物看待。

​	注意事项：**向上转型一定是安全的**。从小范围转向了大范围。从小范围的猫向上转换成为更大范围的动物。

​	**类似于：**

​	double num = 100；//正确		int-->double，自动类型转换。

向上转型也有一个**弊端**：对象一旦向上转型为父类，那么久无法调用子类原本特有的内容。

#### 对象的向下转型：（有前提条件）

​	向上转型了后想还原：

​	解决方案：用对象的向下转型【还原】。

​	2.对象的向下转型，其实就是一个【还原】的动作。（本来是什么才能还原成什么）

​	**格式：子类名称 对象名 = （子类名称）父类对象；**

​	含义：将父类对象还原成为**本来**的子类对象。

​	Animal anmal = new Cat();//本来是猫，向上转型为动物。

​	Cat cat = (Cat)animal;//本来是猫，已经被当作动物了，还原回来成为本来的猫。

​	**注意事项：**

​		a.必须保证对象本来创建的时候就是猫，才能向下转型成为猫。

​		b.如果对象创建的时候本来不是猫，现在非要向下转型成为猫，就会报错。(ClassCastException)

​	**类似于**：基本类型当中的强制类型转换。【int num = (int) 10.0//可以     int num = (int） 10.5//不可以，精度损失】

#### instanceof关键字

```java
/*
如何才能知道一个父类引用的对象本来是什么子类
格式：
对象 instanceof 类型
选择会得到一个boolean值结果，也就是判断前面的对象能不能当作后面类型的实例。
 */
public class Demo02Instanceof {
    public static void main(String[] args) {
        Animal animal = new Dog();//本来为猫
        animal.eat();//猫吃鱼
        //如果希望调用子类特有方法，需要向下转型
//        if(animal instanceof Dog) {
//            Dog dog = (Dog) animal;
//            dog.catchMouse();
//        }
//        if(animal instanceof Cat){
//            Cat cat = (Cat) animal;
//            cat.catchMouse();
//        }
        giveMeAPet(new Dog());
    }
    public static void giveMeAPet(Animal animal){
        if(animal instanceof Dog) {
            Dog dog = (Dog) animal;
            dog.catchMouse();
        }
        if(animal instanceof Cat){
            Cat cat = (Cat) animal;
            cat.catchMouse();
        }
    }
}
```

#### 接口+多态案例

笔记本电脑类：开机powerOn()

​							关机powerOff()

​							使用设备useDevice(**USB usb**)----------(使用接口)--------->>(USB)[打开设备，关闭设备]

​							-------------(实现接口)------------->>>>>>>>

​							------->>鼠标类：

​										打开鼠标		/		关闭鼠标

​							------->>键盘类：

​										打开键盘		/		关闭键盘

​							鼠标	/	键盘和笔记本电脑没有直接联系（通过接口实现）

```java
public interface USB {
    public abstract void open();
    public abstract void close();
}


public class Computer {
    public void powerOn(){
        System.out.println("kai ji ");
    }
    public void powerOff(){
        System.out.println("guan ji ");
    }
    //使用USB设备的方法，使用接口作为方法的参数。
    public void usbDevice(USB usb){
        usb.open();//打开设备
        if(usb instanceof  Mouse){
            Mouse mouse = (Mouse) usb;
            mouse.click();
        }else if(usb instanceof Keyboard){
            Keyboard keyboard = (Keyboard) usb;
            keyboard.type();
        }
        usb.close();//关闭设备
    }
}


public class Mouse implements USB{
    @Override
    public void open() {
        System.out.println("打开鼠标");
    }

    @Override
    public void close() {
        System.out.println("关闭鼠标");
    }
    public void click(){
        System.out.println("鼠标点击");
    }
}
public class Keyboard implements USB{
    @Override
    public void open() {
        System.out.println("打开键盘");
    }

    @Override
    public void close() {
        System.out.println("关闭键盘");
    }
    public void type(){
        System.out.println("键盘输入");
    }
}


public class Demo03Main {
    public static void main(String[] args) {
        //创建一个笔记本
        Computer computer = new Computer();
        computer.powerOn();
        //准备一个鼠标供使用
        //首先进行向上转型
        USB usbMouse = new Mouse();
        //参数是USB类型，我正好传递进去的是USB鼠标
        computer.usbDevice((usbMouse));
        //创建一个USB键盘
        Keyboard keyboard = new Keyboard();//没有使用多态写法
        //方法参数是USB类型，传递进去的是实现类对象
        computer.usbDevice(keyboard);//正确写法，也发生了向上转型
        computer.usbDevice(new Keyboard());//正确!!
        computer.powerOff();

        method(10);//正确写法，double-->double
        method(10.0);//正确写法，int-->double
        int a = 30;
        method(a);
    }
    public static void method(double num){
        System.out.println(num);
    }
}

```