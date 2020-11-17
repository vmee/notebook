# 核心类

## 字符串
- String是一个引用类型，它本身也是一个class
- Java编译器对String有特殊处理，即可以直接用"..."来表示一个字符串
- 字符串在String内部是通过一个char[]数组表示的
- Java字符串的一个重要特点就是字符串不可变,这种不可变性是通过内部的private final char[]字段，以及没有任何修改char[]的方法实现的
- 比较字符串的内容是否相同。必须使用equals()方法而不能用==
- 忽略大小写 equalsIgnoreCase 
- java的String和char在内存中总是以Unicode编码表示
- 创建新对象时，优先选用静态工厂方法而不是new操作符。

```java

// 是否包含子串:
"Hello".contains("ll"); // true
// 搜索子串
"Hello".indexOf("l"); // 2
"Hello".lastIndexOf("l"); // 3
"Hello".startsWith("He"); // true
"Hello".endsWith("lo"); // true
// 提取子串
"Hello".substring(2); // "llo"
"Hello".substring(2, 4); "ll"
//去除空格
"  \tHello\r\n ".trim(); // "Hello"
"\u3000Hello\u3000".strip(); // "Hello"
" Hello ".stripLeading(); // "Hello "
" Hello ".stripTrailing(); // " Hello"

//判断字符串是否为空和空白字符串
"".isEmpty(); // true，因为字符串长度为0
"  ".isEmpty(); // false，因为字符串长度不为0
"  \n".isBlank(); // true，因为只包含空白字符
" Hello ".isBlank(); // false，因为包含非空白字符

//替换子串
String s = "hello";
s.replace('l', 'w'); // "hewwo"，所有字符'l'被替换为'w'
s.replace("ll", "~~"); // "he~~o"，所有子串"ll"被替换为"~~"


//正则替换
String s = "A,,B;C ,D";
s.replaceAll("[\\,\\;\\s]+", ","); // "A,B,C,D"

//分割字符串
String s = "A,B,C,D";
String[] ss = s.split("\\,"); // {"A", "B", "C", "D"}

//拼接字符串
String[] arr = {"A", "B", "C"};
String s = String.join("***", arr); // "A***B***C"

//格式化字符串
// 常用的占位符有：
// %s：显示字符串；
//%d：显示整数；
//%x：显示十六进制整数；
//%f：显示浮点数。
String s = "Hi %s, your score is %d!";
s.formatted("Alice", 80);
String.format("Hi %s, your score is %.2f!", "Bob", 59.5);

//类型转换
//转为字符串
String.valueOf(123); // "123"
String.valueOf(45.67); // "45.67"
String.valueOf(true); // "true"
String.valueOf(new Object()); // 类似java.lang.Object@636be97c

//转为int
int n1 = Integer.parseInt("123"); // 123
int n2 = Integer.parseInt("ff", 16); // 按十六进制转换，255

//转为bool
boolean b1 = Boolean.parseBoolean("true"); // true
boolean b2 = Boolean.parseBoolean("FALSE"); // false

//系统变量转为int
Integer.getInteger("java.version"); // 版本号，11

//转换为char[]
char[] cs = "Hello".toCharArray(); // String -> char[]
String s = new String(cs); // char[] -> String


// 转换编码 
byte[] b1 = "Hello".getBytes(); // 按系统默认编码转换，不推荐
byte[] b2 = "Hello".getBytes("UTF-8"); // 按UTF-8编码转换
byte[] b2 = "Hello".getBytes("GBK"); // 按GBK编码转换
byte[] b3 = "Hello".getBytes(StandardCharsets.UTF_8); // 按UTF-8编码转换
byte[] b = ...
String s1 = new String(b, "GBK"); // 按GBK转换
String s2 = new String(b, StandardCharsets.UTF_8); // 按UTF-8转换
```

## StringBuilder
它是一个可变对象，可以预分配缓冲区，这样，往StringBuilder中新增字符时，不会创建新的临时对象

```java


public class Main {
    public static void main(String[] args) {
        var sb = new StringBuilder(1024);
        sb.append("Mr ")
          .append("Bob")
          .append("!")
          .insert(0, "Hello, ");
        System.out.println(sb.toString());
    }
}


```

## StringJoiner
用分隔符拼接数组

```java
String[] names = {"Bob", "Alice", "Grace"};
var s = String.join(", ", names);
```

## 包装类型

所有的包装类型都是不变类

| 基本类型 | 对应的引用类型 |
| -- | -- |
| boolean | java.lang.Boolean |
| byte | java.lang.Byte |
| short | java.lang.Short |
| int | java.lang.Integer |
| long | java.lang.Long |
| float | java.lang.Float |
| double | java.lang.Double |
| char | java.lang.Character |

```java
// int和Integer可以互相转换
int i = 100;
Integer n = Integer.valueOf(i);
int x = n.intValue();

// 这种直接把int变为Integer的赋值写法，称为自动装箱（Auto Boxing）
Integer n = 100; // 编译器自动使用Integer.valueOf(int) 自动装箱
int x = n; // 编译器自动使用Integer.intValue() 自动拆箱


// 进制转换

int x1 = Integer.parseInt("100"); // 100
int x2 = Integer.parseInt("100", 16); // 256,因为按16进制解析

 System.out.println(Integer.toString(100)); // "100",表示为10进制
System.out.println(Integer.toString(100, 36)); // "2s",表示为36进制
System.out.println(Integer.toHexString(100)); // "64",表示为16进制
System.out.println(Integer.toOctalString(100)); // "144",表示为8进制
System.out.println(Integer.toBinaryString(100)); // "1100100",表示为2进制

// 静态变量
// boolean只有两个值true/false，其包装类型只需要引用Boolean提供的静态字段:
Boolean t = Boolean.TRUE;
Boolean f = Boolean.FALSE;
// int可表示的最大/最小值:
int max = Integer.MAX_VALUE; // 2147483647
int min = Integer.MIN_VALUE; // -2147483648
// long类型占用的bit和byte数量:
int sizeOfLong = Long.SIZE; // 64 (bits)
int bytesOfLong = Long.BYTES; // 8 (bytes)

//所有的整数和浮点数的包装类型都继承自Number
// 向上转型为Number:
Number num = new Integer(999);
// 获取byte, int, long, float, double:
byte b = num.byteValue();
int n = num.intValue();
long ln = num.longValue();
float f = num.floatValue();
double d = num.doubleValue();

//无符号处理
byte x = -1;
byte y = 127;
System.out.println(Byte.toUnsignedInt(x)); // 255
System.out.println(Byte.toUnsignedInt(y)); // 127
```

## javabean

在Java中，有很多class的定义都符合这样的规范：
- 若干private实例字段；
- 通过public方法来读写实例字段

要枚举一个JavaBean的所有属性，可以直接使用Java核心库提供的Introspector
使用Introspector.getBeanInfo()可以获取属性列表

## 枚举类

Java使用enum定义枚举类型，它被编译器编译为final class Xxx extends Enum { … }；

通过name()获取常量定义的字符串，注意不要使用toString()；

通过ordinal()返回常量定义的顺序（无实质意义）；

可以为enum编写构造方法、字段和方法

enum的构造方法要声明为private，字段强烈建议声明为final；

enum适合用在switch语句中。

使用enum来定义枚举类
```java
enum Weekday {
    SUN, MON, TUE, WED, THU, FRI, SAT;
}

enum Weekday {
    MON(1), TUE(2), WED(3), THU(4), FRI(5), SAT(6), SUN(0);

    public final int dayValue;

    private Weekday(int dayValue) {
        this.dayValue = dayValue;
    }
}

enum Weekday {
    MON(1, "星期一"), TUE(2, "星期二"), WED(3, "星期三"), THU(4, "星期四"), FRI(5, "星期五"), SAT(6, "星期六"), SUN(0, "星期日");

    public final int dayValue;
    private final String chinese;

    private Weekday(int dayValue, String chinese) {
        this.dayValue = dayValue;
        this.chinese = chinese;
    }

    @Override
    public String toString() {
        return this.chinese;
    }
}


// 返回常量名
String s = Weekday.SUN.name(); // "SUN"

// 返回定义常量的顺序
int n = Weekday.MON.ordinal(); // 1
```

## 记录类

一个不变类具有以下特点：
- 定义class时使用final，无法派生子类；
- 每个字段使用final，保证创建实例后无法修改任何字段。

从Java 14开始，提供新的record关键字，可以非常方便地定义Data Class
- 使用record定义的是不变类；
- 可以编写Compact Constructor对参数进行验证；
- 可以定义静态方法。

```java
public record Point(int x, int y) {}
```