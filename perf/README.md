## 工具整理

| 工具 | 工具使用说明 |
| ---- | ------------ |
|      |              |
|      |              |
|      |              |



## 用户态性能分析

* 用户态指令周期
* 内核态指令周期
* 系统调用信息
* 





## 内核态性能分析









## TODO

* perf执行的原理？采样频率如何理解？
* 如何理解`perf` 中支持的各种事件？
* `intel` 中的各种`PMU`？
* 如何搜集用户态进程的系统调用信息？
* 如何统计内核中某函数的指令周期
* 分支选项如何使用？



* 如何判断函数是否好好利用了硬件`cache` ？
* 硬件流水线技术？超标量体系结构？乱序执行？如何判断是否好好利用了这些技术？
* 



## 参考资料

* [借助PERF工具分析CPU利用率](http://linuxperf.com/?p=36)
* [Linux kernel profiling with perf](https://perf.wiki.kernel.org/index.php/Tutorial)






```c
I_yuchao@yuchao:~/Documents/github/linux-4.4.155/tools/perf/Documentation$ la -la *.txt
-rw-r--r-- 1 yuchao yuchao  3096 11月 16 10:19 android.txt
-rw-r--r-- 1 yuchao yuchao  1315 11月 16 10:19 Build.txt
-rw-r--r-- 1 yuchao yuchao  3430 11月 16 10:19 callchain-overhead-calculation.txt
-rw-r--r-- 1 yuchao yuchao  8699 5月  10 10:14 examples.txt
-rw-r--r-- 1 yuchao yuchao  2383 11月 16 10:19 intel-bts.txt
-rw-r--r-- 1 yuchao yuchao 29181 11月 16 10:19 intel-pt.txt
-rw-r--r-- 1 yuchao yuchao   823 11月 16 10:19 itrace.txt
-rw-r--r-- 1 yuchao yuchao   447 11月 16 10:19 jit-interface.txt
-rw-r--r-- 1 yuchao yuchao  2306 11月 16 10:19 perf-annotate.txt
-rw-r--r-- 1 yuchao yuchao   475 11月 16 10:19 perf-archive.txt
-rw-r--r-- 1 yuchao yuchao  4249 11月 16 10:19 perf-bench.txt			//TODO
-rw-r--r-- 1 yuchao yuchao  2073 11月 16 10:19 perf-buildid-cache.txt
-rw-r--r-- 1 yuchao yuchao   831 11月 16 10:19 perf-buildid-list.txt
-rw-r--r-- 1 yuchao yuchao   807 11月 16 10:19 perf-data.txt
-rw-r--r-- 1 yuchao yuchao  6153 11月 16 10:19 perf-diff.txt
-rw-r--r-- 1 yuchao yuchao   589 11月 16 10:19 perf-evlist.txt
-rw-r--r-- 1 yuchao yuchao   928 11月 16 10:19 perf-help.txt
-rw-r--r-- 1 yuchao yuchao  1400 11月 16 10:19 perf-inject.txt
-rw-r--r-- 1 yuchao yuchao  1438 11月 16 10:19 perf-kmem.txt
-rw-r--r-- 1 yuchao yuchao  5711 11月 16 10:19 perf-kvm.txt
-rw-r--r-- 1 yuchao yuchao  5042 11月 16 10:19 perf-list.txt
-rw-r--r-- 1 yuchao yuchao  1198 11月 16 10:19 perf-lock.txt
-rw-r--r-- 1 yuchao yuchao  1452 11月 16 10:19 perf-mem.txt
-rw-r--r-- 1 yuchao yuchao  8467 11月 16 10:19 perf-probe.txt
-rw-r--r-- 1 yuchao yuchao 11794 11月 16 10:19 perf-record.txt
-rw-r--r-- 1 yuchao yuchao 12938 11月 16 10:19 perf-report.txt
-rw-r--r-- 1 yuchao yuchao  1567 11月 16 10:19 perf-sched.txt
-rw-r--r-- 1 yuchao yuchao  7370 11月 16 10:19 perf-script-perl.txt
-rw-r--r-- 1 yuchao yuchao 23248 11月 16 10:19 perf-script-python.txt
-rw-r--r-- 1 yuchao yuchao  9201 11月 16 10:19 perf-script.txt
-rw-r--r-- 1 yuchao yuchao  5965 11月 16 10:19 perf-stat.txt
-rw-r--r-- 1 yuchao yuchao   702 11月 16 10:19 perf-test.txt
-rw-r--r-- 1 yuchao yuchao  3496 11月 16 10:19 perf-timechart.txt
-rw-r--r-- 1 yuchao yuchao  7490 11月 16 10:19 perf-top.txt
-rw-r--r-- 1 yuchao yuchao  4817 11月 16 10:19 perf-trace.txt
-rw-r--r-- 1 yuchao yuchao  1152 11月 16 10:19 perf.txt
```

```
perf                perf-diff           perf-list           perf-script-perl
perf-annotate       perf_event_open     perf-lock           perf-script-python
perf-archive        perf-evlist         perf-mem            perf-stat
perf-bench          perf-ftrace         perfmonctl          perf-test
perf-buildid-cache  perf-help           perf-probe          perf-timechart
perf-buildid-list   perf-inject         perf-record         perf-top
perf-c2c[TODO]      perf-kallsyms       perf-report         perf-trace
perf-config         perf-kmem           perf-sched          perf-version
perf-data           perf-kvm            perf-script         
```


