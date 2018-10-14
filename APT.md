# APT

APT 的全称为 Advanced Packaging Tools 。APT的主要包管理工具为APT-GET。

## APT软件源定义

/etc/apt/sources.list 文件存放的是软件源站点。

```
deb http://security.ubuntu.com/ubuntu xenial-security main
```

其中，http://security.ubuntu.com/ubuntu 对应的内容：

```
Index of /ubuntu
[ICO]	Name	Last modified	Size
[PARENTDIR]	Parent Directory	 	-
[DIR]	dists/	2018-05-01 18:56	-
[DIR]	indices/	2018-07-26 07:46	-
[   ]	ls-lR.gz	2018-07-26 07:46	14M
[DIR]	pool/	2010-02-27 06:30	-
[DIR]	project/	2013-06-28 11:52	-
[DIR]	ubuntu/	2018-07-26 07:54	-

```



## 更新软件包

使用apt-get update 命令会去/etc/apt/sources.list 中下载软件列表，并保存到/var/lib/apt/lists 目录下，如下表（文件名中的下划线可以看作是目录的分割符）：

```
-rw-r--r-- 1 root root 105K Jul 26 14:57 security.ubuntu.com_ubuntu_dists_xenial-security_InRelease
-rw-r--r-- 1 root root 3.5M Jul 26 14:57 security.ubuntu.com_ubuntu_dists_xenial-security_main_binary-amd64_Packages
-rw-r--r-- 1 root root 3.0M Jul 26 14:57 security.ubuntu.com_ubuntu_dists_xenial-security_main_binary-i386_Packages
-rw-r--r-- 1 root root 3.5M Jul 26 13:45 security.ubuntu.com_ubuntu_dists_xenial-security_main_i18n_Translation-en
```

其中Packages文件的内容：

```
Package: advancecomp
Architecture: amd64 
Version: 1.20-1ubuntu0.1
Priority: optional
Section: utils 
Origin: Ubuntu
Maintainer: Ubuntu Developers <ubuntu-devel-discuss@lists.ubuntu.com>
Original-Maintainer: Piotr Ożarowski <piotr@debian.org>
Bugs: https://bugs.launchpad.net/ubuntu/+filebug
Installed-Size: 805
Depends: libc6 (>= 2.14), libgcc1 (>= 1:3.0), libstdc++6 (>= 5.2), zlib1g (>= 1:1.1.4)
Filename: pool/main/a/advancecomp/advancecomp_1.20-1ubuntu0.1_amd64.deb
Size: 159294
MD5sum: e22362f5aca5b9a47b28960a7b46b910
SHA1: bc0f81e08b617043319982db0183b91a2107be24
SHA256: 80cad81cfc867f4e685024507271cc72732ca3eb43452ec1a779b58fa8b48ec2
Homepage: http://www.advancemame.it/
Description: collection of recompression utilities
Description-md5: 45269d7ed6ff6092f699fce2e0061b74
Supported: 5y

```



其中，Filename: pool/main/a/advancecomp/advancecomp_1.20-1ubuntu0.1_amd64.deb 属性指定了软件包的位置为 http://security.ubuntu.com/ubuntu/pool/main/a/advancecomp/advancecomp_1.20-1ubuntu0.1_amd64.deb。



## 安装软件包

当执行 sudo apt-get install xxx 时，Ubuntu 就去这些站点下载deb软件包到本地/var/cache/apt/archives 目录下，并执行安装。

 

