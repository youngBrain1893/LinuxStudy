### Shell 基本命令
#### 命令行和 Shell 的概念

在很多不正式的场合，这两个名词代表着相同的概念--命令解释器。
严格意义上讲 : 命令行指的是 __供用户输入的界面__ ，其本身 __只接受输入__ ，然后把命令传递给命令解释器，后者就是 __Shell__

Shell 是一个程序，在用户和操作系统之间提供一个面向行的可交互接口，用户在命令行中输入命令，运行在后台的shell把命令转换成指令代码发送给操作系统。Shell 提供了很多高级特性使用户和操作系统的交互更简单高效。

#### Linux 上常用的 Shell

不同的 Shell 提供不尽相同的语法和特性

* Bourne Again Shell(BASH),Linux 默认 shell
* TCSH Shell
* Z-Shell

#### Linux Shell 下的通配符
- `*` 通配符

    匹配任意长度的字符串

        ls *.cpp  //列出所有 cpp 文件

- `?` 通配符

    匹配任意一个字符

        ls text?    //列出所有以 text 开始，后面跟一个字符的文件

- `[]` 通配符

    匹配 `[]` 内的字符。
            
        ls text[1D]     //列出以 text 开始，后面跟 1 或 D 的文件

#### 查看目录和文件

- 显示当前目录: `pwd`

- 改变目录: `cd`

- 列出目录内容: `ls`

- 列出目录内容: `dir` 和 `vdir`

    `dir` 与 windows 中的 dir 功能类似但更少
    `vdir` 相当于执行了 `ls -l`
    
- 查看__文本__文件: `cat` 和 `more`

    `cat` 一般用于文本内容不是很长的时候，`more` 用于文本内容过长时使用
    `cat` 命令可以通过 `-n` 参数列出每一行的行号
        
        cat -n index.html
        more index.html

- 阅读文件的开头和结尾: `head` 和 `tail`

    `head` 和 `tail` 命令类似一个是显示文本前几行，一个是显示文本最后几行
    通过 `-n [number]` 的形式指定显示几行，不带 `-n` 参数列出所有内容
    
        head -n 2 index.html main.html
        tail -n 4 index.html main.html

- 更好的文本阅读工具: `less`

    `less` 功能和 `more` 类似，但功能更为强大

- 查找文件内容: `grep`

    grep [OPTIONS] PATTERN [FILE ..]
    gerp 通过 "基础正则表达式(basic regular expression)"进行搜索。
    grep 会将文件中出现关键词的行输出，可以指定多个文件来搜索。
        
        grep -n html index.html

#### 查找文件 `find` 命令

find [OPTIONS] [path ...] [expression]
find [文件目录] -name [文件名] [其他参数] -print

暂时已知其他可选参数

- type
    指定文件类型。在 Linux 中，包括目录和设备都以文件的形式表现。可以使用 `-type` 选项来定位特殊文件类型。
 
    |   参数  |      含义     | 参数 |   含义    |
    |  ----  |   ---------  | --- |   ---     |
    |   b    |   块设备文件   |  f  |   普通文件  |
    |   c    |   字符设备文件  |  p  |   命名管道 |
    |   d    |   目录文件     |  l |   符号链接  |

- atime
    最后一次使用时间 

- mtime
    最后一次修改时间

        find ~/ -name test.html -mtime +12 -print
        find ~/ -name test.html -atime -12 -print

#### 更快的定位文件 `locate` 命令

`find`命令已经足够强大了，但当批量操作，以及用户根本不知道文件位置的时候可以使用 `locate` 命令执行文件查找操作，更为高效

`locate` 命令搜寻速度很快是因为其根本__不会去进入子目录进行搜索__, 而是通过索引文件名数据库来确定文件的位置。

`locate` 名字自动建立整个文件名数据库，不需要用户插手，如果希望立即生成该数据库文件的最新版本可以使用`updatedb` 命令，更新整个数据库，大概需要耗时 1 分钟左右。

#### 从终端运行程序

系统提供的命令也是程序`ls`, `cat` 这些都是，我们同样也可以自定义自己的程序在 terminal 下运行，具体的配置以后再说，当程序运行的时候当前终端窗口处于挂起状态，直到我们所启的程序运行完毕(关闭)

#### 查找特定程序命令 `whereis`    

`whereis` 命令主要用于查找程序文件，并提供这个文件的二进制可执行文件，源代码文件和使用手册页存放的位置。

    whereis find        ## 用于查找 find 命令的可执行文件，源代码文件以及使用手册
    whereis -b find     ## 只查找 find 命令的二进制可执行文件

#### 用户及版本信息查看

- `who` 命令

    在同一台服务器上，同一时间往往会有很多人同时登录，可以使用 `who` 查看当前系统中有哪些人登录，以及他们都工作在哪个控制台上。

        young@ubuntu:~$ who
        young    :0           2015-09-25 00:38 (:0)
        young    pts/0        2015-09-27 19:42 (:0)

- `whoami` 命令

    有时候忘记了自己是以什么身份登录到系统的，尤其涉及到特定身份启动服务的时候，可以使用 `whoami` 这个命令查看
    
        young@ubuntu:~$ whoami
        young

- `uname` 命令

    查看当前系统的版本信息，可以使用 `uname` 命令，该命令会显示当前系统的版本信息。
    
        young@ubuntu:~$ uname
        Linux
        
        ## 参数 `-a` 会给出当前操作系统所有有用信息
        young@ubuntu:~$ uname -a
        Linux ubuntu 3.16.0-30-generic #40~14.04.1-Ubuntu SMP Thu Jan 15 17:45:15 UTC 2015 i686 i686 i686 GNU/Linux

        ## 参数 `-r` 只是展示出内核版本信息
        young@ubuntu:~$ uname -r
        3.16.0-30-generic

#### 在 Terminal 中寻求帮助

Linux 中命令繁多，并不能很好的记忆，Linux 几乎为每个命令和系统调用都编写了帮助手册，使用 `man` 命令可以方便的获取某个命令的帮助信息。

`man` 命令在显示手册时调用的是 `less` 程序。

#### 获取命令简介 `whatis` 和 `apropos` 方法

- `whatis`

    `man` 手册太过于详细内容过多，很多情况下只是为了知道这个命令大概可以做些什么，这个时候就可以使用 `whatis` 命令来满足用户的好奇心。

        young@ubuntu:~$ whatis uname
        uname (1)            - print system information
        uname (2)            - get name and information about current kernel
    
    `whatis` 的原理和 `locate` 命令基本一致。

- `apropos`

    `apropos` 命令和 `whatis` 命令相反，当用户想要实现一个功能但又不知道具体的命令的时候可以调用 `apropos` 方法获得帮助

        young@ubuntu:~$ apropos search
        apropos (1)          - search the manual page names and descriptions
        badblocks (8)        - search a device for bad blocks
        bsearch (3)          - binary search of a sorted array
        bzegrep (1)          - search possibly bzip2 compressed files for a regular e...
        bzfgrep (1)          - search possibly bzip2 compressed files for a regular e...
        bzgrep (1)           - search possibly bzip2 compressed files for a regular e...
        find (1)             - search for files in a directory hierarchy
        hsearch (3)          - hash table management
        hsearch_r (3)        - hash table management
        lfind (3)            - linear search of an array
        lsearch (3)          - linear search of an array
        lzegrep (1)          - search compressed files for a regular expression
        lzfgrep (1)          - search compressed files for a regular expression
        lzgrep (1)           - search compressed files for a regular expression
        ...

    `apropos` 将命令简介 (`whatis` 的输出) 中包含传入参数的条目一并列出来，用户从中选择自己想要的。

#### 小结
- 命令行是 Linux 的精华部分。所有的系统管理操作都可以在 Shell 下完成
- 有多种不同的 Shell 可供使用。目前 Linux 上使用最广泛的是 BASH
- 可以使用命令行补全和通配符提高使用 Shell 的效率
- `pwd` 命令用于显示当前目录信息
- `cd` 命令用于在目录间切换，是 Linux 中使用最频繁的命令之一
- `ls` 命令提供了大量选项供用户查看目录内容
- `dir` 和 `vdir` 是 `ls` 命令的袖珍版本
- 使用 `cat` 命令查看文本文件，`-n` 可以列出行号
- 使用 `more` 命令分页显示一个较长的文本文件
- 使用 `head` 和 `tail` 命令显示一个文件的开头和结尾
- `less` 命令提供了查看文件的更高级的功能。 `man` 命令就是通过调用 `less` 显示帮组手册的信息的
- `grep` 程序是查找文件内容的利器
- `find` 命令可以按需查找某个特定的文件 ( 包括目录 )
- `locate` 命令通过事先建立数据库提高搜索文件的速度，更新数据库使用 `updatedb`
- 在 Shell 中可以根据相应的设置，运行个人安装的程序，但可能会使当前控制台窗口一直挂起
- `whereis` 命令可以查找某个特定程序所在的位置
- `who`, `whoami` 可以查看当前用户信息
- `uname` 可以查看系统的版本信息
- `man` 命令让 Linux 的命令使用更为简单，输出结果为对应命令的参数手册
- `whatis` 和 `apropos` 命令能够从 `man` 手册中提取简要信息。

#### 本章涉及的命令
`cd` , `ls` , `cat` , `pwd` , `dir` , `vdir` , `cat` , `more` , `head` , `tail` , `less` , `grep` , `find`, `locate` , `whereis` , `who` , `whoami` , `uname` , `man` , `whatis` , `apropos`

#### 小 Tips 
- Linux 文件系统中的路径的分隔符是正斜杆 `/` 而不是反斜杠 `\` p:35
- 使用 `cd` 命令，不带参数的时候等同于 `cd ~` ,回到当前用户主目录下