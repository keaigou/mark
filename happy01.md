###Linux历史
&ensp;&ensp;&ensp;&ensp;GNU提供软件/Linux提供内核  
&ensp;&ensp;&ensp;&ensp;IEEE 电气和电子工程师协会定义的POSIX  
&ensp;&ensp;&ensp;&ensp;POSIX Portable Operating System Interface 可移植操作系统接口 定义了操作系统应该为应用程序提供的接口标准  
&ensp;&ensp;&ensp;&ensp;API规范 应用程序接口规范  
&ensp;&ensp;&ensp;&ensp;ABI Application Binary Interface 应用程序二进制接口  
###Linux基础
####1.linux终端
&ensp;&ensp;&ensp;&ensp;root用户：称为超级用户（#）、已接近完整的系统控制 ，“uid”为0,可通过id -u 查看  
&ensp;&ensp;&ensp;&ensp;普通用户:：普通用户（$）“uid”非0，权限有限  
&ensp;&ensp;&ensp;&ensp;自动登陆root用户：  
&ensp;&ensp;&ensp;&ensp;编辑`/etc/gdm/custom.conf`  
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`AutomaticLoginEnable=true`   
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`AutomaticLogin=root`  
&ensp;&ensp;&ensp;&ensp;图形终端：属于虚拟终端 startx, xwindows（/dev/tty）  
&ensp;&ensp;&ensp;&ensp;模拟终端：图形界面打开的命令行以及基于ssh协议等远程打开的界面（/dev/pts/#）  
&ensp;&ensp;&ensp;&ensp;切换终端   ctrl+alt+fn  n=1-6  
&ensp;&ensp;&ensp;&ensp;交互式接口  
       &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;GUI：Graphic User Interface（图形界面）  
       &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;三种主流桌面Desktop:  
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;GNOME (C, 图形库gtk)，  
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;KDE (C++,图形库qt)  
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;XFCE (轻量级桌面)  
&ensp;&ensp;&ensp;&ensp;相互间应用程序不兼容，因为底层开发库不同  
&ensp;&ensp;&ensp;&ensp;CLI：Command Line Interface（命令行）   	
####2.字符集和编码  
&ensp;&ensp;&ensp;&ensp;ASCII码： ASCII 码一共规定了128个字符的编码，占用了一个字节的后面7位，最前面的 一位统一规定为0  
&ensp;&ensp;&ensp;&ensp;Unicode：用于表示世界上所有语言中的所有字符。每一个符号都给予一个独 一无二的编码数字，Unicode 是一个很大的集合，现在的规模可以容纳100多 万个符号。Unicode 仅仅只是一个字符集，规定了每个字符对应的二进制代码， 至于这个二进制代码如何存储则没有规定  
&ensp;&ensp;&ensp;&ensp;Unicode编码方案：   
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;UTF-8：变长，1到4个字节   
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;UTF-16：变长，2或4个字节   
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;UTF-32：固定长度，4个字节  
&ensp;&ensp;&ensp;&ensp;UTF-8 ：是目前互联网上使用最广泛的一种 Unicode 编码方式，可变长存储。使 用 1 - 4 个字节表示一个字符，根据字符的不同变换长度  
####3.shell
&ensp;&ensp;&ensp;&ensp;shell: Linux系统的用户界面，提供了用户与内核进行交互操作的一种接口。 它接收用户输入的命令并把它送入内核去执行。sh、csh、tcsh、ksh、bash、zsh，默认使用bash   
&ensp;&ensp;&ensp;&ensp;`echo $SHELL` 	&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;查看当前使用的shell  
&ensp;&ensp;&ensp;&ensp;`cat /etc/shells`    &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;查看Linux可用的shell  
&ensp;&ensp;&ensp;&ensp;`echo $PS1`	   &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp; 显示提示符格式  
&ensp;&ensp;&ensp;&ensp;修改提示符格式  
&ensp;&ensp;&ensp;&ensp;`PS1="\[\e[1;5;41;33m\][\u@\h \W]\\$\[\e[0m\]"`  
&ensp;&ensp;&ensp;&ensp;\e \033 \u 当前用户 &ensp;&ensp;&ensp;&ensp;    \h 主机名简称     &ensp;&ensp;&ensp;&ensp;  \H 主机名   
&ensp;&ensp;&ensp;&ensp;\w 当前工作目录       &ensp;&ensp;&ensp;&ensp;  \W 当前工作目录基名 &ensp;&ensp;&ensp;&ensp; \t  24小时时间格式   
&ensp;&ensp;&ensp;&ensp;\T  12小时时间格式   &ensp;&ensp;&ensp;&ensp;  \! 命令历史数   &ensp;&ensp;&ensp;&ensp;     \# 开机后命令历史数  
&ensp;&ensp;&ensp;&ensp;0m 颜色结束  &ensp;&ensp;&ensp;&ensp; 字体颜色31-37 &ensp;&ensp;&ensp;&ensp; 背景颜色41-47 &ensp;&ensp;&ensp;&ensp;  闪烁5 &ensp;&ensp;&ensp;&ensp; 高显1  
&ensp;&ensp;&ensp;&ensp; 保存于 `.bashrc`或`/etc/bashrc`  
&ensp;&ensp;&ensp;&ensp;修改登录信息	
	&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`/etc/issue`  
&ensp;&ensp;&ensp;&ensp;d 本地端时间的日期；  
&ensp;&ensp;&ensp;&ensp;l 显示第几个终端机介面  
&ensp;&ensp;&ensp;&ensp;m 显示硬体的等级 (i386/i486/i586/i686...)；  
&ensp;&ensp;&ensp;&ensp;n 显示主机的网路名称；  
&ensp;&ensp;&ensp;&ensp;O 显示 domain name；  
&ensp;&ensp;&ensp;&ensp;r 作业系统的版本 (相当于 uname -r)  
&ensp;&ensp;&ensp;&ensp;t 显示本地端时间的时间；  
&ensp;&ensp;&ensp;&ensp;S 作业系统的名称；  
&ensp;&ensp;&ensp;&ensp;v 作业系统的版本  
&ensp;&ensp;&ensp;&ensp;登陆显示信息  
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`cat /etc/motd`      
####4.内部命令和外部命令
&ensp;&ensp;&ensp;&ensp;alias ---- 内部 --hash表（记录外部命令的路径）---$PATH --命令找不到  
&ensp;&ensp;内部命令 shell程序自带的命令  	
&ensp;&ensp;&ensp;&ensp;help 内部命令列表   
&ensp;&ensp;&ensp;&ensp;enable cmd 启用内部命令    
&ensp;&ensp;&ensp;&ensp;enable –n cmd 禁用内部命令   
&ensp;&ensp;&ensp;&ensp;enable –n 查看所有禁用的内部命令  
&ensp;&ensp;外部命令 在系统的某个路径下的可执行程序  
&ensp;&ensp;&ensp;&ensp;外部命令查找依赖于 PATH变量   
&ensp;&ensp;&ensp;&ensp;查看外部命令搜索路径 查看PATH变量  
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;`echo $PATH`  
&ensp;&ensp;&ensp;`type pwd`&ensp;&ensp;&ensp;&ensp;查看pwd是内部命令还是外部命令  
&ensp;&ensp;&ensp;&ensp;`which`&ensp;&ensp;&ensp;&ensp;命令查看命令所在的目录  
&ensp;&ensp;&ensp;&ensp;Hash缓存表   
&ensp;&ensp;&ensp;&ensp;系统初始hash表为空，当外部命令执行时，默认会从PATH路径下寻找该命 令，找到后会将这条命令的路径记录到hash表中，当再次使用该命令时，shell解 释器首先会查看hash表，存在将执行之，如果不存在，将会去PATH路径下寻找。 利用hash缓存表可大大提高命令的调用速率   
&ensp;&ensp;&ensp;&ensp;hash    显示hash缓存   
&ensp;&ensp;&ensp;&ensp;`hash –l`&ensp;&ensp;&ensp;&ensp; 显示hash缓存，可作为输入使用   
&ensp;&ensp;&ensp;&ensp;`hash –p path  name` &ensp;&ensp;&ensp;&ensp;将命令全路径path起别名为name   
&ensp;&ensp;&ensp;&ensp;`hash –t name`&ensp;&ensp;&ensp;&ensp; 打印缓存中name的路径   
&ensp;&ensp;&ensp;&ensp;`hash –d name`&ensp;&ensp;&ensp;&ensp; 清除name缓存   
&ensp;&ensp;&ensp;&ensp;`hash –r`&ensp;&ensp;&ensp;&ensp; 清除缓存  
缓存cache：将刚用硬盘的数据放在内存中，下次用此数据，不需要从硬盘找，直接内存取出hash  
####5. 命令别名 
&ensp;&ensp;&ensp;&ensp;alias&ensp;&ensp;&ensp;&ensp;显示当前shell进程所有可用的命令别名  
    &ensp;&ensp;&ensp;&ensp;`unalias [-a] name [name ...]` &ensp;&ensp;&ensp;&ensp; 撤消别名  
    &ensp;&ensp;&ensp;&ensp;`alias NAME='VALUE'`&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;   定义别名NAME  
   &ensp;&ensp;&ensp;&ensp;在命令行中定义的别名，仅对当前shell进程有效   
&ensp;&ensp;&ensp;&ensp;如果想永久有效，要定义在配置文件中   
&ensp;&ensp;&ensp;&ensp;仅对当前用户：&ensp;&ensp;&ensp;&ensp;`~/.bashrc`   
&ensp;&ensp;&ensp;&ensp;对所有用户有效&ensp;&ensp;&ensp;&ensp;：`/etc/bashrc`  
####6. 命令格式  
&ensp;&ensp;&ensp;&ensp;COMMAND [OPTIONS...] [ARGUMENTS...]   
&ensp;&ensp;&ensp;&ensp;选项：用于启用或关闭命令的某个或某些功能   
&ensp;&ensp;&ensp;&ensp;短选项：c  &ensp;&ensp;&ensp;&ensp;例如：-l, -h   
&ensp;&ensp;&ensp;长选项：--word &ensp;&ensp;&ensp;&ensp;例如：--all, --human-readable   
&ensp;&ensp;&ensp;&ensp;参数：命令的作用对象，比如文件名，用户名等   
&ensp;&ensp;&ensp;&ensp;注意：   
&ensp;&ensp;&ensp;&ensp;多个选项以及多参数和命令之间使用空白字符分隔   
&ensp;&ensp;&ensp;&ensp;取消和结束命令执行：`Ctrl+c`，`Ctrl+d`   
&ensp;&ensp;&ensp;&ensp;多个命令可以用;符号分开   
&ensp;&ensp;&ensp;&ensp;一个命令可以用\分成多行  
