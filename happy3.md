# 用户组和权限管理

## 1.安全模型

        Authentication  认证

        Authorization        授权

        Accouting | Auditin  审计

## 2.用户user

1. 1)每个用户都有一个唯一uid
2. 2)user信息存放在 /etc/passwd  密码信息存放在 /etc/shadow
3. 3)user都一个家目录 /home/
4. 4)管理员 root ：0

普通用户 1000+

## 3.用户组 group

1. 1)管理员组 ：root 0
2. 2)普通组：gid 1000+
3. 3)优先使用基本组 用户只能属于一个基本组 用户的默认组
4. 4)基本组不能满足授权要求了 创建附加组 用户可以属于零个或多个附加组
5. 5)私有组 创建用户时如果没有指定基本组 系统会创建和用户同名的一个组
6. 6)Group信息存储在/etc/group，/ect/gshadow存放Group的密码，极少情况下为Group设密码

/etc/group        (users::20:root,sam                组名:口令:组标识号:组内用户列表)

## 4.用户工作环境

交互式shell    等待用户输入 执行提交的命令  exit

        首先读取/etc/profile--\&gt;/etc/profile.d/\*.sh--\&gt;~/.bash\_profile--\&gt;~/.bashrc--\&gt;/etc/bashrc

非交互式shell  执行shell脚本 脚本执行结束shell退出

首先读取/etc/profile--\&gt;/etc/profile.d/\*.sh--\&gt;~/.bash\_profile--\&gt;~/.bashrc--\&gt;/etc/bashrc

Bash的配置文件保存用户的工作环境

个人配置文件

~/.bash\_profile  ~/.bashrc

全局配置文件

/etc/profile   /etc/profile.d/\*.sh  /etc/bashrc

profile类的文件  设定环境变量 登录前运行的脚本和命令

bashrc 类的文件  设定本地变量 定义命令别名

注意：        全局配置和个人配置设置冲突 以个人设置为准

## 5.密码策略

1. 1)查看用户密码加密类型 /etc/login.defs   md5 128位
2. 2)修改加密方式不需要之前密码加密方式
3. 3)更改加密算法：

authconfig --passalgo=sha256 –update

1. 4)显示passwd密码        pwunconv
2. 5)隐藏passwd密码        pwconv

明文加密 grub2-mkpasswd-pbkdf2

## 产生随机数

        openssl rand -base64 10

        echo | openssl passwd -stdin

        head -c 32 /dev/random | base64

        head -c 32 /dev/urandom | base64

        echo $RANDOM

## 6.管理用户命令

passwd文件结构

root:x:0:0:root:/root:/bin/bash

        用户名:口令:用户标识号:组标识号:注释性描述:主目录:登录Shell

shadow文件格式

tom:$6$7I175sndgdfg:17800:0:99999:7:::

登录名:加密口令:最后一次修改时间:最小使用间隔:最大有效间隔:警告时间:不活动时间:失效时间:标志

用户过期时间（MD5 $1;sha512 $6）

 ![](data:image/*;base64,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)

passwd

        -d        删除用户密码

        -l        锁定用户

        -u        解锁用户

        -e        强制用户下次登陆更改密码

        -S        显示用户密码锁定状态，及加密算法

        -n mindays：指定最短使用期限

-x maxdays：最大使用期限

-w warndays：提前多少天开始警告

-i inactivedays：非活动期限

        --stdin        标准输入修改用户密码，如echo &quot;New&quot; | passwd --stdin Username

useradd                /etc/default/useradd

        -r 创建系统账户

        -N        不创建与用户同名的基本用户组

usermod

-c        填写用户账户的备注信息

-d -m        参数-m与参数-d连用，可重新指定用户的家目录并自动把旧的数据转移过去

-e        账户的到期时间，格式为YYYY-MM-DD

-g        变更所属用户组

-G        变更扩展用户组

-L        锁定用户禁止其登录系统

-U        解锁用户，允许其登录系统

-s        变更默认终端

-u        修改用户的UID

-f INACTIVE: 设定非活动期限

userdel

chage

        d 设置最近一次更改密码时间

-E 设置账户过期时间

        -I 设置密码过期时间

-l --list 列出用户账户密码信息

-m 设置用户最短密码使用时间

-M 设置用户最常密码使用时间

-W 设置密码更改警告时间

chsh                        改变登陆shell

chfn                        改变描述信息

vipw                        直接打开/etc/passwd文件

finger                        查看用户描述信息

pwck                        检查用户完整性

newusers                批量创建用户

chpasswd                批量创建用户密码

        批量创建用户

        user001:x:600:100:user:/home/user001:/bin/bash

        1.newusers \&lt; user.txt

        2.pwunconv         将shadow密码解码，回写到/etc/passwd中，将shadow中密码删掉

        3.chpasswd \&lt; passwd.txt

4.pwconv                将密码编码到shadow中

## 7.管理组命令

vigr                直接打开/etc/group文件

grpck                检查组完整性

groupmems

-a，：指定用户加入组

-g，：更改为指定GID

-d，：从组中删除用户

-l：显示组成员列表

groupadd

        -g        指明GID

        -r        创建系统组

groupdel

groups                        显示组

newgrp                        切换新组

gpasswd

-a ：将用户添加至组中

-d ：将用户从组中删除

-A ：设置组的管理员

groupmod

        -n：修改组名

-g：新的组ID
## 8.权限管理

1. 1)对于目录来说：

&quot;r&quot;表示能够读取目录内的文件列表；&quot;w&quot;表示能够在目录内新增、删除、重命名文件；而&quot;x&quot;则表示能够进入该目录，读取文件数据，&quot;X&quot;只给目录加执行权限

        umask

新建FILE权限: 666-umask 如果所得结果某位存在执行（奇数）权限，则将其权限+1

umask -S 显示权限列表

umask -P 输出

全局设置        /etc/bashrc  用户设置 ~/.bashrc

1. 2)特殊权限

        SUID

                让二进制程序的执行者临时拥有属主的权限

chmod u+s /bin/passwd

SGID

让执行者临时拥有属组的权限（对拥有执行权限的二进制程序进行设置）；

在某个目录中创建的文件自动继承该目录的用户组（只可以对目录进行设置）

chmod g+s

SBIT  粘滞位

除了管理员外，可确保用户只能删除自己的文件

o+t

1. 3)文件的隐藏属性

        chattr

i        无法对文件进行修改；若对目录设置了该参数，则仅能修改其中的子文件内容而不能新建或删除文件

a        仅允许补充（追加）内容，无法覆盖/删除内容（Append Only）

S        文件内容在变更后立即同步到硬盘（sync）

s        彻底从硬盘中删除，不可恢复（用0填充原文件所在硬盘区域）

A        不再修改这个文件或目录的最后访问时间（atime）

b        不再修改文件或目录的存取时间

D        检查压缩文件中的错误

d        使用dump命令备份时忽略本文件/目录

c        默认将文件或目录进行压缩

u        当删除该文件后依然保留其在硬盘中的数据，方便日后恢复

t        让文件系统支持尾部合并（tail-merging）

x        可以直接访问压缩文件中的内容

lsattr                显示文件的隐藏权限

1. 4)文件访问控制列表

                基于普通文件或目录设置ACL其实就是针对指定的用户或用户组设置文件或目录的操作权限。

        对某个目录设置了ACL，则目录中的文件会继承其ACL；对文件设置ACL，则文件不继承其所在目录的ACL

                最后一个点（.）变成了加号（+）

                Mask需要与用户的权限进行逻辑与运算后，才能变成有限的权限

                可以将mask值理解为最高权限，除了所有者，Other之外，其他的都在mask 的管辖之内

                通过ACL赋予目录默认x权限，目录内文件也不会继承x权限

                base ACL 不能删除

                用户或组的设置必须存在于mask权限设定范围内才会生效

                setfacl -m mask::rx file

        setfacl

        -b         删除所有ACL

        -k        删除默认ACL

-d  设定默认的acl规则

        getfacl                查看ACL权限

1. 5)备份和恢复ACL

主要的文件操作命令cp和mv都支持ACL，只是cp命令需要加上-p 参数。但是 tar等常见的备份工具是不会保留目录和文件的ACL信息

                CentOS7 默认创建的xfs和ext4文件系统具有ACL功能

                CentOS7 之前版本，默认手工创建的ext4文件系统无ACL功能,需手动增加

                        tune2fs –o acl /dev/sdb1

mount –o acl /dev/sdb1  /mnt/test

1. 6)su命令与sudo服务

        sudo

限制用户执行指定的命令：

记录用户执行的每一条命令；

配置文件（/etc/sudoers）提供集中的用户管理、权限与主机等参数；

验证密码的后5分钟内（默认值）无须再让用户再次验证密码

visudo

配置用户权限时将禁止多个用户同时修改sudoers配置文件，还可以对配置文件内的参数进行语法检查，并在发现参数错误时进行报错

                只有root管理员才可以使用visudo命令编辑sudo服务的配置文件

                linuxprobe ALL=(ALL) ALL

                谁可以使用  允许使用的主机=（以谁的身份）  可执行命令的列表

                添加NOPASSWD参数，使得用户执行sudo命令时不再需要密码验证

                        linuxprobe ALL=NOPASSWD: /usr/sbin/poweroff
