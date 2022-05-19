```
PGREP(1)                              User Commands                             PGREP(1)

NAME
       pgrep, pkill - look up or signal processes based on name and other attributes
                      基于名称或者其他属性查找或给进程发送信号

SYNOPSIS
       pgrep [options] pattern
       pkill [options] pattern

DESCRIPTION
       pgrep  looks  through  the  currently running processes and lists the process IDs
       which match the selection criteria to stdout.  All the criteria  have  to  match.
       For example,

              $ pgrep -u root sshd

       显示名称为sshd，被root 拥有的进程.
       will only list the processes called sshd AND owned by root.  On the other hand,

              $ pgrep -u root,daemon

       显示root或daemon 拥有的进程.
       will list the processes owned by root OR daemon.

       pkill will send the specified signal (by default SIGTERM) to each process instead
       of listing them on stdout.
       pkill 给给定的进程发送信号.

OPTIONS
       -signal
       --signal signal
              Defines the signal to send to each matched process.  Either the numeric or
              the symbolic signal name can be used.  (pkill only.)
	      指定发送给选中进程的信号(只用于pkill)

       -c, --count
              Suppress normal output; instead print a count of matching processes.  When
              count does not match anything, e.g. returns zero, the command will  return
              non-zero value.
	      抑制正常的输出；输出匹配进程的数量。
	      当没有匹配信息的时候，返回 0，命令行返回，非零值。

       -d, --delimiter delimiter
              Sets  the string used to delimit each process ID in the output (by default
              a newline).  (pgrep only.)
	      设置输出进程号的分割符，默认为换行符。

       -f, --full
              The pattern is normally only matched against the process name.  When -f is
              set, the full command line is used.

       -g, --pgroup pgrp,...
              Only  match processes in the process group IDs listed.  Process group 0 is
              translated into pgrep's or pkill's own process group.
	      匹配进程组信息，进程组 0 表示匹配自己所在的进程组.

       -G, --group gid,...
              Only match processes whose real group ID is listed.  Either the  numerical
              or symbolical value may be used.

       -i, --ignore-case
              Match processes case-insensitively.
	      匹配时忽略大小写.

       -l, --list-name
              List the process name as well as the process ID.  (pgrep only.)
	      输出进程名和进程号.

       -a, --list-full
              List the full command line as well as the process ID.  (pgrep only.)
	      显示完整的命令行信息和进程号.

       -n, --newest
              Select only the newest (most recently started) of the matching processes.
	      只显示匹配进程中最新的进程

       -o, --oldest
              Select only the oldest (least recently started) of the matching processes.
	      只显示匹配进程中最老的进程

       -P, --parent ppid,...
              Only match processes whose parent process ID is listed.
	      匹配父进程信息

       -s, --session sid,...
              Only  match processes whose process session ID is listed.  Session ID 0 is
              translated into pgrep's or pkill's own session ID.
	      匹配会话信息

       -t, --terminal term,...
              Only match processes whose controlling terminal is listed.   The  terminal
              name should be specified without the "/dev/" prefix.
	      匹配终端信息

       -u, --euid euid,...
              Only match processes whose effective user ID is listed.  Either the numer‐
              ical or symbolical value may be used.
	      匹配有效用户ID

       -U, --uid uid,...
              Only match processes whose real user ID is listed.  Either  the  numerical
              or symbolical value may be used.
	      匹配真实用户ID

       -v, --inverse
              Negates the matching.  This option is usually used in pgrep's context.  In
              pkill's context the short option is disabled to avoid accidental usage  of
              the option.
	      反向匹配，只能用于pgrep 的时候，pkill 的时候不让用.

       -w, --lightweight
              Shows  all thread ids instead of pids in pgrep's context.  In pkill's con‐
              text this option is disabled.
	      pgrep 情景中匹配线程信息

       -x, --exact
              Only match processes whose names (or command  line  if  -f  is  specified)
              exactly match the pattern.
	      匹配进程名

       -F, --pidfile file
              Read  PID's  from file.  This option is perhaps more useful for pkill than
              pgrep.

       -L, --logpidfile
              Fail if pidfile (see -F) not locked.

       --ns pid
              Match processes that belong to the same namespaces.  Required  to  run  as
              root  to  match  processes from other users. See --nslist for how to limit
              which namespaces to match.

       --nslist name,...
              Match only the provided namespaces. Available namespaces: ipc,  mnt,  net,
              pid, user,uts.
	      匹配对应的命名空间.

       -V, --version
              Display version information and exit.
	      显示版本信息

       -h, --help
              Display help and exit.
	      显示帮助信息然后退出

OPERANDS
       pattern
              Specifies  an Extended Regular Expression for matching against the process
              names or command lines.
	      指定用于匹配进程名的正则表达式

EXAMPLES
       Example 1: Find the process ID of the named daemon:

              $ pgrep -u root named

       Example 2: Make syslog reread its configuration file:
                  让syslog 重新读取配置文件

              $ pkill -HUP syslogd

       Example 3: Give detailed information on all xterm processes:
                  -f 指定显示的信息，-p 指定进程号

              $ ps -fp $(pgrep -d, -x xterm)

       Example 4: Make all netscape processes run nicer:
                  重新设置netspace 命名空间进程的nice 值:

              $ renice +4 $(pgrep netscape)

EXIT STATUS
       0      One or more processes matched the criteria.
       1      No processes matched.
       2      Syntax error in the command line.
       3      Fatal error: out of memory etc.

NOTES
       The process name used for matching is limited to the 15 characters present in the
       output  of  /proc/pid/stat.  Use the -f option to match against the complete com‐
       mand line, /proc/pid/cmdline.

       The running pgrep or pkill process will never report itself as a match.

BUGS
       The options -n and -o and -v can not be combined.  Let me know if you need to  do
       this.

       Defunct processes are reported.

SEE ALSO
       ps(1), regex(7), signal(7), killall(1), skill(1), kill(1), kill(2)

AUTHOR
       Kjetil Torgrim Homme ⟨kjetilho@ifi.uio.no⟩

REPORTING BUGS
       Please send bug reports to ⟨procps@freelists.org⟩

procps-ng                              March 2015                               PGREP(1)
```
