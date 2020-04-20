# day01

1.Spring 框架的构成 
  IOC   控制反转模块  这是Spring框架的核心 
  DAO   数据访问模块  Spring 框架对jdbc访问数据库做了简化和封装 
  WebMVC   Spring 对web部分 以及 MVC架构的支持 
  AOP   面向切面编程     在面向对象的基础上做了扩展 
  ORM   Mybatis   以面向对象的思想来操作数据库 
  JEE   Spring 对远程调用 消息服务  邮件服务等的支持 

2.什么是IOC
 Inversion   Of Control     控制反转 
  程序中一个组件 用到另外一个组件时  由原来的new 的方式  变成了由容器来进行  组件的创建  管理  
      和维护组件关系。
 这样做的好处是 可以大大降低组件之间的耦合度。    

3.Spring 容器介绍
 任何的java类 都可以在Spring 容器中创建对象 并交由容器管理  使用   和 维护组件关系 
 Spring 容器实现了 IOC  和  AOP 机制 
 Spring 容器的类型  ApplicationContext  和  BeanFactory 类型
4.Spring 容器的使用步骤  -----  使用容器获取日期类型的对象
 4.1 建立一个项目  导入jar包(ioc)   拷贝Spring 容器对应的配置文件到src下 
 4.2 在spring 容器配置文件中 写对象对应的配置 
    <bean id="对象的引用名"  class="包名.类名"></bean> 
 4.3 创建Spring 容器对象  和 从容器中获取对象 
    ApplicationContext  app  = new  ClassPathXmlApplicationContext("配置文件名");
    app.getBean("对象的引用名",类型.class);
5.定义一个员工类 Emp  这个类型中定义几个成员变量   int   id   String name   
  double  salary  提供get set方法  无参 和 全参构造  toString。然后通过Spring 容器创建
  Emp 类型的对象  并通过容器获取这个对象 打印。

6.Spring 容器创建对象的三种方式 
 6.1 构造器方式实例化 
 <bean  id="对象引用名"  class="包名.类名">
   默认调用类型对应的 无参构造 
 Date  date  = new  Date();
 6.2 静态工厂方法实例化
 使用类型 调用对应的静态方法  来获取对象 
 <bean  id="对象引用名"   class="包名.工厂类名"  factory-method="静态方法名" >  </bean>
 6.3 实例工厂方法实例化
  本质上 这种方式  是使用一个已经存在的对象  创建出另外一个类型的对象 
   <bean  id="对象引用名"   class="包名.目标类名"  
        factory-method="成员方法名"  factory-bean="工厂对象的id" ></bean>
7.Spring容器中Bean 对象的作用域
  Spring 容器中 默认一个对象的作用域是单例的   可以在spring 配置文件中通过  bean 标记的
  scope 属性来指定对象的作用域  取值有 singleton 或者 prototype 其它 的如 request
  session 等 到web部分才会涉及。
8.bean 对象的初始化 和 销毁 
  8.1 对象的初始化
  bean 对象创建完成后   可以通过beans标记中的  default-init-method 属性指定初始化方法 
  由于这样会影响到 spring容器中配置的所有对象，这样影响的范围比较大 所以对象对应的类型中没有对应
  的初始化方法也不会报错。也可以在bean标记中通过 init-method 来指定初始化方法 这样只影响到一个
  具体的对象  所以控制的比较精准  当这个对象对应的类型中没有对应的初始化方法时 程序会报错。
  8.2 对象的销毁
   bean 对象即将消亡    可以通过beans标记中的  default-destroy-method 属性指定销毁方法 
  由于这样会影响到 spring容器中配置的所有对象，这样影响的范围比较大 所以对象对应的类型中没有对应
  的销毁方法也不会报错。也可以在bean标记中通过 destroy-method 来指定销毁方法 这样只影响到一个
  具体的对象  所以控制的比较精准  当这个对象对应的类型中没有对应的销毁方法时 程序会报错。销毁方法只针对
  单例模式的对象。

9.写一个扑克牌类Card  里面有两个成员变量  String  suit  String  point  提供get  set
无参 全参  toString 方法 并通过spring 容器创建这个类型的对象  通过容器获取对象 并打印。
再写一个玩家类  Player  里面定义如下成员变量   String  name   int  age    double money 
Card  card  提供get  set无参 全参  toString 方法 并通过spring 容器创建这个类型的对象  
通过容器获取对象 并打印。  

# day02

1.写一个扑克牌类Card  里面有两个成员变量  String  suit  String  point  提供get  set
无参 全参  toString 方法 并通过spring 容器创建这个类型的对象  通过容器获取对象 并打印。
再写一个玩家类  Player  里面定义如下成员变量   String  name   int  age    double money 
Card  card  提供get  set无参 全参  toString 方法 并通过spring 容器创建这个类型的对象  
通过容器获取对象 并打印。

2.bean 对象的延迟实例化
单例对象 默认是 容器启动时 创建  可以通过  bean 标记的 lazy-init="true"  就可以让对象 
推迟实例化   只有用到这个对象时 才会创建对象    

3.什么是 DI?
  Dependence  Injection (依赖注入或者 依赖注射) 
    用来解决组件之间的装配问题
4.实现DI的方式 
 4.1 setter 注入 
  一个组件装配值时  参考的是类型对应的set方法   格式如下
  <bean   id=""  class="">   
      <property  name="set方法去掉set之后首字母小写"   value="值"></property>
  </bean>     
 如果装配的是一个复杂的对象  则 需要把 value 改成 ref  只有 八种基本类型 和 对应的封装类 枚举  String 这些
 可以使用 value  其它使用ref 
4.2 构造器注入 
把上面的 property 换成  constructor-arg
只不过 我们不但可以使用name="构造方法参数名"  还可以使用 index 代表参数的位置 编号从0开始 

4.3 自动化注入 
 在bean 标记中 使用  autowire 属性 可以完成自动化注入  
 autowire 的取值有  byName   byType   constructor
 byName  参考的是 类型中对应的set方法 
 byType  参考的是类型中对应的成员变量类型  然后去容器中找类型匹配的组件  但是可能会找到多个 找到多个就崩溃
 constructor  参考的是构造方法  只要有一个参数无法装配成功 则终止后续装配 但是不报错 

 5.组件扫描 
  5.1 什么是组件扫描
  Component  Scan   组件扫描
     它是Spring 提供的一套基于标注(注解)的技术   
     目的是为了简化 XML 的配置
  5.2 实现组件扫描的步骤 
  a.建立一个项目 导入jar包(ioc aop)  并拷贝配置文件到src下 
  b.在配置文件中开启组件扫描 
  <context:component-scan   base-package="包名"  />
  c.建立一个类   然后在类上打对应的标注 
  @Repository  持久层标注 一般加在DAO上       
  @Service     服务层标注  
  @Controller  控制层标注   
  @Component   通用层标注  
  d.创建Spring 容器 从容器中获取对象 进行测试

# day03

1.Bean 参数的注入 
 1.1 简单值的注入 
        八种基本类型   和 对应的封装类    枚举    String 
        在spring 容器中创建连接池对象
 1.2 复杂值的注入 
   只要把上面的  value  换成 ref 即可         
 1.3 集合参数的注入 
    1.3.1  java 中集合 
    List    Set   Map  Properties  
    1.3.2  List 
    <property  name=""  >
       <list>
           <value> 值 </value>
           <value> 值 </value>
       </list>
    </property> 
     1.3.3  Set 
    <property  name=""  >
       <set>
           <value> 值 </value>
           <value> 值 </value>
       </set>
    </property>        
    1.3.4 Map 
     <map>
          <entry  key="13844381988"  value="李伟杰"></entry>
          <entry  key="13844381986"  value="李嘉诚"></entry>        
    </map>     
    1.3.5 Properties      
     <props>
          <prop key="13844381988" >李伟杰</prop> 
          <prop key="13844381986" >李嘉诚</prop> 
          <prop key="13844381985" >李克强</prop> 
          <prop key="13844381984" >李白</prop> 
     </props>    
    1.3.6 集合参数的单独定义 
    <util:list  id=""      
    <util:set   id=""
    <util:map   id=""
    <util:properties  id=""      
    1.3.7 Properties 单独定义时 可以关联一个.properties 文件 
    
2.Spring DAO 对JDBC的改进 
      简化了DAO 实现类组件的编写  对jdbc的操作步骤 做了简化和封装 
      提供了基于AOP的事务管理  
     对JDBC中的异常 做了封装   把原来的检查异常 封装成了继承自 运行异常的一个异常类  
     DataAccessException 
3.SpringDAO的核心类 
  JdbcTemplate    jdbc模板类  
           自动加载驱动   自动获取连接   自动获取sql执行环境    以及 自动释放资源 
  JdbcDaoSupport   jdbc  DAO 的支持类 
           这个类 可以提供模板对象
4.Spring DAO 访问数据库的步骤   以查询某张表的数据量为例 
  4.1 建立一张银行账户表  并为表建立对应的序列  插入测试数据 提交 
  create  table  xdl_bank_account_29(
      id     number  constraint xdl_bank_account_29_id_pk  primary key,
      acc_no   varchar2(30)  constraint xdl_bank_account_29_acc_no_uk unique,
      acc_password  varchar2(30),
      acc_money   number
  );
  create  sequence  xdl_bank_account_29_id_seq;
  insert into  xdl_bank_account_29 values(xdl_bank_account_29_id_seq.nextval,
     'abc','123',1);
  insert into  xdl_bank_account_29 values(xdl_bank_account_29_id_seq.nextval,
     'liweijie','123',1234567);
  insert into  xdl_bank_account_29 values(xdl_bank_account_29_id_seq.nextval,
     'canglaoshi','123',1);
  commit;          
  4.2 建立一个项目 导入jar包(ioc aop dao 数据库驱动  连接池)  拷贝配置文件到src下
  4.3 编写DAO 接口 
  4.4 编写DAO 的实现类  继承 JdbcDaoSupport 利用模板完成查询 
  4.5 开启组件扫描   在DAO 实现类上打对应的标注 
      同时给 JdbcDaoSupport 这个类 注入一个  dataSource 对象 
  4.6 获取DAO的实现类 测试 

 5.重新建立一个项目  然后使用Spring DAO 访问数据库的步骤  完成 
 根据id 查询银行账号 。 

 6. 使用Spring DAO 访问数据库的步骤  完成 根据id 查询银行账户 。 
    6.1 建立一个项目 导入jar包(ioc aop dao 数据库驱动  连接池)  拷贝配置文件到src下
    6.2 根据表建立对应的实体类 XdlBankAccount
    6.3 编写DAO 接口 
    6.4 编写DAO 的实现类  继承 JdbcDaoSupport 利用模板完成查询 
    6.5 开启组件扫描   在DAO 实现类上打对应的标注 
      同时给 JdbcDaoSupport 这个类 注入一个  dataSource 对象 
    6.6 获取DAO的实现类 测试 

# day04

.根据账号 和 密码 查询银行账户 
 1.1 建立一个项目 导入jar包(ioc aop dao 连接池数据库驱动)  拷贝配置文件到src   
 1.2 根据表 建立对应的实体类 
 1.3 设计DAO 接口 
 1.4 编写DAO 的实现类 让其继承 JdbcDaoSupport   并实现DAO  
    利用父类的模板  完成查询 (依赖RowMapper)
 1.5 开启组件扫描  创建dataSource   注入给JdbcDaosupport  还有创建DAO实现类
     对应的对象 
 1.6 测试

2.查询所有的银行账户 
 2.1 建立一个项目 导入jar包(ioc aop dao 连接池数据库驱动)  拷贝配置文件到src   
 2.2 根据表 建立对应的实体类 
 2.3 设计DAO 接口 
 2.4 编写DAO 的实现类 让其继承 JdbcDaoSupport   并实现DAO  
    利用父类的模板  完成查询 (依赖RowMapper)
 2.5 开启组件扫描  创建dataSource   注入给JdbcDaosupport  还有创建DAO实现类
     对应的对象 
 2.6 测试

3.如何完成增删改 
 使用JdbcTemplate 对应的  update 方法 
以增加银行账户为例 
 3.1 建立一个项目 导入jar包(ioc aop dao 连接池数据库驱动)  拷贝配置文件到src   
 3.2 根据表 建立对应的实体类 
 3.3 设计DAO 接口 
 3.4 编写DAO 的实现类 让其继承 JdbcDaoSupport   并实现DAO  
    利用父类的模板  完成增加
 3.5 开启组件扫描  创建dataSource   注入给JdbcDaosupport  还有创建DAO实现类
     对应的对象 
 3.6 测试

4.根据账号  删除银行账户 

5.更新账号  设计一个方法 可以给一个账户增加钱 

6.不继承JdbcDaoSupport 的方式 完成对数据库的访问 
 和之前的不同在于 需要我们自己在容器中创建一个模板对象 (模板对象依赖于 dataSource)
 再把模板 注入给 DAO 的实现类  使用自己的模板完成对应的操作

7.Spring 的声明式事务 
    通过Spring 的配置 讲操作纳入到事务管理之中
    解除了事务管理代码 和  业务代码的耦合度 
    当不需要事务管理时  可以从配置文件中将其移除
8.Spring 声明式事务的实现步骤 
 8.1 开启声明式事务
 <tx:annotation-driven   transaction-manager="事务管理器id"   
    proxy-target-class="false" />

    proxy-target-class  如果是false 代表优先使用sun公司的代理机制生成代理  
    proxy-target-class  如果是true  代表使用CGLIB的代理机制生成代理 
 8.2  在Spring 容器中创建一个事务管理器对象----  依赖于dataSource    
   <bean   id="事务管理器id"  
      class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
      <property   name="dataSource"   ref="dataSource" /> 
   </bean>
 8.3 在需要事务管理的类或者方法上加事务管理标注
   @Transactional  
9.@Transactional 的属性 
  rollbackFor    Spring 的声明式事务 默认只针对运行时异常进行事务回滚  
  noRollbackFor   针对指定的运行时异常 不回滚 
  isolation      事务隔离级别  
               读未提交     读提交     可重复读       序列化 
               用来解决数据库中的三大读问题
               脏读       一个事务读取到了另外一个事务没有提交的数据       
              不可重复读  一个事务在开始时 读取了一份数据    另外一个事务修改了这份数据 并进行了提交  
                     当第一个事务再次读取数据时 发现数据发了改变。
               幻读    一个事务统计了整张表的所有数据    另外一个事务增加了数据 并进行了提交   当再次
                      统计数据时 数据发生了改变         
   readOnly    只读事务  只有当事务语句都是查询时 才能使用这个属性的值是true
                默认这个属性的值是false。
   propagation  事务传播特性     一个方法 去调用另外一个事务方法时  事务应该如何表现            
      Propagation.REQUIRED  如果当前方法没有事务  则 会开启新事务   如果当前方法有事务  则将事务
               加入到当前事务之中。
propagation_requierd：如果当前没有事务，就新建一个事务，如果已存在一个事务中，加入到这个事务中，这是最常见的选择。
propagation_supports：支持当前事务，如果没有当前事务，就以非事务方法执行。
propagation_mandatory：使用当前事务，如果没有当前事务，就抛出异常。
propagation_required_new：新建事务，如果当前存在事务，把当前事务挂起。
propagation_not_supported：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
propagation_never：以非事务方式执行操作，如果当前事务存在则抛出异常。
propagation_nested：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与propagation_required类似的操作   

# day05

1.MVC的五大核心组件 
 DispatcherServlet       请求的入口 
 HandlerMapping          请求的派发  让请求和控制器建立一一对应的关系
 Controller              真正处理请求的组件 
 ModelAndView            封装数据信息和视图信息的对象 
 ViewResolver            视图处理器 

2.Spring MVC的编写步骤
 2.1 建立一个项目   导入jar包(ioc mvc)  并拷贝配置文件到src下   在WEB-INF下 
    建立一个jsp 页面 
 2.2 在WEB-INF下的 web.xml 中  配置DispatcherServlet  并使用初始化参数 
  contextConfigLocation关联Spring的配置文件
 2.3 在Spring 配置文件中 配置HandlerMapping  的  实现类 
   SimpleUrlHandlerMapping  并且要配置 请求和控制器的对应关系 
 2.4 编写控制器的实现类   实现 Controller 接口 
 在接口方法中 返回 ModelAndView    同时在容器中创建 控制器对象 
 2.5 在Spring 配置文件中  配置ViewResolver 的实现类
  InternalResourceViewResolver   需要配置前缀和后缀 

 3.使用Spring MVC 访问WEB-INF下的jsp   
         
 4.基于标注的Spring  MVC  
  4.1 建立一个项目 导入jar包(ioc aop mvc)  拷贝配置文件到src 下  并 在WEB-INF下建立jsp文件 
  4.2 在web.xml 中配置 DispatcherServlet  并通过 初始化参数 contextConfigLocation 
    关联Spring 配置文件 
  4.3 开启组件扫描 和 标注形式的mvc
  <context:component-san  base-package=""  />
  <mvc:annotation-driven />   
    底层创建了一个 RequestMappingHandlerMapping的组件
  4.4 写一个java类   不用实现Controller接口 
  @RequestMapping("/请求路径")
    控制器方法的返回值类型 可以是String 也可以是 ModelAndView   方法名自由了(参数自由了){

  }  
   在类上加对应的标注  @Controller   可以把普通类 变成控制器  并在容器中创建Controller 对象
  4.5 配置视图处理器   

 6.使用标注形式的 mvc  访问 WEB-INF下的  register.jsp 

# day06

1.搭建基于标注的  mvc

2.Spring  MVC 控制器中如何接收页面请求参数
  2.1 在控制器方法上定义 HttpServletRequest 参数 然后使用request 获取 
  2.2 直接定义和页面参数相同的控制器参数  参数名相同则容器自动获取 
  2.3 当页面参数  和 控制器参数不一致  如何解决?
  @RequestParam("参数名")  可以让请求参数  和 控制器参数对应 
  2.4 定义对象类型的参数 ---  请求参数名 和 set方法保持对应 

3.Spring 控制器中 如何将数据传递给页面 
 3.1 使用域对象 向页面传输
   request   session   ServletContext 
   setAttribute("key",value)
 3.2 使用ModelAndView  进行数据传递
    getModel().put(key,value)
    getModelMap().addAttribute(key,value) 
 3.3 使用 Model  进行数据传输 
   addAttribute("key",value)
 3.4 使用ModelMap 传递数据 
  addAttribute(key,value) 
  put(key,value)
 3.5 使用默认的传输机制 
  当定义复杂的自定义控制器参数时  则控制器会自动把这个类型的数据放入request中
 默认是类名首字母小写  可以使用 @ModelAttribute("名字")  改变默认规范  

 4.Spring 控制器中如何实现重定向?
 4.1 当控制器方法返回 String 类型时 
    在请求路径前  加 redirect:
 4.2 当控制器方法返回ModelAndView 时 
  RedirectView  结合请求来完成     

 5.中文乱码的处理 
  tomcat8 中  get 请求没有乱码问题  
  tomcat8 中  post 请求 乱码的处理  
  a.原来的方式依然可用  但是必须完全遵守之前的方式 
    request.setCharacterEncoding("utf-8");
    request.getParameter("acc_no")
  b.使用编码过滤器   Spring 框架提供的 

# day07

1.什么拦截器 
  拦截器 就是对 之前过滤器的一个封装   功能和之前的过滤器类似    比如有  身份认证   登录检查 
    资源的加密访问    编码设置等功能。
 拦截器在Spring  中 需要实现  HandlerInterceptor 接口
2.HandlerInterceptor  接口介绍 
  preHandle    在HandlerMapping 之后  控制器之前    返回 布尔值类型   如果返回 true
  则代表继续后续逻辑调用     如果返回false  则请求被拦截。
  postHandle   控制器处理之后   视图处理器之前 
  afterCompletion   视图处理器之后  响应之前 
3.练习 
  写一个基于标注的Spring MVC 的登录   用户输入账号 和 密码 进行登录  如果账号是 abc
  密码是 123  则登录成功   登录成功之后 把账号放入 session  重定向到 main.jsp 。
  如果登录失败 则转发到  login.jsp  显示登录失败。  
4.拦截器的使用步骤 
  4.1 搭建一个基于标注的 spring  MVC  完成基本的登录功能  最起码有  toLogin.do
  4.2 编写一个拦截器类  实现HandlerInterceptor  接口   覆盖接口方法 
  4.3 在Spring  配置文件中配置拦截器 
           大体需要配置 拦截的路径   不拦截的路径    拦截器对象 
  4.4  测试功能 
5.写一个检查登录的拦截器逻辑   如果用户登录成功了  则可以跳转到 toMain.do 
如果没有登录成功  则 直接让用户去登录。 这个拦截器不能拦截 toLogin.do  和  login.do 请求。
登录成功不成功的判断标准是  session 中是否有 acc_no。

6.Spring 中的异常处理 
  6.1 全局异常处理 ---- 系统提供的异常处理器   SimpleMappingExceptionResolver
     指定对应的异常  可以跳转到 对应的页面 
  <!--  配置一个 系统提供的异常处理器  什么样对应什么样的页面 --> 
    <bean  id="exceptionResolver"  
        class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
        <property name="exceptionMappings">
            <props>
                <prop key="java.lang.Exception">error2</prop>
                <prop key="java.lang.RuntimeException">error</prop> 
            </props>
        </property>
    </bean>   
  6.2 全局异常处理  -------  自定义异常处理器 
    实现 HandlerExceptionResolver   并且在Spring 容器中创建自定义异常处理器对象
  @Controller
public class MyExceptionResolver implements HandlerExceptionResolver {

	@Override
	public ModelAndView resolveException(HttpServletRequest request,
			HttpServletResponse response, Object method,
			Exception e) {
		// 出现什么样的异常 就跳转到对应的页面 
		ModelAndView  mav  = new  ModelAndView();
		if(e  instanceof  RuntimeException) {
			 mav.setViewName("error");
		}else if(e instanceof  Exception) {
			 mav.setViewName("error2");
		}
		return mav;
	}

}
 6.3 局部异常处理 ----- 只处理具体的一个控制器抛出的异常 
 @ExceptionHandler
 public  String  方法名(Exception  e){
     return "错误页面的名字";
 }

 7.文件上传 
   7.1 编写一个文件上传的页面
   form 表单的method="post"    enctype="multipart/form-data"
   form 表单中要有 type="file"  的组件 
   7.2 编写一个控制器  用来获取文件数据 并将文件数据写入服务器的磁盘中 
   真正完成文件上传 需要依赖于文件解析器  ---- 注意文件解析器依赖于文件上传的jar包  还有这个解析器的
   id必须是  multipartResolver 

    # day08

1.Spring MVC 控制器中如何返回JSON
  1.1 搭建一个基于标注的 Spring  MVC
  1.2 设计一个控制器方法
  @RequestMapping("/请求路径")
  @ResponseBody
  返回值类型就是JSON转换前对应的类型     方法名(){
      return  类型对应的对象； 
  }
  @ReponseBody  需要依赖于 JSON转换包 

2.什么是rest
REST即表述性状态传递（英文：Representational State Transfer，简称REST）
它可以提高系统的可伸缩性   降低组件之间的耦合度   便于分布应用程序的设计和开发
具体做了两个方法的规范
   对URL的风格做了规范   把原来基于操作的设计  转换成了基于资源的设计 
   /addAccount.do  /deleteAccount.do  /updateAccount.do  /getAccount.do 
   /account/{id}
 对http协议的请求方式做了规范 
    get   查询       post  增加       put  更新        delete 删除 
    /account/delete    /account/change   /account/new
    
 3.什么是restful
  符合rest 设计风格和规范的应用程序设计 叫restful
 4.Spring MVC  对 rest的支持 
  4.1 建立基于标注的 Spring MVC
  4.2 对控制器方法做设计 
  @RequestMapping(value="/请求路径/{路径变量}",method=RequestMethod.请求方式枚举值)
  public  返回值类型   方法名(@PathVariable("路径变量名") 类型  变量){

  } 
  如果要返回JSON  则加 @ResponseBody    ----  依赖json转换包 
  4.3 由于rest风格的请求路径上  没有后缀   所有 DispatcherServlet  上 需要使用  /  
     所有的静态资源 都将被拦截处理   所以要放行静态资源 
  在spring 配置文件中 加 <mvc:default-servlet-handler />

 5.基于rest设计 根据id 删除银行账户 

 6.基于rest 设计的  增加银行账户 


 7.基于rest的修改  
  7.1 先把 post 请求 改成 put 请求 
  7.2 前端 
      需要把  post 请求改成 put
     需要加ajax的请求设置   contentType:"application/json"   
     当有上面的设置时  要求参数必须以 json字符串进行传递     把json对象 转换成 json字符串
     eval('(' + jsonStr +')')   JSON.parse(json字符串)
     JSON.stringify(json对象)   可以把json对象转换成json字符串
  7.3 服务端
   把post 请求改成  put
 还需要对象类型的参数上 加 @RequestBody  

# day09

1.什么是AOP
 Aspect  Orientied    Programming   面向切面编程(面向方面编程)
  它其实是对面向对象思想的一个扩展   
  它可以在不修改原有组件 源代码的情况下 给组件增强功能

 AOP 关注重点是切面   通过配置手段就可以将切面嵌入到合适的位置  这样 
 能实现组件的重复利用  解除切面和目标组件之间的耦合度   大大提供程序的灵活性和可扩展性

2. AOP 中涉及到的概念 
##Aspect       切面          切面是用来封装共通业务逻辑的   用切面类型创建的对象叫切面对象
JoinPoint    连接点      要被加入共通业务的位置   一般封装了方法的信息
##Pointcut     切点          切点是一堆连接点的集合  
Target       目标           要被切入共通业务逻辑的组件 
Proxy        代理           被增强之后的目标 叫代理  
##Advice       通知           通知是一种时机   目标方法调用开始之前   目标方法调用之后 
      目标方法调用前后    目标方法执行过程中一定执行    目标方法出现异常
   

切面   ------ 通知  ------- 切点    
3. AOP 编程的实现步骤 
    3.1 建立一个项目  导入jar包(ioc  aop)   拷贝配置文件到src下 
    建立一个服务类  里面定义两个方法     使用伪代码完成逻辑      使用Spring 容器创建
   组件  并通过容器获取组件 调用方法 
    3.2 定义一个切面类    共通业务逻辑是 在方法调用前打印 ****** 
    3.3 在Spring 容器中创建切面对象  并且通过配置手段 将切面嵌入到方法调用位置  测试 

4.写一个DAO 接口  里面定义三个方法 分别是    addAccount   deleteAccount
updateAccount   这些方法使用伪代码即可。 写一个DAO 接口的实现类  实现类的代码
也是伪代码即可  就是简单做一个输出。 在Spring 容器中创建DAO 接口的实现类对象 
并通过容器获取这个对象  调用上面的方法。 然后写一个切面类  里面定义一个方法  这个
方法可以打印系统时间  格式  yyyy-MM-dd hh:mm:ss@1970到现在的毫秒数   
然后通过AOP的手段 将系统时间打印嵌入到 上面的DAO 方法中。

5.切点表达式的写法 
  5.1 bean 限定表达式
  bean(符合条件的对象id)    如果符合对象的id  则对象对应的类型中对应 所有的方法都将切入共通逻辑 
        允许使用通配符  如 *Dao    既所有的以Dao 结尾的组件id 都将被匹配 
  5.2 类型限定表达式 
   within(符合条件的类型表达式)     这个表达式的最后一部分一定是类型
      
   within(com.xdl.dao.XdlBankAccountDAOOracleImp)   com.xdl.dao中的
      XdlBankAccountDAOOracleImp类型中对应的所有方法 都将被切入共通业务逻辑
   within(com.xdl.dao.*)   com.xdl.dao中的
              所有的类型中对应的所有方法 都将被切入共通业务逻辑   
   within(com.xdl..*)   com.xdl 包下的类型 以及 子包下类型 都将被切入共通业务逻辑
   5.3 方法限定表达式  
   execution(方法限定表达式)     方法限定表达式 包括  
          权限修饰     返回值类型   方法名(参数说明) throws 异常   
    其中  返回值类  方法名()  是必须的 
    
 6.Spring AOP 中五种通知类型 
  <aop:before    前置通知        在目标方法调用前调用 
  <aop:after     最终通知        在目标方法调用后 一定会调用
  <aop:after-returing   后置通知   在目标方法调用后 调用   目标方法出现异常就不调用 
  <aop:after-throwing   异常通知   在目标方法出现异常时调用 
  <aop:around      环绕通知      在目标方法调用前后 都会调用 
 7.基于标注的AOP
   7.1 建立一个项目 导入jar包(ioc aop)   拷贝配置文件到src下
   7.2 开启组件扫描  
   <context:component-scan   base-package="" />
   7.3 写一个 DAO 接口  复用上午的
   7.4 写一个DAO 的实现类 复用上午的 
   7.5 在DAO 实现类上加对应的标注   让其在容器中创建对象  测试 
   7.6 写一个切面组件   并在容器中创建对象 
   7.7 开启标注形式的  aop
   <aop:aspectj-autoproxy   proxy-target-class="true" />
   7.8 在切面类上加 把切面类 变成真正的切面的标注 @Aspect
   在切面方法上加对应的通知标注   然后在标注中写切点表达式 

  8.Spring 中的通知对应的标注 
  <aop:before    前置通知        在目标方法调用前调用      @Before
  <aop:after     最终通知        在目标方法调用后 一定会调用  @After
  <aop:after-returning   后置通知   在目标方法调用后 调用   目标方法出现异常就不调用 
      @AfterReturning
  <aop:after-throwing   异常通知   在目标方法出现异常时调用  @AfterThrowing
  <aop:around      环绕通知      在目标方法调用前后 都会调用   @Around  

  

  

   

   

 

 

 

 

 

 

 


​    
​    
​    
​    
​    
​    
​    
​    
​    
​    
​    

   

   
















​      
​      

 

 











  