# 安装python

## 在linux系统中安装python

几乎所有的linux系统都默认安装了python。

### 确定已安装的pthon版本

```bash
luz@luz-Vostro-3470:~$ python --version

找不到命令“python”，您的意思是：

  command 'python3' from deb python3
  command 'python' from deb python-is-python3
```

```bash
luz@luz-Vostro-3470:~$ python3 --version
Python 3.8.10
```

### 安装python3

如果你想安装其它的python3版本，只需执行几个命令即可。

```bash
sudo add-apt-repository ppa:fkrull/deadsnaks
sudo apt-get update
sudo apt-get intall python3.5
```

启动python3.5的终端会话：

```python
$ python3.5
>>>
```



