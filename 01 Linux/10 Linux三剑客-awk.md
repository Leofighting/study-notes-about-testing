### 定义

把文件逐行读入，以空格为默认分割符，将每行切片，切开的部分再进行后续处理

> 处理流程：

![image-20201212215936527](C:\Users\xiaoj\AppData\Roaming\Typora\typora-user-images\image-20201212215936527.png)

> 格式：`awk [参数] 'pattern action' [文件]`
>
> - `pattern`：正则表达式
> - `action`：对匹配到的内容执行的命令（默认为输入每行内容）

### 常用参数：

- `FILENAME`：`awk` 浏览的文件名
- `BEGIN`：处理文本前要执行的操作
- `END`：处理文本后要执行的动作
- `FS`：设置输入域分割符，等价于命令行 `-F` 参数
- `NF`：浏览记录的域的个数（列数）
- `NR`：已读的记录数（行数）
- `OFS`：输出域分割符
- `ORS`：输入记录分割符
- `RS`：控制记录分割符
- `$0`：整条记录
- `$n`：表示当前行的第n个域

### 实战应用

- 搜索 `/etc/passwd` 文件中，包含 root 关键字的所有行，并打印对应的 shell

  ```bash
  $ cat /etc/passwd | head -2
  root:x:0:0:root:/root:/bin/bash
  bin:x:1:1:bin:/bin:/sbin/nologin
  $ awk -F : '/root/{print $0}' /etc/passwd  # $0：整行打印
  root:x:0:0:root:/root:/bin/bash
  operator:x:11:0:operator:/root:/sbin/nologin
  $ awk -F : '/root/{print $7}' /etc/passwd
  /bin/bash
  /sbin/nologin
  ```

- 打印 `/etc/passwd` 文件第2行的信息

  ```bash
  $ awk -F : 'NR==2{print $0}' /etc/passwd
  bin:x:1:1:bin:/bin:/sbin/nologin
  ```

- 使用 `BEGIN` 加入标题

  ```bash
  $ awk -F : 'BEGIN {print "开始"} {print $1 $2}' /etc/passwd | head -5
  开始
  rootx
  binx
  daemonx
  admx
  ```

- 自定义行分割符

  ```bash
  $ echo "111 222|333 444|555 666" | awk 'BEGIN{RS="|"}{print $0}'
  111 222
  333 444
  555 666
  ```




### 课程实战应用

- 找出log中的404，500的报错：

  ```bash
  $ cat nginx.log | head -3
  223.104.7.59 - - [05/Dec/2018:00:00:01 +0000] "GET /topics/17112 HTTP/2.0" 200 9874 "https://www.googleapis.com/auth/chrome-content-suggestions" "Mozilla/5.0 (iPhone; CPU iPhone OS 12_1 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) CriOS/70.0.3538.75 Mobile/15E148 Safari/605.1" 0.040 0.040 .
  $ less nginx.log | awk '$9~/404|500/'
  # 统计个数
  $ less nginx.log | awk '$9~/404|500/'|awk '{print $9}'|sort|uniq -c
      266 404
        1 500
  ```

- 找出访问量最高的 `IP`

  ```bash
  $ less nginx.log|awk '{print $1}'|sort|uniq -c|sort -nr|head -3
      282 216.244.66.241
      130 136.243.151.90
      110 127.0.0.1
  ```

- **实用技巧**：查看切割后，各字段对应第几列

  ```bash
  $ less nginx.log | head -1 | awk '{for(i=1;i<=NF;i++) print "$"i" = "$i}'
  $1 = 223.104.7.59
  $2 = -
  $3 = -
  $4 = [05/Dec/2018:00:00:01
  $5 = +0000]
  $6 = "GET
  $7 = /topics/17112
  $8 = HTTP/2.0"
  $9 = 200
  $10 = 9874
  $11 = "https://www.googleapis.com/auth/chrome-content-suggestions"
  $12 = "Mozilla/5.0
  $13 = (iPhone;
  $14 = CPU
  $15 = iPhone
  $16 = OS
  $17 = 12_1
  $18 = like
  $19 = Mac
  $20 = OS
  $21 = X)
  $22 = AppleWebKit/605.1.15
  $23 = (KHTML,
  $24 = like
  $25 = Gecko)
  $26 = CriOS/70.0.3538.75
  $27 = Mobile/15E148
  $28 = Safari/605.1"
  $29 = 0.040
  $30 = 0.040
  $31 = .
  ```

- 取出最大的响应时间

  ```bash
  $ less nginx.log | awk '{print $(NF-1)}' | sort -nr | head -1
  86462.600
  ```

- 取出 `top3` 的响应时间，并打印出对应的接口

  ```bash
  $ less nginx.log | awk '{print $(NF-1), $7}' | sort -nr | head -3
  86462.600 /cable
  77331.425 /cable
  59394.978 /cable
  ```

- 取出所有请求的平均

  ```bash
  $ less nginx.log | awk '{sum+=$(NF-1)}END{print sum/NR}'
  1082.57
  ```

- 计算每个接口的平均响应时间，并取出 `top3`

  ```bash
  $ less nginx.log | awk '{time[$7]+=$(NF-1); count[$7]+=1}END{for(k in time) print time[k]/count[k], k}'| sort -nr | head -3
  3707.26 /cable
  5.592 /topics/9524
  2.19 /topics/8343?locale=en
  ```

- 计算每个 `URL` 的顶层路由地址所对应的 `QPS`（每秒的请求次数），并打印 `top5` 的顶级路由

  ```bash
  less nginx.log | 
    awk '{print $4,$7}' | 
    sed 's#[?!].*##' |
    sed -E 's#([^ ]*) *(/[^/]*).*#\1:\2#'  |
    sort | 
    awk -F: '
  {cur=($3*60+$4);}
  NR==1{min=cur; max=cur;}
  NR>1{
  count[$NF]+=1; 
  if(cur<min) min=cur; 
  if(cur>max) max=cur;
  }
  END{
  for(k in count) print k,count[k]/(max-min+1)}
  ' | 
    sort -k2 -nr | head -5
  ```

- 每隔1秒统计下 `aliyundun` 的2个进程的 `cpu` 与 内存，分类汇总下 10s 内的平均 `cpu` 与响应时间

  ```bash
  $ top -b -d 1 -n 10 | grep -i aliyundun | awk '{cpu[$NF]+=$(NF-3);mem[$NF]+=$(NF-2);count[$NF]+=1}END{for(k in cpu) print k, cpu[k]/count[k], mem[k]/count[k]}'
  AliYunDunUpdate 0 0.1
  AliYunDun 0.8 1.2
  ```

- 统计当前服务器上每个监听端口对应的网络状态的数量

  ```bash
  $ netstat -tn | awk '{print $4, $NF}' | awk -F: '{print $NF}' | sort | uniq -c
       15 22 ESTABLISHED
        3 25 TIME_WAIT
        1 51368 TIME_WAIT
        1 51374 TIME_WAIT
        1 51380 TIME_WAIT
        1 51386 TIME_WAIT
        1 57968 ESTABLISHED
        4 9101 TIME_WAIT
        1 9102 ESTABLISHED
        1 Local State
        1 (w/o servers)
  ```

  

