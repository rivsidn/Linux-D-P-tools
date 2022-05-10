PERF-INJECT(1)                         perf Manual                        PERF-INJECT(1)

NAME
       perf-inject - Filter to augment the events stream with additional information
                     用于增加事件流的过滤器

SYNOPSIS
       perf inject <options>

DESCRIPTION
       perf-inject reads a perf-record event stream and repipes it to stdout. At any
       point the processing code can inject other events into the event stream - in this
       case build-ids (-b option) are read and injected as needed into the event stream.

       Build-ids are just the first user of perf-inject - potentially anything that
       needs userspace processing to augment the events stream with additional
       information could make use of this facility.

OPTIONS
       -b, --build-ids=
           Inject build-ids into the output stream

       -v, --verbose
           Be more verbose.

       -i, --input=
           Input file name. (default: stdin)

       -o, --output=
           Output file name. (default: stdout)

       -s, --sched-stat
           Merge sched_stat and sched_switch for getting events where and how long tasks
           slept. sched_switch contains a callchain where a task slept and sched_stat
           contains a timeslice how long a task slept.

       --kallsyms=<file>
           kallsyms pathname

       --itrace
           Decode Instruction Tracing data, replacing it with synthesized events.
           Options are:

               i       synthesize instructions events
               b       synthesize branches events
               c       synthesize branches events (calls only)
               r       synthesize branches events (returns only)
               x       synthesize transactions events
               w       synthesize ptwrite events
               p       synthesize power events
               o       synthesize other events recorded due to the use
                       of aux-output (refer to perf record)
               e       synthesize error events
               d       create a debug log
               g       synthesize a call chain (use with i or x)
               l       synthesize last branch entries (use with i or x)
               s       skip initial number of events

               The default is all events i.e. the same as --itrace=ibxwpe,
               except for perf script where it is --itrace=ce

               In addition, the period (default 100000, except for perf script where it is 1)
               for instructions events can be specified in units of:

               i       instructions
               t       ticks
               ms      milliseconds
               us      microseconds
               ns      nanoseconds (default)

               Also the call chain size (default 16, max. 1024) for instructions or
               transactions events can be specified.

               Also the number of last branch entries (default 64, max. 1024) for
               instructions or transactions events can be specified.

               It is also possible to skip events generated (instructions, branches, transactions,
               ptwrite, power) at the beginning. This is useful to ignore initialization code.

               --itrace=i0nss1000000

               skips the first million instructions.

       --strip
           Use with --itrace to strip out non-synthesized events.

       -j, --jit
           Process jitdump files by injecting the mmap records corresponding to jitted
           functions. This option also generates the ELF images for each jitted function
           found in the jitdumps files captured in the input perf.data file. Use this
           option if you are monitoring environment using JIT runtimes, such as Java,
           DART or V8.

       -f, --force
           Don’t complain, do it.

SEE ALSO
       perf-record(1), perf-report(1), perf-archive(1)

perf                                   03/29/2022                         PERF-INJECT(1)
