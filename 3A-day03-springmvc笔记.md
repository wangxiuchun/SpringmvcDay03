#                       3A-day03-springmvc

#### 1.前端控制器（DispatcherServlet）、处理器映射器、处理器适配器、视图解析器

 	1..前端控制器（DispatcherServlet）写在**web.xml**中

​		

```
<!--前端控制器的配置-->
  <servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

    <!--  默认查找WEB-INF下的servlet-name.xml-->
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:spring.xml</param-value>
    </init-param>
  </servlet>

  <servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <!--
      /:所有请求，包括静态资源
      *.action
      *.do
    -->
    <url-pattern>*.action</url-pattern>
  </servlet-mapping>
```

2.处理器映射器、处理器适配器 写在**spring.xml**中

```
<!--配置springmvc注解驱动,包括处理器映射器和处理器适配器的配置-->
    <mvc:annotation-driven/>
```

3.视图解析器 写在**spring.xml**中

```
<!--视图解析器-->
    <!-- 配置返回视图的路径，以及识别后缀是jsp文件 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
        <property name="prefix" value="/WEB-INF/jsp"/>
        <property name="suffix" value=".jsp"/>
    </bean>
```

 