# IO

- InputStream 输入流 OutputStream 输出流 这是最基本的两路IO流
- Reader和Writer表示字符流,字符流传输的蜬上数据单位是char

IO流是一种流式的数据输入/输出模型：
- 二进制数据以byte为最小单位在InputStream/OutputStream中单向流动；
- 字符数据以char为最小单位在Reader/Writer中单向流动。

Java标准库的java.io包提供了同步IO功能：
- 字节流接口：InputStream/OutputStream；
- 字符流接口：Reader/Writer


## Files & Paths

Files工具类还有copy()、delete()、exists()、move()等快捷方法操作文件和目录。

```java

//读取一个文件的全部内容为一个byte[]
byte[] data = Files.readAllBytes(Paths.get("/path/to/file.txt"));

//一个文件的内容全部读取为string

// 默认使用UTF-8编码读取:
String content1 = Files.readString(Paths.get("/path/to/file.txt"));
// 可指定编码:
String content2 = Files.readString(Paths.get("/path/to/file.txt"), StandardCharsets.ISO_8859_1);
// 按行读取并返回每行内容:
List<String> lines = Files.readAllLines(Paths.get("/path/to/file.txt"));

// 写入二进制文件:
byte[] data = ...
Files.write(Paths.get("/path/to/file.txt"), data);
// 写入文本并指定编码:
Files.writeString(Paths.get("/path/to/file.txt"), "文本内容...", StandardCharsets.ISO_8859_1);
// 按行写入文本:
List<String> lines = ...
Files.write(Paths.get("/path/to/file.txt"), lines);




```