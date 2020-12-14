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

  