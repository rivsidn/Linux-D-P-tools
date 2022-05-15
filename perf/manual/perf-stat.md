```
PERF-STAT(1)                           perf Manual                          PERF-STAT(1)

NAME
       perf-stat - Run a command and gather performance counter statistics
                   运行一个命令并搜集性能统计信息

SYNOPSIS
       perf stat [-e <EVENT> | --event=EVENT] [-a] <command>
       perf stat [-e <EVENT> | --event=EVENT] [-a] — <command> [<options>]
       perf stat [-e <EVENT> | --event=EVENT] [-a] record [-o file] — <command> [<options>]
       perf stat report [-i file]

DESCRIPTION
       This command runs a command and gathers performance counter statistics from it.
       运行一个命令并搜集性能统计数据

OPTIONS
       <command>...
           Any command you can specify in a shell.

       record
           See STAT RECORD.

       report
           See STAT REPORT.

       -e, --event=
           Select the PMU(Performance Monitor Unit) event. Selection can be:
	   选中一个性能统计时间，可以选的可以是:

           ·   a symbolic event name (use perf list to list all events)
	       一个符号的事件名

           ·   a raw PMU event (eventsel+umask) in the form of rNNN where NNN is a
               hexadecimal event descriptor.
	       原生的PMU事件

           ·   a symbolic or raw PMU event followed by an optional colon and a list of
               event modifiers, e.g., cpu-cycles:p. See the perf-list(1) man page for
               details on event modifiers.
	       符号或原生的事件和紧随其后的事件修饰符

           ·   a symbolically formed event like pmu/param1=0x3,param2/ where param1 and
               param2 are defined as formats for the PMU in
               /sys/bus/event_source/devices/<pmu>/format/*
	       TODO:没看懂

           ·   a symbolically formed event like pmu/config=M,config1=N,config2=K/ where
               M, N, K are numbers (in decimal, hex, octal format). Acceptable values
               for each of config, config1 and config2 parameters are defined by
               corresponding entries in /sys/bus/event_source/devices/<pmu>/format/*
	       TODO:没看懂

       -i, --no-inherit
           child tasks do not inherit counters
	   子进程并不继续统计

       -p, --pid=<pid>
           stat events on existing process id (comma separated list)
	   统计进程号的事件

       -t, --tid=<tid>
           stat events on existing thread id (comma separated list)
	   统计线程号的事件

       -a, --all-cpus
           system-wide collection from all CPUs (default if no target is specified)

       -c, --scale
           scale/normalize counter values

       -d, --detailed
           print more detailed statistics, can be specified up to 3 times
	   输出更详细的统计信息，最多可以指定3次

                     -d:          detailed events, L1 and LLC data cache
                  -d -d:     more detailed events, dTLB and iTLB events
               -d -d -d:     very detailed events, adding prefetch events

       -r, --repeat=<n>
           repeat command and print average + stddev (max: 100). 0 means forever.
	   指定重复次数

       -B, --big-num
           print large numbers with thousands' separators according to locale
	   数值输出

       -C, --cpu=
           Count only on the list of CPUs provided. Multiple CPUs can be provided as a
           comma-separated list with no space: 0,1. Ranges of CPUs are specified with -:
           0-2. In per-thread mode, this option is ignored. The -a option is still
           necessary to activate system-wide monitoring. Default is to count on all
           CPUs.

       -A, --no-aggr
           Do not aggregate counts across all monitored CPUs.
	   并不将统计信息聚合显示

       -n, --null
           null run - don’t start any counters

       -v, --verbose
           be more verbose (show counter open errors, etc)

       -x SEP, --field-separator SEP
           print counts using a CSV-style output to make it easy to import directly into
           spreadsheets. Columns are separated by the string specified in SEP.
	   以其他格式输出信息，指定输出的间隔符.

       -G name, --cgroup name
           monitor only in the container (cgroup) called "name". This option is
           available only in per-cpu mode. The cgroup filesystem must be mounted. All
           threads belonging to container "name" are monitored when they run on the
           monitored CPUs. Multiple cgroups can be provided. Each cgroup is applied to
           the corresponding event, i.e., first cgroup to first event, second cgroup to
           second event and so on. It is possible to provide an empty cgroup (monitor
           all the time) using, e.g., -G foo,,bar. Cgroups must have corresponding
           events, i.e., they always refer to events defined earlier on the command
           line.
	   指定监听的容器名。

       -o file, --output file
           Print the output into the designated file.
	   输出到文件中，指定文件名

       --append
           Append to the output file designated with the -o option. Ignored if -o is not
           specified.
	   追加到指定文件

       --log-fd
           Log output to fd, instead of stderr. Complementary to --output, and mutually
           exclusive with it. --append may be used here. Examples: 3>results perf stat
           --log-fd 3  — $cmd 3>>results perf stat --log-fd 3 --append — $cmd
	   输出到文件描述符

       --pre, --post
           Pre and post measurement hooks, e.g.:

       perf stat --repeat 10 --null --sync --pre make -s O=defconfig-build/clean — make
       -s -j64 O=defconfig-build/ bzImage

       -I msecs, --interval-print msecs
           Print count deltas every N milliseconds (minimum: 10ms) The overhead
           percentage could be high in some cases, for instance with small, sub 100ms
           intervals. Use with caution. example: perf stat -I 1000 -e cycles -a sleep 5
	   指定信息输出的时间间隔

       --metric-only
           Only print computed metrics. Print them in a single line. Don’t show any raw
           values. Not supported with --per-thread.

       --per-socket
           Aggregate counts per processor socket for system-wide mode measurements. This
           is a useful mode to detect imbalance between sockets. To enable this mode,
           use --per-socket in addition to -a. (system-wide). The output includes the
           socket number and the number of online processors on that socket. This is
           useful to gauge the amount of aggregation.

       --per-core
           Aggregate counts per physical processor for system-wide mode measurements.
           This is a useful mode to detect imbalance between physical cores. To enable
           this mode, use --per-core in addition to -a. (system-wide). The output
           includes the core number and the number of online logical processors on that
           physical processor.

       --per-thread
           Aggregate counts per monitored threads, when monitoring threads (-t option)
           or processes (-p option).

       -D msecs, --delay msecs
           After starting the program, wait msecs before measuring. This is useful to
           filter out the startup phase of the program, which is often very different.
	   启动程序之后，等待一段时间才开始统计，该选项对于过滤掉程序开始的一段时间很
	   有效果。

       -T, --transaction
           Print statistics of transactional execution if supported.

STAT RECORD
       Stores stat data into perf data file.
       将统计信息存储到文件中

       -o file, --output file
           Output file name.

STAT REPORT
       Reads and reports stat data from perf data file.

       -i file, --input file
           Input file name.

       --per-socket
           Aggregate counts per processor socket for system-wide mode measurements.
	   系统范围内的数据，针对每个处理器聚合显示

       --per-core
           Aggregate counts per physical processor for system-wide mode measurements.
	   系统范围内的数据，针对每个物理处理器聚合显示

       -M, --metrics
           Print metrics(指标) or metricgroups(指标组) specified in a comma separated list. For a
           group all metrics from the group are added. The events from the metrics are
           automatically measured. See perf list output for the possble metrics and
           metricgroups.

       -A, --no-aggr
           Do not aggregate counts across all monitored CPUs.
	   不聚合显示统计信息.

       --topdown
           Print top down level 1 metrics if supported by the CPU. This allows to
           determine bottle necks in the CPU pipeline for CPU bound workloads, by
           breaking the cycles consumed down into frontend bound, backend bound, bad
           speculation and retiring.
	   如果CPU支持，输出top down level 1 指标.

       Frontend bound means that the CPU cannot fetch and decode instructions fast
       enough. Backend bound means that computation or memory access is the bottle neck.
       Bad Speculation means that the CPU wasted cycles due to branch mispredictions and
       similar issues. Retiring means that the CPU computed without an apparently
       bottleneck. The bottleneck is only the real bottleneck if the workload is
       actually bound by the CPU and not by something else.
       前绑定表示取指令、解码不够快；后绑定表示计算、内存访问是瓶颈；坏的预测表示CPU在
       错误的预测上浪费了指令周期；Retiring表示CPU运行过程中没有明显的瓶颈。

       For best results it is usually a good idea to use it with interval mode like -I
       1000, as the bottleneck of workloads can change often.

       The top down metrics are collected per core instead of per CPU thread. Per core
       mode is automatically enabled and -a (global monitoring) is needed, requiring
       root rights or perf.perf_event_paranoid=-1.

       Topdown uses the full Performance Monitoring Unit, and needs disabling of the NMI
       watchdog (as root): echo 0 > /proc/sys/kernel/nmi_watchdog for best results.
       Otherwise the bottlenecks may be inconsistent on workload with changing phases.

       This enables --metric-only, unless overriden with --no-metric-only.

       To interpret the results it is usually needed to know on which CPUs the workload
       runs on. If needed the CPUs can be forced using taskset.

       --no-merge
           Do not merge results from same PMUs.

       --smi-cost
           Measure SMI cost if msr/aperf/ and msr/smi/ events are supported.

       During the measurement, the /sys/device/cpu/freeze_on_smi will be set to freeze
       core counters on SMI. The aperf counter will not be effected by the setting. The
       cost of SMI can be measured by (aperf - unhalted core cycles).

       In practice, the percentages of SMI cycles is very useful for performance
       oriented analysis. --metric_only will be applied by default. The output is SMI
       cycles%, equals to (aperf - unhalted core cycles) / aperf

       Users who wants to get the actual value can apply --no-metric-only.

EXAMPLES
       $ perf stat — make -j

           Performance counter stats for 'make -j':

           8117.370256  task clock ticks     #      11.281 CPU utilization factor
                   678  context switches     #       0.000 M/sec
                   133  CPU migrations       #       0.000 M/sec
                235724  pagefaults           #       0.029 M/sec
           24821162526  CPU cycles           #    3057.784 M/sec
           18687303457  instructions         #    2302.138 M/sec
             172158895  cache references     #      21.209 M/sec
              27075259  cache misses         #       3.335 M/sec

           Wall-clock time elapsed:   719.554352 msecs

CSV FORMAT
       With -x, perf stat is able to output a not-quite-CSV format output Commas in the
       output are not put into "". To make it easy to parse it is recommended to use a
       different character like -x \;

       The fields are in this order:

       ·   optional usec time stamp in fractions of second (with -I xxx)

       ·   optional CPU, core, or socket identifier

       ·   optional number of logical CPUs aggregated

       ·   counter value

       ·   unit of the counter value or empty

       ·   event name

       ·   run time of counter

       ·   percentage of measurement time the counter was running

       ·   optional variance if multiple values are collected with -r

       ·   optional metric value

       ·   optional unit of metric

       Additional metrics may be printed with all earlier fields being empty.

SEE ALSO
       perf-top(1), perf-list(1)

perf                                   04/14/2022                           PERF-STAT(1)
```
