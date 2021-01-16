### :boxing_glove: 继承

> <font color="green">继承是多态的前提，如果没有继承，就没有多态。</font>
>
> 

![](https://i.loli.net/2021/01/13/xKUb2BQJlRtZn1X.png)



#### 继承的格式

```java
/*
在继承的关系中，“子类就是一个父类”。也就是说，子类可以被当做父类看待。

定义父类的格式：（一个普通的类定义）
public class 父类名称{
  成员变量
  成员方法
}

定义子类的格式
public class 子类名称 extends 父类名称{

}
*/
public class demo01Extend {
    public static void main(String[] args) {
        teacher teacher=new teacher();
//        teacher类当中虽然什么都没写，但是汇集成来自父类的method方法
        teacher.method();
    }
}

```

- teacher类

```java
//定义了一个员工的子类：讲师
public class teacher extends employee{
}

```

- employee类

```java
public class employee {
    public void method(){
        System.out.println("方法执行");
    }
}
```

#### 继承中成员变量的访问特点

```java
package day01;
/*
在父子类的继承关系当中，如果成员变量重名，则创建子类对象时，访问有两种方式、
直接通过子类对象访问成员变量：
    等号左边是谁，就优先用谁，没有则向上找
间接通过成员方法访问成员变量：
    方法属于谁，优先用谁，没有则往上找
*/
public class demo02ExtendField {
    public static void main(String[] args) {
        fu fu=new fu();// 创建父类对象
        System.out.println(fu.numfu); //只能使用父类的东西，没有任何子类的东西

        zi zi=new zi();//创建子类对象
        System.out.println(zi.numfu);//10
        System.out.println(zi.numzi);//20
        System.out.println("=====");
//        等号左边是谁，就优先用谁
        System.out.println(zi.num);
//        这个方法是子类的，就优先用子类的，没有再往上找
        zi.methodzi();
//        这个方法是属于谁的，输出谁的成员变量
        zi.methodfu();
    }
}

```

- fu类

```java
public class fu {
    int numfu=10;
    int num=100;
    public  void methodfu(){
//        使用本类中的，不会向下找子类的。
        System.out.println(num);
    }

}
```

- zi 类

```java
public class zi extends  fu{
    int numzi=20;
    int num=200;
    public void methodzi(){
//        因为本类中有num，所以这里用的是本类的num
        System.out.println(num);
    }
}

```

#### 区分子类方法中重名的三种变量

```java
public class zi extends  fu {
    int num=20;
    public void method(){
        int num=30;
        System.out.println(num);// 30  局部变量
        System.out.println(this.num);// 20 本类的成员变量
        System.out.println(super.num); // 100 父类的成员变量
    }

}
```

#### 继承中成员方法的访问特点

> 在父子类的继承关系中，创建子类对象，访问成员方法的规则：
>
> ​	创建的对象是谁，就优先用谁，没有则向上找
>
> <font color="red">注意事项：</font>
>
> 无论是成员方法还是成员变量，如果没有都是向上找父类，绝对不会向下找子类的。

#### 继承中方法的覆盖重写(override)

> 重写：在继承关系当中，方法的名称是一样的，参数列表也一样。
>
> 重写（override）：方法名称一样，参数列表也一样。
>
> 重载（overload）：方法名称一样，参数列表不一样。
>
> 方法的覆盖重写特点：创建的是子类对象，则优先使用子类对象
>
> `注意事项：`
>
> :one: 、必须保证父子类之间方法的名称相同，参数列表也相同
>
> ​		@Override  写在方法前面。用来检测是不是有效的正确覆盖重写
>
> ​		这个注解就算不写，只要满足要求，也是正确的方法覆盖重写
>
> :two: 、子类方法的返回值必须【小于等于】父类的返回值范围
>
> ​	     Object类是所有类的公共最高父类
>
> :three: 、子类方法的权限必须【大于等于】父类方法的权限修饰符
>
> ​	`public >protected>(default)>private`
>
> default 不是关键字default  而是什么都不写，留空

- newPhone类

  ```
  package day01;
  
  public class NewPhone extends  phone{
      @Override
      public void show() {
          super.show();
          System.out.println("显示头像");
          System.out.println("显示姓名");
      }
  }
  ```

- phone类

  ```java
  package day01;
  
  public class phone {
      public void send(){
          System.out.println("发短信");
      }
      public void show(){
          System.out.println("显示号码");
      }
      public void call(){
          System.out.println("打电话");
      }
  }
  
  ```

- main方法

  ```java
  package day01;
  
  public class demo01Override {
      public static void main(String[] args) {
          phone phone=new phone();
          phone.send();
          phone.show();
          phone.call();
          System.out.println("========");
          NewPhone newPhone=new NewPhone();
          newPhone.show();
          newPhone.send();
          newPhone.call();
  /*发短信
  显示号码
  打电话
  ========
  显示号码
  显示头像
  显示姓名
  发短信
  打电话*/
      }
  
  
  }
  
  ```



#### 继承中构造方法的访问特点

> 1、子类构造方法当中有一个默认隐含的“super()”调用，所以一定是先调用父类构造，后执行子类构造
>
> 2、可以通过super关键字来子类构造调用父类重载构造
>
> 3、super的父类构造调用，必须是子类构造方法的第一个语句。不能一个子类构造调用多次super构造
>
> 总结：
>
> ​		子类必须调用父类构造方法，不写则赠送super()；写了则用写的指定的super调用，super只能有一个，必须是第一个。
>
> 

- newPhone类

  ```java
  public class NewPhone extends  phone{
      public NewPhone() {
  //        super();  在调用父类无参构造方法
          super(20);//调用父类重载的构造方法
          System.out.println("子类构造方法");
      }
  }
  ```

- phone类

  ```java
  public class phone {
      public phone() {
          System.out.println("父类无参构造");
      }
      public phone(int num){
          System.out.println("父类有参构造"+num);
      }
  
  }
  ```

- main方法

  ```java
  public class demo01Override {
      public static void main(String[] args) {
          NewPhone newPhone=new NewPhone();
  //                父类有参构造20
  //                子类构造方法
     
      }
  }
  ```

#### 继承的三个特点

![](https://i.loli.net/2021/01/15/jGW6TBRNbZhAieD.png)