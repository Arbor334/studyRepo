## Java基础_内部类

> 如果一个事物内部包含另一个事物，那么这就是一个类内部包含另一个类
>
> 例如：
>
> 身体和心脏的关系，又如：汽车和发动机的关系
>
> 分类：
>
> 1、成员内部类
>
> 2、局部内部类（包含匿名内部类）

#### 成员内部类

> ```java
> 成员内部类的定义格式：
> 修饰符 class 外部类名称(){
>    修饰符 class 内部类名称(){
> 
>    }
> }
> 
> 注意：内用外，随意访问，外用内，需要内部类对象
> 
> 如何使用成员内部类
> 1、间接方式：在外部类的方法中，使用内部类，然还好main只能调用外部类的方法
> 2、直接方式：公式：
> 外部类名称.内部类名称 对象名=new 外部类名称().new 内部类名称();
> ```

- Body类

  ```java
  public class Body {//外部类
      public class Heart{//内部类
          public void beat(){
              System.out.println("心脏跳动 蹦蹦蹦");
              System.out.println("我叫"+name);
          }
      }
      private String name;
  
  //外部类的方法
      public void methodBody(){
          System.out.println("外部类的方法");
          new Heart().beat();
      }
  }
  
  ```

- 测试

  ```java
  public class demo01innerClass {
      public static void main(String[] args) {
          Body body=new Body();
  //      通过外部类的对象，调用外部类的方法，里面间接在使用内部类Heart
          body.methodBody();
  //        直接使用
          Body.Heart heart=new Body().new Heart();
          heart.beat();
  
      }
  }
  ```

#### 内部类的同名变量访问

```java
// 如果出现了重名现象，那么格式是：外部类名称.this.外部类成员变量名
public class Outer {
    int num=10;
    public class Inner{
        int num=20;
        public void methodInner(){
            int num=30;
            System.out.println(num);//输出30 就近原则
            System.out.println(this.num);//输出Inner里的20
            System.out.println(Outer.this.num);//输出Outer 的10
        }
    }

}
```

#### 局部内部类

> ```java
> 如果一个类是定义在一个方法内部的，那么这就是一个局部内部类
> “局部”：只有当前所属的方法才能使用它，出了这个方法外面就不能用了
> 定义格式：
> 修饰符 class 外部类名称{
>    修饰符  返回值类型  外部类名称(参数列表){
>        class 局部内部类名称{
> 
>        }
>      }
>    }
> ```

- Outer2

  ```java
  public class Outer2 {
      public  void methodOuter(){
          class Inner{// 局部内部类
              int num=10;
              public void methodInner(){
                  System.out.println(num);
              }
          }
   Inner inner=new Inner();
          inner.methodInner();
      }
  }
  ```

- 测试

  ```java
  public class demo01Outer2 {
      public static void main(String[] args) {
          Outer2 outer2=new Outer2();
          outer2.methodOuter();
      }
  }
  ```

  

#### 总结类的权限修饰符

 public > protected >(default) >private

定义一个类的时候，权限修饰符规则

1、外部类： public / (default)

2、成员内部类： public / protected / (default) / private

3、局部内部类 ：什么都不写



####  匿名内部类

