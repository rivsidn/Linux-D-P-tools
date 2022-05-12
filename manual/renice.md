```
RENICE(1)                             User Commands                            RENICE(1)

NAME
       renice - alter priority of running processes
                修改当前正在运行进程的优先级

SYNOPSIS
       renice [-n] priority [-g|-p|-u] identifier...

DESCRIPTION
       renice  alters  the  scheduling  priority  of one or more running processes.  The
       first argument is the priority value to be used.  The other arguments are  inter‐
       preted  as  process IDs (by default), process group IDs, user IDs, or user names.
       renice'ing a process group causes all processes in  the  process  group  to  have
       their  scheduling priority altered.  renice'ing a user causes all processes owned
       by the user to have their scheduling priority altered.

       修改一个或多个正在运行的进程优先级；第一个参数是进程优先级。

OPTIONS
       -n, --priority priority
              Specify the scheduling priority to be used for the process, process group,
              or user.  Use of the option -n or --priority is optional, but when used it
              must be the first argument.
	      指定应用的优先级，如果出现-n 或 --priority 的时候必须是第一个参数.

       -g, --pgrp
              Interpret the succeeding arguments as process group IDs.
	      指定进程组.

       -p, --pid
              Interpret the succeeding arguments as process IDs (the default).
	      指定进程号(默认).

       -u, --user
              Interpret the succeeding arguments as usernames or UIDs.
	      指定用户名或者用户id.

       -V, --version
              Display version information and exit.
	      显示版本号.

       -h, --help
              Display help text and exit.
	      显示帮助信息.

EXAMPLES
       The following command would change the priority of the processes  with  PIDs  987
       and 32, plus all processes owned by the users daemon and root:
       修改进程优先级(可以同时使用多个参数)

              renice +1 987 -u daemon root -p 32

NOTES
       Users other than the superuser may only alter the priority of processes they own.
       Furthermore, an unprivileged user can only increase  the  ``nice  value''  (i.e.,
       choose  a  lower  priority) and such changes are irreversible unless (since Linux
       2.6.12) the user has a suitable ``nice'' resource limit (see ulimit(1) and  getr‐
       limit(2)).

       The  superuser  may alter the priority of any process and set the priority to any
       value in the range -20 to 19.  Useful priorities are: 19 (the affected  processes
       will run only when nothing else in the system wants to), 0 (the ``base'' schedul‐
       ing priority), anything negative (to make things go very fast).

FILES
       /etc/passwd
              to map user names to user IDs

SEE ALSO
       nice(1), getpriority(2), setpriority(2), credentials(7), sched(7)

HISTORY
       The renice command appeared in 4.0BSD.

AVAILABILITY
       The renice command is part of the util-linux package and is available from  Linux
       Kernel Archive ⟨https://www.kernel.org/pub/linux/utils/util-linux/⟩.

util-linux                              July 2014                              RENICE(1)
```
