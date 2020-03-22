# 1、Spring

## 1.1、简介

- Spring：春天

- 2002，首次推出了Spring框架的雏形interface21框架

- 2004.3.24，Spring发布了1.0版本

- **Rod Johnson**，Spring Framework创始人

- **Spring理念**：使现有的技术更加容易使用，本身是一个大杂烩，整合了现有的技术框架

- SSH:Struct2+Spring+Hibernate

- SSM:SpringMVC+Spring+Mybatis

- **官网：**https://docs.spring.io/spring/docs/5.2.4.RELEASE/spring-framework-reference/overview.html#overview-getting-started

- **官方下载地址**：[http://repo.spring.io/release/org/springframework/spring](https://repo.spring.io/release/org/springframework/spring) 

- **Github：**https://github.com/spring-projects/spring-framework

- 导Spring Web-MVC就行了，它会帮忙很多其他的包

  - ```xml
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.2.4.RELEASE</version>
    </dependency>
    ```

  - 与Mybatis整合时导入Spring JDBC

    ```xml
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>5.2.4.RELEASE</version>
    </dependency>
    ```

## 1.2、优点

- Spring是一个开源的免费的框架
- Spring是一个轻量级的（本身很小）、非入侵式（引入不会改变代码原来的任何情况，不会产生影响）的框架
- 控制反转**IOC**、面向切面编程**AOP**
- 支持事务的处理，对框架整合的支持！
- **总结：Spring就是一个轻量级的控制反转（IOC）和面向切面编程（AOP）的框架**

## 1.3、组成

![image-20200318101735891](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200318101735891.png)

## 1.4、拓展

现代化的Java开发，就是基于Spring的

![image-20200318102149969](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200318102149969.png)

- Spring Boot 
  - 一个快速开发的脚手架
  - 基于SpringBoot可以快速的开发单个微服务（就是一个小模块）
  - **约定大于配置**！
- Spring Cloud
  - 基于Spring Boot实现的
- 学习SpringBoot的前提：Spring及SpringMVC
- 弊端：Spring发展太久违背了原来的理念：配置十分繁琐

# 2、IOC理论推导

IOC使得实现对象不用在程序中写死

## 1、**原来**

1. UserDao接口
2. UserDapImpl实现类
3. UserService业务接口
4. UserServiceImpl业务实现

- 导入SpringWebMVC
  - ![image-20200318103629664](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200318103629664.png)

- 增加了类UserDaoMysqlImpl，客户要想调用就要修改UserServiceImpl的代码
  - 改成private UserDao userDao = new UserDaoMysqlImpl()
  - **这非常的麻烦复杂**
  - **我们因为客户的需求要去手动改原有的代码**
  - ![image-20200318105505082](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200318105505082.png)

- **我们使用一个Set接口实现**
- ![image-20200318110915088](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200318110915088.png)

- **现在我们可以让用户自己选择实现哪个方法，用户只需调用set方法，而我们不用修改原来service的代码了，这是个巨大的区别**
  - ![image-20200318111104905](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200318111104905.png)

- **之前，程序是主动创建对象，控制权在程序猿手上**
- **使用了set注入后，程序不再具有主动性，而是变成了被动的接收对象**



- **这就叫控制反转！**

  - 这种思想，从本质上解决了问题，我们程序员不用再去**管理对象的创建**了，系统的耦合性大大降低，可以更加专注**业务的实现**，以后只需要把实现好的东西接口告诉你，你自己去调

  - **这是IOC的原型**

    

- **也可以理解为主动权交给用户了**

- ![image-20200318113259111](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200318113259111.png)

- IoC是一种设计思想，DI只是IoC实现的一种方法，实现IoC的方法很多
  - 可以使用XML配置
  - 可以使用注解
  - 甚至零配置实现IoC
- 所谓控制反转就是**获得依赖对象的方式反转了**

Spring容器在初始化时先读取配置文件，根据配置文件或元数据创建与组织对象存入容器中，程序员用时再从IoC容器中取出需要的对象

![image-20200318114438388](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200318114438388.png)

![image-20200318114602447](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200318114602447.png)

# 3、HelloSpring

- Doc：https://docs.spring.io/spring/docs/5.2.4.RELEASE/spring-framework-reference/core.html#spring-core

- 创建Hello类

- 创建beans.xml

  - **使用Spring来创建对象，在Spring中这些都称为Bean**

  - 引用对象用ref，不用value

  - ```txt
    <!--这里用ref:引用Spring中创建好的对象-->
    ```

  - ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.springframework.org/schema/beans
            https://www.springframework.org/schema/beans/spring-beans.xsd">
    
        <!--使用Spring来创建对象，在Spring中这些都称为Bean-->
        <bean id="hello" class="com.kang.pojo.Hello">
            <property name="string" value="Spring"/>
        </bean>
    
    </beans>
    ```

- 实例化容器

  - 提供给构造函数的位置路径或路径是资源字符串，允许容器加载来自各种外部资源（如本地文件系统、Java 等）的配置元数据。`ApplicationContext``CLASSPATH`

  - 用XML加载必须写这句话，是固定的

  - ```java
    ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
    ```

- 测试一下

  - ```java
    public class MyTest {
        public static void main(String[] args) {
            //获取Spring的上下文对象
            ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
            //我们的对象现在都在Spring中管理了（控制反转了，不再由我们创建了），我们要使用，直接去里面取出来就可以！
            Hello hello =(Hello) context.getBean("hello");
            System.out.println(hello.toString());
        }
    }
    ```

  - hello对象是由Spring创建的

    - Hello hello =(Hello) context.getBean("hello");

  - hello对象的属性是由Spring容器设置的

    - <property name="string" value="Spring"/>

  - **Spring底层创建对象是通过反射来做的**

- 原来

  - Hello hello=new Hello()

- 现在

  - id=变量名 
  - class=new 的对象 
  - property 相当于给对象中的属性设置值

- **这就是控制反转，以前我们要主动new一个对象，现在把它交给容器，我们只需要配置就好了，用的时候去容器拿**

- **依赖注入本质是用Set方法注入，如果我们把创建的Hello类中的set方法去了程序就报错了！**

- ![image-20200318122305866](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200318122305866.png)

# 4、ClassPathXmlApplicationContext流程

![image-20200318124131874](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200318124131874.png)

# 5、重构HelloSpring

**现在只用修改spring的配置文件，ref**

不像以前要去代码中修改，这样更简洁了

用户那边不用改，我们也不用改

**改配置文件**

# 6、IoC创建对象的方式

获取上下文时bean就有了

## 1、使用无参构造创建对象，默认！

## 2、我们也可以使用有参构造创建对象

### 第一种：下标赋值

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

<!--    <bean id="user" class="com.kang.pojo.User">-->
<!--        <property name="name" value="康"/>-->
<!--    </bean>-->
<!--第一种，下标赋值-->
    <bean id="user" class="com.kang.pojo.User">
        <constructor-arg index="0" value="RNGVSGNR"/>
    </bean>

</beans>
```

```java
package com.kang.pojo;

/**
 * @author klr
 * @create 2020-03-18-13:51
 */
public class User {
    private String name;

    public User() {
        System.out.println("User的无参构造");
    }

    public User(String name) {
        this.name=name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }
}

```

test

```java
import com.kang.pojo.User;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * @author klr
 * @create 2020-03-18-13:53
 */
public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");

        //走无参构造创建对象
        //删掉User中的无参构造就报错了
        //默认实现
        User user =(User) context.getBean("user");
        System.out.println(user.toString());
    }
}
```

### 第二种：type类型创建

不建议使用，当两个参数都是String时就不好弄了

```xml
<bean id="user" class="com.kang.pojo.User">
        <constructor-arg type="java.lang.String" value="康" />
    </bean>
```

### 第三种：引用ref

直接通过参数名来赋值

最常用的

```xml
<bean id="user" class="com.kang.pojo.User">
        <constructor-arg name/ref="name" value="康" />
    </bean>
```

## 3、property和constructor-arg是不同的，前者是对字段赋值，后者是创建有参构造时的初始化赋值

## 4、可以创建多个pojo，然后只get一个bean，会发现所有的bean在获取配置时就都被创建了，并不是getBean时才创建某个bean，而且多次get同一个bean其实是同一个对象，说明内存中只有一份实例，取得都是那一个

总结：在配置文件加载的时候，容器中管理的对象就已经初始化了

# 7、Spring配置说明

## 1、别名

没啥用

```xml
<alias name="user"  alias="asdsddada"/>
```

## 2、Bean的配置

```xml
id:bean的唯一标识符

class：bean对象所对应的类型，全限定名：包名+类名

name：也是别名，比alias更高级，可以同时取多个别名
<bean id="a" class="" name="aq,ag,al"></bean>

scope:作用域，默认单例
```

## 3、import

import一般用于团队开发，他可以将多个配置文件，导入合并为一个

```xml
<import resource="bean1.xml"/>
<import resource="bean2.xml"/>
<import resource="bean3.xml"/>
<import resource="bean4.xml"/>
```

使用时直接使用总配置

或

```java
ApplicationContext context = new ClassPathXmlApplicationContext("beans1.xml","beans2.xml");
```

# 8、DI（依赖注入）环境

DI：本质set注入

- 依赖注入
  - 依赖：bean对象的创建依赖于容器
  - 注入：bean对象中的所有属性，由容器来注入
- 个人理解：控制反转后，假设用户想要一个苹果，就要在IoC容器中创建一个苹果对象，而苹果对象有名字，生产地址，价格等等属性，比如名字是String类型的，价格是int类型的，生产地址是Address类型的（这是你单独创建的一个类（bean），它也会被IoC创建）,其实它还可以list，map，set，null等等类型，这些细节不重要，重要的是你要给这些属性赋值，这个赋值的操作也是通过IoC容器来完成，注入也就可以理解为你要向你创建的这个苹果对象中加入它的名字，价格等信息（赋值）。DI的主要方式是通过set注入的

## 第一种：构造器注入

## 第二种：set方式注入（重点）

- **只有Map和Properties比较特殊**

  ```xml
  <property name="card">
              <map>
                  <entry key="身份证" value="123456"/>
                  <entry key="银行卡" value="234567"/>
              </map>
          </property>
  
  
  
  
  <property name="info">
              <props>
                  <prop key="url">123456</prop>
                  <prop key="password">123456</prop>
                  <prop key="username">klr</prop>
              </props>
          </property>
  ```

  

- 复杂类型

  ```java
  package com.kang.pojo;
  
  /**
   * @author klr
   * @create 2020-03-18-14:52
   */
  public class Address {
      private String address;
  
      public String getAddress() {
          return address;
      }
  
      public void setAddress(String address) {
          this.address = address;
      }
  
      @Override
      public String toString() {
          return "Address{" +
                  "address='" + address + '\'' +
                  '}';
      }
  }
  ```

  

- 真实测试对象（使用了lombok插件）

  ```java
  package com.kang.pojo;
  
  import lombok.Data;
  import lombok.Getter;
  import lombok.NoArgsConstructor;
  import lombok.Setter;
  
  import java.util.*;
  
  /**
   * @author klr
   * @create 2020-03-18-14:52
   */
  @Getter
  @Setter
  @NoArgsConstructor
  public class Student {
      private String name;
      private Address address;
      private String[] books;
      private List<String> hobbys;
      private Map<String,String> card;
      private Set<String> games;
      private String wife;//空指针null
      private Properties info;
  
      @Override
      public String toString() {
          return "Student{" +
                  "name='" + name + '\'' +
                  ", address=" + address.toString() +
                  ", books=" + Arrays.toString(books) +
                  ", hobbys=" + hobbys +
                  ", card=" + card +
                  ", games=" + games +
                  ", wife='" + wife + '\'' +
                  ", info=" + info +
                  '}';
      }
  }
  ```

- beans.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
          https://www.springframework.org/schema/beans/spring-beans.xsd">
  
      <bean id="address" class="com.kang.pojo.Address">
          <property name="address" value="China"/>
      </bean>
  
      <bean id="student" class="com.kang.pojo.Student">
  <!--        普通注入使用value-->
          <property name="name" value="康"/>
  
  <!--        Bean注入，ref-->
          <property name="address" ref="address"/>
  
  <!--        数组注入-->
          <property name="books">
              <array>
                  <value>《红楼梦》</value>
                  <value>《石头记》</value>
                  <value>《水浒传》</value>
                  <value>《三国演义》</value>
                  <value>《西游记》</value>
              </array>
          </property>
  
  <!--        list-->
          <property name="hobbys">
              <list>
                  <value>唱</value>
                  <value>跳</value>
                  <value>Rap</value>
              </list>
          </property>
  
  <!--        Map，用的entry-->
          <property name="card">
              <map>
                  <entry key="身份证" value="123456"/>
                  <entry key="银行卡" value="234567"/>
              </map>
          </property>
  
  <!--        Set-->
          <property name="games">
              <set>
                  <value>LOL</value>
                  <value>COC</value>
                  <value>BOB</value>
              </set>
          </property>
  
  <!--        null不是空字符串value=""-->
          <property name="wife">
              <null></null>
          </property>
  
  <!--        Properties-->
          <property name="info">
              <props>
                  <prop key="url">123456</prop>
                  <prop key="password">123456</prop>
                  <prop key="username">klr</prop>
              </props>
          </property>
  
  
      </bean>
  
  </beans>
  ```

- 测试类

  ```java
  import com.kang.pojo.Student;
  import org.springframework.context.ApplicationContext;
  import org.springframework.context.support.ClassPathXmlApplicationContext;
  
  /**
   * @author klr
   * @create 2020-03-18-15:02
   */
  public class MyTest {
      public static void main(String[] args) {
          ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
          Student student=(Student) context.getBean("student");
  //        System.out.println(student.getName());
  //        System.out.println(student.getAddress());
          System.out.println(student.toString());
  
      }
  }
  
  ```

- 结果

  ```java
  Student{
      name='康', 
      address=Address{address='China'}, 
      books=[《红楼梦》, 《石头记》, 《水浒传》, 《三国演义》, 《西游记》], 
      hobbys=[唱, 跳, Rap], 
      card={
          身份证=123456, 
          银行卡=234567
      }, 
      games=[LOL, COC, BOB], 
      wife='null', 
      info={
          password=123456, 
          url=123456, 
          username=klr}
  }
  ```

## 第三种：拓展方式注入

**需要导入XML约束**

p标签（properties）**对应set注入**

依赖第三方的约束

![image-20200318155828315](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200318155828315.png)

c标签 **对应构造器注入**

**类中要有有参构造器，不然报错**

![image-20200318160115290](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200318160115290.png)

# 9、Bean的作用域（bean scopes)

- singleton	单例，无论你用几个getBean去拿都只是**那一个实例**
  - ![image-20200318161402063](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200318161402063.png)
  - true
  - 默认机制
- prototype    原型模式，每一个bean创建都是单独对象
  - 每次从容器中get的时候都会产生新对象
  - <bean ...   scope="prototype">
  - ![image-20200318161623607](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200318161623607.png)
- 下面是WebMVC里才能用的
- request（一次请求中创建，用完失效）
- session（一直在session中活着）
- application（全局都活着）
- websocket

# 10、自动装配Bean

我们之前都在手动装配，一个个属性都要设置

- 自动装配是Spring满足bean依赖的一种方式
- Spring会在上下文自动寻找，并自动给bean装配属性

## 3种装配的方式

### 1、在xml中显式的配置

### 2、在java中显式的配置

### 3、隐式的自动装配bean（重要）autowire

#### 3.1、测试：环境搭建：一个人有两个宠物

```xml
 <bean id="cat" class="com.kang.pojo.Cat"/>
    <bean id="dog" class="com.kang.pojo.Dog"/>
    <bean id="people" class="com.kang.pojo.People">
        <property name="name" value="kang"/>
        <property name="cat" ref="cat"/>
        <property name="dog" ref="dog"/>
    </bean>
```

#### 3.2、byName自动装配

**现在我们想把ref=cat/dog去掉，让程序在bean自动寻找cat与dog，然后set给它**

```xml
 <bean id="cat" class="com.kang.pojo.Cat"/>
    <bean id="dog" class="com.kang.pojo.Dog"/>
<!--    自动寻找cat/dog-->
    <bean id="people" class="com.kang.pojo.People" autowire="byName">
        <property name="name" value="kang"/>
    </bean>

成功
```

**我们把dog改成dog222**看一看

```xml
 <bean id="cat" class="com.kang.pojo.Cat"/>
    <bean id="dog2222" class="com.kang.pojo.Dog"/>
<!--    自动寻找cat/dog-->
    <bean id="people" class="com.kang.pojo.People" autowire="byName">
        <property name="name" value="kang"/>
    </bean>

报错
空指针异常

当我们把Test中对dog的调用注释掉时程序是成功的，因为我没没用到dog，也就不会set
//        people.getDog().shout();
        people.getCat().shout();
```

**byName**：会自动在容器上下文中查找，和自己对象set方法后面的值对应的beanid！

#### 3.3、byType

会自动在容器上下文中查找，和自己对象属性类型相同的bean！

可以成功

**弊端：如果有两个dog就报错了，要全局唯一**

#### 小结：

- byName，需要保证所有bean的id唯一，并且这个beanid需要和自动注入的属性的set方法的值一致，如setDog111（）看这个Dog111，bean的id=dog111
- byType，需要保证所有bean的class唯一，并且这个bean需要和自动注入的属性的类型一致！

# 11、使用注解实现自动装配

jdk1.5支持的注解，Spring2.5就支持注解了

使用注解须知：

1.导入约束

​	context约束

2.配置注解的支持

​	<context:annotation-config/>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

</beans>
```

## @Autowire（这是Spring实现的）

@Autowired是优先用byType而后用byName

@Autowired+@Qualifier=byType|byName

- 我们甚至连set方法都不需要，因为注解是**通过反射来实现**的
- 直接在属性上使用即可，也可以在set方式上使用
- 使用Autowired我们可以不用编写set方法了，前提是你这个自动装配的属性在IOC（spring）容器中存在

![image-20200318210503310](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200318210503310.png)



科普:

```xml
@Nullable		字段标记了这个注解，说明这个字段可以为null

public People(@Nullable String name){this.name=name}	
这样name为空可以不报错，正常是要报错的


注解
@Target({ElementType.CONSTRUCTOR, ElementType.METHOD, ElementType.PARAMETER, ElementType.FIELD, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Autowired {
    boolean required() default true;
}

//默认为true
```

```java
   //如果显式的定义了required属性为false，说明这个对象可以为null，否则不允许为空
    @Autowired(required = false)
    private Cat cat;
    @Autowired
    private Dog dog;
    private String name;
```



@Autowired+ @Qualifier

```java
package com.kang.pojo;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;

/**
 * @author klr
 * @create 2020-03-18-16:39
 */
public class People {

    //如果显式的定义了required属性为false，说明这个对象可以为null，否则不允许为空
//    @Autowired(required = false)
    @Autowired
    private Cat cat;
    @Autowired
    @Qualifier(value = "dog22")
    private Dog dog;
    private String name;
    //使用注解后我们不再需要set方法，因为注解是用反射实现的
    public String getName() {
        return name;
    }

    public Cat getCat() {
        return cat;
    }

    public Dog getDog() {
        return dog;
    }

    @Override
    public String toString() {
        return "People{" +
                "cat=" + cat +
                ", dog=" + dog +
                ", name='" + name + '\'' +
                '}';
    }
}

```



## @Resource（这是java实现的，没spring高级）

@Resource(name="cat2")

默认byName，再byType

# 12、使用注解开发

**简单的可以用注解，复杂的还是用xml比较好**



**在Spring4之后要使用注解开发，必须要保证aop的包导入了**

**使用注解需要支持，导入context的约束**

1. bean

   1. @Component

2. 属性如何注入

   1. @Value()

3. 衍生的注解

   1. @Component有几个衍生的，在我们web开发中，会按照mvc三层架构

      1. dao	@Repository

      2. service     @Service

      3. controller    @Controller

         这四个注解功能都是一样的，都是代表将某个类注册到Spring中，装配Bean

4. 自动装配

   1. @Autowired

5. 作用域

   1. @Scope("singleton")

6. 小结

   xml与注解：

   xml更加万能，适用于任何场所，维护简单方便

   注解不是自己的类使用不了，维护相对复杂

7. xml与注解最佳实践

   1. xml用来管理bean
   2. 注解只负责完成属性的注入



# 13、使用javaConfig来实现配置

完全不使用Spring的xml配置，全交给java来做

**javaConfig是Spring的一个子项目，在Spring4之后，它成为了一个核心功能**

用注解配置上下文，我们之前使用的是classpath配置

**通过纯注解去实现，不再需要xml**







@Configuration标记的类叫做配置类，它相当于xml文件的beans标签，这个类里面需要注册bean，相当于在beans中注册bean。
bean注册是有两种方式的。
第一种，使用@Bean标记一个返回javabean的方法，返回的javabean作为bean注册到beans中。
第二种，使用@Component+@ComponentScan的方式：@Component标记在要注册bean的类上，@ComponentScan标记在配置类上用于扫描组件。
最后一点，将配置类注册到上下文中来初始化容器。





配置

```java
package com.kang.config;

import com.kang.pojo.User;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

/**
 * @author klr
 * @create 2020-03-18-22:43
 */

//Configuration实现了@Component注解
    //Configuration也会被Spring容器托管，因为它本来就是一个@Component
    //@Configuration代表这是一个配置类，就和beans.xml一样

    //用componentScan就不用@bean了，二选一

//加了该注解就类似于xml的<beans>
@Configuration
@ComponentScan("com.kang.pojo")
@Import(KangConfig1.class)//类似于import
public class KangConfig {


    //@Bean相当于bean标签
    //这个方法的名字相当于标签中的id
    @Bean
    public User getUser(){
        return new User();//返回要注入到bean的对象
    }

}

```

实体类

```java
package com.kang.pojo;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

/**
 * @author klr
 * @create 2020-03-18-22:41
 */

//这里不需要@Component
    //@Bean与@Component的作用重复了
    //@Component和@Configuration+@Bean的区别
@Component
public class User {
    private String name;

    public String getName() {
        return name;
    }

    @Value("康")
    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }
}

```

测试

```java
import com.kang.config.KangConfig;
import com.kang.pojo.User;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

/**
 * @author klr
 * @create 2020-03-18-22:45
 */
public class MyTest {
    public static void main(String[] args) {

        //如果完全使用配置类方式去做，我们就只能通过AnnotationContext上下文来获取容器，通过配置类的class对象来加载
        ApplicationContext context = new AnnotationConfigApplicationContext(KangConfig.class);
        User getUser = context.getBean("getUser", User.class);
        System.out.println(getUser.getName());
    }
}

```

这种纯java的配置方式在SpringBoot中随处可见



# 注解大全（Spring特有的）要与SpringBoot的区分，好理解

1. @Autowired:自动装配通过类型，名字，配合@Qualifier(value="")更好用

2. @Resource:自动装配通过类型，名字

3. ```java
   @Component
   //等价于在xml中写 <bean id="user" class="com.kang.pojo.User">
   //id默认小写
   ```

4. ```
   @Value()  给属性赋值
   
   //相当于<property name="name" value="康"
   @Value("康")
   public String name;
   ```





# 14、代理模式

为什么要学代理模式，因为这就是SpringAOP的底层！（SpringAOP和SpringMVC必问）

![image-20200319104754415](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200319104754415.png)



代理模式的分类：

- 静态代理
- 动态代理

## 1、静态代理（demo1）

角色分析：

- 抽象角色（租房目标）：一般会使用接口或抽象类来解决
- 真实角色（房东）：被代理的角色
- 代理角色（中介）：代理真色角色，然后，我们一般会做些附属操作
- 客户Client（租房的人）：访问代理对象的人



代码步骤：

1.接口

```java
package com.kang.demo1;

//租房
public interface Rent {
    public void rent();
}

```

2.真实角色

```java
package com.kang.demo1;

/**
 * @author klr
 * @create 2020-03-19-11:03
 */
public class Host implements Rent {

    @Override
    public void rent() {
        System.out.println("房东要出租房子");
    }
}

```

3.代理角色

```java
package com.kang.demo1;

/**
 * @author klr
 * @create 2020-03-19-11:06
 */
public class Proxy implements Rent{
    //使用组合的方式
    private Host host;

    public Host getHost() {
        return host;
    }

    public void setHost(Host host) {
        this.host = host;
    }

    public Proxy(Host host) {
        this.host = host;
    }

    //代理帮房东租房子
    @Override
    public void rent() {
        visit();
        host.rent();
        fee();
        contract();
    }

    public void contract(){
        System.out.println("要签租赁合同");
    }
    public void fee(){
        System.out.println("要收中介费");
    }

    public void visit(){
        System.out.println("带客户参观房子");
    }
}

```

4.客户端角色

```java
package com.kang.demo1;

/**
 * @author klr
 * @create 2020-03-19-11:04
 */
public class Client {
    public static void main(String[] args) {
        //原来没有中介，我们直接找房东
//        Host host=new Host();
//        host.rent();


        //现在有了中介
        Host host=new Host();//它是客观存在的，你可以认为它是你挑选的房子

        //代理，除了帮房东租房还有一些附属操作
        Proxy proxy=new Proxy(host);

        //你不用面对房东，直接找中介租房即可
        proxy.rent();

    }
}

```

代理模式的好处：

- 可以使真实角色的操作更加纯粹，不用去关注一些公共的业务
- 公共的业务就交给代理角色，实现了业务的分工
- 公共业务发生扩展的时候方便集中管理

缺点：

- 一个真实角色就会产生一个代理角色，代码量会翻倍，开发效率变低

## 2、静态代理再理解（demo2

代码：对应demo02

## 3、AOP

![image-20200319120927988](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200319120927988.png)

## 4、动态代理详解（demo3

https://www.cnblogs.com/zyzblogs/p/11009872.html

静态代理的缺点：每次代理一个角色就会代码量翻倍

**动态代理的底层是反射，动态加载一些类**

- 动态代理和静态代理角色一样
- 动态代理的代理类是动态生成的，**不是我们直接写好的**

**动态代理分为两大类**

1. 基于接口的动态代理
   1. JDK动态代理（我们在这里使用）
2. 基于类的动态代理
   1. cglib
3. java字节码实现：javassist



需要了解两个类：Proxy 代理，InvocationHandler 调用处理程序

- **Interface InvocationHandler**

  - java.lang.reflect包下的

  - InvocationHandler是由  代理实例   的   调用处理程序  实现的接口

  - 每个代理实例   都有一个关联的   调用处理程序      /说白了有个代理类

    - 代理实例类似于

      ```java
      Proxy proxy=new Proxy(host);
      demo1的例子
      ```

  - 当在代理实例上调用方法时（proxy.rent（）），方法调用将被编码并分配到其   调用处理程序的invoke方法  （就是通过反射的方式去执行一个方法）

    - 比如

      ```java
      //得到对象的某个方法
      Class dog=Dog.class;//得到Dog的Class对象,这个Class对象代表Dog这个类
      Method[] method1=dog.getDeclaredMethods();//得到本类的所有方法，包括private
      for (Method method : methods1) {
                  System.out.println(method);
              }
      或
      Method setName = c1.getDeclaredMethod("setName", String.class);
      
      //通过反射创建一个类
      Dog dog1=(Dog) dog.getDeclaredConstructor().newInstance();
      
      //invoke通过反射获取的方法去跑
      //要知道方法属于哪个对象，因此要传入对象
      setName.invoke(dog1,"小黑")
      ```

  - 我们这里的invoke方法参数不一样

    - Object invoke(Object proxy,method,Object[] args)
    - proxy 我们的代理实例
    - 方法类似上面的setName///rent()
    - args参数，类似"小黑"
    - 结果返回实例类

- **Class Proxy**是一个类

  - java.lang.reflect.Proxy
  - Proxy提供了**创建动态代理类**（proxy）和**实例**的**静态方法**
  - 它也是由这些方法创建的所有动态代理类的父类

**类加载器：把类加载进内存，在堆中生成Class对象**



```txt
每一个动态代理类都要实现InvocationHandler接口

每个代理类的实例都关联到一个handler    

当我们通过代理对象调用一个方法时，这个方法的调用就会被转发，由InvocationHandle这个接口的invoke方法进行处理

Object invoke(Object proxy, Method method, Object[] args) throws Throwable
proxy:　 - 指代我们所代理的那个真实对象
method:　- 指代的是我们所要调用真实对象的某个方法的Method对象
args:　　- 指代的是调用真实对象某个方法时接受的参数


Proxy类用来动态创建一个代理对象的类
public static Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h) throws IllegalArgumentException
loader:　　    一个ClassLoader对象，定义了由哪个ClassLoader对象来对生成的代理对象进行加载
interfaces:　　一个Interface对象的数组，表示的是我将要给我需要代理的对象提供一组什么接口，如果我提供了一组接口给它，那么这个代理对象就宣称实现了该接口(多态)，这样我就能调用这组接口中的方法了
h:　　         一个InvocationHandler对象，表示的是当我这个动态代理对象在调用方法的时候，会关联到哪一个InvocationHandler对象上





同时我们一定要记住，通过 Proxy.newProxyInstance 创建的代理对象是在jvm运行时动态生成的一个对象，它并不是我们的InvocationHandler类型，也不是我们定义的那组接口的类型，而是在运行是动态生成的一个对象，并且命名方式都是这样的形式，以$开头，proxy为中，最后一个数字表示对象的标号。



```

https://blog.csdn.net/Dream_Weave/article/details/84183247

## 5、模板（demo4

```java
package com.kang.demo4;

import com.kang.demo3.Rent;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

/**
 * @author klr
 * @create 2020-03-19-14:24
 */


public class ProxyInvocationHandler implements InvocationHandler {
    //被代理的接口（真实对象）
    private Object target;

    public void setTarget(Object target) {
        this.target = target;
    }


    //生成得到代理类
    public Object getProxy(){
        return Proxy.newProxyInstance(this.getClass().getClassLoader(), target.getClass().getInterfaces(), this);
    }



    //处理代理实例，并返回结果
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {

        log(method.getName());//method是方法的对象，包含了这个方法所有的信息，这里是通过反射实现的

        //调用target接口的方法
        Object result = method.invoke(target, args);
        //System.out.println(method);
        return result;
    }


    //增加个日志功能
    public void log(String msg){
        System.out.println("执行了"+msg+"方法");
    }
}

//模板
//1.代理谁
//2.生成代理类
//3.调用代理程序的一些方法
```

Client

```java
package com.kang.demo4;

import com.kang.demo2.UserService;
import com.kang.demo2.UserServiceImpl;

/**
 * @author klr
 * @create 2020-03-19-15:50
 */
public class Client {
    public static void main(String[] args) {
        //真实角色
        UserServiceImpl userService=new UserServiceImpl();
        //代理角色，现在不存在，找它的handler去做
        ProxyInvocationHandler proxyInvocationHandler = new ProxyInvocationHandler();

        //第一步，设置要代理的真实对象
        proxyInvocationHandler.setTarget(userService);
        //第二步，动态生成代理角色
        //所谓动态就是我们没直接写Proxy类了，而是用下面这一语句直接生成了
        UserService proxy =(UserService) proxyInvocationHandler.getProxy();
        //第三步，调用接口中存在的方法
        proxy.add();
    }
}
```







## 6、最根本的区别

### 1、

静态代理调用方法时直接写死

```java
public void rent() {
        visit();
        host.rent();     这句
        fee();
        contract();
    }
```

动态代理直接在Client中调用

invoke通过反射执行方法

方法是动态的，没有写死

```java
 //处理代理实例，并返回结果
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        //调用target接口的方法
        Object result = method.invoke(target, args);
        return result;
    }
```

### 2、

静态代理的代理角色代理的类直接写死了

```
public class UserServiceProxy implements UserService
```

动态代理代理的类是可变的，只要实现了同一个类，减少了巨大的工作量

```
proxyInvocationHandler.setTarget(userService);
```







## 动态代理的好处：

- 可以使真实角色的操作更加纯粹，不用去关注一些公共的业务
- 公共的业务就交给代理角色，实现了业务的分工
- 公共业务发生扩展的时候方便集中管理

- 一个动态代理类代理的是一个接口，一般就是对应的一类业务
- 一个动态代理类可以代理多个类，只要是实现了同一个接口即可

## 动态代理总结

抽象角色（接口）：租房这个目标

真实角色：房东要出租房子

代理角色：中介帮组房东出租房子（还可以有看房签合同等一系列附加操作）

客户：租房

静态代理可以认为是一对一服务，一个中介人员负责一个房东，当有新的房东有另外的需求时就要

动态代理可认为一对多







# 15、AOP实现方式

![image-20200319165908803](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200319165908803.png)



**我们之前可以自己写一个动态代理**

**在这里就使用AOP的方式去做了**

![image-20200319170142100](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200319170142100.png)



![image-20200319170406484](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200319170406484.png)



## 使用Spring实现AOP

- 导入依赖包，使用AOP织入

- ```xml
  <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
  <dependency>
      <groupId>org.aspectj</groupId>
      <artifactId>aspectjweaver</artifactId>
      <version>1.9.5</version>
  </dependency>
  
  ```



### 方式一：使用Spring的API接口（可以用反射）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <!--    指定要扫描的包，这个包下面的注解就会生效
            这个标签已经包含了annotation-config那个标签
    -->
    <context:component-scan base-package="com.kang"/>

    <!--注册bean-->
    <bean id="userService" class="com.kang.service.UserServiceImpl"/>
    <bean id="log" class="com.kang.log.Log"/>
    <bean id="afterLog" class="com.kang.log.AfterLog"/>

    <!--配置AOP
    需要导入aop的约束
    -->
    <aop:config>
<!--        切入点    expression:表达式  固定的   execution(要执行的位置！ * * * * *)-->
        <aop:pointcut id="pointcut" expression="execution(* com.kang.service.UserServiceImpl.*(..))"/>
        <!--执行环绕增强

        把log这个类切入到上面的方法上去
        -->
        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>
    </aop:config>



</beans>
```

```java
package com.kang.log;

import org.springframework.aop.MethodBeforeAdvice;

import java.lang.reflect.Method;

/**
 * @author klr
 * @create 2020-03-19-17:23
 */
public class Log implements MethodBeforeAdvice {


    //method:要执行的目标对象的方法
    //args：参数
    //target：目标对象
    @Override
    public void before(Method method, Object[] args, Object target) throws Throwable {
        System.out.println(target.getClass().getName()+"的"+method.getName()+"方法被执行了");
    }
}

```

```java
package com.kang.log;

import org.springframework.aop.AfterReturningAdvice;

import java.lang.reflect.Method;

/**
 * @author klr
 * @create 2020-03-19-17:29
 */
public class AfterLog implements AfterReturningAdvice {

    //returnValue：返回值
    @Override
    public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
        System.out.println("执行了"+method.getName()+"方法,返回结果为："+returnValue);
    }
}

```



### 方式二：自定义来实现aop（功能比较弱）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <!--    指定要扫描的包，这个包下面的注解就会生效
            这个标签已经包含了annotation-config那个标签
    -->
    <context:component-scan base-package="com.kang"/>

    <!--注册bean-->
    <bean id="userService" class="com.kang.service.UserServiceImpl"/>
    <bean id="log" class="com.kang.log.Log"/>
    <bean id="afterLog" class="com.kang.log.AfterLog"/>

<!--&lt;!&ndash;    方式一&ndash;&gt;-->
<!--    &lt;!&ndash;配置AOP-->
<!--    需要导入aop的约束-->
<!--    &ndash;&gt;-->
<!--    <aop:config>-->
<!--&lt;!&ndash;        切入点    expression:表达式  固定的   execution(要执行的位置！ * * * * *)&ndash;&gt;-->
<!--        <aop:pointcut id="pointcut" expression="execution(* com.kang.service.UserServiceImpl.*(..))"/>-->
<!--        &lt;!&ndash;执行环绕增强-->

<!--        把log这个类切入到上面的方法上去-->
<!--        &ndash;&gt;-->
<!--        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>-->
<!--        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>-->
<!--    </aop:config>-->




<!--    方式二：自定义类-->
    <bean id="diy" class="com.kang.diy.DiyPointCut"/>
    <aop:config>
<!--        自定义切面，ref要引用的类-->
        <aop:aspect ref="diy">
            <!--切入点-->
            <aop:pointcut id="point" expression="execution(* com.kang.service.UserServiceImpl.*(..))"/>

            <!--通知-->
            <aop:before method="before" pointcut-ref="point"/>
            <aop:after method="after" pointcut-ref="point"/>

        </aop:aspect>
    </aop:config>



</beans>
```

```java
package com.kang.diy;

/**
 * @author klr
 * @create 2020-03-19-17:57
 */
public class DiyPointCut {
    public void before(){
        System.out.println("=========方法执行前");
    }
    public void after(){
        System.out.println("=========方法执行后");
    }
}

```



## 3、使用注解实现

```java
package com.kang.diy;

import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;

/**
 * @author klr
 * @create 2020-03-19-18:50
 */
//标注这个类是一个切面
@Component
@Aspect
public class AnnotationPointCut {
    @Before("execution(* com.kang.service.UserServiceImpl.*(..))")
    public void before(){
        System.out.println("====方法执行前====");
    }
    @After("execution(* com.kang.service.UserServiceImpl.*(..))")
    public void after(){
        System.out.println("====方法执行后====");
    }
}

```

```xml
<!--方式三-->
    <bean id="annotationPointCut" class="com.kang.diy.AnnotationPointCut"/>
    <!--开启注解支持-->
    <aop:aspectj-autoproxy/>
```



使用@Component时要加

```
<context:component-scan base-package="com.kang.diy"/>
```

否则使用<bean>注册

```
<!--    <bean id="annotationPointCut" class="com.kang.diy.AnnotationPointCut"/>-->
```



<!--开启注解支持-->
    <aop:aspectj-autoproxy/>



![image-20200319191156981](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200319191156981.png)



```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <!--    指定要扫描的包，这个包下面的注解就会生效
            这个标签已经包含了annotation-config那个标签
    -->
    <context:component-scan base-package="com.kang.diy"/>

    <!--注册bean-->
    <bean id="userService" class="com.kang.service.UserServiceImpl"/>
    <bean id="log" class="com.kang.log.Log"/>
    <bean id="afterLog" class="com.kang.log.AfterLog"/>

<!--&lt;!&ndash;    方式一&ndash;&gt;-->
<!--    &lt;!&ndash;配置AOP-->
<!--    需要导入aop的约束-->
<!--    &ndash;&gt;-->
<!--    <aop:config>-->
<!--&lt;!&ndash;        切入点    expression:表达式  固定的   execution(要执行的位置！ * * * * *)&ndash;&gt;-->
<!--        <aop:pointcut id="pointcut" expression="execution(* com.kang.service.UserServiceImpl.*(..))"/>-->
<!--        &lt;!&ndash;执行环绕增强-->

<!--        把log这个类切入到上面的方法上去-->
<!--        &ndash;&gt;-->
<!--        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>-->
<!--        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>-->
<!--    </aop:config>-->




<!--&lt;!&ndash;    方式二：自定义类&ndash;&gt;-->
<!--    <bean id="diy" class="com.kang.diy.DiyPointCut"/>-->
<!--    <aop:config>-->
<!--&lt;!&ndash;        自定义切面，ref要引用的类&ndash;&gt;-->
<!--        <aop:aspect ref="diy">-->
<!--            &lt;!&ndash;切入点&ndash;&gt;-->
<!--            <aop:pointcut id="point" expression="execution(* com.kang.service.UserServiceImpl.*(..))"/>-->

<!--            &lt;!&ndash;通知&ndash;&gt;-->
<!--            <aop:before method="before" pointcut-ref="point"/>-->
<!--            <aop:after method="after" pointcut-ref="point"/>-->

<!--        </aop:aspect>-->
<!--    </aop:config>-->


    <!--方式三-->
<!--    <bean id="annotationPointCut" class="com.kang.diy.AnnotationPointCut"/>-->
    <!--开启注解支持  JDK默认  cglib-->
    <aop:aspectj-autoproxy />

<!--JDK    <aop:aspectj-autoproxy proxy-target-class="false"/>-->
    <!--cglib   <aop:aspectj-autoproxy proxy-target-class="true"/>-->


</beans>
```

```java
package com.kang.diy;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.Signature;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;

/**
 * @author klr
 * @create 2020-03-19-18:50
 */
//标注这个类是一个切面
@Component
@Aspect
public class AnnotationPointCut {
    @Before("execution(* com.kang.service.UserServiceImpl.*(..))")
    public void before(){
        System.out.println("====方法执行前====");
    }
    @After("execution(* com.kang.service.UserServiceImpl.*(..))")
    public void after(){
        System.out.println("====方法执行后====");
    }

    //在环绕增强中，我们可以给定一个参数，代表我们要获取  处理切入的点
    @Around("execution(* com.kang.service.UserServiceImpl.*(..))")
    public void around(ProceedingJoinPoint jp) throws Throwable {
        System.out.println("环绕前");
        //获得签名
        Signature signature = jp.getSignature();
        System.out.println(signature);
        //执行方法
        Object proceed = jp.proceed();

        System.out.println("环绕后");
        System.out.println(proceed);


    }




}

```



# 16、整合Mybatis

步骤：

1. 导入相关jar包

   - junit
   - mybatis
   - mysql数据库
   - spring相关的
   - aop织入
   - mybatis-spring（new）

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <parent>
           <artifactId>spring-study</artifactId>
           <groupId>org.example</groupId>
           <version>1.0-SNAPSHOT</version>
       </parent>
       <modelVersion>4.0.0</modelVersion>
   
       <artifactId>spring-10-mybatis</artifactId>
   
       <dependencies>
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
               <version>5.1.48</version>
           </dependency>
           <dependency>
               <groupId>org.mybatis</groupId>
               <artifactId>mybatis</artifactId>
               <version>3.5.4</version>
           </dependency>
           <!--Spring操作数据库的话还需要一个spring-jdbc-->
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-jdbc</artifactId>
               <version>5.2.4.RELEASE</version>
           </dependency>
           <!--mybatis和spring整合的包-->
           <dependency>
               <groupId>org.mybatis</groupId>
               <artifactId>mybatis-spring</artifactId>
               <version>2.0.4</version>
           </dependency>
   
       </dependencies>
   
   </project>
   ```

2. 编写配置文件

3. 测试

## 1、回忆Mybatis

1. 编写实体类
2. 编写核心配置文件
3. 编写接口
4. 编写Mapper.xml
5. 测试

## 2、mybatis-spring

- MyBatis-Spring 会帮助你将 MyBatis 代码无缝地整合到 Spring 中。它将允许 MyBatis 参与到 Spring 的事务管理之中，创建映射器 mapper 和 `SqlSession` 并注入到 bean 中，以及将 Mybatis 的异常转换为 Spring 的 `DataAccessException`。最终，可以做到应用代码不依赖于 MyBatis，Spring 或 MyBatis-Spring。

- 



- **多了一个mapper实现类，因为要用spring要接管对象自动创建，mybatis时无法创建对象的**



1. 编写数据源
2. sqlSessionFactory
3. sqlSessionTemplate
4. 需要给接口加实现类
5. 将自己写的实现类注入到spring中
6. 测试使用



- 第一种

  ```java
  package com.kang.mapper;
  
  import com.kang.pojo.User;
  import org.mybatis.spring.SqlSessionTemplate;
  
  import java.util.List;
  
  /**
   * @author klr
   * @create 2020-03-19-20:45
   */
  public class UserMapperImpl implements UserMapper {
  
      //在原来，我们的所有操作都使用sqlSession来执行
      //现在，都是以sqlSessionTemplate
  
      private SqlSessionTemplate sqlSession;
  
      //DI通过set注入，因此必须要有set方法
      public void setSqlSession(SqlSessionTemplate sqlSession) {
          this.sqlSession = sqlSession;
      }
  
      @Override
      public List<User> selectUser() {
          UserMapper mapper = sqlSession.getMapper(UserMapper.class);
          return mapper.selectUser();
      }
  }
  
  ```

  

  ```xml
  <bean id="userMapper" class="com.kang.mapper.UserMapperImpl">
          <property name="sqlSession" ref="sqlSession"/>
      </bean>
  ```

- 第二种

- ```java
  package com.kang.mapper;
  
  import com.kang.pojo.User;
  import org.apache.ibatis.session.SqlSession;
  import org.mybatis.spring.support.SqlSessionDaoSupport;
  
  import java.util.List;
  
  /**
   * @author klr
   * @create 2020-03-19-21:26
   */
  public class UserMapperImplTwo extends SqlSessionDaoSupport implements UserMapper{
  
  
      //连注入的过程都省略了
      @Override
      public List<User> selectUser() {
          SqlSession sqlSession=getSqlSession();
          UserMapper mapper = sqlSession.getMapper(UserMapper.class);
          return mapper.selectUser();
      }
  }
  
  ```

- ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xmlns:aop="http://www.springframework.org/schema/aop"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
          https://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/aop
          https://www.springframework.org/schema/aop/spring-aop.xsd
          http://www.springframework.org/schema/context
          https://www.springframework.org/schema/context/spring-context.xsd">
  
      <import resource="spring-dao.xml"/>
  
  
      <bean id="userMapper" class="com.kang.mapper.UserMapperImpl">
          <property name="sqlSession" ref="sqlSession"/>
      </bean>
  
      <!--这个通过sqlSessionFactory,直接连sqlSession都不用了-->
      <bean id="userMapper1" class="com.kang.mapper.UserMapperImplTwo">
          <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
      </bean>
  
  </beans>
  ```



- spring-dao.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xmlns:aop="http://www.springframework.org/schema/aop"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
          https://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/aop
          https://www.springframework.org/schema/aop/spring-aop.xsd
          http://www.springframework.org/schema/context
          https://www.springframework.org/schema/context/spring-context.xsd">
      <context:component-scan base-package="com.kang.diy"/>
      <aop:aspectj-autoproxy />
  
  
      <!--DataSource:使用Spring的数据源替换Mybatis的配置   c3p0  dbcp  druid
      class:我们在这里使用Spring提供的JDBC
  
      org.springframework.jdbc.datasource.DriverManagerDataSource
      -->
      <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
          <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
          <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF-8
  "/>
          <property name="username" value="root"/>
          <property name="password" value="1625622764"/>
      </bean>
  
  
  
      <!--sqlSessionFactory-->
      <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
          <property name="dataSource" ref="dataSource"/>
  
          <!--绑定Mybatis配置文件
          这样他们两个就连起来了，可以在xml中扩展它
          -->
          <property name="configLocation" value="classpath:mybatis-config.xml"/>
          <property name="mapperLocations" value="classpath:com/kang/mapper/UserMapper.xml"/>
      </bean>
  
  
      <!--
      使用sqlSessionTemplate代替sqlSession
      sqlSessionTemplate:就是sqlSession
      -->
      <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
          <!--
          通过构造方法向sqlsession注入sqlSessionFactory，因为它没有set方法
          -->
          <constructor-arg index="0" ref="sqlSessionFactory"/>
      </bean>
  
  </beans>
  ```

- 测试

  ```java
  import com.kang.mapper.UserMapper;
  import com.kang.mapper.UserMapperImpl;
  import com.kang.pojo.User;
  import com.kang.utils.MybatisUtils;
  import org.apache.ibatis.session.SqlSession;
  import org.junit.Test;
  import org.springframework.context.ApplicationContext;
  import org.springframework.context.support.ClassPathXmlApplicationContext;
  
  import java.util.List;
  
  /**
   * @author klr
   * @create 2020-03-19-19:57
   */
  public class MyTest {
      @Test
      public void test(){
          ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
          UserMapper userMapper=context.getBean("userMapper1", UserMapper.class);
          for (User user : userMapper.selectUser()) {
              System.out.println(user);
          }
      }
  }
  
  ```

# 17、声明式事务

## 1、回顾事务

- 把一组业务当成一个业务来做；要么都成功，要么都失败
- 事务在项目开发中，十分的重要，涉及到数据的一致性问题，不能马虎！
- 确保完整性和一致性



事务的ACID原则：（面试必考）

- 原子性
- 一致性
- 隔离性
  - 多个业务可能操作同一个资源，防止数据损坏
- 持久性
  - 事务一旦提交，无论系统发生什么问题，结果都不会在被影响，被持久化的写到存储器中

## 2、Spring中的事务

### 1、声明式事务

要开启 Spring 的事务处理功能，在 Spring 的配置文件中创建一个 `DataSourceTransactionManager` 对象：

```
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
  <constructor-arg ref="dataSource" />
</bean>
```



```xml

    <!--配置声明式事务-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <constructor-arg ref="dataSource" />
    </bean>

    <!--结合AOP实现事务的织入-->
    <!--配置事务通知：-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <!--给哪些方法配置事务-->
        <!--配置事务的传播特性：new propagation-->
        <tx:attributes>
            <tx:method name="add" propagation="REQUIRED"/>
            <tx:method name="delete" propagation="REQUIRED"/>
            <tx:method name="update" propagation="REQUIRED"/>
            <tx:method name="query" read-only="true"/>
            <tx:method name="*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>
    
    <!--配置事务切入-->
    <aop:config>
        <aop:pointcut id="txPointCut" expression="execution(* com.kang.mapper.*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointCut"/>
    </aop:config>

</beans>
```



### 2、编程式事务





如果不配置事务，可能存在数据提交不一致的情况



# Maven依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>spring-study</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>spring-01-Ioc1</module>
        <module>spring-02-hellospring</module>
        <module>spring-03-ioc2</module>
        <module>spring-04-DI</module>
        <module>spring-05-Autowire</module>
        <module>spring-06-annotation</module>
        <module>spring-07-javaconfig</module>
        <module>spring-08-proxy</module>
        <module>spring-09-AOP</module>
    </modules>
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.2.4.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.12</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.5</version>
        </dependency>

    </dependencies>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.encoding>UTF-8</maven.compiler.encoding>
        <java.version>11</java.version>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
    </properties>

</project>
```



classpath指的是resources

# 错误

## 1、JDK5

在project struct中调 language level 11

在setting中调

创建新模块是可能会出现重新调的情况，按下面的方式可以解决

在pom.xml中加入

```xml
<properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.encoding>UTF-8</maven.compiler.encoding>
        <java.version>11</java.version>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
    </properties>
```



# Spring框架getBean()方法返回对象为什么只能转成接口对象，转换成接口的实例会报错？

https://blog.csdn.net/qq_38455201/article/details/104017873?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task