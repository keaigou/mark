# 文件管理

## 1.文件系统

普通文件(f) **：** C语言元代码、SHELL脚本、二进制的可执行文件等。分为纯文本和二进制。

目录文件(d) **：** 目录，存储文件的唯一地方。

链接文件(l) **：** 指向同一个文件或目录的的文件。

设备文件 **：** 分为块设备(b)和字符设备(c)。

管道文件(p) **:**  提供进程之间通信的一种方式

套接字(socket) 文件 **：**  该文件类型与网络通信有关

可以通过ls –l, file, stat几个命令来查看文件的类型等相关信息

文件有两类数据：

元数据：metadata   数据的属性、inode信息

数据：data

文件系统分层结构：LSB  Linux Standard Base

inode：文件系统索引,记录文件的属性。

它是文件系统的最基本单元，是文件系统连接任何子目录、任何文件的桥梁。每个子目录和文件只有唯一的一个 inode 块。

它包含了文件系统中文件的基本属性(文件的长度、创建及修改时间、权限、所属关系)、存放数据的位置等相关信息.,一般是128字节或256字节

硬连接和源文件具有相同的 inode

mv或rename文件，只是改变文件名，不影响inode号码

更后文件同样的文件名，生成新的inode，下次运行，文件名就自动指向新文件，旧文件的inode被回收

超级块(Superblock): 这是整个文件系统的第一块空间。包括整个文件系统的基本信息，如块大小，inode/block的总量、使用量、剩余量，指向空间 inode 和数据块的指针等相关信息

一个文件由一个超级块、inode和数据区域块组成。Inode包含文件的属性(如读写属性、owner等，以及指向数据块的指针)，数据区域块则是文件内容。当查看某个文件时，会先从inode table中查出文件属性及数据存放点，再从数据块中读取数据。

硬连接 **：** 原文件名和连接文件名都指向相同的物理地址。

                目录不能有硬连接；

硬连接不能跨越文件系统（不能跨越不同的分区）

文件在磁盘中只有一个拷贝，节省硬盘空间；新旧文件的inode编号一致

由于删除文件要在同一个索引节点属于唯一的连接时才能成功，因此可以防止不必要的误删除。

软连接 **：** 用ln -s命令建立文件的符号连接作为一个文件，它的数据是它所连接的文件的路径名。类似windows下的快捷方式。

        原始文件一般路径用相对路径，相对路径一定相对于软链接文件的路径

新旧文件的inode编号不一致

可以删除原有的文件而保存连接文件，没有防止误删除功能。

任何目录&quot;硬链接&quot;总数等于2加上它的子目录总数（含隐藏目录）,2是父目录对其的&quot;硬链接&quot;和当前目录下的&quot;.硬链接&quot;

硬链接和软链接区别

1 同一个文件？

        2 跨分区？

3 链接数增长？

4 inode Number 是否相同？

5 原始文件删除，链接文件可否访问？

6 大小？

7 支持目录？

8 相对路径？

文件名最长255个字节  包括路径在内文件名称最长4095个字节

蓝色--\&gt;目录 绿色--\&gt;可执行文件 红色--\&gt;压缩文件 浅蓝色--\&gt;链接文 件 灰色--\&gt;其他文件

## 2.linux主要目录

        /boot：引导文件存放目录，内核文件(vmlinuz)、引导加载器(bootloader, grub)都存放于此目录

/bin：供所有用户使用的基本命令；不能关联至独立分区，OS启动即会用到的 程序

/sbin：管理类的基本命令；不能关联至独立分区，OS启动即会用到的程序

/lib：启动时程序依赖的基本共享库文件以及内核模块文件(/lib/modules

/lib64：专用于x86\_64系统上的辅助共享库文件存放位置

/etc：配置文件目录

/home/USERNAME：普通用户家目录

/root：管理员的家目录

/media：便携式移动设备挂载点

/mnt：临时文件系统挂载点

/dev：设备文件及特殊文件存储位置

                b: block device，随机访问

c: character device，线性访问

# /opt：第三方应用程序的安装位置

# /srv：系统上运行的服务用到的数据

# /tmp：临时文件存储位置

/proc: 用于输出内核与进程信息相关的虚拟文件系统

/sys：用于输出当前系统上硬件设备相关信息虚拟文件系统

/var: variable data files

cache: 应用程序缓存数据目录

lib: 应用程序状态信息数据

local：专用于为/usr/local下的应用程序存储可变数据；

lock: 锁文件

log: 日志目录及文件

opt: 专用于为/opt下的应用程序存储可变数据；

run: 运行中的进程相关数据,通常用于存储进程pid文件

spool: 应用程序数据池

tmp: 保存系统两次重启之间产生的临时数据

/usr: universal shared, read-only data

bin: 保证系统拥有完整功能而提供的应用程序

sbin:超级用户的一些管理程序

lib： 32位使用

lib64：只存在64位系统

include: C程序的头文件(header files)

share：结构化独立的数据，例如doc, man等

local：第三方应用程序的安装位置

bin, sbin, lib, lib64, etc, share

基名：basename

目录名：dirname

## 3.文件通配符

        \* 匹配零个或多个字符

? 匹配任何单个字符

～当前用户家目录 

～mage 用户mage家目录 

～+ 当前工作目录 

～- 前一个工作目录

[0-9] 匹配数字范围 

[a-z]：字母 

[A-Z]：字母 

[wang] 匹配列表中的任何的一个字符 

[^wang] 匹配列表中的所有字符以外的字符

[:digit:]：任意数字，相当于0-9

[:lower:]：任意小写字母

[:upper:]: 任意大写字母

[:alpha:]: 任意大小写字母

[:alnum:]：任意数字或字母

[:blank:]：水平空白字符

[:space:]：水平或垂直空白字符

[:punct:]：标点符号

[:print:]：可打印字符

[:cntrl:]：控制（非打印）字符

[:graph:]：图形字符

[:xdigit:]：十六进制字符

## 4.管道重定向

管道技术

1.在管道后面的命令 都不应该再跟文件名

2.在管道中只有标准输出才传递给下一个命令 标准错误输出直接输出到终端

可以把标准错误输出给重定向

find /etc -name &quot;\*.conf&quot; 2\&gt; /dev/null | grep rc

3.有些命令不支持管道技术

xargs让ls支持管道技术

[hanlihui@WebServer ~]$ which cat | xargs ls -l

ls | tee [-a] 1.txt| tr   把命令1保存到文件并显示，输入给命令2

重定向

标准输入（STDIN）－0 默认接受来自键盘的输入

标准输出（STDOUT）－1 默认输出到终端窗口

标准错误（STDERR）－2 默认输出到终端窗口

\&gt; 文件内容会被覆盖

                set –C  禁止将内容覆盖已有文件,但可追加

\&gt;| file 强制覆盖

                set +C  允许覆盖

     \&lt;\&lt;终止词        从键盘输入 称为就地文本（heretext）

## 目录管理

### cd

cd ..                切换至上级目录

cd                切换至当前用户主目录

cd -                切换至以前的工作目录

-P                物理真实路径

当前路径和上一个路径保存在这两个变量中

[root@WebServer ~]# echo $PWD

[root@WebServer ~]# echo $OLDPWD

/home/hanlihui

/root返回到其他用户的主目录

[root@WebServer ~]# cd ~han

### ls

用法： ls [options] [files\_or\_dirs]

list 列出文件夹中的内容 文件 文件夹

-a 列出目录中全部文件和目录 包括隐藏文件

-l 长格式显示

-R 递归显示

-d 只显示目录本身一些属性

–S 按从大到小排序

–t 按mtime排序

–u 配合-t选项，显示并按atime从新到旧排序

–U 按目录存放顺序显示

–X 按文件后缀排序

-r 逆序排序输出

### mkdir

-p, --parents         需要时创建目标目录的上层目录，

-v, --verbose         每次创建新目录都显示信息

### rmdir

只能删除空文件夹

-p, --parents         删除指定目录及其上级文件夹

-v, --verbose         输出处理的目录详情

### tree

-d: 只显示目录

-L level：指定显示的层级数目

-P pattern: 只显示由指定pattern匹配到的路径
## 文件管理

### cp

-a：此参数的效果和同时指定&quot;-dpR&quot;参数相同；

-d：当复制符号连接时，把目标文件或目录也建立为符号连接，并指向与源文件或目录连接的原始文件或目录；

-f：强行复制文件或目录，不论目标文件或目录是否已存在；

-i：覆盖既有文件之前先询问用户；

-l：对源文件建立硬连接，而非复制文件；

-p：保留源文件或目录的属性；

-r：递归处理，将指定目录下的所有文件与子目录一并处理；

-s：对源文件建立符号连接，而非复制文件；

-u：只复制源比目标更新文件或目标不存在的文件

-b：目标存在，覆盖前先备份

### rm

        -i 交互式

-f 强制删除

-r 递归

rm -- -a == rm /data/-a

### mv

        -i: 交互式

-f: 强制

-b: 目标存在，覆盖前先备份

### touch

-a 仅改变 atime和ctime

-m  仅改变 mtime和ctime

-t  [[CC]YY]MMDDhhmm[.ss] 指定atime和mtime的时间戳

-c 如果文件不存在，则不予创建

touch -- -a == touch /data/-a    touch ~a

### file

        -b 列出文件辨识结果时，不显示文件名称

-f filelist 列出文件filelist中文件名的文件类型

-F 使用指定分隔符号替换输出文件名后默认的&quot;:&quot;分隔符

-L 查看对应软链接对应文件的文件类型

### stat

### lsof  用于查看你进程开打的文件，打开文件的进程，进程打开的端口

## 文本查看

### cat

        -E        显示行结束符$

        -n        对每行进行

        -A        显示所有控制符

        -b        非空行编号

        -s        压缩连续的空行为一行

rev                每行反向输出

### tac

### more

### less        一页一页地查看输入

### head        前面部分，默认前10行

        -c        获取前#字节

        -n        获取前#行

### tail   后面部分

        -c        获取#字节

        -f        跟踪显示文件内容

        -F        以文件名跟踪显示文件内容

        tailf 类似 tail -f 文件不增长不访问文件

### locate

        非实时查找(数据库查找)  updatedb 更新数据库

        -i 不区分大小写的搜索

-n  N 只列举前N个匹配项目

-r  使用正则表达式

### find

查找条件：

1. 1)指搜索层级：

        -maxdepth # 最大搜索目录深度#级

-mindepth level 最小搜索目录深度#级

        先处理目录内的文件，再处理目录 -depth

1. 2)根据文件名和inode查找

-name &quot;文件名称&quot;：支持使用glob \*, ?, [], [^]

-iname &quot;文件名称&quot;：不区分字母大小写

-inum n  按inode号查找

-samefile name  相同inode号的文件

-links n   链接数为n的文件

-regex &quot;PATTERN&quot;：以PATTERN匹配整个文件路径，而非文件名称

1. 3)根据属主、属组查找

-user USERNAME：查找属主为指定用户(UID)的文件

-group GRPNAME: 查找属组为指定组(GID)的文件

-uid UserID：查找属主为指定的UID号的文件

-gid GroupID：查找属组为指定的GID号的文件

-nouser：查找没有属主的文件

-nogroup：查找没有属组的文件

1. 4)根据文件类型查

-type TYPE:文件类型

-empty 空文件或目录        find /app -type d  -empty

与：-a 或：-o 非：-not, !

示例：

!A -a !B = !(A -o B)

!A -o !B = !(A -a B)

find / -user joe -o -uid 500

find /tmp -not \( -user root -o -name &#39;f\*&#39; \)

1. 5)根据文件大小来查

        -size

1. 6)根据时间

以&quot;天&quot;为单位

-atime   #: [#,#+1) +#: [#+1,∞] -#: [0,#)

-mtime

-ctime

以&quot;分钟&quot;为单位 -amin -mmin -cmin

1. 7)根据权限查

        -perm # 精确匹配

                        /# 任何一个匹配

                        -#：每一类对象都必须同时拥有指定权限，与关系 0 表示不关注

     find -perm 755 会匹配权限模式恰好是755的文件 •

只有当每个人都有写权限时，find -perm -222才会匹配

只有当其它人（other）有写权限时，find -perm -002才会匹配

1. 8)处理动作

  -print：默认的处理动作，显示至屏幕

-ls：类似于对查找到的文件执行&quot;ls -l&quot;命令

-delete：删除查找到的文件

-fls file：查找到的所有文件的长格式信息保存至指定文件中

-ok CMD {} \;要求用户确认

-exec CMD {} \; 不用确认

## 文本编辑

nano

cut         剪切字节、字符和字段

        -b ：以字节为单位进行分割。这些字节位置将忽略多字节字符边界，除非也指定了 -n 标志。

-c ：以字符为单位进行分割。

-d ：自定义分隔符，默认为制表符。

-f ：与-d一起使用，指定显示哪个区域。

-n ：取消分割多字节字符。仅和 -b 标志一起使用。

paste        合并两个文件同行到一列

        -d        分隔符：默认tab

        -s        所有行合成一行

tr

        tr [OPTION]... SET1 [SET2]

-c：反选替换设定字符。

-d：删除指令字符

-s：缩减连续重复的字符成指定的单个字符

-t：SET1 与 SET2 范围对应

wc

        -c 只显示Bytes数。

-l 只显示行数。

        -w 只显示字数

        -m        只显示字符数

        -L        显示文件最长行长度

strings        查看文件中的可打印的字符

sort

        -r        反向排序

        -R        随机排序

        -n        按数字大小整理

        -u         删除输出中的重复行

        -f 忽略字符串的字符大小写

        -t c使用c字符做字段界定符

        -k x 按照c字符分割的x列整理

uniq        从输入中删除前后相接重复行

        -c        显示每行重复出现的次数

        -d         仅显示重复行

        -u        仅显示不重复行

diff

        -u        输出&quot;统一的（unified）&quot;diff格式文件，最适用于补丁文件

### grep

        -v         取反

        -I        忽略字符大小写

        -n        显示匹配的行号

        -c        统计匹配的行数

        -o        仅显示匹配字符串

        -q        静默，不输出任何信息

        -A \* 后\*行

        -B \* 前\*行

        -C \* 前后各\*行

        -e         多个选项or关系

        -w        匹配整个单词

        -f        模式文件处理 交集

正则表达式

^         行首

$         行尾

\\&lt; 或 \b         词首锚定，用于单词模式的左侧

\\&gt; 或 \b         词尾锚定，用于单词模式的右侧

.                 任意单一字符

[]                 []内任意单一字符

[^]         除[]内任意单一字符

\*                 \*前面字符重复不确定次数

\+                 \+前面字符重复一次以上不确定次数

\?                 ？前面字符重复0或1次

\                 转义符

.\*                 任意长度字符

\{n\}        前面字符重复n次

\{n,\}         前面字符重复n次以上

\{m,n\} 前面字符重复m次和n次之间

分组：\(\) 将一个或多个字符捆绑在一起，当作一个整体处理

### sed

1. 1)常用选项：

-n 不输出模式空间内容到屏幕，即不自动打印

-e 多点编辑

-f  /PATH/SCRIPT\_FILE 从指定文件中读取编辑脚本

-r 支持使用扩展正则表达式

-i.bak 备份文件并原处编辑

1. 2)编辑命令

        d 删除模式空间匹配的行，并立即启用下一轮循环

p 打印当前模式空间内容，追加到默认输出之后

a [\]text 在指定行后面追加文本，支持使用\n实现多行追加

i [\]text 在行前面插入文本

c [\]text 替换行为单行或多行文本

w /path/file 保存模式匹配的行至指定文件

r /path/file 读取指定文件的文本至模式空间中匹配到的行后

= 为模式空间中的行打印行号

! 模式空间中匹配行取反处理

1. 3)高级编辑命令

        模式空间：pattern space临时缓冲区，内容会主动打印到标准输出，并自动清空，相当于流水线

        保持空间：hold space 辅助临时缓冲区,相互独立，内容不会主动清空，也不会主动打印到标准输出

                                需要sed命令来进行处理，相当于仓库

P： 打印模式空间开端至\n内容，并追加到默认输出之前

d D: 删除pattern space的内容/及第一个换行符，开始下一个循环

h H: 复制/追加pattern space的内容到hold space.（复制会覆盖原内容）

g G: 复制/追加hold space的内容到pattern space.复制会覆盖原内容）

x : 交换hold space和pattern space的内容

awk

### vim

        vim -c cmd file: 在打开文件前，先执行指定的命令；

vim -r file: 恢复上次异常退出的文件；

        vim -o file1 file2:水平分割窗口

vim -O file1 file2:垂直分割窗口

        vim -R file: 以只读的方式打开文件，但可以强制保存；

        vim -M file: 以只读的方式打开文件，不可以强制保存；

vim -y num file: 将编辑窗口的大小设为num行；

vim + file: 从文件的末尾开始；

vim +num file: 从第num行开始；

        vim +/string file: 打开file，并将光标停留在第一个找到的string上

1. 正常模式：可以使用快捷键命令

                :r file 读文件内容到当前文件

        :w  file 将当前文件内容写入另一个文件

        :! cmd 执行命令

        : r! cmd 将命令的输出到文件内容

        ：sp 文件水平分割窗口

1. 插入模式：

i 在光标所在处输入

I 在当前光标所在行的行首输入

a 在光标所在处后面输入

A 在当前光标所在行的行尾输入

o 在当前光标所在行的下方打开一个新行

O 在当前光标所在行的上方打开一个新行

1. 可视模式：正常模式下按v总是整行整行的选中。ctrl+v进入可视块模式。
2. 替换模式：正常模式下，按R进入
3. 光标的移动

        h:左  j:下  k:上 l:右  M：页中间 b: 后移

        ^: 跳转至行首的第一个非空白字符

0: 跳转至行首

$: 跳转至行尾

        G：最后一行

gg: 第一行

        w ：到下一个单词开头

e：到下一个单词结尾

1. 翻屏

ctrl+f: 下翻一屏

ctrl+b: 上翻一屏

ctrl+d: 下翻半屏

ctrl+u: 上翻半屏

ctrl+e: 向下滚动一行

ctrl+y: 向上滚动一行

1. 编辑

        x: 删除光标处的字符

        #x: 删除光标处起始的#个字符

xp: 交换光标所在处的字符及其后面字符的位置

~:转换大小写

J:删除当前行后的换行符

d: 删除命令

d$: 删除到行尾

d^:删除到非空行首

d0:删除到行首

D：从当前光标位置一直删除到行尾，等同于d$

y: 复制

c[$ ^ 0 b e w c]: 改写插入

[n]s:删除光标开始1(n)个字符并插入

0y$ 0d$

1. 查找

        # 具体第#行，例如2表示第2行

#,# 从左侧#表示起始行，到右侧#表示结尾行

#,+#  从左侧#表示的起始行，加上右侧#表示的行数  2,+3  表示2到5行

.   当前行

$  最后一行

.,$-1 当前行到倒数第二行

%  全文, 相当于1,$

/pat1/,/pat2/ 从pat1匹配pat2匹配的行结束

2,/pat/ /pat/,$

使用方式：后跟一个编辑命令

d

y

s

w file: 将范围内的行另存至指定文件中

r  file：在指定位置插入指定文件中的所有内容

/#：从当前光标向尾部查找

?#：从当前光标向首部查找

n：向下查找

N：向上查找

1. 替换

        gU (变大写)  gU3e gU3w

gu (变小写)  gu3e gu3w

1. 窗口

Ctrl+w,s: split, 水平分割

Ctrl+w,v: vertical, 垂直分割

ctrl+w,q：取消相邻窗口

ctrl+w,o:取消全部窗口

：wqall 退出

1. 配置文件

        全局：/etc/vimrc

个人：~/.vimrc

扩展模式：当前vim进程有效

(1) 行号

显示： set nu

取消： set nonu

(2) 忽略字符的大小写

启用：set ic

不忽略：set noic

(3) 自动缩进

启用：set ai

禁用：set noai

(4)智能缩进

启用：set si

禁用：set nosi

(5) 高亮搜索

启用：set hlsearch

禁用：noh

(6) 语法高亮

启用：syntax on

禁用：syntax off

(7) 显示Tab和换行符 ^I 和$显示

启用：set list

禁用：set nolist

(8) 文件格式

set ff=dos|unix

(9) 设置文本宽度

set textwidth=65 (vim only)

set wrapmargin=15

(10) 设置光标所在行的标识线

启用：cul

禁用：set no cursorline

(11) 复制保留格式

启用：set paste

禁用：set nopast

## 文件压缩

1. file-roller  图形化压缩查看器
2. compress : .Z

        -d: 解压缩，相当于uncompress

-c: 结果输出至标准输出,不删除原文件

-v: 显示详情

1.
2.
3. gzip        : .gz

        -d：解压缩，相当于gunzip

-c: 结果输出至标准输出,不删除原文件

                 gzip -c messages  \&gt;messages.gz

cat messages | gzip \&gt; m.gz

        -#：1-9，指定压缩比，值越大压缩比越大

 zcat：不显式解压缩的前提下查看文本文件内容

1. bzip2  : .bz2

        -k：keep, 保留原文件

-d：解压缩

-#：1-9，压缩比，默认为9

   bzcat：不显式解压缩的前提下查看文本文件内容

1. xz  .xz

        -k: keep, 保留原文件

-d：解压缩

-#：1-9，压缩比，默认为6

xzcat: 不显式解压缩的前提下查看文本文件内容

1. zip  .zip

        zip -r  f1.zip  f1  压缩

        unzip f1.zip

        cat /data/f1 | zip f1 –

        unzip -p f1    仅显示不解压

1. tar

（1）创建归档

        tar -cpvf /data/f1.tar file

  (2) 追加文件至归档： 注：不支持对压缩文件追加

        tar -r -f  /data/f1.tar file

(3) 查看归档文件中的文件列表

tar -t -f /data/f1.tar

(4) 展开归档

tar -x -f /data/f1.tar

tar -x -f /data/f1.tar -C /PATH/

(5) 结合压缩工具实现：归档并压缩

-j: bzip2, -z: gzip, -J: xz

  （6）-exclude 排除文件

tar zcvf /root/a3.tgz --exclude=/app/1 --exclude=/app/2 /app

  （7）-T指定输入文件列表,-X指定包含要排除的文件列表

1. splist:：分割一个文件为多个文件

        split –b  Size –d tar-file-name  prefix-name 数字编号

        合并： cat mybackup-parts\* \&gt; mybackup.tar.gz

1. cpio

复制文件从或到归档

        cpio通过重定向将文件进行打包备份，还原恢复，可以解压以 &quot;.cpio&quot;或者&quot;.tar&quot;结尾的文件

cpio [选项] \&gt; 文件名或者设备名

cpio [选项] \&lt; 文件名或者设备名

-o 将文件拷贝打包成文件或者将文件输出到设备上

-O filename 输出到指定的归档文件名

-A 向已存在的归档文件中追加文件

-i 解包，将打包文件解压或将设备上的备份还原到系统

-I filename 对指定的归档文件名解压

-t 预览，查看文件内容或者输出到设备上的文件内容

-F filename 使用指定的文件名替代标准输入或输出

-d 解包生成目录，在cpio还原时，自动的建立目录

-v 显示打包过程中的文件名称

        将etc目录备份： find ./etc -print |cpio -ov \&gt;bak.cpio

将/data内容追加bak.cpio find /data | cpio -oA -F  bak.cpio

内容预览 cpio –tv \&lt; etc.cpio

解包文件 cpio –idv \&lt; etc.cpio
