### 定义

根据用户指定的模式（pattern），对目标文件进行过滤，显示被漠视匹配到的行

> 格式：`grep [参数] 匹配内容 [文件]`

常用参数：

- `-v`：显示未匹配到的行
- `-i`：忽略大小写
- `-n`：显示匹配的行号
- `-c`：统计匹配的行数
- `-o`：仅显示匹配到的字符串
- `-E`：使用 ERE，相当于 `egrep`

实战应用：

- 显示含有 root 的行，并显示行号

  ```bash
  $ cat test
  root root hello root
  new
  new
  root
  root
  leo
  kate
  hogwart
  string
  leon
  $ grep -n root test
  1:root root hello root
  4:root
  5:root
  ```

- 显示不包含 root 的行，并显示行号

  ```bash
  $ grep -vn root test
  2:new
  3:new
  6:leo
  7:kate
  8:hogwart
  9:string
  10:leon
  ```

- 查找以 s 开头的行；查找以 n 结尾的行

  ```bash
  $ grep ^s -n test
  9:string
  
  $ grep n$ -n test
  10:leon
  ```

  

