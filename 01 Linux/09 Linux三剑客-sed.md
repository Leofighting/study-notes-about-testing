### 定义

`sed` 是流编辑器，一次处理一行内容

对文本的处理流程：（**模式空间是重点**）

![image-20201212210719196](C:\Users\xiaoj\AppData\Roaming\Typora\typora-user-images\image-20201212210719196.png)

> 格式：`sed [-hn][-e<script>][-f<script file>][file]`
>
> - `-h`：显示帮助文档
> - `-n`：仅显示 `script` 处理后的结果
> - `-e<script>`：已选项中指定的 script 来处理输入的文本文件
> - `-f<script文件>`：已选项中指定的 script 文件来处理输入的文本文件

常用动作（**以下动作不会改变源文件内容**）

- `a`：新增；`sed -e '4 a newline'`：在第4行后新增一行
- `c`：取代；`sed -e '2-5 c No 2-5 number'`
- `d`：删除；`sed -e '2,5 d'`：删除第 2，5 行
- `i`：插入；`sed -e '2 i newline'`：在第2行前插入一行
- `p`：打印；`sed -n '/root/p'`：只打印包含 root 的行；`/***/`中的内容表示正则匹配语法脚本
- `s`：替换；`sed -e 's#old#new#g'`：将 old 替换为 new，`g` 代表全局替换；# 可以换成 `/，@`等

实战应用

- 在第4行后面添加新字符串

  ```bash
  $ cat test
  root root hello root
  root
  root
  leo
  kate
  hogwart
  string
  leon
  $ sed -e '4 a newline' test
  root root hello root
  root
  root
  leo
  newline  # 新插入的行
  kate
  hogwart
  string
  leon
  ```

- 在第2行前添加新字符串

  ```bash
  $ sed -e '2 i newline' test
  root root hello root
  newline  # 在第2行前添加
  root
  root
  leo
  kate
  hogwart
  string
  leon
  ```

- 全局替换

  ```bash
  $ sed -e 's/root/hello/g' test
  hello hello hello hello
  hello
  hello
  leo
  kate
  hogwart
  string
  leon
  ```

- 直接修改源文件；**注意：要提前对源文件进行备份**

  ```bash
  $ sed -i 's/root/user/' test
  $ cat test
  user root hello root
  user
  user
  leo
  kate
  hogwart
  string
  leon
  ```

  

### 课程实战应用

- 结合 `w` 命令查看在线人数

  ```bash
  w | sed '1, 2d' | awk '{print $1}' | sort | uniq -c | wc -l
  ```

- 