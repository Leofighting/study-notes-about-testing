### `curl`

- `-G`：使用 get 请求（默认为 get 请求）

  `curl -G https://www.baidu.com`

  `curl -X GET https://www.baidu.com`

- `-d`：使用 post 请求

  `curl -d 'login=1234' https://www.baidu.com`

  `curl -X POST 'login=1234' https://www.baidu.com`

- `-o`：保存响应内容

- `-v`：输出通信的整个过程

- `-s`：不输出错误和进程信息

```bash
curl -x 代理端口 网址
```



### `jq`

用于格式优化，转化为 json 格式

```bash
[root@xiaojw ~]# echo '{"a":10,"b":20}'
{"a":10,"b":20}
[root@xiaojw ~]# echo '{"a":10,"b":20}'|jq .
{
  "a": 10,
  "b": 20
}
```

