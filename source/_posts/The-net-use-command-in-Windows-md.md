---
title: The 'net use' command in Windows
date: 2022-10-09 20:38:46
tags: 
    - net use
categories:
    - 操作技巧
---

## net use command

```
C:\Users\zeerr>net use /help
```
<!-- more -->
### 此命令的语法是:


```

    NET USE
    [devicename | *] [\\computername\sharename[\volume] [password | *]]
            [/USER:[domainname\]username]
            [/USER:[dotted domain name\]username]
            [/USER:[username@dotted domain name]
            [/SMARTCARD]
            [/SAVECRED]
            [/REQUIREINTEGRITY]
            [/REQUIREPRIVACY]
            [/WRITETHROUGH]
            [[/DELETE] | [/PERSISTENT:{YES | NO}]]

    NET USE {devicename | *} [password | *] /HOME

    NET USE [/PERSISTENT:{YES | NO}]

    NET USE 将计算机连接到共享资源
    或将计算机与共享资源断开连接。使用时如果没有选项，它会列出
    计算机的连接。

    devicename       分配一个名称以连接到资源，或指定
                    要断开连接的设备。有两种
                    设备名称: 磁盘驱动器(D: 至 Z:)和打印机
                    (LPT1: 至 LPT3:)。键入星号而不是
                    特定设备名称以分配下一个可用
                    设备名称。
    \\computername   为控制共享资源的计算机
                    的名称。如果计算机名包含空白字符，
                    则用引号(" ")将双反斜杠(\\)和计算机名
                    括起来。计算机名的长度可以为
                    1 至 15 个字符。
    \sharename       为共享资源的网络名称。
    \volume          指定服务器上的 NetWare 卷。必须已安装并正在运行
                    Netware 客户端服务(Windows Workstations)
                    或 Netware 网关服务(Windows Server)
                    才能连接到 NetWare 服务器。
    password         为访问共享资源所需的密码。
    *                产生密码提示。在密码提示处
                    键入密码时不显示密码。
    /USER            指定进行连接的另一个
                    用户名。
    domainname       指定其他域。如果忽略域，
                    则使用当前登录的域。
    username         指定登录所使用的用户名。
    /SMARTCARD       指定连接将使用智能卡上
                    的凭据。
    /SAVECRED        指定要保存用户名和密码。
                    该开关将被忽略，除非命令提示输入用户名
                    和密码。
    /HOME            将用户连接到他们的主目录。
    /DELETE          取消网络连接并
                    从持续连接列表中删除该连接。
    /REQUIREINTEGRITY
                    需要签名的共享连接。如果提供程序
                    不支持签名连接，则操作将失败。
    /REQUIREPRIVACY  需要加密的共享连接。如果提供程序
                    不支持加密连接，则操作将失败。

    /PERSISTENT      控制持续网络连接的使用。
                    默认为上次使用的设置。
    YES              进行连接时将它们保存，并在下次
                    登录时将它们恢复。
    NO               不保存正在进行的连接或随后的
                    连接；下次登录时将恢复
                    现有连接。使用 /DELETE 开关删除
                    持续连接。

    ```

    NET HELP 命令 | MORE 显示帮助，一次显示一屏。