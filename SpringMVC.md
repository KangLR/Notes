# SpringMVC

ssm:mybatis+Spring+SpringMVC	**MVC三层架构**

SpringMVC+Vue+SpringBoot+SpringCloud+Linux

SpringMVC的执行流程

SSM框架整合

我们可以将SpringMVC中所有要用到的bean，注册到Spring中

# MVC

模型（dao，service）

视图（jsp）

控制器（servlet）：接收用户请求，委托模型进行处理，处理完毕把返回的模型数据返回给视图，由视图负责展示

dao（连接数据库

service（调dao执行业务

servlet：转发/重定向（接收前端的数据，把数据交给service处理，service返回结果，servlet控制返回页面

jsp/html





pojo

vo

dto

![image-20200320093733132](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200320093733132.png)

# MVVM

M

V

VM

ViewModel:双向绑定



https://blog.kuangstudy.com/index.php/archives/318/

# 1、Servlet

在模块名上右击点add Framework support

# 2、初识SpringMVC

Spring的web框架围绕DispatcherServlet设计，**本质也是个Servlet**

DispatcherServlet就是将请求分发到不同的处理器（类）

所有的请求都会经过doService()

![image-20200320110244321](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200320110244321.png)

![image-20200320114623495](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200320114623495.png)

流程

1.前端控制器：DispatcherServlet

2.请求回车，所有请求都会经过它

![image-20200320122025806](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200320122025806.png)

3.委托请求给处理器

我们在springmvc-servlet.xml中配置的HandlerMapping等

4.处理器可以简单认为是Controller

![image-20200320122242101](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200320122242101.png)



![image-20200320155227866](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200320155227866.png)

![image-20200320155506505](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200320155506505.png)



adapter：实现controller接口都去适配一下，找到hellocontroller

hellocontroller实现了controller接口（函数式接口），要实现handlrRequest方法

```java
package com.kang.controller;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * @author klr
 * @create 2020-03-20-11:31
 */
//导入Controller接口
public class HelloController implements Controller {

    @Override
    public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
        //ModelAndView 模型和视图  不用写servlet了
        ModelAndView mv=new ModelAndView();
        
        //调用业务，但我们现在没有

        //封装对象，放在ModelAndView中的Model
        mv.addObject("msg","HelloSpringMVC");

        //封装要跳转的视图，放在ModelAndView中
        mv.setViewName("hello");    //在/WEB-INF/jsp/hello.jsp
        return mv;
    }
}

```

![image-20200320160245296](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200320160245296.png)



# 3、使用注解开发SpringMVC

![image-20200320173321735](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200320173321735.png)

Controller接口用注解写，而且用了注解就自动被spring装配了。不用再xml写<bean id .....class= ...>



url用@RequestMapping("/hello")实现



@RequestMapping("/hello")作用在类上时相当于多了一层路径



![image-20200320175443980](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200320175443980.png)



http://localhost:8080/good/hello

```java
package com.kang.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

/**
 * @author klr
 * @create 2020-03-20-17:38
 */

@Controller
@RequestMapping("/good")
public class HelloController {
    //当注解到类时，就相当于 //localhost:8080/应用名/good/hello   相当于给该类下的方法加了一层路径
    //一种父子关系
    @RequestMapping("/hello")
    public String hello(Model model){
        //封装数据
        model.addAttribute("msg","Hello,SpringMVCAnnotation!");


        return "hello";//会被视图解析器处理，拼接成XXX.jsp,被渲染出来
    }

    //要写多个映射，就多写几个方法，改个名字，不用写一堆servlet了
//    @RequestMapping("/hello")
//    public String hello12(Model model){
//        //封装数据
//        model.addAttribute("msg","Hello,SpringMVCAnnotation!");
//
//
//        return "hello12";//会被视图解析器处理，拼接成XXX12.jsp,被渲染出来
//    }

}

```

# 4、Controller及RestFul风格

## 1、Controller

![image-20200320182800058](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200320182800058.png)



```java
@Component	组件

@Service	service

@Controller	controller

@Repository	dao

这四个注解等效的，都代表它是一个组件
```

![image-20200320184430221](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200320184430221.png)



## 2、@RequestMapping

![image-20200320184625734](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200320184625734.png)



建议url写在方法上，别写在类上，调试太难

![image-20200320185126463](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200320185126463.png)



## 3、Restful风格

以前：localhost:8080/method?add=1&		非常麻烦



restful风格是以/的

localhost:8080/method/add/1/2/3/4

![image-20200320185807639](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200320185807639.png)





![image-20200320190831095](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200320190831095.png)



### 两种方式实现get/delete。。。

1.@RequestMapping(value = "/add/{a}/{b}",method = RequestMethod.GET)

2.

![image-20200320192211044](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200320192211044.png)



```java
package com.kang.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

/**
 * @author klr
 * @create 2020-03-20-19:00
 */
@Controller
public class RestFulController {

    //原来的：http://localhost:8080/add?a=2&b=3
//    @RequestMapping("/add")
//    public String add(int a, int b, Model model){
//        int res=a+b;
//        model.addAttribute("msg","结果为"+res);
//        return "restful";
//    }

    //RestFul:http://localhost:8080/add/a/b
    //跟传参似的，一一对应
    //http://localhost:8080/add/1/2
    //get方式
//    @RequestMapping("/add/{a}/{b}")
//    public String add(@PathVariable int a,@PathVariable int b, Model model){
//        int res=a+b;
//        model.addAttribute("msg","结果为"+res);
//        return "restful";
//    }

//    //限定delete方式，结果会失败
//    @RequestMapping(value = "/add/{a}/{b}",method = RequestMethod.GET)
//    public String add(@PathVariable int a,@PathVariable int b, Model model){
//        int res=a+b;
//        model.addAttribute("msg","结果为"+res);
//        return "restful";
//    }

    //用getmapper，与上面一起用浏览器就懵了，要避免这种情况
    @GetMapping(value = "/add/{a}/{b}")
    public String add1(@PathVariable int a,@PathVariable int b, Model model){
        int res=a+b;
        model.addAttribute("msg","结果为"+res);
        return "restful";
    }
}

```



**Spring MVC会自动实例化一个Model对象用于向视图中传值**



# 5、结果跳转方式



![image-20200321090845778](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200321090845778.png)





# 6、解决乱码问题

```xml

    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>com.kang.filter.EncodingFilter</filter-class>
    </filter>
    <!--过滤所有的请求，解决乱码问题-->
    <filter-mapping>
        <filter-name>encoding</filter-name>
        <url-pattern>/</url-pattern>
    </filter-mapping>
```



```java
package com.kang.filter;

import javax.servlet.*;
import java.io.IOException;

/**
 * @author klr
 * @create 2020-03-21-9:28
 */
public class EncodingFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        servletRequest.setCharacterEncoding("utf-8");
        servletResponse.setCharacterEncoding("utf-8");

        //一定要让链子继续往下走，不然就卡死了
        filterChain.doFilter(servletRequest,servletResponse);

    }

    @Override
    public void destroy() {

    }
}

```



controller中的方法要换成get方法，post还是会报乱码，或者把/换成/*也行

**SpringMVC给我们提供了解决乱码的，直接在XML中配置即可**

```java
<!--使用SpringMVC自带的乱码过滤-->
    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encoding</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

/*可以把jsp页面也包含进去

# 7、JSON

前后端分离：

后端部署后端，提供接口（我们写的controller就是个接口，别人可以通过地址栏访问到我们），提供数据



json（前端不知道后端的对象，统一用json）JSON.parse  / JSON.stringify()



前端独立部署：负责渲染后端的数据



Jackson:json解析工具

```xml
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.10.3</version>
</dependency>

```

## 解决JSON乱码

方式一

```java
    //用JSON实现，但一定要解决乱码
    //解决乱码，用produces属性设置编码格式为utf-8
        @RequestMapping(value = "/j2",produces = "application/json;charset=utf-8")
 
```

方式二：使用springMVC统一解决乱码

```xml
<mvc:annotation-driven>
    <mvc:message-converters register-defaults="true">
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <constructor-arg value="UTF-8"/>
        </bean>
        <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
            <property name="objectMapper">
                <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                    <property name="failOnEmptyBeans" value="false"/>
                </bean>
            </property>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```

@RestController不走视图解析器，统一返回字符串

## 日期格式返回JSON工具类

```java
package com.kang.utils;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.SerializationFeature;
import java.text.SimpleDateFormat;

/**
 * @author klr
 * @create 2020-03-21-11:41
 */
public class JsonUtils {
    public static String getJson(Object object) throws JsonProcessingException {
        return getJson(object,"YYYY-MM-DD HH:MM:ss");
        //很多源码的写法都是这样的，调用
    }
    public static String getJson(Object object, String dateFormat) throws JsonProcessingException {
        ObjectMapper mapper=new ObjectMapper();

        //不使用时间戳的方式
        mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS,false);
        //自定义日期的格式
        SimpleDateFormat sdf=new SimpleDateFormat(dateFormat);
        mapper.setDateFormat(sdf);

        return mapper.writeValueAsString(object);
    }
}

```

```java
package com.kang.controller;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.kang.pojo.User;
import com.kang.utils.JsonUtils;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.bind.annotation.RestController;

import java.util.ArrayList;
import java.util.Date;
import java.util.List;

/**
 * @author klr
 * @create 2020-03-21-11:06
 */
@RestController
//@Controller
public class JsonController {
    @RequestMapping("/j1")
//    @ResponseBody//它就不会走视图解析器(也就是不会转发任何jsp页面)，会直接返回一个字符串
    //把User对象转化成JSON字符串输出给前端
    public String json1(){
        User user=new User(1,"康",21);

        return user.toString();
    }
    //结果：User(id=1, name=kang, age=21)



    //用JSON实现，但一定要解决乱码
    //解决乱码，用produces属性设置编码格式为utf-8
        @RequestMapping(value = "/j2",produces = "application/json;charset=utf-8")
        @ResponseBody
    public String json2() throws JsonProcessingException {
        User user=new User(1,"康",21);

        //jackson  ObjectMapper
        ObjectMapper objectMapper=new ObjectMapper();
        String str=objectMapper.writeValueAsString(user);
        return str;
    }
    //结果：{"id":1,"name":"?","age":21}
    //2：{"id":1,"name":"康","age":21}


    @RequestMapping("j3")
    public String json3() throws JsonProcessingException {
        Date date=new Date();
        return JsonUtils.getJson(date,"YYYY-MM-DD HH:MM:ss");
    }

    @RequestMapping("j4")
    public String json4() throws JsonProcessingException {
        List<User> users=new ArrayList<>();
        User user1=new User(1,"康",21);
        User user2=new User(2,"柳",21);
        User user3=new User(3,"荣",21);
        User user4=new User(4,"号",21);
        users.add(user1);
        users.add(user2);
        users.add(user3);
        users.add(user4);

        return JsonUtils.getJson(users);
    }

}

```





fastjson:json解析工具

阿里开发的，方便实现json对象与javaBean对象的转换

```xml
<!-- https://mvnrepository.com/artifact/com.alibaba/fastjson -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.67</version>
</dependency>

```



# 8、SSM整合

```txt

```







# SpringMVC配置模板

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc
        https://www.springframework.org/schema/mvc/spring-mvc.xsd
">

    <!--自动扫描包，让指定包下注解生效，由IOC容器统一管理-->
    <context:component-scan base-package="com.kang.controller"/>

    <!--让SpringMVC不处理静态资源， .css  .js  .html  .mp3  .mp4
    自动过滤-->
    <mvc:default-servlet-handler/>

    <!--handlerMapper,adapter不用配了，直接用下面的搞定-->
    <mvc:annotation-driven/>






    <!--视图解析器
    模板引擎：thymeleaf  freemarker ...
    下面这个类是可以变得

    得到ModelAndView，把视图的名字拼接上
    交给DispacterServlet
    用户就拿到数据了
    -->
    <bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>




</beans>
```





# 视图解析器的作用

https://blog.csdn.net/niqinge/article/details/79130106







































# 问题

![image-20200320113753411](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200320113753411.png)

![image-20200320114127076](C:\Users\klr10\AppData\Roaming\Typora\typora-user-images\image-20200320114127076.png)

http://localhost:8080/springmvc_02_hellomvc_war_exploded/hello