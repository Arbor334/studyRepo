## Spring[4]_Aop

> 什么是AOP？
>
>  AOP为Aspect Oriented Programming
>
> 1）面向切面编程，利用Aop可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发效率
>
> 2）通俗解释，不通过修改软代码方式，在主干功能里面添加新功能

:one: 例子进行解释

![](https://i.loli.net/2021/03/15/4JZTo2ciNdK8rDs.png)

> 网站通过用户名密码进行登录，在用数据库进行信息判断。
>
> 我们想在登录基础上添加功能，但是又不想修改源代码，
>
> 这个时候就可以使用一个模块进行判断，写好后直接调用就可以。当不需要这个模块的时候，不进行调用就可以
>
> 这种不修改源代码方式添加新功能的方式就是AOP

:two: 底层原理

> 一、Aop底层使用动态代理方式
>
>  	1、有两种情况动态代理
>
> ​		1）第一种接口情况，使用JDK动态代理
>
> ​			使用代理对象进行增强功能
>
> ​       2）第二种没有接口情况，使用CGLIB动态代理
>
> ​     

![](https://i.loli.net/2021/03/15/hb7KJsgw1tzjlvx.png)

![](https://i.loli.net/2021/03/15/ELV4KS3himluAY7.png)

:three:AOP(JDK动态代理)

> 1、使用JDK动态代理，使用Proxy类里面的方法创建 
>
> ​	1)调用newProxyInstance方法

![](https://i.loli.net/2021/03/15/r42FhoZ7tdKvSsJ.png)

![](https://i.loli.net/2021/03/15/W4YbG1uqdvBNMaS.png)

>   方法里面有三个参数：
>
> ​		1、类加载器
>
> ​		2、增强方法所在的类，这个类实现的接口，支持多个接口
>
> ​		3、实现这个接口InvocationHandler 创建代理对象，写增强的部分

:four: 编写JDK动态代理代码

- 1、创建接口，定义方法

```java
package com.atguigu.spring.test.ProxyTest;

public interface boyDao {
    public int add(int a,int b);
    public String  update(String id);
}

```

- 2、创建接口实现类，实现方法

```java
package com.atguigu.spring.test.ProxyTest;

public class boyDaoImpl implements boyDao{
    @Override
    public int add(int a, int b) {
        System.out.println("add执行了");
        return a+b;
    }

    @Override
    public String update(String id) {
        System.out.println("update执行了");
        return id;
    }
}


```

- 3、使用Proxy类创建接口代理对象

```java
package com.atguigu.spring.test.ProxyTest;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
import java.util.Arrays;

public class JDKproxy {
    public static void main(String[] args) {
//       创建接口实现类的代理对象
        Class[] interfaces={boyDao.class};
//        下面是写一个InvocationHandler的匿名内部类的方式
//        Proxy.newProxyInstance(JDKproxy.class.getClassLoader(), interfaces, new InvocationHandler() {
//            @Override
//            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
//                return null;
//            }
//        });
        boyDaoImpl boyDao=new boyDaoImpl();
        boyDao Dao = (boyDao)Proxy.newProxyInstance(JDKproxy.class.getClassLoader(), interfaces, new boyDaoProxy(boyDao));
        int result=Dao.add(1,2);
        System.out.println("result:"+result);
    }
}
//创建代理对象代码
class boyDaoProxy implements InvocationHandler{
//    1、把创建的是·谁·的代理对象，把·谁·传递进来
//       通过有参构造传递
    private  Object obj;
    public  boyDaoProxy(Object obj){
        this.obj=obj;
    }

//增强的逻辑
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
//    方法之前
        System.out.println("方法之前执行"+method.getName()+"传递的参数"+ Arrays.toString(args));
//被增强的方法执行
        method.invoke(obj,args);
        Object res=method.invoke(obj,args);
//     方法之后
        System.out.println("方法之后执行"+obj);


        return res;
    }
}
```

![](https://i.loli.net/2021/03/15/Skg81dPB6euKnQs.png)



:five: AOP（术语）

> 1、连接点
>
> 2、切入点
>
> 3、通知（增强）
>
> 4、切面

![](https://i.loli.net/2021/03/15/4j9tzdBYNGCJOFX.png)

:six: AOP操作准备

> 1、spring 框架一般都是基于AspectJ实现AOP操作
>
> ```java
> 1）什么是AspectJ？
> ```
>
> ​			AspectJ不是Spring组成部分，独立于AOP框架，一般把AspectJ和Spring框架一起使用，进行AOP操作
>
> 2、基于AspectJ实现AOP操作
>
> ```java
> 1) 基于xml配置文件实现
> 2）基于注解方式实现（使用）
> ```
>
> 3、在项目工程里面引入AOP相关依赖

![](https://i.loli.net/2021/03/15/ixvP9A8Tk6wXJcL.png)

![](https://i.loli.net/2021/03/15/hrspP4u7a3Vfd6t.png)

> 4、切入点表达式
>
> ```java
> 1）切入点表达式作用：知道那哪个类里面的哪个方法进行增强
> 
> 2）语法结构
> 
> execution([权限修饰符][返回值类型][类全路径][方法名称]([参数列表]))
> 举例1：对com.xttc.Test.HumanDao类中的add方法进行增强
> execution(* com.xttc.Test.HumanDao.add(参数列表))
> 举例2：对com.xttc.Test.HumanDao类中的所有方法进行增强
> execution (* com.xttc.Test.HumanDao.*())
> 举例3：对com.xttc.Test包中的所有方法进行增强
> execution (* com.xttc.Test.*.*())
> ```

:seven: AOP（AspectJ注解）

- 1、创建类，在类里面定义方法

```java
package com.atguigu.spring.test.aopanno;

public class User
{
    public void add(){
        System.out.println("add..");
    }
}

```



- 2、创建增强类（编写增强逻辑）

> ```java
> 1)在增强类里面，创建方法，让不同方法代表不同通知类型
> package com.atguigu.spring.test.aopanno;
> //被增强的类
> public class Proxy {
>     public void before(){
> //        前置通知
>         System.out.println("before...");
>     }
> }
> 
> ```
>
> 

- 3、进行通知的配置
  - 1)在spring配置文件中，开启注解扫描

  ```java
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xmlns:aop="http://www.springframework.org/schema/aop"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                              http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
                             http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
  <!--开启注解扫描-->
      <context:component-scan base-package="com.atguigu.spring.test.aopanno"></context:component-scan>
  
  </beans>
  ```

  

  - 2)使用注解创建Uer和Proxy对象

  ![](https://i.loli.net/2021/03/15/FZDWjCaRi3cTAtr.png)

  ![](https://i.loli.net/2021/03/15/FZDWjCaRi3cTAtr.png)

  - 3)在增强类上面添加注解@Aspect

  ```java
  package com.atguigu.spring.test.aopanno;
  
  import org.aspectj.lang.annotation.Aspect;
  import org.aspectj.lang.annotation.Before;
  import org.springframework.stereotype.Component;
  //被增强的类
  @Component
  @Aspect  //生成代理对象
  public class Proxy {
  //        前置通知
  //    @Before注解表示作为前置通知
      @Before(value = "execution(* com.atguigu.spring.test.aopanno.User.add(..))")
      public void before(){
          System.out.println("before......");
      }
  }
  
  ```

  

  - 4)在spring配置文件中开启生成代理对象

```java
<!--开启Aspect生成代理对象-->
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
```



- 4、

  - - 1)在增强类的里面，在作为通知方法上面添加通知类型注解，使用切入点表达式来配置内容

    ```JAVA
    import org.aspectj.lang.annotation.Aspect;
    import org.aspectj.lang.annotation.Before;
    import org.springframework.stereotype.Component;
    
    @Component
    //被增强的类
    @Aspect  //生成代理对象
    public class Proxy {
    //        前置通知
    //    @Before注解表示作为前置通知
        @Before(value = "execution(* com.atguigu.spring.test.aopanno.User.add())")
        public void before(){
            System.out.println("before...");
        }
    }
    
    ```

    

    - 2）测试方法

    ```java
    package com.atguigu.spring.test.aopanno;
    
    import org.junit.Test;
    import org.springframework.context.ApplicationContext;
    import org.springframework.context.support.ClassPathXmlApplicationContext;
    
    public class ProxyTest {
        @Test
        public void TestAOP(){
            ApplicationContext ac=new ClassPathXmlApplicationContext("bean2.xml");
            User user = ac.getBean("user", User.class);
    
            user.add();
        }
    }
    
    ```



结果

![](https://i.loli.net/2021/03/15/YOCcUxMZslTmdDf.png)



`其他`

```java
package com.atguigu.spring.test.aopanno;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.*;
import org.springframework.stereotype.Component;
//被增强的类
@Component
@Aspect  //生成代理对象
public class Proxy {
//        前置通知
//    @Before注解表示作为前置通知
    @Before(value = "execution(* com.atguigu.spring.test.aopanno.User.add(..))")
    public void before(){
        System.out.println("before......");
    }
    @After(value = "execution(* com.atguigu.spring.test.aopanno.User.add(..))")
    public void after(){
        System.out.println("afteradd..");
    }
    @AfterThrowing(value = "execution(* com.atguigu.spring.test.aopanno.User.add(..))")
    public void afterTh(){
        System.out.println("afterThrow");
    }
    @AfterReturning(value = "execution(* com.atguigu.spring.test.aopanno.User.add(..))")
    public void afterReturn(){
        System.out.println("after return");
    }
//    环绕通知
    @Around(value = "execution(* com.atguigu.spring.test.aopanno.User.add(..))")
    public void around(ProceedingJoinPoint proceedingJoinPoint) throws  Throwable{
        System.out.println("环绕之前");
        proceedingJoinPoint.proceed();
        System.out.println("环绕之后");
    }
}

```

- 5、相同切入点的配置

```java
//    相同切入点抽取
     @Pointcut(value = "execution(* com.atguigu.spring.test.aopanno.User.add(..))")
     public void pointcut(){

     }

//        前置通知
//    @Before注解表示作为前置通知
    @Before(value = "pointcut()")
    public void before(){
        System.out.println("before......");
    }
```

- 6、有多个增强类对同一种方法进行增强，设置增强类优先级
  - 1）在增强类上面添加注解Order(数字类型值 数字值越小优先级越高)

```java
//被增强的类
@Component
@Aspect  //生成代理对象
@Order(1)
public class Proxy {
```



:eight: AOP（AapectJ配置文件）​

一、创建类和增强类

```java
package com.atguigu.spring.test.AOPannotation;

public class Book {
    public void buy(){
        System.out.println("buy//");
    }
}

```

```java
package com.atguigu.spring.test.AOPannotation;

import org.aspectj.lang.JoinPoint;



public class BookProxy {
    public void buy(){
        System.out.println("before ");
    }

    public void before(JoinPoint joinPoint) throws Throwable {
    }
}

```

二、配置文件

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                            http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
<!--创建对象-->
    <bean id="book" class="com.atguigu.spring.test.AOPannotation.Book"></bean>
    <bean id="bookProxy" class="com.atguigu.spring.test.AOPannotation.BookProxy"></bean>
<!-- 配置aop增强-->
    <aop:config>
<!--        切入点-->
        <aop:pointcut id="p" expression="execution(* com.atguigu.spring.test.AOPannotation.Book.buy(..))"></aop:pointcut>
<!--    配置切面-->
        <aop:aspect ref="bookProxy">
<!--            配置增强所用在具体的方法上-->
            <aop:before method="buy" pointcut-ref="p"/>
        </aop:aspect>
    </aop:config>
</beans>
```

三、测试类

```java
package com.atguigu.spring.test.AOPannotation;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {
    @org.junit.Test
    public  void Test(){
        ApplicationContext ac=new ClassPathXmlApplicationContext("bean2.xml");
        Book book = ac.getBean("book",Book.class);
        book.buy();

    }
}

```

四、结果

![](https://i.loli.net/2021/03/15/CGN6X5iy8AQgR3a.png)
