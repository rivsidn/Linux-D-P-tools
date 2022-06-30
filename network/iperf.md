## 测试示例

### UDP测试

```bash
# server
iperf -u -s -i 3 -B 172.31.3.140
# client
iperf -u -b 1000m -i 3 -t 1800 -c 172.31.3.140
```

| 设备   | 选项               | 解释                 |
| ------ | ------------------ | -------------------- |
| 服务器 | -u                 | 使用UDP协议，默认TCP |
|        | -s                 | 运行在服务器模式     |
|        | -i 3               | 输出信息时间间隔     |
|        | -B xxx.xxx.xxx.xxx | 绑定地址             |
| 客户端 | -u                 | 使用UDP协议，默认TCP |
|        | -b 1000m           | 指定发送速率         |
|        | -i 3               | 输出信息时间间隔     |
|        | -t 1800            | 持续时间             |
|        | -c xxx.xxx.xxx.xxx | 指定服务器地址       |



### TCP测试



## 附录

```
IPERF(1)                              User Manuals                              IPERF(1)

NAME
       iperf - perform network throughput tests
               网络吞吐测试

SYNOPSIS
       iperf -s [ options ]

       iperf -c server [ options ]

       iperf -u -s [ options ]

       iperf -u -c server [ options ]

DESCRIPTION
       iperf  is  a  tool  for  performing network throughput measurements.  It can test
       either TCP or UDP throughput.  To perform an iperf test the user  must  establish
       both a server (to discard traffic) and a client (to generate traffic).
       iperf 是一个用于网络吞吐测试的工具，可以用于测试TCP或者UDP吞吐，必须要用两台设备(客户端、服务器).

GENERAL OPTIONS
       -e, --enhanced
              Display enhanced output in reports
	      报告中显示输出增强版

       -f, --format
              [kmgKMG]    format  to report: Kbits, Mbits, KBytes, MBytes (see NOTES for more)
	      报告格式

       -h, --help
              print a help synopsis
	      输出帮助概要输出

       -i, --interval n
              pause n seconds between periodic bandwidth reports
	      输出报告时间间隔

       -l, --len n[kmKM]
              set read/write buffer size (TCP) or length (UDP) to n (TCP  default  128K,
              UDP default 1470)

       -m, --print_mss
              print TCP maximum segment size (MTU - TCP/IP header)
	      输出TCP的分段大小

       -o, --output filename
              output the report or error message to this specified file
	      输出报告、错误信息到特定文件

       -p, --port n
              set server port to listen on/connect to to n (default 5001)
	      设置服务器监听、连接端口

       -u, --udp
              use UDP rather than TCP
	      使用UDP协议

           --udp-counters-64bit
              use 64 bit UDP sequence numbers

       -w, --window n[kmKM]
              TCP window size (socket buffer size)

       -z, --realtime
              Request realtime scheduler, if supported.

       -B, --bind host
              bind to host, an interface or multicast address
	      绑定主机，接口或者是多播地址

       -C, --compatibility
              for use with older versions does not sent extra msgs
	      兼容老版本

       -M, --mss n
              set TCP maximum segment size (MTU - 40 bytes)
	      设置TCP最大分段大小

       -N, --nodelay
              set TCP no delay, disabling Nagle's Algorithm

       -S, --tos
              set the socket's IP_TOS (byte) field

       -v, --version
              print version information and quit

       -x, --reportexclude [CDMSV]
              exclude C(connection) D(data) M(multicast) S(settings) V(server) reports

       -y, --reportstyle C|c
              if set to C or c report results as CSV (comma separated values)

SERVER SPECIFIC OPTIONS
       -s, --server
              run in server mode
	      运行在服务器模式

       -D, --daemon
              run  the server as a daemon.  On Windows this will also install the IPerf‐Service.

       -R, --remove
              remove the IPerfService (Windows only).

       -U, --single_udp
              run in single threaded UDP mode

       -V, --ipv6_domain
              Enable IPv6 reception by setting the domain and socket  to  AF_INET6  (Can
              receive on both IPv4 and IPv6)

CLIENT SPECIFIC OPTIONS
       -b, --bandwidth n[kmgKMG] | npps
              set  target  bandwidth to n bits/sec (default 1 Mbit/sec) or n packets per
              sec.  This may be used with TCP or UDP.

       -c, --client host
              run in client mode, connecting to host

       -d, --dualtest
              Do a bidirectional test simultaneously

       -n, --num n[kmKM]
              number of bytes to transmit (instead of -t)

       -r, --tradeoff
              Do a bidirectional test individually

       -t, --time n
              time in seconds to listen for new traffic connections, receive traffic  or
              transmit  traffic  (Defaults: transmit is 10 secs while listen and receive
              are indefinite)

       -B, --bind ip | ip:port | ipv6 -V | [ipv6]:port -V
              bind src addr(s) and ports from  which  to  originate  traffic  (Note:  -V
              option required for ipv6)

       -F, --fileinput name
              input the data to be transmitted from a file

       -I, --stdin
              input the data to be transmitted from stdin

       -L, --listenport n
              port to receive bidirectional tests back on

       -P, --parallel n
              number of parallel client threads to run

       -R, --reverse
              reverse the traffic flow after header exchange, useful for testing through
              firewalls

       -T, --ttl n
              time-to-live, for multicast (default 1)

       -V, --ipv6_domain
              Set the domain to IPv6 (send packets over IPv6)

       -X, --peerdetect
              run server version detection prior to traffic.

       -Z, --linux-congestion algo
              set TCP congestion control algorithm (Linux only)

ENVIRONMENT
       TCP_WINDOW_SIZE
              Controls the size of TCP buffers.

NOTES
       Some numeric options support format characters per '<value>c'  (e.g.  10M)  where
       the  c  format  characters are k,m,g,K,M,G.  Lowercase format characters are 10^3
       based and uppercase are 2^n based, e.g. 1k = 1000, 1K = 1024, 1m = 1,000,000  and
       1M = 1,048,576

DIAGNOSTICS
       This section needs to be filled in.

BUGS
       See https://sourceforge.net/p/iperf2/tickets/

AUTHORS
       Iperf2,  based from iperf (originally written by Mark Gates and Alex Warshavsky),
       has a goal of maintenance with some  feature  enhancement.   Other  contributions
       from  Ajay Tirumala, Jim Ferguson, Jon Dugan <jdugan at x1024 dot net>, Feng Qin,
       Kevin Gibbs, John Estabrook <jestabro at ncsa.uiuc.edu>,  Andrew  Gallatin  <gal‐
       latin  at gmail.com>, Stephen Hemminger <shemminger at linux-foundation.org>, Tim
       Auckland, Robert J. McMahon <rjmcmahon at rjmcmahon.com>

SEE ALSO
       http://sourceforge.net/projects/iperf2/

NLANR/DAST                             APRIL 2008                               IPERF(1)
```
