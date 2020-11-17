# 单元测试

TDD

        编写接口
        │
        ▼
        编写测试
        │
        ▼    
    ┌─> 编写实现  
    │    │ 
    │ N  ▼  
    └── 运行测试

        │ Y
        ▼
        任务完成


单元测试的好处
单元测试可以确保单个方法按照正确预期运行，如果修改了某个方法的代码，只需确保其对应的单元测试通过，即可认为改动正确。此外，测试代码本身就可以作为示例代码，用来演示如何调用该方法。

使用JUnit进行单元测试，我们可以使用断言（Assertion）来测试期望结果，可以方便地组织和运行测试，并方便地查看测试结果。此外，JUnit既可以直接在IDE中运行，也可以方便地集成到Maven这些自动化工具中运行。

在编写单元测试的时候，我们要遵循一定的规范：
- 一是单元测试代码本身必须非常简单，能一下看明白，决不能再为测试代码编写测试；
- 二是每个单元测试应当互相独立，不依赖运行的顺序；
- 三是测试时不但要覆盖常用测试用例，还要特别注意测试边界条件，例如输入为0，null，空字符串""等情况。


JUnit是一个单元测试框架，专门用于运行我们编写的单元测试：
一个JUnit测试包含若干@Test方法，并使用Assertions进行断言，注意浮点数assertEquals()要指定delta。

## Fixture
- 通过@BeforeEach来初始化，通过@AfterEach来清理资源
- @BeforeAll和@AfterAll，它们在运行所有@Test前后运行


## 异常测试
```java
@Test
void testNegative() {
    assertThrows(IllegalArgumentException.class, () -> {
        Factorial.fact(-1);
    });
}
```

## 条件测试
- @Disabled 禁止运行此测试
- @EnableOnOs就是一个条件测试判断 @EnabledOnOs({ OS.LINUX, OS.MAC })
- @DisabledOnOs(OS.WINDOWS)
- @DisabledOnJre(JRE.JAVA_8)
- @EnabledIfSystemProperty 64位系统
- @EnabledIfEnvironmentVariable 环境变量  EnabledIfEnvironmentVariable(named = "DEBUG", matches = "true")

## 参数化测试
@ParameterizedTest注解，用来进行参数化测试