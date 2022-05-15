```
CHRT(1)                               User Commands                              CHRT(1)

NAME
       chrt - manipulate the real-time attributes of a process
              管理进程的实时调度属性

SYNOPSIS
       chrt [options] priority command [argument...]
       chrt [options] -p [priority] pid
       设置的时候必须要指定优先级，省略优先级只能是在获取当前优先级的时候.

DESCRIPTION
       chrt sets or retrieves the real-time scheduling attributes of an existing pid, or
       runs command with the given attributes.
       可以以指定优先级运行命令，也可以修改正在运行命令的优先级

POLICIES
       -o, --other
              Set scheduling policy to SCHED_OTHER.  This is the default Linux  schedul‐
              ing policy.
	      设置调度策略，这是默认的调度策略

       -b, --batch
              Set scheduling policy  to  SCHED_BATCH  (Linux-specific,  supported  since
              2.6.16).  The priority argument has to be set to zero.
	      设置调度策略

       -i, --idle
              Set  scheduling  policy  to  SCHED_IDLE  (Linux-specific,  supported since
              2.6.23).  The priority argument has to be set to zero.
	      设置调度策略

       -f, --fifo
              Set scheduling policy to SCHED_FIFO.
	      设置调度策略，为 SCHED_FIFO.

       -r, --rr
              Set  scheduling  policy  to  SCHED_RR.   When  no  policy  is defined, the
              SCHED_RR is used as the default.
	      设置调度策略为 SCHED_RR，命令执行时如果没指定策略，默认使用该策略.

       -d, --deadline
              Set scheduling policy to SCHED_DEADLINE (Linux-specific,  supported  since
              3.14).    The  priority  argument  has  to  be  set  to  zero.   See  also
              --sched-runtime, --sched-deadline and --sched-period.   The  relation  be‐
              tween the options required by the kernel is runtime <= deadline <= period.
              chrt copies period to deadline if --sched-deadline is  not  specified  and
              deadline to runtime if --sched-runtime is not specified.  It means that at
              least --sched-period has to be specified.  See sched(7) for more details.

SCHEDULING OPTIONS
       -T, --sched-runtime nanoseconds
              Specifies runtime parameter for SCHED_DEADLINE policy (Linux-specific).
	      指定运行时参数

       -P, --sched-period nanoseconds
              Specifies period parameter for SCHED_DEADLINE policy (Linux-specific).

       -D, --sched-deadline nanoseconds
              Specifies deadline parameter for SCHED_DEADLINE policy (Linux-specific).

       -R, --reset-on-fork
              Add SCHED_RESET_ON_FORK flag to the SCHED_FIFO or SCHED_RR scheduling pol‐
              icy (Linux-specific, supported since 2.6.31).

OPTIONS
       -a, --all-tasks
              Set or retrieve the scheduling attributes of all the tasks (threads) for a
              given PID.
	      修改给你PID 所有线程的调度属性

       -m, --max
              Show minimum and maximum valid priorities, then exit.
	      显示可用参数的最大值

       -p, --pid
              Operate on an existing PID and do not launch a new task.
	      操作执行PID

       -v, --verbose
              Show status information.
	      显示详细信息

       -V, --version
              Display version information and exit.
	      显示版本信息

       -h, --help
              Display help text and exit.
	      显示帮助信息

USAGE
       The default behavior is to run a new command:
              chrt priority command [arguments]

       You can also retrieve the real-time attributes of an existing task:
       获取当前进程的调度策略和优先级:
              chrt -p pid

       Or set them:
       设置:
              chrt -r -p priority pid

PERMISSIONS
       A user must possess  CAP_SYS_NICE  to  change  the  scheduling  attributes  of  a
       process.  Any user can retrieve the scheduling information.

NOTES
       Only  SCHED_FIFO,  SCHED_OTHER  and  SCHED_RR  are  part of POSIX 1003.1b Process
       Scheduling.  The other scheduling attributes may be ignored on some systems.

       Linux' default scheduling policy is SCHED_OTHER.

SEE ALSO
       nice(1), renice(1), taskset(1), sched(7)

       See sched_setscheduler(2) for a description of the Linux scheduling scheme.

AUTHORS
       Robert Love ⟨rml@tech9.net⟩
       Karel Zak ⟨kzak@redhat.com⟩

AVAILABILITY
       The chrt command is  part  of  the  util-linux  package  and  is  available  from
       https://www.kernel.org/pub/linux/utils/util-linux/.

util-linux                            January 2016                               CHRT(1)
```
