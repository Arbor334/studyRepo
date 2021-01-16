## :fallen_leaf: 集合

### ArrayList集合

> 数组长度是不可以改变的
>
> ArrayList集合的长度是可以<font color="red">随意改变的</font>>
>
> 对于ArrayList集合来说 ，有一个尖括号<E> 代表泛型
>
> `泛型：也就是装在集合中的所有元素，都是什么类型的`
>
> 注意：泛型只能是`引用类型`，不能是基本类型

```java
import java.util.ArrayList;
/*
注意事项：
对于ArrayList集合来说，直接打印得到的不是地址值，而是内容
如果内容是空，得到的是空的中括号:[]

*/
public class demo01ArrayList {
    public static void main(String[] args) {
//        创建了一个ArrayList集合 集合名称是list 里面装的全是String字符串类型的数据
//        从JDK1.7开始，右侧的尖括号内部可以不写内容，但是<>本身还是要写的
        ArrayList<String> list=new ArrayList<>();
        System.out.println(list);
    }
}
```

#### 常用的方法

```java
public boolean add(E e);   向集合当中添加元素，参数的类型和泛型一致   返回值代表是否返回成功
public E  get(int index);   从集合当中获取元素，参数是索引编号，返回值就是对应位置的元素
public E  remove(int index); 从集合当中删除元素，参数是索引编号，返回值就是被删除掉的元素
public int size();   获取集合的元素个数
```

```java
public class demo01ArrayList {
    public static void main(String[] args) {
       ArrayList<String> list=new ArrayList<>();
//        向集合中添加元素: add
        boolean success=list.add("海绵宝宝");
        System.out.println(list);
        System.out.println("元素添加成功了么？"+success);
        list.add("派大星");
        list.add("章鱼哥");
        list.add("珍珍");

//        从集合中获取元素：get 索引值从0开始
        String name=list.get(2);
        System.out.println("第二号索引位置是"+name);


//         从集合中删除元素：remove  索引值从0开始
        String remove = list.remove(3);
        System.out.println("删除了"+remove);
        System.out.println(list);

//        获取集合的元素个数
        int  size=list.size();
        System.out.println("集合的长度"+size);


//        遍历集合
        for (int i = 0; i < list.size(); i++) {
            System.out.println("第"+i+"人是:"+list.get(i));
        }
    }
}

```

#### ArrayList存储基本类型

> ```java
> 如果希望向集合ArrayList中存储基本类型数据，必须使用基本类型对应的"包装类"
> 基本类型           包装类(引用类型   都在java.lang中)
> byte                Byte
> short               Short
> int                 Integer    【特殊】
> long                Long
> float               Float
> double              Double
> char                Character  【特殊】
> boolean             Boolean
> 
> 从JDK1.5开始  支持自动装箱，自动拆箱
> 自动装箱：基本类型  ->> 包装类型
> 自动拆箱：包装类型  ->> 基本类型
> ```

#### :kissing: 练习题

- ```java
  生成6个1~33之间的随机整数，添加到集合，并遍历集合
  
  思路：
  1、需要存储6个数字，创建一个集合<Integer>
  2、产生随机数   Random
  3、用循环6次，来产生6个
  ```

```java
public class demo03ArrayListRandom {
    public static void main(String[] args) {
        ArrayList<Integer> list=new ArrayList<>();
        for (int i = 0; i < 6; i++) {
            int num=(new Random()).nextInt(33)+1;
            list.add(num);
        }
        for (int i = 0; i < list.size(); i++) {
            System.out.println("第"+i+"个数字是"+list.get(i));
        }
    }
}

```

- ```java
  自定义4个Person对象，添加到集合，并遍历
  思路：
  1、自定义Person类
  2、创建一个结合，存储Person对象 泛型：<Person>
  3、根据类创建对象
  4、add
  5、for size get
  ```

```java
public class demo04ArrayListAddPerson {
    public static void main(String[] args) {
        ArrayList<Person> list=new ArrayList<>();
        Person one=new Person("arbor1",12);
        Person two=new Person("arbor2",13);
        Person three=new Person("arbor3",14);
        Person four=new Person("arbor4",15);
        list.add(one);
        list.add(two);
        list.add(three);
        list.add(four);
        for (int i = 0; i < list.size(); i++) {
           Person per= list.get(i);
            System.out.println("姓名："+per.getName()+"年龄："+per.getAge());
        }
    }
}
```

