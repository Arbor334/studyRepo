## Java基础

<font color="red">三大基本特征：封装、继承、多态</font>

### :musical_score: Scanner类

> <font color="red">Scanner 类的功能：可以实现键盘输入数据，到程序当中</font>
>
> 引用类型的一般使用步骤：
>
> 1、导包
>
> import 包路径.类名称
>
> 如果需要使用的目标类，和当前类位于同一个包下，则可以省略导包语句
>
> 只有java.lang包下的内容不需要导包，其他的包都需要import语句
>
> 2、创建
>
> 类名称 对象名=new 类名称();
>
> 3、使用
>
> 对象名.成员方法名();

```java
//导包
import java.util.Scanner;

public class demo07Scanner {
    public static void main(String[] args) {
//        创建
//        备注：System.in 代表从键盘输入
        Scanner sc=new Scanner(System.in);
//       获取键盘输入的一个int数字
        int num=sc.nextInt();
//        获取键盘输入的一个字符串
        String str=sc.next();
    }
}
```

### :musical_note: 匿名对象

```java
创建对象的标准格式
类名称 对象名=new 类名称();
匿名对象就是只有右边的对象，没有左边的名字和赋值运算符
new 类名称();

注意事项：
匿名对象只能使用唯一的一次，下次再用不得不再创建一个新对象
使用建议：
如果确定有一个对象只需要使用唯一的一次，就可以使用匿名对象
```

```java
public class demo08Anonymous {
    public static void main(String[] args) {
//        标准格式
        Person one=new Person();
        one.name="hhh";
        one.showName();//显示的是 hhh
//        匿名对象格式
        new Person().name="qqqq";
        new Person().showName();  //显示的是 null 因为这个是第三个对象
    }

}
```

#### :ear_of_rice: 使用匿名对象作为方法的参数和返回值

```java
package day01;
import java.util.Scanner;

public class demo09Anonymous {
    public static void main(String[] args) {
        //    普通使用方式
        Scanner sc=new Scanner(System.in);
        int num1=sc.nextInt();
//    使用匿名对象的方式
        int num2=new Scanner(System.in).nextInt();
        System.out.println("你输入的数字是"+num2);
//        使用一般写法传入参数
        Scanner sc2=new Scanner(System.in);
        methodParam(sc);
//        使用匿名对象来进行传参
        methodParam(new Scanner(System.in));
//
        Scanner sc3=methodReturn();
        int num3=sc3.nextInt();
        System.out.println("输入的数是"+num3);
    }
    public static void methodParam(Scanner sc){
        int num=sc.nextInt();
        System.out.println("输入的是"+num);
    }
    public static Scanner methodReturn(){
        return new Scanner(System.in);
    }
}

```

### :mag_right: Random类

```java
Random类用来生成随机数字，使用起来也是三个步骤
1、导包
 import java.util.Random
2、创建
Random r=new Random();
3、使用
获取一个随机的int数字(范围是int所有范围，有正负两种) int num=r.nextInt();
获取一个随机的int数字(参数代表了范围，左闭右开区间) int num=r.nextInt(3);
实际上代表的含义是[0,3);

创建一个1-n的随机数范围
int num=r.nextInt(n)+1;
```

```java
import java.util.Random;
public class demo10Random {
    public static void main(String[] args) {
        Random r=new Random();
        int num=r.nextInt();
        System.out.println("随机数是多少"+num);
        Random r1=new Random();
        for (int i = 0; i < 100; i++) {
            int num1=r1.nextInt(10);
            System.out.println("输出为："+num1);
        }
    }
}
```

### :cloud_with_snow: String类

> java.lang.String 类代表字符串
>
> API 当中说，Java程序中素有字符串字面值（如：“abc”）都作为此类的实例实现
>
> 其实就是说：程序当中所有的双引号字符串，都是String类的对象。（就算是没有new  也是）

#### 字符串的特点

- 1、字符串的内容永不可变【重点】
- 2、正是因为字符串不可变，所以字符串是可以共享使用的
- 3、字符串效果上相当于char[]字符数组，但是底层原理是byte[]字节数组

```java
/*
创建字符串的3+1种方式
三种构造方法：
1、public String()  创建一个空白字符串 不含任何内容
2、public String(char[] array) 根据字符数组的内容，来创建对应的字符串
3、public String(byte[] array) 根据字节数组的内容，来创建对应的字符串

一种直接创建：


*/
public class demo01String {
    public static void main(String[] args) {
//        使用空参创建
        String str1=new String();//小括号留空，说明字符串什么内容都没有
        System.out.println("str1的内容是"+str1);
//        根据字符数组创建字符串
        char[] charArray={'a','b','c'};
        String str2=new String(charArray);
        System.out.println(str2);
//        根据字节数组创建字符串
        byte[] byteArray={97,98,99};
        String str3=new String(byteArray);
        System.out.println(str3);
//        直接创建
        String str4="aaaa";
        System.out.println(str4);


    }
}
```

#### :electric_plug: 字符串常量池

> 字符串常量池：程序中直接写上的双引号字符串，就在字符串常量池中
>
> 对于基本类型来说，==是进行数值的比较
>
> 对于引用类型来说，==是进行[地址值]的比较

```java
public class demo02String {
    public static void main(String[] args) {
        String str1="abc";
        String str2="abc";
        char[] charArray={'a','b','c'};
        String str3=new String(charArray);
        System.out.println(str1==str2);//true
        System.out.println(str1==str3);//false
        System.out.println(str2==str3);//false
    }
}
```



![](https://i.loli.net/2021/01/13/OYLhKdjJzk9NXbQ.png)

#### 字符串比较的相关方法

```java
==是进行对象的地址值比较，如果确实需要字符串的内容比较，可使用两个方法
public boolean equals(Object obj):参数可以是任何对象，只有参数是一个字符串并且内容相同的才会给true，否则返回false；
注意事项：
1、任何对象都能用Object进行接收
2、equals方法具有对称性，a.equals(b) 和 b.equals(a)效果一样
3、如果比较双方一个常量一个变量，推荐把常量字符串写在前面。
推荐："abc".equals(str)  不推荐：str.equals("abc")

public boolean equalsIgnoreCase(String str):忽略大小写 进行内容比较
只区分英文大小写，不区分其他大小写


```

```java
public class demo03StringEquals {
    public static void main(String[] args) {
        String str1="abc";
        String str2="abc";
        char[] charArray={'a','b','c'};
        String str3=new String(charArray);

        System.out.println(str1.equals(str2));
        System.out.println(str2.equals(str3));
        System.out.println(str3.equals("abc"));
        System.out.println("abc".equals(str1));

        String str4="ABC";
        System.out.println(str1.equals(str4));
        String str5="abc";
        System.out.println("abc".equals(str5));//推荐
        System.out.println(str5.equals("abc"));//不推荐  如果str5是空指针的话 会报错NullPointerException



        String s1="123一java";
        String s2="123壹java";
        System.out.println(s1.equalsIgnoreCase(s2));
    }
}

```

#### 字符串的获取相关的常用方法：

- public int length()：获取字符串当中含有的字符个数，拿到字符串长度
- public String concat(String str):将当前字符串和参数字符串拼接成一个，然后返回新的字符串
- public char charAt(int index): 获取指定索引位置的单个字符。(索引从0开始)
- public int  indexOf(String str):查找参数字符串在本字符串当中首次出现的索引，没有就返回-1

#### 字符串的截取

- public String substring(int index):截取从参数位置一直到字符串末尾，返回新字符串
- public String substring(int begin,int end):截取从begin开始，一直到end结束，中间的字符串[begin,end)

#### 字符串的转换

- public char[] toCharArray():将当前字符串拆分成为字符数组作为返回值
- public byte[] getBytes():获得当前字符串底层的字节数组、
- public String replace(CharSequence oldString,CharSequence newString):
- 将所有出现的老字符串替换为新字符串，返回替换后的新的字符串

```java
public class demo04StringConvert {
    public static void main(String[] args) {
//        转换为字符数组
        char[] chars="hello".toCharArray();
        System.out.println(chars[0]);
        System.out.println(chars.length);
//        转换为字节数组
        byte[] bytes="abc".getBytes();
        System.out.println(bytes[0]);
        System.out.println(bytes.length);
//      字符串的内容替换
        String str1="hhhhhssssjjjshhss";
        String str2=str1.replace("hhh","aaa");
        System.out.println(str2);

    }
}
```

#### 字符串的分割方法

- public String[] split(String regex)：按照参照的规则，将字符串切分为若干部分

```java
public class demo05StringSplit {
    public static void main(String[] args) {
        String str1="aaa,bbb,ccc";
        System.out.println(str1);
        String[] array1=str1.split(",");
        for (int i = 0; i < array1.length; i++) {
            System.out.println(array1[i]);
        }
    }
}
```



### :car: 数组

>:a: 数组的概念：是一种容器，可以同时存放多个数据值
>:b: 数组的特点：
> 1、数组是一种引用数据类型
> 2、数组当中的多个数据，类型必须统一
> 3、数组的长度在程序运行期间不能改变
>
>:asterisk: 数组的初始化：
>   在内存当中创建一个数组，并且向其中赋予一些默认值
>:astonished: 两种创建的初始化方式：
>       1、动态初始化（指定长度）   数据类型[] 数组名称=new  数据类型[数组长度];
>       2、静态初始化（指定内容）   数据类型[] 数组名称=new  数据类型[] {元素1，元素2...};
>:biking_woman: 还有省略的静态初始化
>数据类型[] 数组名称={元素1，元素2，元素3..};
>
>
>直接打印数组名称，得到的是数组对应的内存地址哈希值
>
>访问数组元素的格式：数组名称[索引值]
>索引值：从0开始
>
>使用动态初始化的时候，其中的元素将会自动拥有一个默认值
>规则如下：
>`整数     默认为0`
>`浮点     默认为0.0`
>`字符类型 默认为 '\u0000'`
>`布尔类型 默认为false`
>`引用类型 默认为null`

- 代码

```java
public class demo05Array {
    public static void main(String[] args) {
         CreateStaticArray();
        CreateStaticArray2();
    }
    public  static void CreateDynamicArray() {
//        创建一个数组，里面可以存放100个int数据
        int[] arrayA=new int[100];
//        创建一个数组，存放5个字符串
        String[] arrayB=new String[5];
//        动态初始化可以拆分为两个步骤
        int[]  arrayC;
        arrayC=new int[6];
    }

    public static void CreateStaticArray(){
//        创建一个数组，里面装的全是int数字，具体是 1,2,3,4
        int[] arrayC=new int[]{1,2,3,4};
//        创建一个数组，里面存的是String类型
        String[] arrayD=new String[] {"hello","ni","how"};
//        静态初始化的标准格式可以拆分成为两个步骤
        int[] arrayB;
        arrayB=new int[]{1,2,3,4,56};
        System.out.println(arrayC[1]);
        System.out.println(arrayD[1]);

    }
   public static  void CreateStaticArray2(){
//        创建一个省略的静态数组
       int[] arrayE={1,2,3,4};
//       静态初始化一旦使用省略格式，就不能拆分成为两个步骤了
       System.out.println(arrayE[1]);

   }
```

#### :eagle: 获取数组的长度

> 数组名.length   即可

### :dagger: 对象数组

```java
/*
定义一个数组，用来存储3个Person对象

数组有一个缺点：一旦创建，运行期间长度不可以发生改变
*/
public class demo11Array {
    public static void main(String[] args) {
//        首先创建一个长度为3的数组，里面用来存放Person类型的对象
        Person[] array=new Person[3];
        Person one=new Person("迪丽热巴",19);
        Person two=new Person("古力娜扎",29);
        Person three=new Person("马尔扎哈",39);
//        将one当中的地址值赋值到数组中得0号元素位置
        array[0]=one;
        array[1]=two;
        array[2]=three;
        System.out.println(array[0]);//地址值
        System.out.println(array[1]);//地址值
        System.out.println(array[2]);//地址值
        System.out.println(array[0].getName());//迪丽热巴

    }
}
```



### :full_moon: 方法的注意事项

> 1、方法应该定义在类当中，但是不能在方法中在定义方法，不能嵌套
>
> 2、方法定义的前后顺序无所谓
>
> 3、方法定义之后不会执行，如果希望执行，一定要调用
>
> 4、如果方法有返回值，那么必须写上“return 返回值”，不能没有
>
> 5、return 后面的返回值必须和方法的返回值类型对应起来
>
> 6、对于一个void没有返回值的方法，不能写return后面的返回值，只能写return自己
>
> 7、对于方法当中最有一行return可以省略不写
>
> 8、一个方法当中可以由多个return语句，但是必须保证同时只有一个被执行到

### :basketball_woman: 方法重载

>  方法重载（overload）:多个方法的名称一样，但是参数列表不一样
>
> 好处：只需要记住唯一一个方法名称，就可以实现类似的多个功能
>
> <font color="red">注意事项：</font>
>
> 方法重载与下列因素相关：
>
> 1、参数个数不同
>
> 2、参数类型不同
>
> 3、参数的多类型顺序不同
>
> 方法重载与下列因素无关：
>
> 1、与参数的名称无关
>
> 2、与方法的返回值类型无关

```java
public class demo06Overload {
    public static void main(String[] args) {
        System.out.println(sum(10,20));
        System.out.println(sum(10,20,30));
      ystem.out.println(sum(10.0,20.0,30.0));
      
    }
    public static int sum(int a,int b){
        return a+b;
    }
    public static int sum(int a,int b,int c){
        return a+b+c;
    }
   public static double sum(double a,double b,double c){
     return a+b+c;
   }
}
```

### :clinking_glasses: 抽象方法和抽象类

#### 抽象方法和抽象类的格式

```java
/*
抽象方法：就是加上abstract 关键字，然后去掉大括号，直接分号结束
抽象类： 抽象方法所在的类，必须是抽象类才行，在class之前加上abstract就行
*/
public abstract class Animal {
//   这就是一个抽象方法，代表吃东西，但是具体吃什么，不确定
    public abstract void eat();
//    这是一个普通方法
    public void normalMethod(){

    }
}
```

#### 抽象方法和抽象类的使用

> 1、不能直接创建new 抽象类对象。
>
> 2、必须用一个子类来继承抽象类
>
> 3、子类必须覆盖重写父类当中所有的抽象方法
>
> 实现：  子类去掉抽象方法的abstract关键字，然后补上方法体大括号
>
> 4、创建子类对象进行使用

- demoMain类

  ```java
  public class demoMain {
      public static void main(String[] args) {
  //        Animal animal=new Animal(); 错误写法，不能直接创建抽象类对象
          cat cat=new cat();
          cat.eat();
      }
  }
  
  ```

- Animal类

  ```java
  public abstract class Animal {
   public abstract void eat();
  }
  ```

- cat类

  ```java
  public class cat extends Animal{
      @Override
      public void eat(){
          System.out.println("猫吃鱼");
      }
  }
  ```

#### 抽象类和抽象方法的注意事项

- 抽象类不能创建对象，如果创建，编译无法通过而报错，只能创建其非抽象子类的对象
- 抽象类中，可以有构造方法，是供子类创建对象时，初始化父类成员使用的
- 抽象类中，不一定包含抽象方法，但是有抽象方法的类必须是抽象类
- 抽象类的子类，必须重写抽象父类中所有的抽象方法，否则，编译无法通过而报错。除非该子类也是抽象类

- Animal类(爷爷辈)

  ```java
  public abstract class Animal {
   public abstract void eat();
   public abstract void sleep();
  }
  ```

- Dog类(儿子辈)

  ```java
  //  Dog 也是一个抽象类了
  public abstract  class Dog  extends Animal{
      @Override
      public void eat() {
          System.out.println("吃骨头");
      }
  }
  ```

- Dog2ha类(孙子辈)

  ```java
  public class Dog2ha  extends Dog{
      @Override
      public void sleep() {
          System.out.println("呼呼呼。。");
      }
  }
  ```

- main方法

  ```java
  public class demoMain {
      public static void main(String[] args) {
  //        Animal animal=new Animal(); 错误写法，不能直接创建抽象类对象
  //       Dog dog=new Dog();错误写法
          Dog2ha ha=new Dog2ha();
          ha.eat();
          ha.sleep();
  //        吃骨头
  //        呼呼呼。。
      }
  }
  ```



### :memo: Java内存

> Java 的内存需要划分为:five: 个部分
>
> :one: 栈（Stack）: 存放的都是方法中的局部变量。<font color="red">方法的运行一定要在栈当中</font>
>
> ​						`局部变量`：方法的参数，方法{}内部的变量
>
> ​						`作用域`：一旦超出作用域，立刻从栈内存当中消失
>
> :two:堆（Heap）：<font color="red">凡是`new`出来的东西，都在堆中</font>
>
> ​						堆内存中的东西都有一个地址值：16进制
>
> ​						堆内存里面的数据，都有默认值
>
> :three:<font color="red"> 方法区（Method Area）</font>:存储`.class`相关信息，包含方法的信息。
>
> :four: 本地方法栈（Native Method Stack）:与操作系统相关
>
> :five:寄存器（pc Register）:与CPU相关

![](https://i.loli.net/2021/01/11/41xy8rjgwDTHnbJ.png)

###  :star: 静态关键字  static

> 一旦用了static关键字 那么这样的内容就不再属于对象自己，而是属于类的
>
> 所以凡是本类的对象，都共享同一份。

- demo01StaticField

```java
package day01;

public class demo01StaticField {
    public static void main(String[] args) {
        Student one=new Student("郭靖",20);
        Student two=new Student("黄蓉",16);
        one.room="101教室";
        System.out.println("姓名"+one.getName()+"年龄"+one.getAge()+" "+one.room+"学号"+one.getId());
        System.out.println("姓名"+two.getName()+"年龄"+two.getAge()+" "+two.room+"学号"+two.getId());

    }
}

```

- Student

```java
package day01;

public class Student {
    private String name;
    private  int  age;
    private int id;
    private  static int idCounter;
    static  String room;
    public Student() {
    idCounter++;
    }
    public Student(String name,int age){
        this.age=age;
        this.name=name;
        this.id=++idCounter;
    }
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public static int getIdCounter() {
        return idCounter;
    }

    public static void setIdCounter(int idCounter) {
        Student.idCounter = idCounter;
    }
}

```

- Myclass

```java
package day01;

public class Myclass {
    int num; //成员变量
    static int numStatic;// 静态变量
    public void method(){
        System.out.println("这是一个普通的成员方法");
//        成员方法可以访问成员变量
        System.out.println(num);
//        成员方法可以访问静态变量
        System.out.println(numStatic);
    }
    public static  void methodStatic(){
        System.out.println("这是一个静态方法");
//        静态可以访问静态变量
        System.out.println(numStatic);
//        静态不能直接访问非静态
//        System.out.println(num); 错误写法
//        静态方法中不能使用this关键字
//        System.out.println(this); 错误写法
    }

}

```

- demo02StaticMethod

```java
package day01;
/*
*
*一旦使用static修饰成员方法，那么这就是静态方法 静态方法不属于对象，而是属于类的
* 如果没有static 关键字，那么必须首先创建对象，然后通过对象才能使用它
*如果有了static关键字，那么不需要创建对象，直接就能通过类名来使用它
*
*无论是成员变量，还是成员方法 。如果有了 static  都推荐使用类名称进行调用
* 静态变量：类名称.静态变量
* 静态方法：类名称.静态方法()
*
*
*注意事项：
* 1、静态只能直接访问静态，不能直接访问非静态
*     原因:在内存中【先】有的静态内容,【后】有的非静态内容
*     “先人不知道后人，后人知道先人”
* 2、静态方法中不能写this。
*     原因：this代表当前对象，通过谁调用的方法，谁就是当前对象。
* */
public class demo02StaticMethod {
    public static void main(String[] args) {
       Myclass obj=new Myclass();//创建对象
            //对于静态方法来说，可以通过对象名进行调用，也可以直接通过类名进行调用
       obj.method();
       obj.methodStatic();//正确  但不推荐 这种写法在变异之后也会被javac翻译成“类名.静态方法名”
       Myclass.methodStatic();//正确 推荐
//对于本类当中的静态方法，可以省略类的名称
        myMethod();
        demo02StaticMethod.myMethod(); //和上面是一样的效果
    }
    public static  void myMethod(){
        System.out.println("自己的方法");
    }

}

```

#### 静态内存图

![](https://i.loli.net/2021/01/10/6uwIBcAHhZa9yP7.png)

#### 静态代码块

> ```java
> /*
>  * 静态代码块的格式是：
>  *
>  * public class 类名称{
>  *    static {
>  *       //静态代码块的内容
>  *           }
>  * }
>  *特点是： 当第一次用到本类时，静态代码块执行唯一的一次
>  *  静态内容总是优先于非静态，所以静态代码块比构造方法先执行
>  *
>  * 静态代码块的典型用途：
>  * 用来一次性地对静态成员变量进行赋值
>  * */
> ```

```java
public class demo04StaticBlock {
    public static void main(String[] args) {
        Person one=new Person();
        Person two=new Person();
    }
}
```

```java
package day01;

public class Person {
    static {
        System.out.println("Start 静态代码块");
    }
    public Person(){
        System.out.println("构造方法执行");
    }
}

```

### :yum: Private关键字

> private关键字 将需要保护的成员变量进行修饰
>
> 一旦使用了private进行修饰，在本类中可以随意访问
>
> 在其他地方要用 Getter / Setter 进行操作

### :deciduous_tree: this关键字

> 当方法的局部变量和累的成员变量重名的时候，根据“就近原则”，游侠使用局部变量
>
> 如果需要访问本类中的成员变量，需要使用this关键字
>
> this.成员变量名

### super关键字

> 1、在子类的成员方法中，访问父类的成员变量
>
> 2、在子类的成员方法中，访问父类的成员方法
>
> 3、在子类的构造方法中，访问父类的构造方法

- Zi类

  ```java
  public class zi extends  fu {
      int num=20;
      public zi(){
          super();// 构造方法
      }
      public void methodZi(){
          super.method();// 访问父类的method
          System.out.println(super.num);//访问父类的num
      }
  }
  ```

  

- Fu类

  ```java
  public class fu {
    int num=20;
    public void method(){
        System.out.println(num);
    }
  
  }
  ```

  

### this和super的关系

![](https://i.loli.net/2021/01/15/HyFGnEWLx6swtrJ.png)