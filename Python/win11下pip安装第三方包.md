问题：

在本地python环境下使用pip安装opentrons包后，引入opentrons时，除了init.py以外，其余包找不到引用。

解决办法：

使用conda创建了一个新的虚拟环境，再配上换源，在虚拟环境下安装opentrons包，项目引入虚拟环境下的解释器即可。

[参考链接](https://cloud.tencent.com/developer/article/1744525)

换源安装

```
清华：https://pypi.tuna.tsinghua.edu.cn/simple

阿里云：http://mirrors.aliyun.com/pypi/simple/

中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/

华中理工大学：http://pypi.hustunique.com/

山东理工大学：http://pypi.sdutlinux.org/ 

豆瓣：http://pypi.douban.com/simple/
```

临时使用
```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple some-package
```
