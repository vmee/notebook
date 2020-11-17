# SpringFramework

## 主要模块
- 支持IoC和AOP的容器
- 支持JDBC和ORM的数据访问模块
- 支持声明式事务的模块
- 支持基于Servlet的MVC开发
- 支持基于Reactive的Web开发
- 以及集成JMS, JavaMail, JMX,缓存等其他模块

## IoC容器

Spring的核心就是提供了一个IoC容器, 它可以管理所有轻量级的JavaBean组件, 提供的底层服务包括组件的生命周期\配置和组装服务\AOP支持,以及建立 在AOP基础上的声明式事务服务等.

IoC全称Inversion of Control, 直译为控制反转.
IoC双称为依赖注入(DI: Dependency Injection), 它解决的最主要的问题:将组件的创建+配置与组件的使用相分离,并且由IoC容器负责管理组件的生命周期

在Spring的IoC容器中, 我们把所有的组件统称为JavaBean, 即配置一个组件就是配置一个Bean



控制反转
- 传统应用程序中,控制权在程序本身,程序的控制流程安全由开发者控制 
- 在IoC模式下,控制发生了反转,即从应用程序转移到了IoC容器,所有组件不再由应用程序自己创建和配置,而是由IoC容器负责,这样,应用程序只需要直接使用已经创建好的并且配置好的组件.

依赖注入方式
- 构造方法注入
- 属性注入

无侵入容器

Spring的IoC容器是一个高度可扩展的无侵入容器。所谓无侵入，是指应用程序的组件无需实现Spring的特定接口，或者说，组件根本不知道自己在Spring的容器中运行

这种无侵入设计的好处
- 应用程序组件既可以在Spring的IoC容器中运行,也可以自已编写代码自行组装配置
- 测试的时候并不依赖Spring容器, 可单独进入测试,大在提高了开发效率

## 注解

- @Component 定义一个Bean 写在类名上
- @Autowired 指定类型的bean注入到指定的字段中 写在字段上或构造方法中
- @Configuration 配置类
- @ComponentScan 让容器自动搜索当前类所在的包以及子包,把所有标注为@Commpent的Bean自动创建出来,并根据@Autowired进行装配
  
### @Component
- 我们把一个Bean标记为@Component后,它就会自动为我们创建一个单例（Singleton），即容器初始化时创建Bean，容器关闭前销毁Bean。在容器运行期间，我们调用getBean(Class)获取到的Bean总是同一个实例。
- 还有一种Bean，我们每次调用getBean(Class)，容器都返回一个新的实例，这种Bean称为Prototype（原型），它的生命周期显然和Singleton不同。声明一个Prototype的Bean时，需要添加一个额外的@Scope注解

配置原型Bean(Prototype Bean)
```java
@Component
@Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE) // @Scope("prototype")
public class MailSession {}
```

若想对Bean排序 可以使用@Order注解
```java
@Component
@Order(1)
public class EmailValidator implements Validator {
}
```

### @Autrowired
可选注入@Autowired(required = false)

当我们标记了一个@Autowired后，Spring如果没有找到对应类型的Bean，它会抛出NoSuchBeanDefinitionException异常
这个参数告诉Spring容器，如果找到一个类型为ZoneId的Bean，就注入，如果找不到，就忽略。
这种方式非常适合有定义就使用定义，没有就使用默认值的情况

```java
@Component
public class MailService {
    @Autowired(required = false)
    ZoneId zoneId = ZoneId.systemDefault();
    ...
}
```

### @SuppressWarnings
- @SuppressWarnings("unchecked") 抑制单类型警告
- @SuppressWarnings(value={"unchecked", "rawtypes"}) 抑制多类型警告
- @SuppressWarnings("all") 抑制所有警告

### 创建第三方Bean @Bean
在@Configuration类中编写一个Java方法创建并返回它，注意给方法标记一个@Bean注解
Spring对标记为@Bean的方法只调用一次，因此返回的Bean仍然是单例

```java
@Configuration
@ComponentScan
public class AppConfig {
    // 创建一个Bean:
    @Bean
    ZoneId createZoneId() {
        return ZoneId.of("Z");
    }
}
```

### 初始化和销毁 @PostConstruct @PreDestroy

有些时候，一个Bean在注入必要的依赖后，需要进行初始化（监听消息等）。在容器关闭时，有时候还需要清理资源（关闭连接池等）。

Spring容器会对上述Bean做如下初始化流程：

调用构造方法创建MailService实例；
根据@Autowired进行注入；
调用标记有@PostConstruct的init()方法进行初始化。
而销毁时，容器会首先调用标记有@PreDestroy的shutdown()方法。


### 使用别名

- @Bean("name") 指定别名
- @Bean+@Qualifier("name")指定别名



```java

@Configuration
@ComponentScan
public class AppConfig {
    @Bean
    @Primary // 指定为主要Bean
    @Qualifier("z") //指定别名
    ZoneId createZoneOfZ() {
        return ZoneId.of("Z");
    }

    @Bean
    @Qualifier("utc8") //指定别名
    ZoneId createZoneOfUTC8() {
        return ZoneId.of("UTC+08:00");
    }
}

```

使用

@Autowired(required = false)
@Qualifier("z") // 指定注入名称为"z"的ZoneId


### 使用factoryBean
```java

@Component
public class ZoneIdFactoryBean implements FactoryBean<ZoneId> {

    String zone = "Z";

    @Override
    public ZoneId getObject() throws Exception {
        return ZoneId.of(zone);
    }

    @Override
    public Class<?> getObjectType() {
        return ZoneId.class;
    }
}
```


## 使用resource

@Value("classpath:/logo.txt")

读取配置文件、资源文件等

```java

@Component
public class AppService {
    @Value("classpath:/logo.txt")
    private Resource resource;

    private String logo;

    @PostConstruct
    public void init() throws IOException {
        try (var reader = new BufferedReader(
                new InputStreamReader(resource.getInputStream(), StandardCharsets.UTF_8))) {
            this.logo = reader.lines().collect(Collectors.joining("\n"));
        }
    }
}

```

## 注入配置

@PropertySource("app.properties") // 表示读取classpath的app.properties
@Value("${app.zone:Z}") 注入

Spring容器看到@PropertySource("app.properties")注解后，自动读取这个配置文件，然后，我们使用@Value正常注入

```java
@Value("${app.zone:Z}")
String zoneId;
```

注意注入的字符串语法，它的格式如下：

"${app.zone}"表示读取key为app.zone的value，如果key不存在，启动将报错；
"${app.zone:Z}"表示读取key为app.zone的value，但如果key不存在，就使用默认值Z。

Spring容器可以通过@PropertySource自动读取配置，并以@Value("${key}")的形式注入；

可以通过${key:defaultValue}指定默认值；

以#{bean.property}形式注入时，Spring容器自动把指定Bean的指定属性值注入。

## 使用条件装配

- @Profile条件来决定是否创建某个Bean
- @Conditional决定是否创建某个Bean


### @Profile
profile
- native
- test
- production

```java
@Configuration
@ComponentScan
public class AppConfig {
    @Bean
    @Profile("!test")
    ZoneId createZoneId() {
        return ZoneId.systemDefault();
    }

    @Bean
    @Profile("test")
    //@Profile({ "test", "master" }) // 同时满足test和master
    ZoneId createZoneIdForTest() {
        return ZoneId.of("America/New_York");
    }
}
```

在运行程序时，加上JVM参数-Dspring.profiles.active=test就可以指定以test环境启动
-Dspring.profiles.active=test,master
可以表示test环境，并使用master分支代码

### @Conditional

```java
@Component
@Conditional(OnSmtpEnvCondition.class)
public class SmtpMailService implements MailService {
    ...
}
// 如果满足OnSmtpEnvCondition的条件，才会创建SmtpMailService这个Bean
public class OnSmtpEnvCondition implements Condition {
    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
        return "true".equalsIgnoreCase(System.getenv("smtp"));
    }
}

// Spring boot 
// 存在环境变量smtp，值为true
// @ConditionalOnProperty(name="app.smtp", havingValue="true")
// 当前classpath中存在类javax.mail.Transport，则创建MailService
// @ConditionalOnClass(name = "javax.mail.Transport")
// @ConditionalOnProperty(name = "app.storage", havingValue = "file", matchIfMissing = true)
// @ConditionalOnProperty(name = "app.storage", havingValue = "s3")

@Component
@ConditionalOnProperty(name = "app.storage", havingValue = "file", matchIfMissing = true)
public class FileUploader implements Uploader {
    ...
}

@Component
@ConditionalOnProperty(name = "app.storage", havingValue = "s3")
public class S3Uploader implements Uploader {
    ...
}

@Component
public class UserImageService {
    @Autowired
    Uploader uploader;
}
```


## AOP

本质就是一个动态代理，让我们把一些常用功能如权限检查、日志、事务等，从每个业务方法中剥离出来

在AOP编程中，我们经常会遇到下面的概念：

- Aspect：切面，即一个横跨多个核心逻辑的功能，或者称之为系统关注点；
- Joinpoint：连接点，即定义在应用程序流程的何处插入切面的执行；
- Pointcut：切入点，即一组连接点的集合；
- Advice：增强，指特定连接点上执行的动作；
- Introduction：引介，指为一个已有的Java对象动态地增加新的接口；
- Weaving：织入，指将切面整合到程序的执行流程中；
- Interceptor：拦截器，是一种实现增强的方式；
- Target Object：目标对象，即真正执行业务的核心逻辑对象；
- AOP Proxy：AOP代理，是客户端持有的增强后的对象引用。

> Spring通过CGLIB创建的代理类，不会初始化代理类自身继承的任何成员变量，包括final类型的成员变量！


## JDBC

Spring为了简化数据库访问，主要做了以下几点工作：

- 提供了简化的访问JDBC的模板类，不必手动释放资源；
- 提供了一个统一的DAO类以实现Data Access Object模式；
- 把SQLException封装为DataAccessException，这个异常是一个RuntimeException，并且让我们能区分SQL异常的原因，例如，DuplicateKeyException表示违反了一个唯一约束；
- 能方便地集成Hibernate、JPA和MyBatis这些数据库访问框架。