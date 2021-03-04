# 一、Spring5 入门

## 1、Spring框架

- :one: Spring  是一个轻量级的开源的JavaEE框架
- :two: Spring 可以解决企业应用开发的复杂性
- :three: Spring 有很多组成部分 两个核心部分：IOC AOP
  - IOC  :`控制反转，把创建对象的过程交给spring进行`
  - AOP :`面向切面，不修改源代码进行功能增强`
- :four: Spring特点
  - `方便解耦，简化开发`
  - `AOP编程的支持` 
  - `方便程序的测试`
  - `方便集成各种优秀框架`
  - `降低API开发难度`

## 2、入门案例

### 1、下载Spring5最新稳定版本  `GA表示稳定版`  	`SNAPSHOT表示测试版`

![](https://i.loli.net/2021/01/05/ycj25gvUlLhFW9J.png)

### 2、[下载地址](https://repo.spring.io/release/org/springframework/spring/)

- `找到最新的RELEASE下载  xxxxRELEASE-dist.zip解压就可以了`
- ![](https://i.loli.net/2021/01/05/UOyb3H5Y72w6eqn.png)

### 3、打开IDEA工具

- `创建一个新的java工程``导入Spring5的jar包``spring5 的模块`![](https://i.loli.net/2021/01/05/oTUekN5F1wRSuYj.png)`导入Beans、 Core 、Context、 Expression  还有额外的logging  jar包`
- ![](https://i.loli.net/2021/01/05/J3BuYdr7Q19pVnv.png)
- 在idea 中的 project Structure 中导入包

![](https://i.loli.net/2021/01/05/gVaFb67szKuhfPi.png)



![](https://i.loli.net/2021/01/05/P1a2iplHNfLIC7x.png)

### 4、创建普通类，在这个类创建普通方法

```java
package com.atgui.spring5;

public class User {
    public void add(){
        System.out.println("add method");
    }

}

```



### 5、创建Spring配置文件，在配置文件配置创建的对象

- (1) Spring 配置文件使用xml格式   （配置文件.xml放在src文件夹下）

![](https://i.loli.net/2021/01/14/wb4iH5npx7sqNGk.png)

![](https://i.loli.net/2021/01/14/1bl27efSgPFpxmo.png)



```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
< !--配置user对象创建-->
    <bean id="user" class="com.atguigu.spring.User"></bean>
    <!--id 是起名字 以后会用    class就是全限定类名-->
</beans>
```

### 6、进行测试代码编写

![](https://i.loli.net/2021/01/14/VlvGk7e2XMqNL1d.png)



## 3、IOC容器

- （1）、IOC底层原理
- （2）、IOC接口（BeanFactory）
- （3）、IOC操作Bean管理（基于xml）
- （4）、IOC操作Bean管理（基于注解）

### IOC基本概念

> 什么是IOC（Inversion of Control）
>
> （1）控制反转，把对象创建和对象之间额调用过程，交给Spring管理
>
> （2）使用IOC目的：为了降低耦合度
>
> （3）入门案例就是IOC实现

### IOC底层原理

> (1)xml解析、工厂模式、反射

###  画图IOC底层原理

![](https://i.loli.net/2021/01/14/Bzgf7tyQiwuxWYd.png)

### IOC(接口)

> 1、IOC思想基于IOC容器完成，IOC容器底层就是对象工厂
>
> 2、Spring 提供IOC容器实现两种方式：（两个接口 都可以`加载spring配置文件和获取配置创建的对象`）
>
> （1）、BeanFactory:IOC容器基本实现，是Spring内部的使用接口，不提供开发人员进行使用
>
> ​		<font color="red">加载配置文件的时候不会创建对象，在获取或使用对象的时候才创建对象</font>
>
> （2）、ApplicationContext:BeanFactory接口的子接口，提供更多强大的功能，一般由开发人员进行使用
>
> <font color="red"> 加载配置文件的时候就会把在配置文件对象进行创建</font>
>
> 3、ApplicationContext接口有实现类
>
> ![](https://i.loli.net/2021/01/14/gwH86ES7sIubAR4.png)
>
> FileSystemXmlApplicationContext ：用的时候要写全路径（从盘符开始）
>
> 而 
>
> ClassPathXmlApplicationContext：对应src下的路径

### Bean管理XML方式（创建对象和set注入属性）

> `1、什么是Bean管理`
>
> Bean管理指的是两个操作
>
> （1）、Spring创建对象
>
> ​		<font color="red">简单来说，就是上面我们入门案例中第五个 创建对象时  写`<bean> </bean>`</font>
>
> （2）、Spring注入属性
>
> ​	<font color="red"> 简单来说，这个就是 我们在创建对象时候的一些属性，开始使用Getter。Setter进行访问和设置的，接下来就交给Spring进行处理</font>
>
> `2、Bean管理操作有两种方式`
>
> （1）、基于xml配置文件方式实现
>
> （2）、基于注解方式实现

#### IOC操作Bean管理（基于xml方式）

- 1、基于xml方式创建对象

![](https://i.loli.net/2021/01/15/t4Eigdwle9uMN8X.png)

​				<font color="red">在Spring配置文件中，使用Bean标签，标签里面添加对应属性就可以实现对象创建</font>

​					<font size="4">常用属性</font>

| 属性名    | 含义                                                         |
| :-------- | :----------------------------------------------------------- |
| id属性    | 唯一标识                                                     |
| class属性 | 类全路径（包含路径）                                         |
| name属性  | 和id属性作用一样，但是属性值可以包含特殊符号，而id的属性值不行 |
|           |                                                              |

​					<font color="red">创建对象的时候，默认也是执行无参数构造方法</font>

- 2、基于xml方式注入属性

  - （1）DI：依赖注入，就是注入属性（`DI是IOC中的一种具体实现，就是表还是注入属性，但是注入属性要在创建对象的基础之上进行`)
  - 

- 3、第一种注入方式：使用set方法进行注入

  - (1)、创建一个类，定义属性和对应的set方法

    ```java
    public class Book {
        private String bname;
        private String bauthor;
    
        public void setBname(String bname) {
            this.bname = bname;
        }
    
        public void setBauthor(String bauthor) {
            this.bauthor = bauthor;
        }
    }
    
    ```

    

  - (2)：在spring配置文件配置对象创建，配置属性注入

    ```java
    <!--使用Set方式进行注入-->
        <bean id="book" class="com.atguigu.spring.Book">
            <!--使用property完成属性注入
            name：类名称中的值
            value：向属性中注入的值
            -->
            <property name="bname" value="Spring基础"></property>
            <property name="bauthor" value="arbor"></property>
    
       </bean>
    ```

  - (3)、测试

    ```java
    public class testSpring5Test {
        @Test
        public void testBook() {
    //      1、  加载spring配置文件
            BeanFactory context=new ClassPathXmlApplicationContext("bean1.xml");
    
    //        2、获取配置创建的对象
            Book book= context.getBean("book", Book.class);
            System.out.println(book);//输出地址值
            book.testDemo();//输出testDemo
    //        com.atguigu.spring.Book@553a3d88
    //        Spring基础::arbor
        }
    }
    ```

    

- 4、第二种注入方式：使用有参数构造进行注入

  - （1)、创建一个类，定义属性和有参构造

    ```java
    public class order {
    //    属性
        private String name;
        private  String address;
    //          有参数的构造
        public order(String name, String address) {
            this.name = name;
            this.address = address;
        }
    }
    ```

    

  - (2)、在spring配置文件配置对象创建，配置有参构造注入

    ```java
    <!--有参构造进行注入-->
        <bean id="order" class="com.atguigu.spring.order">
            <constructor-arg name="name" value="Computer"></constructor-arg>
            <constructor-arg name="address" value="China"></constructor-arg>
            <!--        <constructor-arg index="0" value="Computer"></constructor-arg>-->
          //  也可以通过上面的 index 属性按照0 1 2 这样的方法对 对象的属性进行赋值 ，不过不推荐
        </bean>
    ```

  - 3、测试

    ```java
    public class testSpring5Test {
        @Test
        public void testBook() {
    //      1、  加载spring配置文件
            ApplicationContext context=new ClassPathXmlApplicationContext("bean1.xml");
    
    //        2、获取配置创建的对象
            Order order= context.getBean("order", Order.class);
            System.out.println(order);//输出地址值
            order.orderTest();//输出
    //        com.atguigu.spring.Order@1fc2b765
    //        Computer:China
        }
    ```

- 5、第三种方式：p名称空间注入（了解）

  - (1)、使用p名称空间注入，可以简化基于xml配置方式

    - 第一步：添加p名称空间在配置文件中

      ![](https://i.loli.net/2021/01/15/1FrIPHOx2jBbvwz.png)

    - 第二步：进行属性注入，在bean标签中进行测试

      ```java
      <!--底层用的set注入方式-->
      <bean id="book" class="com.atguigu.spring.Book" p:bname="九阳神功" p:bauthor="无名氏">
          </bean>
      ```

  - (2)、测试

    ```java
        public void testBook() {
    //      1、  加载spring配置文件
            ApplicationContext context=new ClassPathXmlApplicationContext("bean1.xml");
    
    //        2、获取配置创建的对象
            Book book= context.getBean("book", Book.class);
            System.out.println(book);//输出地址值
            book.testDemo();//输出
    //        com.atguigu.spring.Book@13eb8acf
    //        九阳神功::无名氏
    ```

#### IOC操作Bean管理（xml注入其他属性类型）

> 对字面量进行控制，可以设为`null值`和`属性值中包含特殊符号`

- `设置null值`

  ```java
    <!--4、设置null值-->
      <bean id="book" class="com.atguigu.spring.Book">
          <property name="bname" value="java基础"></property>
         <property name="bauthor" value="arbor"></property>
          <property name="address">
              <null/>
          </property>
      </bean>
  ```

- 测试

  ```java
  public class testSpring5Test {
      @Test
      public void testBook() {
  //      1、  加载spring配置文件
          ApplicationContext context=new ClassPathXmlApplicationContext("bean1.xml");
  
  //        2、获取配置创建的对象
          Book book= context.getBean("book", Book.class);
          System.out.println(book);//输出地址值
          book.testDemo();//输出
  //        com.atguigu.spring.Book@553a3d88
  //        java基础::arbor::null
      }
  }
  ```

- `设置属性值中包含特殊符号`

  ```java
  <!--5、属性值包含特殊符号
     1、把<> 进行转义 &lt,&gt 这样
     2、把带特殊符号内容写到CDATA中   <value><![CDATA[内容]]> </value>
  -->
      <bean id="book" class="com.atguigu.spring.Book">
                  <property name="bname" value="java基础"></property>
                  <property name="bauthor" value="arbor"></property>
          <property name="address">
              <value> <![CDATA[<<南京>>]]> </value>
          </property>
      </bean>
  ```

  

- 测试

  ```java
      public void testBook() {
  //      1、  加载spring配置文件
          ApplicationContext context=new ClassPathXmlApplicationContext("bean1.xml");
  
  //        2、获取配置创建的对象
          Book book= context.getBean("book", Book.class);
          System.out.println(book);//输出地址值
          book.testDemo();//输出
  //        com.atguigu.spring.Book@553a3d88
  //        java基础::arbor:: <<南京>>
      }
  ```





#### IOC操作Bean管理(xml注入外部Bean)

> （1）创建两个类 service类和dao类
>
> （2）在service调用dao里面的方法
>
> （3）在spring配置文件中进行配置

- 1、创建UserDao接口

  ```java
  public interface UserDao {
      public void update();
  }
  
  ```

- 2、创建UserDao接口的而实现类

  ```java
  public class UserDaoImpl implements UserDao{
  
      @Override
      public void update() {
          System.out.println("UserDao service ..");
      }
  }
  ```

- 3、创建UserService类

  ```java
  public class UserService {
  //    创建UserDao类型属性，生成set方法
      private  UserDao userDao;
  
      public void setUserDao(UserDao userDao) {
          this.userDao = userDao;
      }
  
      public void add(){
          System.out.println("service add......");
          userDao.update();
  
      }
  }
  ```

- 4、配置文件

  ```java
  <!--1、service 和  dao 对象创建-->
      <bean id="userService" class="com.atguigu.spring.service.UserService">
          <!--注入userDao对象
          name属性值：类里面的属性名称
          ref属性：创建userDao对象bean标签id值 把外部bean加进来
          -->
          <property name="userDao" ref="userDao"> </property>
      </bean>
      <bean id="userDao" class="com.atguigu.spring.dao.UserDaoImpl"></bean>
  
  ```

- 5、实现

  ```java
  public class TestBean {
      @Test
     public void testAdd(){
  // 加载spring 配置文件
          ApplicationContext context=new ClassPathXmlApplicationContext("bean2.xml");
  //        获取配置创建的对象
          UserService userService=context.getBean("userService",UserService.class);
          userService.add();
  //        service add......
  //UserDao service ..
     }
  }
  
  ```



#### IOC操作Bean管理(注入属性 -内部Bean和级联赋值）

- 1、创建Emp类

  ```java
  
  //员工类
  public class Emp {
      private String ename;
      private String gender;
  //    员工属于某一个部门，使用对象形式表示
      private Dept dept;
      public void setDept(Dept dept) {
          this.dept = dept;
      }
  //在级联赋值的第二种方法的时候才需要这个方法
      public Dept getDept() {
          return dept;
      }
  
      public void setEname(String ename) {
          this.ename = ename;
      }
  
      public void setGender(String gender) {
          this.gender = gender;
      }
  
      public void add(){
          System.out.println(ename+" "+gender+" "+dept);
      }
  
  }
  
  ```

- 2、创建Dept类

  ```java
  //部门类
  public class Dept {
      private String dname;
  
      public void setDname(String dname) {
          this.dname = dname;
      }
  
      @Override
      public String toString() {
          return "Dept{" +
                  "dname='" + dname + '\'' +
                  '}';
      }
  }
  
  ```

- 3、配置文件

  > 在创建内部类的时候 ，一般的属性直接赋值，
  >
  > 其他的 对象属性，需要再创建一个新的bean 在property中  一个对象当做一个属性

  ```java
  <!--内部bean-->
      <bean id="emp" class="com.atguigu.spring.bean.Emp">
         <property name="ename" value="lucy"></property>
         <property name="gender" value="girl"></property>
          <property name="dept">
              <bean id="dept" class="com.atguigu.spring.bean.Dept">
                  <property name="dname" value="安保部"></property>
              </bean>
          </property>
      </bean>
  </beans>
  ```

- 4、测试

  ```java
  public class TestBean {
      @Test
     public void testAdd(){
  // 加载spring 配置文件
          ApplicationContext context=new ClassPathXmlApplicationContext("bean3.xml");
  //        获取配置创建的对象
          Emp emp=context.getBean("emp",Emp.class);
          emp.add();
  
     }
  }
  ```

- 级联赋值的配置 

- > 使用的时候只需要修改 配置文件的名字就行  测试类和上面的一样

  ```java
  <!--级联赋值-->
      <bean id="emp" class="com.atguigu.spring.bean.Emp">
          <property name="ename" value="lucy"></property>
          <property name="gender" value="女"></property>
  <!--        级联赋值第一种写法
       <property name="dept" ref="dept"></property>
       <bean id="dept" class="com.atguigu.spring.bean.Dept">
          <property name="dname" value="财务部"></property>
      </bean>
  
      -->
  
          <property name="dept" ref="dept"></property>
  
  <!--        第二种写法  需要在emp的添加 dname的get方法-->
          <property name="dept.dname" value="技术部" ></property>
      </bean>
      <bean id="dept" class="com.atguigu.spring.bean.Dept">
          <property name="dname" value="财务部"></property>
      </bean>
  
  </beans>
  ```



#### IOC操作 Bean管理（注入集合属性类型）

-  1、注入数组类型属性

- 2、注入List集合类型属性

- 3、注入Map集合类型属性

  - （1）、创建类，定义数组，list，map，set类型属性,生成对应的set方法

    ```java
    package com.atguigu.spring.collectiontype;
    
    import java.util.List;
    import java.util.Map;
    import java.util.Set;
    
    public class stu {
    //    1、数组类型属性
        private String[] classes;
    
        public void setClasses(String[] classes) {
            this.classes = classes;
        }
    //    2、list集合类型属性
        private List<String> list;
    
        public void setList(List<String> list) {
            this.list = list;
        }
    //    3、map集合类型属性
        private Map<String,String> maps;
    
        public void setMaps(Map<String, String> maps) {
            this.maps = maps;
        }
    //    4、set集合类型属性
        private Set<String> sets;
    
        public void setSets(Set<String> sets) {
            this.sets = sets;
        }
    }
    
    ```

  - （2）、在spring配置文件中进行配置

  - 

  - 









