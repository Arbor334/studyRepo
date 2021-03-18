## Spring[5]_JdbcTemplate

> 什么是JDBCTemplate？
>
>   spring对JDBC进行了封装，使用JDBCTemplate方便实现对数据库的操作
>
> 

### :one: 准备工作

一、导入需要的jar包

![](https://i.loli.net/2021/03/16/D7INJ9H4kls8ho1.png)

二、在spring配置文件配置数据库连接池

```java
 <!-- 配置连接池 -->
    <!-- DruidDataSource dataSource = new DruidDataSource(); -->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <!-- dataSource.setDriverClassName("com.mysql.jdbc.Driver");
            set方法注入
        -->
        <!-- 获取properties文件内容，根据key获取，使用spring表达式获取 -->
        <property name="driverClassName" value="${jdbc.driverclass}"></property>
        <property name="url" value="${jdbc.url}"></property>
        <property name="username" value="${jdbc.username}"></property>
        <property name="password" value="${jdbc.password}"></property>
    </bean>
```

三、配置JdbcTemplate对象,注入dataSource

```java
<!--   jdbcTemplate对象-->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
<!--        注入datasource-->
        <property name="dataSource" ref="dataSource" />
    </bean>
```

四、创建service类，创建dao类，在dao注入jbdcTemplate对象

- 在配置文件中要开启组件扫描

```java
<!--    组件扫描-->
    <context:component-scan base-package="com.atguigu"></context:component-scan>

```



- 创建service类，注入dao对象

```java

import com.atguigu.spring.test.dao.bookDao;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class bookService {
//    注入dao
    @Autowired
    private bookDao bookDao;
}

```



- 创建dao的实现类

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Repository;

@Repository
public class bookDaoImpl {
//    注入jdbcTemplate
    @Autowired
    private JdbcTemplate jdbcTemplate;

}

```

### :two: 操作数据库的添加操作

![](https://i.loli.net/2021/03/16/kdDVBQrU1jYybXR.png)

- 一、对应数据库表，创建实体类

```java
package com.atguigu.spring.test.entity;

public class Book {
    private String bookid;
    private String bookname;
    private String bookstatus;

    public String getBookid() {
        return bookid;
    }

    public void setBookid(String bookid) {
        this.bookid = bookid;
    }

    public String getBookname() {
        return bookname;
    }

    public void setBookname(String bookname) {
        this.bookname = bookname;
    }

    public String getBookstatus() {
        return bookstatus;
    }

    public void setBookstatus(String bookstatus) {
        this.bookstatus = bookstatus;
    }
}

```



- 二、编写service和dao

  - 1）在dao进行数据库添加操作

  ```java
  package com.atguigu.spring.test.dao;
  
  
  import com.atguigu.spring.test.entity.Book;
  
  public interface bookDao {
      public void add(Book book);
  }
  
  ```

  ```java
  package com.atguigu.spring.test.service;
  
  import com.atguigu.spring.test.dao.bookDao;
  import com.atguigu.spring.test.entity.Book;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.stereotype.Service;
  
  @Service
  public class bookService {
  //    注入dao
      @Autowired
      private bookDao bookDao;
  //    添加方法
    public void addBook(Book book){
        bookDao.add(book);
  
    }
  
  }
  
  ```

  ```java
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                              http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
  
  <!--    组件扫描-->
      <context:component-scan base-package="com.atguigu"></context:component-scan>
      <!--     引入外部属性文件-->
      <context:property-placeholder location="jdbc.properties"></context:property-placeholder>
  
      <!-- 配置连接池 -->
      <!-- DruidDataSource dataSource = new DruidDataSource(); -->
      <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
          <!-- dataSource.setDriverClassName("com.mysql.jdbc.Driver");
              set方法注入
          -->
          <!-- 获取properties文件内容，根据key获取，使用spring表达式获取 -->
          <property name="driverClassName" value="${prop.driverClassName}"></property>
          <property name="url" value="${prop.url}"></property>
          <property name="username" value="${prop.userName}"></property>
          <property name="password" value="${prop.password}"></property>
      </bean>
  <!--   jdbcTemplate对象-->
      <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
  <!--        注入datasource-->
          <property name="dataSource" ref="dataSource" />
      </bean>
  </beans>
  ```

  ```java
  prop.driverClassName=com.mysql.cj.jdbc.Driver
  prop.url=jdbc:mysql://localhost:3306/jdbctemplate
  prop.userName=root
  prop.password=root
  ```

  ```java
  package com.atguigu.spring.test.Test;
  
  import com.atguigu.spring.test.entity.Book;
  import com.atguigu.spring.test.service.bookService;
  import org.junit.Test;
  import org.springframework.context.ApplicationContext;
  import org.springframework.context.support.ClassPathXmlApplicationContext;
  
  import javax.xml.ws.Service;
  
  public class jdbcTemplateTest {
      @Test
      public void TestAdd(){
          ApplicationContext ac=new ClassPathXmlApplicationContext("bean1.xml");
          bookService bookService=ac.getBean("bookService", bookService.class);
          Book book=new Book();
          book.setBookid("1");
          book.setBookname("java");
          book.setBookstatus("a");
          bookService.addBook(book);
      }
  }
  
  ```

  - 2）调用jdbcTemplate对象里面的update方法实现添加操作

  ```java
  package com.atguigu.spring.test.dao;
  
  import com.atguigu.spring.test.entity.Book;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.jdbc.core.JdbcTemplate;
  import org.springframework.stereotype.Repository;
  
  @Repository
  public class bookDaoImpl implements bookDao{
  //    注入jdbcTemplate
      @Autowired
      private JdbcTemplate jdbcTemplate;
  //添加方法
      @Override
      public void add(Book book) {
  //        1、创建sql语句
          String sql="insert into t_unit values(?,?,?)";
  //      2、调用方法实现
          Object[] args={book.getBookid(),book.getBookname(),book.getBookstatus()};
        int update=  jdbcTemplate.update(sql,args);
          System.out.println(update);
      }
  }
  ```



### :three: 删改



```java
package com.atguigu.spring.test.dao;


import com.atguigu.spring.test.entity.Book;

public interface bookDao {
    public void addBook(Book book);
    public void updateBook(Book book);
    public void deleteBook(String id);
}

```

```java
package com.atguigu.spring.test.service;

import com.atguigu.spring.test.dao.bookDao;
import com.atguigu.spring.test.entity.Book;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class bookService {
//    注入dao
    @Autowired
    private bookDao bookDao;
//    添加方法
  public void addBook(Book book){
      bookDao.addBook(book);
  }
    public void updateBook(Book book){
      bookDao.updateBook(book);
    };
    public void deleteBook(String id){
        bookDao.deleteBook(id);
    };

}

```

```java
package com.atguigu.spring.test.dao;

import com.atguigu.spring.test.entity.Book;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Repository;

@Repository
public class bookDaoImpl implements bookDao{
//    注入jdbcTemplate
    @Autowired
    private JdbcTemplate jdbcTemplate;
//添加方法
    @Override
    public void addBook(Book book) {
//        1、创建sql语句
        String sql="insert into t_unit values(?,?,?)";
//      2、调用方法实现
        Object[] args={book.getBookid(),book.getBookname(),book.getBookstatus()};
      int update=  jdbcTemplate.update(sql,args);
        System.out.println(update);
    }


    @Override
    public void updateBook(Book book) {
    String sql="update t_unit set book_name=?,book_status=? where book_id=?";
    Object[] args={book.getBookname(),book.getBookstatus(),book.getBookid()};
    int update=jdbcTemplate.update(sql,args);
        System.out.println(update);
    }

    @Override
    public void deleteBook(String id) {
        String sql="delete from t_unit where book_id=?";
        int update=jdbcTemplate.update(sql,id);
        System.out.println(update);
    }
}

```

### :four: 查询

#### 查询返回某个值

用到的方法是

![](https://i.loli.net/2021/03/18/FZoJtdSgqkcGNxp.png)

- 在bookDao中创建抽象方法 

```java
int selectCount();
```

- 在bookDaoImpl中实现抽象方法

```java
//    查询count
    @Override
    public int selectCount(){
        String sql="select COUNT(*) from t_unit";
        Integer res = jdbcTemplate.queryForObject(sql,Integer.class);// 返回类型的Class
        return res;
    }
```

- 在bookService中调用方法

```java
//    查询表记录数
    public int findCount(){
       return bookDao.selectCount();
    }

```

- 实现方法

```java
    Integer res=bookService.findCount();
        System.out.println(res); //输出1 
```

![](https://i.loli.net/2021/03/18/qaOtDR67u3sVMw5.png)

####  查询返回对象

用到的方法是 

![](https://i.loli.net/2021/03/18/Jnke1gptKqcQW5r.png)

- 第一个参数是sql语句
- 第二个参数是RowMapper，是一个接口，返回不同类型的数据，使用者接口里面实现类完成数据的封装
- 第三个参数是sql语句中？的值(`注意：类中的属性名和数据库中的属性名一样才可以取出来数据，不然为null`)



- 在service包中添加

```java
//查询返回对象
    public Book findone(String id){
        return bookDao.findBookInfo(id);
    }
```

- 在daoImpl中添加

```java
 @Override
    public Book findBookInfo(String id) {
       String sql="select * from t_unit where book_id = ?;";
      Book book= jdbcTemplate.queryForObject(sql,new BeanPropertyRowMapper<Book>(Book.class),id);

        return book;
    }
```



- 在dao中添加

```java
  public Book findBookInfo(String id);

```

- 在test中添加

```java
 Book book1=bookService.findone("3");
        System.out.println(book1);
```





#### 查询返回某个集合

使用到的方法

![](https://i.loli.net/2021/03/18/VFOKuIxQjXok8Mz.png)

- 第一二个参数和上面的一样的作用，第三个没有可以不写

- 在service中添加

```java
//查询返回集合操作
    public List<Book> findall(){
        return bookDao.findall();
    }
```

- 在dao中添加

```java
public List<Book> findall();
```

- 在daoImpl中添加

```java
   @Override
    public List<Book> findall() {
        String sql="select * from t_unit";
        List<Book> query = jdbcTemplate.query(sql, new BeanPropertyRowMapper<Book>(Book.class));
        return query;
    }
```

- 在test中添加

```java
//        查询返回集合
        List<Book> findall = bookService.findall();
        System.out.println(findall);

```

### :five: 批量操作

> 操作表中的多条操作

#### 批量添加操作

使用方法

![](https://i.loli.net/2021/03/18/BmJqS4Ebo97ktGn.png)



- 第一个参数 sql语句
- 第二个参数 List集合 添加多条记录数据

- 在service中添加

```java
//批量添加
    public  void batchAdd(List<Object[]> batchArgs){
      bookDao.batchAddBook(batchArgs);
    }
```

- 在daoImpl中添加

```java
    public void batchAddBook(List<Object[]> batchArgs) {
      String sql="insert into t_unit values(?,?,?)";
        int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs);
        System.out.println(Arrays.toString(ints));

    }
```

- 在dao中添加

```java
   void batchAddBook(List<Object[]> batchArgs);
```

- 在test中添加

```java
//批量添加
        List<Object[]> batchArgs=new ArrayList<>();
        Object[] o1={"5","liming","q"};
        Object[] o2={"6","aa","2"};
        Object[] o3={"7","c++","4"};
        batchArgs.add(o1);
        batchArgs.add(o2);
        batchArgs.add(o3);
        bookService.batchAdd(batchArgs);
```

#### 批量修改和删除操作

使用的方法和上面的批量添加的一样

- 在service中添加

```java
 public void batchUpdate(List<Object[]> batchArgs){
    bookDao.batchUpdateBook(batchArgs);
    }
```

- 在daoImpl中添加

```java
//批量修改
    @Override
    public void batchUpdateBook(List<Object[]> batchArgs) {
        String sql="update t_unit set book_name=?,book_status=? where book_id=?";
        int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs);
        System.out.println(Arrays.toString(ints));

    }

```

- 在test中添加

```java
  List<Object[]> batchArgs=new ArrayList<>();
        Object[] o1={"liming11","q","5"};
        Object[] o2={"aa11","2","6"};
        Object[] o3={"c++11","4","7"};
        batchArgs.add(o1);
        batchArgs.add(o2);
        batchArgs.add(o3);
        bookService.batchUpdate(batchArgs);
```

- 在dao中添加

```java
    void batchUpdateBook(List<Object[]> batchArgs);
```

