# 使用rufus制作ubuntu的U盘启动盘

参考链接：https://www.how2shout.com/linux/how-to-create-ubuntu-22-04-bootable-usb-drive-on-windows/

### 1.下载ubuntu镜像

[官网地址](https://ubuntu.com/download/desktop)

### 2.下载rufus

[官网地址](http://rufus.ie/en/#google_vignette)

### 3.运行rufus并选择ubuntu镜像

1. 双击rufus软件，设备选择准备的U盘，引导类型选择ubuntu镜像，点击**开始**。

![image-20221216194905430](https://cdn.jsdelivr.net/gh/sfdsv/pic/img/202212161949473.png)

2. 点击开始后，会弹出写入镜像所使用模式，选择推荐的**以 ISO 镜像模式写入**，点击**OK**。

![image-20221216195550695](https://cdn.jsdelivr.net/gh/sfdsv/pic/img/202212161955727.png)

3. 弹出警告，U盘上的所有数据会被清除，点击**确定**。

![image-20221216195745262](https://cdn.jsdelivr.net/gh/sfdsv/pic/img/202212161957342.png)

4. 等待完成。状态栏显示**准备就绪**。点击关闭。

<img width="297" alt="image" src="https://user-images.githubusercontent.com/73980771/208101326-a6d1b403-2970-48e7-9754-2c69a4e2e6af.png">

最后，再弹出U盘，U盘就可以用啦。

### 踩坑

1. 第2步动作点击后，弹出有另一个程序占用u盘。

采用弹出u盘的操作，警告仍然存在，且制作u盘失败。

接着在调出任务管理器，点击性能，点击资源监视器，在关联的句柄处搜索U盘的盘符`E:/`，未发现有关联的程序。然后直接拔出U盘重新插入。

2. 错误的U盘弹出动作。

执行拔U盘再弹出U盘的操作后，制作开始后弹出错误提示：

```
[0xC0030001] Incorrect function.
```

受到了不知名启发，重新拔出U盘插入U盘，但是不弹出了，再制作，就一切ok了。
