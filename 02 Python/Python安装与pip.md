### Python安装

安装包下载：[Python官网](https://www.python.org/)

[[Python和PyCharm环境安装配置](https://ceshiren.com/t/topic/57)]



### pip 依赖管理

Python3 的 3.4 版本开始，pip 被直接包括在 Python 的安装包内

[pypi-pip包管理网站](https://pypi.org/project/pip/)



Python 项目中必须包含一个 requirements.txt 文件，用于记录所有依赖包及其精确的版本号，以便新环境部署：

```python
pip freeze > requirements.txt
```



#### 常用国内 pip 源

1. 阿里云：http://mirrors.aliyun.com/pypi/simple/
2. 豆瓣：http://pypi.douban.com/simple/
3. 清华大学：https://pypi.tuna.tsinghua.edu.cn/simple/
4. 中国科学技术大学：http://pypi.mirrors.ustc.edu.cn/simple/
5. 华中科技大学：http://pypi.hustunique.com/