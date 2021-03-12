## IOC容器 bean管理注解方式

> 什么是注解？
>
> （1）注解是代码特殊标记，格式：@注解名称(属性名称=属性值，属性名称=属性值)
>
> （2）使用注解，注解作用在类上面，方法上面，属性上面
>
> （3）使用注解目的：简化xml配置

### IOC容器 bean管理注解方式（创建对象）

> spring针对Bean管理中创建对象提供注解
>
> 1、@Component 
>
> 2、@Service       对service层实现类的注解
>
> 3、@Controller	对控制器实现类的注解
>
> 4、@Repository   对于Dao实现类的注解
>
> 上面的四个注解，他们的功能是一样的，都可以用来创建bean实例

![](https://i.loli.net/2021/03/12/5QSbmjVeEBnJfoY.png)

#### :one: 基于注解方式实现对象创建

（1）、引入aop依赖

![](https://i.loli.net/2021/03/09/f7yPBI3dRbuFLrE.png)

（2）、开启组件扫描

> 告诉spring我要到哪里去进行注解
>
> 需要引用context 名称空间

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
<!--开启组件扫描
 1、如果扫描多个包 多个包使用逗号隔开
 2、扫描包上层目录
-->
    <context:component-scan base-package="com.atguigu.spring.testDemo.TestDao,com.atguigu.spring.testDemo.TestService"></context:component-scan>
</beans>
```

（3）、创建类，在类上面要添加创建对象注解

- 类

```java
package com.atguigu.spring.test.TestService;

import org.springframework.stereotype.Component;
//在注解里面value属性值可以省略不写
//默认值是类名称，首字母小写
//UserService-- userService
@Component(value ="userService")// <bean id="userService" class>   除了Component 其他三个也是可以的
public class UserService {
    public void add(){
        System.out.println("service add .");
    }
}

```

- 配置文件

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
<!--开启组件扫描
 1、如果扫描多个包 多个包使用逗号隔开
 2、扫描包上层目录
-->
    <context:component-scan base-package="com.atguigu.spring.test.TestDao,com.atguigu.spring.test.TestService"></context:component-scan>
</beans>
```

- 测试类	

```java
package com.atguigu.spring.test.testDemo;

import com.atguigu.spring.test.TestService.UserService;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class TestspringDemo {
   @Test
    public void testService(){
       ApplicationContext ac=new ClassPathXmlApplicationContext("bean1.xml");
       UserService userService=ac.getBean("userService",UserService.class);
       System.out.println(userService);

       userService.add();
   }
}
```

- 结果

![](https://i.loli.net/2021/03/09/sOnuP6o9kvqQeDJ.png)



###  IOC容器 Bean管理注解方式（组件扫描配置）

> 下面两个实例 包含 
>
> include-filter
>
> exclude-filter
>
> 看看就会了

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
<!--开启组件扫描
 1、如果扫描多个包 多个包使用逗号隔开
 2、扫描包上层目录
-->
    <context:component-scan base-package="com.atguigu.spring.test.TestDao,com.atguigu.spring.test.TestService"></context:component-scan>

<!--    组件配置
   设置的更详细一点
  实例1： use-default-filters="false" 表示现在不使用默认的filter 而是使用自己配置的filter
         context:include-filter 设置扫描哪些内容
         type="annotation"  expression="...Controller" 只扫描是Controller注解的类
-->
    <context:component-scan base-package="com.atguigu.spring.test" use-default-filters="false">
        <context:include-filter type="annotation"
                                expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>

<!--    实例2：
        下面配置扫描包中的不包含哪些东西的其他所有内容
        context:exclude-filter ： 设置哪些内容不进行扫描
        type="annotation"  expression="...Controller" 和exclude-filter搭配就是不扫描包含Controller注解的类
-->
    <context:component-scan base-package="com.atguigu.spring.test">
        <context:exclude-filter type="annotation"
                                expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>

</beans>
```



### IOC容器Bean管理注解方式（实现属性注入）

- @Autowired

> 根据属性类型进行自动注入

- @Qualifier

> 根据属性名称进行注入 要和Autowired一起使用

- @Resource

> 可以根据类型注入，也可以根据名称注入

- @Value

> 注入普通类型属性

#### :one: 实现@Autowried

- 第一步、把service和dao对象创建，在service和dao类中添加对象注解

> 创建 HumanService 类  HumanDao接口  HumanDaoImpl 类
>
> 并对service 和dao的实现类（不是dao接口）添加对象注解

```java
package com.atguigu.spring.test.TestDao;

import org.springframework.stereotype.Service;

@Service  
public class HumanService {
}

```

```java
package com.atguigu.spring.test.TestDao;

public interface HumanDao {
    public void add();
}

```

```java
package com.atguigu.spring.test.TestDao;

import org.springframework.stereotype.Repository;

@Repository
public class HumanDaoImpl implements HumanDao{
    @Override
    public void add() {
      System.out.println("humanDaoImpl add");
    }
}

```



- 第二步、在service注入dao对象，在service类添加dao类型属性，在属性上面进行注解

```java
package com.atguigu.spring.test.TestDao;

        import org.springframework.beans.factory.annotation.Autowired;
        import org.springframework.stereotype.Service;

@Service
public class HumanService {
//    定义dao属性
//    不需要添加set方法
//    添加属性注解
    @Autowired //根据类型进行注入
    private HumanDao humanDao;

    public void add(){
        System.out.println("service add...");
        humanDao.add();
    }
}

```

- 第三步、测试

  - Test类

  ```java
  package com.atguigu.spring.test.TestDao;
  
  import com.atguigu.spring.test.TestService.UserService;
  import org.springframework.context.ApplicationContext;
  import org.springframework.context.support.ClassPathXmlApplicationContext;
  
  public class Test {
      @org.junit.Test
      public void TestDemo(){
          ApplicationContext ac=new ClassPathXmlApplicationContext("bean1.xml");
          HumanService humanService=ac.getBean("humanService",HumanService.class);
          System.out.println(humanService);
          humanService.add();
      }
  }
  ```

  

  - 配置文件(要开启组件扫描)

  ```java
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:context="http://www.springframework.org/schema/context"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                              http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
  <!--开启组件扫描
   1、如果扫描多个包 多个包使用逗号隔开
   2、扫描包上层目录
  -->
      <context:component-scan base-package="com.atguigu.spring.test.TestDao"></context:component-scan>
  </beans>
  ```

- 结果

![](https://i.loli.net/2021/03/12/AvoX9Hk3Vm5cNPS.png)





#### :two:实现@Qualifier

> 实现@Qualifier 需要和@Autowired一起使用
>
> 简单来说  一个接口有多个实现类，但是名字不一样，Qualifier就是根据名称去找实现类
>
> 比如说 HumanDao有两个实现类 HumanDaoImpl2(这里面已经有注解@Repository(value = "humanDaoImpl2") )和HumanDaoImpl3 (这里面已经有注解@Repository(value = "humanDaoImpl3"))
>
> 我们需要的是HumanDaoImpl2 就可以用@Qualifier (value="humanDaoImpl2 ")

> 下面的实例仅改变了HumanDaoImpl 和HumanService 其他的文件和上面的相同

```java
package com.atguigu.spring.test.TestDao;

        import org.springframework.beans.factory.annotation.Autowired;
        import org.springframework.beans.factory.annotation.Qualifier;
        import org.springframework.stereotype.Service;

@Service
public class HumanService {
//    定义dao属性
//    不需要添加set方法
//    添加属性注解
    @Autowired //根据类型进行注入
    @Qualifier(value = "humanDaoImpl2") //和上面相比 添加了的
    private HumanDao humanDao;

    public void add(){
        System.out.println("service add...");
        humanDao.add();
    }
}

```

```java
package com.atguigu.spring.test.TestDao;

import org.springframework.stereotype.Repository;

import javax.naming.Name;

@Repository(value = "humanDaoImpl2") 
public class HumanDaoImpl2 implements HumanDao{
    @Override
    public void add() {
        System.out.println("humanDaoImpl add");
    }
}

```

- 结果

![](https://i.loli.net/2021/03/12/h5n6guvBf4pRJ9F.png)



#### :three:实现@Resource

> 修改了HumanService类 使用@Resource两种方式都可以实现上面的结果

```java
package com.atguigu.spring.test.TestDao;

        import org.springframework.beans.factory.annotation.Autowired;
        import org.springframework.beans.factory.annotation.Qualifier;
        import org.springframework.stereotype.Service;

        import javax.annotation.Resource;

@Service
public class HumanService {
//    定义dao属性
//    不需要添加set方法
//    添加属性注解
// @Resource    //根据类型进行注入
    @Resource(name = "humanDaoImpl2")
    private HumanDao humanDao;

    public void add(){
        System.out.println("service add...");
        humanDao.add();
    }
}

```



#### :four: 实现@value

> 只需要在 属性上面加上@Value(value="xxxx")就可以了

```java
package com.atguigu.spring.test.TestDao;

        import org.springframework.beans.factory.annotation.Autowired;
        import org.springframework.beans.factory.annotation.Qualifier;
        import org.springframework.beans.factory.annotation.Value;
        import org.springframework.stereotype.Service;

        import javax.annotation.Resource;

@Service
public class HumanService {
//    定义dao属性
//    不需要添加set方法
//    添加属性注解
// @Resource    //根据类型进行注入
    @Resource(name = "humanDaoImpl2")
    private HumanDao humanDao;
//    属性值注入
    @Value(value = "abc")
    private String name;

    public void add(){
        System.out.println("service add..."+name);
        humanDao.add();
    }
}

```

- 测试代码没有变化
- 结果

![](https://i.loli.net/2021/03/12/9mUeuaWgHhylLx7.png)





### IOC容器Bean管理注解方式（完全注解开发）

- 第一步、创建配置类，替代xml配置文件

```java
package com.atguigu.spring.test.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration //作为配置类，替代xml配置文件
@ComponentScan(basePackages = {"com.atguigu.spring.test"}) //相当于上面的<context:component-scan base-package=""></context:component-scan>
public class SpringConfig {
}

```



- 第二步、编写测试类

```java
package com.atguigu.spring.test.config;


import com.atguigu.spring.test.TestDao.HumanService;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class SpringTest {
//    1、这是之前有xml配置文件时候我们写的
//    @Test
//    public void testService(){
//        ApplicationContext ac=new ClassPathXmlApplicationContext("bean1.xml");
//        UserService userService=ac.getBean("userService",UserService.class);
//        System.out.println(userService);
//
//        userService.add();
//    }
//    2、完全注解开发使用的
    @Test
public void TestAnnotation() {
        ApplicationContext ac=new AnnotationConfigApplicationContext(SpringConfig.class);//这里改变了
        HumanService humanService=ac.getBean("humanService",HumanService.class);
        System.out.println(humanService);
        humanService.add();
        
    }
}

```

