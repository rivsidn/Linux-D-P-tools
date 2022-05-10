PERF-EVLIST(1)                         perf Manual                        PERF-EVLIST(1)

NAME
       perf-evlist - List the event names in a perf.data file
                     列出perf.data 中的事件名称

SYNOPSIS
       perf evlist <options>

DESCRIPTION
       This command displays the names of events sampled in a perf.data file.

OPTIONS
       -i, --input=
           Input file name. (default: perf.data unless stdin is a fifo)

       -f, --force
           Don’t complain, do it.

       -F, --freq=
           Show just the sample frequency used for each event.

       -v, --verbose=
           Show all fields.

       -g, --group
           Show event group information.

       --trace-fields
           Show tracepoint field names.

SEE ALSO
       perf-record(1), perf-list(1), perf-report(1)

perf                                   03/29/2022                         PERF-EVLIST(1)
