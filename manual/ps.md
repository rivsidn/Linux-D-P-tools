```
PS(1)                                 User Commands                                PS(1)

NAME
       ps - report a snapshot of the current processes.
            显示当前进程的快照信息

SYNOPSIS
       ps [options]

DESCRIPTION
       ps displays information about a selection of the active processes.  If you want a
       repetitive update of the selection and the displayed information, use top(1)
       instead.

       This version of ps accepts several kinds of options:
       该版本的ps 支持多种类型的选项:

       1   UNIX options, which may be grouped and must be preceded by a dash.
           开头必须要有一个dash
       2   BSD options, which may be grouped and must not be used with a dash.
           开头必须没有dash
       3   GNU long options, which are preceded by two dashes.
           开头必须有两个dash

       Options of different types may be freely mixed, but conflicts can appear.  There
       are some synonymous options, which are functionally identical, due to the many
       standards and ps implementations that this ps is compatible with.
       由于ps 的兼容性，导致有许多相同功能的选项.

       Note that "ps -aux" is distinct from "ps aux".  The POSIX and UNIX standards
       require that "ps -aux" print all processes owned by a user named "x", as well as
       printing all processes that would be selected by the -a option.  If the user
       named "x" does not exist, this ps may interpret the command as "ps aux" instead
       and print a warning.  This behavior is intended to aid in transitioning old
       scripts and habits.  It is fragile, subject to change, and thus should not be
       relied upon.
       POSIX和UNIX模式下，"ps -aux" 模式会打印用户"x" 的进程，以及-a 选项选中的进程，
       如果用户x 不存在，会解释为"ps aux"而不会输出告警信息。这部分比较脆弱，所以不应
       该依赖它.

       By default, ps selects all processes with the same effective user ID (euid=EUID)
       as the current user and associated with the same terminal as the invoker.  It
       displays the process ID (pid=PID), the terminal associated with the process
       (tname=TTY), the cumulated CPU time in [DD-]hh:mm:ss format (time=TIME), and the
       executable name (ucmd=CMD).  Output is unsorted by default.
       ps 默认情况下只输入相同终端下执行的进程，默认会显示，进程号、终端、累计CPU时间、
       可执行文件名，输出默认是不排序的.

       The use of BSD-style options will add process state (stat=STAT) to the default
       display and show the command args (args=COMMAND) instead of the executable name.
       You can override this with the PS_FORMAT environment variable. The use of
       BSD-style options will also change the process selection to include processes on
       other terminals (TTYs) that are owned by you; alternately, this may be described
       as setting the selection to be the set of all processes filtered to exclude
       processes owned by other users or not on a terminal.  These effects are not
       considered when options are described as being "identical" below, so -M will be
       considered identical to Z and so on.
       说两个选项相同的时候，该特性并没有考虑在内.

       Except as described below, process selection options are additive.  The default
       selection is discarded, and then the selected processes are added to the set of
       processes to be displayed.  A process will thus be shown if it meets any of the
       given selection criteria.
       除特殊说明，选中的进程是累加的。默认的选项被丢弃，之后选中的其他进程会添加到显示
       集合中，如果匹配任何的选项，进程会被添加到显示列表中.

EXAMPLES
       To see every process on the system using standard syntax:
       查看系统中的所有进程:
          ps -e
          ps -ef
          ps -eF
          ps -ely

       To see every process on the system using BSD syntax:
       查看系统中的所有进程:
          ps ax
          ps axu

       To print a process tree:
       输出进程树:
          ps -ejH
          ps axjf

       To get info about threads:
       获取线程信息:
          ps -eLf
          ps axms

       To get security info:
       获取加密信息:
          ps -eo euser,ruser,suser,fuser,f,comm,label
          ps axZ
          ps -eM

       To see every process running as root (real & effective ID) in user format:
       查看所有用户为root 的进程，-U -u 指定显示的用户， u 指定显示的格式:
          ps -U root -u root u

       To see every process with a user-defined format:
       以用户定义的格式查看进程:
          ps -eo pid,tid,class,rtprio,ni,pri,psr,pcpu,stat,wchan:14,comm
          ps axo stat,euid,ruid,tty,tpgid,sess,pgrp,ppid,pid,pcpu,comm
          ps -Ao pid,tt,user,fname,tmout,f,wchan

       Print only the process IDs of syslogd:
       输出进程的进程号信息(通过命令名获取进程，输出进程号，由于此时pid=空所有只输出进程号信息):
          ps -C syslogd -o pid=

       Print only the name of PID 42:
       输出进程号的进程名信息:
          ps -q 42 -o comm=

SIMPLE PROCESS SELECTION(简单程序选择)
       a      Lift the BSD-style "only yourself" restriction, which is imposed upon the
              set of all processes when some BSD-style (without "-") options are used or
              when the ps personality setting is BSD-like.  The set of processes
              selected in this manner is in addition to the set of processes selected by
              other means.  An alternate description is that this option causes ps to
              list all processes with a terminal (tty), or to list all processes when
              used together with the x option.
	      显示出所有带有终端的进程，与 x 一起使用显示所有进程.

       -A     Select all processes.  Identical to -e.
              选择所有的进程，等同于 -e.

       -a     Select all processes except both session leaders (see getsid(2)) and
              processes not associated with a terminal.
	      选择所有的进程，除了session leader 和 没绑定终端的进程.

       -d     Select all processes except session leaders.
              选择所有的进程出了session leader.

       --deselect
              Select all processes except those that fulfill the specified conditions
              (negates the selection).  Identical to -N.
	      选择除了这些进程之外的所有进程.

       -e     Select all processes.  Identical to -A.
              选择所有进程.

       g      Really all, even session leaders.  This flag is obsolete and may be
              discontinued in a future release.  It is normally implied by the a flag,
              and is only useful when operating in the sunos4 personality.
	      选择所有进程，已经过时，可能会被废弃.

       -N     Select all processes except those that fulfill the specified conditions
              (negates the selection).  Identical to --deselect.
	      选择所有没匹配该选项的进程.

       T      Select all processes associated with this terminal.  Identical to the t
              option without any argument.
	      选择该终端的所有进程，等同于不加任何选项的 t

       r      Restrict the selection to only running processes.
              之显示当前运行的进程.

       x      Lift the BSD-style "must have a tty" restriction, which is imposed upon
              the set of all processes when some BSD-style (without "-") options are
              used or when the ps personality setting is BSD-like.  The set of processes
              selected in this manner is in addition to the set of processes selected by
              other means.  An alternate description is that this option causes ps to
              list all processes owned by you (same EUID as ps), or to list all
              processes when used together with the a option.
	      接触BSD 风格"必须有一个tty" 的限制，与 x 一起显示所有进程.

PROCESS SELECTION BY LIST(链表进程选择)
       These options accept a single argument in the form of a blank-separated or
       comma-separated list.  They can be used multiple times.  For example:
       ps -p "1 2" -p 3,4
       可以通过空格或者逗号隔开.

       -123   Identical to --pid 123.
              等同于 --pid 123
       123    Identical to --pid 123.
              等同于 --pid 123

       -C cmdlist
              Select by command name.  This selects the processes whose executable name
              is given in cmdlist.
	      通过命令名称获取信息

       -G grplist
              Select by real group ID (RGID) or name.  This selects the processes whose
              real group name or ID is in the grplist list.  The real group ID
              identifies the group of the user who created the process, see getgid(2).
	      真实组ID

       -g grplist
              Select by session OR by effective group name.  Selection by session is
              specified by many standards, but selection by effective group is the
              logical behavior that several other operating systems use.  This ps will
              select by session when the list is completely numeric (as sessions are).
              Group ID numbers will work only when some group names are also specified.
              See the -s and --group options.

       --Group grplist
              Select by real group ID (RGID) or name.  Identical to -G.

       --group grplist
              Select by effective group ID (EGID) or name.  This selects the processes
              whose effective group name or ID is in grplist.  The effective group ID
              describes the group whose file access permissions are used by the process
              (see getegid(2)).  The -g option is often an alternative to --group.

       p pidlist
              Select by process ID.  Identical to -p and --pid.
	      选择进程ID.

       -p pidlist
              Select by PID.  This selects the processes whose process ID numbers appear
              in pidlist.  Identical to p and --pid.

       --pid pidlist
              Select by process ID.  Identical to -p and p.

       --ppid pidlist
              Select by parent process ID.  This selects the processes with a parent
              process ID in pidlist.  That is, it selects processes that are children of
              those listed in pidlist.
	      选择父进程ID.

       q pidlist
              Select by process ID (quick mode).  Identical to -q and --quick-pid.

       -q pidlist
              Select by PID (quick mode).  This selects the processes whose process ID
              numbers appear in pidlist.  With this option ps reads the necessary info
              only for the pids listed in the pidlist and doesn't apply additional
              filtering rules. The order of pids is unsorted and preserved. No
              additional selection options, sorting and forest type listings are allowed
              in this mode.  Identical to q and --quick-pid.

       --quick-pid pidlist
              Select by process ID (quick mode).  Identical to -q and q.

       -s sesslist
              Select by session ID.  This selects the processes with a session ID
              specified in sesslist.

       --sid sesslist
              Select by session ID.  Identical to -s.

       t ttylist
              Select by tty.  Nearly identical to -t and --tty, but can also be used
              with an empty ttylist to indicate the terminal associated with ps.  Using
              the T option is considered cleaner than using t with an empty ttylist.

       -t ttylist
              Select by tty.  This selects the processes associated with the terminals
              given in ttylist.  Terminals (ttys, or screens for text output) can be
              specified in several forms: /dev/ttyS1, ttyS1, S1.  A plain "-" may be
              used to select processes not attached to any terminal.

       --tty ttylist
              Select by terminal.  Identical to -t and t.

       U userlist
              Select by effective user ID (EUID) or name.  This selects the processes
              whose effective user name or ID is in userlist.  The effective user ID
              describes the user whose file access permissions are used by the process
              (see geteuid(2)).  Identical to -u and --user.

       -U userlist
              Select by real user ID (RUID) or name.  It selects the processes whose
              real user name or ID is in the userlist list.  The real user ID identifies
              the user who created the process, see getuid(2).

       -u userlist
              Select by effective user ID (EUID) or name.  This selects the processes
              whose effective user name or ID is in userlist.

              The effective user ID describes the user whose file access permissions are
              used by the process (see geteuid(2)).  Identical to U and --user.

       --User userlist
              Select by real user ID (RUID) or name.  Identical to -U.

       --user userlist
              Select by effective user ID (EUID) or name.  Identical to -u and U.

OUTPUT FORMAT CONTROL(输出格式控制)
       These options are used to choose the information displayed by ps.  The output may
       differ by personality.

       -c     Show different scheduler information for the -l option.
              显示不同的调度信息

       --context
              Display security context format (for SELinux).
	      显示安全上下文信息

       -f     Do full-format listing. This option can be combined with many other
              UNIX-style options to add additional columns.  It also causes the command
              arguments to be printed.  When used with -L, the NLWP (number of threads)
              and LWP (thread ID) columns will be added.  See the c option, the format
              keyword args, and the format keyword comm.

       -F     Extra full format.  See the -f option, which -F implies.

       --format format
              user-defined format.  Identical to -o and o.
	      用户定义格式，等同于 -o 或 o .

       j      BSD job control format.

       -j     Jobs format.

       l      Display BSD long format.

       -l     Long format.  The -y option is often useful with this.

       -M     Add a column of security data.  Identical to Z (for SELinux).
              添加显示安全信息的一栏

       O format
              is preloaded o (overloaded).  The BSD O option can act like -O
              (user-defined output format with some common fields predefined) or can be
              used to specify sort order.  Heuristics are used to determine the behavior
              of this option.  To ensure that the desired behavior is obtained (sorting
              or formatting), specify the option in some other way (e.g.  with -O or
              --sort).  When used as a formatting option, it is identical to -O, with
              the BSD personality.

       -O format
              Like -o, but preloaded with some default columns.  Identical to -o pid,
              format,state,tname,time,command or -o pid,format,tname,time,cmd, see -o
              below.

       o format
              Specify user-defined format.  Identical to -o and --format.
	      指定用户输出格式

       -o format
              User-defined format.  format is a single argument in the form of a
              blank-separated or comma-separated list, which offers a way to specify
              individual output columns.  The recognized keywords are described in the
              STANDARD FORMAT SPECIFIERS section below. 
	      Headers may be renamed (ps -o pid,ruser=RealUser -o comm=Command) as desired.
	      表头重命名
	      If all column headers are empty (ps -o pid= -o comm=) then the header line will not be output.
	      如果有的表头没输入任何信息，则不显示表头
              Column width will increase as needed for wide headers; this may be used to
              widen up columns such as WCHAN (ps -o pid,wchan=WIDE-WCHAN-COLUMN -o
              comm).  Explicit width control (ps opid,wchan:42,cmd) is offered too.
	      可以用于订制显示宽度
	      The behavior of ps -o pid=X,comm=Y varies with personality; output may be one
              column named "X,comm=Y" or two columns named "X" and "Y".  Use multiple -o
              options when in doubt.
	      当存在疑问的时候，指定两个 -o 选项.
	      Use the PS_FORMAT environment variable to specify
              a default as desired; DefSysV and DefBSD are macros that may be used to
              choose the default UNIX or BSD columns.
	      可以通过环境变量指定默认显示的内容.

       s      Display signal format.
              显示信号格式

       u      Display user-oriented format.
              显示面向用户格式

       v      Display virtual memory format.
              显示虚拟内存格式

       X      Register format.
              寄存器格式

       -y     Do not show flags; show rss in place of addr.  This option can only be
              used with -l.
	      不显示flags; addr 位置显示 rss 信息. 该选项只能跟 -l 选项一起使用.

       Z      Add a column of security data.  Identical to -M (for SELinux).
              添加安全显示的一栏.

OUTPUT MODIFIERS(输出修改器)
       c      Show the true command name.  This is derived from the name of the
              executable file, rather than from the argv value.  Command arguments and
              any modifications to them are thus not shown.  This option effectively
              turns the args format keyword into the comm format keyword; it is useful
              with the -f format option and with the various BSD-style format options,
              which all normally display the command arguments.  See the -f option, the
              format keyword args, and the format keyword comm.

       --cols n
              Set screen width.
	      设置屏幕宽度

       --columns n
              Set screen width.
	      设置屏幕宽度

       --cumulative
              Include some dead child process data (as a sum with the parent).
	      累积模式显示

       e      Show the environment after the command.
              命令之后显示环境变量

       f      ASCII art process hierarchy (forest).
              显示进程树信息

       --forest
              ASCII art process tree.
	      显示进程树信息

       h      No header.  (or, one header per screen in the BSD personality).  The h
              option is problematic.  Standard BSD ps uses this option to print a header
              on each page of output, but older Linux ps uses this option to totally
              disable the header.  This version of ps follows the Linux usage of not
              printing the header unless the BSD personality has been selected, in which
              case it prints a header on each page of output.  Regardless of the current
              personality, you can use the long options --headers and --no-headers to
              enable printing headers each page or disable headers entirely,
              respectively.
	      通过--headers 或者 --no-headers 控制显示头部信息

       -H     Show process hierarchy (forest).
              显示进程层级信息

       --headers
              Repeat header lines, one per page of output.

       k spec Specify sorting order.  Sorting syntax is [+|-]key[,[+|-]key[,...]].
              Choose a multi-letter key from the STANDARD FORMAT SPECIFIERS section.
              The "+" is optional since default direction is increasing numerical or
              lexicographic order.  Identical to --sort.
	      指定排序信息

                      Examples:
                      ps jaxkuid,-ppid,+pid
                      ps axk comm o comm,args
                      ps kstart_time -ef

       --lines n
              Set screen height.
	      设置屏幕高度

       n      Numeric output for WCHAN and USER (including all types of UID and GID).

       --no-headers
              Print no header line at all.  --no-heading is an alias for this option.
	      输出不显示头部信息

       O order
              Sorting order (overloaded).  The BSD O option can act like -O
              (user-defined output format with some common fields predefined) or can be
              used to specify sort order.  Heuristics are used to determine the behavior
              of this option.  To ensure that the desired behavior is obtained (sorting
              or formatting), specify the option in some other way (e.g.  with -O or
              --sort).

              For sorting, obsolete BSD O option syntax is O[+|-]k1[,[+|-]k2[,...]].  It
              orders the processes listing according to the multilevel sort specified by
              the sequence of one-letter short keys k1,k2, ...  described in the
              OBSOLETE SORT KEYS section below.  The "+" is currently optional, merely
              re-iterating the default direction on a key, but may help to distinguish
              an O sort from an O format.  The "-" reverses direction only on the key it
              precedes.

       --rows n
              Set screen height.
	      设置屏幕高度

       S      Sum up some information, such as CPU usage, from dead child processes into
              their parent.  This is useful for examining a system where a parent
              process repeatedly forks off short-lived children to do work.
	      汇总CPU 占用率等信息，该选项对于查看父进程创建一个生命周期很短的子进程做某些
	      工作时很有用.

       --sort spec
              Specify sorting order.  Sorting syntax is [+|-]key[,[+|-]key[,...]].
              Choose a multi-letter key from the STANDARD FORMAT SPECIFIERS section.
              The "+" is optional since default direction is increasing numerical or
              lexicographic order.  Identical to k.  For example: ps jax --sort=uid,
              -ppid,+pid
	      指定排序顺序.

       w      Wide output.  Use this option twice for unlimited width.

       -w     Wide output.  Use this option twice for unlimited width.
                            使用两次表示不限制宽度

       --width n
              Set screen width.
	      设置屏幕宽度

THREAD DISPLAY(显示线程信息)
       H      Show threads as if they were processes.
              将线程当作进程显示

       -L     Show threads, possibly with LWP and NLWP columns.
              显示线程信息

       m      Show threads after processes.
              进程之后显示线程

       -m     Show threads after processes.
              进程之后显示线程

       -T     Show threads, possibly with SPID column.

OTHER INFORMATION(其他信息)
       --help section
              Print a help message.  The section argument can be one of simple, list,
              output, threads, misc or all.  The argument can be shortened to one of the
              underlined letters as in: s|l|o|t|m|a.
	      显示帮助信息

       --info Print debugging info.
              输出调试信息

       L      List all format specifiers.
              列出所有格式描述符

       V      Print the procps-ng version.
              输出版本信息

       -V     Print the procps-ng version.
              输出版本信息

       --version
              Print the procps-ng version.
	      输出版本信息

NOTES
       This ps works by reading the virtual files in /proc.  This ps does not need to be
       setuid kmem or have any privileges to run.  Do not give this ps any special
       permissions.
       通过读取/proc 文件获取到的内容，不需要其他任何权限.

       CPU usage is currently expressed as the percentage of time spent running during
       the entire lifetime of a process.  This is not ideal, and it does not conform to
       the standards that ps otherwise conforms to.  CPU usage is unlikely to add up to
       exactly 100%.

       The SIZE and RSS fields don't count some parts of a process including the page
       tables, kernel stack, struct thread_info, and struct task_struct.  This is
       usually at least 20 KiB of memory that is always resident.  SIZE is the virtual
       size of the process (code+data+stack).
       SIZE 和 RSS 区域并没有计算页表、内核栈、线程thread_info 和 task_struct 信息.
       SIZE 表示的是进程的虚拟内存大小.

       Processes marked <defunct> are dead processes (so-called "zombies") that remain
       because their parent has not destroyed them properly.  These processes will be
       destroyed by init(8) if the parent process exits.
       <defunct> 是僵尸进程.

       If the length of the username is greater than the length of the display column,
       the username will be truncated. See the -o and -O formatting options to customize
       length.
       如果用户名超过了显示长度，会被截取，可以通过-o -O 选项调整.

       Commands options such as ps -aux are not recommended as it is a confusion of two
       different standards.  According to the POSIX and UNIX standards, the above
       command asks to display all processes with a TTY (generally the commands users
       are running) plus all processes owned by a user named "x".  If that user doesn't
       exist, then ps will assume you really meant "ps aux".
       不建议使用"ps -aux" 命令，因为可能导致混淆.

PROCESS FLAGS(进程标识)
       The sum of these values is displayed in the "F" column, which is provided by the
       flags output specifier:

               1    forked but didn't exec
               4    used super-user privileges

PROCESS STATE CODES(进程状态代码)
       Here are the different values that the s, stat and state output specifiers
       (header "STAT" or "S") will display to describe the state of a process:

               D    uninterruptible sleep (usually IO)
               R    running or runnable (on run queue)
               S    interruptible sleep (waiting for an event to complete)
               T    stopped by job control signal
               t    stopped by debugger during the tracing
               W    paging (not valid since the 2.6.xx kernel)
               X    dead (should never be seen)
               Z    defunct ("zombie") process, terminated but not reaped by its parent

       For BSD formats and when the stat keyword is used, additional characters may be
       displayed:

               <    high-priority (not nice to other users)
	            高优先级
               N    low-priority (nice to other users)
	            低优先级
               L    has pages locked into memory (for real-time and custom IO)
	            有锁定在内容中的页面
               s    is a session leader
	            是一个会话的leader
               l    is multi-threaded (using CLONE_THREAD, like NPTL pthreads do)
	            多线程
               +    is in the foreground process group
	            是一个前台进程组

OBSOLETE SORT KEYS(废弃的排序关键字)
       These keys are used by the BSD O option (when it is used for sorting).  The GNU
       --sort option doesn't use these keys, but the specifiers described below in the
       STANDARD FORMAT SPECIFIERS section.  Note that the values used in sorting are the
       internal values ps uses and not the "cooked" values used in some of the output
       format fields (e.g.  sorting on tty will sort into device number, not according
       to the terminal name displayed).  Pipe ps output into the sort(1) command if you
       want to sort the cooked values.
       此时排序的数值是没有经过处理的，如果需要使用经过处理的可以使用sort 命令.

       KEY   LONG         DESCRIPTION
       c     cmd          simple name of executable
       C     pcpu         cpu utilization
       f     flags        flags as in long format F field
       g     pgrp         process group ID
       G     tpgid        controlling tty process group ID
       j     cutime       cumulative user time
       J     cstime       cumulative system time
       k     utime        user time
       m     min_flt      number of minor page faults
       M     maj_flt      number of major page faults
       n     cmin_flt     cumulative minor page faults
       N     cmaj_flt     cumulative major page faults
       o     session      session ID
       p     pid          process ID
       P     ppid         parent process ID
       r     rss          resident set size
       R     resident     resident pages
       s     size         memory size in kilobytes
       S     share        amount of shared pages
       t     tty          the device number of the controlling tty
       T     start_time   time process was started
       U     uid          user ID number
       u     user         user name
       v     vsize        total VM size in KiB
       y     priority     kernel scheduling priority

AIX FORMAT DESCRIPTORS
       This ps supports AIX format descriptors, which work somewhat like the formatting
       codes of printf(1) and printf(3).  For example, the normal default output can be
       produced with this: ps -eo "%p %y %x %c".  The NORMAL codes are described in the
       next section.
       格式化输出描述符

       CODE   NORMAL   HEADER
       %C     pcpu     %CPU
       %G     group    GROUP
       %P     ppid     PPID
       %U     user     USER
       %a     args     COMMAND
       %c     comm     COMMAND
       %g     rgroup   RGROUP
       %n     nice     NI
       %p     pid      PID
       %r     pgid     PGID
       %t     etime    ELAPSED
       %u     ruser    RUSER
       %x     time     TIME
       %y     tty      TTY
       %z     vsz      VSZ


STANDARD FORMAT SPECIFIERS
       Here are the different keywords that may be used to control the output format
       (e.g. with option -o) or to sort the selected processes with the GNU-style --sort
       option.

       For example: ps -eo pid,user,args --sort user

       This version of ps tries to recognize most of the keywords used in other
       implementations of ps.

       The following user-defined format specifiers may contain spaces:
       args, cmd, comm, command, fname, ucmd, ucomm, lstart, bsdstart, start.

       Some keywords may not be available for sorting.
       部分关键字不能用于排序.

       CODE        HEADER    DESCRIPTION

       %cpu        %CPU      cpu utilization of the process in "##.#" format.
                             Currently, it is the CPU time used divided by the time the
                             process has been running (cputime/realtime ratio),
                             expressed as a percentage.  It will not add up to 100%
                             unless you are lucky.  (alias pcpu).

       %mem        %MEM      ratio of the process's resident set size  to the physical
                             memory on the machine, expressed as a percentage.  (alias
                             pmem).

       args        COMMAND   command with all its arguments as a string. Modifications
                             to the arguments may be shown.  The output in this column
                             may contain spaces.  A process marked <defunct> is partly
                             dead, waiting to be fully destroyed by its parent.
                             Sometimes the process args will be unavailable; when this
                             happens, ps will instead print the executable name in
                             brackets.  (alias cmd, command).  See also the comm format
                             keyword, the -f option, and the c option.
                             When specified last, this column will extend to the edge of
                             the display.  If ps can not determine display width, as
                             when output is redirected (piped) into a file or another
                             command, the output width is undefined (it may be 80,
                             unlimited, determined by the TERM variable, and so on).
                             The COLUMNS environment variable or --cols option may be
                             used to exactly determine the width in this case.  The w or
                             -w option may be also be used to adjust width.

       blocked     BLOCKED   mask of the blocked signals, see signal(7).  According to
                             the width of the field, a 32 or 64-bit mask in hexadecimal
                             format is displayed.  (alias sig_block, sigmask).

       bsdstart    START     time the command started.  If the process was started less
                             than 24 hours ago, the output format is " HH:MM", else it
                             is " Mmm:SS" (where Mmm is the three letters of the month).
                             See also lstart, start, start_time, and stime.

       bsdtime     TIME      accumulated cpu time, user + system.  The display format is
                             usually "MMM:SS", but can be shifted to the right if the
                             process used more than 999 minutes of cpu time.

       c           C         processor utilization. Currently, this is the integer value
                             of the percent usage over the lifetime of the process.
                             (see %cpu).

       caught      CAUGHT    mask of the caught signals, see signal(7).  According to
                             the width of the field, a 32 or 64 bits mask in hexadecimal
                             format is displayed.  (alias sig_catch, sigcatch).

       cgname      CGNAME    display name of control groups to which the process
                             belongs.

       cgroup      CGROUP    display control groups to which the process belongs.

       class       CLS       scheduling class of the process.  (alias policy, cls).
                             Field's possible values are:

                                      -   not reported
                                      TS  SCHED_OTHER
                                      FF  SCHED_FIFO
                                      RR  SCHED_RR
                                      B   SCHED_BATCH
                                      ISO SCHED_ISO
                                      IDL SCHED_IDLE
                                      ?   unknown value

       cls         CLS       scheduling class of the process.  (alias policy, cls).
                             Field's possible values are:

                                      -   not reported
                                      TS  SCHED_OTHER
                                      FF  SCHED_FIFO
                                      RR  SCHED_RR
                                      B   SCHED_BATCH
                                      ISO SCHED_ISO
                                      IDL SCHED_IDLE
                                      ?   unknown value

       cmd         CMD       see args.  (alias args, command).

       comm        COMMAND   command name (only the executable name).  Modifications to
                             the command name will not be shown.  A process marked
                             <defunct> is partly dead, waiting to be fully destroyed by
                             its parent.  The output in this column may contain spaces.
                             (alias ucmd, ucomm).  See also the args format keyword, the
                             -f option, and the c option.
                             When specified last, this column will extend to the edge of
                             the display.  If ps can not determine display width, as
                             when output is redirected (piped) into a file or another
                             command, the output width is undefined (it may be 80,
                             unlimited, determined by the TERM variable, and so on).
                             The COLUMNS environment variable or --cols option may be
                             used to exactly determine the width in this case.  The
                             w or -w option may be also be used to adjust width.

       command     COMMAND   See args.  (alias args, command).

       cp          CP        per-mill (tenths of a percent) CPU usage.  (see %cpu).

       cputime     TIME      cumulative CPU time, "[DD-]hh:mm:ss" format.  (alias time).

       drs         DRS       data resident set size, the amount of physical memory
                             devoted to other than executable code.

       egid        EGID      effective group ID number of the process as a decimal
                             integer.  (alias gid).

       egroup      EGROUP    effective group ID of the process.  This will be the
                             textual group ID, if it can be obtained and the field width
                             permits, or a decimal representation otherwise.  (alias
                             group).

       eip         EIP       instruction pointer.

       esp         ESP       stack pointer.

       etime       ELAPSED   elapsed time since the process was started, in the form
                             [[DD-]hh:]mm:ss.

       etimes      ELAPSED   elapsed time since the process was started, in seconds.

       euid        EUID      effective user ID (alias uid).

       euser       EUSER     effective user name.  This will be the textual user ID, if
                             it can be obtained and the field width permits, or a
                             decimal representation otherwise.  The n option can be used
                             to force the decimal representation.  (alias uname, user).

       f           F         flags associated with the process, see the PROCESS FLAGS
                             section.  (alias flag, flags).

       fgid        FGID      filesystem access group ID.  (alias fsgid).

       fgroup      FGROUP    filesystem access group ID.  This will be the textual group
                             ID, if it can be obtained and the field width permits, or a
                             decimal representation otherwise.  (alias fsgroup).

       flag        F         see f.  (alias f, flags).

       flags       F         see f.  (alias f, flag).

       fname       COMMAND   first 8 bytes of the base name of the process's executable
                             file.  The output in this column may contain spaces.

       fuid        FUID      filesystem access user ID.  (alias fsuid).

       fuser       FUSER     filesystem access user ID.  This will be the textual user
                             ID, if it can be obtained and the field width permits, or a
                             decimal representation otherwise.

       gid         GID       see egid.  (alias egid).

       group       GROUP     see egroup.  (alias egroup).

       ignored     IGNORED   mask of the ignored signals, see signal(7).  According to
                             the width of the field, a 32 or 64 bits mask in hexadecimal
                             format is displayed.  (alias sig_ignore, sigignore).

       ipcns       IPCNS     Unique inode number describing the namespace the process
                             belongs to. See namespaces(7).

       label       LABEL     security label, most commonly used for SELinux context
                             data.  This is for the Mandatory Access Control ("MAC")
                             found on high-security systems.

       lstart      STARTED   time the command started.  See also
                             bsdstart, start, start_time, and stime.

       lsession    SESSION   displays the login session identifier of a process, if
                             systemd support has been included.

       lwp         LWP       light weight process (thread) ID of the dispatchable entity
                             (alias spid, tid).  See tid for additional information.

       lxc         LXC       The name of the lxc container within which a task is
                             running.  If a process is not running inside a container, a
                             dash ('-') will be shown.

       machine     MACHINE   displays the machine name for processes assigned to VM or
                             container, if systemd support has been included.

       maj_flt     MAJFLT    The number of major page faults that have occurred with
                             this process.

       min_flt     MINFLT    The number of minor page faults that have occurred with
                             this process.

       mntns       MNTNS     Unique inode number describing the namespace the process
                             belongs to. See namespaces(7).

       netns       NETNS     Unique inode number describing the namespace the process
                             belongs to. See namespaces(7).

       ni          NI        nice value. This ranges from 19 (nicest) to -20 (not nice
                             to others), see nice(1).  (alias nice).

       nice        NI        see ni.(alias ni).

       nlwp        NLWP      number of lwps (threads) in the process.  (alias thcount).

       nwchan      WCHAN     address of the kernel function where the process is
                             sleeping (use wchan if you want the kernel function name).
                             Running tasks will display a dash ('-') in this column.

       ouid        OWNER     displays the Unix user identifier of the owner of the
                             session of a process, if systemd support has been included.

       pcpu        %CPU      see %cpu.  (alias %cpu).

       pending     PENDING   mask of the pending signals. See signal(7).  Signals
                             pending on the process are distinct from signals pending on
                             individual threads.  Use the m option or the -m option to
                             see both.  According to the width of the field, a 32 or 64
                             bits mask in hexadecimal format is displayed.  (alias sig).

       pgid        PGID      process group ID or, equivalently, the process ID of the
                             process group leader.  (alias pgrp).

       pgrp        PGRP      see pgid.  (alias pgid).

       pid         PID       a number representing the process ID (alias tgid).

       pidns       PIDNS     Unique inode number describing the namespace the process
                             belongs to. See namespaces(7).

       pmem        %MEM      see %mem.  (alias %mem).

       policy      POL       scheduling class of the process.  (alias class, cls).
                             Possible values are:

                                      -   not reported
                                      TS  SCHED_OTHER
                                      FF  SCHED_FIFO
                                      RR  SCHED_RR
                                      B   SCHED_BATCH
                                      ISO SCHED_ISO
                                      IDL SCHED_IDLE
                                      ?   unknown value

       ppid        PPID      parent process ID.

       pri         PRI       priority of the process.  Higher number means lower
                             priority.

       psr         PSR       processor that process is currently assigned to.

       rgid        RGID      real group ID.

       rgroup      RGROUP    real group name.  This will be the textual group ID, if it
                             can be obtained and the field width permits, or a decimal
                             representation otherwise.

       rss         RSS       resident set size, the non-swapped physical memory that a
                             task has used (in kiloBytes).  (alias rssize, rsz).

       rssize      RSS       see rss.  (alias rss, rsz).

       rsz         RSZ       see rss.  (alias rss, rssize).

       rtprio      RTPRIO    realtime priority.

       ruid        RUID      real user ID.

       ruser       RUSER     real user ID.  This will be the textual user ID, if it can
                             be obtained and the field width permits, or a decimal
                             representation otherwise.

       s           S         minimal state display (one character).  See section PROCESS
                             STATE CODES for the different values.  See also stat if you
                             want additional information displayed.  (alias state).

       sched       SCH       scheduling policy of the process.  The policies SCHED_OTHER
                             (SCHED_NORMAL), SCHED_FIFO, SCHED_RR, SCHED_BATCH,
                             SCHED_ISO, and SCHED_IDLE are respectively displayed as 0,
                             1, 2, 3, 4, and 5.

       seat        SEAT      displays the identifier associated with all hardware
                             devices assigned to a specific workplace, if systemd
                             support has been included.

       sess        SESS      session ID or, equivalently, the process ID of the session
                             leader.  (alias session, sid).

       sgi_p       P         processor that the process is currently executing on.
                             Displays "*" if the process is not currently running or
                             runnable.

       sgid        SGID      saved group ID.  (alias svgid).

       sgroup      SGROUP    saved group name.  This will be the textual group ID, if it
                             can be obtained and the field width permits, or a decimal
                             representation otherwise.

       sid         SID       see sess.  (alias sess, session).

       sig         PENDING   see pending.  (alias pending, sig_pend).

       sigcatch    CAUGHT    see caught.  (alias caught, sig_catch).

       sigignore   IGNORED   see ignored.  (alias ignored, sig_ignore).

       sigmask     BLOCKED   see blocked.  (alias blocked, sig_block).

       size        SIZE      approximate amount of swap space that would be required if
                             the process were to dirty all writable pages and then be
                             swapped out.  This number is very rough!

       slice       SLICE     displays the slice unit which a process belongs to, if
                             systemd support has been included.

       spid        SPID      see lwp.  (alias lwp, tid).

       stackp      STACKP    address of the bottom (start) of stack for the process.

       start       STARTED   time the command started.  If the process was started less
                             than 24 hours ago, the output format is "HH:MM:SS", else it
                             is "  Mmm dd" (where Mmm is a three-letter month name).
                             See also lstart, bsdstart, start_time, and stime.

       start_time  START     starting time or date of the process.  Only the year will
                             be displayed if the process was not started the same year
                             ps was invoked, or "MmmDD" if it was not started the same
                             day, or "HH:MM" otherwise.  See also
                             bsdstart, start, lstart, and stime.

       stat        STAT      multi-character process state.  See section PROCESS STATE
                             CODES for the different values meaning.  See also
                             s and state if you just want the first character displayed.

       state       S         see s. (alias s).

       suid        SUID      saved user ID.  (alias svuid).

       supgid      SUPGID    group ids of supplementary groups, if any.  See
                             getgroups(2).

       supgrp      SUPGRP    group names of supplementary groups, if any.  See
                             getgroups(2).

       suser       SUSER     saved user name.  This will be the textual user ID, if it
                             can be obtained and the field width permits, or a decimal
                             representation otherwise.  (alias svuser).

       svgid       SVGID     see sgid.  (alias sgid).

       svuid       SVUID     see suid.  (alias suid).

       sz          SZ        size in physical pages of the core image of the process.
                             This includes text, data, and stack space.  Device mappings
                             are currently excluded; this is subject to change.  See
                             vsz and rss.

       tgid        TGID      a number representing the thread group to which a task
                             belongs (alias pid).  It is the process ID of the thread
                             group leader.

       thcount     THCNT     see nlwp.  (alias nlwp).  number of kernel threads owned by
                             the process.

       tid         TID       the unique number representing a dispatchable entity (alias
                             lwp, spid).  This value may also appear as: a process ID
                             (pid); a process group ID (pgrp); a session ID for the
                             session leader (sid); a thread group ID for the thread
                             group leader (tgid); and a tty process group ID for the
                             process group leader (tpgid).

       time        TIME      cumulative CPU time, "[DD-]HH:MM:SS" format.  (alias
                             cputime).

       tname       TTY       controlling tty (terminal).  (alias tt, tty).

       tpgid       TPGID     ID of the foreground process group on the tty (terminal)
                             that the process is connected to, or -1 if the process is
                             not connected to a tty.

       trs         TRS       text resident set size, the amount of physical memory
                             devoted to executable code.

       tt          TT        controlling tty (terminal).  (alias tname, tty).

       tty         TT        controlling tty (terminal).  (alias tname, tt).

       ucmd        CMD       see comm.  (alias comm, ucomm).

       ucomm       COMMAND   see comm.  (alias comm, ucmd).

       uid         UID       see euid.  (alias euid).

       uname       USER      see euser.  (alias euser, user).

       unit        UNIT      displays unit which a process belongs to, if systemd
                             support has been included.

       user        USER      see euser.  (alias euser, uname).

       userns      USERNS    Unique inode number describing the namespace the process
                             belongs to. See namespaces(7).

       utsns       UTSNS     Unique inode number describing the namespace the process
                             belongs to. See namespaces(7).

       uunit       UUNIT     displays user unit which a process belongs to, if systemd
                             support has been included.

       vsize       VSZ       see vsz.  (alias vsz).

       vsz         VSZ       virtual memory size of the process in KiB (1024-byte
                             units).  Device mappings are currently excluded; this is
                             subject to change.  (alias vsize).

       wchan       WCHAN     name of the kernel function in which the process is
                             sleeping, a "-" if the process is running, or a "*" if the
                             process is multi-threaded and ps is not displaying threads.

ENVIRONMENT VARIABLES
       The following environment variables could affect ps:

       COLUMNS
          Override default display width.
	  覆盖默认的宽度
       LINES
          Override default display height.
	  覆盖默认的高度
       PS_PERSONALITY
          Set to one of posix, old, linux, bsd, sun, digital...  (see section
          PERSONALITY below).

       CMD_ENV
          Set to one of posix, old, linux, bsd, sun, digital...  (see section
          PERSONALITY below).

       I_WANT_A_BROKEN_PS
          Force obsolete command line interpretation.

       LC_TIME
          Date format.

       PS_COLORS
          Not currently supported.
	  当前并不支持

       PS_FORMAT
          Default output format override. You may set this to a format string of the
          type used for the -o option.  The DefSysV and DefBSD values are particularly
          useful.
	  覆盖默认的输出模式.

       POSIXLY_CORRECT
          Don't find excuses to ignore bad "features".

       POSIX2
          When set to "on", acts as POSIXLY_CORRECT.

       UNIX95
          Don't find excuses to ignore bad "features".

       _XPG
          Cancel CMD_ENV=irix non-standard behavior.

       In general, it is a bad idea to set these variables.  The one exception is
       CMD_ENV or PS_PERSONALITY, which could be set to Linux for normal systems.
       Without that setting, ps follows the useless and bad parts of the Unix98
       standard.

PERSONALITY
       390        like the OS/390 OpenEdition ps
       aix        like AIX ps
       bsd        like FreeBSD ps (totally non-standard)
       compaq     like Digital Unix ps
       debian     like the old Debian ps
       digital    like Tru64 (was Digital Unix, was OSF/1) ps
       gnu        like the old Debian ps
       hp         like HP-UX ps
       hpux       like HP-UX ps

       irix       like Irix ps
       linux      ***** recommended *****
       old        like the original Linux ps (totally non-standard)
       os390      like OS/390 Open Edition ps
       posix      standard
       s390       like OS/390 Open Edition ps
       sco        like SCO ps
       sgi        like Irix ps
       solaris2   like Solaris 2+ (SunOS 5) ps
       sunos4     like SunOS 4 (Solaris 1) ps (totally non-standard)
       svr4       standard
       sysv       standard
       tru64      like Tru64 (was Digital Unix, was OSF/1) ps
       unix       standard
       unix95     standard
       unix98     standard

SEE ALSO
       pgrep(1), pstree(1), top(1), proc(5).

STANDARDS
       This ps conforms to:

       1   Version 2 of the Single Unix Specification
       2   The Open Group Technical Standard Base Specifications, Issue 6
       3   IEEE Std 1003.1, 2004 Edition
       4   X/Open System Interfaces Extension [UP XSI]
       5   ISO/IEC 9945:2003

AUTHOR
       ps was originally written by Branko Lankester ⟨lankeste@fwi.uva.nl⟩.  Michael K.
       Johnson ⟨johnsonm@redhat.com⟩ re-wrote it significantly to use the proc filesys‐
       tem, changing a few things in the process.  Michael Shields ⟨mjshield@nyx.cs.du.
       edu⟩ added the pid-list feature.  Charles Blake ⟨cblake@bbn.com⟩ added
       multi-level sorting, the dirent-style library, the device name-to-number mmaped
       database, the approximate binary search directly on System.map, and many code and
       documentation cleanups.  David Mossberger-Tang wrote the generic BFD support for
       psupdate.  Albert Cahalan ⟨albert@users.sf.net⟩ rewrote ps for full Unix98 and
       BSD support, along with some ugly hacks for obsolete and foreign syntax.

       Please send bug reports to ⟨procps@freelists.org⟩.  No subscription is required
       or suggested.

procps-ng                              August 2015                                 PS(1)
```
