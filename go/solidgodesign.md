# SOLID Go Design

## 代码评审

### 糟糕的代码

* Rigid: 代码是否死板？它是否有强类型或参数以至于修改起来很困难？
* Fragile: 代码是否脆弱？对代码做轻微的改变是否就会引起程序极大的破坏？
* Immobile: 代码是否很难重构？
* Complex: 代码是否过于复杂，是否过度设计？
* Verbose: 代码是否过于冗长而使用起来很费劲？当查阅代码是否很难看出来代码在做什么？

## 好的设计

五个可重用软件设计的原则 - “SOLID”（英文首字母缩略字）：

* Single Responsibility Principle: 单一功能原则
* Open / Closed Principle: 开闭原则
* Liskov Substitution Principle: 里氏替换原则
* Interface Segregation Principle: 接口隔离原则
* Dependency Inversion Principle: 依赖反转原则

### Single Reponsibility Principle

> A class should have one, and only one, reason to change. –Robert C Martin

一段代码应该只因一个原因发生改变 我认为Go语言的Package体现了UNIX哲学精神。实际上每个package自身就是一个具有单一原则的变化单元的小型Go语言项目。

### Open / Closed Principle

> Bertrand Meyer曾经写道： &gt; Software entities should be open for extension, but closed for modification. –Bertrand Meyer, Object-Oriented Software Construction

嵌入是一个强大的工具，它允许Go语言types对扩展是开放的，但对修改是关闭的.

```go
type A struct {
        year int
}

type B struct {
        A
}
```

### Liskov Substiution Principle

> Require no more, promise no less. –Jim Weirich

经典：io.Reader

### Interface Segregation Principle

> Clients should not be forced to depend on methods they do not use. –Robert C. Martin A great rule of thumb for Go is accept interfaces, return structs. –Jack Lindamood

### Dependency Inversion Principle

> High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details. Details should depend on abstractions. –Robert C. Martin

## SOLID

* 单一功能原则鼓励你在package中构建functions、types以及方法表现出自然的凝聚力。types属于彼此，functions为单一目的服务。
* 开闭原则鼓励你使用嵌入将简单的type组合成更为复杂的。
* 里氏替换原则鼓励你在package之间表达依赖关系时用interface，而非具体类型。通过定义小巧的interface，我们可以更有信心地切实满足其合约。
* 接口隔离原则鼓励你仅取决于所需行为来定义函数和方法。如果你的函数仅仅需要有一个方法的interface做为参数，那么它很有可能只有一个责任。
* 依赖反转原则鼓励你在编译时将package所依赖的东西移除 - 在Go语言中我们可以看到这样做使得运行时用到的某个特定的package的import声明的数量减少。

### 资料

[SOLID Go Design - Go语言面向对象设计](https://blog.gokit.info/post/go-solid-design/)

