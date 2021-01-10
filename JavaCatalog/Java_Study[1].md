## java基础[1]

### 一、静态关键字  static

> 一旦用了static关键字 那么这样的内容就不再属于对象自己，而是属于类的
>
> 所以凡是本类的对象，都共享同一份。

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

