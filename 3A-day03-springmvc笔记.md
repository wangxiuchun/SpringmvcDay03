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



![](https://images2015.cnblogs.com/blog/249993/201612/249993-20161212142542042-2117679195.jpg)







#### 2.依赖包

```
<dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>

    <dependency>
      <groupId>jstl</groupId>
      <artifactId>jstl</artifactId>
      <version>1.2</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-core</artifactId>
      <version>5.1.5.RELEASE</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.springframework/spring-beans -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-beans</artifactId>
      <version>5.1.5.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context-support</artifactId>
      <version>5.1.5.RELEASE</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.1.5.RELEASE</version>
    </dependency>
  </dependencies>
```

#### 3.注解

DaoImpl 实现类 用 **@Repository**

```
@Repository
public class StuDaoImpl implements StuDao {

    @Override
    public void selectStu() {
        System.out.println("查询成功!!!!!!!!!!!!!!!");
    }
}

```

ServiceImpl  实现类 用 **@Service**

```
@Service
public class StuServiceImpl implements StuService {
    @Autowired
    private StuDao dao;

    @Override
    public void selectStu() {
        dao.selectStu();
    }

    public void setDao(StuDao dao) {
        this.dao = dao;
    }
}
```

Controller 用 **@Controller**、**@RequestMapping**("/selectStu")

```
@Controller
//@RequestMapping("/stu")
public class StuController {
    List<Student> list = new ArrayList<Student>();

    //查询
    @RequestMapping("/selectStu")
    public ModelAndView selectStu(){
        ModelAndView mav=new ModelAndView();
        //填充模型数据

        mav.addObject("list",list);
        //返回视图
        mav.setViewName("/qiantai/selectStu");
        return  mav;
    }

    //添加
    @RequestMapping("/addStu")
    public String addStu(Student student){
        System.out.println(student.getName());
        list.add(student);
        return "redirect:/selectStu.action";
    }

```



**注：service层依赖dao层**



**main方法**

```
 public static void main(String[] args) {
        //实例化spring容器
        ApplicationContext act = new ClassPathXmlApplicationContext("spring.xml");
        //通过id获得bean
        StuService stuService = (StuService)act.getBean("stuServiceImpl");
        stuService.selectStu();
```

