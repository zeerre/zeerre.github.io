---
title: VMware 安装 Arch Linux
date: 2021-10-25 20:58:00
tags:
    - VMware
    - Arch Linux
    - command line
categories:
    - OS
---

这里只记录在虚拟机中安装过程，至于 VMware 就不在赘述了。需要说明一点的是需要配置虚拟机高级选项的“固件类型”为 “UEFI"。

<!-- more -->

## 安装准备

下载 Arch Linux ISO 添加到 VMware 启动项，启动安装。


``` 

    root@archiso ~ #

```
### 验证启动模式

``` 

    ls /sys/firmware/efi/efivars

```

### 添加网络

VMware 初始开启网络链接状态 （NAT)

### 更新系统时间

``` 

    timedatectl set-ntp true

```

### 建立硬盘分区

所选为 GPT/UEFI 模式，故只分 EFI , swap 和 / 对应分区为 /dev/sda1 , /dev/sda2 ,/dev/sda3 。


``` 
    cfdisk /dev/sda

```

格式化分区：

```
    mkfs.fat -F32 /dev/sda1
    mkswap /dev/sda2 -L Swap
    mkfs.ext4 /dev/sda3

```

### 挂在分区

```
    mount /dev/sda3 /mnt
    mkdir -p /mnt/boot/efi
    mount /dev/sda1 /mnt/boot/efi

```

## 安装基本系统

### 配置镜像源

打开文件并将 `Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch` 添加到地址列的第一行。

```
    vim /etc/pacman.d/mirrorlist
```

### 基本系统安装

``` 
    pacstrap  -i /mnt base base-devel linux linux-firmware vim dhcpcd openssh net-tools xfsprogs man
```
一路默认回车就好。

### 生成 fstab 文件

``` 
    genfstab -U /mnt >> /mnt/etc/fstab 
```

## 切换到新系统

```
    arch-chroot /mnt /bin/bash

```

### 配置时区

``` 
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
    hwclock --systohc
```

### 配置本地 locale

```
    vim /etc/locale.gen
```
将 `en_US.UTF-8` 和 `zh_CN>UTF-8` 前 ”#“ 删除。

```
    locale-gen
    echo LANG=en_US.UTF-8 > /etc/locale.conf

```
### 配置网络

```
    systemctl enable dhcpcd.service
```

### 用户配置

root 用户密码配置，添加新用户

```
    passwd
    useradd -m -G wheel -s /bin/bash username
    passwd username
    visudo
```

修改 /etc/sudoers, 将 ”# %wheel ALL=(ALL) ALL" 中的 “#” 删除

### 安装引导

因为采用 GPT/UEFI 模式，故只安装 grub 和 efibootmgr。

```
    pacman -S grub efibootmgr
```
安装 EFI 中,`sda` 后不要加编号。

```
    grub-install --recheck /dev/sda
```
生成 grub 文件

```
    grub-mkconfig -o /boot/grub/grub.cfg
```
### 重启系统

```
    exit
    reboot
```

## 小结

Arch Linux 的安装还算简单，不过安装桌面我觉得就没的必要了，接下来要研究 `gentoo` ，感觉比较烧脑，桌面就在这里在研究吧。
gentoo 真心不好弄，慢慢来，不管花多少时间一定要啃下来。