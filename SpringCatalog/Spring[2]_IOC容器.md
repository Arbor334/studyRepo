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

#### Bean管理XML方式（创建对象和set注入属性）

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

- > 提供带所有参数的有参构造方法

  - （1）DI：依赖注入，就是注入属性（`DI是IOC中的一种具体实现，就是表还是注入属性，但是注入属性要在创建对象的基础之上进行`)
  - 

- 3、第一种注入方式：使用set方法进行注入

- > 1、提供默认空参构造方法
  >
  > 2、为所有属性提供setter方法

  - (1)、创建一个类，定义属性和对应的set方法

    ```java
    public class Book {
        private String bname;
        private String bauthor;
       public Book(){
         
       }
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

    

- 4、第二种注入方式：使用有参数构造进行注入(xml注入)

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

#### IOC操作Bean管理（SPEL）

> 

:one: 实现

- 第一步、 创建Message类

```java
package com.company.class2.Spel;

public class Message {
    private  String message;

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }

    @Override
    public String toString() {
        return "Message{" +
                "message='" + message + '\'' +
                '}';
    }
}

```



- 第二步、配置文件

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
<bean id="message" class="com.company.class2.Spel.Message">
       <property name="message" value="#{systemProperties['user.name']}" />
</bean>
</beans>
```



- 第三步、测试

```java
package com.company.class2.Spel;

import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class TestSpel {
    @Test
    public void Test(){
        ApplicationContext ac=new ClassPathXmlApplicationContext("beans7.xml");
        System.out.println(ac.getBean("message"));
    }
}

```



- 结果

![](https://i.loli.net/2021/03/11/LyJ32Pm5ivxUo4G.png)

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
  
    ```java
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
      <bean id="stu" class="com.atguigu.spring.Stu.Stu">
    <!--数组类型的注入-->
          <property name="classes">
                <array>
                    <value>java课程教学</value>
                    <value>数据库课程</value>
                </array>
            </property>
    <!--list 类型注入-->
            <property name="list">
                <list>
                    <value>张三</value>
                    <value>李四</value>
                </list>
            </property>
    <!--  map类型注入      -->
            <property name="maps">
                <map>
                    <entry key="Java" value="java"></entry>
                    <entry key="PHP" value="php"></entry>
                </map>
            </property>
    <!--set类型注入-->
            <property name="sets">
                <set>
                    <value>Mysql</value>
                    <value>PHP</value>
                </set>
            </property>
        </bean>
    
    </beans>
    ```
  
    
  
  - （3）、测试类
  
    ```java
    public class TestSpring {
        @Test
        public void TestCollection(){
            ApplicationContext ac=new ClassPathXmlApplicationContext("bean1.xml");
            Stu stu=ac.getBean("stu",Stu.class);
            stu.test();
        }
    }
    ```
  
    
  
-  `细节问题`

   -  1、在集合之中设置对象类型值
   -  （1）、对上面的Stu类添加一个list，里面存的是对象类型

   ```java
   package com.atguigu.spring.Stu;
   
   import java.util.Arrays;
   import java.util.List;
   import java.util.Map;
   import java.util.Set;
   
   public class Stu {
   // 新添加的
       private List<Course> courseList;
   
       public List<Course> getCourseList() {
           return courseList;
       }
   
       public void setCourseList(List<Course> courseList) {
           this.courseList = courseList;
       }
   
       public void  test(){
           System.out.println(courseList);
       }
   }
   
   ```

   - （2）、创建一个Course类

     ```java
     package com.atguigu.spring.Stu;
     
     public class Course {
         private String cname;
     
         public String getCname() {
             return cname;
         }
     
         public void setCname(String cname) {
             this.cname = cname;
         }
     
         @Override
         public String toString() {
             return "Course{" +
                     "cname='" + cname + '\'' +
                     '}';
         }
     }
     
     ```

     

   -  （3）、xml文件设置

      ```java
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
          <bean id="stu" class="com.atguigu.spring.Stu.Stu">
      <!--        注入list集合类型，值是一个对象-->
              <property name="courseList">
                  <list>
                      <ref bean="course1"></ref>
                      <ref bean="course2"></ref>
                  </list>
              </property>
          </bean>
      <!--创建多个course对象-->
          <bean name="course1" class="com.atguigu.spring.Stu.Course">
              <property name="cname" value="Spring5框架"></property>
          </bean>
          <bean name="course2" class="com.atguigu.spring.Stu.Course">
              <property name="cname" value="java学习"></property>
          </bean>
      
      </beans>
      ```

   -  （4）、测试类

      ```java
      package com.atguigu.spring.Testdemo;
      
      import com.atguigu.spring.Stu.Stu;
      import org.junit.Test;
      import org.springframework.context.ApplicationContext;
      import org.springframework.context.support.ClassPathXmlApplicationContext;
      
      public class TestSpring {
          @Test
          public void TestCollection(){
              ApplicationContext ac=new ClassPathXmlApplicationContext("bean1.xml");
              Stu stu=ac.getBean("stu",Stu.class);
              stu.test();
      
          }
      }
      
      ```

      

   -  2、把集合注入部分提取出来

      -  （1）、在spring配置文件中引入名称空间util

         > ```
         > <?xml version="1.0" encoding="UTF-8"?>
         > <beans xmlns="http://www.springframework.org/schema/beans"
         >        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         >        xmlns:p="http://www.springframework.org/schema/p"
         >        xmlns:util="http://www.springframework.org/schema/util"
         >        xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
         >                             http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd">
         > 
         > </beans>
         > ```

      -  （2）、使用util标签

      ```java
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xmlns:p="http://www.springframework.org/schema/p"
             xmlns:util="http://www.springframework.org/schema/util"
             xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                                  http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd">
      
      <!--    1、提取list集合类型属性注入-->
          <util:list id="bookList">
              <value>易筋经</value>
              <value>九阳神功</value>
          </util:list>
      <!--    2、提取list集合类型属性注入使用-->
          <bean id="book" class="com.atguigu.spring.collectionType.Book">
              <property name="list" ref="bookList"></property>
          </bean>
      </beans>
      ```

      - 创建Book类

      ```java
      package com.atguigu.spring.collectionType;
      import java.util.List;
      
      public class Book {
          private List<String> list;
      
          public void setList(List<String> list) {
              this.list = list;
          }
      
          @Override
          public String toString() {
              return "Book{" +
                      "list=" + list +
                      '}';
          }
      }
      
      ```

      

      - 测试

        ```java
        package com.atguigu.spring.Testdemo;
        
        import com.atguigu.spring.collectionType.Book;
        import com.atguigu.spring.collectionType.Stu;
        import org.junit.Test;
        import org.springframework.context.ApplicationContext;
        import org.springframework.context.support.ClassPathXmlApplicationContext;
        
        public class TestSpring {
            @Test
            public void TestCollection(){
                ApplicationContext ac=new ClassPathXmlApplicationContext("bean2.xml");
                Book book=ac.getBean("book",Book.class);
                System.out.println(book.toString());
            }
        }
        
        ```






#### IOC操作Bean管理（构造器）

:one: 实现：

- 第一步、创建类

```java
package com.company.class2.constructor;

public class Bean1 {
}

```



- 第二步、配置文件

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="BeanId" class="com.company.class2.constructor.Bean1"> </bean>
</beans>
```



- 第三步、Test

```java
package com.company.class2.constructor;

import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class TestMethod {
    @Test
    public void Test1(){
        ApplicationContext ac=new ClassPathXmlApplicationContext("bean2.xml");
        Bean1 bean1=ac.getBean("BeanId",Bean1.class);
        System.out.println(bean1);
    }
}

```



#### IOC 操作Bean管理（静态工厂）

> 使用静态工厂是实例化Bean的另一种方式，该方式要求开发者创建一个静态工厂的方法来创建Bean的实例，其Bean的配置中的class属性所制定的不再是Bean的实现类，而是静态工厂类，同时还需要使用`factory-method` 属性来制定所创建的静态工厂方法。

:one: 实现

- 第一步、创建Bean2类，不需要写方法

```java
package com.company.class2.StaticFactory;

public class Bean2 {
}

```

- 第二步、创建工厂类，并在其中创建一个静态方法返回Bean2实例

```java
package com.company.class2.StaticFactory;

public class Mybean2Factory {
    public static Bean2 creatBean(){
        return new Bean2();
    }
}

```

- 第三步、创建配置文件并注入

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
<bean id="bean2" class="com.company.class2.StaticFactory.Mybean2Factory" factory-method="creatBean">
</bean>
</beans>
```



- 第四步、测试

```java
package com.company.class2.StaticFactory;

import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class TestFatoryBean {
    @Test
    public void TestBean(){
        ApplicationContext ac=new ClassPathXmlApplicationContext("beans2.xml");
        System.out.println(ac.getBean("bean2"));
    }
}

```

![](https://i.loli.net/2021/03/11/wzhui2UpL8Ig7oc.png)

#### IOC操作Bean管理（FactoryBean）

  :one: Spring 有两种类型Bean 一种普通Bean,另外一种是工厂Bean（FactoryBean）

> 普通Bean：在配置文件中定义Bean类型，返回什么类型
>
> 工厂Bean：在配置文件中定义什么类型，可以返回其他类型

:two:  实现

- 1、第一步 创建类，让这个类作为工厂Bean，实现接口FactoryBean

- 2、第二步 实现接口里面的方法，在实现的方法中，定义返回的Bean类型

```java
package com.atguigu.spring.FactoryBean;

import com.atguigu.spring.collectionType.Course;
import org.springframework.beans.factory.FactoryBean;

public class Mybean implements FactoryBean<Course> {
//定义返回bean
    @Override
    public Course getObject() throws Exception {
        Course course=new Course();
        course.setCname("abc");
        return course;
    }

    @Override
    public Class<?> getObjectType() {
        return null;
    }

    @Override
    public boolean isSingleton() {
        return false;
    }
}

```

- 配置文件

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="mybean" class="com.atguigu.spring.FactoryBean.Mybean"></bean>
</beans>
```

- 测试类

```java
package com.atguigu.spring.Testdemo;

import com.atguigu.spring.FactoryBean.Mybean;
import com.atguigu.spring.collectionType.Book;
import com.atguigu.spring.collectionType.Course;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class TestSpring {
    @Test
    public void TestCollection(){
        ApplicationContext ac=new ClassPathXmlApplicationContext("bean3.xml");
        Course course=ac.getBean("mybean", Course.class);
        System.out.println(course);
    }
}

```



#### IOC操作Bean管理(Bean作用域)

:one: 在Spring里面，设置创建Bean实例是单实例还是多实例

:two: 在Spring里面，默认情况下，bean是单实例对象，

- (1)、创建Book类

```java
package com.atguigu.spring.collectionType;
import java.util.List;

public class Book {
    private List<String> list;

    public void setList(List<String> list) {
        this.list = list;
    }
}
```



- (2)、设置xml配置文件

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="book" class="com.atguigu.spring.collectionType.Book">
        <property name="list" >
            <list>
            <value>aaa</value>
            <value>aaa1</value>
            </list>
        </property>
    </bean>
</beans>
```



- (3)、测试

```java
package com.atguigu.spring.Testdemo;

import com.atguigu.spring.collectionType.Book;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class TestSpring {
    @Test
    public void TestCollection(){
        ApplicationContext ac=new ClassPathXmlApplicationContext("bean3.xml");
        Book book1=ac.getBean("book", Book.class);
        Book book2=ac.getBean("book", Book.class);
        System.out.println(book1);
        System.out.println(book2);
    }
}

```

- (4)、输出  单实例的输出地址是一样的

  ![](https://i.loli.net/2021/03/08/IAnPbC8NpslwSV4.png)



:three: 如何设置单实例还是多实例

> 在spring配置文件bean标签里面有属性(`scope`)用于设置单实例还是多实例

- scope属性值

  - 第一个值，默认值 `singleton` 表示是单实例对象
  - 第二个值，`protetype`表示的是多实例对象

- 测试

  - 修改xml配置文件

  ```java
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
      <bean id="book" class="com.atguigu.spring.collectionType.Book" scope="prototype">
          <property name="list" >
              <list>
              <value>aaa</value>
              <value>aaa1</value>
              </list>
          </property>
      </bean>
  </beans>
  ```

  

  - 结果
  - ![](https://i.loli.net/2021/03/08/VsC3gSZxzYDAWBy.png)

- singleton和prototype的区别

> 第一：singleton 单实例，prototype 多实例
>
> 第二：设置scope值是singleton的时候，加载spring配置文件的时候，即` ApplicationContext ac=new ClassPathXmlApplicationContext("xxx.xml");` 的时候就创建单实例对象
>
> ​          设置scope值是prototype的时候，不是在加载spring配置文件的时候创建对象，而是在调用getBean方法的时候创建多实例对象

![](https://i.loli.net/2021/03/11/QtP1eSzrNIuyGDB.png)

![](https://i.loli.net/2021/03/11/MxFTZBf5vKGLEbt.png)

#### IOC操作Bean管理(Bean生命周期)

> 生命周期：从对象创建到对象销毁的过程

- Bean的声明周期
  - (1)、通过构造器创建bean实例(无参数构造)
  - (2)、为bean的属性设置值和对其他bean引用(调用set方法)
  - (3)、调用bean的初始化的方法（需要进行配置）
  - (4)、bean可以使用了(对象获取到了)
  - (5)、当容器关闭的时候，调用bean的销毁的方法(需要进行配置销毁的方法)
- Bean的生命周期(加上后置处理器)
  - (1)、通过构造器创建bean实例(无参数构造)
  - (2)、为bean的属性设置值和对其他bean引用(调用set方法)
  - <font color="red">(3)、把bean实例传递bean后置处理器的方法</font>(postProcessBeforeInitialization)
  - (4)、调用bean的初始化的方法（需要进行配置）
  - <font color="red">(5)、把bean实例传递bean后置处理器的方法</font>(postProcessAfterInitialization)
  - (6)、bean可以使用了(对象获取到了)
  - (7)、当容器关闭的时候，调用bean的销毁的方法(需要进行配置销毁的方法)

![](https://i.loli.net/2021/03/12/nmAfLVjZKN7CdRx.png)

`演示bean 的声明周期`

（1）、Orders类

```java
package com.atguigu.spring.Bean;

public class Orders {
//    无参数构造
    public Orders() {
        System.out.println("第一步：执行无参数构造创建bean实例");
    }
    private String oname;
    public void setOname(String oname) {
        this.oname = oname;
        System.out.println("第二步：调用set方法 设置属性值");
    }
//    创建执行的初始化方法
 public void initMethod(){
     System.out.println("第三步：执行初始化的方法");
 }
//     创建执行销毁的方法
    public void destoryMethod(){
        System.out.println("第五步：销毁的方法");
    }
}

```

（2）、配置文件

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="orders" class="com.atguigu.spring.Bean.Orders" init-method="initMethod" destroy-method="destoryMethod"> <!-- 初始化方法和销毁方法-->
        <property name="oname" value="手机">
        </property>
    </bean>

</beans>
```



（3）、测试方法

```java
 @Test
    public void TestBean1(){

        ClassPathXmlApplicationContext ac=new ClassPathXmlApplicationContext("bean4.xml");
        Orders orders=ac.getBean("orders", Orders.class);
        System.out.println("第四步：获取创建bean实例对象");
        System.out.println(orders);
//        手动让bean实例销毁
        ac.close();//需要将 ApplicationContext改为ClassPathXmlApplicationContext
    }
```



（4）、结果

![](https://i.loli.net/2021/03/08/ROSqlPfXbsGuaEN.png)



`演示带有后置处理器方法的bean生命周期`

(1)、创建类，实现接口BeanPostProcessor，创建后置处理器

```java
package com.atguigu.spring.Bean;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanPostProcessor;
import org.springframework.lang.Nullable;

public class MyBeanPost  implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("在我们的初始化之前执行的方法");
        return bean;
    }

    @Override
   public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("在初始化之后执行的方法");
        return bean;
    }
}

```



(2)、配置文件

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="orders" class="com.atguigu.spring.Bean.Orders" init-method="initMethod" destroy-method="destoryMethod"> <!-- 初始化方法和销毁方法-->
        <property name="oname" value="手机">
        </property>
    </bean>
<!--配置后置处理器   配置文件中的每一个bean都会有配置处理器 此时你看到的配置文件样子中 有两个bean，表示会创建两个后置处理器-->
    <bean id="mybeanpost" class="com.atguigu.spring.Bean.MyBeanPost"></bean>
</beans>
```

(3)、实现BeanPostProcessor接口

```java
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by Fernflower decompiler)
//

package org.springframework.beans.factory.config;

import org.springframework.beans.BeansException;
import org.springframework.lang.Nullable;

public interface BeanPostProcessor {
    @Nullable
    default Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        return bean;
    }

    @Nullable
    default Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        return bean;
    }
}

```



(4)、测试

```java
    @Test
    public void TestBean1(){

        ClassPathXmlApplicationContext ac=new ClassPathXmlApplicationContext("bean4.xml");
        Orders orders=ac.getBean("orders", Orders.class);// 这里用的是orders 根据上面配置文件的描述 也会显示
        System.out.println("第四步：获取创建bean实例对象");
        System.out.println(orders);
//        手动让bean实例销毁
        ac.close();//需要将 ApplicationContext改为ClassPathXmlApplicationContext
    }
```

(5)、结果

![](https://i.loli.net/2021/03/09/l237hpAnDr9ktVT.png)





#### IOC操作 Bean管理(xml自动装配)

> 什么是自动装配？
>
> ​	答：根据指定装配规则（属性名称或者属性类型），spring自动将匹配的属性值进行注入。

`演示自动装配`

- 根据名称自动装配

- (1)、Emp类

  - ```java
    package com.atguigu.spring.autowrite;
    
    public class Emp {
       private Dept dept;
    
        public void setDept(Dept dept) {
            this.dept = dept;
        }
    
        @Override
        public String toString() {
            return "Emp{" +
                    "dept=" + dept +
                    '}';
        }
    
        public void test(){
            System.out.println(dept);
        }
    }
    
    ```

  - (2)、Dept类

  - ```java
    package com.atguigu.spring.autowrite;
    
    public class Dept {
        @Override
        public String toString() {
            return "Dept{}";
        }
    }
    
    ```

  - (3)、配置文件

  - ```java
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--   实现自动装配
            bean标签属性autowire 配置自动装配
            autowire 属性常用两个值
            byName 根据属性名称注入，注入值bean的id和类属性名称一样
            byType 根据属性类型注入
        -->
         <bean id="emp" class="com.atguigu.spring.autowrite.Emp" autowire="byName">
    <!--      这个是以前写的手动装配的方式   <property name="dept" ref="dept"></property>-->
         </bean>
    
         <bean id="dept" class="com.atguigu.spring.autowrite.Dept"></bean>
    </beans>
    ```

  - (4)、测试类

  - ```java
      @Test
        public void TestAutowrite1(){
      
            ClassPathXmlApplicationContext ac=new ClassPathXmlApplicationContext("bean5.xml");
            Emp emp=ac.getBean("emp", Emp.class);
            System.out.println(emp);
         }
    ```

  - (5)、结果

  - ![](https://i.loli.net/2021/03/09/zstqfEw6pI7GDNe.png)

- 根据类型进行注入

  - (1)、将autowire属性值改为byType

  - ```java
         <bean id="emp" class="com.atguigu.spring.autowrite.Emp" autowire="byType">
    <!--      这个是以前写的手动装配的方式   <property name="dept" ref="dept"></property>-->
         </bean>
    
         <bean id="dept" class="com.atguigu.spring.autowrite.Dept"></bean>
    ```

  - (2)、结果

  - ![](https://i.loli.net/2021/03/09/jobmvNAyhqu5cpe.png)





#### IOC操作Bean管理（外部属性文件）

1、直接配置数据库信息

（1）、配置德鲁伊连接池

（2）、引入德鲁伊连接池依赖jar包

> jar包链接：https://pan.baidu.com/s/1Md6pEWwmVPgL4YMRZxXC8w 
> 提取码：3jf8 

- 手动配置文件

- ```java
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
  <!--直接配置连接池-->
  
            <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
                  destroy-method="close">
                 <property name="url" value="jdbc:mysql://localhost:3306/userDb" />
                 <property name="username" value="root" />
                 <property name="password" value="root" />
                 <property name="driverClassName" value="com.mysql.jdbc.Driver" />
            </bean>
  </beans>
  ```

  



2、通过引入外部属性文件配置数据库连接池

（1）、创建外部属性文件，properties格式文件，写数据库信息

![](https://i.loli.net/2021/03/09/YnLyKcx9eaOhZRD.png)

（2）、把外部properties属性文件引入到spring配置文件中

- 引入context名称空间

```java
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                            http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd
                            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

```

- 在spring配置文件使用标签引入外部属性文件

- ```java
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xmlns:util="http://www.springframework.org/schema/util"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                              http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd
                              http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
  <!--     引入外部属性文件-->
       <context:property-placeholder location="jdbc.properties"></context:property-placeholder>
  <!--               配置连接池-->
       <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"destroy-method="close">
                      <property name="url" value="${prop.url}" /> 
                      <property name="username" value="${prop.useName}" />
                      <property name="password" value="${prop.password}" />
                      <property name="driverClassName" value="${prop.driverClassName}" />
                 </bean>
  </beans>
  ```


