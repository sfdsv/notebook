# win11环境下使用pip安装第三方包

问题：

在本地python环境下使用pip安装opentrons包后，引入opentrons时，除了init.py以外，其余包找不到引用。

解决办法：

使用conda创建了一个新的虚拟环境，再配上换源，在虚拟环境下安装opentrons包，项目引入虚拟环境下的解释器即可。

```
(ot2) C:\Users\ot2-1>pip --version
pip 22.3.1 from C:\Users\ot2-1\Desktop\workspace\env\anaconda3\envs\ot2\lib\site-packages\pip (python 3.8)
(ot2) C:\Users\ot2-1>pip --version
pip 22.3.1 from C:\Users\ot2-1\Desktop\workspace\env\anaconda3\envs\ot2\lib\site-packages\pip (python 3.8)

(ot2) C:\Users\ot2-1>pip install -i https://pypi.tuna.tsinghua.edu.cn/simple opentrons
Looking in indexes: https://pypi.tuna.tsinghua.edu.cn/simple
Collecting opentrons
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/d3/8d/3256f255b4b8face17a9a32d38cf62fe7e8bbd2d58e1d166e8b0af6f1f14/opentrons-6.2.1-py2.py3-none-any.whl (1.2 MB)
     ---------------------------------------- 1.2/1.2 MB 1.3 MB/s eta 0:00:00
Collecting pyserial==3.5
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/07/bc/587a445451b253b285629263eb51c2d8e9bcea4fc97826266d186f96f558/pyserial-3.5-py2.py3-none-any.whl (90 kB)
     ---------------------------------------- 90.6/90.6 kB 271.4 kB/s eta 0:00:00
Collecting typing-extensions<5,>=4.0.0
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/31/25/5abcd82372d3d4a3932e1fa8c3dbf9efac10cc7c0d16e78467460571b404/typing_extensions-4.5.0-py3-none-any.whl (27 kB)
Collecting anyio==3.3.0
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/94/72/c728ad9e34f53be06f4dfbff4d511907bafb46920f10dc34836e58bd6894/anyio-3.3.0-py3-none-any.whl (77 kB)
     ---------------------------------------- 77.9/77.9 kB 4.2 MB/s eta 0:00:00
Collecting jsonschema==3.0.2
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/54/48/f5f11003ceddcd4ad292d4d9b5677588e9169eef41f88e38b2888e7ec6c4/jsonschema-3.0.2-py2.py3-none-any.whl (54 kB)
     ---------------------------------------- 54.7/54.7 kB 1.4 MB/s eta 0:00:00
Collecting pydantic==1.8.2
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/51/68/6579cb896863715b6a5c63e4983b1c0ab7693685a7c2ded469ef37eb3539/pydantic-1.8.2-cp38-cp38-win_amd64.whl (2.0 MB)
     ---------------------------------------- 2.0/2.0 MB 577.0 kB/s eta 0:00:00
Collecting click<9,>=8.0.0
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/c2/f1/df59e28c642d583f7dacffb1e0965d0e00b218e0186d7858ac5233dce840/click-8.1.3-py3-none-any.whl (96 kB)
     ---------------------------------------- 96.6/96.6 kB 918.6 kB/s eta 0:00:00
Collecting opentrons-shared-data==6.2.1
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/33/9f/ba095b9b97e1e0257fab8da034495dc58c87db8e34f6c6823a26366b0338/opentrons_shared_data-6.2.1-py2.py3-none-any.whl (326 kB)
     ---------------------------------------- 326.7/326.7 kB 1.3 MB/s eta 0:00:00
Collecting aionotify==0.2.0
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/d4/ca/104c3332c6c9be78108840fb3b08f40cfd2c598df4ce1cd7eb999416825c/aionotify-0.2.0-py3-none-any.whl (6.6 kB)
Collecting numpy<2,>=1.15.1
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/bf/8c/3d36cef521739bd481e9a5b30e5c0f9faf8b7fe7b904238368908a9d149d/numpy-1.24.2-cp38-cp38-win_amd64.whl (14.9 MB)
     ---------------------------------------- 14.9/14.9 MB 407.1 kB/s eta 0:00:00
Collecting idna>=2.8
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/fc/34/3030de6f1370931b9dbb4dad48f6ab1015ab1d32447850b9fc94e60097be/idna-3.4-py3-none-any.whl (61 kB)
     ---------------------------------------- 61.5/61.5 kB 657.7 kB/s eta 0:00:00
Collecting sniffio>=1.1
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/c3/a0/5dba8ed157b0136607c7f2151db695885606968d1fae123dc3391e0cfdbf/sniffio-1.3.0-py3-none-any.whl (10 kB)
Collecting six>=1.11.0
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/d9/5a/e7c31adbe875f2abbb91bd84cf2dc52d792b5a01506781dbcf25c91daf11/six-1.16.0-py2.py3-none-any.whl (11 kB)
Collecting attrs>=17.4.0
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/fb/6e/6f83bf616d2becdf333a1640f1d463fef3150e2e926b7010cb0f81c95e88/attrs-22.2.0-py3-none-any.whl (60 kB)
     ---------------------------------------- 60.0/60.0 kB 138.7 kB/s eta 0:00:00
Requirement already satisfied: setuptools in c:\users\ot2-1\desktop\workspace\env\anaconda3\envs\ot2\lib\site-packages (from jsonschema==3.0.2->opentrons) (65.6.3)
Collecting pyrsistent>=0.14.0
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/b1/8d/bbce2d857ecdefb7170a8a37ade1de0f060052236c07693856ac23f3b1ee/pyrsistent-0.19.3-cp38-cp38-win_amd64.whl (62 kB)
     ---------------------------------------- 62.7/62.7 kB 557.9 kB/s eta 0:00:00
Collecting colorama
  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/d1/d6/3965ed04c63042e047cb6a3e6ed1a63a35087b6a609aa3a15ed8ac56c221/colorama-0.4.6-py2.py3-none-any.whl (25 kB)
Installing collected packages: pyserial, aionotify, typing-extensions, sniffio, six, pyrsistent, numpy, idna, colorama, attrs, pydantic, jsonschema, click, anyio, opentrons-shared-data, opentrons
Successfully installed aionotify-0.2.0 anyio-3.3.0 attrs-22.2.0 click-8.1.3 colorama-0.4.6 idna-3.4 jsonschema-3.0.2 numpy-1.24.2 opentrons-6.2.1 opentrons-shared-data-6.2.1 pydantic-1.8.2 pyrsistent-0.19.3 pyserial-3.5 six-1.16.0 sniffio-1.3.0 typing-extensions-4.5.0
```

**换源安装**

[参考链接](https://cloud.tencent.com/developer/article/1744525)

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
