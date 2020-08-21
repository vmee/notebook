# 测试

## 单元测试

* Table-Driven Test
* T类型 testing.T
* Parallel测试

## 测试用例

* TestXxx\(t \*testing.T\) //基本测试用例
* BenchmarkXxxx\(b \*testing.B\) //压力测试的测试用例
* Example\_xxx\(\) //测试控制台输出的例子
* TestMain\(m \*testing.M\) //测试Main函数

## 基准测试

* func BenchmarkXxx\(b \*testing.B\)
* 通过 go test 命令，加上 -bench 标志来执行
* -benchtime 标志指定运行时间
* 计时方式 StartTimer StopTimer ResetTimer
* 并行执行 RunParallel函数
* 内存统计 -benchmem
* 结果
  * 2000000 ：基准测试的迭代总次数 b.N
  * 898 ns/op：平均每次迭代所消耗的纳秒数
  * 368 B/op：平均每次迭代内存所分配的字节数
  * 9 allocs/op：平均每次迭代的内存分配次数

## 子测试与基准测试

## 运行并验证示例

* 通过阅读 go test 命令源码和 testing 包中 example.go 文件了解

## TestMain

## HTTP测试辅助工具

## 代码覆盖率 Test Coverage

* go test -cover
* go tool cover

