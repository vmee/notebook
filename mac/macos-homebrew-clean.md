# macOs磁盘清理之Homebrew数据清理

最近磁盘空间频频告警,还剩下不到10G的空间,MacBookPro 256G的工作本,严重不够用,想申请换大的无奈级别不够

所以清理了一波磁盘
- 卸载不用的应用  工具:appcleaner(免费)
- 清理聊天软件数据, wechat,wecom,feishu 使用一两年 占据小30G
- 大文件清理 工具:lemon(国内软件,腾讯出品,用完就删,用着还行)

上面大约清理出了50G空间

但感觉还是不太理想,我用户目录大约100G点,应用30G,系统15G 其他还占40多G

终端根目录命令执行
> du -h -d 1

终于又找到个占大空间的应用homebrew, 目录如下,约有14个G:
/usr/local/Homebrew/Library/Taps/homebrew/homebrew-cask/.git

没想到homebrew-cask的.git文件这么大,坑呀
homebrew-cask一般用于下载桌面应用,用不着可以直接uninstall了
