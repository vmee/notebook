# 泛型

泛型就是定义一种模板

- 泛型就是编写模板代码来适应任意类型；
- 泛型的好处是使用时不必对类型进行强制转换，它通过编译器对类型进行检查；
- 注意泛型的继承关系：可以把ArrayList<Integer>向上转型为List<Integer>（T- 不能变！），但不能把ArrayList<Integer>向上转型为ArrayList<Number>（T不能变成父类）。
- 使用泛型时，把泛型参数<T>替换为需要的class类型，例如：ArrayList<String>，ArrayList<Number>等
- 不指定泛型参数类型时，编译器会给出警告，且只能将<T>视为Object类型
- 可以在接口中定义泛型类型，实现此接口的类必须实现正确的泛型类型

Java的泛型是采用擦拭法实现的；

擦拭法决定了泛型<T>：

- 不能是基本类型，例如：int；
- 不能获取带泛型类型的Class，例如：Pair<String>.class；
- 不能判断带泛型类型的类型，例如：x instanceof Pair<String>；
- 不能实例化T类型，例如：new T()。
- 泛型方法要防止重复定义方法，例如：public boolean equals(T obj)；
- 子类可以获取父类的泛型类型<T>。


## ArrayList
- 如果不定义泛型类型时，泛型类型实际上就是Object

```java
// 无编译器警告:
List<String> list = new ArrayList<String>();
// 可以省略后面的Number，编译器可以自动推断泛型类型：
List<Number> list = new ArrayList<>();

```

## 泛型接口
```java
public interface Comparable<T> {
    /**
     * 返回负数: 当前实例比参数o小
     * 返回0: 当前实例与参数o相等
     * 返回正数: 当前实例比参数o大
     */
    int compareTo(T o);
}
```



## extends通配符

使用类似<? extends Number>通配符作为方法参数时表示：

方法内部可以调用获取Number引用的方法，例如：Number n = obj.getFirst();；

方法内部无法调用传入Number引用的方法（null除外），例如：obj.setFirst(Number n);。

即一句话总结：使用extends通配符表示可以读，不能写。

使用类似<T extends Number>定义泛型类时表示：

泛型类型限定为Number以及Number的子类。


## super通配符