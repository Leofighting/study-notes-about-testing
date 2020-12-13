### 变量

命名规则：

- 只能使用英文字母、数字与下划线，首个字符不能以数字开头
- 中间不能有空格，允许使用下划线
- 不能使用标点符号
- 不能使用 bash 里的关键字（可用help命令查看保留关键字）

定义与使用变量：

```bash
# 定义常规变量
[root@xiaojw ~]# name="leo"
[root@xiaojw ~]# echo $name
leo
# 定义只读变量：不能被更改和删除
[root@xiaojw ~]# name=tom
[root@xiaojw ~]# readonly name
[root@xiaojw ~]# unset name
-bash: unset: name: cannot unset: readonly variable
# 删除变量
[root@xiaojw ~]# age=18
[root@xiaojw ~]# echo $age
18
[root@xiaojw ~]# unset age
[root@xiaojw ~]# echo $age

```

变量类型

- 字符串：`name="leo"`
- 拼接字符串：`greeting="hello,"$name"!"`
- 数组：`array=(v1, v2, v3)`
  - 取数组中某个值：`value=${array}[n]`
  - 单独赋值：`array[n]=value`



### 条件分支

#### `if`

```bash
if condition
then
	command1
	command2
	……
fi
```



### 循环

#### `for`

```bash
for var in 取值范围;
do
	command1
	command2
	……
done
```

#### `while`

```bash
while condition;
do
	command
done
```



### bash 的基本使用

#### `read`

- 用于从终端或者文件中读取输入的内部命令

- 读取整行输入

- 每行末尾的换行符不被读入

使用：

- 从标准输入读取输入并赋值给变量

  `read var`

- 从标准输入读取多个内容

  `read var1 var2 var3`

- 不指定变量（默认赋值给 REPLAY）

  `read`

#### 脚本参数传递

- `$0`：脚本名称
- `$1-$n`：获取参数
- `$#`：传递到脚本的参数个数
- `$$`：脚本运行的当前进程 id 号
- `$*`：以一个单字符串显示所有向脚本传递的参数
- `$?`：显示最后命令的退出状态；0 表示没有错误，其他任何值表明有错误

### 算术运算

```bash
[root@xiaojw ~]# a=10
[root@xiaojw ~]# b=20
# 加法
[root@xiaojw ~]# expr $a + $b
30
# 减法
[root@xiaojw ~]# expr $a - $b
-10
#乘法
[root@xiaojw ~]# expr $a \* $b
200
# 除法；结果只保留整数部分
[root@xiaojw ~]# expr $a / $b
0
# 取余
[root@xiaojw ~]# expr $a % $b
10
# 复制
a=$b
# 相等
[ $a == $b ]
# 不等于
[ $a != $b ]
# -eq：检测相等
[ $a -eq $b ]
# -ne：检测不相等
[ $a -ne $b ]
# -gt：检测左边是否大于右边的值
[ $a -gt $b ]
# -lt：检测是否小于
[ $a -lt $b ]
# -ge：检测是否大于等于
[ $a -ge $b ]
# -le：检测是否小于等于
[ $a -le $b ]
```



### 学习资料

[阮一峰-Bash 脚本教程](https://wangdoc.com/bash/)

