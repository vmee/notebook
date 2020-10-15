# btrfs文件系统

- b-tree\Butter fs\Better fs
- GPL 开源
- Oracle 2007
- CoW 写时复制
- 替代ext3/ext4
- xfs
- 支持快照及快照的快照

核心特性
- 多物理卷支持; btrfs可由多个底层物理卷组成
- 支持RAID,以及联机"添加","移除","修改"
- 写时复制更新机制(CoW): 复制,更新及替换指针,而非"就地"更新,更新原文件
- 数据及数据检验码: checksum
- 子卷: sub_volume
- 快照: 支持快照的快照
- 透明压缩:自动解压缩, 会浪费cpu时钟周期



