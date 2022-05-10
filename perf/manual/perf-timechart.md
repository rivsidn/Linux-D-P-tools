```
PERF-TIMECHART(1)                      perf Manual                     PERF-TIMECHART(1)

NAME
       perf-timechart - Tool to visualize total system behavior during a workload
                        可视化工具
SYNOPSIS
DESCRIPTION
       There are two variants of perf timechart:

           'perf timechart record <command>' to record the system level events
           of an arbitrary workload. By default timechart records only scheduler
           and CPU events (task switches, running times, CPU power states, etc),
           but it's possible to record IO (disk, network) activity using -I argument.
	   统计系统级的事件，默认情况下只统计调度事件和CPU事件(进程切换、运行时间、CPU电源状态等)，
	   也可以通过 -I 参数指定IO 事件。

           'perf timechart' to turn a trace into a Scalable Vector Graphics file,
           that can be viewed with popular SVG viewers such as 'Inkscape'. Depending
           on the events in the perf.data file, timechart will contain scheduler/cpu
           events or IO events.
	   转换trace信息为一个矢量图，可以通过图片查看器查看。
	   适量图包含的信息依赖与perf.data文件中的内容。

           In IO mode, every bar has two charts: upper and lower.
           Upper bar shows incoming events (disk reads, ingress network packets).
           Lower bar shows outgoing events (disk writes, egress network packets).
           There are also poll bars which show how much time application spent
           in poll/epoll/select syscalls.

TIMECHART OPTIONS
       -o, --output=
           Select the output file (default: output.svg)
	   指定要输出的文件

       -i, --input=
           Select the input file (default: perf.data unless stdin is a fifo)
	   指定要输入的文件

       -w, --width=
           Select the width of the SVG file (default: 1000)
	   指定SVG文件的宽度

       -P, --power-only
           Only output the CPU power section of the diagram
	   只输出图标中CPU电源段的信息

       -T, --tasks-only
           Don’t output processor state transitions

       -p, --process
           Select the processes to display, by name or PID
	   选择要输出的进程

       --symfs=<directory>
           Look for files with symbols relative to this directory.

       -n, --proc-num
           Print task info for at least given number of tasks.

       -t, --topology
           Sort CPUs according to topology.
	   按照拓扑顺序给CPU排序

       --highlight=<duration_nsecs|task_name>
           Highlight tasks (using different color) that run more than given duration or
           tasks with given name. If number is given it’s interpreted as number of
           nanoseconds. If non-numeric string is given it’s interpreted as task name.

       --io-skip-eagain
           Don’t draw EAGAIN IO events.

       --io-min-time=<nsecs>
           Draw small events as if they lasted min-time. Useful when you need to see
           very small and fast IO. It’s possible to specify ms or us suffix to specify
           time in milliseconds or microseconds. Default value is 1ms.

       --io-merge-dist=<nsecs>
           Merge events that are merge-dist nanoseconds apart. Reduces number of figures
           on the SVG and makes it more render-friendly. It’s possible to specify ms or
           us suffix to specify time in milliseconds or microseconds. Default value is
           1us.

RECORD OPTIONS
       -P, --power-only
           Record only power-related events
	   只记录电源相关的事件

       -T, --tasks-only
           Record only tasks-related events
	   只记录进程相关的事件

       -I, --io-only
           Record only io-related events
	   记录IO相关的事件

       -g, --callchain
           Do call-graph (stack chain/backtrace) recording
	   调用栈记录

EXAMPLES
       $ perf timechart record git pull

           [ perf record: Woken up 13 times to write data ]
           [ perf record: Captured and wrote 4.253 MB perf.data (~185801 samples) ]

       $ perf timechart

           Written 10.2 seconds of trace to output.svg.

       Record system-wide timechart:

           $ perf timechart record

           then generate timechart and highlight 'gcc' tasks:

           $ perf timechart --highlight gcc

       Record system-wide IO events:

           $ perf timechart record -I

           then generate timechart:

           $ perf timechart

SEE ALSO
       perf-record(1)

perf                                   04/14/2022                      PERF-TIMECHART(1)
```
