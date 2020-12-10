### 网络相关命令

`ifconfig`：查看网卡信息

```bash
[root@localhost chmod_test]# ifconfig
enp0s3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.0.112  netmask 255.255.255.0  broadcast 192.168.0.255
        inet6 fe80::6f25:133:e614:f7c4  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:b5:11:ba  txqueuelen 1000  (Ethernet)
        RX packets 1710  bytes 128876 (125.8 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 549  bytes 59638 (58.2 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 4  bytes 344 (344.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 4  bytes 344 (344.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```



`ping`：测试远程主机联通性

常用方式：

- `ping -c 次数 IP地址`：连接多少次
- `ping -i 间隔时间 IP地址`：每次 `ping` 的间隔时间

```bash
[root@localhost chmod_test]# ping -c 3 -i 3 www.baidu.com
PING www.baidu.com (14.215.177.39) 56(84) bytes of data.
64 bytes from www.baidu.com (14.215.177.39): icmp_seq=1 ttl=128 time=12.9 ms
64 bytes from www.baidu.com (14.215.177.39): icmp_seq=2 ttl=128 time=21.3 ms
64 bytes from www.baidu.com (14.215.177.39): icmp_seq=3 ttl=128 time=16.4 ms

--- www.baidu.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 6021ms
rtt min/avg/max/mdev = 12.904/16.909/21.382/3.480 ms
```



`netstat`：打印网络连接状况、所开放的端口、路由表等信息

常用方式：

- `netstat -npl`：打印当前系统启动哪些端口



 