### :arrow_down_small: 接口

> 接口就是一种<font color="red">公共的规范标准</font>

#### 接口的定义基本格式

> 接口就是多个类的公共规范
>
> 接口是一种引用数据结构，最重要的就是其中的<font color="red">抽象方法</font>
>
> 如何定义一个接口的格式：
>
> public interface 接口名称{
>
> ​			//接口内容	}
>
> 备注：换成关键字`interface` 之后，编译生成的字节码文件仍然是 .class文件
>
> 如果是JAVA7，那么接口中可以包含的内容有：
>
> ​					1、常量
>
> ​					2、抽象方法
>
> 如果是JAV8，还可以额外包含有：
>
> ​					3、默认方法
>
> ​					4、静态方法
>
> 如果是JAVA9，还可以额外包含有：
>
> ​					5、私有方法

#### 接口抽象方法定义

```java
/*
在任何版本的java中，接口都能定义接口方法：
格式：
public abstract 返回值类型  方法名称(参数列表);
注意事项：
1、接口当中的抽象方法，修饰符必须是两个固定的关键字：public abstract
2、这两个关键字修饰符，可以选择性地省略
3、方法三要素(返回值类型，方法名，参数列表)，可以随意定义
* */
public interface MyInterfaceAbstract {
//    这是一个抽象方法
    public abstract  void methodAbs();
//    这也是抽象方法
    abstract  void methodAbs2();
//    这也是抽象方法
    public  void methodAbs3();
//    这也是抽象方法
    void  methodAbs4();
    
}
```

#### 接口抽象方法使用

> ```java
> 接口使用步骤：
> 1、接口不能直接使用，必须有一个：“实现类”来“实现”该接口
> 格式：
>     public class 实现类名称 implements 接口名称{
>     }
> 2、接口的实现类必须覆盖重写（实现）接口中所有的抽象方法
> 去掉 abstract关键字，加上方法体大括号
> 3、创建实现类的对象，进行使用
> 
> 注意事项：
> 如果实现类并没有覆盖重写接口中所有的抽象方法，那么这个实现类自己就必须是抽象类
> ```

- demo01interface

  ```java
  public class demo01interface {
      public static void main(String[] args) {
  //         创建实现类的对象使用
          MyInterfaceAbstractImpl impl=new MyInterfaceAbstractImpl();
          impl.methodAbs1();
      }
  }
  
  ```

- MyInterfaceAbstract

  ```java
  public interface MyInterfaceAbstract {
  //    这是一个抽象方法
      public abstract  void methodAbs1();
  //    这也是抽象方法
      abstract  void methodAbs2();
  //    这也是抽象方法
      public  void methodAbs3();
  //    这也是抽象方法
      void  methodAbs4();
  
  }
  
  ```

- MyInterfaceAbstractImpl 

  ```java
  public class MyInterfaceAbstractImpl implements MyInterfaceAbstract{
  
      @Override
      public void methodAbs1() {
          System.out.println("第1个");
      }
  
      @Override
      public void methodAbs2() {
          System.out.println("第2个");
      }
  
      @Override
      public void methodAbs3() {
          System.out.println("第3个");
      }
  
      @Override
      public void methodAbs4() {
          System.out.println("第4个");
      }
  }
  ```

  

#### 接口默认方法定义

> ```java
> 从java8开始，接口里允许定义默认方法
> 格式
> public default 返回值类型  方法名称(参数列表){
>     方法体
>     }
> 备注：接口当中的默认方法，可以解决接口升级的问题
> 
> 1、接口的默认方法，可以通过接口实现类对象，直接调用
> 2、接口的默认方法，也可以被接口实现类进行覆盖重写
> ```

#### 接口默认方法使用

- demo02Myinterface

  ```java
  public class demo02Myinterface {
      public static void main(String[] args) {
          //    创建实现类对象
          MyInterfaceDefaultA a=new MyInterfaceDefaultA();
          a.methodAbs();// 调用抽象方法，
  //        调用默认方法，如果实现类当中没有，会向上找接口
          a.methodDefault();
  
  
          MyInterfaceDefaultB b=new MyInterfaceDefaultB();
          b.methodAbs();
          b.methodDefault();
      }
  
  }
  ```

- MyInterfaceDefault

  ```java
  public interface MyInterfaceDefault {
  //    抽象方法
      public abstract  void methodAbs();
  //    新添加的抽象方法
  //    public  abstract void methodAbs2();
  //新添加的默认方法
      public default void methodDefault(){
          System.out.println("这是新添加的默认方法");
      }
  
  }
  ```

- MyInterfaceDefaultA

  ```java
  public class MyInterfaceDefaultA implements MyInterfaceDefault{
      @Override
      public void methodAbs() {
          System.out.println("实现类");
      }
  
  }
  ```

- MyInterfaceDefaultB

  ```java
  public class MyInterfaceDefaultB implements MyInterfaceDefault{
      @Override
      public void methodAbs() {
          System.out.println("B");
      }
      @Override
      public void methodDefault(){
          System.out.println("实现类覆盖重写了接口的默认方法");
      }
  }
  ```

  

#### 接口静态方法定义

> ```java
> 从java8开始，接口当中允许定义静态方法
> 格式：
> public static 返回值类型  方法名称(参数列表){
>             方法体
>             }
> 提示：就是将abstract或者default替换成static 即可，带上方法体
> ```



#### 接口静态方法使用

- demo03Interface

  ```java
  public class demo03Interface {
      public static void main(String[] args) {
     // MyInterfaceStaticImpl impl=new MyInterfaceStaticImpl();
      //错误写法
      //impl.methodStatic();
     //        直接通过接口名称调用静态方法
          MyInterfaceStatic.methodStatic();
      }
  }
  ```

- MyInterfaceStatic

  ```java
  public interface MyInterfaceStatic {
      public static void methodStatic(){
          System.out.println("这是接口的静态方法");
      }
  }
  ```

  

#### 接口私有方法使用

- demo04private

  ```java
  public class demo04private {
      public static void main(String[] args) {
  //              静态方法直接调用
          MyInterfacePrivateB.methodStatic1();
      }
  }
  
  ```

- MyInterfacePrivateImpl

  ```java
  public class MyInterfacePrivateImpl implements MyInterfacePrivateA{
      public void method(){
          methodDefault1();
      }
  }
  
  ```

- MyInterfacePrivateA

  ```java
  package day03;
  /*
  methodDefault1和methodDefault2 需要用到methodCommon 里的代码
  但是调用的时候不想让别人直接调用methodCommon
  就可以使用private 关键字 只能在这个接口中可以使用
  methodDefault1和methodDefault2 是可以直接 点出来的
  */
  public interface MyInterfacePrivateA {
  
        public default void methodDefault1(){
            System.out.println("默认方法1");
            methodCommon();
        }
        public  default void methodDefault2(){
            System.out.println("默认方法2");
            methodCommon();
        }
        private void methodCommon(){
            System.out.println("Aaa");
            System.out.println("Bbb");
            System.out.println("Ccc");
        }
  }
  
  ```

- MyInterfacePrivateB

  ```java
  package day03;
  /*
  methodStatic1和methodStatic2中都需要使用methodCommon方法
  但是不想让别人直接调用methodCommon 方法
  可以使用这样的方式
  */
  public interface MyInterfacePrivateB {
   public  static void methodStatic1(){
       System.out.println("静态方法1");
       methodCommon();
   }
      public  static void methodStatic2(){
          System.out.println("静态方法2");
          methodCommon();
      }
      private static  void methodCommon(){
          System.out.println("Aaa");
          System.out.println("Bbb");
          System.out.println("Ccc");
      }
  
  }
  
  ```

  

#### 接口常量定义和使用

> ```java
> 接口当中也可以定义“成员变量”,但是必须使用public static final 三个关键字进行修饰
> 从效果上看，这其实就是接口的【常量】
> 格式：
> public static final 数据类型   常量名称=数据值
> 备注：
> 一旦使用final关键字进行修饰，说明不可改变
> 注意事项：
> 1、接口当中的常量，可以省略public static final 注意，不写也照样是这样
> 2、接口当中的常量，必须进行赋值，不能不赋值
> 3、接口中常量的名称，使用完全大写的字母，用下划线进行分割。
> ```

- MyInterfaceConst

  ```java
  public interface MyInterfaceConst {
  //    这其实就是一个常量，一旦赋值，不可以修改
      public static final String NAME="string";
  
  
  }
  ```

- demo05Interface

  ```java
  public class demo05Interface {
      public static void main(String[] args) {
  //        访问接口中的常量
          System.out.println(MyInterfaceConst.NAME);
      }
  }
  ```



#### 接口内容总结

> 在Java 9+ 版本中，接口的内容可以有：

- 1、成员变量其实是常量，格式：
  - [public] [static] [final] 数据类型  常量名称=数据值
  - <font color="red">注意，常量必须进行赋值，而且一旦赋值不能改变，常量名称完全大写，用下划线进行分隔</font>
- 2、接口中最重要的就是抽象方法，格式：
  - [public] [abstract]  返回值类型  方法名称(参数列表);
  - 注意：实现类必须覆盖重写接口所有的抽象方法，除非实现类是抽象类
- 3、从Java 8 开始，接口里允许定义默认方法，格式：
  - [public] [default] 返回值类型 方法名称(参数列表) {方法体}
  - 注意：默认方法也可以被覆盖重写
- 4、从Java 8 开始，接口里允许定义静态方法，格式：
  - [public] static 返回值类型 方法名称(参数列表){ 方法体}
  - 注意：应该通过接口名称进行调用，不能通过实现类对象调用接口静态方法
- 5、从 Java 9开始，接口里允许定义私有方法，格式：
  - 普通私有方法：private 返回值类型 方法名称(参数列表) {方法体}
  - 静态私有方法：private static 返回值类型 方法名称(参数列表) {方法体}
  - 注意： private 的方法只有接口自己才能调用，不能被实现类或别人使用

#### 继承父类并实现多个接口

> ```java
> 使用接口的时候，需要注意：
> 1、接口还没有静态代码块或者构造方法的
> 2、一个类的直接父类是唯一的，但是一个类可以同时实现多个接口
> 格式：
> public class MyInterfaceImpl implements MyInterface1,MyInterface2{
> //  覆盖重写所有抽象方法
> }
> 3、如果实现类所实现的多个接口当中，存在重复的抽象方法，那么只需要覆盖重写一次即可
> 4、如果实现类没有覆盖重写所有接口当中的所有抽象方法，那么实现类就必须是一个抽象类
> 5、如果实现类所实现的多个接口当中，存在重复的默认方法，那么实现类一定要对冲突的默认方法进行覆盖重写
> 6、一个类如果直接父类当中的方法，和接口当中的默认方法产生了冲突，优先使用父类中的方法
> ```

- demo06Interface 

  ```JAVA
  public class demo06Interface  {
      public static void main(String[] args) {
          Zi zi=new Zi();
          zi.method();
      }
  }
  ```

- Zi

  ```java
  public class Zi extends Fu  implements MyInterface {
  }
  ```

- Fu

  ```java
  public class Fu {
      public void method(){
          System.out.println("父类");
      }
  }
  ```

- MyInterface

  ```java
  public interface MyInterface {
      public default void method(){
          System.out.println("接口默认方法");
      }
  }
  
  ```



#### 接口之间的多继承

> ```java
> 1、类与类之间是单继承的，直接父类只有一个
> 2、类与接口之间是多实现的，一个类可以实现多个接口
> 3、接口和接口之间是多继承的
> 注意事项：
> 1、多个父接口当中的抽象方法如果重复，没关系
> 2、多个父接口当中的默认方法如果重复，那么子接口必须进行默认方法的覆盖重写，【而且带着default关键字】
> ```



- Myinterface接口 继承MyinterfaceA 接口和MyinterfaceB接口

- MyinterfaceA

  ```java
  public interface MyinterfaceA {
      public abstract void methodA();
      public abstract void common();
      public default void methodCommon(){
          System.out.println("aaaa");
      }
  }
  
  ```

- MyinterfaceB

  ```java
  public interface MyinterfaceB {
      public abstract void methodB();
      public abstract void common();
      public default void methodCommon(){
          System.out.println("bbbb");
      }
  }
  ```

- Myinterface

  ```java
  public interface MyInterface extends MyinterfaceA,MyinterfaceB{
   public abstract void methodInterface();
  
      @Override
      default void methodCommon() {
          System.out.println("hhhhh");
      }
  }
  
  ```

- MyinterfacImpl

  ```java
  public class MyInterfaceImpl implements MyInterface {
      @Override
      public void methodInterface() {
  
      }
  
      @Override
      public void methodA() {
  
      }
  
      @Override
      public void common() {
  
      }
  
      @Override
      public void methodB() {
  
      }
  }
  ```

  