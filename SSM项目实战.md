# 以下为项目完整流程👇

# 1、导入依赖

## **pom.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>ssmbuild</artifactId>
    <version>1.0-SNAPSHOT</version>

    <!--依赖： junit，数据库驱动，连接池，servlet，jsp，mybatis，mybatis-spring，spring-->
    <dependencies>
        <!-- https://mvnrepository.com/artifact/junit/junit -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13</version>
            <scope>test</scope>
        </dependency>

        <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.19</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/com.mchange/c3p0 -->
        <dependency>
            <groupId>com.mchange</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.5.5</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/javax.servlet/servlet-api -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
            <scope>provided</scope>
        </dependency>

        <!-- https://mvnrepository.com/artifact/javax.servlet.jsp/jsp-api -->
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.2</version>
            <scope>provided</scope>
        </dependency>

        <!-- https://mvnrepository.com/artifact/javax.servlet/jstl -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.4</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.4</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.2.4.RELEASE</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.2.4.RELEASE</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.12</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.encoding>UTF-8</maven.compiler.encoding>
        <java.version>11</java.version>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
    </properties>

    <!--静态资源导出问题-->
    <build>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <!--                <filtering>true</filtering>-->
            </resource>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <!--                <filtering>false</filtering>-->
            </resource>
        </resources>
    </build>
</project>
```

## 疑难杂症

```xml
 	<properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.encoding>UTF-8</maven.compiler.encoding>
        <java.version>11</java.version>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
    </properties>

    <!--静态资源导出问题-->
    <build>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <!--                <filtering>true</filtering>-->
            </resource>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <!--                <filtering>false</filtering>-->
            </resource>
        </resources>
    </build>
```

# 2、连接数据库

## SQL语句

```sql
create database `ssmbuild`; 
use `ssmbuild`;
drop table if exists `books`;
create table books(
	bookID int(10) not null auto_increment comment '书ID',
    bookName varchar(100) not null comment'书名',
    bookCounts int(11) not null comment '数量',
    detail varchar(200) not null comment '描述',
    key bookID(boobookskID)
)engine=InnoDB

insert into books()values(1,'Java',1,'从入门到放弃'),
(2,'MySQL',10,'从删库到跑路'),
(3,'Linux',5,'从入门到套牢');
```



# 3、建包

- com
  - kang
    - controller
    - pojo
    - dao
    - service

# 4、resources

## 1、mybatis-config.xml

mybatis的核心配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>

    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>


    <!--给包下的类取别名，别名=类名首字母小写-->
    <typeAliases>
        <package name="com.kang.pojo"/>
    </typeAliases>


    <!--
        Mybatis本来要配置事务，数据源的，整合Spring之后，就可以交给Spring去做
    -->


    <!--两个名字一样，直接使用class-->
    <mappers>
        <mapper class="com.kang.dao.BookMapper"/>
    </mappers>



</configuration>
```



## 2、applicationContext.xml

spring的核心配置文件

## 3、database.properties

数据库参数配置

```properties
# 如果使用的是MySQL8.0，改成com.mysqk.cj.jdbc.Driver
jdbc.driver=com.mysql.jdbc.Driver
# 如果使用的是MySQL8.0，增加一个时区的配置；&serverTimeZone=Asia/Shanghai
jdbc.url=jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF-8
jdbc.username=root
jdbc.password=1625622764
```

## 4、spring-dao.xml

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
    <!--
        1.关联数据库配置文件
        用context
        通过spring来读
        我们之前是通过mybatis读的
    -->
    <context:property-placeholder location="classpath:database.properties"/>

    <!--
        2.连接池
        数据库连接池非常多
        dbcp：半自动化操作，不能自动连接
        c3p0：自动化操作（自动化的加载配置文件，并且可以自动设置到对象中）
        druid(很多公司在用)
        hikari(SpringBoot2.X默认使用的)
        我们之前用的使Spring原生的DriverManagerDataSource
        现在我们换个c3p0
    -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driver}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>

        <!--c3p0私有属性-->
        <property name="maxPoolSize" value="30"/>
        <property name="minPoolSize" value="10"/>
        <!--关闭连接后不自动commit-->
        <property name="autoCommitOnClose" value="false"/>
        <!--获取连接超时时间-->
        <property name="checkoutTimeout" value="10000"/>
        <!--当获取连接失败重试次数-->
        <property name="acquireRetryAttempts" value="2"/>
    </bean>

    <!--3.sqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!--绑定Mybatis配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
    </bean>

    <!--4.配置dao接口扫描包，动态的实现了dao接口并注入到Spring容器中
    不像之前写daoImpl类，然后继承factory，在手动注入spring，太麻烦了
    -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!--注入sqlSessionFactory-->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
        <!--要扫描的dao包-->
        <property name="basePackage" value="com.kang.dao"/>
    </bean>

</beans>
```

## 5、spring-service.xml

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

    <!--1.扫描service下的包，业务-->
    <context:component-scan base-package="com.kang.service"/>

    <!--2.将我们的所有业务类，注入到Spring，可以通过配置，或者注解实现

    ref="bookMapper"的由来：我们在spring-dao.xml配置了dao层扫描包，自动把BookMapper的实现类注入到了Spring容器中，id=bookMapper

    自动实现了getMapper（BookMapper.clss）生成对象

    多了一个mapper实现类，因为要用spring要接管对象自动创建，mybatis时无法创建对象的
    <bean id="userMapper" class="com.kang.mapper.UserMapperImpl">
        <property name="sqlSession" ref="sqlSession"/>
    </bean>
    -->
    <bean class="com.kang.service.BookServiceImpl" id="bookService">
        <property name="bookMapper" ref="bookMapper"/>
    </bean>
    <!--3.声明式事务:配置数据源
    要横切事务的话，还要写aop的织入，导入aop的包
    -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
       <!--注入数据源-->
        <property name="dataSource" ref="dataSource"/>
    </bean>


    <!--4.aop事务支持-->
</beans>
```

## 6、spring-mvc.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/mvc
        https://www.springframework.org/schema/mvc/spring-mvc.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
">

    <!--
    三步：映射器，适配器，视图解析器
    由于我们使用注解，可以省略前两个
    -->

    <!--
        所以
        1.注解驱动
        2.静态资源过滤
        3.扫描包：controller
        4.视图解析器
    -->

    <mvc:annotation-driven/>
    <mvc:default-servlet-handler/>
    <context:component-scan base-package="com.kang.controller"/>
    <bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

<!--    &lt;!&ndash;处理器映射器-->
<!--    根据bean的名字找-->
<!--    &ndash;&gt;-->
<!--    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>-->
<!--    &lt;!&ndash;处理器适配器&ndash;&gt;-->
<!--    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>-->
</beans>
```

## 7、web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!--DispatchServlet
    不走传统的servlet了，我们配置一个dispatcherServlet，这个是springmvc的核心：请求分发器/前端控制器

    所有请求被核心分发器接管
    -->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--保证DispatchServlet要绑定spring的配置文件-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring-mvc.xml</param-value>
        </init-param>
        <!--springmvc跟服务器一起启动-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <!--乱码过滤-->
    <filter>
        <filter-name>encodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <!--该参数设置了@Nuallable，因此没有提示-->
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

    <!--为了安全，设置session，为15分钟-->
    <session-config>
        <session-timeout>15</session-timeout>
    </session-config>

</web-app>
```



# SSM之Mybatis层👇

## 1、创建实体类Books

```java
package com.kang.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

/**
 * @author klr
 * @create 2020-03-21-14:45
 */

//Java中有个类叫Book，为了避免混乱，加个s
//使用Lombok插件
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Books {

    //尽量与数据库字段名一致

    private int bookID;

    private String bookName;

    private int bookCounts;

    private String detail;
}
```

## 2、用Mybatis写Dao（Mapper）层

### 1、创建接口BookMapper

```java
package com.kang.dao;

import com.kang.pojo.Books;
import org.apache.ibatis.annotations.Param;

import java.util.List;

public interface BookMapper {

    //增加一本书
    int addBook(Books books);

    //删除一本书
    int deleteBookById(@Param("bookId") int id);

    //更新一本书
    int updateBookById(int id);

    //查询一本书
    Books queryBookById(@Param("bookId") int id);

    //查询全部的书
    List<Books> queryAllBook();

}
```

### 2、写BookMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.kang.dao.BookMapper">

    <!--主键自增-->
    <insert id="addBook" parameterType="books">
        insert into ssmbuild.books(bookName,bookCounts,detail)
        values(#{bookName},#{bookCounts},#{detail})
    </insert>

    <delete id="deleteBookById" parameterType="_int">
        delete from ssmbuild.books where bookID=#{bookId}
    </delete>

    <update id="updateBookById" parameterType="books">
        update ssmbuild.books
        set bookName=#{bookName},bookCounts=#{bookCounts},detail=#{detail}
        where bookID=#{bookID};
    </update>

    <select id="queryBookById" resultType="books">
        select * from ssmbuild.books
        where bookID=#{bookId}
    </select>

    <!--这里没有一对多，多对一，返回的还是List里的东西-->
    <select id="queryAllBook" resultType="books">
        select * from ssmbuild.books
    </select>

</mapper>
```

## 3、写Service层

service层和dao层没有本质的区别

### 1、BookService接口

```java
package com.kang.service;

import com.kang.pojo.Books;
import org.apache.ibatis.annotations.Param;

import java.util.List;

public interface BookService {
    //增加一本书
    int addBook(Books books);

    //删除一本书
    int deleteBookById(int id);

    //更新一本书
    int updateBookById(int id);

    //查询一本书
    Books queryBookById(int id);

    //查询全部的书
    List<Books> queryAllBook();
}
```

### 2、BookServiceImpl（接口的实现类）

```java
package com.kang.service;

import com.kang.dao.BookMapper;
import com.kang.pojo.Books;

import java.util.List;

/**
 * @author klr
 * @create 2020-03-21-15:25
 */
public class BookServiceImpl implements BookService {

    //service层调dao层：组合Dao
    //实现set方法，让Spring托管它
    private BookMapper bookMapper;

    public void setBookMapper(BookMapper bookMapper) {
        this.bookMapper = bookMapper;
    }

    @Override
    public int addBook(Books books) {
        return bookMapper.addBook(books);
    }

    @Override
    public int deleteBookById(int id) {
        return bookMapper.deleteBookById(id);
    }

    @Override
    public int updateBookById(int id) {
        return bookMapper.updateBookById(id);
    }

    @Override
    public Books queryBookById(int id) {
        return bookMapper.queryBookById(id);
    }

    @Override
    public List<Books> queryAllBook() {
        return bookMapper.queryAllBook();
    }
}
```



## 4、Service和Dao就相当于MVC的model





# SSM之Spring层👇

---

## Spring整合dao层👇

## Spring就是个大杂烩，整合一切

## 1、写database.properties和spring-dao.xml

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

    <!--
        1.关联数据库配置文件
        用context
        通过spring来读
        我们之前是通过mybatis读的
    -->
    <context:property-placeholder location="classpath:database.properties"/>

    <!--
        2.连接池
        数据库连接池非常多
        dbcp：半自动化操作，不能自动连接
        c3p0：自动化操作（自动化的加载配置文件，并且可以自动设置到对象中）
        druid(很多公司在用)
        hikari(SpringBoot2.X默认使用的)
        我们之前用的使Spring原生的DriverManagerDataSource
        现在我们换个c3p0
    -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driver}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>

        <!--c3p0私有属性-->
        <property name="maxPoolSize" value="30"/>
        <property name="minPoolSize" value="10"/>
        <!--关闭连接后不自动commit-->
        <property name="autoCommitOnClose" value="false"/>
        <!--获取连接超时时间-->
        <property name="checkoutTimeout" value="10000"/>
        <!--当获取连接失败重试次数-->
        <property name="acquireRetryAttempts" value="2"/>
    </bean>

    <!--3.sqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!--绑定Mybatis配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
    </bean>

    <!--4.配置dao接口扫描包，动态的实现了dao接口并注入到Spring容器中
    不像之前写daoImpl类，然后继承factory，在手动注入spring，太麻烦了
    -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!--注入sqlSessionFactory-->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
        <!--要扫描的dao包-->
        <property name="basePackage" value="com.kang.dao"/>
    </bean>

</beans>
```

## Spring整合service层👇**（事务）**

![image-20200321165337318](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200321165337318.png)

还可以通过<import resouce=""/>

```xml
<import resource="classpath:spring-dao.xml"/>
<import resource="classpath:spring-service.xml"/>
```

## 2、spring-service.xml

### 1、方式一（手动注入bean）

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

    <!--1.扫描service下的包，业务-->
    <context:component-scan base-package="com.kang.service"/>

    <!--2.将我们的所有业务类，注入到Spring，可以通过配置，或者注解实现

    ref="bookMapper"的由来：我们在spring-dao.xml配置了dao层扫描包，自动把BookMapper的实现类注入到了Spring容器中，id=bookMapper

    自动实现了getMapper（BookMapper.clss）生成对象

    多了一个mapper实现类，因为要用spring要接管对象自动创建，mybatis时无法创建对象的
    <bean id="userMapper" class="com.kang.mapper.UserMapperImpl">
        <property name="sqlSession" ref="sqlSession"/>
    </bean>
    -->
    <bean class="com.kang.service.BookServiceImpl" id="bookService">
        <property name="bookMapper" ref="bookMapper"/>
    </bean>
    <!--3.声明式事务:配置数据源
    要横切事务的话，还要写aop的织入，导入aop的包
    -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
       <!--注入数据源-->
        <property name="dataSource" ref="dataSource"/>
    </bean>


    <!--4.aop事务支持-->
</beans>
```

### 2、方式二（通过注解@Service将对象注入Spring容器和@Autowired自动装配属性）

```java
package com.kang.service;

import com.kang.dao.BookMapper;
import com.kang.pojo.Books;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

/**
 * @author klr
 * @create 2020-03-21-15:25
 */

@Service
public class BookServiceImpl implements BookService {

    //service层调dao层：组合Dao
    //实现set方法，让Spring托管它
    private BookMapper bookMapper;

    @Autowired
    public void setBookMapper(BookMapper bookMapper) {
        this.bookMapper = bookMapper;
    }

    @Override
    public int addBook(Books books) {
        return bookMapper.addBook(books);
    }

    @Override
    public int deleteBookById(int id) {
        return bookMapper.deleteBookById(id);
    }

    @Override
    public int updateBookById(int id) {
        return bookMapper.updateBookById(id);
    }

    @Override
    public Books queryBookById(int id) {
        return bookMapper.queryBookById(id);
    }

    @Override
    public List<Books> queryAllBook() {
        return bookMapper.queryAllBook();
    }
}
```

# SSM之SpringMVC层👇

## 1、把项目设置成Web项目

![image-20200321171730165](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200321171730165.png)

## 2、写web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!--DispatchServlet
    不走传统的servlet了，我们配置一个dispatcherServlet，这个是springmvc的核心：请求分发器/前端控制器

    所有请求被核心分发器接管
    -->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--保证DispatchServlet要绑定spring的配置文件-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring-mvc.xml</param-value>
        </init-param>
        <!--springmvc跟服务器一起启动-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <!--乱码过滤-->
    <filter>
        <filter-name>encodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <!--该参数设置了@Nuallable，因此没有提示-->
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

    <!--为了安全，设置session，为15分钟-->
    <session-config>
        <session-timeout>15</session-timeout>
    </session-config>

</web-app>
```

## 3、写spring-mvc.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/mvc
        https://www.springframework.org/schema/mvc/spring-mvc.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
">

    <!--
    三步：映射器，适配器，视图解析器
    由于我们使用注解，可以省略前两个
    -->

    <!--
        所以
        1.注解驱动
        2.静态资源过滤
        3.扫描包：controller
        4.视图解析器
    -->

    <mvc:annotation-driven/>
    <mvc:default-servlet-handler/>
    <context:component-scan base-package="com.kang.controller"/>
    <bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

</beans>
```

# BUG排错

## 1、Bean不存在

![image-20200321190439044](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200321190439044.png)

### **第一种：去web.xml改**

```xml
把
<init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring-mvc.xml</param-value>
        </init-param>

改成
<init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:applicationContext.xml</param-value>
        </init-param>


因为applicationContext.xml整合了所有的配置文件，有注册的bean

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

    <import resource="classpath:spring-dao.xml"/>
    <import resource="classpath:spring-service.xml"/>
    <import resource="classpath:spring-mvc.xml"/>


</beans>
```

## 2、MySQL连接出错

![image-20200321192351159](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200321192351159.png)

- 1.因为我之前使用了别的数据库，没有改数据库名就直接拿来用了，直接报错，**这个细节一定要注意**

- MySQL数据库版本是8.0以上的话，驱动要加上cj

  - com.mysql.cj.jdbc.Driver

- ```properties
  # 如果使用的是MySQL8.0，改成com.mysqk.cj.jdbc.Driver
  jdbc.driver=com.mysql.jdbc.Driver
  # 如果使用的是MySQL8.0，增加一个时区的配置；&serverTimeZone=Asia/Shanghai
  jdbc.url=jdbc:mysql://localhost:3306/ssmbuild?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimeZone=UTC
  jdbc.username=root
  jdbc.password=1625622764
  ```

- 在xml里面配置：需要使用转义符：&amp;代替&

- 而在properties里面，则不能使用转义符，而直接使用:&

- 我是把它写在properties文件中，然后用下面的方法引入到spring-dao.xml

  ```
  <context:property-placeholder location="classpath:database.properties"/>
  ```

## 3、JSON的坑

@ResponseBody注解，方便的返回json数据
它会将内容或对象进行合适的格式转换作为 HTTP 响应正文返回

```
//根据关键字预查询
    @RequestMapping(value = "/keyword",produces ="application/json;charset=utf-8")
    @ResponseBody
    public Object keywordSerach(String bookName) throws JsonProcessingException {
        System.out.println(bookName);
        List<Books> books = bookService.queryBookByName(bookName);
//        ObjectMapper mapper=new ObjectMapper();
//        String str=mapper.writeValueAsString(books);
//        System.out.println(str);
        return books;
    }
```

JSON.parse()里必须是一个json格式的



//        ObjectMapper mapper=new ObjectMapper();
//        String str=mapper.writeValueAsString(books);
//        System.out.println(str);

这种是错的

![image-20200322161624512](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200322161624512.png)

![image-20200322161705446](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200322161705446.png)

@ResponseBody是作用在方法上的，@ResponseBody 表示该方法的返回结果直接写入 HTTP response body 中，一般在异步获取数据时使用【也就是AJAX】。
注意：在使用 @RequestMapping后，返回值通常解析为跳转路径，但是加上 @ResponseBody 后返回结果不会被解析为跳转路径，而是直接写入 HTTP response body 中。 比如异步获取 json 数据，加上 @ResponseBody 后，会直接返回 json 数据。@RequestBody 将 HTTP 请求正文插入方法中，使用适合的 HttpMessageConverter 将请求体写入某个对象。



https://blog.csdn.net/originations/article/details/89492884

