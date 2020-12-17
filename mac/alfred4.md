# Alfred4

## 参考文档
[程序员的macOS系列：高效Alfred进阶](https://juejin.cn/post/6844904062484217863)


## Features

### File Search
- Quick Search（快速搜索） 通过'或 spacebar空格键可以可以快速查找文件
- 空格 open 打开文件
- 空格 find 找到文件
- 空格 in 查找文件内容

### Navigation
- /：在 Alfred 输入栏中首先输入“/”，会带你进入 macOS根目录；
- ~：在 Alfred 输入栏中首先输入“~”，会带你进入当前的用户目录； 
- 逆序排列，快捷键：⇧Shift + ⌘Command + S
- 隐藏预览面板，快捷键：⇧Shift + ⌘Command + I  

###  Buffer
- Alt + ↑ ：把该文件加入列表；
- Alt + ↓ ：把该文件加入列表，光标跳向下一个文件；
- Alt + ← ：删除列表最后一个文件；
- Alt + → ：对列表文件进行统一操作。


###  Web Bookmarks（网页书签）
- mb 搜索书签

###  Clipboard History（剪切版历史）

默认不开启, 很有用的功能
快捷键⌥ + ⌘ + C（alt + commaand + C）打开剪切版历史视图


###  System

Alfred 支持系统功能操作，例如：
- screensaver（显示待机屏幕）、
- emptytrash（清空回收站）
- logout（登出当前用户）
- sleep（睡眠模式）
- sleepdisplays（关闭屏幕显示）
- lock（锁屏）
- restart（重启）
- shutdown（关机）
- volup（增加音量）
- voldown（减少音量）
- mute（静音）等快捷命令。
针对应用程序可以：
- hide（隐藏）
- quit（退出程序）
- forcequit（强制退出）
- quit All（退出所有程序）。
  
Eject 是弹出磁盘、存储卡或者虚拟磁盘镜像，如 .dmg 挂载后的磁盘。Eject All是全部弹出。
以上的操作，都是可以自定义关键字，另外有一些后面带 Confirm的，表示是一些危险的操作，可以勾选，在操作时会先弹窗提示操作的风险。
比较常用的推荐：Lock 锁屏，放心离开办公位，开会！如果是去厕所，则可以用 Sleep Displays，临时关闭屏幕；Empty 清空回收站。

### Terminal

- > 命令

### LargeType

- large 放大
- COMMAND+l 快捷键放大

## workflow

高效workflow推荐
- [妙用 Alfred 让你最近使用的文件触手可及](https://sspai.com/post/47063) 下载：[mpco/AlfredWorkflow-Recent-Documents](https://github.com/mpco/AlfredWorkflow-Recent-Documents/releases)
- 