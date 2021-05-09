---
title: 【数据库测试工具】认识Sysbench
type: categories
copyright: false
date: 2020-04-12 08:54:04
tags: 数据库实践
categories: 数据库技术
title_en: db_sysbench
---


> 本文基于课堂PPT笔记整理，主要介绍一下Sysbench及其简单使用，实验代码部分在代码中有重点注释，不另作说明。 

#### 一，基准测试

什么是数据库的基准测试？

> 数据库的基准测试是对数据库的性能指标进行定量的、可复现的、可对比的测试。

数据库的基准测试有何作用？

> 对数据库的基准测试的作用，就是分析在当前的配置下（包括硬件配置、OS、数据库设置等），数据库的性能表现，从而找出MySQL的性能阈值，并根据实际系统的要求调整配置。

基准测试可以理解为针对系统的一种压力测试。但基准测试不关心业务逻辑，更加简单、直接、易于测试，数据可以由工具生成，不要求真实；而压力测试一般考虑业务逻辑(如购物车业务)，要求真实的数据。

数据库的基准测试有哪些测试指标？

> 1）TPS/QPS：衡量吞吐量；
>
> 2）响应时间：包括平均响应时间、最小响应时间、最大响应时间、时间百分比等，其中时间百分比参考意义较大，如前95%的请求的最大响应时间；
>
> 3）并发量：同时处理的查询请求的数量。

#### 二，Sysbench简介

Sysbench是跨平台的基准测试工具，支持多线程，支持多种数据库；主要包括以下几种测试：

> cpu性能
>
> 磁盘io性能
>
> 调度程序性能
>
> 内存分配及传输速度
>
> POSIX线程性能
>
> 数据库性能(OLTP基准测试)

Sysbench基本命令一览表：

```bash
☁  ~  sysbench --help
Usage:
  sysbench [options]... [testname] [command]

Commands implemented by most tests: prepare run cleanup help

General options:
  --threads=N                     number of threads to use [1]
  --events=N                      limit for total number of events [0]
  --time=N                        limit for total execution time in seconds [10]
  --forced-shutdown=STRING        number of seconds to wait after the --time limit before forcing shutdown, or 'off' to disable [off]
  --thread-stack-size=SIZE        size of stack per thread [64K]
  --rate=N                        average transactions rate. 0 for unlimited rate [0]
  --report-interval=N             periodically report intermediate statistics with a specified interval in seconds. 0 disables intermediate reports [0]
  --report-checkpoints=[LIST,...] dump full statistics and reset all counters at specified points in time. The argument is a list of comma-separated values representing the amount of time in seconds elapsed from start of test when report checkpoint(s) must be performed. Report checkpoints are off by default. []
  --debug[=on|off]                print more debugging info [off]
  --validate[=on|off]             perform validation checks where possible [off]
  --help[=on|off]                 print help and exit [off]
  --version[=on|off]              print version and exit [off]
  --config-file=FILENAME          File containing command line options
  --tx-rate=N                     deprecated alias for --rate [0]
  --max-requests=N                deprecated alias for --events [0]
  --max-time=N                    deprecated alias for --time [0]
  --num-threads=N                 deprecated alias for --threads [1]

Pseudo-Random Numbers Generator options:
  --rand-type=STRING random numbers distribution {uniform,gaussian,special,pareto} [special]
  --rand-spec-iter=N number of iterations used for numbers generation [12]
  --rand-spec-pct=N  percentage of values to be treated as 'special' (for special distribution) [1]
  --rand-spec-res=N  percentage of 'special' values to use (for special distribution) [75]
  --rand-seed=N      seed for random number generator. When 0, the current time is used as a RNG seed. [0]
  --rand-pareto-h=N  parameter h for pareto distribution [0.2]

Log options:
  --verbosity=N verbosity level {5 - debug, 0 - only critical messages} [3]

  --percentile=N       percentile to calculate in latency statistics (1-100). Use the special value of 0 to disable percentile calculations [95]
  --histogram[=on|off] print latency histogram in report [off]

General database options:

  --db-driver=STRING  specifies database driver to use ('help' to get list of available drivers) [mysql]
  --db-ps-mode=STRING prepared statements usage mode {auto, disable} [auto]
  --db-debug[=on|off] print database-specific debug information [off]


Compiled-in database drivers:
  mysql - MySQL driver

mysql options:
  --mysql-host=[LIST,...]          MySQL server host [localhost]
  --mysql-port=[LIST,...]          MySQL server port [3306]
  --mysql-socket=[LIST,...]        MySQL socket
  --mysql-user=STRING              MySQL user [sbtest]
  --mysql-password=STRING          MySQL password []
  --mysql-db=STRING                MySQL database name [sbtest]
  --mysql-ssl[=on|off]             use SSL connections, if available in the client library [off]
  --mysql-ssl-cipher=STRING        use specific cipher for SSL connections []
  --mysql-compression[=on|off]     use compression, if available in the client library [off]
  --mysql-debug[=on|off]           trace all client library calls [off]
  --mysql-ignore-errors=[LIST,...] list of errors to ignore, or "all" [1213,1020,1205]
  --mysql-dry-run[=on|off]         Dry run, pretend that all MySQL client API calls are successful without executing them [off]

Compiled-in tests:
  fileio - File I/O test
  cpu - CPU performance test
  memory - Memory functions speed test
  threads - Threads subsystem performance test
  mutex - Mutex performance test

See 'sysbench <testname> help' for a list of options for each test.
```


#### 三，Mac安装使用Sysbench

```bash
☁  ~  brew install sysbench
# 添加路径
☁  ~  export LDFLAGS="-L/usr/local/opt/mysql@5.7/lib"
☁  ~  export CPPFLAGS="-I/usr/local/opt/mysql@5.7/include"
# 查看版本
☁  ~  sysbench --version
# 测试自带lua脚本
☁  ~  sysbench --test=/usr/local/Cellar/sysbench/1.0.19/share/sysbench/tests/include/oltp_legacy/oltp.lua
WARNING: the --test option is deprecated. You can pass a script name or path on the command line without any options.
sysbench 1.0.19 (using bundled LuaJIT 2.1.0-beta2)
☁  ~  sysbench /usr/local/Cellar/sysbench/1.0.19/share/sysbench/tests/include/oltp_legacy/oltp.lua
sysbench 1.0.19 (using bundled LuaJIT 2.1.0-beta2)
# 测试cpu
☁  ~  sysbench cpu --cpu-max-prime=20000 --threads=3 run
# 测试file，run是执行正式的测试，prepare是为测试提前准备数据，cleanup是在测试完成后对数据库进行清理
☁  ~  sysbench fileio --threads=16 --file-total-size=1G --file-test-mode=rndrw prepare
☁  ~  sysbench fileio --threads=16 --file-total-size=1G --file-test-mode=rndrw run
☁  ~  sysbench fileio --threads=16 --file-total-size=1G --file-test-mode=rndrw cleanup
# 测试threads
☁  ~  sysbench threads --threads=64 --thread-yields=100 --thread-locks=2 run
# 测试memory
☁  ~  sysbench memory --memory-block-size=8k --memory-total-size=40G run
# OLTP测试
☁  ~  sysbench /usr/local/Cellar/sysbench/1.0.19/share/sysbench/tests/include/oltp_legacy/oltp.lua --mysql-table-engine=innodb --oltp-tables-count=3 --oltp-table-size=100000 --mysql-user=root --mysql-password=xxxxxxx --mysql-socket=/tmp/mysql.sock prepare
☁  ~  sysbench /usr/local/Cellar/sysbench/1.0.19/share/sysbench/tests/include/oltp_legacy/oltp.lua --mysql-table-engine=innodb --oltp-tables-count=3 --oltp-table-size=100000 --mysql-user=root --mysql-password=xxxxxx --mysql-socket=/tmp/mysql.sock --max-time=60 --max-requests=0 --num-threads=8 --report-interval=10 run
WARNING: --num-threads is deprecated, use --threads instead
WARNING: --max-time is deprecated, use --time instead
sysbench 1.0.19 (using bundled LuaJIT 2.1.0-beta2)

Running the test with following options:
Number of threads: 8
Report intermediate results every 10 second(s)
Initializing random number generator from current time


Initializing worker threads...

Threads started!

[ 10s ] thds: 8 tps: 658.76 qps: 13182.94 (r/w/o: 9229.30/2635.43/1318.21) lat (ms,95%): 26.68 err/s: 0.00 reconn/s: 0.00
[ 20s ] thds: 8 tps: 717.41 qps: 14350.05 (r/w/o: 10045.00/2870.13/1434.91) lat (ms,95%): 22.69 err/s: 0.00 reconn/s: 0.00
[ 30s ] thds: 8 tps: 745.71 qps: 14914.47 (r/w/o: 10439.89/2983.15/1491.43) lat (ms,95%): 21.11 err/s: 0.00 reconn/s: 0.00
[ 40s ] thds: 8 tps: 754.39 qps: 15085.46 (r/w/o: 10560.23/3016.45/1508.78) lat (ms,95%): 21.11 err/s: 0.00 reconn/s: 0.00
[ 50s ] thds: 8 tps: 687.47 qps: 13749.32 (r/w/o: 9624.39/2749.98/1374.94) lat (ms,95%): 24.83 err/s: 0.00 reconn/s: 0.00
[ 60s ] thds: 8 tps: 620.78 qps: 12417.29 (r/w/o: 8692.61/2483.12/1241.56) lat (ms,95%): 27.17 err/s: 0.00 reconn/s: 0.00
SQL statistics:
    queries performed:
        read:                            585984
        write:                           167424
        other:                           83712
        total:                           837120
    transactions:                        41856  (697.33 per sec.)
    queries:                             837120 (13946.58 per sec.)
    ignored errors:                      0      (0.00 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          60.0215s
    total number of events:              41856

Latency (ms):
         min:                                    3.54
         avg:                                   11.47
         max:                                  138.32
         95th percentile:                       23.95
         sum:                               479897.13

Threads fairness:
    events (avg/stddev):           5232.0000/18.17
    execution time (avg/stddev):   59.9871/0.01
☁  ~  sysbench /usr/local/Cellar/sysbench/1.0.19/share/sysbench/tests/include/oltp_legacy/oltp.lua --mysql-table-engine=innodb --oltp-tables-count=3 --oltp-table-size=100000 --mysql-user=root --mysql-password=xxxxxxx --mysql-socket=/tmp/mysql.sock --report-interval=10 cleanup
```

#### 四，安装并使用TPCC-MYSQL

Tpcc-mysql默认自带的数据库比较大，约10G，请实验前保证磁盘空间充足。

```bash
# 下载源码
☁  tools git clone https://github.com/Percona-Lab/tpcc-mysql.git
# 配置mysql_config
☁  ~  sudo ln -s /usr/local/opt/mysql@5.7/bin/mysql_config /usr/local/bin/mysql_config
# Bulid binaries
☁  tools  cd tpcc-mysql/src/
☁  src [master] ⚡  make
# Load data
☁  ~  mysqladmin -uroot -p create tpcc1000
☁  ~  mysql -uroot -p  tpcc1000 < /home/tools/tpcc-mysql/create_table.sql
☁  ~  mysql -uroot -p  tpcc1000 < /home/tools/tpcc-mysql/add_fkey_idx.sql
☁  tpcc-mysql [master] ⚡  ./tpcc_load -h127.0.0.1 -d tpcc1000 -uroot -pxxxxxx -w 100
# Start benchmark
☁  tpcc-mysql [master] ⚡  ./tpcc_start -h 127.0.0.1 -p 3306 -d tpcc1000 -uroot -p "xxxxxxx" -w 100 -c 10 -r 100 -l 300 -i 20 -f /var/log/tpcc_mysql.log -t /var/log/tpcc_mysql.rtx
....
<Constraint Check> (all must be [OK])
 [transaction percentage]
        Payment: 43.48% (>=43.0%) [OK]
   Order-Status: 4.35% (>= 4.0%) [OK]
       Delivery: 4.35% (>= 4.0%) [OK]
    Stock-Level: 4.35% (>= 4.0%) [OK]
 [response time (at least 90% passed)]
      New-Order: 0.00%  [NG] *
        Payment: 30.49%  [NG] *
   Order-Status: 34.91%  [NG] *
       Delivery: 12.76%  [NG] *
    Stock-Level: 0.00%  [NG] *

<TpmC>
                 4061.400 TpmC
```

#### 五，遇到的问题及其解决方案

问题描述：ld: library not found for -lrt 

> 解决方案：在`src/Makefile`中去掉 `lrt`，产生的错误按照提示去解决即可。

```bash
LIBS=       `mysql_config --libs_r` # -lrt
```


