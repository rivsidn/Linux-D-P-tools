```
SADC(8)                            Linux User's Manual                           SADC(8)

NAME
       sadc - System activity data collector.
              系统活动数据收集器

SYNOPSIS
       /usr/lib/sysstat/sadc [ -C  comment ] [ -D ] [ -F ] [ -L ] [ -V ]
       [ -S { DISK | INT | IPV6 | POWER | SNMP | XDISK | ALL | XALL [,...] } ]
       [ interval [ count ] ] [ outfile ]

DESCRIPTION
       The  sadc  command  samples  system data a specified number of times (count) at a
       specified interval measured in seconds (interval). It writes in binary format  to
       the  specified  outfile  or to standard output. If outfile is set to -, then sadc
       uses the standard system activity daily data file (see below).  In this case,  if
       the  file  already exists, sadc will overwrite it if it is from a previous month.
       By default sadc collects most of the data available from the kernel.   But  there
       are  also  optional  metrics,  for  which the relevant options must be explicitly
       passed to sadc to be collected (see option -S below).
       但是这里也有一些可选项，之后明确的传递给sadc 才能开始搜集.

       The standard system activity daily data file is named saDD unless  option  -D  is
       used,  in  which  case  its name is saYYYYMMDD, where YYYY stands for the current
       year, MM for the current month and DD for the current  day.   By  default  it  is
       located  in  the  /var/log/sysstat  directory.  Yet  it is possible to specify an
       alternate location for it: If outfile is a directory (instead of  a  plain  file)
       then  it  will  be considered as the directory where the standard system activity
       daily data file will be saved.
       如果指定的oufile 是目录，该目录就会作为输出文件的默认路径.

       When the count parameter is not specified, sadc writes its data endlessly.   When
       both  interval  and  count  are not specified, and option -C is not used, a dummy
       record, which is used at system  startup  to  mark  the  time  when  the  counter
       restarts  from 0, will be written.  For example, one of the system startup script
       may write the restart mark to the daily data file by the command entry:

       /usr/lib/sysstat/sadc -

       The sadc command is intended to be used as a backend to the sar command.
       sadc命令是sar 命令的后端.

       Note: The sadc command only reports on local activities.

OPTIONS
       -C comment
              When neither the interval nor the count  parameters  are  specified,  this
              option tells sadc to write a dummy record containing the specified comment
              string.  This comment can then be displayed with option -C of sar.
	      运行sadc时，如果没指定时间间隔和次数，使用该选项可以让sadc 写一个包含comment
	      信息的假的日志到文件中。sar 显示的时候可以通过制定 -C 显示.

       -D     Use saYYYYMMDD instead of saDD as the standard system activity daily  data
              file name.
	      指定文件名格式.

       -F     The creation of outfile will be forced. If the file already exists and has
              a format unknown to sadc then it will be truncated. This may be useful for
              daily  data  files created by an older version of sadc and whose format is
              no longer compatible with current one.
	      强制创建文件，如果文件名已存在会被清空.

       -L     sadc will try to get an exclusive lock on the outfile before writing to it
              or  truncating it. Failure to get the lock is fatal, except in the case of
              trying to write a normal (i.e. not a dummy and not a header) record to  an
              existing  file,  in  which  case sadc will try again at the next interval.
              Usually, the only reason a lock  would  fail  would  be  if  another  sadc
              process  were  also writing to the file. This can happen when cron is used
              to launch sadc.  If the system is under heavy  load,  an  old  sadc  might
              still  be running when cron starts a new one. Without locking, this situa‐
              tion can result in a corrupted system activity file.
	      sadc写文件、截取文件之前会去获取互斥锁。
	      如果无法获取锁会导致异常，向一个已经存在的文件写正常日志记录的时候不会导致
	      异常，会在下一个时间间隔之后再次尝试。

       -S { DISK | INT | IPV6 | POWER | SNMP | XDISK | ALL | XALL [,...] }
              Specify which optional activities  should  be  collected  by  sadc.
	      Some activities are optional to prevent data files from growing too large.
	      The DISK keyword indicates that sadc should collect data  for  block  devices.
	      搜集块设备活动信息
              The INT keyword indicates that sadc should collect data for system interrupts.
	      搜集设备中断信息
	      The IPV6 keyword indicates that IPv6  statistics  should  be  collected  by  sadc.
	      搜集IPV6统计信息
	      The POWER  keyword indicates that sadc should collect power management statistics.
	      搜集店员管理信息
	      The SNMP keyword indicates that SNMP statistics should be collected by sadc. 
	      搜集SNMP统计信息
	      The ALL keyword is equivalent to specifying all the keywords above and therefore 
	      all previous  activities  are collected.

              The  XDISK keyword is an extension to the DISK one and indicates that par‐
              titions and filesystems statistics should be collected by sadc in addition
              to  disk statistics. This option works only with kernels 2.6.25 and later.
              The XALL keyword is  equivalent  to  specifying  all  the  keywords  above
              (including  keyword  extensions) and therefore all possible activities are
              collected.

              Important note: The activities  (including  optional  ones)  saved  in  an
              existing  data file prevail over those selected with option -S.  As a con‐
              sequence, appending data to an existing data file will result in option -S
              being ignored.
	      向一个已经存在的文件中写入数据的时候，会优先按照文件指定搜集的信息来继续信息搜集，
	      所以此处可能会导致-S 选项不生效.

       -V     Print version number then exit.
              输出版本信息并退出.

ENVIRONMENT
       The sadc command takes into account the following environment variable:
       影响sadc 的环境变量:

       S_TIME_DEF_TIME
              If  this variable exists and its value is UTC then sadc will save its data
              in UTC time.  sadc will also use UTC time instead of local time to  deter‐
              mine  the  current  daily data file located in the /var/log/sysstat direc‐
              tory.

EXAMPLES
       /usr/lib/sysstat/sadc 1 10 /tmp/datafile
              Write 10 records of one second intervals to the /tmp/datafile binary file.

       /usr/lib/sysstat/sadc -C Backup_Start /tmp/datafile
              Insert the comment Backup_Start into the file /tmp/datafile.

BUGS
       The /proc filesystem must be mounted for the sadc command to work.

       All the statistics are not necessarily available, depending on the kernel version
       used.  sadc assumes that you are using at least a 2.6 kernel.

FILES
       /var/log/sysstat/saDD
       /var/log/sysstat/saYYYYMMDD
              The  standard system activity daily data files and their default location.
              YYYY stands for the current year, MM for the current month and DD for  the
              current day.

       /proc and /sys contain various files with system statistics.

AUTHOR
       Sebastien Godard (sysstat <at> orange.fr)

SEE ALSO
       sar(1), sa1(8), sa2(8), sadf(1), sysstat(5)

       http://pagesperso-orange.fr/sebastien.godard/

Linux                                 DECEMBER 2016                              SADC(8)
```

