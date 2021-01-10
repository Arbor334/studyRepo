## java基础[1]

### 一、静态关键字  static

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

