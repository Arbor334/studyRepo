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
>   	1）什么是AspectJ？
>
> ​			AspectJ不是Spring组成部分，独立于AOP框架，一般把AspectJ和Spring框架一起使用，进行AOP操作