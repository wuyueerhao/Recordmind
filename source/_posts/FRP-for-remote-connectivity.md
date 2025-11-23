---
title: FRP内网穿透实现远程连接
date: 2023-02-22 16:19:48
tags: [FRP,远程连接]
categories:
	- 记录
---

由于工作的原因，基本上不间断的连接远程主机，远程主机在美国，中间试过向日葵、todesk、teamviewer、zerotier，都不是很理想。

> zerotier自建moon，这个方式也不错

FRP可玩性很高，这里只是拿来用做远程，可以参考详细文档和下载地址

[https://gofrp.org](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fgofrp.org)

需要准备：固定公网 IP 的机器， 一般为云主机，本文选择的是腾讯云的香港主机

服务端系统：Centos 8

远程系统：Windows10

#### 搭建 FRP 服务端

官方项目地址：https://github.com/fatedier/frp

#### 下载

首先从 [FRP release](https://github.com/fatedier/frp/releases/tag/v0.47.0) 页面下载适合你服务端机器的包，一般是 amd64，Linux 还是 Windows 依你服务器端的系统决定。解压，然后将它传到你的服务器上；如果你对命令行足够熟悉，也可以直接用 `wget` 命令在服务器上下载解压。
<!-- more -->

```
wget https://github.com/fatedier/frp/releases/download/v0.47.0/frp_0.47.0_linux_amd64.tar.gz
```

![image-20230222114455614](https://raw.githubusercontent.com/wuyueerhao/upimage/master/image-20230222114455614.png)

然后，进入 FRP 的路径，修改 frps.ini，基础的结构如下：

```
[common]
bind_port = 7000  #系统端口

dashboard_port = 7500 #后台登陆端口
dashboard_user = admin #后台用户名
dashboard_pwd = admin #后台密码
```

完成之后，输入命令`./frps -s frps.ini` 来启动服务端

**访问面板**

`frps` 运行成功后可以通过 `http://公网IP:面板端口` 访问 frp 面板（前提是配置了访问面板）。登录面板可以看到如下界面：

![image-20230222161054896](https://raw.githubusercontent.com/wuyueerhao/upimage/master/image-20230222161054896.png)

#### FRP客户端搭建

```
[common]
server_addr = xx.xx.xx.xx #服务器IP
server_port = 7000 #系统端口


[A4] # 这个名称可以自定义
type = tcp # SSH协议建立在TCP的基础上，所以填TCP
local_ip = 127.0.0.1 
local_port = 3389 #本地端口
remote_port = 33389 #远程端口
```

需要使用 cmd 或者 PowerShell 等终端来启动，不能直接双击 frpc.exe。

frp启动方式一：当前窗口运行，关闭当前窗口会自动退出

**服务端**

```
/存放frp的目录/frps -c /存放frp的目录/frps.ini
```

**客户端**

```
/存放frp的目录/frpc.exe -c /存放frp的目录/frpc.ini
```

frp启动方式二：后台运行，可以关闭当前窗口也不会自动退出

**服务端**

```
nohup /存放frp的目录/frps -c /存放frp的目录/frps.ini >/dev/null 2>&1 &
```

**客户端**

```
nohup /存放frp的目录/frpc.exe -c /存放frp的目录/frpc.ini >/dev/null 2>&1 &
```

frp启动方式三：以服务的形式后台运行

**添加开机自启**

```
sudo vim /lib/systemd/system/frps.service
```

```
[Unit]
Description=frps service
After=network.target syslog.target
Wants=network.target

[Service]
Type=simple
#启动服务的命令
ExecStart=/存放frp的目录/frps -c /存放frp的目录/frps.ini

[Install]
WantedBy=multi-user.target
```

启动服务 systemctl start frps

开机自启动 systemctl enable frps

重启服务 systemctl restart frps

停止服务 systemctl stop frps

查看日志与状态 systemctl status frps

#### Windows端设置frpc开机自启

使用 `winsw` 注册 windows 后台服务

##### 下载 winsw

GitHub下载 [winsw.exe](https://github.com/winsw/winsw/releases)

![image-20230222114522163](https://raw.githubusercontent.com/wuyueerhao/upimage/master/image-20230222114522163.png)

将软件重命名为 `winsw.exe`，并将该软件放到 `frpc` 客户端所在目录下备用。

还是在 `frpc` 客户端所在目录下创建 `winsw.xml` 文件，并写入一下内容并保存：

```
<service>
    <id>frpc</id>
    <name>frpc</name>
    <description>frpc</description>
    <executable>frpc.exe</executable>
    <arguments>-c frpc.ini</arguments>
    <onfailure action="restart" delay="60 sec"/>
    <onfailure action="restart" delay="120 sec"/>
    <logmode>reset</logmode>
</service>
```

启动 winsw

管理员权限启动 CMD（或按住shift键，点击鼠标右键选择），进入到 `frpc` 客户端所在目录下，添加服务

```
.\winsw.exe install
```

启动服务

```
.\winsw start
```

##### 其他

*#添加服务* 

```
winsw.exe install 
```

*#开始* 

```
winsw start 
```

*#关闭* 

```
winsw stop 
```

*#卸载服务* 

```
winsw uninstall
```



> 在将任何东西暴露于公网之前, 做好防护措施
