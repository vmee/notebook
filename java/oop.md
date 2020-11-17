# 面向对象


- 在OOP中，class和instance是“模版”和“实例”的关系；
- 定义class就是定义了一种数据类型，对应的instance是这种数据类型的实例；
- class定义的field，在每个instance都会拥有各自的field，且互不干扰；
- 通过new操作符创建新的instance，然后用变量指向它，即可通过变量来引用这个instance；
- 访问实例字段的方法是变量名.字段名；
- 指向instance的变量都是引用变量。
- 通过this.field就可以访问当前实例的字段 如果没有命名冲突，可以省略this
- 方法可变参数用类型...定义，可变参数相当于数组类型
- 没有在构造方法中初始化字段时，引用类型的字段默认是null，数值类型的字段用默认值，int类型默认值是0，布尔类型默认值是false
- 方法名相同，但各自的参数不同，称为方法重载（Overload）
- Java只允许一个class继承自一个类
- 子类无法访问父类的private字段或者private方法
- 用protected修饰的字段可以被子类访问
- super关键字表示父类（超类
- 只要某个class没有final修饰符，那么任何类都可以从该class继承
- 允许使用sealed修饰class，并通过permits明确写出能够从该class继承的子类名称
- 一个子类类型安全地变为父类类型的赋值，被称为向上转型（upcasting)
- 把一个父类类型强制转型为子类类型，就是向下转型（downcasting）
- instanceof实际上判断一个变量所指向的实例是否是指定类型，或者这个类型的子类
- 继承是is关系，组合是has关系
- 在继承关系中，子类如果定义了一个与父类方法签名完全相同的方法，被称为覆写（Override）
- 方法名相同，方法参数相同，但方法返回值不同，也是不同的方法
- @Override可以让编译器帮助检查是否进行了正确的覆写
- 多态是指，针对某个类型的方法调用，其真正执行的方法取决于运行时期实际类型的方法
- final修饰的方法不能被Override
- final修饰的类不能被继承
- final修饰的字段在初始化后不能被修改
- class定义了方法，但没有具体执行代码，这个方法就是抽象方法，抽象方法用abstract修饰
- 所谓interface，就是比抽象类还要抽象的纯抽象接口，因为它连字段都不能有
- 一个类可以实现多个interface
- 一个interface可以继承自另一个interface。interface继承自interface使用extend
- 接口可以定义default方法
- 静态字段属于所有实例“共享”的字段，实际上是属于class的字段；
- 调用静态方法不需要实例，无法访问this，但可以访问静态字段和其他静态方法；
- 静态方法常用于工具类和辅助方法。
- 定义为public的class、interface可以被其他任何类访问
- 定义为private的field、method无法被其他类访问
- 定义在一个class内部的class称为嵌套类（nested class），Java支持好几种嵌套类
- protected作用于继承关系。定义为protected的字段和方法可以被子类访问，以及子类的子类
- 包作用域是指一个类允许访问同一个package的没有public、private修饰的class，以及没有public、protected、private修饰的字段和方法
- Java内建的访问权限包括public、protected、private和package权限；
- Java在方法内部定义的变量是局部变量，局部变量的作用域从变量声明开始，到一个块结束；
- 使用局部变量时，应该尽可能把局部变量的作用域缩小，尽可能延后声明局部变量
- 用final修饰class可以阻止被继承
- 用final修饰method可以阻止被子类覆写
- 用final修饰field可以阻止被重新赋值
- 用final修饰局部变量可以阻止被重新赋值
- final修饰符不是访问权限，它可以修饰class、field和method
- 一个.java文件只能包含一个public类，但可以包含多个非public类
- 不要把任何Java核心库添加到classpath中！JVM根本不依赖classpath加载核心库！
- 模块只有它声明的导出的包，外部代码才被允许访问

## 构造方法

由于构造方法是如此特殊，所以构造方法的名称就是类名


## Object定义了几个重要的方法
- toString()：把instance输出为String；
- equals()：判断两个instance是否逻辑相等；
- hashCode()：计算一个instance的哈希值。

## 面向抽象编程的本质就是：

- 上层代码只定义规范（例如：abstract class Person）；
- 不需要子类就可以实现业务逻辑（正常编译）；
- 具体的业务逻辑由不同的子类实现，调用者并不关心
- 如果不实现抽象方法，则该子类仍是一个抽象类

## 抽象类与接口

| | abstract class  | interface|
| -- | -- | -- |
|继承  | 只能extends一个class  | 可以implements多个interface|
|字段  | 可以定义实例字段  | 不能定义实例字段|
|抽象方法  | 可以定义抽象方法  | 可以定义抽象方法|
|非抽象方法  | 可以定义非抽象方法 | 	可以定义default方法|


## 内部类
- Inner Class 内部类
- Anonymous Class 匿名类
- Static Nested Class 静态内部类

Java的内部类可分为Inner Class、Anonymous Class和Static Nested Class三种：

Inner Class和Anonymous Class本质上是相同的，都必须依附于Outer Class的实例，即隐含地持有Outer.this实例，并拥有Outer Class的private访问权限；

Static Nested Class是独立类，但拥有Outer Class的private访问权限。

## classpath和jar
VM通过环境变量classpath决定搜索class的路径和顺序；

不推荐设置系统环境变量classpath，始终建议通过-cp命令传入；

jar包相当于目录，可以包含很多.class文件，方便下载和使用；

MANIFEST.MF文件可以提供jar包的信息，如Main-Class，这样可以直接运行jar包

```java
//classpath默认 .当前目录 检索xxx.yxx.hello.class
java -classpath .;bin/path xxx.yxx.hello
//简写-cp
java -cp .;/bin/path xxx.yxx.hello
```


## 模块

> 模块只有它声明的导出的包，外部代码才被允许访问

```sh
# 当前目录下编译所有的.java文件，并存放到bin目录下
$ javac -d bin src/module-info.java src/com/itranswarp/sample/*.java
# 把bin目录下的所有class文件先打包成jar，在打包的时候，注意传入--main-class参数，让这个jar包能自己定位main方法所在的类
$ jar --create --file hello.jar --main-class com.itranswarp.sample.Main -C bin .

#运行jar包
$java -jar hello.jar

#把一个jar包转换成模块
$ jmod create --class-path hello.jar hello.jmod

#运行一个模块 模块不可以直接运行
$ java --module-path hello.jar --module hello.world

# jlink 只需要带上程序用到的模块
$  jlink --module-path hello.jmod --add-modules java.base,java.xml,hello.world --output jre/

# 运行模块
$ jre/bin/java --module hello.world
```


##  Compact Constructor

```java
//方法public Point {...}被称为Compact Constructor
public record Point(int x, int y) {
    public Point {
        if (x < 0 || y < 0) {
            throw new IllegalArgumentException();
        }
    }
}

public final class Point extends Record {
    public Point(int x, int y) {
        // 这是我们编写的Compact Constructor:
        if (x < 0 || y < 0) {
            throw new IllegalArgumentException();
        }
        // 这是编译器继续生成的赋值代码:
        this.x = x;
        this.y = y;
    }
    ...
}
```