PERF-BUILDID-LIST(1)                   perf Manual                  PERF-BUILDID-LIST(1)

NAME
       perf-buildid-list - List the buildids in a perf.data file
                           列出perf.data 文件中的buildids 信息

SYNOPSIS
       perf buildid-list <options>

DESCRIPTION
       This command displays the buildids found in a perf.data file, so that other tools
       can be used to fetch packages with matching symbol tables for use by perf report.

       It can also be used to show the build id of the running kernel or in an ELF file
       using -i/--input.

OPTIONS
       -H, --with-hits
           Show only DSOs with hits.
	   只显示命中的DSO.

       -i, --input=
           Input file name. (default: perf.data unless stdin is a fifo)
	   输入文件名

       -f, --force
           Don’t do ownership validation.
	   不进行所有权认证

       -k, --kernel
           Show running kernel build id.
	   显示内核的build id

       -v, --verbose
           Be more verbose.

SEE ALSO
       perf-record(1), perf-top(1), perf-report(1)

perf                                   03/29/2022                   PERF-BUILDID-LIST(1)
