# ubuntu22.04安装jetbrains-toolbox

## 下载jetbrains-toolbox

官网：https://www.jetbrains.com/toolbox-app/


## 安装

1. 解压安装包
```bash
tar -zxvf jetbrains-toolbox-1.27.2.13801.tar.gz
```
2. 启动解压文件夹里的AppImages文件
```bash
luz@luz-R720-15IKBN:~/.env/jetbrains-toolbox-1.27.2.13801$ ./jetbrains-toolbox 
dlopen(): error loading libfuse.so.2

AppImages require FUSE to run. 
You might still be able to extract the contents of this AppImage 
if you run it with the --appimage-extract option. 
See https://github.com/AppImage/AppImageKit/wiki/FUSE 
for more information
```
根据报错提示，缺少FUSE。

**安装fuse**：
```bash
sudo apt-get install fuse
```
再次执行AppImages文件，成功。