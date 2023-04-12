整理几个具体的场景：

* [场景一](#场景一) 统计内核调用栈，火焰图输出









## 场景一

### 执行perf命令

```bash
# 记录采样信息，信息会存储到perf.data中
perf record -F 99 -a -g -- sleep 10
# 解析采样信息，将内容输出到out.perf
perf script > out.perf
```

参数说明：

| 参数        | 说明                        |
| ----------- | --------------------------- |
| -F 99       | 指定采样频率每秒 99 次      |
| -a          | 采样系统范围内所有CPU的信息 |
| -g          | 使能调用栈记录              |
| -- sleep 10 | 执行10秒                    |

### 火焰图

```bash
# 下载地址 https://github.com/brendangregg/FlameGraph.git
# 执行命令
./stackcollapse-perf.pl out.perf > out.folded
./flamegraph.pl out.folded > test.svg
```







## 附录

### 参考资料

* [Perf和火焰图](https://lgl88911.github.io/2020/03/19/Perf%E5%92%8C%E7%81%AB%E7%84%B0%E5%9B%BE/)

