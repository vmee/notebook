# java.util.zip.ZipException: zip END header not found

第一次运行quarkus的started项目
在执行
```sh
$ ./mvnw compile quarkus:dev
```

报以下错误




```sh

Exception in thread "main" java.util.zip.ZipException: zip file is empty
        at java.base/java.util.zip.ZipFile$Source.zerror(ZipFile.java:1585)
        at java.base/java.util.zip.ZipFile$Source.findEND(ZipFile.java:1439)
        at java.base/java.util.zip.ZipFile$Source.initCEN(ZipFile.java:1448)
        at java.base/java.util.zip.ZipFile$Source.<init>(ZipFile.java:1249)
        at java.base/java.util.zip.ZipFile$Source.get(ZipFile.java:1211)
        at java.base/java.util.zip.ZipFile$CleanableResource.<init>(ZipFile.java:701)
        at java.base/java.util.zip.ZipFile.<init>(ZipFile.java:240)
        at java.base/java.util.zip.ZipFile.<init>(ZipFile.java:171)
        at java.base/java.util.zip.ZipFile.<init>(ZipFile.java:185)
        at org.apache.maven.wrapper.Installer.unzip(Installer.java:169)
        at org.apache.maven.wrapper.Installer.createDist(Installer.java:86)
        at org.apache.maven.wrapper.WrapperExecutor.execute(WrapperExecutor.java:121)
        at org.apache.maven.wrapper.MavenWrapperMain.main(MavenWrapperMain.java:61)

```

这是为什么呢? 

我们先看mvnw是什么

> Maven Wrapper就是给一个项目提供一个独立的，指定版本的Maven给它使用
> Maven Wrapper的另一个作用是把项目的mvnw、mvnw.cmd和.mvn提交到版本库中，可以使所有开发人员使用统一的Maven版本


那我们看下项目下./mvnw用的是什么版本

在.mvn/wrapper/maven-wrapper.properties文件里
```
distributionUrl=https://repo.maven.apache.org/maven2/org/apache/maven/apache-maven/3.6.3/apache-maven-3.6.3-bin.zip
wrapperUrl=https://repo.maven.apache.org/maven2/io/takari/maven-wrapper/0.5.6/maven-wrapper-0.5.6.jar
```
这里写的是3.6.3的版本

看下我们的本地的默认mvn版本
```sh
$ mvn -v
Apache Maven 3.6.1 (d66c9c0b3152b2e69ee9bac180bb8fcc8e6af555; 2019-04-05T03:00:29+08:00)
Maven home: /usr/local/Cellar/maven/3.6.1/libexec
Java version: 15.0.1, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-15.0.1.jdk/Contents/Home
Default locale: en_CN, platform encoding: UTF-8
OS name: "mac os x", version: "10.13.6", arch: "x86_64", family: "mac"

```

这里是3.6.1版本, 所以要运行./mvnw 就需要下载3.6.3的版本,那我们看下~/.m2/wrapper/dists/apache-maven-3.6.3-bin/是否有:
```
-rw-r--r--  1 xxx  xxx     0B Nov 17 10:44 apache-maven-3.6.3-bin.zip
```
我这里是个0字节的文件


但若是下面的情况,下载部分或未下载完成时,就会报"zip END header not found"错误

```
-rw-r--r--  1 xxx  xxx     0B Nov 17 10:44 apache-maven-3.6.3-bin.zip.part
```
```sh

Exception in thread "main" java.util.zip.ZipException: zip END header not found
        at java.base/java.util.zip.ZipFile$Source.zerror(ZipFile.java:1585)
        at java.base/java.util.zip.ZipFile$Source.findEND(ZipFile.java:1439)
        at java.base/java.util.zip.ZipFile$Source.initCEN(ZipFile.java:1448)
        at java.base/java.util.zip.ZipFile$Source.<init>(ZipFile.java:1249)
        at java.base/java.util.zip.ZipFile$Source.get(ZipFile.java:1211)
        at java.base/java.util.zip.ZipFile$CleanableResource.<init>(ZipFile.java:701)
        at java.base/java.util.zip.ZipFile.<init>(ZipFile.java:240)
        at java.base/java.util.zip.ZipFile.<init>(ZipFile.java:171)
        at java.base/java.util.zip.ZipFile.<init>(ZipFile.java:185)
        at org.apache.maven.wrapper.Installer.unzip(Installer.java:169)
        at org.apache.maven.wrapper.Installer.createDist(Installer.java:86)
        at org.apache.maven.wrapper.WrapperExecutor.execute(WrapperExecutor.java:121)
        at org.apache.maven.wrapper.MavenWrapperMain.main(MavenWrapperMain.java:61)

```

> 所以报错是为3.6.3的maven版本未下载成功

这个地址"https://repo.maven.apache.org/maven2/org/apache/maven/apache-maven/3.6.3/apache-maven-3.6.3-bin.zip" .

你可以:
1. 删除maven对应版本重新下载
2. 更换国内镜像
3. 翻出去
