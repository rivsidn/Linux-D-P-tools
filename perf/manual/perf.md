```
PERF(1)                                perf Manual                               PERF(1)

NAME
       perf - Performance analysis tools for Linux
              Linux性能分析工具

SYNOPSIS
       perf [--version] [--help] [OPTIONS] COMMAND [ARGS]

OPTIONS
       --debug
           Setup debug variable (see list below) in value range (0, 10). Use like:
           --debug verbose # sets verbose = 1 --debug verbose=2 # sets verbose = 2

               List of debug variables allowed to set:
	       设置调试信息的范围:
                 verbose          - general debug messages
                 ordered-events   - ordered events object debug messages
                 data-convert     - data convert command debug messages
                 stderr           - write debug output (option -v) to stderr
                                    in browser mode

       --buildid-dir
           Setup buildid cache directory. It has higher priority than buildid.dir config
           file option.

       -v, --version
           Display perf version.
	   显示版本信息

       -h, --help
           Run perf help command.
	   显示帮助信息

DESCRIPTION
       Performance counters for Linux are a new kernel-based subsystem that provide a
       framework for all things performance analysis. It covers hardware level (CPU/PMU,
       Performance Monitoring Unit) features and software features (software counters,
       tracepoints) as well.
       性能计数器是一个提供性能监控的内核子系统，同时覆盖软件硬件.

SEE ALSO
       perf-stat(1), perf-top(1), perf-record(1), perf-report(1), perf-list(1)

perf                                   03/29/2022                                PERF(1)
```
