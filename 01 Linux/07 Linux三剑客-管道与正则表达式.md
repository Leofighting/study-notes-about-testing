### 管道

Linux 提供管道符 `|` 将两个命令隔开；

管道符左边命令的输出就会作为管道符右边命令的输入

![image-20201212201730669](C:\Users\xiaoj\AppData\Roaming\Typora\typora-user-images\image-20201212201730669.png)

```bash
[root@xiaojw ~]# echo "hello 123" | grep "hello"
hello 123
```



### 正则表达式

> 记录文本规则的代码
>
> [正则表达式语法](https://www.runoob.com/regexp/regexp-syntax.html)

#### 常用元字符

![image-20201212202504273](C:\Users\xiaoj\AppData\Roaming\Typora\typora-user-images\image-20201212202504273.png)

#### 常用限定符

![image-20201212202614628](C:\Users\xiaoj\AppData\Roaming\Typora\typora-user-images\image-20201212202614628.png)

