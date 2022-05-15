```
TASKSET(1)                            User Commands                           TASKSET(1)

NAME
       taskset - set or retrieve a process's CPU affinity
                 设置或者恢复CPU 亲和性

SYNOPSIS
       taskset [options] mask command [argument...]
       运行新的命令
       taskset [options] -p [mask] pid
       设置正在运行进程的CPU亲和性

DESCRIPTION
       taskset  is  used  to set or retrieve the CPU affinity of a running process given
       its pid, or to launch a new command with a given CPU affinity.  CPU affinity is a
       scheduler  property  that "bonds" a process to a given set of CPUs on the system.
       The Linux scheduler will honor the given CPU affinity and the  process  will  not
       run  on  any other CPUs.  Note that the Linux scheduler also supports natural CPU
       affinity: the scheduler attempts to keep processes on the same  CPU  as  long  as
       practical for performance reasons.  Therefore, forcing a specific CPU affinity is
       useful only in certain applications.
       taskset用于设置或者恢复CPU的亲和性，可以改变一个正在运行的进程，也可以以给定的CPU
       亲和性运行一个新的命令。

       The CPU affinity is represented as a bitmask, with the lowest  order  bit  corre‐
       sponding  to the first logical CPU and the highest order bit corresponding to the
       last logical CPU.  Not all CPUs may exist on a given system but a mask may  spec‐
       ify more CPUs than are present.  A retrieved mask will reflect only the bits that
       correspond to CPUs physically on the system.  If an invalid mask is given  (i.e.,
       one  that  corresponds  to  no  valid CPUs on the current system) an error is re‐
       turned.  The masks may be specified in hexadecimal (with  or  without  a  leading
       "0x"), or as a CPU list with the --cpu-list option.  For example,
       CPU亲和性通过掩码表示，如果掩码中有无效掩码存在，无效掩码无效，按照有效掩码设置，
       如果整个掩码中所有的掩码都是无效的则返回错误.

           0x00000001  is processor #0,

           0x00000003  is processors #0 and #1,

           0xFFFFFFFF  is processors #0 through #31,

           32          is processors #1, #4, and #5,

           --cpu-list 0-2,6
                       is processors #0, #1, #2, and #6.

           --cpu-list 0-10:2
                       is  processors #0, #2, #4, #6, #8 and #10. The suffix ":N" speci‐
                       fies stride in the range, for example 0-10:3  is  interpreted  as
                       0,3,6,9 list.

       When  taskset returns, it is guaranteed that the given program has been scheduled
       to a legal CPU.
       tasket 返回之后，确保程序已经调度到合法的CPU上.

OPTIONS
       -a, --all-tasks
              Set or retrieve the CPU affinity of all the tasks (threads)  for  a  given
              PID.
	      设置给定PID 所有线程的CPU 亲和性

       -c, --cpu-list
              Interpret mask as numerical list of processors instead of a bitmask.  Num‐
              bers are separated  by  commas  and  may  include  ranges.   For  example:
              0,5,8-11.
	      按照数字而不是掩码方式解释CPU

       -p, --pid
              Operate on an existing PID and do not launch a new task.
	      操作给定的PID而不是执行一个新的进程

       -V, --version
              Display version information and exit.
	      显示版本号

       -h, --help
              Display help text and exit.
	      显示帮助信息 

USAGE
       The default behavior is to run a new command with a given affinity mask:
              taskset mask command [arguments]

       You can also retrieve the CPU affinity of an existing task:
       恢复指定进程的CPU亲和性
              taskset -p pid

       Or set it:
       设置PID的CPU亲和性
              taskset -p mask pid

PERMISSIONS
       A  user  can  change the CPU affinity of a process belonging to the same user.  A
       user must possess CAP_SYS_NICE to change the CPU affinity of a process  belonging
       to another user.  A user can retrieve the affinity mask of any process.
       权限问题

SEE ALSO
       chrt(1), nice(1), renice(1), sched_getaffinity(2), sched_setaffinity(2)

       See sched(7) for a description of the Linux scheduling scheme.

AUTHOR
       Written by Robert M. Love.

COPYRIGHT
       Copyright © 2004 Robert M. Love.  This is free software; see the source for copy‐
       ing conditions.  There is NO warranty; not even for  MERCHANTABILITY  or  FITNESS
       FOR A PARTICULAR PURPOSE.

AVAILABILITY
       The  taskset  command  is  part  of  the util-linux package and is available from
       https://www.kernel.org/pub/linux/utils/util-linux/.

util-linux                             August 2014                            TASKSET(1)
```
