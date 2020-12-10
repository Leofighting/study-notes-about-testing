### 帮助文档

`man`：用于查看命令的帮助文档

> 格式：`man 需要查询的命令`
>
> 例如：`man ls`

相关快捷键使用：

> 退出：q
>
> 下一页：空格键
>
> 上下移动：上下方向键



`--help`：也可以用于查看帮助文档

> 格式：`需要查询的命令 --help`



### 文件管理命令

`ls`：用于列出指定目录或者文件

常用方式：

- `ls -l` = `ll`
- `ls -a`：显示所有文件，包含隐藏文件



`cd`：用于切换用户所在的目录

常用方式：

- `cd`：如果后面什么都不跟，直接进入当前用户的根目录下
- `cd 路径`：可以是绝对路径，或者相对路径
- `cd ..`：返回上一级目录

其他：

> 善于利用路径的自动补全功能，按 Tab 键



`pwd`：显示当前目录的绝对路径



`mkdir`：创建新目录

> 格式：`mkdir [-mp] [目录名称]`

常用方式：

- `mkdir 目录名称`
- `mkdir -p 目录名称`：能够递归创建文件夹



`touch`：创建空文件

常用方式：

- `touch 文件名`



`rm`：删除文件或者目录，**谨慎使用**

常用方式：

- `rm -r 文件或文件夹目录`：删除目录前会咨询

  ```bash
  [root@localhost test1]# rm -r rm_test
  rm：是否删除目录 "rm_test"？y
  ```

- `rm -f 文件或文件夹目录`：辨识强制删除，不会咨询，而是直接删除

- `rm -rf 文件或文件夹目录`

**注意**：`rm -rf` 后面不能直接加 `/`，否则会导致整个系统文件被全部删除，非常危险



`cp`：复制

> 格式：`cp [选项] [来源文件（要复制的文件）] [目的文件（复制后的文件名）]`

常用方式：

- `cp -r 来源文件 目的文件`：用于复制目录

  ```bash
  [root@localhost test1]# mkdir 123
  [root@localhost test1]# cp 123 456
  cp: 略过目录"123"
  [root@localhost test1]# cp -r 123 456
  [root@localhost test1]# ll
  总用量 0
  drwxr-xr-x. 2 root root 6 12月  6 22:16 123
  drwxr-xr-x. 2 root root 6 12月  6 22:17 456
  ```



`mv`：移动或者重命名

> 格式：`mv [选项] [源文件或目录] [目标文件或目录]`

如果移动到当前目录，则重命名



`ln`：建立链接文件

> 格式：`ln [-s] [来源文件] [目的文件]`

关于软链接与硬链接：

- 创建软链接：`ln -s [来源文件] [目的文件]`，相当于快捷方式，如果源文件被删除，则软链接失效
- 创建硬链接：`ln [来源文件] [目的文件]`，即使源文件被删除，硬链接依然有效，可用

## 看上海悠悠的视频，关于 /bin 目录下软链接的应用



`find`：搜索文件

> 格式：`find [路径] [参数] 文件名`

常用方式：

- `find 路径 -name 文件名（支持通配符* 与 ?）`

  ```bash
  [root@localhost test1]# ll
  总用量 0
  drwxr-xr-x. 2 root root 6 12月  6 22:16 123
  drwxr-xr-x. 2 root root 6 12月  6 22:17 456
  -rw-r--r--. 1 root root 0 12月  6 22:07 rm_test1
  [root@localhost test1]# find . -name 123
  ./123
  [root@localhost test1]# find . -name '1*'
  ./123
  [root@localhost test1]# find . -name '*1*'
  ./rm_test1
  ./123
  ```

- `find 路径 -type 文件类型`

  ```bash
  [root@localhost test1]# ll
  总用量 0
  drwxr-xr-x. 2 root root 6 12月  6 22:16 123
  drwxr-xr-x. 2 root root 6 12月  6 22:17 456
  -rw-r--r--. 1 root root 0 12月  6 22:07 rm_test1
  [root@localhost test1]# find . -type d
  .
  ./123
  ./456
  ```

  

`cat`：用于查看一个文件的内容并将其显示在屏幕上

> 格式：`cat [参数] 文件名`

常用方式：

- `cat 文件名`
- `cat -n 文件名`：查看文件时，把行号页显示在屏幕上
- `cat -A 文件名`：显示所有内容，包括特殊字符



`more`：查看文件时，一页一页地查阅

> 格式：`more 文件名`

快捷键：

- `Ctrl+D`：向上翻页
- `Ctrl+F 或者 空格`：向下翻页
- 回车键：下一行
- `q`：退出
- `/搜索内容`：向下搜索
- `?搜索内容`：向上搜索



`less`：与 `more` 命令类似



`head`：后面直接跟文件名，默认显示文件的前10行

> 格式：`head -n 行数 文件名`
>
> 注：-n后有无空格均可，字母n也可以省略
>
> `head -行数 文件名`

```bash
[root@localhost test1]# head -3 /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
```



`tail`：与 `head` 类似，后面直接跟文件名，用于显示文件最后10行

> 格式：`tail -n 行数 文件名`

常用方式：

- `tail -f 行数 文件名`：动态显示文件的最后 n 行，默认是10行；查看日志时非常有用



`tar`：可以对文件目录进行打包压缩或者解压缩

> 格式：`tar [参数] 压缩后文件名 需要压缩的文件/目录`

常用方式：

- `tar -zcvf 压缩后文件名 需要压缩的文件/目录`：打包同时进行压缩

  ```bash
  [root@localhost test1]# ll
  总用量 0
  drwxr-xr-x. 2 root root 6 12月  6 22:16 123
  drwxr-xr-x. 2 root root 6 12月  6 22:17 456
  -rw-r--r--. 1 root root 0 12月  6 22:07 rm_test1
  [root@localhost test1]# tar -zcvf file.tar.gz 123 456 rm_test1
  123/
  456/
  rm_test1
  [root@localhost test1]# ll
  总用量 4
  drwxr-xr-x. 2 root root   6 12月  6 22:16 123
  drwxr-xr-x. 2 root root   6 12月  6 22:17 456
  -rw-r--r--. 1 root root 153 12月  7 11:56 file.tar.gz
  -rw-r--r--. 1 root root   0 12月  6 22:07 rm_test1
  ```

- `tar -zxvf 需要解压缩的文件`：解压缩

  ```bash
  [root@localhost tar_test]# ll
  总用量 4
  -rw-r--r--. 1 root root 153 12月  7 11:56 file.tar.gz
  [root@localhost tar_test]# tar -zxvf file.tar.gz
  123/
  456/
  rm_test1
  [root@localhost tar_test]# ll
  总用量 4
  drwxr-xr-x. 2 root root   6 12月  6 22:16 123
  drwxr-xr-x. 2 root root   6 12月  6 22:17 456
  -rw-r--r--. 1 root root 153 12月  7 11:56 file.tar.gz
  -rw-r--r--. 1 root root   0 12月  6 22:07 rm_test1
  ```

- `tar -zxvf 需要解压缩的文件 -C 指定路径`：解压缩到指定路径

  ```bash
  [root@localhost tar_test]# ll
  总用量 4
  -rw-r--r--. 1 root root 153 12月  7 11:56 file.tar.gz
  drwxr-xr-x. 2 root root   6 12月  7 12:02 tar_dir
  [root@localhost tar_test]# tar -zxvf file.tar.gz -C tar_dir/
  123/
  456/
  rm_test1
  [root@localhost tar_test]# cd tar_dir/
  [root@localhost tar_dir]# ll
  总用量 0
  drwxr-xr-x. 2 root root 6 12月  6 22:16 123
  drwxr-xr-x. 2 root root 6 12月  6 22:17 456
  -rw-r--r--. 1 root root 0 12月  6 22:07 rm_test1
  ```



`chmod`：修改文件权限

> 格式：`chmod [-R] 权限数字xxx 文件`：`-R`：表示递归修改整个目录

```bash
[root@localhost chmod_test]# ll
总用量 0
-rw-r--r--. 1 root root 0 12月  7 12:26 test
[root@localhost chmod_test]# chmod 777 test
[root@localhost chmod_test]# ll
总用量 0
-rwxrwxrwx. 1 root root 0 12月  7 12:26 test
```