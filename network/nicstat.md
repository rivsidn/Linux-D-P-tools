```

nicstat(1)                       General Commands Manual                      nicstat(1)

NAME
       nicstat, enicstat - print network traffic statistics
       输出网络流量统计

SYNOPSIS
       nicstat [-hvnsxpztualkMU] [-iinterface] [-Sint:mbps[fd|hd]] [interval [count]]

       enicstat <same options & operands>

DESCRIPTION
       nicstat  prints  out  network  statistics for all network cards (NICs), including
       packets, kilobytes per second, average packet sizes and more.
       输出所有网卡的统计信息，包括包文、每秒数据、平均包文大小等.

OPTIONS
       -h        Display brief usage information (help).
                 输出简单的帮助信息

       -v        Display nicstat version (and additional fields when combined with '-l')
                 显示版本信息

       -n        Show statistics for non-local (i.e. non-loopback) interfaces only.
                 显示非回环口的统计信息

       -s        Display summary output - just the amount of data  received  (read)  and
                 transmitted (written).
		 显示汇总信息 - 仅仅显示获取和发送的数据总量

       -x        Display extended output.  See OUTPUT section for details.
                 显示拓展信息

       -U        Display  separate  read  and write utilization statistics. This affects
                 the default, extended (-x) and all (-a) format outputs. For the default
                 format the "Sat" statistic is dropped to fit the output in 80 columns.
		 读、写使用率分开显示。在默认显示格式时，会将"Sat" 列丢弃。

       -M        Display  interface throughput statistics in Mbps (megabits per second),
                 instead of the default KB/s (kilobytes per second).
		 以Mbps 格式显示接口吞吐，默认是KB/s 格式。

                 NOTE - interface statistics are reported to operating systems in bytes.
                 nicstat  does  not know if Ethernet or other hardware overheads are in‐
                 cluded in the statistic on each platform.

       -p        Display output in parseable format.  This outputs one line  per  inter‐
                 face, in the following formats (which correspond to the default, -x, -t
                 and -u options; respectively):

              time:In:rKB/s:wKB/s:rPk/s:wPk/s:%Util:Sat
              time:In:rKB/s:wKB/s:rPk/s:wPk/s:%Util:Sat:IErr:OErr:Coll:NoCP:Defer
              time:TCP:InKB:OutKB:InSeg:OutSeg:Reset:AttF:%ReTX:InConn:OutCon:Drops
              time:UDP:InDG:OutDG:InErr:OutErr

                 where time is the number of seconds since midnight, Jan  1  1970  (UST)
                 and the other fields are as described in the OUTPUT section below.

                 NOTE  -  throughput statistics are always in KB/s (kilbytes per second)
                 for parseable formats, even if the "-M" flag has been specified.
		 即使设置了"-M"选项，此时还是会以KB/s 模式显示。

       -z        Skip interfaces for which there was zero traffic for the sample period.
       		 跳过采样期间没有流量的接口

       -t        Show TCP statistics.
         	 显示TCP统计信息.

       -u        Show UDP statistics.
       		 显示UDP统计信息.

       -a        Equvalent to '-x -t -u'.
       		 等同于'-x -t -u'

       -l        Just list interfaces.
       		 仅仅显示接口

       -iinterface[,interface...]
                 Show statistics for only the interface(s) listed.  Multiple  interfaces
                 can be listed, separated by commas (,).
		 仅仅统计列出的接口，可以写多个接口

       -Sint:speed[fd|hd]
                 (Linux only).  Specify the speed (and optionally duplex mode) of one or
                 more interfaces.  The given speed(s) are in megabits/second.   The  du‐
                 plex  mode will default to "full" unless a suffix beginning with "h" or
                 "H" is specified.  Speed and duplex mode are obtained automatically  on
                 Solaris using the "ifspeed" and "link_duplex" kstat values.

       -k        (Solaris  only).   Search  for active network interfaces by looking for
                 kstat "link_state" statistics with a value of 1.  This is only of value
                 on  systems  running  Solaris  10  (or early releases of Solaris 11 Ex‐
                 press), with Exclusive IP Zones, where the interfaces given to  an  Ex‐
                 clusive  IP Zone are not otherwise visible.  If you are running Solaris
                 9 (or earlier), or Solaris 11 (or later) you do not need this option.

OPERANDS
       interval  Specifies the number of seconds between samples.
		 指定采样时间周期

       count     Specifies the number of times that the statistics are repeated.  If  no
                 count is specified, nicstat will repeat statistics indefinitely.
		 制定采样的次数

OUTPUT
       The fields of nicstat's display are:
       nicstat显示的信息:

       Time      The time corresponding to the end of the sample shown, in HH:MM:SS for‐
                 mat (24-hour clock).

       Int       The interface name.
       		 接口名

       rKB/s, InKB
                 Kilobytes/second read (received).
		 接收速率

       wKB/s, OutKB
                 Kilobytes/second written (transmitted).
		 发送速率

       rMbps, RdMbps
                 Megabits/second read (received).

       wMbps, WrMbps
                 Megabits/second written (transmitted).

       rPk/s, InSeg, InDG
                 Packets (TCP Segments, UDP Datagrams)/second read (received).

       wPk/s, OutSeg, OutDG
                 Packets (TCP Segments, UDP Datagrams)/second written (transmitted).

       rAvs      Average size of packets read (received).
       		 平均接收报文大小

       wAvs      Average size of packets written (transmitted).
       		 平均发送报文大小

       %Util     Percentage utilization of the interface.  For  full-duplex  interfaces,
                 this  is the greater of rKB/s or wKB/s as a percentage of the interface
                 speed.  For half-duplex interfaces, rKB/s and wKB/s are summed.

       %rUtil, %wUtil
                 Percentage utilization for bytes read and written, respectively.

       Sat       Saturation.  This the number of errors/second seen for the interface  -
                 an indicator the interface may be approaching saturation.  This statis‐
                 tic is combined from a number of kernel statistics.  It is  recommended
                 to  use  the  '-x' option to see more individual statistics (those men‐
                 tioned below) when attempting to diagnose a network issue.

       IErr      Packets received that could not be processed because they contained er‐
                 rors

       OErr      Packets that were not successfully transmitted because of errors

       Coll      Ethernet collisions during transmit.
       		 以太冲突

       NoCP      No-can-puts.   This  is  when  an incoming packet can not be put to the
                 process reading the socket.  This suggests the local process is  unable
                 to process incoming packets in a timely manner.
		 报文无法放到进程socket中，表示报文没有被及时处理.

       Defer     Defer  Transmits.   Packets without collisions where first transmit at‐
                 tempt was delayed because the medium was busy.

       Reset     tcpEstabResets. The number of times TCP connections have made a  direct
                 transition to the CLOSED state from either the ESTABLISHED state or the
                 CLOSE-WAIT state.

       AttF      tcpAttemptFails - The number of times that TCP connections have made  a
                 direct transition to the CLOSED state from either the SYN-SENT state or
                 the SYN-RCVD state, plus the number of times TCP connections have  made
                 a direct transition to the LISTEN state from the SYN-RCVD state.

       %ReTX     Percentage  of  TCP segments retransmitted - that is, the number of TCP
                 segments transmitted containing  one  or  more  previously  transmitted
                 octets.

       InConn    tcpPassiveOpens  - The number of times that TCP connections have made a
                 direct transition to the SYN-RCVD state from the LISTEN state.

       OutCon    tcpActiveOpens - The number of times that TCP connections have  made  a
                 direct transition to the SYN-SENT state from the CLOSED state.

       Drops     tcpHalfOpenDrop + tcpListenDrop + tcpListenDropQ0.

       tcpListenDrop  and  tcpListenDropQ0 - Number of connections dropped from the com‐
       pleted connection queue and incomplete connection queue,  respectively.   tcpHal‐
       fOpenDrops  -  Number of connections dropped after the initial SYN packet was re‐
       ceived.

       The first set of statistics printed are averages since system boot.  If no inter‐
       val  operand is specified, or a count value of "1" is specified, this will be the
       only sample printed.

EXAMPLES
       Print average statistics from boot time to now only:

            $ nicstat

       Print statistics for all interfaces, every 3 seconds:

            $ nicstat 3

       Print statistics for all interfaces, every 5 seconds, finishing after 10 samples:

            $ nicstat 5 10

       Print statistics every 3 seconds, only for interfaces "hme0" and "hme1":

            $ nicstat -i hme0,hme1 3

       Print statistics for non-local interfaces, setting speed of "eth0" and "eth1"  to
       10mbps/half-duplex and 1000mbps/full-duplex, respectively:
       可以用于设置端口速率:

            $ nicstat -n -S eth0:10h,eth1:1000 5

SEE ALSO
       netstat(1M) kstat(1M), kstat(3KSTAT), mibiisa(1M), ethtool(8)

       "nicstat  -  the  Solaris  and Linux Network Monitoring Tool You Did Not Know You
       Needed" -http://blogs.oracle.com/timc/entry/nicstat_the_solaris_and_linux

NOTES
       On Linux, the NoCP, Defer, TCP InKB, and TCP OutKB statistics are always reported
       as zero.

       The way that saturation is reported is a best effort, as there is no standardized
       naming to capture all errors related to an interface's inability  to  receive  or
       transmit  a packet.  Monitoring %Util and packet rates, along with an understand‐
       ing of the specific NICs may be more useful in judging whether  you  are  nearing
       saturation.

       The  -S  option  is provided for the Linux edition as nicstat requires super-user
       privilege to obtain speed and duplex mode information for interfaces.  If you are
       unable  to  set  up nicstat as setuid-root, a script named enicstat is available,
       which uses the ethtool utility then calls nicstat with an -S value.  ethtool  it‐
       self requires super-user privilege for this to work.

4th Berkeley Distribution              27 Jan 2014                            nicstat(1)
```
