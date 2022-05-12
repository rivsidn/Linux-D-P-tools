```
NICE(1)                               User Commands                              NICE(1)

NAME
       nice - run a program with modified scheduling priority
              指定优先级运行进程

SYNOPSIS
       nice [OPTION] [COMMAND [ARG]...]

DESCRIPTION
       Run COMMAND with an adjusted niceness, which affects process scheduling.  With no
       COMMAND, print the current niceness.  Niceness values range from -20 (most favor‐
       able to the process) to 19 (least favorable to the process).
       指定进程运行的nice 值，会对进程调度产生影响。
       不指定命令的时候，输出当前优先级。
       nice值范围为 -20 到 19，数字越大，优先级越低。

       Mandatory arguments to long options are mandatory for short options too.

       -n, --adjustment=N
              add integer N to the niceness (default 10)
	      指定nice值，默认为10(此处的默认值是该命令的默认值，即省略N的时候等同于指定10；
	      进程执行的时候，默认nice值为0)

       --help display this help and exit
              显示帮助信息退出

       --version
              output version information and exit
	      显示版本号信息

       NOTE:  your  shell may have its own version of nice, which usually supersedes the
       version described here.  Please refer to your shell's documentation  for  details
       about the options it supports.
       shell可能实现了自己版本的nice命令，可能会取代此处描述的命令，可以去shell 文件查询。

AUTHOR
       Written by David MacKenzie.

REPORTING BUGS
       GNU coreutils online help: <http://www.gnu.org/software/coreutils/>
       Report nice translation bugs to <http://translationproject.org/team/>

COPYRIGHT
       Copyright  © 2017 Free Software Foundation, Inc.  License GPLv3+: GNU GPL version
       3 or later <http://gnu.org/licenses/gpl.html>.
       This is free software: you are free to change and redistribute it.  There  is  NO
       WARRANTY, to the extent permitted by law.

SEE ALSO
       nice(2), renice(1)

       Full documentation at: <http://www.gnu.org/software/coreutils/nice>
       or available locally via: info '(coreutils) nice invocation'

GNU coreutils 8.28                    January 2018                               NICE(1)
```
