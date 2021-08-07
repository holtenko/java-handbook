# Spring面试题

## \# Spring IoC 是什么？

2. 控制反转即IoC (Inversion of Control)，它把传统上由程序代码直接操控的对象的调用权交给容器，通过容器来实现对象组件的装配和管理。所谓的“控制反转”概念就是对组件对象控制权的转移，从程序代码本身转移到了外部容器。

   Spring IOC 负责创建对象，管理对象（通过依赖注入（DI），装配对象，配置对象，并且管理这些对象的整个生命周期。

## \# Spring IoC 的实现原理是什么？

4. 工厂模式加反射机制

## \# Spring IoC 的初始化流程？

6. 1. 定位：定位配置文件的位置或扫描相关的注解。
   2. 加载：将配置信息加载到内存中。
   3. 注册：根据载入的配置信息将对象初始化到IOC容器中

## \# Spring AOP 的概念？

8. 1. 切面（Aspect）：是对横切逻辑的抽象与封装，一个切面由通知和切点两部分组成。在实际应用中，切面通常用一个类来表示，称其为切面类。
   2. 通知（Advice）：是横切逻辑的具体实现。在实际应用中，通知被定义成切面类中的一个方法，该方法就是用来放横切代码的地方。通知的分类：以目标方法为参照点，根据切入方位的不同，可分为前置通知（Before）、后置通知（AfterReturning）、异常通知（AfterThrowing）、最终通知（After）与环绕通知（Around）5种。
   3. 切点（Pointcut）：用于说明将通知织入到哪个方法上，它是由切点表达式来定义的。
   4. 目标对象（Target）：是指那些即将织入切面的对象。这些对象中已经只剩下干干净净的核心业务逻辑的代码了，所有的横切逻辑的代码都等待 AOP 框架的织入。
   5. 代理对象（Proxy）：是指将切面应用到目标对象之后由 AOP 框架创建的对象。可以简单地理解为，代理对象的功能等于目标对象的核心业务逻辑功能加上横切业务功能。
   6. 织入（Weaving）：是指将切面应用到目标对象从而创建一个新的代理对象的过程。

## \# Spring AOP 的实现原理？

10. 基于动态代理技术来实现。动态代理基于反射技术，允许程序在运行期间动态生成代理类与代理对象。

    Spring 用代理类包裹切面，把他们织入到Spring管理的bean中。也就是说代理类伪装成目标类，它会截取对目标类中方法的调用，让调用者对目标类的调用都先变成调用伪装类，伪装类中就先执行了切面，再把调用转发给真正的目标bean。

## \# Spring 中创建bean的过程？

12. 1. Spring对bean进行实例化；
    2. Spring将值和bean的引用注入到bean对应的属性中；
    3. 执行一些Spring前处理/后处理方法；
    4. bean已经准备就绪，可以被应用程序使用了，它们将一直驻留在应用上下文中，直到该应用上下文被销毁；
    5. 销毁时，执行一些指定的方法；

## \# bean的生命周期？

14. 1. Spring启动，查找并加载需要被Spring管理的bean，进行Bean的实例化
    2. Bean实例化后对将Bean的引入和值注入到Bean的属性中
    3. 如果Bean实现了BeanNameAware接口的话，Spring将Bean的Id传递给setBeanName()方法
    4. 如果Bean实现了BeanFactoryAware接口的话，Spring将调用setBeanFactory()方法，将BeanFactory容器实例传入
    5. 如果Bean实现了ApplicationContextAware接口的话，Spring将调用Bean的setApplicationContext()方法，将bean所在应用上下文引用传入进来。
    6. 如果Bean实现了BeanPostProcessor接口，Spring就将调用他们的postProcessBeforeInitialization()方法。
    7. 如果Bean 实现了InitializingBean接口，Spring将调用他们的afterPropertiesSet()方法。类似的，如果bean使用init-method声明了初始化方法，该方法也会被调用
    8. 如果Bean 实现了BeanPostProcessor接口，Spring就将调用他们的postProcessAfterInitialization()方法。
    9. 此时，Bean已经准备就绪，可以被应用程序使用了。他们将一直驻留在应用上下文中，直到应用上下文被销毁。
    10. 如果bean实现了DisposableBean接口，Spring将调用它的destory()接口方法，同样，如果bean使用了destory-method 声明销毁方法，该方法也会被调用。

## \# Spring Boot的加载机制？

    ![springboot_load](../assets/springboot_load.png)

    [SpringBoot启动流程解析](evernote:///view/21328630/s47/a9df826a-751f-4f36-bbb5-03fa882887fa/a9df826a-751f-4f36-bbb5-03fa882887fa/)

    1. SpringApplication的实例化；
    2. 1. 判断是否为webEnvironment；
       2. 实例化并加载所有可以加载的ApplicationContextInitializer；
       3. 实例化并加载所有可以加载的监听器ApplicationListener；
    3. 调用该实例运行run方法；
    4. 1. 通过SpringFactoriesLoader获取所有的SpringApplicationRunListeners；
       2. 加载环境变量；
       3. 创建ApplicationContext；
       4. 刷新应用上下文ApplicationContext；
    5. 自动化配置；

## \# @SpringBootApplication的功能

17. 1. @EnableAutoConfiguration：SpringBoot根据应用所声明的依赖来对Spring框架进行自动配置
    2. @SpringBootConfiguration(内部为@Configuration)：被标注的类等于在spring的XML配置文件中(applicationContext.xml)，装配所有bean事务，提供了一个spring的上下文环境
    3. @ComponentScan：组件扫描，可自动发现和装配Bean，默认扫描SpringApplication的run方法里的Booter.class所在的包路径下文件，所以最好将该启动类放到根包路径下

## \# Spring MVC 的流程？

19. 1. 用户发送请求至前端控制器DispatcherServlet
    2. DispatcherServlet收到请求调用处理器映射器HandlerMapping。
    3. 处理器映射器根据请求url找到具体的处理器，生成处理器执行链HandlerExecutionChain(包括处理器对象Handler和处理器拦截器Interceptor)一并返回给DispatcherServlet。
    4. 1. 根据url在map里直接找到处理器执行链；
       2. 根据url找到匹配的pattern然后找到处理器执行链；
    5. DispatcherServlet根据处理器Handler获取处理器适配器HandlerAdapter执行HandlerAdapter处理一系列的操作，如：参数封装，数据格式转换，数据验证等操作
    6. 执行处理器Handler(Controller，也叫页面控制器)。
    7. Handler执行完成返回ModelAndView
    8. HandlerAdapter将Handler执行结果ModelAndView返回到DispatcherServlet
    9. DispatcherServlet将ModelAndView传给ViewReslover视图解析器
    10. ViewReslover解析后返回具体View
    11. DispatcherServlet对View进行渲染视图（即将模型数据model填充至视图中）。
    12. DispatcherServlet响应用户。

## \# Spring、Spring MVC、Spring Boot, Spring Cloud相关组件

21. 1. Spring：Spring是一个开源容器框架，可以接管web层，业务层，dao层，持久层的组件，并且可以配置各种bean,和维护bean与bean之间的关系。其核心就是控制反转(IOC),和面向切面(AOP),简单的说就是一个分层的轻量级开源框架。
    2. Spring MVC：SpringMVC是一种web层mvc框架，用于替代servlet（处理|响应请求，获取表单参数，表单校验等。SpringMVC是一个MVC的开源框架，SpringMVC=struts2+spring，springMVC就相当于是Struts2加上Spring的整合。
    3. Spring Boot：一个微服务框架，延续了spring框架的核心思想IOC和AOP，简化了应用的开发和部署。Spring Boot是为了简化Spring应用的创建、运行、调试、部署等而出现的，使用它可以做到专注于Spring应用的开发，而无需过多关注XML的配置。提供了一堆依赖打包，并已经按照使用习惯解决了依赖问题--->习惯大于约定。
    4. Spring Cloud：一系列框架的有序集合。它利用Spring Boot的开发便利性巧妙地简化了分布式系统基础设施的开发，如服务发现注册、配置中心、智能路由、消息总线、负载均衡、断路器、数据监控等，都可以用Spring Boot的开发风格做到一键启动和部署。Spring Cloud并没有重复制造轮子，它只是将各家公司开发的比较成熟、经得起实际考验的服务框架组合起来，通过Spring Boot风格进行再封装屏蔽掉了复杂的配置和实现原理，最终给开发者留出了一套简单易懂、易部署和易维护的分布式系统开发工具包。Spring Cloud包含了多个子项目（针对分布式系统中涉及的多个不同开源产品），比如：Spring Cloud Config、Spring Cloud Netflix、Spring Cloud CloudFoundry、Spring Cloud AWS、Spring Cloud Security、Spring Cloud Commons、Spring Cloud Zookeeper、Spring Cloud CLI等项目。