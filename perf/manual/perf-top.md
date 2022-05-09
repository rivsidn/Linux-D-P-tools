```
PERF-TOP(1)                            perf Manual                           PERF-TOP(1)

NAME
       perf-top - System profiling tool.
                  系统采样工具

SYNOPSIS
       perf top [-e <EVENT> | --event=EVENT] [<options>]

DESCRIPTION
       This command generates and displays a performance counter profile in real time.
       实时显示性能统计信息

OPTIONS
       -a, --all-cpus
           System-wide collection. (default)
	   默认统计系统范围内的信息

       -c <count>, --count=<count>
           Event period to sample.
	   事件采样周期

       -C <cpu-list>, --cpu=<cpu>
           Monitor only on the list of CPUs provided. Multiple CPUs can be provided as a
           comma-separated list with no space: 0,1. Ranges of CPUs are specified with -:
           0-2. Default is to monitor all CPUS.
	   只监控提供的CPUs列表

       -d <seconds>, --delay=<seconds>
           Number of seconds to delay between refreshes.
	   刷新的时间间隔，单位为秒

       -e <event>, --event=<event>
           Select the PMU event. Selection can be a symbolic event name (use perf list
           to list all events) or a raw PMU event (eventsel+umask) in the form of rNNN
           where NNN is a hexadecimal event descriptor.
	   选择PMU事件，选项可以是符号的事件名或者是原始的PMU事件

       -E <entries>, --entries=<entries>
           Display this many functions.

       -f <count>, --count-filter=<count>
           Only display functions with more events than this.

       --group
           Put the counters into a counter group.

       -F <freq>, --freq=<freq>
           Profile at this frequency. Use max to use the currently maximum allowed
           frequency, i.e. the value in the kernel.perf_event_max_sample_rate sysctl.

       -i, --inherit
           Child tasks do not inherit counters.
	   子进程并不继承统计

       -k <path>, --vmlinux=<path>
           Path to vmlinux. Required for annotation functionality.
	   vmlinux路径

       --ignore-vmlinux
           Ignore vmlinux files.

       --kallsyms=<file>
           kallsyms pathname
	   内核符号表路径

       -m <pages>, --mmap-pages=<pages>
           Number of mmap data pages (must be a power of two) or size specification with
           appended unit character - B/K/M/G. The size is rounded up to have nearest
           pages power of two value.

       -p <pid>, --pid=<pid>
           Profile events on existing Process ID (comma separated list).
	   监控特定的进程号

       -t <tid>, --tid=<tid>
           Profile events on existing thread ID (comma separated list).
	   监控特定的线程号

       -u, --uid=
           Record events in threads owned by uid. Name or number.
	   监控特定用户的事件(名称或数字)

       -r <priority>, --realtime=<priority>
           Collect data with this RT SCHED_FIFO priority.
	   设置搜集信息的实时进程优先级

       --sym-annotate=<symbol>
           Annotate this symbol.
	   注释该符号

       -K, --hide_kernel_symbols
           Hide kernel symbols.
	   隐藏内核符号

       -U, --hide_user_symbols
           Hide user symbols.
	   隐藏用户态符号

       --demangle-kernel
           Demangle kernel symbols.
	   拆解内核符号

       -D, --dump-symtab
           Dump the symbol table used for profiling.
	   dump统计的符号表

       -v, --verbose
           Be more verbose (show counter open errors, etc).
	   显示更多信息

       -z, --zero
           Zero history across display updates.
	   显示更新间清空历史记录

       -s, --sort
           Sort by key(s): pid, comm, dso, symbol, parent, srcline, weight,
           local_weight, abort, in_tx, transaction, overhead, sample, period. Please see
           description of --sort in the perf-report man page.
           按照特定信息排序

       --fields=
           Specify output field - multiple keys can be specified in CSV format.
           Following fields are available: overhead, overhead_sys, overhead_us,
           overhead_children, sample and period. Also it can contain any sort key(s).

               By default, every sort keys not specified in --field will be appended
               automatically.

       -n, --show-nr-samples
           Show a column with the number of samples.
	   显示采样次数栏

       --show-total-period
           Show a column with the sum of periods.
	   多显示period 一栏，应该是时间(具体单位是什么？)

       --dsos
           Only consider symbols in these dsos. This option will affect the percentage
           of the overhead column. See --percentage for more info.

       --comms
           Only consider symbols in these comms. This option will affect the percentage
           of the overhead column. See --percentage for more info.

       --symbols
           Only consider these symbols. This option will affect the percentage of the
           overhead column. See --percentage for more info.

       -M, --disassembler-style=
           Set disassembler style for objdump.
	   显示反汇编样式

       --source
           Interleave source code with assembly code. Enabled by default, disable with
           --no-source.

       --asm-raw
           Show raw instruction encoding of assembly instructions.

       -g
           Enables call-graph (stack chain/backtrace) recording.
	   生成调用关系图记录

       --call-graph [mode,type,min[,limit],order[,key][,branch]]
           Setup and enable call-graph (stack chain/backtrace) recording, implies -g.
           See --call-graph section in perf-record and perf-report man pages for
           details.

       --children
           Accumulate callchain of children to parent entry so that then can show up in
           the output. The output will have a new "Children" column and will be sorted
           on the data. It requires -g/--call-graph option enabled. See the ‘overhead
           calculation’ section for more details. Enabled by default, disable with
           --no-children.

       --max-stack
           Set the stack depth limit when parsing the callchain, anything beyond the
           specified depth will be ignored. This is a trade-off between information loss
           and faster processing especially for workloads that can have a very long
           callchain stack.

               Default: /proc/sys/kernel/perf_event_max_stack when present, 127 otherwise.

       --ignore-callees=<regex>
           Ignore callees of the function(s) matching the given regex. This has the
           effect of collecting the callers of each such function into one place in the
           call-graph tree.

       --percent-limit
           Do not show entries which have an overhead under that percent. (Default: 0).
	   不显示低于该百分比的消耗

       --percentage
           Determine how to display the overhead percentage of filtered entries. Filters
           can be applied by --comms, --dsos and/or --symbols options and Zoom
           operations on the TUI (thread, dso, etc).
	   如何显示过滤表项的百分比显示.

               "relative" means it's relative to filtered entries only so that the
               sum of shown entries will be always 100%. "absolute" means it retains
               the original value before and after the filter is applied.

       -w, --column-widths=<width[,width...]>
           Force each column width to the provided list, for large terminal readability.
           0 means no limit (default behavior).
	   限制列宽

       --proc-map-timeout
           When processing pre-existing threads /proc/XXX/mmap, it may take a long time,
           because the file may be huge. A time out is needed in such cases. This option
           sets the time out limit. The default value is 500 ms.

       -b, --branch-any
           Enable taken branch stack sampling. Any type of taken branch may be sampled.
           This is a shortcut for --branch-filter any. See --branch-filter for more
           infos.

       -j, --branch-filter
           Enable taken branch stack sampling. Each sample captures a series of
           consecutive taken branches. The number of branches captured with each sample
           depends on the underlying hardware, the type of branches of interest, and the
           executed code. It is possible to select the types of branches captured by
           enabling filters. For a full list of modifiers please see the perf record
           manpage.

               The option requires at least one branch type among any, any_call, any_ret, ind_call, cond.
               The privilege levels may be omitted, in which case, the privilege levels of the associated
               event are applied to the branch filter. Both kernel (k) and hypervisor (hv) privilege
               levels are subject to permissions.  When sampling on multiple events, branch stack sampling
               is enabled for all the sampling events. The sampled branch type is the same for all events.
               The various filters must be specified as a comma separated list: --branch-filter any_ret,u,k
               Note that this feature may not be available on all processors.

       --raw-trace
           When displaying traceevent output, do not use print fmt or plugins.

       --hierarchy
           Enable hierarchy output.
	   使能层级显示

       --overwrite
           Enable this to use just the most recent records, which helps in high core
           count machines such as Knights Landing/Mill, but right now is disabled by
           default as the pausing used in this technique is leading to loss of metadata
           events such as PERF_RECORD_MMAP which makes perf top unable to resolve
           samples, leading to lots of unknown samples appearing on the UI. Enable this
           if you are in such machines and profiling a workload that doesn’t creates
           short lived threads and/or doesn’t uses many executable mmap operations. Work
           is being planed to solve this situation, till then, this will remain disabled
           by default.

       --force
           Don’t do ownership validation.

       --num-thread-synthesize
           The number of threads to run when synthesizing events for existing processes.
           By default, the number of threads equals to the number of online CPUs.

       --namespaces
           Record events of type PERF_RECORD_NAMESPACES and display it with the
           cgroup_id sort key.

       --switch-on EVENT_NAME
           Only consider events after this event is found.

               E.g.:

               Find out where broadcast packets are handled

               perf probe -L icmp_rcv

               Insert a probe there:

               perf probe icmp_rcv:59

               Start perf top and ask it to only consider the cycles events when a
               broadcast packet arrives This will show a menu with two entries and
               will start counting when a broadcast packet arrives:

               perf top -e cycles,probe:icmp_rcv --switch-on=probe:icmp_rcv

               Alternatively one can ask for --group and then two overhead columns
               will appear, the first for cycles and the second for the switch-on event.

               perf top --group -e cycles,probe:icmp_rcv --switch-on=probe:icmp_rcv

               This may be interesting to measure a workload only after some initialization
               phase is over, i.e. insert a perf probe at that point and use the above
               examples replacing probe:icmp_rcv with the just-after-init probe.

       --switch-off EVENT_NAME
           Stop considering events after this event is found.

       --show-on-off-events
           Show the --switch-on/off events too. This has no effect in perf top now but
           probably we’ll make the default not to show the switch-on/off events on the
           --group mode and if there is only one event besides the off/on ones, go
           straight to the histogram browser, just like perf top with no events
           explicitely specified does.

INTERACTIVE PROMPTING KEYS
       [d]
           Display refresh delay.
	   显示刷新间隔

       [e]
           Number of entries to display.
	   显示的表项个数

       [E]
           Event to display when multiple counters are active.

       [f]
           Profile display filter (>= hit count).

       [F]
           Annotation display filter (>= % of total).

       [s]
           Annotate symbol.

       [S]
           Stop annotation, return to full profile display.

       [K]
           Hide kernel symbols.

       [U]
           Hide user symbols.

       [z]
           Toggle event count zeroing across display updates.

       [qQ]
           Quit.

       Pressing any unmapped key displays a menu, and prompts for input.

OVERHEAD CALCULATION
       The overhead can be shown in two columns as Children and Self when perf collects
       callchains. The self overhead is simply calculated by adding all period values of
       the entry - usually a function (symbol). This is the value that perf shows
       traditionally and sum of all the self overhead values should be 100%.

       The children overhead is calculated by adding all period values of the child
       functions so that it can show the total overhead of the higher level functions
       even if they don’t directly execute much. Children here means functions that are
       called from another (parent) function.

       It might be confusing that the sum of all the children overhead values exceeds
       100% since each of them is already an accumulation of self overhead of its child
       functions. But with this enabled, users can find which function has the most
       overhead even if samples are spread over the children.

       Consider the following example; there are three functions like below.

           .ft C
           void foo(void) {
               /* do something */
           }

           void bar(void) {
               /* do something */
               foo();
           }

           int main(void) {
               bar()
               return 0;
           }
           .ft

       In this case foo is a child of bar, and bar is an immediate child of main so foo
       also is a child of main. In other words, main is a parent of foo and bar, and bar
       is a parent of foo.

       Suppose all samples are recorded in foo and bar only. When it’s recorded with
       callchains the output will show something like below in the usual
       (self-overhead-only) output of perf report:

           .ft C
           Overhead  Symbol
           ........  .....................
             60.00%  foo
                     |
                     --- foo
                         bar
                         main
                         __libc_start_main

             40.00%  bar
                     |
                     --- bar
                         main
                         __libc_start_main
           .ft

       When the --children option is enabled, the self overhead values of child
       functions (i.e. foo and bar) are added to the parents to calculate the children
       overhead. In this case the report could be displayed as:

           .ft C
           Children      Self  Symbol
           ........  ........  ....................
            100.00%     0.00%  __libc_start_main
                     |
                     --- __libc_start_main

            100.00%     0.00%  main
                     |
                     --- main
                         __libc_start_main

            100.00%    40.00%  bar
                     |
                     --- bar
                         main
                         __libc_start_main

             60.00%    60.00%  foo
                     |
                     --- foo
                         bar
                         main
                         __libc_start_main
           .ft

       In the above output, the self overhead of foo (60%) was add to the children
       overhead of bar, main and __libc_start_main. Likewise, the self overhead of bar
       (40%) was added to the children overhead of main and \_\_libc_start_main.

       So \_\_libc_start_main and main are shown first since they have same (100%)
       children overhead (even though they have zero self overhead) and they are the
       parents of foo and bar.

       Since v3.16 the children overhead is shown by default and the output is sorted by
       its values. The children overhead is disabled by specifying --no-children option
       on the command line or by adding report.children = false or top.children = false
       in the perf config file.

SEE ALSO
       perf-stat(1), perf-list(1), perf-report(1)

perf                                   03/29/2022                            PERF-TOP(1)
```
