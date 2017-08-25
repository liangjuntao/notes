# 第一张 spring之旅 #
1. spring使命：简化java开发。
  
2. spring的四种策略。
 - pojo，最小侵入性
 - di，面向接口
 - 切面，惯例，进行声明式编程
 - 切面和模板减少样板式代码

3. 样板代码
 - 如jdbcTemplate 

4. spring容器
 - 负责bean的创建，装配，配置，管理bean的整个声明周期。
 - 从new到finalize（）

5. spring容器的类型
 - bean工厂。由`org.springframework.beans.factory.beanFactory` 接口定义，提供基本的di支持，
 - 应用上下文。由`org.springframework.context.ApplicationContext` 接口定义，基于beanFactory构建。

6. spring应用上下
 - `AnnotationConfigApplicationContext`
 - `AnnotationConfigWebApplicationContext`
 - `ClassPathXmlApplicationContext`：是在所有的类路径（包含JAR文件） 下查找 ac.xml文件
 - `FileSystemXmlapplicationcontext`：在指定的文件系统路径下查找ac.xml文件
 - `XmlWebApplicationContext`

7. spirng模块
 - 发布版本20个不同的模块
 - 6类功能

8. spring的6类功能
 - 核心容器
 - aop
 - 数据访问与集成
 - web与远程调用。自带了远程调用框架，http invoker，提供了暴露和使用rest api的支持。
 - Instrumentation，为jvm添加代理的功能。不懂。。没接触过。
 - 测试


# 第2章 装配Bean #
## spring配置的可选方案 ##
- 依赖注入的本质：创建应用对象之间协作关系的行为通常称为装配（wiring）
- 三种装配机制：
 1. xml中显式配置
 2. java中显式配置
 3. 隐式bean发现机制和自动装配
## 自动装配bean ##
- `@Component` ：这个简单
的注解表明该类会作为组件类，并告知Spring要为这个类创建bean。默认组件扫描是不启用的，需要显式配置。从而让他去找`@Component`注解的类，并创建为bean。`<context:component-scan>`
- `@ComponentScan`注解启用了组件扫描
- 测试组件中的注解
 `@ContextConfiguration`
- bean命名
 1. 默认情况下是类名字母小写。
 2. 使用Java依赖注入规范（Java Dependency Injection） 中所提供
的@`Named`，`@Inject`注解来为bean设置ID。*一般不建议使用。*
- 设置组件扫描
 1. `@ComponentScan（basePackages=""）`，基于包名
 2. `@ComponentScan(basePackageClasses="")`，基于类和接口
- 自动装配
 1. `@Autowired`，构造器、 Setter方法还是其他的方法（其他方法也是可以的，只要是作为一个参数传入，都可以使用这个注解）
 2. `@Autowired(required=false)`，当required属性设置为false时，spring会尝试自动装配，但如果没有匹配的bean时，spring会让这个bean处于未装配的状态。  

##@Autowiredh 和 @Resource区别：  

**@Autowire**  
默认按照类型装配。  
默认情况下它要求依赖对象必须存在如果允许为null，可以设置它required属性为false，如果我们想使用按照名称装配，可 以结合@Qualifier注解一起使用;  

**@Resource**  
@Resource默认按照名称装配。  
当找不到与名称匹配的bean才会按照类型装配，可以通过name属性指定，如果没有指定name属 性，当注解标注在字段上，即默认取字段的名称作为bean名称寻找依赖对象，当注解标注在属性的setter方法上，即默认取属性名作为bean名称寻找 依赖对象。  
注意：  
如果没有指定name属性，并且按照默认的名称仍然找不到依赖的对象时候，会回退到按照类型装配，但一旦指定了name属性，就只能按照名称 装配了。  



## 通过java代码装配bean ##
- 创建JavaConfig类</br>关键在于为其添加`@Configuration`注解， `@Configuration`注解表明这个类是一个配置类， 该类应该包含在Spring应用上下文中如何创建bean的细节。
- 声明简单的bean</br>默认情况下，bean id与带有@Bean注解的方法名是一样的可以通过`@Bean(name="")`，来指定一个名字。
```
@Bean
public CompactDisc sgtPepers(){
return new SgtPepeers();
}
```
- 通过javaConfig实现注入</br>注：章节2.3.3，这种方法很少见，不常用。

## 通过xml装配bean ##
- 构造器注入的两种选择
 1. `<constructor-arg>`元素
 2. 使用spring所引入的c-命名空间

- 构造器注入用的比价多，不做笔记了
- `c:cd-ref="bean_id"`
 1. c：命名空间
 2. cd ：构造器参数名
 3. -ref ：注入bean引用
 4. bena_id ：要注入bean的id</br>缺点：构造器参数名字变化了，在xml中的配置会报错。不推荐。
 5. `c:_0-ref="bean_id"`,采用 _0 ,去标注是第一个参数，索引。

- 将字面量注入到构造器中</br>
```
<bean id="" class="" 
c:_0="hi"
c:_1="tom"/>
```</br>
通过命名空间，和索引，注入字面量的值  
	
- 设置属性</br>
    p:xxx-ref="xxx" 
 1. p：命名空间前缀
 2. xxx：属性名，相当于`property`
 3. -ref:注入bean引用
 4. xxx：所注入bean的id

- 配置a采用JavaConf，配置b采用ac.xml，如何将b整合进a中呢？
 采用`@ImportResource(classpath:ac.xml)`注解 

# 第三章 高级装配 #

- spring profile  
这个作用是为了区别对待dev，test环境下，不同配置的使用策略。  
`spring.profiles.default`  
`spring.profiles.active`
这两个属性，可以指定开启那个profile。

- 条件化的bean： `@Conditional`    
  一个bean的创建，等到另外一个bena声明后，我才创建，可以使用这个条件化bean的装配。  
- 标识首选的bean：`@Primary`  
- 限定自动装配的bean：`@Qualifier` 
- 自定义的限定符，在bean声明上添加`@Qualifier`
 *详细使用章节3.3.2，先不细看。*

# 第四章 面向切面编程 #  
典型的安全，事务，管理是每个模块都需要的功能。这就是一个横切面。  

**术语**
- 通知 advice
什么时候执行？
 1. 前置：目标方法被调用前
 2. 后置：目标方法完成之后调用，不关心输出
 3. 返回：目标方法成功执行之后
 4. 异常：目标方法有异常时
 5. 环绕：在通知方法调用之前好之后执行自定义的行为 

- 连接点 join point
连接点是在应用执行过程中能够插入切面的一个点。  
这个点可以试调用方法时，抛出异常时，甚至是修改一个字段时。  
切面代码利用这个点插入到应用的正常流程中，添加新功能。  

- 切点 pointCut
新功能切入到哪里？

- 切面 aspect
切入的是什么功能

- 引入 introduction
- 织入 weaving
织入是把切面应用到目标对象并创建新的代理对象的过程。  
切面在指定的连接点被织入到目标对象中。  
在目标的声明周期中有多个点可以进行切入：
 1. 编译期
 2. 类加载期
 3. 运行期   

切点是连接点的子集。？ 

spring Aop构建在动态代理基础上。因为Spring运行时才创建代理对象， 所
以我们不需要特殊的编译器来织入Spring AOP的切面。
- spring只支持方法级别的连接点  
因为Spring基于动态代理， 所以Spring只支持方法连接点。  
例如AspectJ和JBoss， 除了方法切点， 它们还提供了字段和构造器接入点。 Spring缺少对字段连接点的支持， 无法
让我们创建细粒度的通知， 例如拦截对象字段的修改。 而且它不支持构造器连接点， 我们就无法在bean创建时应用通知。

# 第五章 构建spring web应用程序 #
模型:其实指的是数据原信息，而且仅仅是数据，不是经过html，xml包装过的表示。 
视图：返回给用户原始信息时不合理的，需要对用户友好的方式格式化，一般是html，所以信息要发送给一个view。


modelAndView：这个是控制器处理完请求之后，产生的数据和视图名。   
modelAndView返回个前段控制器之后，会基于视图名去找真正的视图。即此阿勇viewResolver来讲逻辑视图匹配为一个特定的视图实现。视图将用模型数据渲染输出，这个输出会通过响应传给客户端


## 搭建spring Mvc ##
- AbstractAnnotationConfigDispatcherServletInitializer剖析
 1. servlet3.0环境中，容器会在类路径中查找`javax.servlet.ServletContainerInitializer`接口的类，如果有的话，就用它来配置Servlet容器。
 2. spring提供了这个接口的实现SpringServletcontainerInitializeer,其中有协助这个实现的类去完成配置的任务，这个类是WebApplicationInitializer,也就是AbstractAnnotationConfigDispatcherServletInitializer，从而我们自己写的类继承AbstractAnnotationConfigDispatcherServletInitializer，容器就会拿这个子类来配置Servlet上下文。
- 其中我们自己定义的类，要实现三个方法
 1. `getServletMappings()`,这个方法将一个或多个路径映射到DipatcherServlet。”/“，就是默认的servlet。所有的请求都会给它来处理。
 2. `getRootconfClasses()`,此方法带有`@Configuration`注解的类，将会来配置ContextLoaderListener创建的应用上下文中的bean；
 3. `GetServletConfigClasses()`方法返回的带有`@Configuration`注解的
类将会用来定义DispatcherServlet应用上下文中的bean

- 说明：不采用web.xml配置，使用java将DispatcherServlet配置在Servlet容器中。  
 1. 以前是采用`<mvc:annotation-driven>`，现在采用`@EnableWebMvc`修饰一个配置类。
 2. 不配置视图解析器。spring默认采用BeanNameView-Resolver,根据beanId与视图名称匹配的bean，并且要实现view接口，以这样的方式解析视图。  
 3. 没有启用组件扫描。spring只能显示声明在配置类中的控制器。
 4. 默认会处理所有的请求，包括对静态资源的请求

- 几种常用的视图解析器
 1. InterResourceViewResolver，将视图解析为Web应用的内部资源（一般为JSP）
 2. ContentNegotiatingViewResolver，通过考虑客户端需要的内容类型来解析视图， 委托给另外一个能够产生对应内容类型的视图解析器
 3. JstlView，可以解析jstl格式化标签。
- 具体的步骤
 1. `@EnableWebMvc`, 开启springmvc组件
 2. `@ComponentScan`, 开启组件扫描
 3. `InterResourceViewResolver`,配置jsp视图解析器
 4. `DefaultServletHandlerConfigurer`，配置对静态资源的处理  

- 异常处理
	1. @ResponseStatus，将异常映射转换为http中特定的状态码
	2. @ExceptionHandler，可以处理某个类抛出来的所有异常

- 重定向中的数据如何实现跨请求
	1. 对于重定向来说， 模型并不能用来传递数据。 但是我们也有一些其他方案， 能够从发起重定向的方法传递数据给处理重定向方法。（使用URL模板以路径变量和/或查询参数的形式传递数据）
	2. 除了连接String的方式来构建重定向URL， Spring还提供了使用模板的方式来定义重定向URL。比如：` return "redirect:/spitter/{username}" ;`
	3. 通过flash属性发送数据。RedirectAttributes提供了一组addFlashAttribute()方法来添加flash属性。




# 第八章 spring web flow #
有时候， Web应用程序需要控制网络冲浪者的方向， 引导他们一步步地访问应用。 比较典型的例子就是电子商务站点的结账流程， 从购物车开
始， 应用程序会引导你依次经过派送详情、 账单信息以及最终的订单确认流程。
跳过，没需求。

# 第九章 Spring Security #



# 第11章 使用对象-关系映射持久化数据 #
- 更加复杂的特性
 1. 延迟加载 需要的时间获取数据
 2. 预先抓取 一次操作，全部获取，不用多次查询，和延迟加载相对。
 3. 级联 级联删除
 4. 分布式缓存 

## 在Spring中集成hiberate中session工厂 ##

- hibernate主要接口是：`org.hibernate.Session`  
这个接口实现了提供了基本的数据访问，查询，更新等功能。  
- 获取Seesion对象，借助于SessionFactory接口的实现类。这个接口作用主要是获取Session，打开，关闭和管理的作用。  
- spring提供三个Seesion工程bean：
 1. `org.springframework.orm.hibernate3.LocalSessionFactoryBean`  
 2. `org.springframework.orm.hibernate3.annotation.AnnotationSessionFactoryBean`  
 3. `org.springframework.orm.hibernate4.LocalSessionFactoryBean`
 4. 采用xml映射，使用LocalSessionFactoryBean。  
 5. 采用注解来定义持久化，使用AnnotationSessionFactoryBean。  

# 第十二章 使用nosql数据库 #

# 第十三章 缓存数据 #
# 第十六章 使用springMvc创建REST API #
## 基础知识 ##
RPC是面向服务的， 并关注于行为和动作； 
REST是面向资源的， 强调描述应用程序的事物和名词。  
- 单词拆开看  
 1. representational :表达性。对资源的表示，可以用http，xml，json等。  
 2. state：状态。使用rest时，关注的是资源的状态，而不是对资源采取的行为。  
 3. transfer:转移。转移资源数据。？？？？  
- 更简洁地讲， REST就是将资源的状态以最适合客户端或服务端的形式从服务器端转移到客户端（或者反过来） 。  

- spring是如何支持REST的？
 1. controller可以处理所有http方法
 2. `@PathVariable`注解，可以处理参数化的url
 3. 借助于modelAndView，资源可以很好的展现出来，xml，html等
 4. 使用ContentNegotiatingViewResolver来选择最适合客户端的表述；
 5. 借助@ResponseBody注解和各种HttpMethodConverter实现， 能够替换基于视图的渲染方式
 6. @RequestBody注解以及HttpMethodConverter实现可以将传入的HTTP数据转化为传入控制器处理方法的Java对象；
借助RestTemplate， Spring应用能够方便地使用REST资源。

- spring提供两种方法将资源 --> 表示
 1. 内容协商：Content negotiation，选择一个视图， 它能够将模型渲染为呈现给客户端的表述形式；  
 2. 消息转换器：Message conversion，通过一个消息转换器将控制器所返回的对象转换为呈现给客户端的表述形式。  

- 协商资源表述
`ContentNegotiatingViewResolver`是一个特殊的视图解析器，它考虑到了客户端所需要的内容类型
 1. 确定请求的媒体类型    
 2. 找到适合请求媒体类型的最佳视图。

- 请求媒体类型
 1. 首先查看url中的文件扩展名；如果扩展名是.json，那么返回的便是“application/json”;如果在url中，得不到任何媒体类型的话，看2；
 2. 查看Accpet头部信息，客户端会表明想要的MIME类型。如果没有Accept信息，看3；
 3. `ContentNegotiatingViewResolver`将会使用“/”作为默认的内容类型，这就意味着客户端必须要接收服务器发送的任何形式的表示。
 4. 一旦内容类型确定后。`ContentNegotiatingViewResolver`就会将将逻辑视图名解析为渲染模型的view。


- 使用HTTP信息转换器
 1. @ResponseBody注解会告知Spring， 我们要将返回的对象作为资源发送给客户端， 并将其转换为客户端可接受的表述形式。 更具体地
讲， DispatcherServlet将会考虑到请求中Accept头部信息， 并查找能够为客户端提供所需表述形式的消息转换器。</br>
例子：</br>
假设客户端的Accept头部信息表明它接受`“application/json”`， 并且Jackson JSON库位于应用的类路径下，
那么将会选择MappingJacksonHttpMessage-Converter或MappingJackson2HttpMessageConverter（这取决于类路径下是哪个版本的
Jackson）。
消息转换器会将控制器返回的Spittle列表转换为JSON文档， 并将其写入到响应体中。


# 第二十一章 springboot #
