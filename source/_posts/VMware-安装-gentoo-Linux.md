---
title: VMware 安装 gentoo Linux
date: 2021-10-26 20:34:12
tags:
    - gentoo
    - Arch Linux
categories:
    - OS
---

gentoo 比起 Arch Linux 安装貌似难了些，整个过程需要编译，自然就费时间和系统资源了。来吧，为了写出通俗易懂的文字，亲自实测记录。

<!-- more -->

同样，对 VMware 这里就不再赘述了。

## 安装基本系统

### 系统镜像 ISO 下载

下载地址：[https://github.com/HougeLangley/archzfs-iso/releases](https://github.com/HougeLangley/archzfs-iso/releases),
我喜欢最新版本。

### 分区和创建文件系统

```
    sgdisk --zap-all /dev/sda
    cfdisk -z /dev/sda
```
分成三个区（/，/boot ，swap），格式化分区

```
    mkfs.vfat -F32 /dev/sda1
    mkswap /dev/sda2 -L Swap
    mkfs.ext4 /dev/sda3
    swapon /dev/sda2
```
创建文件系统

```
    mkdir -p /mnt/gentoo
    mount /dev/sda3 /mnt/gentoo
    mkdir -p /mnt/gentoo/boot
    mount /dev/sda1 /mnt/gentoo/boot
```
### 目标 gentoo Linux 配置

```
    cd /mnt/gentoo
```
准备 stage3 ，用的清华镜像的 systemd。
```
    wget https://mirrors.tuna.tsighua.edu.cn/gentoo/releases/amd64/autobuilds/current-stage3-amd64-systemd/stage3-amd64-systemd-20211018T200943Z.tar.xz
```
然后解压，并删除此文件包。
```
    tar xvpf stage3-amd64-systemd-20211018T200943Z.tar.xz
    rm stage3-amd64-systemd-20211018T200943Z.tar.xz
```

### 配置 ssh 访问

```
    systemctl start sshd.service
    passwd

    ssh ipaddress -l root
```

### 生成 fstab 文件
```
    genfstab -U /mnt/gentoo >> /mnt/gentoo/etc/fstab 
    cat /mnt/gentoo/etc/fstab
```
### 定制 Gentoo Linux 

需要配置 **/mnt/gentoo/etc/portage/make.conf** ，**/mnt/gentoo/etc/portage/package.use/** ， **/mnt/gentoo/etc/portage/repos.conf/gentoo.conf** 这些文件。

1. 配置 `make.conf`
```
    vim /mnt/gentoo/etc/portage/make.conf
```
配置文件如下：

```
    #
    #注释信息
    #
    #COMMON_FLAGS="-march=skylake -O3 -pipe"
    COMMON_FLAGS="-march=skylake --Ofast -pipe"
    CFLAGS="${COMMON_FLAGS}"
    CXXFLAGS="${COMMON_FLAGS}"
    FCFLAGS="${COMMON_FLAGS}"
    FFLAGS="${COMMON_FLAGS}"
    #CPU_FLAGS_X86="aes avx f16c mmx mmxext pclmul popcnt rdrand sse sse2 sse3 sse4_1 sse4_2 ssse3"

    #PORTDIR="/var/db/repos/gentoo"
    #DISTDIR="/var/cache/distfiles"
    #PKGDIR="/var/cache/binpkgs"

    LC_MESSAGES=C
    MAKEOPTS="-j40"
    GENTOO_MIRRORS="https://mirrors.ustc.edu.cn/gentoo/"
    EMERGE_DEFAULT_OPTS="--keep-going --with-bdeps=y"

    #ACCEPT_KEYWORDS="~amd64"
    ACCEPT_LICENSE="*"
    L10N="en-US zh-CN en zh"
    LINGUAS="en_US zh_CN en zh"
    GRUB_PLATFORMS="efi-64"
    VIDEO_CARDS="nvidia"
    RUBY_TARGETS="ruby30"
    #QEMU_SOFTMMU_TARGETS="alpha aarch64 arm i386 mips mips64 mips64el mipsel ppc ppc64 s390x sh4 sh4eb sparc sparc64 x86_64"
    #QEMU_USER_TARGETS="alpha aarch64 arm armeb i386 mips mipsel ppc ppc64 ppc64abi32 s390x sh4 sh4eb sparc sparc32plus sparc64 x86_64"
    LLVM_TARGETS="X86"
    FEATURES="ccache"
    CCACHE_DIR="/var/cache/ccache"
```
2. 定义一些 USE
定义 python 版本，为后续编译工作打下基础。新建 `vim /mnt/gentoo/etc/portage/package.use/python` 可以像这样：
```
    */* PYTHON_TARGETS: -python2_7
    */* PYTHON_COMPAT: python3_9 python3_10
```

打开 GCC lto 和 pgo 优化，新建 `vim /mnt/gentoo/etc/portage/package.use/gcc` ，输入内容：
```
    sys-devel/gcc pgo lto
```

新建 `vim /mnt/gentoo/etc/portage/package.use/ghostscript-gpl` ，输入内容：
```
    app-text/ghostscript-gpl -l10n_zh-CN
```
这样就不会桌面系统安装楷体了。

选择 `systemd-boot` ，同样在上面那个目录创建一个文件 `vim /mnt/gentoo/etc/portage/package.use/systemd`，输入内容：
```
    sys-apps/systemd gnuefi
```

3. 配置官方源

首先是创建一个目录。 
```
    mkdir -p /mnt/gentoo/etc/portage/repos.conf 
```
这个目录的意义是所有的源地址配置文件都会放到这里，于是在这个目录下创建 gentoo.conf 官方源文件。

```
    [gentoo]
    location = /usr/portage
    sync-type = rsync
    sync-uri = rsync://mirrors.ustc.edu.cn/gentoo-portage/
    auto-sync = yes
    sync-rsync-verify-metamanifest = no ## 我的网络问题，导致无法同步到密钥，如果各位朋友的网络好不要加这一行。
```
### 准备 chroot 

```
    cp --dereference /etc/resolv.conf /mnt/gentoo/etc/
    mount -t proc /proc /mnt/gentoo/proc
    mount --rbind /sys /mnt/gentoo/sys
    mount --make-rslave /mnt/gentoo/sys
    mount --rbind /dev /mnt/gentoo/dev
    mount --make-rslave /mnt/gentoo/dev
    chroot /mnt/gentoo /bin/bash
    source /etc/profile
```
### 系统更新

```
    emerge-webrsync
    emerge --sync
```

### 选择 profile

```
    eselect profile list
    eselect profile set X
```

安装 `cpuid2cpuflags` 软件，命令如下：
```
    emerge -av --oneshot app-portage/cpuid2cpuflags
```
重新安装一遍 gcc ，命令如下：
```
    emerge -av --oneshot gcc
```
切换到新版本GCC：
```
    eselect gcc list
    eselect gcc set X
```
安装 ccache 
```
    emerge -av dev-util/ccache
```

完成安装后，用以下命令创建目录，设置权限：
```
    mkdir -p /var/cache/ccache
    chown root:portage /var/cache/ccache
    chmod 2775 /var/cache/ccache
```
编辑 `ccache` 配置文件 `/var/cache/ccache/ccache.conf` ，内容如下：
```
    max_size = 100.0G
    umask = 002
    cache_dir_levels = 3
```
最后到 `make.conf` 中启用 `ccache` 就可以了。
```
    FEATURES="ccache"
    CCACHE_DIR="/var/cache/ccache"
```
### 更新系统

```
    emerge -auvDN --with-bdeps=y --autounmask-write @world
```

先别纠结这一堆东西有什么意义，一般来说这里会提示类似 /etc 下有些文件需要更新，或者出现循环依赖。这些都好办。出现 /etc 下需要更新的，直接运行：
```
    etc-update --automode -3
```

出现循环依赖的，就目前几次我安装的经验来看都会提示 pyhon3.9 这个版本需要用 -bluetooth 这个 USE 编译一遍，那么首先我们去 mask 那里新建一个 python 文件，限定好只安装 python3.9 而不是 python3.10 这类的，写法类似上面介绍 mask 的部分，限定好版本然后用下面的命令重装 python3.9：
```
    USE=-bluetooth emerge -av python
```
这里需要说明的是 **USE** 的使用，根据提示，和 *emerge* 配合使用。

完成好这一切工作之后再跑刚才那个命令：
```
    emerge -auvDN --with-bdeps=y --autounmask-write @world
```
这个时候应该就开始运行了。整个编译所需要的时间由你 CPU 性能决定，线程越多，内存越大，速度越快。

如果中途因为某个包挂了，可以尝试以下两个命令：

```
    emerge @preserved-rebuild
    perl-cleaner --all
```
重复运行更新命令：

```
    emerge -auvDN --with-bdeps=y --autounmask-write @world
```

### 配置基础系统

```
    echo "Asia/Shanghai" > /etc/timezone
    emerge --config sys-libs/timezone-data

    echo "en_US.UTF-8 UTF-8
    zh_CN.UTF-8 UTF-8" >> /etc/locale.gen

    locale-gen
```
查看我们目前可选择的语言和设定该语言为我们系统语言，建议各位先不要定义语言为中文，因为目前的 TTY 下是不支持中文：

```
    eselect locale list
    eselect locale set X
```
> 针对树内文件系统的系统管理员
```
    emerge -av sys-fs/btrfs-progs sys-fs/xfsprogs sys-fs/jfsutils networkmanager app-admin/sysklogd sys-process/cronie layman sudo grub dev-vcs/git
```
因为我使用 `systemd` ，所以我在这里就默认 
```
    systemctl enable NetworkManager
```
如果你是 sudo 用户，使用以下代码给 wheel 用户组能够使用 sudo 的权限：
```
    sed -i 's/\# \%wheel ALL=(ALL) ALL/\%wheel ALL=(ALL) ALL/g' /etc/sudoers
```
我们现在可以配置我们的 root 用户密码了，直接 `passwd` 就可以了，默认情况下 Gentoo 要求非常严格的密码，我也不清楚怎么修改这个严格程度，所以大家知道的可以留言告诉我。

非常感谢 crackself 朋友的留言给我提供了思路。具体设置 Gentoo Linux passwd 密码强度是这样的。首先我们可以查看两个配置文件， `/etc/pam.d/passwd` 和 `/etc/pam.d/system-auth` 大家会注意到后者告诉我们相关配置文件在 `/etc/security/passwdqc.conf` 。所以，我们只需要设置这个配置文件就好了，那么默认的情况大概是这样的（之所以说是大概是因为我忘记默认配置文件了）。
```
    min=disabled,24,11,8,7
    max=40
    passphrase=8
    match=4
    similar=deny
    random=47
    enforce=everyone
    retry=3
```
我后面修改成：
```
    min=3,3,3,3,3
    max=8
    passphrase=0
    match=4
    similar=permit
    random=47
    enforce=everyone
    retry=3
```
这样就能设置简单的 `passwd` 密码了。具体这些值意味这什么，大家可以参考这篇文章获得启发。这里发散一下思维，假设你现在看到这篇文章的时候，已经完成了整个系统的安装，而且和我一样使用了 Gnome 桌面环境，那么通过上面的方法虽然你已经完成了登陆密码和 passwd 的密码修改，但是 gnome 的密码密钥却还是以前老的，如果你打开 chrome 或者 vivaldi 的时候，会要求你输入密码解锁。那么如何解决呢？需要你安装 `app-crypt/seahorse` 包，安装好之后，将默认的密码修改成你登录系统的密码，这样将来就不会要求重新输入密码了。

完善一下 `grub` 的配置，我并没有像 Yangmame 巨菊那样设置。只是在 `/etc/default/grub` 底部增加了一行，最后关于 grub 和 systemd-boot 的设置，我们第一部分结束前再弄，现在不急：

```
    GRUB_DISABLE_OS_PROBER=false
```
再输入下面两行代码，以防万一，虽然不输入，好像也没有关系，因为有一次安装我忘记了，也能正常用。

```
    ln -sf /proc/self/mounts /etc/mtab
    systemd-machine-id-setup
```
### 编译内核





