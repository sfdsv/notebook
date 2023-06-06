# 安装canFestival环境

## 1.安装python2

```bash
sudo apt install python2
```

## 2.安装wxpython

### 2.1添加ubuntu20.04源

**安装完成后，可以取消勾选。**

![image-20230601115851881](/home/hcsci/.config/Typora/typora-user-images/image-20230601115851881.png)

### 2.2更新缓存

```bash
sudo apt update
```

但不要`upgrade`!

### 2.3安装python-wxgtk3.0

```bash
sudo apt install python-wxgtk3.0
```

## 3.安装gnosis

解压文件`Gnosis_Utils-current.tar.gz`.

```bash
cd ./Gnosis_Utils-current/Gnosis_Utils-1.2.2
sudo python2 setup.py install
```



**最后，设置clion中python解释器为python2，就可以用了。**

