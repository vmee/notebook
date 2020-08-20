# SPI机制

## 它是什么

SPI全称Service Provider Interface，是Java提供的一套用来被第三方实现或者扩展的API，它可以用来启用框架扩展和替换组件。

[Java SPI机制](https://upload-images.jianshu.io/upload_images/5618238-5d8948367cb9b18e.png?imageMogr2/auto-orient/strip|imageView2/2/w/848/format/webp)

Java SPI 实际上是“基于接口的编程＋策略模式＋配置文件”组合实现的动态加载机制。

Java SPI就是提供这样的一个机制：为某个接口寻找服务实现的机制。有点类似IOC的思想，就是将装配的控制权移到程序之外，在模块化设计中这个机制尤其重要。所以SPI的核心思想就是解耦。

## 适用场景

调用者根据实际使用需要，启用、扩展、或者替换框架的实现策略

-- JDBC -- SLF4J -- Spring -- Dubbo

## 使用

要使用Java SPI，需要遵循如下约定：

1、当服务提供者提供了接口的一种具体实现后，在jar包的META-INF/services目录下创建一个以“接口全限定名”为命名的文件，内容为实现类的全限定名； 2、接口实现类所在的jar包放在主程序的classpath中； 3、主程序通过java.util.ServiceLoder动态装载实现模块，它通过扫描META-INF/services目录下的配置文件找到实现类的全限定名，把类加载到JVM； 4、SPI的实现类必须携带一个不带参数的构造方法；

## 原理

## 学习资料

[高级开发必须理解的Java中SPI机制](https://www.jianshu.com/p/46b42f7f593c)

