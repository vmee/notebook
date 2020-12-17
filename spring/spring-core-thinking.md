# Spring核心编程思想

## 核心特性(core)
- IoC容器(IoC Container)
- Spring事件(Events) 基于Java 
- 资源管理(Resources) 基于Java 
- 国际化(i18n) 基于java api
- 校验(Validation) spring实现
- 数据绑定(Data Binding) spring实现
- 类型转换(Type Conversion) spring特性
- Spring表达式(Spring Express Language) 同jsp
- 面向切片编程(AOP) spring

## 数据存储(Data Access)
- JDBC
- 事务抽象 (Transaction)
- Dao支持 (Dao Support)
- O/R映射 (O/R Mapping)
- XML编列 (XML Marshalling)

jdbc的API实现
- MyBatis
- Hibernete
- jdbcTemplate (spring)

事务抽象
来源EJB,Spring只是在EJB的基础上面做一些简化工作

DAO支持
Spring一个很大的封装,帮助我们简化DAO的写法或一个实现方式,如常见的SQLException

O/R映射
- JPA
- Hibernet
它俩其实是一个东西

XML编列
JAXB:JAva Api for Xml Binding


## web技术(Web)

- Web Servlet技术栈
  - Spring MVC
  - WebSocket
  - SockJS

- Web Reactive 技术栈
  - Spring WebFlux
  - WebClient
  - WebSocket

Reative是Spring5引入的

Spring MVC 和 WebFlux在注解上是一模一样的, 只是底层实现发生了变化
Spring MVC是需要Servlet引擎进行支撑的, Recative通常默认情况下是Netty的web server
当然reactive也可以运用Spring MVC的引擎来进行实现,也就是说它也可以运用到Servlet引擎来进行实现

WebClient把过去的HttpClient的同步执行变成异步回调的方式

## 技术整合 (Integration)

- 远程调用 Remoting
- Java消息服务 JMS
- Java 连接架构 JCA
- Java管理扩展 JMX
- Java 邮件客户端 Email
- 本地任务 Tasks
- 本地调度 Scheduling
- 缓存抽象 Caching
- Spring 测试 Testing

这一部分的内容没有具体的API或具体的规范标准来统一整合,

Remoting

- RMI JAVA标准的远程调用方案
- Hessian 社区开源方案 Dubbo基于Hessian协议
- Thrift
Spring在这层做了统一的封装,通常来说远程调用是采用的同步的模式


Java消息服务 JMS:Java Message Service

异步调用模型,通过传统的JMS规范实现,比如说ActiveMQ这么一个规范, 不包括RocketMQ或kafka,它们是另外的规范

Java连接架构 JCA

JavaEE的架构 主要是统一一些资源连接,几乎不用

Java的管理扩展 JMX:Java Management Extensions

用于Java的一个管理, CPU利用率,磁盘利用率,它是一个复杂的规范
包括四大模型
- 标准Bean
- 动态Bean
  - 标准动态Bean
  - 模型Bean
  - 开放Bean
这个在Spring里非常简单,从Spring framework1.2引入了一个注解 叫@ManagedResource, 它能帮助我们简化实现

Java邮件客户端 Email
- 有标准客户端
- Spring客户端

本地任务 Tasks 本地调度 Scheduling
本地任务与本地调度类型 都是用java的多线程实现
可以使用Java的JUC框架现实分布式的调度, 可支持一些调度表达式

缓存抽象 Caching

通过注释的方式, 抽象掉一些基本的含义,帮助你实现缓存透明化

Sping测试 Tesing
- 模拟对象 Mock Objects
- TestContext 框架 TestContext Framework
- Spring MVC测试 Spring MVC Test
- Web 测试客户端 WebTestClient

模拟对象
Mock可以动态生成对象,完成单元测试

TestContext测试框架
是对Spring集成测试过程的整合
如对数据库的连接的远程方法调用,这时你要把Spring应用上下文启动,需要借助TestContext进行整合

Spring MVC测试是服务端测试
Web 测试客户端是客户端测试

## Spring模块化设计:Spring功能特性如何在不同的模块中组织

| 模块 | 说明 | |
| -- | -- | --|
| spring-aop | 面向切片编程 | |
| spring-aspects | Spring提供的对AspectJ框架的整合 | |
| spring-beans | Spring IOC的基础实现，包含访问配置文件、创建和管理bean等| spring-beans 和 spring-context是Spring核心的一个重要实现 都需要spring-core来支持 | 
| spring-context | 在基础IOC功能上提供扩展服务，此外还提供许多企业级服务的支持，有邮件服务、任务调度、JNDI定位，EJB集成、远程访问、缓存以及多种视图层框架的支持 | |
| spring-Context-support | Spring context的扩展支持，用于MVC方面 ||
| spring-core | Spring的核心工具包 包括java语法特性支持等 | | 
| spring-expression | Spring的表达式语言 | spring3引入 类似于jSP的EL语言| 
| spring-instrument | java装配 是对java的agent的支持 | spring2引入 | 
| spring-jcl | 一套新型的日志框架,帮助spring统一日志管理 | spring5引入 Comming logging统一了java的logging,就是java的日志和Log4j日志, java logging之后双出一个Logback , Logback是一个新型的日志框架,它用的是SLF4j,SLF4j就相当于又又把java logging的Log4j和Logback进行统一, Spring为了解决这个情况| 
| spring-jdbc | spring对JDBC的一个整合 | | 
| spring-jms | java的一个消息服务,Java Message Service的缩写| ActiveMQ或其他消息中间件| 
| spring-messaging | spring想统一一下消息服务的一个实现 | 包括kakfa RocketMQ RabbitMQ | 
| spring-orm | 整合第三方的orm实现，如hibernate，ibatis，jdo以及spring 的jpa实现 | | 
| spring-oxm | 编列 就是marshal和unmarshal, XML里面的序列化与反序列化 | | 
| spring-test | 对JUNIT等测试框架的简单封装 | 包括Mock test, TestContext, MVC测试,WebClient测试|
| spring-tx | 事务抽象 为JDBC、Hibernate、JDO、JPA等提供的一致的声明式和编程式事务管理 | 它是基本上借鉴JDBC的一部分事务实现,以及Java EE尤其是EJB的一个事务实现,做了一个统一的封装 |
| spring-web |||
| spring-webflux |||
| spring-webmvc |||
| spring-websocket |||




Java语法变化

|语法 | Java版本 | spring版本|  代表实现|
| -- | -- | -- | -- |
| 枚举 | 5| 1.2+| Propagation |
| 泛型 | 5| 3.0+ |  ApplicationListener |
| 注解 | 5| 1.2+ |  @Transactional |
| for-each语法 | 5| 3.0+ |  AbstractApplicationContext |
| 封箱/解箱(AutoBoxing) |5 | 3.0+|
| @Override接口 | 6 | 4.0+ | | 
| Diamond语法 | 7 | 5.0+ | DefaultListableBeanFactory |
| 多Catch | 7| | 
| try with resources | 7 | 5.0+ | ResourceBundleMessageSource | 
| Lambda语法 | 8| 5.0+| PropertyEditorRegistrySupport | 
| 可重复注解 |8 | | 
| 类型注解|8 | | 
| 模块化 | 9 | |
| 接口私有化 |8| |
| 局部变量类型推断 | 10 ||



## JDK API实践

java5以前
- 反射 reflection   --MethodMatcher
- Java Beans --CachedIntorspectionResults
- 动态代理 Dynamic Proxy  Proxy或InvocationHandler API --JdkDynamicAopProxy

java5
- 并发框架(J.U.C) --ThreadPoolTaskScheduler
- 格式化(Formatter)  --DateFormatter
- Java管理扩展 JMX --@ManagedResource
- Instrumentation -- InstrumentationSavingAgent
- XML处理 DOM,SAX,XPath,XSTL  --XmlBeanDefinitinReader

java6
- JDBC4.0 jsr221 --JdbcTemplate
- JAXB JSR222  --Jaxb2Marshaller
- 可插拔注释处理API JSR269 --@Indexed
- Common Annotations JSR250 --CommonAnnotaionBeanPostProcessor
- Java Compiler API JSR199  --TestCompiler(单元测试)
- Scripting in jvm JSR 223 --StandardScriptFactory

java7
- NIO 2 JSR 203 --PathResource
- Fork/Join框架 JSR166  --ForkJoinPoolFactoryBean
- invokedynamic 字节码 JSR202

java8
- Stream API JSR335 --StreamConverter
- CompletableFuture J.U.C --CompletableToListenableFutureAdapter
- Annotation on java Types JSR308
- Date and Time Api JSR 310  --DatetimeContext
- 可重复Annotations JSR337 --@PropertySources
- Javascript 运行时 JSR223

java9
- Reactive Stream Flow API J.U.C
- Process API Updates JEP102
- Variable Handles JEP 193
- Method Handles JEP277
- Spin-Wait Hints 自旋锁 JEP285
- Stack-Walking API JEP259


## Spring 对 Java EE API 整合

java EE Web技术相关
| JSR规范 | Spring支持版本 | 代表实现 |
| -- | -- | -- |
| Servlet + JSP(JSR 035) |  1.0+|  DispatcherServlet | 
| JSTL(JSR 052)| 1.0+ | JstlView | 
| JavaServer Faces (JSR127) | 1.1+  | FacesContextUtils | 
| Portlet(JSR 168) | 2.5+ | DispatcherPortlet | 
| SOAP(JSR 068) | 2.5+ | SoapFaultException | 
| WebServices(JSR109) | 2.5+ | CommonAnnotationBeanPostProcessor  | 
| WebSocket(JSR356) | 4.0+ | WebSocketHandler | 


Java EE数据存储相关
| JSR规范 | Spring支持版本 | 代表实现 |
| -- | -- | -- |
| JDO(JSR 12)| 1.0-4.2 | JdoTemplate |
| JTA(JSR 907) | 1.0+ | JTaTransactionManger |
| JPA(EJJB 3.0 JSR 220的成员 )| 2.0+ | JpaTranSactionManager | 
| Java Caching API (JSR 107) | 3.2+ | JCacheCache |

Java EE Bean技术相关
| JSR规范 | Spring支持版本 | 代表实现 |
| -- | -- | -- |
| JMS(JSR914) | 1.1+ | JmsTemplate |
| EJB2.0 JSR19 | 1.0+ | AbstractStatefulSessionBean |
| Dependency Injection For Java JSR330 | 2.5+ | AutowiredAnnotationBeanPostProcessor |
| Bean Validation JSR303 | 3.0+ | LocalValidatorFacdtoryBean |

- 有状态Bean stateBean
- 无状态Bean stateLessBean

资源相关
- JRS官方网址:https://jcp.org/
- 小马哥JSR收藏: https://github.com/mercyblitz/jsr
- spring的官方文档根路径: https://docs.spring.io/spring/docs

## Spring对编程模型

- 面向对象编程
- 面向切面编程
- 面向元编程
- 函数驱动
- 模块驱动

面向对象编程

契约接口 
- Aware接口 接口回调
- BeanPostProcessor

设计模式
- 观察者模式 ApplicationEvent
- 组合模式
- 模板模式

继承


## Srping的核心价值

生态系统
- Spring Boot
- Spring Cloud
- Spring Security
- Spring Data
- 其他

API抽象设计
- AOP抽象
- 事务抽象
- Environment抽象
- 生命周期

编程模型
- 面向对象编程
  - 契约接口
- 面向切面编程
  - 动态代理
  - 字节码提升
- 面向元编程
  - 配置元信息
  - 注解
- 面向模块编程
  - Maven Artifacts
  - OSGI Bundles
  - Java9 Antomatic Module
  - Spring @Enable*注解
- 面向函数编程
  - Lambda
  - Reactive
- 设计思想
 - Object-Oriented Progamming OOP
 - IoC/DI
 - Domain-Driven Development DDD
 - Event-Driven Programming EDP
 - Test-Driven Development TDD
 - Functional Programming FP
- 设计模式
 - 专属模式
   - 前缀模式
     - Enable模式
     - Configurable模式
   - 后缀模式
     - 处理器模式
       - Processor
       - Resolver
       - Handler
     - 意识模式
       - Aware
     - 配置器模式
       - Congiuror
     - 选择器模式
       - springframework Context annotation ImportSelector
 - 传统GoF23
- 用户基础
 - Spring用户 
   - Spring Framework
   - Spring Boot
   - Spring Cloud
 - 传统用户
   - JAVA SE
   - JAVA EE

## IoC
- 实现策略
- 职责
- 实现
- 传统容器实现
- 轻量级IoC容器
- 依赖查找 VS 依赖注入
- 构造器注入 VS Setter注入


> 好莱坞原则
> 控制反转

### IoC实现策略
1. 使用service locator pattern 服务定位模式, 通常通过JNDI这种技术获取java EE组件或EJB组件
2. 通过依赖注入 构造器注入 参数注入 Setter注入 接口注入
3. 上下文的依赖查询 如BeanContext
4. 模板方法的设计模式 JDBC Template
5. 策略模式 

两种重要的实现策略
- 依赖查询
- 依赖注入

### IoC容器职责

- 通用职责
- 依赖处理
  - 依赖查找
  - 依赖注入
- 生命周期管理
  - 窗口
  - 托管的资源 
- 配置
  - 容器
  - 外部化配置
  - 托管的资源
 
### 传统IoC容器的实现
- Java Beans作为IoC容器
- 特性
  - 依赖查找
  - 生命周期管理
  - 配置元信息
  - 事件
  - 自定义
  - 资源管理
  - 持久化
- 规范
  - JavaBeans 
  - BeanContext

> 什么是Java Bean: 可以认为是一个简单的POJO（Plain Ordinary Java Object）简单的Java对象