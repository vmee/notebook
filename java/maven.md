# maven

- groupId 公司或组织名
- artifactId 项目名
- version 版本

依赖关系

| scope| 说明| 示例 |
| -- | -- | -- |
| compile| 编译时需要用到该jar包（默认）| commons-logging |
| test| 编译Test时需要用到该jar包| junit |
| runtime| 编译时不需要，但运行时需要用到| mysql |
| provided| 编译时需要用到，但运行时由JDK或某个服务器提供| servlet-api |


> 只有以-SNAPSHOT结尾的版本号会被Maven视为开发版本，开发版本每次都会重复下载，这种SNAPSHOT版本只能用于内部私有的Maven repo，公开发布的版本不允许出现SNAPSHOT


命令行编译

pom.xml所在目录 target目录下获得编译后自动打包的jar
```sh
mvn clean package
```

构建流程

生命周期 lifecycle
- default 
- clean 

阶段phase
- validate
- initialize
- generate-sources
- process-sources
- generate-resources
- process-resources
- compile
- process-classes
- generate-test-sources
- process-test-sources
- generate-test-resources
- process-test-resources
- test-compile
- process-test-classes
- test
- prepare-package
- package
- pre-integration-test
- integration-test
- post-integration-test
- verify
- install
- deploy

运行mvn命令 后面的参数是phase，Maven自动根据生命周期运行到指定的phase

常用操作:
- mvn clean：清理所有生成的class和jar；
- mvn clean compile：先清理，再执行到compile；
- mvn clean test：先清理，再执行到test，因为执行test前必须执行compile，所以这里不必指定compile；
- mvn clean package：先清理，再执行到package

Goal

执行一个phase又会触发一个或多个goal
goal的命名总是abc:xyz这种形式。

| 执行的Phase  | 对应执行的Goal |
| compile  | compiler:compile |
| test  | compiler:testCompile surefire:test |

类比
- lifecycle相当于Java的package，它包含一个或多个phase；
- phase相当于Java的class，它包含一个或多个goal；
- goal相当于class的method，它其实才是真正干活的

插件

使用Maven，实际上就是配置好需要使用的插件，然后通过phase调用它们

Maven已经内置了一些常用的标准插件：

| 插件名称 | 对应执行的phase |
| clean | clean |
| compiler | compile |
| surefire | test |
| jar | package |

模块管理

在软件开发中，把一个大项目分拆为多个模块是降低软件复杂度的有效方法


mvnw

Maven Wrapper就是给一个项目提供一个独立的，指定版本的Maven给它使用
Maven Wrapper的另一个作用是把项目的mvnw、mvnw.cmd和.mvn提交到版本库中，可以使所有开发人员使用统一的Maven版本


## 修改配置后,查看配置是否有问题

> mvn help:effective-settings