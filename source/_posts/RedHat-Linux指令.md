---
title: 'RedHat-Linux指令'
date: 2013-05-08 08:33:06
categories: Linux
tags:
comments: ture
---

## 第一节

```
|dev 设备文件
|boot 启动文件
|etc 存放系统的配置文件
|bin 运行文件
|home 用户的住目录
--|用户的配置文件
|lib 库文件
|media 挂在的设备文件
|mnt 挂在的分区
|sbin var sys 相应的程序文件
|root 根用户的主目路：右上角有X的打不开
|查看-显示隐藏文件 
|命令：区分大小写
--|vim 编辑文件
--|cat 打开文件
--|ls 列出文件
  --|ls -a 显示全部文件
  --|ls *    :显示所有的文件或者文件夹，但是有.开头的不可以显示出来。
  --|ls .*    :显示以.开头的隐藏文件
  --|ls ？    :匹配单个的任意字符
  --|ls [1-3]
  --|ls /dev/sd[abcd][1-9]    :显示dev下串口分区的有abcd的并且在1-9范围内的
--|cd /usr/l+tab    :自动检索目录下l开头的文件，自动扩展功能
--|yum --help    :生成该命令的帮助
--|man history    :查看该命令帮助手册
--|history     :运行过得命令
  --|history -w his     :将前面使用过得命令保存到当前目录his 文件夹下，
  --|history -c    :清空history文件
  --|history -r    :将保存的history文件重新写入
  --|!32    :调用history文件中的第32条命令
--|clear    :清屏
--|alias    :对命令起别名：显示当前系统中定义过得别名
  --|alias 'aa=history'    :对history设置别名为aa:进对本次登录生效：永久生效需要对配置文件修改
|/home/user/.bashc    :对该文件增加别名可以使命令每次重启之后依旧有效
|通配符：
--|yum -i linuxqq.rpm    :RADHAT下安装QQ的方法
```



## 第二节

```
创建用户
|--useradd zkx ：创建一个用户zkx
  |--useradd -d /home/张三 zzx :指定主目录的用户账户
  |--useradd -u 602 zxx :指定用户ID的用户账户
  |--useradd -g cuser s4 :指定主组群的用户账户
|--passwd zkx ：用来更改密码
|--系统->管理->用户和群组 对用户进行设置：系统建立的用户的ID号都是小于500的
|--定位到-->文件系统-->etc-->passwd文件 ：可以看到自己的用户，项目和项目之间用：隔开
cuser:x:500:500:commonuser:/home/cuser:/bin/bash
s4:x:603:500::/home/s4:/bin/bash
用户名：密码：用户的ID号：用户所属组的组的ID号：用户的描述：用户的主目录：用户登录界面的文件保存位置
|--userdel s4 :删除用户s4：但是删不掉用户里面的文件
  |--userdel -r zxx :连用户的主目录一起删掉
|--groupadd m1 :添加一个用户组群
|exit :exit退出root 身份
|--su zkx :切换到zkx的用户身份
  |--su -c "ls /homex" zkx :执行一条命令使用zkx用户权限（需要提示输入密码）

chmod 740 /etc/sudoers    为这个文件提供保存文件的权限

编辑sudoers文件：为一个用户提供某种命令的权限
（使用root身份）
------------------------------
##定义用户的别名
User_Alias S=zkx,cuser
Runas_Alias R=root
Cmdn_Alias USER=/usr/sbin/useradd,/usr/sbin/userdel,/user/bin/passwad
##定义群组的别名
Cmdn_Alias GROP=/uer/sbin/group*
#定义权限
S ALL=(R) NOPASSWD:USER,GROP
----------------------------------

chmod 440 /etc/sudoers
是配置文件生效

sudo -l
查看是否已经生效

```

## 第三节

```
ls  查看文件，显示当前目录下面的内容。不包括以.开头的。（蓝色是文件夹，黑色是文件名字，绿色表示可执行，红色是压缩，浅蓝色是链接文件，灰色表示其他格式的文件，）

ls -x 按列输出，横向排序，

ls -c 按列输出，纵向排序。

ls -F 目录文件后面加上/ ，  * 为可执行文件    ，让文件类型指示符显示出来。

ls -R 将当前文件夹下面的子目录所有文件都显示出来。

ls -rl  以列表形式显示详细信息

一列            二列    三列         四列      五列              六列                         七列

drwxr-xr-x.     2       root          root       4096          8月 25 2010          yum.repos.d
-rw-r--r--.  1 root root       813  8月 25 2010 yum.conf
drwxr-xr-x.  4 root root      4096  7月 14 2011 yum
-rw-r--r--.  1 root root       585  6月 24 2010 yp.conf
drwxr-xr-x.  2 root root      4096  7月 14 2011 xml

lrwxrwxrwx. 1 root root           4  5月 22 23:39 root -> sda1
drwxr-xr-x. 2 root root          60  5月 22 23:39 raw
crw-rw-rw-. 1 root root      1,   8  5月 22 23:39 random
brw-rw----. 1 root disk      1,   9  5月 22 23:39 ram9
第一列：：
第一位：文件的性质 

d：表示文件夹

c：是一个设备文件，字符型的设备文件

b：块设备文件，一个块可能是一个扇区或者几个扇区

l：表示是一个链接文件     -->表示文件链接所指向的是那个文件

-：表示普通文件

后三位：rwx 

r：是读取权限

w：写权限

x：执行权限

第一个rwx是文件拥有者

再后三位：rwx 

属于拥有者用户所在组的用户的权限。

最后三位：rwx

对于其他用户的权限。

第二列：：文件连接树

第三列：：拥有者

第四列：：拥有者所在组的其他用户

第五列：：文件大小

第六列：：时间

第七列：：文件名称



touch abc 新建一个abc 的文件

gedit abc 编辑文件



[cuser@f102 ~]$ file 3     显示文件类型，
3: ASCII text

[cuser@f102 ~]$ file /dev/sda2
/dev/sda2: block special

[cuser@f102 ~]$ file -f f      读取f文件中的内容（以列表形式），判断列表里面所有文件的类型（f中内容为1<br>2）
1: ASCII text
2: cannot open `2' (No such file or directory)
:  cannot open `' (No such file or directory)

[cuser@f102 ~]$ more  -d /etc/termcap       查看长文档的一些命令


cat 命令查看文档，cat 文件路径名

cat 建立新文档     cat >s1.txt   用ctrl+d 结束编辑           >是输入重定向

cat s1.txt s2 >s3      将s1.txt 和s2 链接起来   保存到s3 下面

[cuser@f102 ~]$ mkdir a1       建立一个文件夹

[cuser@f102 a1]$ mkdir a1/b1/c1/d1 -p  创建多层的目录

[cuser@f102 a1]$ rmdir a1/b1/c1/d1 删除这些目录最后的一个文件夹（删除文件夹要确保是空的）

[cuser@f102 ~]$ rmdir -p a1/a1/b1/c1  删除所有的文件夹。（先删除最后的文件夹，以此向上，如果是空的就删除，不是空的就不删）

[cuser@f102 ~]$ mv a1 aa1   将a1改为aa1   当第二个参数存在，完成的就是移动。，第二个参数必须是一个目录才可以移动。

[cuser@f102 ~]$ mv 1 3 aa    将 1 3 都移动到aa 里面

[cuser@f102 aa]$ mv -bi 2 aa1   - 后面连续两个参数，可以使用。该指令b 表示覆盖文件的时候自动备份，i是在覆盖文件之前有相应的询问。

[cuser@f102 aa]$ mv -bfS .dak 3 aa1   强制替换文件，不做任何提问。    指令S是要对备份文件制定备份后缀。

[cuser@f102 aa]$ rm -r aa1    删除aa1并且删除里面的所有文件    指令r是递归的删除一个目录，删除文件也可以使用用，后面的参数必须是一个目录的名字，不可以是文件。



[cuser@f102 aa]$ mkdir  aa1/b1/b2 -p
[cuser@f102 aa]$ rm -ir aa1
rm：是否进入目录"aa1"? Y
rm：是否进入目录"aa1/b1"? Y
rm：是否删除目录 "aa1/b1/b2"？Y
rm：是否删除目录 "aa1/b1"？Y
rm：是否删除目录 "aa1"？Y                    提示确认删除的参数


[cuser@f102 aa]$ cp f /tmp    将文件拷贝到tmp     ，可以拷贝多个，最后一个是文件夹


[root@f102 ~]# cp -r a1 aa1     将a1文件夹 拷贝到aa1 中


查看文件的类型：

```

## 第四节

```
find 查找文件命令

--|find /dev -type d -print      将dev下面所有类型是文件夹的东西显示出来
--|find /dev -type b -print      查找块设备文件

--|find /etc -name "passwd*"        查找以passwd开头的文件

--|find /etc -name "passwd" -exec ls -l {} \;        查找文件passwd开头的，以列表详细信息形式显示出来。

--|find /etc -size +10000k -ok ls -l {} \;            查找大于1WK 的文件

实例：



[root@f102 cuser]# find /etc -size +1000k -ok ls -l {} \;
< ls ... /etc/selinux/targeted/policy/policy.24 > ? y
-rw-r--r--. 1 root root 5820086  7月 14 2011 /etc/selinux/targeted/policy/policy.24


--|find /root -perm 644 -exec ls -l {} \;         按照用户的权限查找文件



-rw-r--r--. 1 root root 129 12月  4 2004 /root/.tcsh

解释：644



读写执行

rw-   头三位，权限是110 。数字6
有权限就是1 ，没有权限的就是0，每三位转换为二进制，

r-- 代表4.。。。。。

查询指定文件拥有者文件

[root@f102 cuser]# find /home -user root              
/home
/home/cuser/100
/home/cuser/a
/home/cuser/a~


[root@f102 cuser]# find /home -user root -exec ls -l {} \;
总用量 24
drwx------. 34 cuser cuser 4096  5月 30 00:01 cuser
drwx------.  4 stu1  users 4096 10月 12 2011 stu1
drwx------.  4 stu11 stu11 4096 10月 19 2011 stu11
drwx------.  4 stu12 stu12 4096 10月 19 2011 stu12
drwx------. 27 stu13 stu13 4096 10月 19 2011 stu13
drwx------.  4 stu14 stu14 4096 10月 19 2011 stu14
-rw-r--r--. 1 root root 6 11月  2 2011 /home/cuser/100
-rw-r--r--. 1 root root 37 10月 26 2011 /home/cuser/a
-rw-r--r--. 1 root root 37 10月 26 2011 /home/cuser/a~




七天之内修改过的日志文件



[root@f102 cuser]# find /var/log -mtime 7 -exec ls -l {} \;
七天之内没有修改过的日志文件

[root@f102 cuser]# find /var/log -mtime +7 -exec ls -l {} \;
总用量 4
drwx------. 2 root root 4096  8月 24 2010 old


按照已知文件为标准，查找新的文件或者老的文件
查找比1新的文件

[root@f102 cuser]# find /home/cuser -newer /home/cuser/1
/home/cuser
/home/cuser/.xsession-errors
/home/cuser/2
/home/cuser/.local/share/gvfs-metadata
/home/cuser/.local/share/gvfs-metadata/root-30cf838a.log
/home/cuser/.local/share/gvfs-metadata/home-cedbf307.log

查看分区情况  这些信息存在在 proc /partitons 下面







[root@f102 cuser]# fdisk -l

Disk /dev/sda: 317.9 GB, 317891895808 bytes
255 heads, 63 sectors/track, 38648 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0xf0b1ebb0

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        7770       11979    33816793+  83  Linux
/dev/sda2           31675       35522    30909060    5  Extended
/dev/sda5           31675       32209     4297356   82  Linux swap / Solaris
/dev/sda6           32210       33483    10233373+   c  W95 FAT32 (LBA)

63个扇区。38648







sda

sd 串口，a 第一个口。b 第二个口

小于4的分区不可以让逻辑分区占用



分区调整



[root@f102 cuser]# fdisk -h

Usage:
 fdisk [options] <disk>    change partition table
 fdisk [options] -l <disk> list partition table(s)
 fdisk -s <partition>      give partition size(s) in blocks

Options:
 -b <size>                 sector size (512, 1024, 2048 or 4096)
 -c                        switch off DOS-compatible mode
 -h                        print help
 -u <size>                 give sizes in sectors instead of cylinders
 -v                        print version
 -C <number>               specify the number of cylinders
 -H <number>               specify the number of heads
 -S <number>               specify the number of sectors per track


挂载分区


[root@f102 cuser]# mount -t vfat /dev/sda6 /mnt/c 

编辑 /etc/fstab可以编辑挂在分区配置文件


/dev/sda6		/mnt/c			vfat	defaults	0 0
添加一行。可以在重启之后直接挂载上去


```

## 第五节

```
|ifconfig

配置网络环境命令

[root@f102 桌面]# ifconfig
eth1      Link encap:Ethernet  HWaddr 10:78:D2:93:C6:ED  
          inet addr:192.168.31.177  Bcast:192.168.31.255  Mask:255.255.255.0
          inet6 addr: fe80::1278:d2ff:fe93:c6ed/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:259983 errors:0 dropped:0 overruns:0 frame:0
          TX packets:156970 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:100 
          RX bytes:369104215 (352.0 MiB)  TX bytes:11998939 (11.4 MiB)
          Memory:fbec0000-fbee0000 

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:16436  Metric:1
          RX packets:454 errors:0 dropped:0 overruns:0 frame:0
          TX packets:454 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:23140 (22.5 KiB)  TX bytes:23140 (22.5 KiB)

virbr0    Link encap:Ethernet  HWaddr C6:2D:65:28:26:4A  
          inet addr:192.168.122.1  Bcast:192.168.122.255  Mask:255.255.255.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:25 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:0 (0.0 b)  TX bytes:4220 (4.1 KiB)


|ifconfig



[root@f102 桌面]# ifconfig eth1 192.168.31.111 netmask 255.255.255.0 up ^C
设置一个IP以及子网眼吗，以及激活状态







[root@f102 桌面]# ifconfig eth1
查看是IP地址







[root@f102 桌面]# hostname
f102
显示当前的主机名







[root@f102 桌面]# hostname f111
更改自己的主机名称





设置默认网关路由：



[root@f102 桌面]# route add default gw 192.168.31.1

更改DNS





[root@f102 桌面]# gedit /etc/resolv.conf
更改DNS的文件



格式：

nameserver 8.8.8.8



查看软件是否安装：



[root@f102 桌面]# rpm -qa |grep vsftp
vsftpd-2.2.2-6.el6.i686
查询软件vsftp






制作FTP服务器：



[root@f102 桌面]# rpm -ivh ftp-0.17-51.1.el6.i686.rpm 
warning: ftp-0.17-51.1.el6.i686.rpm: Header V3 RSA/SHA256 Signature, key ID fd431d51: NOKEY
Preparing...                ########################################### [100%]
   1:ftp                    ########################################### [100%]
安装rpm软件包




查看服务运行状态



[root@f102 桌面]# service vsftpd status
vsftpd 已停


重启服务：



[root@f102 桌面]# /etc/init.d/vsftpd restart
关闭 vsftpd：                                              [确定]
为 vsftpd 启动 vsftpd：                                    [确定]


关闭服务：



[root@f102 桌面]# service vsftpd stop
关闭 vsftpd：                                              [确定]

开启服务：





[root@f102 桌面]# service vsftpd start
为 vsftpd 启动 vsftpd：                                    [确定]


设置开机启动的服务

ntsysv 



ftp使用



[root@f111 桌面]# ftp localhost
Connected to localhost (127.0.0.1).
220 (vsFTPd 2.2.2)
Name (localhost:root): anonymous
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
227 Entering Passive Mode (127,0,0,1,159,114).
150 Here comes the directory listing.
-rw-r--r--    1 0        0               0 Jun 05 17:15 123
drwxr-xr-x    2 0        0            4096 May 26  2010 pub
226 Directory send OK.
ftp> get 123
local: 123 remote: 123
227 Entering Passive Mode (127,0,0,1,81,102).
150 Opening BINARY mode data connection for 123 (0 bytes).
226 Transfer complete.
登录本地FTP



使用匿名用户登录

查看所有的文件

下载文件123

下载到了桌面上



bye推出ftp服务器登录状态



更改ftp文件配置



[root@f111 桌面]# gedit /etc/vsftpd/vsftpd.conf






```

