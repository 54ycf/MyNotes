# (1)Java类基础知识

## 基本类型和运算符

- boolean：默认false；

  注意boolean a=1;语句是错的

- byte：1字节；有符号的以二进制补码表示的整数；默认为0

  注意byte a=128;是错的，但byte a=(byte)128;将会进行截断

- short：2字节；有符号的以二进制补码表示的整数；默认为0


- int：4字节；


- long：8字节；默认为0L

  注意long a=2147483648;会报错，超过int必须加L，int内会隐式转换成long。最好加上L

- float：4字节；默认为0.0f

  注意float a=1.23f;必须加f

- double：8字节；默认为0.0d

  可以省略d，因为默认是double

- char：是一个单一的16位Unicode字符

  可以存汉字



## 选择和循环结构

- 注意switch(a)中的a可以为String


- 注意if else 语句别被结构蒙骗




## 自定义函数

- 重载函数(overload)：函数名相同，函数参数的个数或者类型必须有所不同

  注意不能以返回值来区分同名函数



## 类和对象

- Java的基本型别是值拷贝，对象赋值是Reference(指针)赋值。

  故swap(int , int)不会交换

- 函数内的局部变量，编译器不会给默认值，必须初始化。


- 类的成员变量，编译器会默认都是"0"，可以直接使用。





# (2)继承、接口和抽象类

## 继承

```java
public class Base {
	private int num = 10;
	
	public int getNum()
	{
		return this.num;
	}
}

public class Derived extends Base{
	private int num = 20;	//part1
	
	public int getNum()	{	//part2
		return this.num;
	}	
	
	public static void main(String[] args) {
		Derived foo = new Derived();
		System.out.println(foo.getNum());
        //有子类的方法则优先调用，没有再去找父类，故输出20
	}
}
//若有part1，无part2，则输出10，因为getNum方法只有父类才有。
//若有part2，无part1，则编译报错，因为this.num只在本类找
//若都没有，则输出10
```

单根继承原则：每个类都只能继承一个类

如果不写extends，则Java类都默认继承java.lang.Object类，所有类都从此开始构建出一个类型继承树



```java
public class A {
	public A()
	{
		System.out.println("111111");
	}
	public A(int a)
	{
		System.out.println("222222");
	}
}

public class B extends A{
	public B()
	{
		//super(); 编译器自动增加super()
		System.out.println("333333");
	}
	public B(int a)
	{
		super(a);  //编译器不会自动增加super();
		System.out.println("444444");
	}
	public static void main(String[] a)
	{
		B obj1 = new B();
		System.out.println("==============");
		B obj2 = new B(10);		
	}
}
输出
111111
333333
==============
222222
444444
若super(a)删除，则输出1 3 = 1 4
```

每个子类的构造函数的第一句话，都默认调用父类的无参构造函数super()，除非子类的构造函数第一句话是super，且语句位于第一条。

​	不会出现连续两条super语句。super()只能放在构造函数最前面。



## 抽象类

方法只有方法名字，没有方法体，则是抽象类

```java
public abstract class Shape {
	int area;
    public abstract void calArea(); 
}

abstract class A{}	//OK
```

A a=new A();错，抽象类不可以new出来

若方法只有名字，则必须加abstract前缀变成抽象方法，只要有抽象方法，就必须要声明成抽象类。

随便一个类加上abstract前缀不会报错，但是就new不出来了。如果一个方法前缀加上abstract，则会报错，因为abstract只能修饰方法名。

抽象类也是类，一个类继承于抽象类，就不能继承于其他的类或抽象类。

抽象类的子类：

​	1、继承的子类把所有的抽象方法都实现了，变成一般类。

​	2、继承的子类未全部实现，依旧是抽象类，需要一个abstract帽子。



## 接口

接口是为了弥补单根继承的不足。

如果类的所有方法都是空的，则这个类就算是接口interface，是更特殊的抽象类，可以不算类。

```java
public interface Animal {
	public void eat();
	public void move();
}
```



```java
public class Cat implements Animal
{
	public void eat() {
		System.out.println("Cat: I can eat");
	}
	
	public void move(){
		System.out.println("Cat: I can move");
	}
}
public abstract class LandAnimal implements Animal {

	public abstract void eat() ;

	public void move() {
		System.out.println("I can walk by feet");
	}
}
```

类去实现接口，就必须实现接口所有的空的方法，否则就一定是个抽象类，要戴个abstract帽子。



```java
public abstract class Rabbit extends LandAnimal implements ClimbTree {

	public void climb() {
		System.out.println("Rabbit: I can climb");		
	}

	public void eat() {
		System.out.println("Rabbit: I can eat");		
	}
}
```

继承和实现可以同时。但是注意extends和implements的顺序不能颠倒。



```java
public interface ClimbTree {
	public void climb();
}
public interface CatFamily extends Animal, ClimbTree{
	//包含以下三个方法
	//eat()
	//move()
	//climb()
}
```

**接口**可以**继承多个接口。**

**类**只可以继承(extends)一个类，但是**可以实现(implements)多个接口**。

接口可以定义变量，但一般是常量。



## 转型

基本类型变量互相转化，如int a=(int)3.5;

类型可以相互转型，但是只限制于有继承关系的类。

​	子类可以转化成父类，而父类不能转化成子类。

``` java
//Man继承于Human
Human obj1 = new Man();	//OK
Man   obj2 = new Human(); //No
```



父类转化成子类的情况

​	即该父类本身就是子类转化过来的。

```java
Human obj1 = new Man();
Man   obj2 = (Man)obj1;
```



## 多态

```java
public class Human {
	int height;    
    int weight;
    
    public void eat()  {
    	System.out.println("I can eat!");
    }
}

public class Man extends Human {
	public void eat() {
		System.out.println("I can eat more");
	}
	
	public void plough() { }

	public static void main(String[] a)	{
		Man obj1 = new Man();
		obj1.eat();   // call Man.eat()
		Human obj2 =  (Human) obj1;
		obj2.eat();   // call Man.eat() 这里千万注意！！！！！！！！
		Man obj3 = (Man) obj2;
		obj3.eat();	  // call Man.eat()
	}
}
//obj1==obj2==obj3 三个指向同一个内存，调用的方法都是依赖这个对象指定的。
相当于obj1指向一个大盒子，obj2也是一个指向obj1的那个大盒子，但是被遮住了一部分。obj3相当于把遮住的地方又揭开了。
```

子类继承父类的所有方法，但是可以重新定义一个和父类某个方法的外表长相一模一样的方法(即名字、参数都一样)，这就是重写(overwrite)。

​	注意区分重载(overload是同名不同参)。

子类的方法优先级高于父类。

子类对象转为父类对象之后所调用的普通方法，依旧是子类本身的方法。



```java
public interface Animal {
	public void eat();
	public void move();
}

public class Cat implements Animal
{
	public void eat() {
		System.out.println("Cat: I can eat");
	}
	
	public void move(){
		System.out.println("Cat: I can move");
	}
}

public class Dog implements Animal
{
	public void eat() {
		System.out.println("Dog: I can eat");
	}
	
	public void move() {
		System.out.println("Dog: I can move");
	}
}

public class AnimalTest {
	
	public static void haveLunch(Animal a)
    {
		a.eat();
	}
	
	public static void main(String[] args) {
		Animal[] as = new Animal[4];	//相当于new的指针
		as[0] = new Cat();
		as[1] = new Dog();
		as[2] = new Cat();
		as[3] = new Dog();
		
		for(int i=0;i<as.length;i++) {
			as[i].move();  //调用每个元素的自身的move方法
		}
		for(int i=0;i<as.length;i++) {
			haveLunch(as[i]);	//调用每个动物自身的eat
		}
		
		haveLunch(new Cat());  //隐含向上转型Animal a = new Cat(); haveLunch(a);
		haveLunch(new Dog());	
        /*类相互调用中，Java普遍采用接口的契约设计，使程序之间的耦合度减轻*/
		haveLunch(
				new Animal()
				{
					public void eat() {
						System.out.println("I can eat from an anonymous class");						
					}
					public void move() {
						System.out.println("I can move from an anonymous class");
					}
					//Animal接口不能new，但是这里可以吧eat和move补全。
                    //没有取名字，是个匿名类，这个类只在这里用一次就结束了
                    //主需要传进来一个实现Animal接口的对象就可以运行，完成解耦关系
				});
	}
}
/*
Cat: I can move
Dog: I can move
Cat: I can move
Dog: I can move
Cat: I can eat
Dog: I can eat
Cat: I can eat
Dog: I can eat
Cat: I can eat
Dog: I can eat
I can eat from an anonymous class
*/
```

以统一的接口来操纵某一类中不同对象之间的动态行为

对象之间的解耦



# (3)static，final和常量设计

## static

static静态变量(**类变量**)是类共有成员，是依赖于类存在的，不依赖对象实例存在。整个类就只有一个地方放该变量，即所有对象实例都共享这个空间(栈)。

static方法即静态方法也不需要对象来引用，通过类名可以直接引用。

​	静态方法只能使用静态变量且进制引用非静态方法。

```java
public class StaticMethodTest {
	int a = 111111;
	static int b = 222222;
	public static void hello()
	{
		System.out.println("000000");
		System.out.println(b);
		//System.out.println(a);  //error, cannot call non-static variables
		//hi()                    //error, cannot call non-static method
	}
	public void hi()
	{
		System.out.println("333333");
	}
	public static void main(String[] a)
	{
		StaticMethodTest.hello();
		//StaticMethodTest.hi(); //error, 不能使用类名来引用非静态方法
		StaticMethodTest foo = new StaticMethodTest();
		foo.hello();  //warning, but it is ok，建议通过类名来访问静态方法
		foo.hi();     //right
	}
}

```

注意static修饰的context中不能用this



**static块**

只在类第一次加载时调用，在程序运行期间，该代码值运行一次。

匿名块每次都会调用，是动态的。代码块不建议写。

执行顺序：static块 > 匿名块 > 构造函数。

```java
class StaticBlock
{
	//staticl block > anonymous block > constructor function	
	static
	{
		System.out.println("22222222222222222222");
	}
	{
		System.out.println("11111111111111111111");
	}
	public StaticBlock()
	{
		System.out.println("33333333333333333333");
	}
	{
		System.out.println("44444444444444444444");
	}
}

public class StaticBlockTest {

	public static void main(String[] args) {

		StaticBlock obj1 = new StaticBlock();
		StaticBlock obj2 = new StaticBlock();
	}

}
/*输出
22222222222222222222
11111111111111111111
44444444444444444444
33333333333333333333
11111111111111111111
44444444444444444444
33333333333333333333
*/
```



**单例模式**(singleton)

保证一个类有且只有一个对象：

采用static来共享对象实例

采用private构造函数防止外界new操作

```java
public class Singleton {
	private static Singleton obj = new Singleton(); 
    	//共享同一个对象，此处调用构造函数
	private String content;
	
	private Singleton()  //确保只能在类内部调用构造函数
	{
		this.content = "abc";
	}
	
	public String getContent() 	{
		return content;
	}
	public void setContent(String content) {
		this.content = content;
	}	
	
	public static Singleton getInstance()	{
		//静态方法使用静态变量
		//另外可以使用方法内的临时变量，但是不能引用非静态的成员变量
        //外界只能通过getInstance来获取对象，且所有对象实例都是obj
		return obj;
	}
	
	
	public static void main(String[] args) {
		Singleton obj1 = Singleton.getInstance();
		System.out.println(obj1.getContent());  //abc
		
		Singleton obj2 = Singleton.getInstance();
		System.out.println(obj2.getContent());  //abc
		
		obj2.setContent("def");
		System.out.println(obj1.getContent());
		System.out.println(obj2.getContent());
		
		System.out.println(obj1 == obj2); //true, obj1和obj2指向同一个对象
	}
}
/*
abc
abc
def
def
true
*/
```



## final

用来修饰类、方法、成员变量

final基本变量不能再赋值，final对象实例不能更改指针，但是可以更改final对象里面的内容。

final的类不能被继承，子类也不能更改父类中的final方法。但是改变参数列表就可以写。

​	即不可重写，可以重载。



## 常量设计

Java没有constant关键字，故采用：

**public static final**

不能修改用final

不会修改，只要一份，用static

方便访问用public

​	注意interface里的变量默认是常量，不需要psf修饰。因为接口是大家都遵循的，不希望随便修改



## 常量池

Java为了提高程序性能，给很多基本类型的**包装类/字符串**(这里指**常量字符串**，new出来的不算)都建立了常量池。

常量池中，相同的值只存储一份，需要用就指向它，节省空间，共享访问

```java
Integer n1 = 127;
Integer n2 = 127;
System.out.println(n1==n2); //true
//对象双等号是比较指针是否指向同一个东西

Integer n3 = 128;
Integer n4 = 128;
System.out.println(n3==n4);	//false

Integer n5 = new Integer(127);
System.out.println(n1==n5);	//false
```

```java
public class A {
	public Integer num = 100;
	public Integer num2 = 128;
	public Character c = 100;
}

public class B {
	public Integer num = 100;
	public Integer num2 = 128;
	public Character c = 100;
	
	public static void main(String[] args) {
		A a = new A();
		B b = new B();
        
		//Integer -128~127有常量池  true
		System.out.println(a.num == b.num);
        
		//Integer 128不在常量池          false
		System.out.println(a.num2 == b.num2);
        
		//Character 0-127在常量池    true
		System.out.println(a.c == b.c);
	}
}
```

Java为以下包装类建立了常量池：

Boolean： true、false

Byte：-128~127，Character：0~127

Short、Integer、Long：-128~127

注意Float和Double没有常量池

```java
Boolean b1 = true;  //true,false
Boolean b2 = true;
System.out.println("Boolean Test: " + String.valueOf(b1 == b2));

Byte b3 = 127;     //\u0000-\u007f
Byte b4 = 127;
System.out.println("Byte Test: " + String.valueOf(b3 == b4));

Character c1 = 127;  //\u0000-\u007f
Character c2 = 127;
System.out.println("Character Test: " + String.valueOf(c1 == c2));

Short s1 = -128;  //-128~127
Short s2 = -128;
System.out.println("Short Test: " + String.valueOf(s1 == s2));

Integer i1 = -128;  //-128~127
Integer i2 = -128;
System.out.println("Integer Test: " + String.valueOf(i1 == i2));

Long l1 = -128L;  //-128~127
Long l2 = -128L;
System.out.println("Long Test: " + String.valueOf(l1 == l2));

Float f1 = 0.5f;
Float f2 = 0.5f;
System.out.println("Float Test: " + String.valueOf(f1 == f2));	//false

Double d1 = 0.5;
Double d2 = 0.5;
System.out.println("Double Test: " + String.valueOf(d1 == d2));	//false
```

字符串如下

```java
String s1 = "abc";
String s2 = "abc";
String s3 = "ab" + "c"; //都是肉眼可见的常量，编译器将自动识别并优化，下同
String s4 = "a" + "b" + "c";
System.out.println(s1 == s2); //true
System.out.println(s1 == s3); //true
System.out.println(s1 == s4); //true
//这四个变量都是指向的一个abc
```



注意基本类型的包装类和字符串有两种创建方式

​	常量式赋值，放在**栈内存**，将被常量化，常量池中相同值的指针都是一个

​	new对象创建，放在堆内存，不被常量化，每个new出来的都指向不同地方

```java
int i1 = 10;
Integer i2 = 1;                // 自动装箱
System.out.println(i1 == i2);   //true
// 自动拆箱  基本类型和包装类进行比较，包装类自动拆箱

Integer i3 = new Integer(10);
System.out.println(i1 == i3);  //true
// 自动拆箱  基本类型和包装类进行比较，包装类自动拆箱

System.out.println(i2 == i3); //false
// 两个对象比较，比较其地址。 
// i2是常量，放在栈内存常量池中，i3是new出对象，放在堆内存中

Integer i4 = new Integer(5);
Integer i5 = new Integer(5);
System.out.println(i1 == (i4+i5));   //true
System.out.println(i2 == (i4+i5));   //true
System.out.println(i3 == (i4+i5));   //true
// i4+i5 操作将会使得i4,i5自动拆箱为基本类型并运算得到10. 
// 基础类型10和对象比较, 将会使对象自动拆箱，做基本类型比较

Integer i6 = i4 + i5;  // +操作使得i4,i5自动拆箱，得到10，因此i6 == i2.
System.out.println(i1 == i6);  //true
System.out.println(i2 == i6);  //true
System.out.println(i3 == i6);  //false
```

比较时，有一方为基本类型，则拆箱进行值比较

若有一方进行算数运算，则比较时都拆箱进行值比较

若两边都是包装类，则进行指针比较，常量池的指针都是一个

注意Integer i6 = i4 + i5; 此时将会拆箱并寻找常量池



```java
String s0 = "abcdef";
String s1 = "abc";
String s2 = "abc";
String s3 = new String("abc");
String s4 = new String("abc");
System.out.println(s1 == s2); //true 常量池
System.out.println(s1 == s3); //false 一个栈内存，一个堆内存
System.out.println(s3 == s4); //false 两个都是堆内存

String s5 = s1 + "def";    //涉及到变量，故编译器不优化
String s6 = "abc" + "def"; //都是常量 编译器会自动优化成abcdef
String s7 = "abc" + new String ("def");//涉及到new对象，编译器不优化
System.out.println(s5 == s6); //false
System.out.println(s5 == s7); //false
System.out.println(s6 == s7); //false
System.out.println(s0 == s6); //true 

String s8 = s3 + "def";//涉及到new对象，编译器不优化
String s9 = s4 + "def";//涉及到new对象，编译器不优化
String s10 = s3 + new String("def");//涉及到new对象，编译器不优化
System.out.println(s8 == s9); //false
System.out.println(s8 == s10); //false
System.out.println(s9 == s10); //false
```

注意String s5 = s1 + "def";编译器看到s1是个变量，不会去推算是什么，对编译器而言s5也是未知的，编译器看不出来，不会优化，放在堆里

String s6 = "abc" + "def";是一个确定的值，确定的东西编译器来帮你优化，便和s0共享

"+"成分中有涉及到堆，则生成的对象都是放在堆里，不会优化。



## 不可变对象和字符串

不可变对象

​	一旦创建，这个对象的状态/值就不能被修改了。

​	其内在成员变量的值就不能修改了

​	有改变，就要clone/new一个对象出来进行修改

​	包括八个基本型别的包装类以及BigInt、BigDecimal、String等等



```java
String a = new String("abc");
String b = a;
a = "def";
```

此时的b输出依旧是abc，理解成a和b原来都指向abc，但是a最后单独指向了一块新的空间def，a和b现在已经分离，互不影响。



```java
public static void change(String b){b = "def"}

a = new String("abc");
change(a);
```

此时a依旧指向abc，a和b原来就是两个地方的指针，本来指向同一个地方abc，但是后来b断掉了，指向def，不会影响a。

不可变对象也是传指针(引用)

由于不可变，临时变量指向新内存，外部实参的指针不改动。



如何创建不可变对象

​	所有的属性都是final(固定)和private(无法访问)的

​	不提供setter方法

​	类是final的，所有的方法都是final的，不可以偷偷重写方法来修改里面的值

​	类中包含mutable对象，那么返回拷贝需要深度clone



优点

​	只读，线程安全。

​	并发读，提高性能。

​	可以重复使用。

缺点

​	修改对象会开辟新空间，制造垃圾，旧对象呗搁置，浪费空间，直到垃圾回收。



字符串比较

​	内容比较：equals方法

​	是否指向同一个对象：指针比较==



字符串加法

​	a = a + "def"; 由于String不可修改，效率差。

​	使用StringBuffer/StringBuilder类的append方法进行修改，这俩都是可变对象。

​	StringBuffer同步，线程安全，修改快速

​	StringBuilder不同步，线程不安全，修改更快。



# (4)package，import和classpath

## package和import

```java
package cn.edu.ecnu;
public class PackageExample
{
    
}
```

类全程cn.edu.ecnu.PackageExample（包名+类名），短名称PackageExample

引用类用全程引用，程序正文可以用短名称

PackageExample.java必须严格放在cn/edu/ecnu目录下

包名尽量唯一

包名和目录层次一样，但是包具体放在什么位置不重要，编译和运行的时候用classpath再指定



```java
package cn.edu.ecnu;
import cn.edu.ecnu.PackageExample;
```

```java
import cn.edu.ecnu.*
```

*代表这个目录下的所有的文件，但不包括子文件夹及其里面的内容（此时引入了某个目录下的所有 的类），不推荐，因为别人可能会加个同名的类导致冲突（此时需要明确a.Man，也不能同时import a.Man和b.Man，默认调用import里面的，重名需要全名）

如果两个类在同一个包下，import不写也可以

import必须放在package之后（package只能是第一句话），类定义之前

多个import顺序无关



## jar文件导出

是一组class文件的压缩包，没有源码，用于可执行程序文件的传播



## 命令行

支持多目录放置java，通过package，import，classpath，jar机制等配合使用，支持多处放置和调用java类

如c:\temp 创建 cn.com.test.Man.java，即创建在c:\temp\cn\com\test\Man.java

```java
package cn.com.test;
public class Man
{
    public static void main(String[] args)
    {
        System.out.println("hello");
    }
}
```

随便一个cmd窗口

编译 javac c:\temp\cn\com\test\Man.java 要**全路径**

​	要给出这个类所依赖的类，以及所依赖的类再次依赖的类所需的路径

运行 java -classpath .;c:\temp cn.com.test.Man 需要**类名全称**，无需扩展名

​	要给出这个类，以及被依赖类的路径总和

1. java是执行命令java.exe的简称
2. -classpath可以简写成-cp
3. ;隔开子目录（mac/Linus用:隔开）。.是当前目录下寻找类，找到一个就不找后面的了，有优先级。可以包含jar包，临时解压开来找类。路径有空格需要整体参数加一个双引号
4. 主执行类包名加类名的全称



c:\m1\A\B\C.java 我此处m是路劲，后面是包

​	A.B.C编译 javac c:\m1\A\B\C.java

​	运行 java -cp .;c:\m1 A.B.C

c:\m2\D\E\F.java	F引用了C

​	D.E.F编译 javac -cp .;c:\m1 c:\m2\D\E\F.java

​	运行 java -cp .;c:\m1;c:\m2 D.E.F

c:\m3\G\H.java	H引用了F，即间接也引用了C

​	G.H编译 javac -cp .;c:\m1;c:\m2 c:\m3\G\H.java

​	运行 java -cp .;c:\m1;c:\m2;c:\m3 G.H



## 访问权限

private

default

protected

public

总结以下几句话

同一个包里new，能访问default及以下

同一个包里继承，能访问default及以下

不同包里new，只能访问public

不同包里继承，能访问protected及以下



# (5)Java常用类

## 数字相关类

整数 Short，Integer，Long；

浮点数Float，Double

大数类BigInteger(大整数)，BigDecimal(大浮点数)

随机数Random

工具类Math

java.math包



大整数类

```java
import java.math.BigInteger;

public class BigIntegerTest {

	public static void main(String[] args) {
		BigInteger b1 = new BigInteger("123456789"); // 声明BigInteger对象
		BigInteger b2 = new BigInteger("987654321"); // 声明BigInteger对象
		System.out.println("b1: " + b1 +  ", b2:" + b2);
		System.out.println("加法操作：" + b2.add(b1)); // 加法操作
		System.out.println("减法操作：" + b2.subtract(b1)); // 减法操作
		System.out.println("乘法操作：" + b2.multiply(b1)); // 乘法操作
		System.out.println("除法操作：" + b2.divide(b1)); // 除法操作
		System.out.println("最大数：" + b2.max(b1)); // 求出最大数
		System.out.println("最小数：" + b2.min(b1)); // 求出最小数
		BigInteger result[] = b2.divideAndRemainder(b1); // 求出余数的除法操作
		System.out.println("商是：" + result[0] + "；余数是：" + result[1]);
		System.out.println("等价性是：" + b1.equals(b2));
		int flag = b1.compareTo(b2);
		if (flag == -1)
			System.out.println("比较操作: b1<b2");
		else if (flag == 0)
			System.out.println("比较操作: b1==b2");
		else
			System.out.println("比较操作: b1>b2");
	}
}
```



大浮点数

```java
import java.math.BigDecimal;
import java.math.BigInteger;



BigDecimal b1 = new BigDecimal("123456789.987654321"); // 声明BigDecimal对象
BigDecimal b2 = new BigDecimal("987654321.123456789"); // 声明BigDecimal对象
System.out.println("b1: " + b1 +  ", b2:" + b2);
System.out.println("加法操作：" + b2.add(b1)); // 加法操作
System.out.println("减法操作：" + b2.subtract(b1)); // 减法操作
System.out.println("乘法操作：" + b2.multiply(b1)); // 乘法操作
//需要指定位数，防止无限循环，或者包含在try-catch中
System.out.println("除法操作：" + b2.divide(b1,10,BigDecimal.ROUND_HALF_UP)); // 除法操作

System.out.println("最大数：" + b2.max(b1)); // 求出最大数
System.out.println("最小数：" + b2.min(b1)); // 求出最小数

int flag = b1.compareTo(b2);
if (flag == -1)
    System.out.println("比较操作: b1<b2");
else if (flag == 0)
    System.out.println("比较操作: b1==b2");
else
    System.out.println("比较操作: b1>b2");

System.out.println("===================");

//尽量采用字符串赋值
System.out.println(new BigDecimal("2.3"));
System.out.println(new BigDecimal(2.3));//这个不精准

System.out.println("===================");

BigDecimal num1 = new BigDecimal("10");
BigDecimal num2 = new BigDecimal("3");
//需要指定位数，防止无限循环，或者包含在try-catch中
BigDecimal num3 = num1.divide(num2, 3, BigDecimal.ROUND_HALF_UP);
System.out.println(num3);
```



随机数类

```java
import java.util.Random;


//第一种办法，采用Random类 随机生成在int范围内的随机数
Random rd = new Random();
System.out.println(rd.nextInt());
System.out.println(rd.nextInt(100)); //0--100的随机数
System.out.println(rd.nextLong());
System.out.println(rd.nextDouble());		
System.out.println("=========================");

//第二种，生成一个范围内的随机数 例如0到时10之间的随机数
//Math.random()返回一个[0,1)之间的double
System.out.println(Math.round(Math.random()*10));
System.out.println("=========================");


//JDK 8 新增方法
rd.ints();  //返回无限个int类型范围内的数据
int[] arr = rd.ints(10).toArray();  //生成10个int范围类的个数。
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
System.out.println("=========================");

arr = rd.ints(5, 10, 100).toArray();//5个10到100
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}

System.out.println("=========================");

arr = rd.ints(10).limit(5).toArray();
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
```



数字工具类

```java
System.out.println(Math.abs(-5));    //绝对值
System.out.println(Math.max(-5,-8)); //最大值
System.out.println(Math.pow(-5,2));  //求幂
System.out.println(Math.round(3.5)); //四舍五入
System.out.println(Math.ceil(3.5));  //向上取整
System.out.println(Math.floor(3.5)); //向下取整
```



## 字符串相关类

String类还有haseCode，matches，split，valueOf

```java
String a = "123;456;789;123 ";
System.out.println(a.charAt(0)); // 返回第0个元素
System.out.println(a.indexOf(";")); // 返回第一个;的位置
System.out.println(a.concat(";000")); // 连接一个新字符串并返回，a不变
System.out.println(a.contains("000")); // 判断a是否包含000
System.out.println(a.endsWith("000")); // 判断a是否以000结尾
System.out.println(a.equals("000")); // 判断是否等于000
System.out.println(a.equalsIgnoreCase("000"));// 判断在忽略大小写情况下是否等于000
System.out.println(a.length()); // 返回a长度
System.out.println(a.trim()); // 返回a去除前后空格后的字符串，中间空格去不了，a不变

String[] b = a.split(";"); // 将a字符串按照;分割成数组
for (int i = 0; i < b.length; i++) {
    System.out.println(b[i]);
}

System.out.println("===================");

System.out.println(a.substring(2, 5)); // 截取a的第2个到第5个字符 a不变
//这个数字可以理解成指向间隙的一个针头
System.out.println(a.replace("1", "a"));//也是全部替换
System.out.println(a.replaceAll("1", "a")); // replaceAll第一个参数是正则表达式

System.out.println("===================");

String s1 = "12345?6789";
String s2 = s1.replace("?", "a");
String s3 = s1.replaceAll("[?]", "a");
// 这里的[?] 才表示字符问号，这样才能正常替换。不然在正则中会有特殊的意义就会报异常
System.out.println(s2);
System.out.println(s3);
System.out.println(s1.replaceAll("[\\d]", "a")); //将s1内所有数字替换为a并输出，s1的值未改变。
```



可变字符串，这是可变对象了

```java
StringBuffer sb1 = new StringBuffer("123");
StringBuffer sb2 = sb1;

sb1.append("12345678901234567890123456789012345678901234567890");
System.out.println(sb2);  //sb1 和 sb2还是指向同一个内存的
```

StringBuffer 同步，性能好

StringBuilder 不同步，性能更好



注意length和capacity，capacity>=length

```java
//StringBuffer的的初始大小为（16+初始字符串长度）即capacity=16+初始字符串长度
//length 实际长度  capacity 存储空间大小
StringBuffer sb1 = new StringBuffer();
System.out.println("sb1 length: " + sb1.length());
System.out.println("sb1 capacity: " + sb1.capacity());
System.out.println("=====================");

StringBuffer sb2 = new StringBuffer("");
sb2.append("456");
System.out.println("sb2 length: " + sb2.length());
System.out.println("sb2 capacity: " + sb2.capacity());
System.out.println("=====================");

sb2.append("7890123456789");
System.out.println("sb2 length: " + sb2.length());
System.out.println("sb2 capacity: " + sb2.capacity());
System.out.println("=====================");

sb2.append("0");
System.out.println("sb2 length: " + sb2.length());
System.out.println("sb2 capacity: " + sb2.capacity());
//一旦length大于capacity时，capacity便在前一次的基础上加1后翻倍；
System.out.println("=====================");

//当前sb2length 20   capacity 40， 再append 70个字符 超过(加1再2倍数额)
sb2.append("1234567890123456789012345678901234567890123456789012345678901234567890");
System.out.println("sb2 length: " + sb2.length());
System.out.println("sb2 capacity: " + sb2.capacity());
//如果append的对象很长，超过(加1再2倍数额)，将以最新的长度更换

System.out.println("=====================");
sb2.append("0");
System.out.println("sb2 length: " + sb2.length());
System.out.println("sb2 capacity: " + sb2.capacity());
sb2.trimToSize();
System.out.println("=====after trime================");
System.out.println("sb2 length: " + sb2.length());
System.out.println("sb2 capacity: " + sb2.capacity());
```

即

初始化的capacity是16+初始化长度

如果length超过了capacity，则capacity+1再*2

如果一下子append太多，超过+1*2，则capacity更新成最新长度

trimToSize()删除空隙后，length和capacity一样长

最好还是StringBuffer的构造函数先前就预估一部分空间用来字符串操作



## 时间类

```java
import java.util.Calendar;

public class CalendarTest {
	
	Calendar calendar = Calendar.getInstance();
	
	public void test1() {
        // 获取年
        int year = calendar.get(Calendar.YEAR);
        // 获取月，这里需要需要月份的范围为0~11，因此获取月份的时候需要+1才是当前月份值
        int month = calendar.get(Calendar.MONTH) + 1;
        // 获取日
        int day = calendar.get(Calendar.DAY_OF_MONTH);

        // 获取时
        int hour = calendar.get(Calendar.HOUR);
        // int hour = calendar.get(Calendar.HOUR_OF_DAY); // 24小时表示
        // 获取分
        int minute = calendar.get(Calendar.MINUTE);
        // 获取秒
        int second = calendar.get(Calendar.SECOND);
        int ms=calendar.get(Calendar.MILLISECOND);
        
        // 星期，英语国家星期从星期日开始计算
        int weekday = calendar.get(Calendar.DAY_OF_WEEK);

        System.out.println("现在是" + year + "年" + month + "月" + day + "日" + hour
                + "时" + minute + "分" + second + "秒" + ms +"毫秒" + "星期" + weekday);
    }

    // 一年后的今天
    public void test2() {
        // 同理换成下个月的今天calendar.add(Calendar.MONTH, 1);
        calendar.add(Calendar.YEAR, 1);

        // 获取年
        int year = calendar.get(Calendar.YEAR);
        // 获取月
        int month = calendar.get(Calendar.MONTH) + 1;
        // 获取日
        int day = calendar.get(Calendar.DAY_OF_MONTH);

        System.out.println("一年后的今天：" + year + "年" + month + "月" + day + "日");
    }

    // 获取任意一个月的最后一天
    public void test3() {
        // 假设求6月的最后一天
        int currentMonth = 6;
        // 先求出7月份的第一天，实际中这里6为外部传递进来的currentMonth变量
        // 1
        calendar.set(calendar.get(Calendar.YEAR), currentMonth, 1);

        calendar.add(Calendar.DATE, -1);

        // 获取日
        int day = calendar.get(Calendar.DAY_OF_MONTH);

        System.out.println("6月份的最后一天为" + day + "号");
    }

    // 设置日期
    public void test4() {
        calendar.set(Calendar.YEAR, 2020);
        System.out.println("现在是" + calendar.get(Calendar.YEAR) + "年");

        calendar.set(2018, 7, 8);
        // 获取年
        int year = calendar.get(Calendar.YEAR);
        // 获取月
        int month = calendar.get(Calendar.MONTH)+1;
        // 获取日
        int day = calendar.get(Calendar.DAY_OF_MONTH);

        System.out.println("现在是" + year + "年" + month + "月" + day + "日");
        System.out.println(calendar.getTimeInMillis());
    }
    
    //add和roll的区别
    public void test5() {     

        calendar.set(2018, 7, 8);
        calendar.add(Calendar.DAY_OF_MONTH, -8);

        // 获取年
        int year = calendar.get(Calendar.YEAR);
        // 获取月
        int month = calendar.get(Calendar.MONTH)+1;
        // 获取日
        int day = calendar.get(Calendar.DAY_OF_MONTH);

        System.out.println("2018.8.8, 用add减少8天，现在是" + year + "." + month + "." + day);
        
        calendar.set(2018, 7, 8);
        calendar.roll(Calendar.DAY_OF_MONTH, -8);
        
        // 获取年
        year = calendar.get(Calendar.YEAR);
        // 获取月
        month = calendar.get(Calendar.MONTH)+1;
        // 获取日
        day = calendar.get(Calendar.DAY_OF_MONTH);

        System.out.println("2018.8.8, 用roll减少8天，现在是" + year + "." + month + "." + day);
    }
    
    
	public static void main(String[] args) {
		CalendarTest c = new CalendarTest();
		c.test1();
		System.out.println("============");
		c.test2();
		System.out.println("============");
		c.test3();
		System.out.println("============");
		c.test4();
		System.out.println("============");
		c.test5();

	}

}
```



## 格式化相关类

```java
import java.text.DecimalFormat;

DecimalFormat df1,df2;

System.out.println("整数部分为0的情况，0/#的区别");
// 整数部分为0 ， #认为整数不存在，可不写； 0认为没有，但至少写一位，写0
df1 = new DecimalFormat("#.00");
df2 = new DecimalFormat("0.00");

System.out.println(df1.format(0.1)); // .10  
System.out.println(df2.format(0.1)); // 0.10  

System.out.println("小数部分0/#的区别");
//#代表最多有几位，0代表必须有且只能有几位
df1 = new DecimalFormat("0.00");
df2 = new DecimalFormat("0.##");

System.out.println(df1.format(0.1)); // 0.10
System.out.println(df2.format(0.1)); // 0.1

System.out.println(df1.format(0.006)); // 0.01
System.out.println(df2.format(0.006)); // 0.01

System.out.println("整数部分有多位");
//0和#对整数部分多位时的处理是一致的 就是有几位写多少位
df1 = new DecimalFormat("0.00");
df2 = new DecimalFormat("#.00");

System.out.println(df1.format(2)); // 2.00
System.out.println(df2.format(2)); // 2.00

System.out.println(df1.format(20)); // 20.00
System.out.println(df2.format(20)); // 20.00

System.out.println(df1.format(200)); // 200.00
System.out.println(df2.format(200)); // 200.00
```

```java
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;

String strDate = "20081019" ;  

// 准备第一个模板，从字符串中提取出日期数字  
String pat1 = "yyyyMMdd" ;  
// 准备第二个模板，将提取后的日期数字变为指定的格式  
String pat2 = "yyyy年MM月dd日" ;  

SimpleDateFormat sdf1 = new SimpleDateFormat(pat1) ;        // 实例化模板对象  
SimpleDateFormat sdf2 = new SimpleDateFormat(pat2) ;        // 实例化模板对象  
Date d = null ;  
try{  
    d = sdf1.parse(strDate) ;   // 将给定的字符串中的日期提取出来  
}catch(Exception e){            // 如果提供的字符串格式有错误，则进行异常处理  
    e.printStackTrace() ;       // 打印异常信息  
}  
System.out.println(sdf2.format(d)) ;    // 将日期变为新的格式  
```



# (6)Java异常和异常处理

## 异常分类

Throwable：所有错误的祖先

Error：系统内部错误或者资源耗尽，不管。

Exception：程序有关的异常，重点关注。

​	RuntimeException：程序自身的错误。如空指针、数组越界、5/0。

​	非RuntimeException：外界相关的错误。如打开一个不存在的文件、加载一个不存在的类。



Unchecked Exception：编译器不会辅助检查的，需要程序员自己管的异常。包括Error子类和RuntimeException子类。如a/b

Checked Exception：**编译器会辅助检查的异常**，非RuntimeException的Exception子类，如读取不存在的文件。



总之

Checked Exception程序员必须处理，以发生后处理为主。编译器会辅助检查。

Unchecked Exception中的RuntimeException子类，程序必须处理，以预防为主。编译器不关心此类异常，不会辅助检查

Error的子类，不用处理



## 异常处理

允许用户及时保存结果，并以适当方式关闭程序

抓住异常，分析异常内容

控制程序返回到安全状态



try-catch-finally：一种保护代码正常运行的机制

异常结构：

​	try……catch（catch可以有多个，下同）

​	try……catch……finally

​	try……finally

try必须有，也不能只有try，catch和finally至少有一个。



try：正常业务逻辑代码

catch： try发生异常，将执行catch代码。

finally： try或catch执行结束以后，如果有finally肯定会被执行

```java
public class TryDemo {

	public static void main(String[] args) {
		try
		{
			int a = 5/2; //无异常，将不会执行catch，try结束以后直接进入finally
			System.out.println("a is " + a);
		}
		catch(Exception ex)
		{
			ex.printStackTrace();
		}
		finally
		{
			System.out.println("Phrase 1 is over");
		}
		
		try
		{
			int a = 5/0; //ArithmeticException，不会执行下一句输出，直接进入catch
			System.out.println("a is " + a);
		}
		catch(Exception ex)
		{
			ex.printStackTrace();//执行完了之后进入finally
		}
		finally
		{
			System.out.println("Phrase 2 is over");
		}
		
		try
		{
			int a = 5/0; //ArithmeticException
			System.out.println("a is " + a);
		}
		catch(Exception ex)
		{
			ex.printStackTrace();
			int a = 5/0;
            //再次发生异常，但finally还是会执行，finally执行完了之后将会报告上面的异常
		}
		finally
		{
			System.out.println("Phrase 3 is over");
		}
	}
}

/*
a is 2
Phrase 1 is over
java.lang.ArithmeticException: / by zero
Phrase 2 is over
Phrase 3 is over
	at TryDemo.main(TryDemo.java:21)
java.lang.ArithmeticException: / by zero
	at TryDemo.main(TryDemo.java:35)
Exception in thread "main" java.lang.ArithmeticException: / by zero
	at TryDemo.main(TryDemo.java:41)
*/
```

catch内部再次发生异常也不影响finally的正常运行

catch块有多个，每个都有不同的入口形参。当已发生的异常和某个catch块中的形参类型一致时，将会执行该catch块里的代码。如果没有一个匹配，catch也不会被触发。最后都进入finally块

进入catch块后，并不会返回到try发生异常的位置，也不会执行后续的catch块。一个异常只能进入一个catch块。

catch块的异常匹配是从上而下进行匹配，上面的优先级高。所以把小异常写在前，宽泛的异常写在末尾，如Exception(所有的异常都继承自Exception)

try-catch-finally每个模块里面也会发生异常，所以也可以在内部继续写一个**完整**的try结构，但是不可以嵌套，如try里try，catch里catch，finally里finally等。



方法可能存在异常的语句，但是没有能力处理它，可以用**throws**来声明和传播异常，在不同的类和不同的方法中处理。

调用带有throws异常(checked exception)的方法，要么处理这些异常，要么再次向外throws，直到main函数为止。

```java
public class ThrowsDemo
{
	public static void main(String [] args)
	{
		try
		{
			int result = new Test().divide( 3, 1 );
			System.out.println("the 1st result is" + result );
		}
		catch(ArithmeticException ex)
		{
			ex.printStackTrace();
		}
		int result = new Test().divide( 3, 0 );
        //main函数没有处理这个异常
		System.out.println("the 2nd result is" + result );
	}
}
class Test
{
	//ArithmeticException is a RuntimeException, not checked exception
	public int divide(int x, int y) throws ArithmeticException
	{
		int result = x/y;
		return x/y;
	}
}
```

```java

```



继承相关

一个方法被覆盖，覆盖它的方法必须抛出相同的异常，或者异常的子类

如果父类方法抛出多个异常，那么重写的子类方法必须抛出那些异常的子集，也就是不能抛出新的异常

```java
public class Father {
	public void f1() throws ArithmeticException
	{
		
	}
}

public class Son extends Father {
	public void f1() throws Exception
        //所抛出的异常不能超出父类规定的范围，不能大于父类f1()方法的异常，要兼容
        //也不能抛出新的异常，如IOException
	{
		//子类重写方法，
		
	}
}
```



## 自定义异常

Exception类是所有异常的父类

Exception继承自java.lang.Throwable，兄弟叫Error(更严重的问题，一般是系统层面的，无须程序员操心，程序只需要处理Exception)

**自定义异常需要继承Exception类或者其子类**

​	继承自Exception，就变成Checked Exception，IDE帮你检查

​	继承自RuntimeException，就变成了Unchecked Exception，不帮你检查

自定义重点在**构造函数**

​	调用父类Exception的message构造函数

​	可以自定义自己的成员变量

在程序中采用throws主动抛出异常

```java
public class MyException extends Exception {

	private String returnCode ;  //异常对应的返回码
	private String returnMsg;  //异常对应的描述信息
	
	public MyException() {
		super();
	}

	public MyException(String returnMsg) {
		super(returnMsg);
		this.returnMsg = returnMsg;
	}

	public MyException(String returnCode, String returnMsg) {
		super();
		this.returnCode = returnCode;
		this.returnMsg = returnMsg;
	}

	public String getReturnCode() {
		return returnCode;
	}

	public String getreturnMsg() {
		return returnMsg;
	}
}

public class MyExceptionTest {
	public static void testException() throws MyException {  
       throw new MyException("10001", "The reason of myException");  
         //在方法内部程序中，主动抛出异常采用throw关键字，
    }  
	
	public static void main(String[] args) /*throws MyException*/ {

		MyExceptionTest.testException();	
        	//将报错，因为有异常未被处理，要么像下面try-catch处理异常，要么像上面抛出异常
		
//		try {
//			MyExceptionTest.testException();
//		} catch (MyException e) {
//			e.printStackTrace();
//			System.out.println("returnCode:"+e.getReturnCode());
//			System.out.println("returnMsg:"+e.getreturnMsg());
//		}
	}
}
```

```java
public class DivideByMinusException extends Exception {
	int divisor;
	public DivideByMinusException(String msg, int divisor)
	{
		super(msg);
		this.divisor = divisor;
	}
	public int getDevisor()
	{
		return this.getDevisor();
	}
}
```

```java
public class Student {
	
	public int divide(int x, int y) 
	{
		return x/y;
        //ArithmeticException是属于Runtime，是Unchecked Exception，编译器不会辅助检查
	}
	
	public static void main(String[] args) throws DivideByMinusException{
		Student newton = new Student();
		//newton.divide2(5, 0);
		newton.divide5(5, -2);
	}	
	
	public int divide2(int x, int y)
	{
		int result;
		try
		{
			result = x/y;
			System.out.println("result is " + result);
		}
		catch(ArithmeticException ex)
		{
			System.out.println(ex.getMessage());
			return 0;
		}
		catch(Exception ex)
		{
			ex.printStackTrace();
			return 0;
		}
		return result;
	}
	
	//ArithmeticException is a unchecked exception,编译器可以不管，具体体现在divide4可以不管它，不用throws也不用try-catch 
	public int divide3(int x, int y) throws ArithmeticException
	{		
		return x/y;
	}
	
	public int divide4(int x, int y) 
	{		
//		try
//		{
//			return divide3(x,y);
//		}
//		catch(ArithmeticException ex)
//		{
//			ex.printStackTrace();
//			return 0;
//		}
		return divide3(x,y);  //尽管divide3报告异常，divide4无需处理。因为这个异常是unchecked exception
		//如果调用divide5(x,y);  那么就需要做try-catch处理，因为它抛出checked exception
	}
	
	public int divide5(int x, int y) throws DivideByMinusException
	{		
		try
		{
			if(y<0)
			{
				throw new DivideByMinusException("The divisor is negative", y);//先给下面的catch，不匹配，继续往外抛
			}
			return divide3(x,y);
		}
		catch(ArithmeticException ex)
		{
			ex.printStackTrace();
			return 0;
		}//catch可能兜不住所有的异常，有的异常可能不匹配，谁调用谁处理
	}
}
```



# (7)Java数据结构

## 数组

```java
int a[]; //a 还没有new操作  实际上是null，也不知道内存位置
int[] b; //b 还没有new操作  实际上是null，也不知道内存位置，中括号可交换
int[] c = new int[2]; //c有2个元素，都是0
c[0] = 10; c[1] = 20; //逐个初始化

int d[] = new int[]{0,2,4};//d有3个元素, 0,2,4，同时定义和初始化
int d1[] = {1,3,5};        //d1有3个元素, 1,3,5 同时定义和初始化

//注意声明变量时候没有分配内存，不需要指定大小，以下是错误示例
//int e[5];
//int[5] f;
//int[5] g = new int[5];
//int h[5] = new int[5];

//需要自己控制索引位置
for(int i=0;i<d.length;i++)	{
    System.out.println(d[i]);
}

//无需控制索引位置
for(int e : d) {
    System.out.println(e);
}
```

多维数组

```java
int a[][] = new int[2][3];
//不规则数组 
int b[][];
b = new int[3][];
b[0]=new int[3];
b[1]=new int[4];
b[2]=new int[5];

int k = 0;
for(int i=0;i<a.length;i++)
{
    for(int j=0;j<a[i].length;j++)
    {
        a[i][j] = ++k; 
    }
}

for(int[] items : a)
{
    for(int item : items)
    {
        System.out.print(item + ", ");
    }
    System.out.println();
}
```



## JCF

## 列表List

有序的Collection，允许重复和嵌套

{1，2，4，{5，6}，7}



ArrayList

数组实现的列表，非同步，利用索引快速定位访问，不适合指定位置的插入删除，元素填满容器是自动扩充容量大小的50%

只能装对象

```java
import java.util.ArrayList;
import java.util.Iterator;

ArrayList<Integer> al = new ArrayList<Integer>();
al.add(3);
al.add(2);
al.add(1);
al.add(4);
al.add(5);
al.add(6);

al.remove(3); //从0开始第3个删除
al.add(3,9); //从0开始第3个前面添加

ArrayList<Integer> al2 = new ArrayList<Integer>(1000);

Iterator<Integer> iter = al2.iterator(); //迭代器遍历 最慢
while(iter.hasNext())
{
    iter.next();
}

for(int i=0;i<al2.size();++i) //索引位置遍历 快
{
    al2.get(i);
}

for(Integer item : al2) //for-each遍历 更快
```



LinkedList

非同步，双向链表，可当作堆栈，队列等，顺序访问高效，随机访问差，删除和插入高效

```java
import java.util.LinkedList;
import java.util.Iterator;

LinkedList<Integer> ll = new LinkedList<Integer>();

ll.add(3);
ll.add(2);
ll.add(1);
ll.add(4);
ll.add(5);
ll.add(6);

ll.addFirst(9);
ll.add(3,10);
ll.remove(3);

//索引位置遍历 最慢
//迭代器遍历 很快
//for-each遍历 更快
```

Vector同步



## 集合Set

确定性

内容互异

无序性



HashSet

```java
import java.util.HashSet;
import java.util.Iterator;

HashSet<Integer> hs = new HashSet<Integer>();
hs.add(null); //可以容纳null元素
hs.add(1000);
hs.add(20);
hs.add(3);
hs.add(40000);
hs.add(5000000);
hs.add(3);                      //3 重复
hs.add(null);                   //null重复
//输出无序
System.out.println(hs.size());  //6

if(!hs.contains(6))
{
    hs.add(6);
}

hs.clear();

//交集
set1.retainAll(set2); //会修改set1

//for-each比迭代器快
```



LinkedHashSet

通过双向链表维护插入顺序，顺序是保留的，可以容纳null元素



以上两个类判定重复的原则

hashCode不同false

hashCode相同，判定equals方法，不同则false

注：hashCode和equals是Object类里有的，即所有类都有这两个方法

```java
class Dog {
    private int size;
 
    public Dog(int s) {
        size = s;
    }      
    public int getSize() {
		return size;
	}

	public boolean equals(Object obj2)   {
    	System.out.println("Dog equals()~~~~~~~~~~~");
    	if(0==size - ((Dog) obj2).getSize()) {
    		return true;
    	} else {
    		return false;
    	}
    }
    
    public int hashCode() {
    	System.out.println("Dog hashCode()~~~~~~~~~~~");
    	return size;
    }
    
    public String toString() {
    	System.out.print("Dog toString()~~~~~~~~~~~");
        return size + "";
    }
}

```



TreeSet

根据compareTo方法或者指定Comparator排序，不可以容纳null元素

继承自Comparable接口，比较CompareTo方法

```java
public class Tiger implements Comparable{
	private int size;
	 
    public Tiger(int s) {
        size = s;    
    }    
    
    public int getSize() {
		return size;
	}
    
	public int compareTo(Object o) {
    	System.out.println("Tiger compareTo()~~~~~~~~~~~");
        return size - ((Tiger) o).getSize();
    }
}
```



## 映射Map

K-V对



Hashtable

K和V都不允许为null，同步，无序，适合小数据



HashMap

K和V都允许为null，不同步，无序

```java
HashMap<Integer,String> hm =new  HashMap<Integer,String>();
hm.put(1, null); 
hm.put(null, "abc");  
hm.put(1000, "aaa");
hm.put(2, "bbb");
hm.put(30000, "ccc");
System.out.println(hm.containsValue("aaa"));
System.out.println(hm.containsKey(30000));
System.out.println(hm.get(30000));

hm.put(30000, "ddd");  //更新覆盖ccc
System.out.println(hm.get(30000));

hm.remove(2);
System.out.println("size: " + hm.size());

hm.clear();
System.out.println("size: " + hm.size());	
```

```java
HashMap<Integer,String> hm2 =new  HashMap<Integer,String>();
for(int i=0;i<100000;i++)
{
    hm2.put(i, "aaa");
}
traverseByEntry(hm2);
traverseByKeySet(hm2);	 //略快

public static void traverseByEntry(HashMap<Integer,String> ht)
{
    long startTime = System.nanoTime();
    System.out.println("============Entry迭代器遍历==============");
    Integer key;
    String value;
    Iterator<Entry<Integer, String>> iter = ht.entrySet().iterator();
    while(iter.hasNext()) {
        Map.Entry<Integer, String> entry = iter.next();
        // 获取key
        key = entry.getKey();
        // 获取value
        value = entry.getValue();
        //System.out.println("Key:" + key + ", Value:" + value);
    }
    long endTime = System.nanoTime();
    long duration = endTime - startTime;
    System.out.println(duration + "纳秒");
}


public static void traverseByKeySet(HashMap<Integer,String> ht)
{
    long startTime = System.nanoTime();
    System.out.println("============KeySet迭代器遍历=============="); 
    Integer key;
    String value;
    Iterator<Integer> iter = ht.keySet().iterator();
    while(iter.hasNext()) {
        key = iter.next();		    
        // 获取value
        value = ht.get(key);
        //System.out.println("Key:" + key + ", Value:" + value);
    }
    long endTime = System.nanoTime();
    long duration = endTime - startTime;
    System.out.println(duration + "纳秒");
}
```



LinkedHashMap

保存了记录的插入顺序



## 工具类

排序、搜索

Arrays类处理对象是数组

```java
public static void main(String[] args) {
    testSort();
    testSearch();
    testCopy();
    testFill();
    testEquality();
}
public static void testSort() {
    Random r = new Random();
    int[] a = new int[10];
    for(int i=0;i<a.length;i++)	{
        a[i] = r.nextInt();
    }
    System.out.println("===============测试排序================");
    System.out.println("排序前");
    for(int i=0;i<a.length;i++)	{
        System.out.print(a[i] + ",");
    }
    System.out.println();
    System.out.println("排序后");
    Arrays.sort(a);
    for(int i=0;i<a.length;i++)	{
        System.out.print(a[i] + ",");
    }
    System.out.println();		
}

public static void testSearch() {
    Random r = new Random();
    int[] a = new int[10];
    for(int i=0;i<a.length;i++)
    {
        a[i] = r.nextInt();
    }
    a[a.length-1] = 10000;
    System.out.println("===========测试查找============");
    System.out.println("10000 的位置是" + Arrays.binarySearch(a, 10000));
    //有多个重复元素只会随机返回一个
}

public static void testCopy() {
    Random r = new Random();
    int[] a = new int[10];
    for(int i=0;i<a.length;i++)
    {
        a[i] = r.nextInt();
    }
    int[] b = Arrays.copyOf(a, 5);
    System.out.println("===========测试拷贝前五个元素============");
    System.out.print("源数组：");
    for(int i=0;i<a.length;i++)
    {
        System.out.print(a[i] + ",");
    }
    System.out.println();
    System.out.print("目标数组：");
    for(int i=0;i<b.length;i++)
    {
        System.out.print(b[i] + ",");
    }
    System.out.println();
}
public static void testFill() {
    int[] a = new int[10];
    Arrays.fill(a, 100);
    Arrays.fill(a, 2, 8, 200);
    System.out.println("===========测试批量赋值============");
    System.out.print("数组赋值后：");
    for(int i=0;i<a.length;i++)
    {
        System.out.print(a[i] + ",");
    }
    System.out.println();
}
public static void testEquality() {
    int[] a = new int[10];
    Arrays.fill(a, 100);
    int[] b = new int[10];
    Arrays.fill(b, 100);		
    System.out.println(Arrays.equals(a, b));
    b[9] = 200;
    System.out.println(Arrays.equals(a, b));
}
```



Collections类

对象是Collection及其子类，聚焦于List

```java
ArrayList<Integer> list = new ArrayList<Integer>();
list.add(1);
list.add(12);
list.add(2);
list.add(19);

// 排序
Collections.sort(list);
// 检索
System.out.println("元素所在的索引值是：" + Collections.binarySearch(list, 12));
//最大最小
System.out.println("最大值：" + Collections.max(list));
System.out.println("最小值：" + Collections.min(list));
Collections.reverse(list); //翻转不需要用到排序

Collections.fill(list, 100); //全部赋值为100
```



Comparable接口里面只有compareTo方法

大于返回1，小于返回-1，等于返回0

若对象不可更改，则新建Comparator，实现compare方法

比较器Comparator作为参数提交给工具类的sort方法

```java
import java.util.Arrays;

public class Person implements Comparable<Person> {
	String name;
	int age;

	public String getName() {
		return name;
	}

	public int getAge() {
		return age;
	}

	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}

	public int compareTo(Person another) {
		int i = 0;
		i = name.compareTo(another.name); // 使用字符串的比较
		if (i == 0) {
			// 如果名字一样,比较年龄, 返回比较年龄结果
			return age - another.age;
		} else {
			return i; // 名字不一样, 返回比较名字的结果.
		}
	}

	public static void main(String... a) {
		Person[] ps = new Person[3];
		ps[0] = new Person("Tom", 20);
		ps[1] = new Person("Mike", 18);
		ps[2] = new Person("Mike", 20);

		Arrays.sort(ps);
		for (Person p : ps) {
			System.out.println(p.getName() + "," + p.getAge());
		}
	}
}
```

```java
public class Person2 {
	private String name;
    private int age;
	public String getName() {
		return name;
	}
	public int getAge() {
		return age;
	}

    public Person2(String name, int age)
    {
    	this.name = name;
    	this.age = age;
    }
}


import java.util.Arrays;
import java.util.Comparator;

public class Person2Comparator  implements Comparator<Person2> {
	public int compare(Person2 one, Person2 another) {
		int i = 0;
		i = one.getName().compareTo(another.getName());
		if (i == 0) {
			// 如果名字一样,比较年龄,返回比较年龄结果
			return one.getAge() - another.getAge();
		} else {
			return i; // 名字不一样, 返回比较名字的结果.
		}
	}

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Person2[] ps = new Person2[3];
		ps[0] = new Person2("Tom", 20);
		ps[1] = new Person2("Mike", 18);
		ps[2] = new Person2("Mike", 20);

		Arrays.sort(ps, new Person2Comparator());
		for (Person2 p : ps) {
			System.out.println(p.getName() + "," + p.getAge());
		}
	}
}
```



# (8)Java文件读写

## 文件系统及Java文件基本操作

java.io.File是文件和目录的重要类，目录也使用File类进行表示

```java
import java.io.*;
public class FileAttributeTest{
  public static void main(String[] args){
	//创建目录
	File d=new File("c:/temp");
	if(!d.exists())
	{
		d.mkdirs();  //mkdir 创建单级目录  mkdirs 连续创建多级目录
	}
	System.out.println("Is d directory? " + d.isDirectory());
	
	//创建文件  
    File f=new File("C:/temp/abc.txt");    
    if(!f.exists())
    {    	
      try
      {
        f.createNewFile(); //创建abc.txt
      }
      catch(IOException e){ //可能会因为权限不足或磁盘已满报错
    	  e.printStackTrace();
      }
    }  
    
    //输出文件相关属性
    System.out.println("Is f file? " + f.isFile());
    System.out.println("Name: "+f.getName());
    System.out.println("Parent: "+f.getParent());//上一层目录路径
    System.out.println("Path: "+f.getPath());//文件的全路径
    System.out.println("Size: "+f.length()+" bytes");//大小
    System.out.println("Last modified time: "+f.lastModified()+"ms");//最后一次修改时间，从1970年开始的毫秒数
    
    //遍历d目录下所有的文件信息
    System.out.println("list files in d directory");
    File[] fs = d.listFiles();  //列出d目录下所有的子文件，不包括子目录下的文件
    for(File f1:fs)
    {
    	System.out.println(f1.getPath());
    }
    
    //f.delete(); //删除此文件
    //d.delete(); //删除目录
  }
}
```



Path的用法

```java
import java.io.File;
import java.nio.file.FileSystems;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;

public class PathTest {
	public static void main(String[] args) {
		// Path 和 java.io.File 基本类似
		// 获得path方法一,c:/temp/abc.txt
		Path path = FileSystems.getDefault().getPath("c:/temp", "abc.txt");
        //目录名和文件名隔开
		System.out.println(path.getNameCount());

		// 获得path方法二，用File的toPath()方法获得Path对象
		File file = new File("c:/temp/abc.txt");
		Path pathOther = file.toPath();
		// 比较文件是否是同一个，0,说明这两个path是相等的
		System.out.println(path.compareTo(pathOther));

		// 获得path方法三，get()获取文件的Path句柄
		Path path3 = Paths.get("c:/temp", "abc.txt");
		System.out.println(path3.toString());

		// 合并两个path
		Path path4 = Paths.get("c:/temp");
		System.out.println("path4: " + path4.resolve("abc.txt"));

		if (Files.isReadable(path)) {
			System.out.println("it is readable");
		} else {
			System.out.println("it is not readable");
		}
	}
}
```



NIO功能

```java

```



## java.io包

关于文件内容的输入和输出类，访问和操作文件内容

文件可能很大，Java读写文件只能以数据流的形式

java.io包中

​	节点类：直接对文件进行读写

​		InputStream，OutputStream（字节）

​		Reader，Writer（字符）

​	包装类：

​		转化类：字符/字节/数据类型的转化类

​			InputStreamReader：文件读取时，字节转化成Java能理解的字符

​			OutputStreamWriter：Java将字符转化为字节输入到文件中

​		装饰类：装饰节点类

​			DataInputStream，DataOutputStream：封装数据流

​			BufferedInputStream，BufferOutputStream：缓存字节流

​			BufferedReader，BufferedWriter：缓存字符流



## 文本文件读写

写文件

​	创建文件，写入数据，关闭文件

​	FileOutputStream(往文件写字节)，OutputStreamWriter(字节和字符转化)，BufferedWriter(写缓冲类，加速写操作)（write写，newLine换行）

​	try-resource语句，自动关闭资源。文件程序被打开就处于锁定状态，防止其他程序访问。如果文件没有关闭，别人就写不进去

​	关闭最外层的数据流，就会把其上所有的数据流关闭

```java
import java.io.*;

public class TxtFileWrite {
	public static void main(String[] args) {
		writeFile1();
		System.out.println("===================");
		//writeFile2(); // JDK 7及以上才可以使用
	}

	public static void writeFile1() {
		FileOutputStream fos = null;
		OutputStreamWriter osw = null;
		BufferedWriter bw = null;
		try {
			fos = new FileOutputStream("c:/temp/abc.txt"); // 节点类
			osw = new OutputStreamWriter(fos, "UTF-8"); // 转化类
			//osw = new OutputStreamWriter(fos); // 转化类
			bw = new BufferedWriter(osw); // 装饰类
			// br = new BufferedWriter(new OutputStreamWriter(new
			// FileOutputStream("c:/temp/abc.txt")))
			bw.write("我们是");
			bw.newLine();
			bw.write("Ecnuers.^^");
			bw.newLine();
		} catch (Exception ex) {
			ex.printStackTrace();
		} finally {
			try {
				bw.close(); // 关闭最后一个类，会将所有的底层流都关闭
                //bw包含osw，osw包含fos，最外层就是bw
			} catch (Exception ex) {
				ex.printStackTrace();
			}
		}
	}

	public static void writeFile2() {
		//try-resource 语句，自动关闭资源，就不需要写finally进行关闭了
		try (BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(new FileOutputStream("c:/temp/abc.txt")))) {
			bw.write("我们是");
			bw.newLine();
			bw.write("Ecnuers.^^");
			bw.newLine();
		} catch (Exception ex) {
			ex.printStackTrace();
		}
	}
}
```



读文件

​	打开文件，逐行读入主句，关闭文件

​	FileInputStream，InputStream，BufferedReader（readLine）

​	try-resource关闭资源，关闭最外层数据流

```java
import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.InputStreamReader;
import java.util.HashMap;

public class TxtFileRead {
	public static void main(String[] args) {
		readFile1();
		System.out.println("===================");
		//readFile2(); //JDK 7及以上才可以使用
	}

	public static void readFile1() {
		FileInputStream fis = null;	//节点类，读字节
		InputStreamReader isr = null;	//转化类，字节到字符
		BufferedReader br = null;	//装饰类，缓冲区读入字符
		try {
			fis = new FileInputStream("c:/temp/abc.txt"); // 节点类
			isr = new InputStreamReader(fis, "UTF-8"); // 转化类，写是UTF-8则读也需要保持一致用UTF-8
			//isr = new InputStreamReader(fis);
			br = new BufferedReader(isr); // 装饰类
			// br = new BufferedReader(new InputStreamReader(new
			// FileInputStream("c:/temp/abc.txt")))
			String line;
			while ((line = br.readLine()) != null) // 每次读取一行
			{
				System.out.println(line);
			}
		} catch (Exception ex) {
			ex.printStackTrace();
		} finally {
			try {
				br.close(); // 关闭最后一个类，会将所有的底层流都关闭
			} catch (Exception ex) {
				ex.printStackTrace();
			}
		}
	}

	public static void readFile2() {
		String line;
		//try-resource 语句，自动关闭资源
		try (BufferedReader in = new BufferedReader(new InputStreamReader(new FileInputStream("c:/temp/abc.txt")))) {
			while ((line = in.readLine()) != null) {
				System.out.println(line);
			}
		}
		catch(Exception ex)
		{
			ex.printStackTrace();
		}
	}
}
```



## 二进制文件的读写

​	狭义上说，采用字节编码，非字符编码的文件

​	广义上说，一切文件都是二进制文件

​	用记事本等无法打开/阅读



写文件

​	创建文件，写入数据，关闭文件

​	FileOutputStream，BufferedOutputStream，DataOutputStream（flush，write/writeBoolean/writeChars/WriteDouble/WriteInt/WriteUTF

​	……

```java
import java.io.*;
public class BinFileWrite{
  public static void main(String[] args) throws Exception{
	writeFile();
    System.out.println("done.");
  }
  
  public static void writeFile() {
	  FileOutputStream fos = null;
	  DataOutputStream dos = null;
	  BufferedOutputStream bos = null;
		try {
			fos = new FileOutputStream("c:/temp/def.dat"); // 节点类
			bos = new BufferedOutputStream(fos); // 装饰类
			dos = new DataOutputStream(bos); // 转化类		
			
			dos.writeUTF("a"); //Unicode
			dos.writeInt(20);
			dos.writeInt(180);
			dos.writeUTF("b");
		} catch (Exception ex) {
			ex.printStackTrace();
		} finally {
			try {
				dos.close(); // 关闭最后一个类，会将所有的底层流都关闭
			} catch (Exception ex) {
				ex.printStackTrace();
			}
		}
	}
}
```



读文件

​	打开文件，读入数据，关闭文件

​	FileInputStream，BufferedInputStream，DataInputStream（read/readBoolean/readChar/readDouble/readInt/readUnicode）

```java
import java.io.*;
public class BinFileRead{
  public static void main(String[] args) throws Exception{
	  readFile();
  }
  public static void readFile() {
		//try-resource 语句，自动关闭资源
		try (DataInputStream dis = new DataInputStream(new BufferedInputStream(new FileInputStream("c:/temp/def.dat")))) {
			String a, b;
		    int c, d;
		    a=dis.readUTF();
		    c=dis.readInt();
		    d=dis.readInt();
		    b=dis.readUTF();
		    System.out.println("a: "+a);
		    System.out.println("c: "+c);
		    System.out.println("d: "+d);
		    System.out.println("b: "+b);
		}
		catch(Exception ex)
		{
			ex.printStackTrace();
		}
	}
}
```

