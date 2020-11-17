# 函数式编程

函数式编程的一个特点就是，允许把函数本身作为参数传入另一个函数，还允许返回一个函数！

## Lambda表达式
```java

(s1, s2) -> {
    return s1.compareTo(s2);
}

Arrays.sort(array, (s1, s2) -> s1.compareTo(s2));

```

## FunctionalInterface

定义了单方法的接口称之为FunctionalInterface，用注解@FunctionalInterface标记

- 单方法接口被称为FunctionalInterface。
- 接收FunctionalInterface作为参数的时候，可以把实例化的匿名类改写为Lambda表达式，能大大简化代码。
- Lambda表达式的参数和返回值均可由编译器自动推断。

```java
@FunctionalInterface
public interface Callable<V> {
    V call() throws Exception;
}
```

因为Comparator<String>接口定义的方法是int compare(String, String)，和静态方法int cmp(String, String)相比，除了方法名外，方法参数一致，返回类型相同，因此，我们说两者的方法签名一致，可以直接把方法名作为Lambda表达式传入

```java
public class Main {
    public static void main(String[] args) {
        String[] array = new String[] { "Apple", "Orange", "Banana", "Lemon" };
        Arrays.sort(array, Main::cmp);
        System.out.println(String.join(", ", array));
    }

    static int cmp(String s1, String s2) {
        return s1.compareTo(s2);
    }
}
```

> 在这里，方法签名只看参数类型和返回类型，不看方法名称，也不看类的继承关系

方法引用
- 静态方法
- 实例方法
- 构造方法 

```java

public class Main {
    public static void main(String[] args) {
        List<String> names = List.of("Bob", "Alice", "Tim");
        List<Person> persons = names.stream().map(Person::new).collect(Collectors.toList());
        System.out.println(persons);
    }
}

class Person {
    String name;
    public Person(String name) {
        this.name = name;
    }
    public String toString() {
        return "Person:" + this.name;
    }
}


```

FunctionalInterface允许传入：

- 接口的实现类（传统写法，代码较繁琐）；
- Lambda表达式（只需列出参数名，由编译器推断类型）；
- 符合方法签名的静态方法；
- 符合方法签名的实例方法（实例类型被看做第一个参数类型）；
- 符合方法签名的构造方法（实例类型被看做返回类型）。

FunctionalInterface不强制继承关系，不需要方法名称相同，只要求方法参数（类型和数量）与方法返回类型相同，即认为方法签名相同。


## stream

划重点：这个Stream不同于java.io的InputStream和OutputStream，它代表的是任意Java对象的序列

| 	| java.io | java.util.stream |
| -- |-- | -- |
| 存储 | 顺序读写的byte或char | 顺序输出的任意Java对象实例 |
| 用途 | 序列化至文件或网络 | 内存计算／业务逻辑 |

再次划重点：这个Stream和List也不一样，List存储的每个元素都是已经存储在内存中的某个Java对象，而Stream输出的元素可能并没有预先存储在内存中，而是实时计算出来的。

List的用途是操作一组已存在的Java对象，而Stream实现的是惰性计算，两者对比如下：

| | java.util.List | java.util.stream |
| --  | --  | -- |
| 元素 | 已分配并存储在内存 | 可能未分配，实时计算 |
| 用途 | 操作一组已存在的Java对象 | 惰性计算 |

Stream API的特点是：

- Stream API提供了一套新的流式处理的抽象序列；
- Stream API支持函数式编程和链式操作；
- Stream可以表示无限序列，并且大多数情况下是惰性求值的。

### 创建stream


```java
// Stream.of()
Stream<String> stream = Stream.of("A", "B", "C", "D");
        // forEach()方法相当于内部循环调用，
        // 可传入符合Consumer接口的void accept(T t)的方法引用：
        stream.forEach(System.out::println);

//基于数组或Collection
Stream<String> stream1 = Arrays.stream(new String[] { "A", "B", "C" });
Stream<String> stream2 = List.of("X", "Y", "Z").stream();
//Collection（List、Set、Queue等），直接调用stream()方法就可以获得Stream


//基于Supplier
Stream<String> s = Stream.generate(Supplier<String> sp);

//创建Stream的第三种方法是通过一些API提供的接口，直接获得Stream
try (Stream<String> lines = Files.lines(Paths.get("/path/to/file.txt"))) {
    ...
}

// 另外，正则表达式的Pattern对象有一个splitAsStream()方法，可以直接把一个长字符串分割成Stream序列而不是数组
Pattern p = Pattern.compile("\\s+");
Stream<String> s = p.splitAsStream("The quick brown fox jumps over the lazy dog");
s.forEach(System.out::println);

// Java标准库提供了IntStream、LongStream和DoubleStream这三种使用基本类型的Stream，它们的使用方法和范型Stream没有大的区别，设计这三个Stream的目的是提高运行效率
// 将int[]数组变为IntStream:
IntStream is = Arrays.stream(new int[] { 1, 2, 3 });
// 将Stream<String>转换为LongStream:
LongStream ls = List.of("1", "2", "3").stream().mapToLong(Long::parseLong);
```