# Random

​	Random类用于生成随机数字。使用起来也是**三个步骤：**

​		**1.导包**

​			import java.util.Random;

​		**2.创建**

​			Random r = new Random();//小括号当中留空即可

​		**3.使用**

​			获取一个随机的int数字（范围是int所有范围，有正负两种）：int num = r.nextInt()

​			获取一个随机的int数字（参数代表了范围，左闭右开区间）：int num = r.nextInt(3)

​			实际上代表的含义是：[0,3),也就是0~2

​	题一：

​		根据int变量n的值，来获取随机数字，范围是[1,n],可以取到1也可以取到n。

​		思路：

​			1.定义一个int定量n，随意赋值。

​			2.要使用Random，三个步骤，导包、创建、使用。

​			3.如果写10，那么就是0~9，然而想要的是1~10，可以发现，整体+1即可。

​			4.打印随机数字。

```java
public class Random1 {
    public static void main(String[] args) {
        int n = 5;
        Random r = new Random();

        for(int i = 0;i < 100;i++){
            //本来范围是[0,n),整体+1后变成了[1,n+1),也就是[1,n]
            int result = r.nextInt(n)+1;
            System.out.println(result);
        }
    }
}
```

​	题二：

​		用代码模拟猜数字小游戏

​		思路：

​			1.首先需要产生一个随机数字，并且一旦产生不再变化。用Random的nextInt方法

​			2.需要键盘输入，所以用到了Scanner

​			3.获取键盘输入的数字，用Scnner当中的nextInt方法

​			4.已经得到了两个数字，判断（if）一下：

​				如果太小了，提示太小，并且重试。

​				如果太大了，提示太大，并且重试。

​				如果猜中了，游戏结束。

​			5.重试就是再来一次，循环次数不确定，用while（true）。

```java
public class Random2 {
    public static void main(String[] args) {
        Random r = new Random();
        int randomNum = r.nextInt(100)+1;//[1,100]
        Scanner sc = new Scanner(System.in);

        while(true){
            System.out.println("输入你的数字：");
            int guessNum = sc.nextInt();//自己猜的数
            
            if(guessNum > randomNum){
                System.out.println("big");
            } else if(guessNum < randomNum){
                System.out.println("small");
            }else{
                System.out.println("true");
                break;
            }
        }
        System.out.println("victory!");
    }
}
```