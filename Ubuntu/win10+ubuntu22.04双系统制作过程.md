#  win10+ubuntu-22.04双系统制作过程

参考链接：https://huaweicloud.csdn.net/6356034cd3efff3090b58a9b.html

**已存在win10系统**。

## Windows设置

### 1.分区

分出一个空的区域给ubuntu系统做存储。

在桌面上，点击此电脑(右键) -> 管理 -> 磁盘 -> 磁盘管理。

右键点击你想要分区的硬盘，选择压缩卷，填入压缩空间量。

点击压缩。

### 2.启动盘制作

[制作启动盘](https://github.com/sfdsv/notebook/blob/main/Ubuntu/%E5%88%B6%E4%BD%9CU%E7%9B%98%E5%90%AF%E5%8A%A8%E7%9B%98.md)

## Ubuntu安装

### 1.进入bios设置U盘启动

拯救者r720启动热键是F2。

自己查自己型号的按键。参考[这篇文章](https://www.jb51.net/os/82023.html?login=from_csdn)。

进入Bios界面后，切换到Boot；选择USB Boot，将其置为Enabled；将USB启动置于第一位。按F10保存退出。

### 2.U盘启动

U盘启动后，进入安装引导界面。

1. 选择**安装ubuntu**，继续
2. 默认英语，
3. 默认不联网，继续

​	然后出错了。

```
提示：
弹出界面说我需要"关闭RST" 。

解决办法：
    1. 点击重启。
    2. 拔出U盘，按下Enter，重启进入Windows。
    3. 启动后，我又插入U盘，重启？？？
    4. 重启过程中，一直按F2，直到进入Bios界面，切换到Configuration，选择SATA Controller Mode，按Enter弹出选项，切换到AHCI，按Enter。
     !!!注意一定要再切到Boot界面，将启动方式改为USB启动，不然就会陷入错误修复中。
  	 最后，按F10保存退出。
```

再次启动后，按照步骤1-3进行，正常了进入下一个界面。

4. 默认正常安装，继续

5. 选择安装类型。这里我们自定义安装，选**其他选项**，如果不想折腾也可以选择默认的第一个选项（安装Ubuntu，与Windows Boot Manager共存）

6. 进入重点：分区。**前面预留的硬盘空间要用上了。**点击空余空间，点+号新建分区。

   * / 根目录整个系统的大区域一般15G以上。（50G）
   * /boot 启动目录，开机启动所需目录。一般200M-2G。（2G）
   * swap 交换空间，一般和内存一样大。（8G）
   * /home 主目录，就是我们自己存放用户数据的目录。一般有多少给多少。（剩余所有空间）

   根据挂载点不同：

    * 类型都选择逻辑分区
    * 新分区的位置默认空间起始位置
    * 除了swap的用途是交换空间以外，其余分区都用做Ext4日志文件系统

   之后，点击现在安装。

7. 选择时区，Shanghai。

8. 设置用户。

9. 等待安装。

10. 安装完成以后，点击现在重启。

11. 然后拔出U盘，按下Enter。



## 解决问题

1. 如何切换回Win10

   参考[这篇文章](https://blog.csdn.net/u012011079/article/details/119885503)。

# 其他

[Intel RST不支持Ubuntu解决方案_安装Linux至移动硬盘_手把手教程](https://blog.csdn.net/JCMLSY/article/details/122315806)

