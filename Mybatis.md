# Mybatis

mybatis实际上就是通过对接口的反射进行动态代理创建对象（a=sqlSession.getMapper(XXX.class)     ），执行a.getXXX（应该把实现语句都生成了）然后连接数据库把xml中的sql语句实现并返回结果集，它实际上帮我们省了连接数据库等乱七八糟的操作

# Mybatis中文官网

https://mybatis.org/mybatis-3/zh/index.html

# 1、简介

## 1.1、什么是Mybatis

- MyBatis 是一款优秀的**持久层框架**

- 它支持定制化 SQL、存储过程以及高级映射。

- MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。

- MyBatis 可以使用简单的 **XML 或注解**来配置和映射原生类型、接口和 Java 的 POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。

## 1.2、Mybatis   Maven仓库

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.4</version>
</dependency>
```

## 1.3、持久化

- 持久化就是将程序的数据在持久状态和瞬时状态转化的过程
- 内存：**断电即失**
- 持久化：数据库、io文件
- 为什么需要持久化？因为有些对象不能让它丢掉，比如你存在支付宝里的 **钱** 而且 内存很贵

## 1.4、持久层

DAO(data access object)层、Service层、Controller层

- **完成持久化工作的代码块**
- 层的界限明显

## 1.5、为什么需要Mybatis

- **帮助程序员将数据存到数据库**
- 方便
- 简化JDBC代码
- 自动化
- **<u>使用的人多</u>**

Spring SpringMVC SpringBoot

# 2、第一个Mybatis程序

## 2.1、思路

1. 搭建环境
2. 导入Mybatis
3. 编写代码

## 2.2、创建Mysql数据库

```sql
-- create database `mybatis`;
use `mybatis`;
-- drop table `user`;
-- create table `user`(
-- 	id int(20) not null,
--     name varchar(20) default null,
--     pwd varchar(20) default null,
--     primary key(id)
-- )engine=innodb;

insert into `user` values
(1,'一','123456'),
(2,'二','123456'),
(3,'三','123456'),
(4,'四','123456')
```

## 2.3、新建Maven项目

删除src

导入依赖

```xml
<dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.47</version>
  </dependency>
  <dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.4</version>
  </dependency>
</dependencies>
```

## 2.4、创建模块

![image-20200306175526422](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200306175526422.png)

## 2.5、编写Mybatis的核心配置

在resources下创建Mybatis-config.xml文件

从官网得到放入文件内

```xml
官网的

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
        <property name="driver" value="${driver}"/>
        <property name="url" value="${url}"/>
        <property name="username" value="${username}"/>
        <property name="password" value="${password}"/>
      </dataSource>
    </environment>
  </environments>
  <mappers>
    <mapper resource="org/mybatis/example/BlogMapper.xml"/>
  </mappers>
</configuration>


最终版
尽量不要有中文注释，可能会出些奇怪的错

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="你自己的密码"/>
            </dataSource>
        </environment>
    </environments>
    <!--每一Mapper.xml都需要在Mybatis核心配置文件中注册-->
    <mappers>
        一定要用斜杠/，使用时请删除这句话
        <mapper resource="com/kang/dao/UserMapper.xml"/>
    </mappers>
</configuration>

```

连接Database

点击Test Connection可能会出现错误，要设置时区+8：00

点击第三个Schemas选项，勾选你的数据库名

![image-20200306190906049](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200306190906049.png)

![image-20200306191720970](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200306191720970.png)

## 2.6、编写Mybatis的工具类

```java
package com.kang.utils;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

/**
 * 工具类
 *
 * @author klr
 * @create 2020-03-07-13:29
 */
public class MybatisUtils {
    private static SqlSessionFactory sqlSessionFactory;
    //    直接读取resources下的xml
    //    放入static中初始就加载
    //    每个基于 MyBatis 的应用都是以一个 SqlSessionFactory 的实例为核心的。
    //    SqlSessionFactory 的实例可以通过 SqlSessionFactoryBuilder 获得。
    //    而 SqlSessionFactoryBuilder 则可以从 XML 配置文件或一个预先定制的 Configuration 的实例构建出 SqlSessionFactory 的实例。
    static {
        try {
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    //既然有了 SqlSessionFactory，顾名思义，我们就可以从中获得 SqlSession 的实例了。SqlSession 完全包含了面向数据库执行 SQL 命令所需的所有方法。你可以通过 SqlSession 实例来直接执行已映射的 SQL 语句。
    public static SqlSession getSqlSession(){
        return sqlSessionFactory.openSession();
    }
}

```

## 2.7、编写User实体类

```java
package com.kang.pojo;

/**
 * 实体类
 *
 * @author klr
 * @create 2020-03-07-14:02
 */
public class User {
    private int id;
    private String name;
    private String pwd;

    //不定义无参不能new空对象，使用反射时需要
    public User(){
    }
    public User(int id, String name, String pwd) {
        this.id = id;
        this.name = name;
        this.pwd = pwd;
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public String getPwd() {
        return pwd;
    }

    public void setId(int id) {
        this.id = id;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
    }
    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", pwd='" + pwd + '\'' +
                '}';
    }
}

```

## 2.8、编写UserDao

```java
package com.kang.dao;

import com.kang.pojo.User;

import java.util.List;

public interface UserDao {
    List<User> getUserList();
}

```

## 2.9、编写UserMapper.xml(接口实现类由原来的DaoImp转换为Mapper.xml文件）

**sql语句的;可写可不写**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace绑定一个对应的Dao/Mapper接口，该xml类似于以前的Dao实现类-->
<mapper namespace="com.kang.dao.UserDao">
    <!--id对应原来的方法,执行sql语句，返回List<User>结果集,resultType会对结果集封装-->
    <select id="getUserList" resultType="com.kang.pojo.User">
        select * from mybatis.user;
    </select>
</mapper>
```

## 2.10、Junit测试

**测试类**

```java
package com.kang.dao;

import com.kang.pojo.User;
import com.kang.utils.MybatisUtils;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;

import java.util.List;

/**
 * @author klr
 * @create 2020-03-07-14:25
 */
public class UserDaoTest {
    @Test
    public void test(){
        //得到sqlSession对象
        SqlSession sqlSession= MybatisUtils.getSqlSession();
        try {
            //执行sql 方式一：getMapper
            UserDao userDao=sqlSession.getMapper(UserDao.class);//得到Dao接口
            List<User> user=userDao.getUserList();//执行Dao接口里的方法

            //方式二，过时了，不建议使用
            //List<User> userList=sqlSession.selectList("com.kang.dao.UserDao.getUserList");
            for (User user1 : user) {
                System.out.println(user1);
            }
        }catch(Exception e){
            e.printStackTrace();
        }
        finally {
            //关闭sqlSession
            sqlSession.close();
        }
    }
}

```



## 2.11、坑

### 读取不到UserMapper.xml文件

**执行结果**

```java
org.apache.ibatis.binding.BindingException: Type interface com.kang.dao.UserDao is not known to the MapperRegistry.
```

**原因**

maven约定大于配置，我们可能会遇到配置文件无法被导出或者生效的问题，需要手动配置导出java文件下的东西，因为它默认的是resources文件下的

主要是为了解决以下语句读取不到的情况

```xml
<mappers>
        <mapper resource="com/kang/dao/UserMapper.xml"/>
    </mappers>
```

**解决办法**：在模块一下的pom.xml文件中添加resource

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>mybatis</artifactId>
        <groupId>com.kang</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>mybatis-01</artifactId>
    <!--在build中配置resources,来防止我们资源导出失败的问题-->
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

- **pom.xml下最好不要加中文注释，有的时候可以运行，有的时候就会报错，真的有毒，一时间想不到的话真的排查不出来**

## 2.12、最终文件目录

![image-20200307165555944](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200307165555944.png)

![image-20200307165635016](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200307165635016.png)

## 2.13、小知识

SqlSessionFactoryBuilder一旦创建了SqlSessionFactory就不再需要它了

SqlSessionFactory一旦被创建就应该在应用的运行期间一直存在，需要关闭

SqlSession关闭操作非常重要，建议放入trycatchfinally

## 2.14、分析错误

- 标签不要匹配错
- resouce绑定mapper需要使用路径
- 程序配置文件必须符合规范
- NullPointerException,没有注册到资源
- 输出的xml文件中存在中文乱码问题
- Maven资源没有导出问题（加上resources）

# 3、CRUD

**注意：增删改一定要提交事务**

## 3.1、namespace

- namespace中的包名要和Dao/Mapper接口的包一致

## 3.2、select/insert/update/delete

- id:namespace中对应的方法名
- resultType:sql语句执行的返回值
- parameterType:参数的类型

## 3.3、步骤

1. 编写接口
2. 编写对应的mapper中的sql语句
3. 测试

## 3.4、代码示例

- **把之前的UserDao接口名改成UserMapper**

- **UserMapper**

  ```java
  package com.kang.dao;
  
  import com.kang.pojo.User;
  
  import java.util.List;
  
  public interface UserMapper {
      //查询全部用户
      List<User> getUserList();
      //根据id查询用户
      User getUserById(int id);
      //插入一个用户
      int addUser(User user);
      //修改一个用户
      int updateUser(User user);
      //删除一个用户
      int deleteUser(int id);
  }
  
  ```

- **UserMapper.xml**

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <!--sql语句一定要加;-->
  <mapper namespace="com.kang.dao.UserMapper">
      <select id="getUserList" resultType="com.kang.pojo.User">
          select * from mybatis.user
      </select>
      <select id="getUserById" parameterType="int" resultType="com.kang.pojo.User">
          select * from mybatis.user where id=#{id};
      </select>
      <!--对象中的属性id,name,pwd可以直接取出来-->
      <insert id="addUser" parameterType="com.kang.pojo.User">
          insert into mybatis.user(id,`name`,pwd) values (#{id},#{name},#{pwd});
      </insert>
      <update id="updateUser" parameterType="com.kang.pojo.User">
          update mybatis.user set `name`=#{name},pwd=#{pwd} where id=#{id};
      </update>
      <delete id="deleteUser" parameterType="int">
          delete from mybatis.user where id=#{id};
      </delete>
  </mapper>
  ```

- UserMapperTest

  ```java
  package com.kang.dao;
  
  import com.kang.pojo.User;
  import com.kang.utils.MybatisUtils;
  import org.apache.ibatis.session.SqlSession;
  import org.junit.Test;
  
  import java.util.List;
  
  /**
   * @author klr
   * @create 2020-03-07-14:25
   */
  public class UserMapperTest {
      @Test
      public void getAllUser(){
          //得到sqlSession对象
          SqlSession sqlSession= MybatisUtils.getSqlSession();
          try {
              //执行sql 方式一：getMapper
              UserMapper userMapper =sqlSession.getMapper(UserMapper.class);//得到Dao接口
              List<User> user= userMapper.getUserList();//执行Dao接口里的方法
  
              //方式二，过时了，不建议使用
              //List<User> userList=sqlSession.selectList("com.kang.dao.UserMapper.getUserList");
              for (User user1 : user) {
                  System.out.println(user1);
              }
          }catch(Exception e){
              e.printStackTrace();
          }
          finally {
              //关闭sqlSession
              sqlSession.close();
          }
      }
  
      @Test
      public void getUserById(){
          SqlSession sqlSession=MybatisUtils.getSqlSession();
          try{
              UserMapper userMapper=sqlSession.getMapper(UserMapper.class);
              User user=userMapper.getUserById(4);
              System.out.println(user);
          }catch (Exception e){
              e.printStackTrace();
          }
          finally {
              sqlSession.close();
          }
      }
  
      //增删改需要提交事务
      @Test
      public void addUser(){
          SqlSession sqlSession=MybatisUtils.getSqlSession();
          try{
              UserMapper userMapper=sqlSession.getMapper(UserMapper.class);
              int user=userMapper.addUser(new User(5, "五", "123456"));
              System.out.println(user);
          }catch (Exception e){
              e.printStackTrace();
          }
          finally {
              sqlSession.commit();//一定要提交事务
              sqlSession.close();
          }
      }
  
      @Test
      public void updateUser(){
          SqlSession sqlSession=MybatisUtils.getSqlSession();
          try{
              UserMapper userMapper=sqlSession.getMapper(UserMapper.class);
              int user=userMapper.updateUser(new User(5, "五", "789123"));
              System.out.println(user);
          }catch (Exception e){
              e.printStackTrace();
          }
          finally {
              sqlSession.commit();//一定要提交事务
              sqlSession.close();
          }
      }
  
      @Test
      public void deleteUser(){
          SqlSession sqlSession=MybatisUtils.getSqlSession();
          try{
              UserMapper userMapper=sqlSession.getMapper(UserMapper.class);
              int user=userMapper.deleteUser(4);
              System.out.println(user);
          }catch (Exception e){
              e.printStackTrace();
          }
          finally {
              sqlSession.commit();//一定要提交事务
              sqlSession.close();
          }
      }
  }
  
  ```

## 3.5、万能的Map

- 通过map可以随意制造参数，想怎么写怎么写，比实体类（user）灵活很多

- 假设我们的实体类或数据库中的表、字段、参数过多，应当考虑使用map

- Map传递参数直接在sql中取出key即可

- 对象传递参数取对象的属性即可

- 只有一个基本类型参数的情况下可以在sql中直接取到

  ```java
      //万能的Map
      User getUserByMap(Map<String,Object> map);
  
      int addUserByMap(Map<String, Object> map);
  
  
  
  
  <insert id="addUserByMap" parameterType="map">
          insert into mybatis.user(id,`name`,pwd) values (#{userid},#{username},#{password});
      </insert>
          
          
          
  @Test
      public void addUserByMap(){
          SqlSession sqlSession=MybatisUtils.getSqlSession();
  
          try{
              Map<String, Object> map = new HashMap<>();
              map.put("userid",6);
              map.put("username",'六');
              map.put("password", "234567");
              UserMapper userMapper=sqlSession.getMapper(UserMapper.class);
              int user=userMapper.addUserByMap(map);
              System.out.println(user);
          }catch (Exception e){
              e.printStackTrace();
          }
          finally {
              sqlSession.commit();//一定要提交事务
              sqlSession.close();
          }
      }
  
  ```

## 3.6、模糊查询

- 要避免sql注入，如 1 or 1=1

- 2种方式

- java代码执行的时候传入通配符

  ```java
  map.put("username", "%五%");
  
  <select id="getUserByMap" parameterType="map" resultType="com.kang.pojo.User">
          select * from mybatis.user where `name` like #{username};
      </select>
  ```

- sql拼接时使用通配符

  ```xml
  map.put("username", "五");
  
  <select id="getUserByMap" parameterType="map" resultType="com.kang.pojo.User">
          select * from mybatis.user where `name` like "%"#{username}"%";
      </select>
  ```



![image-20200309143507885](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200309143507885.png)

# 4、配置解析

## **核心配置文件**

**mybatis-config.xml**

```xml
configuration（配置）
    properties（属性）
    settings（设置）
    typeAliases（类型别名）
    typeHandlers（类型处理器）
    objectFactory（对象工厂）
    plugins（插件）
    environments（环境配置）
    	environment（环境变量）
    		transactionManager（事务管理器）
    		dataSource（数据源）
    databaseIdProvider（数据库厂商标识）
    mappers（映射器）
```

## **环境配置（environments）**

- MyBatis 可以配置成适应多种环境
- **不过要记住：尽管可以配置多个环境，但每个 SqlSessionFactory 实例只能选择一种环境。**
  - 学会使用配置多套运行环境
- **每个数据库对应一个 SqlSessionFactory 实例**

### 事务管理器有两种，不要说只有一种

- JDBC
  -  这个配置直接使用了 JDBC 的提交和回滚设施，它依赖从数据源获得的连接来管理事务作用域
- MANAGED
  - 这个配置几乎没做什么。它从不提交或回滚一个连接，而是让容器来管理事务的整个生命周期（比如 JEE 应用服务器的上下文）。

### 数据源（连接数据库）dbcp、c3p0、druid

**池：用完可以回收**

- POOLED（连接池）
  - 利用“池”的概念将 JDBC 连接对象组织起来，避免了创建新的连接实例时所必需的初始化和认证时间。 这种处理方式很流行，能使并发 Web 应用快速响应请求。
- UNPOOLED（没有连接池）
  - 会每次请求时打开和关闭连接。虽然有点慢，但对那些数据库连接可用性要求不高的简单应用程序来说，是一个很好的选择。
- JNDI（正常连接）以前EJB用的

### **Mybatis默认的事务管理器就是JDBC，连接池：POOLED**

## 4.1、属性优化（properties）

- 我们可以利用properties属性来实现引用配置文件

- **这些属性可以在外部进行配置，并可以进行动态替换。你既可以在典型的 Java 属性文件中配置这些属性，也可以在 properties 元素的子元素中设置。**

- **properties 元素的子元素中设置**

  ```xml
  <dataSource type="POOLED">
                  <property name="driver" value="com.mysql.jdbc.Driver"/>
                  <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
                  <property name="username" value="root"/>
                  <property name="password" value="1625622764"/>
              </dataSource>
  ```

- **Java 属性文件中配置这些属性**(db.properties文件)

  ```properties
  driver=com.mysql.jdbc.Driver
  url=jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF-8
  username=root
  password=1625622764
  ```

  **在核心配置文件种引入properties标签**（在xml中所有的标签都可以规定其顺序）

  ​	或者配置文件中写一半，标签中写一半

  ```xml
  configuration>
      <!---->
      <properties resource="db.properties"/>
      <environments default="development">
          <environment id="development">
              <transactionManager type="JDBC"/>
              <dataSource type="POOLED">
  <!-- 第一种方法
                 <property name="driver" value="com.mysql.jdbc.Driver"/>-->
  <!--                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>-->
  <!--                <property name="username" value="root"/>-->
  <!--                <property name="password" value="1625622764"/>-->
                  <property name="driver" value="${driver}"/>
                  <property name="url" value="${url}"/>
                  <property name="username" value="${username}"/>
                  <property name="password" value="${password}"/>
              </dataSource>
          </environment>
  ```

  **优先级**：优先使用外部配置文件

## 4.2、别名优化（typealiases）

- **意义：类型别名可为 Java 类型设置一个缩写名字。 它仅用于 XML 配置，意在降低冗余的全限定类名书写。**

- 修改xml中resultType

  ```xml
  <select id="getUserList" resultType="com.kang.pojo.User">
      
      
  resultType="com.kang.pojo.User"
  ```

- **在mybatis-config.xml中的typealiases标签中设置别名**

  - 第一种给一个类取别名

    mybatis-config.xml

    ```xml
      <typeAliases>
            <typeAlias type="com.kang.pojo.User" alias="User"/>
        </typeAliases>
        <environments default="development">
            <environment id="development">
                <transactionManager type="JDBC"/>
                <dataSource type="POOLED">
    ```

    UserMapper.xml

    ```xml
    <select id="getUserList" resultType="User">
            select * from mybatis.user
        </select>
    ```

    

  - 第二种可以扫描一个包

    - 也可以指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean
    - 扫描实体类的包，它的默认别名就为这个类的类名，首字母小写！
    - **如果有注解则别名使用注解值**

    mybatis-config.xml

    ```xml
      <typeAliases>
            <package	name="com.kang.pojo"/>
        </typeAliases>
        <environments default="development">
            <environment id="development">
                <transactionManager type="JDBC"/>
                <dataSource type="POOLED">
    ```

    UserMapper.xml

    ```xml
    <select id="getUserList" resultType="user">	
            select * from mybatis.user
        </select>
    ```

    - 注解

      - ```java
        @Alias("hello")
        public class User {
            private int id;
            private String name;
            private String pwd;
        
        ```

      - resultType="hello"

  - 在实体类较少的时候使用第一种，实体类多的时候使用第二种**

    - **第一种可以自定义别名，第二种只能用类名**（也可以用注解）

## 4.3、映射器（Mapper）说明

- MapperRegister:注册绑定映射器

1. ```xml
   推荐使用
   <mappers>
           <mapper resource="com/kang/dao/UserMapper.xml"/>
       </mappers>
   ```

2. ```xml
   使用class文件绑定
   1.接口UserMapper要与配置文件UserMapper.xml同名
   2.两者要在同一个包下
   <mappers>
           <mapper class="com.kang.dao.UserMapper"/>
       </mappers>
   ```

3. ```xml
   使用package扫描
   1.接口UserMapper要与配置文件UserMapper.xml同名
   2.两者要在同一个包下
   
   <mappers>
           <mapper package="com.kang.dao"/>
       </mappers>
   ```

## 4.4、plugins

1. mybatis-plus
2. mybatis-generator-core:根据数据库自动生成
3. 通用mapper

## 4.5、生命周期和作用域

- 会导致并发问题

- mybatis-config.xml配置文件》创建sqlsessionfactorybuilder》builder创建sqlsessionfactory》sqlsession》拿到sqlMapper》结束
- ![image-20200315113401917](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200315113401917.png)

### 1.SqlSessionFactoryBuilder

- 一旦创建了SqlSessionFactory就不再需要它了
- 用局部变量写

### 2.SqlSessionFactory

- 可以当作数据库连接池（等待别人来连
- **一旦创建就应该在应用的运行期间一直存在**，不要多次创建它（多个连接池会造成资源的浪费
- 应用作用域

### 3.SqlSession

- 连接到连接池的一个请求
- 每个线程都应该有一个sqlsession实例
- 不是线程安全的
- 用完后关闭

![image-20200315114552435](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200315114552435.png)

- 这里的每一个Mapper都代表一个具体的业务

## 报错：ClassNotFound

![image-20200315173609440](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200315173609440.png)



# 5、解决属性名和字段名不一致

## 5.1、数据库中的字段

表中的字段与JavaBean中的字段不一致

![image-20200315174110869](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200315174110869.png)

## 5.2、User类

```java
public class User {
    private int id;
    private String name;
    private String password;
    //private String pwd;
    // 变成password，这样字段就不一致了
```

测试出现问题

返回值为null

```xml
select * from mybatis.user where id=#{id}
//类型处理器
select id,name,pwd from mybatis.user where id=#{id}
```

**解决方案：**

### **1、起别名**

```xml
select id,name,pwd as password
```

**结果：**

```xml
User{id=2, name='二', password='123456'}
```

### **2、resultType改成resultMap，resultMap结果集映射**

- ```xml
  resultType="com.kang.pojo.User"
  改成
  resultMap
  ```

- ```xml
  id name pwd
  id name password
  ```

- ```xml
  <!--    把结果映射成User-->
      <resultMap id="UserMap" type="User">
          <!--column数据库中的字段，properties实体类中的属性-->
          <result column="pwd" property="password"/>
      </resultMap>
      <select id="getUserById" parameterType="int" resultMap="UserMap">
          select * from mybatis.user where id=#{id};
      </select>
  ```

- 



## 5.3、出现报错或者null就点击maven的clean和test再run，很多问题就出在这

## 5.4、IDEA的模块图标下的小蓝点消失不见的解决方法

**将idea右侧的加号，加上该项目的pom.xml，刷新即可。**

---

# 6、日志

## 6.1、日志工厂

如果一个数据库操作出现了异常，需要排错，用日志！

以前：sout、debug

现在：日志

**logImpl:指定mybatis所用日志的具体实现，未指定时自动查找**log4j

## 6.2、STDOUT_LOGGING标准日志输出

```xml
<configuration>
    <properties resource="db.properties"/>
    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>
```

- **结果**
- ![image-20200315225509515](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200315225509515.png)

## 6.3、LOG4J

### 1、导包

```xml
<!-- https://mvnrepository.com/artifact/log4j/log4j -->
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```

### 2、log4j.properties配置文件

https://blog.csdn.net/manmanxiaohui/article/details/79922546

```properties
# priority  :debug<info<warn<error
#you cannot specify every priority with different file for log4j 
log4j.rootLogger=debug,stdout,info,debug,warn,error 
 
#console
log4j.appender.stdout=org.apache.log4j.ConsoleAppender 
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout 
log4j.appender.stdout.layout.ConversionPattern= [%d{yyyy-MM-dd HH:mm:ss a}]:%p %l%m%n
#info log
log4j.logger.info=info
log4j.appender.info=org.apache.log4j.DailyRollingFileAppender 
log4j.appender.info.DatePattern='_'yyyy-MM-dd'.log'
log4j.appender.info.File=./src/com/hp/log/info.log
log4j.appender.info.Append=true
log4j.appender.info.Threshold=INFO
log4j.appender.info.layout=org.apache.log4j.PatternLayout 
log4j.appender.info.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss a} [Thread: %t][ Class:%c >> Method: %l ]%n%p:%m%n
#debug log
log4j.logger.debug=debug
log4j.appender.debug=org.apache.log4j.DailyRollingFileAppender 
log4j.appender.debug.DatePattern='_'yyyy-MM-dd'.log'
log4j.appender.debug.File=./src/com/hp/log/debug.log
log4j.appender.debug.Append=true
log4j.appender.debug.Threshold=DEBUG
log4j.appender.debug.layout=org.apache.log4j.PatternLayout 
log4j.appender.debug.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss a} [Thread: %t][ Class:%c >> Method: %l ]%n%p:%m%n
#warn log
log4j.logger.warn=warn
log4j.appender.warn=org.apache.log4j.DailyRollingFileAppender 
log4j.appender.warn.DatePattern='_'yyyy-MM-dd'.log'
log4j.appender.warn.File=./src/com/hp/log/warn.log
log4j.appender.warn.Append=true
log4j.appender.warn.Threshold=WARN
log4j.appender.warn.layout=org.apache.log4j.PatternLayout 
log4j.appender.warn.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss a} [Thread: %t][ Class:%c >> Method: %l ]%n%p:%m%n
#error
log4j.logger.error=error
log4j.appender.error = org.apache.log4j.DailyRollingFileAppender
log4j.appender.error.DatePattern='_'yyyy-MM-dd'.log'
log4j.appender.error.File = ./src/com/hp/log/error.log 
log4j.appender.error.Append = true
log4j.appender.error.Threshold = ERROR 
log4j.appender.error.layout = org.apache.log4j.PatternLayout
log4j.appender.error.layout.ConversionPattern = %d{yyyy-MM-dd HH:mm:ss a} [Thread: %t][ Class:%c >> Method: %l ]%n%p:%m%n
```

![image-20200315234239430](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200315234239430.png)

**结果：**

![image-20200315234419138](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200315234419138.png)

### 3、简单使用：

1. 导入包import org.apache.log4j.Logger
2. 日志对象，参数为当前类的class
   1.  static Logger logger=Logger.getLogger(UserMapperTest.class);
3. 日志级别
   1. logger.info("info:进入了testLog4j");
              logger.debug("debug:进入了testLog4j");
              logger.error("error:进入了testLog4j");

```java
package com.kang.dao;

import com.kang.pojo.User;
import com.kang.utils.MybatisUtils;
import org.apache.ibatis.session.SqlSession;
import org.apache.log4j.Logger;
import org.junit.Test;

import java.util.List;

/**
 * @author klr
 * @create 2020-03-07-14:25
 */
public class UserMapperTest {
    static Logger logger=Logger.getLogger(UserMapperTest.class);
    @Test
    public void getAllUser(){
        //得到sqlSession对象
        SqlSession sqlSession= MybatisUtils.getSqlSession();
        try {
            //执行sql 方式一：getMapper
            UserMapper userMapper =sqlSession.getMapper(UserMapper.class);//得到Dao接口
            List<User> user= userMapper.getUserList();//执行Dao接口里的方法

            //方式二，过时了，不建议使用
            //List<User> userList=sqlSession.selectList("com.kang.dao.UserMapper.getUserList");
            for (User user1 : user) {
                System.out.println(user1);
            }
        }catch(Exception e){
            e.printStackTrace();
        }
        finally {
            //关闭sqlSession
            sqlSession.close();
        }
    }
    @Test
    public void getUserById(){
        SqlSession sqlSession= MybatisUtils.getSqlSession();
//        logger.info();
//        logger.debug();
        try {
            UserMapper userMapper =sqlSession.getMapper(UserMapper.class);//得到Dao接口
            User user=userMapper.getUserById(2);//执行Dao接口里的方法
            System.out.println(user);
        }catch(Exception e){
            e.printStackTrace();
        }
        finally {
            //关闭sqlSession
            sqlSession.close();
        }
    }
    @Test
    public void testLog4j(){
        logger.info("info:进入了testLog4j");
        logger.debug("debug:进入了testLog4j");
        logger.error("error:进入了testLog4j");
    }
}

```

### 4、日志文件所在位置

log4j.appender.info.File=./src/com/hp/log/info.log

![image-20200316001412670](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200316001412670.png)

# 7、分页

- 减少数据的处理量

## 1、Limit分页

```sql
语法：select * from user limit startindex,pagesize;

select * from user limit 0,4;
```

代码：

```java
//实现limit分页
    List<User> getUserByLimit(Map<String,Integer>map);




<!--    把结果映射成User-->
    <resultMap id="UserMap" type="User">
        <!--column数据库中的字段，properties实体类中的属性-->
        <result column="pwd" property="password"/>
    </resultMap>
    <select id="getUserById" parameterType="int" resultMap="UserMap">
        select * from mybatis.user where id=#{id};
    </select>
 <select id="getUserByLimit" parameterType="map" resultMap="UserMap">
        select * from mybatis.user limit #{startIndex},#{pageSize};
    </select>




public void getUserByLimit(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper user = sqlSession.getMapper(UserMapper.class);
        Map<String,Integer>map=new HashMap<>();
        map.put("startIndex",1);
        map.put("pageSize",4);
        for (User user1 : user.getUserByLimit(map)) {
            System.out.println(user1);
        }
        sqlSession.close();

    }
```

## 2、Rowbounds分页

不建议使用

## 3、pagehelper分页

# 8、使用注解开发

## 8.1、面向接口编程

接口：**定义与实现的分离**

- 第一类是对一个个体的抽象，它可对应为一个**抽象体**（abstract class）
- 第二类是对一个个体某一方面的抽象，即形成一个**抽象面**（interface）

**三个面向区别**

- 面向对象：考虑问题时以对象为单位，考虑它的属性及方法
- 面向过程：以一个具体的流程为单位，考虑它的实现
- 接口设计与非接口设计是针对复用技术而言的，与面向对象（过程）不是一个问题，更多的体现就是对系统整体的架构

## 8.2、使用注解

1. ```java
   public interface UserMapper {
       //查询全部用户
       @Select("select * from mybatis.user")
       List<User> getUserList();
   }
   ```

2. 删除UserMapper.xml

3. 修改mybatis-config.xml配置文件，绑定接口

   <mapper class="com.kang.dao.UserMapper"/>

4. 缺点，password=null,因为数据库中是pwd

5. 要完成复杂的最好用xml

6. **使用反射，可以得到注解信息**

7. **本质：反射机制实现，底层：动态代理**

8. 设置true自动提交事务

   ```java
       public static SqlSession getSqlSession(){
           return sqlSessionFactory.openSession(true);
       }
   
   ```

   ![image-20200317142954784](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317142954784.png)

![image-20200317143616054](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317143616054.png)

- **@Param()注解**
  - 基本类型的参数或者String类型需要加上
  - 引用类型不需要加
  - 如果只有一个基本类型的话可以忽略，但是建议大家都加上
  - 我们在SQL引用的就是我们这里的@Param("uid")中设定的属性名uid



# 9、Mybatis详细的执行流程

- configuration在build的时候就做了
  - ![image-20200317140150963](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317140150963.png)

![image-20200317142234921](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317142234921.png)

# 10、Lombok

1. 在IDEA中安装Lombok插件

2. 导入jar包

   ```xml
   <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
   <dependency>
       <groupId>org.projectlombok</groupId>
       <artifactId>lombok</artifactId>
       <version>1.18.12</version>
       <scope>provided</scope>
   </dependency>
   ```

3. ![image-20200317145211769](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317145211769.png)

- @Data
  - 自动生成
  - ![image-20200317145345004](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317145345004.png)



# 11、多对一处理

- **多对一**和一对多
  - 比如多个学生对应一个老师
  - 对学生而言：**关联association**
  - 对于老师而言：一对多，**包含/集合collection**
- 结果集映射resultMap

**SQL**

```sql
use `mybatis`;
create table `teacher`(
	id int(10) not null,
    `name` varchar(30) default null,
    primary key(id)
)engine=innodb ;
insert into teacher(id,`name`) values(1,'康老师');

create table `student`(
	id int(10) not null,
    `name` varchar(30) default null,
    tid int(10) default null,
    primary key(id),
    key fktid(tid),
    constraint fktid foreign key(tid) references teacher(id)
)engine=innodb ;
insert into student(id,`name`,tid)values(1,'小明','1');
insert into student(id,`name`,tid)values(2,'小红','1');
insert into student(id,`name`,tid)values(3,'小张','1');
insert into student(id,`name`,tid)values(4,'小李','1');
insert into student(id,`name`,tid)values(5,'小王','1');

```



- **测试环境搭建**

  1. 导入lombok
  2. 新建实体类Teacher,Student
  3. 建立Mapper接口
  4. 建立Mapper.XML文件
  5. 在核心配置文件中绑定注册Mapper接口或文件
  6. 测试查询是否能够成功

  ### ![image-20200317155832305](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317155832305.png)



## **查询学生**

### 1.类似于sql的子查询

按照查询嵌套处理

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.kang.dao.StudentMapper">
<!--    <select id="getStudent" resultType="Student">-->
<!--        select s.id,s.name,t.name from student s,teacher t where s.tid=t.id;-->
<!--    </select>-->

    <!--
        Student中有Teacher参数，如何解决属性名与字段名不一致的问题?
        思路：
        1.查询所有的学生信息
        2.根据学生信息的tid寻找对应的老师
    -->

    <select id="getStudent" resultMap="StudentTeacher">
        select * from student
    </select>
    <resultMap id="StudentTeacher" type="Student">
        <result property="id" column="id"/>
        <result property="name" column="name"/>
        <!--复杂的属性需要单独处理
            对象（老师是一个对象）：使用association
            集合：collection

            javaType给属性确定类型,teacher是Teacher类型
            然后嵌套查询
        -->
        <association property="teacher" column="tid" javaType="Teacher" select="getTeacher"/>
    </resultMap>
    <select id="getTeacher" resultType="Teacher">
        select * from teacher where id=#{id}
    </select>


</mapper>
```

```java
结果：
Student(id=1, name=小明, teacher=Teacher(id=1, name=康老师))
Student(id=2, name=小红, teacher=Teacher(id=1, name=康老师))
Student(id=3, name=小张, teacher=Teacher(id=1, name=康老师))
Student(id=4, name=小李, teacher=Teacher(id=1, name=康老师))
Student(id=5, name=小王, teacher=Teacher(id=1, name=康老师))
```

### 2.按照结果嵌套处理（联表）

```xml
<!--按照结果嵌套查询-->
    <select id="getStudent1" resultMap="StudentTeacher1">
        select s.id sid,s.name sname,t.name tname
        from student s,teacher t
        where s.tid=t.id
    </select>
    <resultMap id="StudentTeacher1" type="Student">
        <result property="id" column="sid"/>
        <result property="name" column="sname"/>
        <association property="teacher" javaType="Teacher">
            <result property="name" column="tname"/>
        </association>
    </resultMap>
```

```java
Student(id=1, name=小明, teacher=Teacher(id=0, name=康老师))
Student(id=2, name=小红, teacher=Teacher(id=0, name=康老师))
Student(id=3, name=小张, teacher=Teacher(id=0, name=康老师))
Student(id=4, name=小李, teacher=Teacher(id=0, name=康老师))
Student(id=5, name=小王, teacher=Teacher(id=0, name=康老师))
    
    
    这个id有点问题
```

# 12、一对多

## Teacher类

```java
public class Teacher {
    private int id;
    private String name;
    private List<Student> students;
}
```

## Student类

```java
ublic class Student {
    //组合
    private int id;
    private String name;
    //学生需要关联一个老师
    private int tid;
}
```

## TeacherMapper

```java
//获取老师，students=null
List<Teacher> getTeacher();

//获取指定老师下的所有学生及老师的信息
Teacher getTeacher1(@Param("tid")int id);

```

## xml

```xml
<select id="getTeacher" resultType="Teacher">
	select * from mybatis.teacher
</select>





<select id="getTeacher1" resultMap="TeacherStudent">
	select s.id sid,s.name sname,t.name tname,t.id tid
    from student s,teacher t
    where s.tid=t.id and t.id=#{tid}		tid对应@Param("tid")
</select>
<resultMap id="TeacherStudent" type="Teacher">
    <result property="id" column="tid"/>
    <result property="name" column="tname"/>
    <!--javaType指定属性的类型
    集合中的泛型信息，使用ofType获取，没什么区别-->
    <collection property="students" ofType="Student">
    	<result property="id" column="sid"/>
        <result property="name" column="sname"/>
        <result property="tid" column="tid"/>
    </collection>
</resultMap>



```

**按照查询嵌套处理**

![image-20200317170811499](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317170811499.png)

![image-20200317171107491](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317171107491.png)

## 测试

# 13、面试必问

## Mysql引擎

## InnoDB底层原理

## 索引

## 索引优化！

# 14、动态SQL

- **根据不同的条件生成不同的SQL语句**

```txt
if
choose (when, otherwise)
trim (where, set)
foreach
```

- **搭建环境**

![image-20200317203935733](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317203935733.png)

**getUUid，避免mysql冲突**

![image-20200317204310842](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317204310842.png)

- **属性名和字段名不一致**

  ```xml
  开启自动驼峰命名映射
  aB>a_b
  
  <setting name="mapUnderscoreToCamelCase" value="true"/>
  ```

- ![image-20200317204815695](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317204815695.png)

![image-20200317204859439](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317204859439.png)



## 1、IF

![image-20200317205530519](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317205530519.png)

![image-20200317205650864](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317205650864.png)

## 2、常用标签

choose对应于switch语句，when相当于case，otherwise相当于default

where/set/

where可以用trim来修剪

```java
where 元素只会在子元素返回任何内容的情况下才插入 “WHERE” 子句。而且，若子句的开头为 “AND” 或 “OR”，where 元素也会将它们去除。
```

![image-20200317210434119](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317210434119.png)



![image-20200317210617491](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317210617491.png)

![image-20200317210934004](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317210934004.png)

**所谓的动态SQL，本质还是SQL语句，只是我们可以在SQL层面去执行一个逻辑代码**

## 3、SQL片段

xml

```xml
有的时候，我们可能会将一些公共的部分抽取出来，方便复用

<sql id="if-title">
	<if test="title!=null">
    	title=#{title}
    </if>
    <if	test="author!=null">
        and author=#{author}
    </if>
</sql>


<select id="queryBlogIf" parameterType="map" resultType="blog">
	select * from mybatis.blog
    <where>
    	<include refid="if-title"></include>
    </where>
</select>
    

<where>自动去掉where/and/or
<set>自动去掉,
```

**最好基于单表来定义SQL片段，别太复杂**，sql标签中不要存在where

## 4、foreach

```xml
<select id="selectPostIn" resultType="domain.blog.Post">
  SELECT *
  FROM POST P
  WHERE ID in
  <foreach item="item" index="index" collection="list"
      open="(" separator="," close=")">
        #{item}
  </foreach>
</select>


(1,2,3,4,5)		open="(" separator="," close=")"
(1 or 2 or 3)	open="(" separator="or" close=")"
```

例

```xml
select * from blog where 1=1 and (id=1 or id=2 or id=3)/id in (1,2,3)

//构造一个map，map中可以存在一个集合
<select id ="get11" parameterType="map" resultType="blog">
	select * from blog
    <where>
    	<foreach collection="ids" item="id" open="and (" close=")" separator="or">
            id=#{id}
        </foreach>
    </where>
</select>
```

```jva
public void forEach(){
	...
	Map map=new HashMap();
	
	ArrayList<Integer> ids=new ArrayList<>();
	ids.add(1);
	ids.add(2);
	
	map.put("ids",ids);
	List<Blog> blogs=mapper.get11(map);
	...

}
```

# 15、缓存

## 1、简介

- 查询要连接数据库，因此耗资源，要想办法优化
  - 一次查询的结果，暂存在一个可以直接取到的地方
  - 再次查询时走缓存，不用走数据库了
- Cache
  - 存在内存中的临时数据
  - 解决高并发的问题
- ![image-20200317215850934](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317215850934.png)



- 70%都是读操作
- 使用缓存：经常查询且不经常改变的数据



## 1、一级缓存

- SqlSession级别的缓存，称为本地缓存，就是一次会话
- **默认开启**,在拿到和关闭期间有效
- close后就无效了
- 默认清除策略：LRU
- 一级缓存就是一个map

## 2、二级缓存

- 工作机制
  - 一个会话查询一条数据，这个数据就会被放在当前会话的一级缓存中
  - 如果当前会话关闭了，对应的一级缓存就没了。但开启二级缓存后，一级缓存中的数据被保存到二级缓存中
  - 新的会话查询信息就可以从二级缓存中获取内容
  - 不同的mapper查出的数据会放在自己对应的缓存中

- 需要手动开启，基于namespace级别的缓存

```xml
<cache/>
可以定义一些功能FIFO/LRU等
定期刷新
最多存多少
```

1. 开启全局缓存
   1. 在mybatis-config.xml添加标签
   2. <setting name="cacheEnabled" value="true"/>
2. ![image-20200317222112541](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317222112541.png)

- 问题，需要将实体类序列化，不然可能会报错

## 3、缓存失效

- **查询不同的东西**

- **增删改操作可能会改变原来的，所以必定会刷新缓存**

- 查询不同的**Mapper.xml**
- 手动清理缓存
  - sqlSession.clearCache();

![image-20200317224436192](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200317224436192.png)





Mapper映射解析

https://blog.csdn.net/niunai112/article/details/80247901?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task