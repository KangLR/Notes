# 1、注解Annotation

- 什么是注解
  - JDK5.0引入的技术
  - 可以被其他程序（编译器）读取
  - 如@SuppressWarnings(value="unchecked")
  - **注解可以附加在package,class,method,field上面，给他们提供额外的辅助信息，我们可以通过反射机制编程实现对这些元数据的访问**

- 内置注解
  - @Override
  - @Deprecated     v.不赞成的，弃用
  - @SuppressWarnings
  - ![image-20200316121617612](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200316121617612.png)
  - ![image-20200316122010929](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200316122010929.png)

# 2、元注解(meta-annotation)

retention  n.保留，扣留，滞留

- **元注解的作用就是负责注解其他注解，用来提供对其他annotation类型说明**

- 4个标准元注解

  - @Target
    - 用于描述注解的使用范围（用在什么地方）
  - @Retention
    - 用于描述注解的生命周期（source<class<runtime)
    - 表示需要在什么级别保存该注释信息
    - 就是起不起效果
  - @Document
    - 说明该注解被包含在javadoc中
  - @Inherited
    - 说明子类可以继承父类中的该注解

- 定义一个注解

- ```java
  @Target(value={ElementTyoe.METHOD,ElementType.TYPE})//表明myAnnotation注解对方法，类有效
  @Retention(value={RetentionPolicy.RUNTIME,RententionPolicy.CLASS})//运行时和class有效，其实写runtime就表示所有都有效source/class/runtime
  @Inherited//子类可以继承这个注解
  @interface myAnnotation{
      
  }
  ```

# 3、自定义注解

- **使用@interface自定义注解，自动继承了java.lang.annotation.Annotation接口**
- ![image-20200316124528548](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200316124528548.png)
  - 注解的参数
    - 参数类型+参数名+(); //这并不是方法
    - String name () default "";   //默认为空，没有默认值必须赋值

- ![image-20200316143226473](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200316143226473.png)

有一个参数，默认值要写value，不然会报错

# 4、反射机制（reflection）

变成动态语言

## 静态VS动态语言

### 1、动态语言

- 运行时可以改变其结构的语言，如新的函数，对象等，有javascript，python

### 2、静态语言

- 运行时结构不可变的语言，如java，c，c++
- 但java有一定的动态性，可以称为准动态语言

## 4.1、反射概述

- 反射机制允许程序在执行期间借助于Reflection API取得任何类的内部信息**(类名，类的接口，方法，字段等，可以读取到private)**，并能直接操作任意对象的内部属性及方法
- ![image-20200316144627937](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200316144627937.png)

- 优点：
  - 可以实现动态创建对象和编译，体现出很大的灵活性
- 缺点：
  - 对性能有影响
  - 使用反射是一种解释操作，我们可以告诉JVM，我们希望做什么并且它满足我们的要求，跟正常的比慢了几十倍

## 4.2、理解Class类并获取Class实例

- ### 主要API

  ```java
  	java.lang.Class	代表一个类
  	java.lang.reflect.Method	代表类的方法
      java.lang.reflect.Field	代表类的成员变量
      java.lang.reflect.Constructor	代表类的构造器
  ```

- **Class.forName()**

  - Class是一个类
  - forName()是静态方法

- 一个类在内存中只有一个Class对象，它们的hashcode是相同的（感觉和静态字段可以类比一下，只有一份）

- 一个类在加载后，类的整个结构都会被封装在Class对象中

- ```java
  @Test
      public void classTest() throws ClassNotFoundException {
          Class c1 = Class.forName("com.kang.pojo.User");
          System.out.println(c1);
          Class c2 = Class.forName("com.kang.pojo.User");
          Class c3 = Class.forName("com.kang.pojo.User");
          Class c4 = Class.forName("com.kang.pojo.User");
          //一个类只有一个Class对象，它们的hashCode（）是相同的
          System.out.println(c1.hashCode());
          System.out.println(c2.hashCode());
          System.out.println(c3.hashCode());
          System.out.println(c4.hashCode());
      }
  ```

- ![image-20200316150505964](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200316150505964.png)

### 获得的方法

1. 所有类都继承Object类（extends Object）

   ```java
   有个getClass()方法，返回Class对象
       
       public final Class getClass()
       此方法将被所有子类继承，有final不可改写
   ```

   ![image-20200316151935613](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200316151935613.png)

![image-20200316172751321](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200316172751321.png)



## 4.3、类的加载与ClassLoader

![image-20200316152216887](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200316152216887.png)

![image-20200316172557755](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200316172557755.png)

## 4.4、创建运行时类的对象

## 4.5、获取运行时类的完整结构

## 4.6、调用运行时类的指定结构

# 5、哪些类型有Class对象

1. class:外部类、成员（成员内部类、静态内部类），局部内部类，匿名内部类
2. interface：接口
3. []：数组
4. enum：枚举
5. annotation：注解@interface
6. primitive type：基本数据类型
7. void

```java
public void classTest() throws ClassNotFoundException {
//        Class c1 = Class.forName("com.kang.pojo.User");
//        System.out.println(c1);
//        Class c2 = Class.forName("com.kang.pojo.User");
//        Class c3 = Class.forName("com.kang.pojo.User");
//        Class c4 = Class.forName("com.kang.pojo.User");
//        //一个类只有一个Class对象，它们的hashCode（）是相同的
//        System.out.println(c1.hashCode());
//        System.out.println(c2.hashCode());
//        System.out.println(c3.hashCode());
//        System.out.println(c4.hashCode());
        Class c1=Object.class;//类
        Class c2=Comparable.class;//接口
        Class c3=String.class;//字符串
        Class c4=String[].class;
        Class c5=String[][].class;
        Class c6=int[][].class;
        Class c7=Override.class;//注解
        Class c8= ElementType.class;//枚举
        Class c9=Integer.class;//基本数据类型
        Class c10=void.class;//void
        Class c11=Class.class;//Class类

        System.out.println(c1);
        System.out.println(c2);
        System.out.println(c3);
        System.out.println(c4);
        System.out.println(c5);
        System.out.println(c6);
        System.out.println(c7);
        System.out.println(c8);
        System.out.println(c9);
        System.out.println(c10);
        System.out.println(c11);

    }


结果：
    
class java.lang.Object
interface java.lang.Comparable
class java.lang.String
class [Ljava.lang.String;
class [[Ljava.lang.String;
class [[I
interface java.lang.Override
class java.lang.annotation.ElementType
class java.lang.Integer
void
class java.lang.Class
```

# 6、类加载内存分析

## 6.1、Java内存分析

- 堆
  - 存放**new**的对象和数组
  - 可以被所有线程共享，不会存放别的对象引用
- 栈
  - 存放基本变量类型（会包含这个基本类型的具体数值）
  - 引用对象的变量person（会存放这个引用在堆里面的具体地址）Person person=new student();
- 方法区
  - 包含了所有的class和static变量
  - 可以被所有线程共享

## 6.2、类加载过程

- ![image-20200316180606361](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200316180606361.png)

![image-20200316183037695](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200316183037695.png)

- **static变量默认值，然后再赋值**

- 我们自己定义的类什么的被会被替换成描述符，都是常量，在常量池内

- ```java
  public class Test{
      public static void main(String[] args) {
          A a=new A();
          System.out.println(A.m);
      }
  }
  
  class A{
      static{
          System.out.println("A类的静态代码块初始化");
          m=300;
      }
      static int m=100;
      public A(){
          System.out.println("A类的无参构造初始化");
      }
  }
  
  
  
  结果：A类的静态代码块初始化
      A类的无参构造初始化
      m=100
      
      
      m的过程：0>300>100
      
  ```

- ![image-20200316190345636](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200316190345636.png)

- ```txt
  1.加载到内存，会产生一个类对应Class对象
  2.链接，链接结束后m=0
  3.初始化
  	类构造器方法<clinit>（）{
  		把静态代码合并
  		System.out.println("A类的静态代码块初始化");
          m=300;
          m=100;
  	}
  	m=100
  ```

- **Class对象在类加载进去就产生了**

## 6.3、分析类初始化

- ![image-20200316190814306](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200316190814306.png)

**反射会造成主动引用，把该类以上的父类全部加载，极大的消耗资源**

- 通过子类调用父类的静态变量，子类并不会被加载，父类会被加载
- ![image-20200316221756656](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200316221756656.png)

```java
public class Test{
    static {
        System.out.println("Main类被加载");
    }
    public static void main(String[] args) {
        //1.主动引用
        Son son=new Son();//父类没被初始化，先加载Father
            //Main_父类_子类
        //2.反射也会产生主动引用
        Class.forName("com.kang.dao.Son");
            //Main_父类_子类

        //3.static在链接时已经初始化了
        //调用父类的b
        System.out.println(Son.b);
            //Main——父类——2

        //4.
        Son[] array=new Son[5];//只是开辟了个空间并且命名
            //Main

        //5.调用常量池的
        System.out.println(Son.M);
    }
}

class Father{
    static int b=2;
    static{
        System.out.println("父类被加载");
    }
}
class Son extends Father{
    static {
        System.out.println("子类被加载");
        m=300;
    }
    static int m=100;
    static final int M=1;
}
```

## 6.4、类加载器

![image-20200316223139441](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200316223139441.png)

![image-20200316223451339](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200316223451339.png)

```java
 @Test
    public void getClassLoader(){
        //系统类加载器
        ClassLoader classLoader=ClassLoader.getSystemClassLoader();
        System.out.println(classLoader);
        //扩展类加载器
        ClassLoader classLoader1=classLoader.getParent();
        System.out.println(classLoader1);
        //引导类加载器，无法直接获取
        ClassLoader classLoader2=classLoader1.getParent();
        System.out.println(classLoader2);
        //测试当前类是由谁加载的
        ClassLoader classLoader3=Class.forName("com.kang.dao.UserMapperTest").getClassLoader();
        System.out.println(classLoader3);
        //测试jdk内置类是由谁加载的
        ClassLoader classLoader4=Class.forName("java.lang.Object").getClassLoader();
        System.out.println(classLoader4);
        
        //如何获取系统类加载器可以加载的路径
        System.out.println(System.getProperty("java.class.path"));
    }


结果：
    
jdk.internal.loader.ClassLoaders$AppClassLoader@78308db1
jdk.internal.loader.ClassLoaders$PlatformClassLoader@18be83e4
null
jdk.internal.loader.ClassLoaders$AppClassLoader@78308db1
null
    
C:\IDEA\IntelliJ IDEA 2019.3.3\lib\idea_rt.jar;
C:\IDEA\IntelliJ IDEA 2019.3.3\plugins\junit\lib\junit5-rt.jar;
C:\IDEA\IntelliJ IDEA 2019.3.3\plugins\junit\lib\junit-rt.jar;
C:\IDEA\mybatis\mybatis-04\target\test-classes;
C:\IDEA\mybatis\mybatis-04\target\classes;
C:\Users\klr10\.m2\repository\junit\junit\4.11\junit-4.11.jar;
C:\Users\klr10\.m2\repository\org\hamcrest\hamcrest-core\1.3\hamcrest-core-1.3.jar;
C:\Users\klr10\.m2\repository\mysql\mysql-connector-java\5.1.47\mysql-connector-java-5.1.47.jar;
C:\Users\klr10\.m2\repository\org\mybatis\mybatis\3.5.4\mybatis-3.5.4.jar;
C:\Users\klr10\.m2\repository\log4j\log4j\1.2.17\log4j-1.2.17.jar
```

# 7、创建运行时类的对象

![image-20200316225611787](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200316225611787.png)

```java
//获得类的信息
    @Test
    public void getClassInfo() throws ClassNotFoundException, NoSuchFieldException, NoSuchMethodException {
        Class c1=Class.forName("com.kang.pojo.User");
        //获得类的名字
        String name = c1.getName();
        System.out.println(name);
        System.out.println("===========================");

        //获得类的属性
        //getField只能找到public的，要用getDeclareFields获得所有
        Field[] fields=c1.getFields();
        for (Field field : fields) {
            System.out.println(field);
        }
        System.out.println("===========================");

        Field[] fields1=c1.getDeclaredFields();
        for (Field field : fields1) {
            System.out.println(field);
        }
        System.out.println("===========================");

        //获得指定的属性
        //Field field=c1.getField("name");//private会报错
        //System.out.println(field);
        Field field1=c1.getDeclaredField("name");
        System.out.println(field1);

        //获得类的方法
        System.out.println("===========================");
        Method[] methods=c1.getMethods();//获得本类及其父类的所有public
        for (Method method : methods) {
            System.out.println("正常的："+method);
        }
        System.out.println("===========================");

        Method[] methods1=c1.getDeclaredMethods();//获得本类的所有方法
        for (Method method : methods1) {
            System.out.println(method);
        }
        System.out.println("===========================");
        //获得指定的方法
        Method method=c1.getMethod("getName",null);
        System.out.println(method);
        Method setName=c1.getMethod("setName", String.class);//因为有重载，所以需要参数String.class
        System.out.println(setName);
        System.out.println("===========================");
        //获得所有的构造器
        Constructor[] constructors = c1.getConstructors();//获得public
        for (Constructor constructor1 : constructors) {
            System.out.println(constructor1);
        }
        Constructor[] declaredConstructors = c1.getDeclaredConstructors();
        for (Constructor declaredConstructor : declaredConstructors) {
            System.out.println("#"+declaredConstructor);
        }
        //获得指定的构造器
        System.out.println(c1.getDeclaredConstructor( int.class, String.class,String.class));
    }
    }
```

结果：

正常的方法全是public

```java
com.kang.pojo.User
===========================
===========================
private int com.kang.pojo.User.id
private java.lang.String com.kang.pojo.User.name
private java.lang.String com.kang.pojo.User.password
===========================
private java.lang.String com.kang.pojo.User.name
===========================
正常的：public java.lang.String com.kang.pojo.User.toString()
正常的：public java.lang.String com.kang.pojo.User.getName()
正常的：public void com.kang.pojo.User.setName(java.lang.String)
正常的：public int com.kang.pojo.User.getId()
正常的：public java.lang.String com.kang.pojo.User.getPassword()
正常的：public void com.kang.pojo.User.setId(int)
正常的：public void com.kang.pojo.User.setPassword(java.lang.String)
正常的：public final native void java.lang.Object.wait(long) throws java.lang.InterruptedException
正常的：public final void java.lang.Object.wait(long,int) throws java.lang.InterruptedException
正常的：public final void java.lang.Object.wait() throws java.lang.InterruptedException
正常的：public boolean java.lang.Object.equals(java.lang.Object)
正常的：public native int java.lang.Object.hashCode()
正常的：public final native java.lang.Class java.lang.Object.getClass()
正常的：public final native void java.lang.Object.notify()
正常的：public final native void java.lang.Object.notifyAll()
===========================
public java.lang.String com.kang.pojo.User.toString()
public java.lang.String com.kang.pojo.User.getName()
public void com.kang.pojo.User.setName(java.lang.String)
public int com.kang.pojo.User.getId()
public java.lang.String com.kang.pojo.User.getPassword()
public void com.kang.pojo.User.setId(int)
public void com.kang.pojo.User.setPassword(java.lang.String)
===========================
public java.lang.String com.kang.pojo.User.getName()
public void com.kang.pojo.User.setName(java.lang.String)
===========================
public com.kang.pojo.User()
public com.kang.pojo.User(int,java.lang.String,java.lang.String)
#public com.kang.pojo.User()
#public com.kang.pojo.User(int,java.lang.String,java.lang.String)
public com.kang.pojo.User(int,java.lang.String,java.lang.String)
```

![image-20200316233320077](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200316233320077.png)

# 8、动态创建对象执行方法

![image-20200317103857032](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317103857032.png)

- **删除无参构造器时会报错，找不到构造器方法**

  ```java
  public void getObjectByReflect() throws ClassNotFoundException, IllegalAccessException, InstantiationException, NoSuchMethodException, InvocationTargetException {
          //获得Class对象
          Class c1 = Class.forName("com.kang.pojo.User");
          //构造一个对象
  
          //本质上是调用了类的无参构造器初始化对象，把User类的无参构造器删了会报错
          //User o = (User) c1.newInstance();
          //System.out.println(o);
          User user=(User) c1.getDeclaredConstructor().newInstance();
          System.out.println(user);
  
  
      }
  
  
  
  结果：
      
      java.lang.NoSuchMethodException: com.kang.pojo.User.<init>()
  
  	at java.base/java.lang.Class.getConstructor0(Class.java:3349)
  	at java.base/java.lang.Class.getDeclaredConstructor(Class.java:2553)
  	at com.kang.dao.UserMapperTest.getObjectByReflect(UserMapperTest.java:216)
  	at com.intellij.rt.junit.IdeaTestRunner$Repeater.startRunnerWithArgs(IdeaTestRunner.java:33)
  	at com.intellij.rt.junit.JUnitStarter.prepareStreamsAndStart(JUnitStarter.java:230)
  	at com.intellij.rt.junit.JUnitStarter.main(JUnitStarter.java:58)
  ```

  

![image-20200317111828295](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317111828295.png)

![image-20200317111957017](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317111957017.png)

![image-20200317112058694](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317112058694.png)

## 通过反射来动态创建对象

```java
 @Test
    public void getObjectByReflect() throws ClassNotFoundException, IllegalAccessException, InstantiationException, NoSuchMethodException, InvocationTargetException, NoSuchFieldException {
        //获得Class对象
        Class c1 = Class.forName("com.kang.pojo.User");
        //构造一个对象

        //本质上是调用了类的无参构造器初始化对象，把User类的无参构造器删了会报错
        //User o = (User) c1.newInstance();
        //System.out.println(o);

        //实际上是通过构造器创建对象
        User user=(User) c1.getDeclaredConstructor().newInstance();
        System.out.println(user);

        //通过反射调用普通方法
        //c1是Class，并不是通过user这种实例调用方法
        Method setName = c1.getDeclaredMethod("setName", String.class);

        //invoke=调用
        //(对象，“方法的值”)
        //invoke通过反射获取的方法去跑的
        //要知道方法属于哪个对象，因此要传入对象
        setName.invoke(user,"kang");//invoke调用，给个对象传递参数
        System.out.println(user.getName());


        //通过反射操作属性
        User user1 =(User)c1.newInstance();
        Field name=c1.getDeclaredField("name");
        //不能直接操作私有属性
        name.setAccessible(true);//name是private的，要想修改，必须关闭权限检测，方法页适用
        name.set(user1,"kang1");
        System.out.println(user1.getName());
    }


结果：
    
    User{id=0, name='null', password='null'}
kang
kang1
```



# 9、JAVA9之后class.newInstance()方法被弃用

- 取而代之的是class.getDeclareConstructor().newInstance()

- 原因

  - class.newInstance()会直接调用该类的无参构造函数进行实例化
  - class.getDeclaredConstructor().newInstance()方法会根据它的参数对该类的构造函数进行搜索并返回对应的构造函数，没有参数就返回该类的无参构造函数，然后再通过newInstance进行实例化。

- ```java
  public class Test {
      public Test() {
          System.out.println("HelloTest");
      }
      public static void main(String[] args) throws Exception {
          C c = C.class.getDeclaredConstructor(int.class).newInstance(5);    
      }
  }
  
  class C {
      public C() {}
      private C(int i) {
      	System.out.println("HelloC" + i);
      }
      
  }
  ```

- class.getDeclaredConstructor().newInstance() 更明确化，明确调用的是哪一个构造器，而不是直接采用默认的无参构造器了

# 10、性能对比分析

**最好把它们分成三个方法然后调用**

```java
//分析性能问题
    @Test
    public void AnalyzePerformanceProblems() throws NoSuchMethodException, InvocationTargetException, IllegalAccessException {
        //普通方式调用
        User user=new User();
        long startTime=System.currentTimeMillis();
        for(int i=0;i<100000000;i++)
            user.getName();
        long endTime=System.currentTimeMillis();
        System.out.println("普通方式执行亿次："+(endTime-startTime)+"ms");
        //通过反射方式调用
        User user1=new User();
        Class c1=user1.getClass();
        Method method1=c1.getDeclaredMethod("getName",null);
        startTime=System.currentTimeMillis();
        for(int i=0;i<100000000;i++)
        {
            method1.invoke(user,null);
        }
        endTime=System.currentTimeMillis();
        System.out.println("反射方式执行亿次："+(endTime-startTime)+"ms");
        //反射关闭检测
        User user2=new User();
        Class c2=user2.getClass();
        Method method=c2.getDeclaredMethod("getName",null);
        method.setAccessible(true);
        startTime=System.currentTimeMillis();
        for(int i=0;i<100000000;i++)
        {
            method.invoke(user,null);
        }
        endTime=System.currentTimeMillis();
        System.out.println("反射方式关闭检测执行亿次："+(endTime-startTime)+"ms");
    }
```

```java
结果：
普通方式执行亿次：62ms
反射方式执行亿次：404ms
反射方式关闭检测执行亿次：262ms
```

**如果使用反射较多最好关闭权限检测**

# 11、获取泛型信息

![image-20200317114630729](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317114630729.png)

![image-20200317115250012](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317115250012.png)



![image-20200317115313987](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317115313987.png)

![image-20200317115339954](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317115339954.png)

# 12、获取注解信息

- **反射操作注解**
  - getAnnotations
  - getAnnotation
- ORM (object relationship Mapping-->对象关系映射)
  - ![image-20200317115823009](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317115823009.png)

```java
package com.kang.dao;

import java.lang.annotation.*;
import java.lang.reflect.Field;

/**
 * @author klr
 * @create 2020-03-17-12:02
 */
public class te {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchFieldException {
        Class c1=Class.forName("com.kang.dao.student");
        //通过反射获得所有注解
        Annotation[] annotations = c1.getAnnotations();
        for (Annotation annotation : annotations) {
            System.out.println(annotation);
        }
        //获取注解的value的值
        Table annotation =(Table) c1.getAnnotation(Table.class);
        System.out.println(annotation.value());

        //获得类指定的注解
        Field declaredField = c1.getDeclaredField("name");
        field annotation1 = declaredField.getAnnotation(field.class);
        System.out.println(annotation1.columnName());
        System.out.println(annotation1.type());
        System.out.println(annotation1.length());

    }
}

@Table("db_student")
class student{
    @field(columnName = "db_id",type = "int",length = 10)
    private int id;
    @field(columnName = "db_age",type = "int",length = 10)
    private int age;
    @field(columnName = "db_name",type = "varchar",length = 3)
    private String name;

    public student() {
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public student(int id, int age, String name) {
        this.id = id;
        this.age = age;
        this.name = name;
    }
}

//类名的注解
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@interface Table{
    String value();
}

//属性的注解
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@interface field{
    String columnName();
    String type();
    int length();
}

```

```java
结果
    @com.kang.dao.Table(value="db_student")
db_student
db_name
varchar
3
```

