---
title: 优雅的使用 CleanMyMac X 4.12.6 中文版
date: 2023-02-27 10:37:00
tags: [CleanMyMac X]
password: hello
categories:
	- 记录
---

![MAC系统清理CleanMyMac X破解版- Mac软件](https://raw.githubusercontent.com/wuyueerhao/upimage/master/screenshot-cleanmymac-x-1.png)

# CleanMyMac X

免费解锁（几乎）所有 CleanMyMac X 的功能！

已更新至版本 4.12.6（App Store）- Intel 芯片

不支持 Apple Silicon。

## 如何使用
<!-- more -->
1. 卸载旧版，从系统自带的 App Store 下载正版的CleanMyMac X
2. 打开终端窗口并导航到要克隆存储库的目录。
3. 键入`git clone https://github.com/davidepedrazzi/CleanMyMacX.git`并按 Enter 将存储库下载到您的计算机。
4. 键入`cd CleanMyMacX`并按回车键导航到 CleanMyMacX 目录。
5. 键入`chmod u+x patch.py`并按回车键使 patch.py 文件可执行。
6. 键入`sudo ./patch.py`或`sudo python3 ./patch.py`并按回车键运行该工具。

```linux
$ sudo ./patch.py
Patching /Applications/CleanMyMac-MAS.app/Contents/MacOS/CleanMyMac-MAS...
Patching /Applications/CleanMyMac-MAS.app/Contents/Library/LoginItems/CleanMyMac-MAS Menu.app/Contents/MacOS/CleanMyMac-MAS Menu...
Re-signing /Applications/CleanMyMac-MAS.app...
/Applications/CleanMyMac-MAS.app: replacing existing signature
/Applications/CleanMyMac-MAS.app: valid on disk
/Applications/CleanMyMac-MAS.app: satisfies its Designated Requirement
Enjoy!
```



> 且用且珍惜

原文链接：https://github.com/davidepedrazzi/CleanMyMacX
