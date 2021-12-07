# Java-ArrayList

## 一、**基本方法**

​	数组的长度不可以发生改变。

​	但是ArrayL ist集合的长度是可以随意变化的。

​	对于ArrayList来说，有一个尖括号<E>代表泛型。

​	泛型:也就是装在集合当中的所有元素，全都是统一-的什么类型.

**注意：**泛型只能是引用类型，不能是基本类型。

​	//错误写法!泛型只能是引用类型，不能是基本类型

​	ArrayList<int> listB = new ArrayList<>();

​	ArrayList<Integer> list = new ArrayList<>();

**定义方法的三要素：**

​	返回值类型:只是进行打印而已，没有运算，没有结果，所以用void

​	·方法名称: printArrayList

​	参数列表: Arraylist

### **注意事项:**

​	对于ArrayList集合来说，直接打印得到的不是地址值，而是内容。	

​	如果内容是空，得到的是空的中括号: []

**举例：：**

​	public static void main(String[] args) {

​		//创建了一个ArrayList集合，集合的名称是list，里面装的全都是String字符串类型的数据

​		//备注:从JDK 1.7+开始，右侧的尖括号内部可以不写内容，但是<>本身还是要写的。

​		ArrayList<String> list = new ArrayList<>();

​		System. out . println(list); // []

​	//向集合当中添加一-些数据，需要用到add方法。

**ArrayList当中的常用方法有:**

​	public boolean add(E e):向集合当中添加元素，参数的类型和泛型一致。返回值代表添加是否成	功。

​	备注:对于ArrayList集合来说，add添加动作一 -定是成功的， 所以返回值可用可不用。

​	但是对于其他集合(今后学习)来说，add添加动作不一-定 成功。

​	public E get(int index): 从集合当中获取元素，参数是索引编号，返回值就是对应位置的元素。

​	public E remove(int index): 从集合当中删除元素，参数是索引编号，返回值就是被删除掉的元素。

​	public int size(): 获取集合的尺寸长度，返回值是集合中包含的元素个数。

**add：：：**

​	//向集合中添加元素:add

​	boolean success =list. add("柳岩");

​	list. add(100); //错误写法!因为创建的时候尖括号泛型已经说了是字符串，添加进去的元素就必须都	是字符串才行

**size：：：**

​	//获取集合的长度尺寸，也就是其中元素的个数

​	int size = list.size();

**get：：：**

​	//从元素中获取元素，get，索引值从0开始。

​	String name = listget(2);.

**remove：：：**

​	//从集合中删除元素：remove，索引值从0开始。

​	String whoRemoved = list.remove(index:3);

### 包装类：：：

如果希望向集合ArrayList当中存储基本类型数据，必须使用基本类型对应的“包装类”。

基本类型					包装类(引用类型，包装类都位于java. lang包下)

byte							Byte

short						Short

int								Integer

long							Long

float							Float

double						Double

char							Character

boolean						Boolean

**题一**

​	生成6个1~33之间的随机整数，添加到集合，并遍历集合。

​	思路：

​		1.需要存储6个数字，创建一个集合， <Integer>

​		2.产生随机数，需要用到Random

​		3.用循环6次，来产生6个随机数字: for循环

​		4.循环内调用r. nextInt(int n)，参数是33，0~32， 整体+1才是1~33

​		5.把数字添加到集合中: add

​		6.遍历集合: for、 size、 get

**题二**

​	自定义4个学生对象，添加到集合，并遍历。

​	思路：

​		1.自定义Student学生类， 四个部分。

​		2.创建一个集合，用来存储学生对象。泛型: <Student>

​		3.根据类，创建4个学生对象。

​		4.将4个学生对象添加到集合中: add

​		5.遍历集合: for、 size、get 

**题三**

​	定义以指定格式打印集合的方法(ArrayList类型作为参数)，使用{}扩起集合，使用@分隔每个元素。

​	格式参照{元素@元素@元素}。

​	Sys tem. out. println(list);		[10，20, 30]

​	printArraylist(list);					{10@20@30}

**题四**

​	用一个大集合存入20个随机数字，然后筛选其中的偶数元素，放到小集合当中。

​	要求使用自定义的方法来实现筛选。

​	分析:

​		1.需要创建一个大集合，用来存储int数字: <Integer>

​		2.随机数字就用Random nextInt

​		3.循环20次，把随机数字放入大集合: for循环、add方法

​		4.定义一个方法，用来进行筛选。

​	筛选:根据大集合，筛选符合要求的元素，得到小集合。

​	三要素

​	返回值类型: ArrayList小集合 (里面元素个数不确定)

​	方法名称: getSmalllist

​	参数列表: ArrayList大集合(装着20个随机数字)

​		5.判断(if)是偶数:num%2==0

​		6.如果是偶数，就放到小集合当中，否则不放。