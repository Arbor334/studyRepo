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

### :one:、下载Spring5最新稳定版本  `GA表示稳定版`  	`SNAPSHOT表示测试版`

![](https://i.loli.net/2021/01/05/ycj25gvUlLhFW9J.png)

### :two:、[下载地址](https://repo.spring.io/release/org/springframework/spring/)

- `找到最新的RELEASE下载  xxxxRELEASE-dist.zip解压就可以了`
- ![](https://i.loli.net/2021/01/05/UOyb3H5Y72w6eqn.png)

### :three:、打开IDEA工具

- `创建一个新的java工程``导入Spring5的jar包``spring5 的模块`![](https://i.loli.net/2021/01/05/oTUekN5F1wRSuYj.png)`导入Beans、 Core 、Context、 Expression  还有额外的logging  jar包`
- ![](https://i.loli.net/2021/01/05/J3BuYdr7Q19pVnv.png)
- 在idea 中的 project Structure 中导入包

![](https://i.loli.net/2021/01/05/gVaFb67szKuhfPi.png)



![](https://i.loli.net/2021/01/05/P1a2iplHNfLIC7x.png)

### :four:、创建普通类，在这个类创建普通方法

```java
package com.atgui.spring5;

public class User {
    public void add(){
        System.out.println("add method");
    }

}

```



### :five:、创建Spring配置文件，在配置文件配置创建的对象

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

### :six:、进行测试代码编写

![](https://i.loli.net/2021/01/14/VlvGk7e2XMqNL1d.png)

