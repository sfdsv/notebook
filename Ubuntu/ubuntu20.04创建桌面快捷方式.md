快捷方式(*.desktop)文件夹：

```bash
/usr/share/applications
```

1. 新的快捷方式

创建.desktop文件：

```text
[Desktop Entry]
Version=1.0
Type=Application
Name=xxx
Icon=/home/luz/Desktop/xxx.png
Exec=/home/luz/Desktop/xxx.sh,
Comment= software
Categories=Education;
Terminal=false
StartupWMClass=xxx
StartupNotify=true
```

再给.desktop文件加执行权限：

```bash
chmod +x $HOME/Desktop/xxx.desktop
```

设置允许启动属性：

```bash
gio set $HOME/Desktop/xxx.desktop metadata::trusted true
```

2. 程序已有开始菜单快捷方式
    ![2022-11-21 13-56-17屏幕截图](https://cdn.jsdelivr.net/gh/sfdsv/pic/img/2022-11-21 13-56-17屏幕截图.png)
