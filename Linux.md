# Linux

## 文件系统

Linux 和 UNIX 中的文件系统是一个以 / 为根的树状式文件结构，/ 是 Linux 和 UNIX 中的根目录，同样它也是文件系统的起点。所有的文件和目录都位于 / 路径下，包括我们经常听到的 /usr、/etc、/bin、/home 等。在早期的 UNIX 系统中，各个厂家都定义了自己文件系统的命名构成，比较混乱，而且难以区分。

为了避免在 Linux 系统上也出现这种命名混乱的问题，在 1994 年推出了 FSSTND(FileSystem Standard) 的 Linux 文件系统层次结构标准，后来 UNIX 团队把 FSSTND 发扬光大，成为了后来的 FHS(FileSystem Hierarchy Standard) 。
FHS 标准使得众多的 Linux distributions(Linux 发行版) 有了统一的文件系统命名标准，换一种说法：FHS 就是一种文件系统的命名标准。一般来说，Linux distributions 都需要遵循 FHS 规定的

![image-20220107224140127](https://raw.githubusercontent.com/a1254898873/images/master/202203242153406.awebp)



### /home 目录

*/home* 目录是系统默认的使用者主文件夹（home directory）。



### /boot 目录

/boot 目录包含启动操作系统所需的静态文件，比如 Linux 内核，这些文件对系统的启动至关重要。*Linux Kernel* 常用的文件名为 *vmlinuz*, 但是如果你使用的是 *grub2* 这个开机程序，还会存在 /boot/grub2 这个目录。

### /dev 目录

*/dev* 目录都是一些设备节点，这些设备节点是 Linux 系统中的设备或者是内核提供的虚拟设备。这些设备节点同样也对系统正常运行至关重要。/dev 目录和子目录下的设备是字符设备和块设备。字符设备就是**鼠标、键盘、调制解调器**，块设备就是**硬盘、软盘驱动器**。存储 /dev 目录下的文件就相当于是存储某个设备。

### /etc 目录

/etc 目录是为计算机本地的配置文件保留的，系统主要的配置文件都放在这个目录下，比如账号密码，服务的启停，一般来说，这个目录下面一般用户只有读权限，只有 root 用户具有修改权限



### /lib 目录

系统的函数库有很多，而 /lib 目录就像一个仓库，它用于存放执行 */bin* 和 */sbin* 中二进制文件所需要的库，这些共享库映像对于系统 boot 和执行根文件系统中的命令特别重要



### /opt 目录

*/opt/* 目录为大多数应用程序软件包提供存储空间，将文件放置在 /opt/ 目录中的包会创建一个与包同名的目录。 反过来，该目录保存了原本会分散在整个文件系统中的文件，从而为系统管理员提供了一种简单的方法来确定特定包中每个文件的角色。



### /usr 目录

/usr 目录是需要好好聊聊得一个目录了，很多读者都误以为 /usr 是 user 的缩写，其实 usr 是 *Unix Software Resource* 的缩写，FHS 建议软件开发者应该将数据合理的放置在这个目录的次目录下，不要自己创建软件独立的目录。





## 基本操作

一、目录操作

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203242153407.awebp)

ls

列出文件和目录，它是 Linux 最常用的命令之一。

- -a 显示所有文件和目录包括隐藏的
- -l 显示详细列表
- -h 适合人类阅读的
- -t 按文件最近一次修改时间排序
- -i 显示文件的 inode （ inode 是文件内容的标识）



cd

cd 是英语 change directory 的缩写，表示切换目录。

```
cd /	--> 跳转到根目录
cd ~	--> 跳转到家目录
cd ..	--> 跳转到上级目录
cd ./home	--> 跳转到当前目录的home目录下
cd /home/lion	--> 跳转到根目录下的home目录下的lion目录
cd	--> 不添加任何参数，也是回到家目录
```



cp

```
cp file file_copy	--> file 是目标文件，file_copy 是拷贝出来的文件
cp file one	--> 把 file 文件拷贝到 one 目录下，并且文件名依然为 file
cp file one/file_copy	--> 把 file 文件拷贝到 one 目录下，文件名为file_copy
cp *.txt folder	--> 把当前目录下所有 txt 文件拷贝到 folder 目录下
```



mv

```
mv file one	--> 将 file 文件移动到 one 目录下
mv new_folder one	--> 将 new_folder 文件夹移动到one目录下
mv *.txt folder	--> 把当前目录下所有 txt 文件移动到 folder 目录下
mv file new_file	--> file 文件重命名为 new_file
```



rm

```
rm new_file 	--> 删除 new_file 文件
rm f1 f2 f3 	--> 同时删除 f1 f2 f3 3个文件
```

- -i 向用户确认是否删除；
- -f 文件强制删除；
- -r 递归删除文件夹，著名的删除操作 rm -rf 。





## 权限管理

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203242153409.awebp)

将文件 file1.txt 设为所有人皆可读取 :

```
chmod a+r file1.txt
```

将文件 file1.txt 与 file2.txt 设为该文件拥有者，与其所属同一个群体者可写入，但其他以外的人则不可写入 :

```
chmod ug+w,o-w file1.txt file2.txt
```

为 ex1.py 文件拥有者增加可执行权限:

```
chmod u+x ex1.py
```



## 网络管理

ipconfig

```
[root@machine1 /sbin]#ifconfig
eth0 Link encap:Ethernet Hwaddr 52:54:AB:DD:6F:61
inet addr:210.34.6.89 Bcast:210.34.6.127 Mask:255.255.255.128
UP BROADCAST RUNNING MULTICAST MTU:1500 Metric:1
RX packets:46299 errors:0 dropped:0 overruns:0 frame:189
TX packets:3057 errors:0 dropped:0 overruns:0 carrier:0
collisions:0 txqueuelen:100
Interrupt:5 Base address:0xece0
lo Link encap:Local Loopback
inet addr:127.0.0.1 Mask:255.0.0.0
UP LOOPBACK RUNNING MTU:3924 Metric:1
RX packets:44 errors:0 dropped:0 overruns:0 frame:0
TX packets:44 errors:0 dropped:0 overruns:0 carrier:0
collisions:0 txqueuelen:0
```

以eth0 为首的部分是本机的以太网卡配置参数，这里显示了网卡在下的设备名/dev/eth0 和硬件的MAC 地址52:54:AB:DD:6F:61，MAC 地址是生产厂家定的，每个网卡拥有的唯一地址。

下一行显示本机的IP 地址信息，分别是本机的IP 地址，网络广播地址和子网掩码。





netstat

netstat -p 可以与其它开关一起使用，就可以添加 “PID/进程名称” 到 netstat 输出中，这样 debugging 的时候可以很方便的发现特定端口运行的程序。

```
netstat -tlnp | grep :22
```



## 防火墙

### ubuntu

由于LInux原始的防火墙工具iptables过于繁琐，所以ubuntu默认提供了一个基于iptable之上的防火墙工具ufw。

ubuntu 9.10默认的便是UFW防火墙，它已经支持界面操作了。

打开或关闭某个端口，例如：

sudo ufw allow smtp　允许所有的外部IP访问本机的25/tcp (smtp)端口

sudo ufw allow 22/tcp 允许所有的外部IP访问本机的22/tcp (ssh)端口

sudo ufw allow 53 允许外部访问53端口(tcp/udp)

sudo ufw allow from 192.168.1.100 允许此IP访问所有的本机端口

sudo ufw allow proto udp 192.168.0.1 port 53 to 192.168.0.2 port 53

sudo ufw deny smtp 禁止外部访问smtp服务

sudo ufw delete allow smtp 删除上面建立的某条规则

一般用户，只需如下设置：

sudo apt-get install ufw

sudo ufw enable

sudo ufw default deny

以上三条命令已经足够安全了，如果你需要开放某些服务，再使用sudo ufw allow开启。





## ssh

如果你只是想登陆别的机器的SSH只需要安装openssh-client（ubuntu有默认安装，如果没有则sudo apt-get install openssh-client），如果要使本机开放SSH服务就需要安装openssh-server。

一般Ubuntu都会默认安装openssh-client,但是没有安装openssh-server。









## vim

### 一、vim模式

```
普通模式（按Esc或Ctrl+[进入） 左下角显示文件名或为空
插入模式（按i进入） 左下角显示--INSERT--
可视模式（按v进入） 左下角显示--VISUAL--
命令行模式（输入:进入） 左下角显示 :
```

普通模式：Vim 强大的编辑功能来自于其普通模式命令。普通模式命令往往需要一个操作符结尾。例如普通模式命令dd删除当前行，但是第一个"d"的后面可以跟另外的移动命令来代替第二个d，比如用移动到下一行的"j"键就可以删除当前行和下一行。另外还可以指定命令重复次数，2dd（重复dd两次），和dj的效果是一样的。用户学习了各种各样的文本间移动／跳转的命令和其他的普通模式的编辑命令，并且能够灵活组合使用的话，能够比那些没有模式的编辑器更加高效地进行文本编辑。

插入模式：在这个模式中，大多数按键都会向文本缓冲中插入文本。大多数新用户希望文本编辑器编辑过程中一直保持这个模式。

可视模式：这个模式与普通模式比较相似。但是移动命令会扩大高亮的文本区域。高亮区域可以是字符、行或者是一块文本。当执行一个非移动命令时，命令会被执行到这块高亮的区域上。Vim 的"文本对象"也能和移动命令一样用在这个模式中。

命令行模式：在命令行模式中可以输入会被解释成并执行的文本。例如执行命令（:键），搜索（/和?键）或者过滤命令（!键）。在命令执行之后，Vim 返回到命令行模式之前的模式，通常是普通模式。

### 二、打开文件

```
# 打开单个文件
vim file    
# 同时打开多个文件
vim file1 file2..  

# 在vim窗口中打开一个新文件
:open [file]       

【举个例子】
# 当前打开1.txt，做了一些编辑没保存
:open!         放弃这些修改，并重新打开未修改的文件

# 当前打开1.txt，做了一些编辑并保存
:open 2.txt    直接退出对1.txt的编辑，直接打开2.txt编辑，省了退出:wq再重新vim 2.txt的步骤


# 打开远程文件，比如ftp或者share folder
:e ftp://192.168.10.76/abc.txt
:e \qadrive\test\1.txt

# 以只读形式打开文件，但是仍然可以使用 :wq! 写入
vim -R file 

# 强制性关闭修改功能，无法使用 :wq! 写入
vim -M file 

```



### 三、插入命令

```
i 在当前位置生前插入
I 在当前行首插入

a 在当前位置后插入
A 在当前行尾插入

o 在当前行之后插入一行
O 在当前行之前插入一行

```



### 四、查找

```
/text 查找text，按n健查找下一个，按N健查找前一个。
?tex  查找text，反向查找，按n健查找下一个，按N健查找前一个。
```

![img](https://raw.githubusercontent.com/a1254898873/images/master/202203242153410.png)



### 五、替换

```
:s/old/new/    用old替换new，替换当前行的第一个匹配
:s/old/new/g   用old替换new，替换当前行的所有匹配

:%s/old/new/   用old替换new，替换所有行的第一个匹配
:%s/old/new/g  用old替换new，替换整个文件的所有匹配

```



### 六、撤销与重做

```
u 撤销（Undo）

U 撤销对整行的操作

Ctrl + r 重做（Redo），即撤销的撤销。
```



### 七、删除

进入普通模式，使用下列命令可以进行文本快速删除：

| 命令   | 说明                       |
| ------ | -------------------------- |
| x      | 删除游标所在的字符         |
| X      | 删除游标所在前一个字符     |
| Delete | 同 x                       |
| dd     | 删除整行                   |
| dw     | 删除一个单词（不适用中文） |
| d$或D  | 删除至行尾                 |
| d^     | 删除至行首                 |
| dG     | 删除到文档结尾处           |
| d1G    | 删至文档首部               |

进入可视模式，输入d删除所选区域内容



### 八、复制

进入可视模式，输入y复制选取区域内容







### 九、退出保存

```
:wq 保存并退出

ZZ 保存并退出

:q! 强制退出并忽略所有更改

:e! 放弃所有修改，并打开原来文件。

ZZ 保存并退出

:sav(eas) new.txt  另存为一个新文件，退出原文件的编辑且不会保存
:f(ile) new.txt    新开一个文件，并不保存，退出原文件的编辑且不会保存

```

