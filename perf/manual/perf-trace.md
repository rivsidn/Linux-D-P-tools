```
PERF-TRACE(1)                          perf Manual                         PERF-TRACE(1)

NAME
       perf-trace - strace inspired tool

SYNOPSIS
       perf trace
       perf trace record

DESCRIPTION
       This command will show the events associated with the target, initially syscalls,
       but other system events like pagefaults, task lifetime events, scheduling events,
       etc.
       该命令可以用于显示进程相关的某些事件，默认是系统调用还可以是缺页异常、调度事件、
       生命周期事件等.

       This is a live mode tool in addition to working with perf.data files like the
       other perf tools. Files can be generated using the perf record command but the
       session needs to include the raw_syscalls events (-e raw_syscalls:*).
       Alternatively, perf trace record can be used as a shortcut to automatically
       include the raw_syscalls events when writing events to a file.

       The following options apply to perf trace; options to perf trace record are found
       in the perf record man page.

OPTIONS
       -a, --all-cpus
           System-wide collection from all CPUs.
	   监控所有CPU

       -e, --expr, --event
           List of syscalls and other perf events (tracepoints, HW cache events, etc) to
           show. Globbing is supported, e.g.: "epoll_*", "msg", etc. See perf list for a
           complete list of events. Prefixing with ! shows all syscalls but the ones
           specified. You may need to escape it.
	   列出系统调用和其他perf事件.

       -D msecs, --delay msecs
           After starting the program, wait msecs before measuring. This is useful to
           filter out the startup phase of the program, which is often very different.
	   启动程序之后，等待一段时间后才开始统计，可以过滤掉程序的启动阶段。

       -o, --output=
           Output file name.
	   输出文件名

       -p, --pid=
           Record events on existing process ID (comma separated list).
	   统计进程号的事件

       -t, --tid=
           Record events on existing thread ID (comma separated list).
	   统计线程的事件

       -u, --uid=
           Record events in threads owned by uid. Name or number.

       --filter-pids=
           Filter out events for these pids and for trace itself (comma separated list).
	   过滤掉该这部分进程的事件

       -v, --verbose=
           Verbosity level.

       --no-inherit
           Child tasks do not inherit counters.
	   不统计子进程

       -m, --mmap-pages=
           Number of mmap data pages (must be a power of two) or size specification with
           appended unit character - B/K/M/G. The size is rounded up to have nearest
           pages power of two value.

       -C, --cpu
           Collect samples only on the list of CPUs provided. Multiple CPUs can be
           provided as a comma-separated list with no space: 0,1. Ranges of CPUs are
           specified with -: 0-2. In per-thread mode with inheritance mode on (default),
           Events are captured only when the thread executes on the designated CPUs.
           Default is to monitor all CPUs.

       --duration: Show only events that had a duration greater than N.M ms.
                   只显示持续时间大于N.M ms的事件

       --sched: Accrue thread runtime and provide a summary at the end of the session.
                累积线程运行时间，最终提供一个列表

       -i --input Process events from a given perf data file.
                  处理perf data文件中事件

       -T --time Print full timestamp rather time relative to first sample.
                 输出时间戳而不是相对时间

       --comm
           Show process COMM right beside its ID, on by default, disable with --no-comm.
	   显示进程名

       -s, --summary
           Show only a summary of syscalls by thread with min, max, and average times
           (in msec) and relative stddev.
	   显示统计信息

       -S, --with-summary
           Show all syscalls followed by a summary by thread with min, max, and average
           times (in msec) and relative stddev.

       --tool_stats
           Show tool stats such as number of times fd→pathname was discovered thru
           hooking the open syscall return + vfs_getname or via reading /proc/pid/fd,
           etc.

       -F=[all|min|maj], --pf=[all|min|maj]
           Trace pagefaults. Optionally, you can specify whether you want minor, major
           or all pagefaults. Default value is maj.

       --syscalls
           Trace system calls. This options is enabled by default, disable with
           --no-syscalls.

       --call-graph [mode,type,min[,limit],order[,key][,branch]]
           Setup and enable call-graph (stack chain/backtrace) recording. See
           --call-graph section in perf-record and perf-report man pages for details.
           The ones that are most useful in perf trace are dwarf and lbr, where
           available, try: perf trace --call-graph dwarf.

               Using this will, for the root user, bump the value of --mmap-pages to 4
               times the maximum for non-root users, based on the kernel.perf_event_mlock_kb
               sysctl. This is done only if the user doesn't specify a --mmap-pages value.

       --kernel-syscall-graph
           Show the kernel callchains on the syscall exit path.

       --max-stack
           Set the stack depth limit when parsing the callchain, anything beyond the
           specified depth will be ignored. Note that at this point this is just about
           the presentation part, i.e. the kernel is still not limiting, the overhead of
           callchains needs to be set via the knobs in --call-graph dwarf.

               Implies '--call-graph dwarf' when --call-graph not present on the
               command line, on systems where DWARF unwinding was built in.

               Default: /proc/sys/kernel/perf_event_max_stack when present for
                        live sessions (without --input/-i), 127 otherwise.

       --min-stack
           Set the stack depth limit when parsing the callchain, anything below the
           specified depth will be ignored. Disabled by default.

               Implies '--call-graph dwarf' when --call-graph not present on the
               command line, on systems where DWARF unwinding was built in.

       --proc-map-timeout
           When processing pre-existing threads /proc/XXX/mmap, it may take a long time,
           because the file may be huge. A time out is needed in such cases. This option
           sets the time out limit. The default value is 500 ms.

PAGEFAULTS
       When tracing pagefaults, the format of the trace is as follows:

       <min|maj>fault [<ip.symbol>+<ip.offset>] ⇒ <addr.dso@addr.offset[1]> (<map
       type><addr level>).

       ·   min/maj indicates whether fault event is minor or major;

       ·   ip.symbol shows symbol for instruction pointer (the code that generated the
           fault); if no debug symbols available, perf trace will print raw IP;

       ·   addr.dso shows DSO for the faulted address;

       ·   map type is either d for non-executable maps or x for executable maps;

       ·   addr level is either k for kernel dso or .  for user dso.

       For symbols resolution you may need to install debugging symbols.

       Please be aware that duration is currently always 0 and doesn’t reflect actual
       time it took for fault to be handled!

       When --verbose specified, perf trace tries to print all available information for
       both IP and fault address in the form of dso@symbol[2]+offset.

EXAMPLES
       Trace only major pagefaults:

           $ perf trace --no-syscalls -F

       Trace syscalls, major and minor pagefaults:

           $ perf trace -F all

           1416.547 ( 0.000 ms): python/20235 majfault [CRYPTO_push_info_+0x0] => /lib/x86_64-linux-gnu/libcrypto.so.1.0.0@0x61be0 (x.)

           As you can see, there was major pagefault in python process, from
           CRYPTO_push_info_ routine which faulted somewhere in libcrypto.so.

SEE ALSO
       perf-record(1), perf-script(1)

NOTES
        1. addr.dso@addr.offset
           mailto:addr.dso@addr.offset

        2. dso@symbol
           mailto:dso@symbol

perf                                   04/14/2022                          PERF-TRACE(1)
```

