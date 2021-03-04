## Java基础_多态

> extends 继承或者implements 实现，是多态性的前提

### 多态的定义

>  小明即有学生形态，又有人的形态。
>
> 多态

### 多态的格式和使用

```java
代码当中体现多态性，其实就是一句话，父类引用指向子类对象。
格式：
父类名称  对象名=new 子类名称();
或者：
接口名称 对象名 =new  实现类名称();
```

- demo01Multi

  ```java
  public class demo01Multi {
      public static void main(String[] args) {
  //        使用多态的写法
  //        左侧父类的引用，指向了右侧子类的对象
          Fu obj=new Zi();
          obj.method(); // new的谁 就优先使用谁
          obj.methodFu();
      }
  }
  ```

- Fu

  ```java
  public class Fu {
      public void method(){
          System.out.println("父类方法");
      }
  public void methodFu(){
      System.out.println("父类特有的方法");
  }
  }
  ```

- Zi

  ```java
  public class Zi extends Fu{
      @Override
      public void method() {
          System.out.println("子类方法");
      }
  }
  
  ```



### 多态中成员变量的使用特点

```java
访问成员变量的两种方式：
1、直接通过对象名称访问成员变量，看等号左边是谁，优先用谁，没有则往上找
2、间接通过成员方法访问成员变量：看该方法属于谁，优先用谁，没有则往上找
```

- demo01MultiField

  ```java
  public class demo01MultiField {
      public static void main(String[] args) {
  //        使用多态的写法 ，父类引用指向子类对象
          Fu obj=new Zi();
          System.out.println(obj.num);
  //        子类没有覆盖重写 就是父 10
  //        子类如果覆盖重写 就是子 20
          obj.showNum();
      }
  }
  ```

- Fu

  ```java
  public class Fu {
  int  num=10;
      public void showNum(){
          System.out.println(num);
      }
  }
  ```

- Zi

  ```java
  public class Zi extends Fu{
    int num=20;
    public void showNum(){
      System.out.println(num);
    }
  }
  ```

### 多态中成员方法的使用特点

> 成员变量和成员方法比较：
>
> 成员变量：编译看左边，运行还看左边
>
> 成员方法：编译看左边，运行看右边
>
> 编译看左边：左边是什么对象，对象中没有的成员方法，编译会报错
>
> 运行看右边：右边方法子类有没有，没有向上找父类，有的话就是子类覆盖重写了父类的，然后运行子类的

### 多态的向上、向下转型

![](https://i.loli.net/2021/01/17/x6tX7bOlavLoYDq.png)



### instanceof 类型转换

> 如何才能知道一个父类引用对象，本来是什么类型呢？
>
> 格式：
>
> 对象  instanceof 类名称
>
> 这将会得到一个boolean 值结果，也就是判断前面的对象是不是当做后面类型的实例。