# ubuntu22.04 桌面图标消失，右键菜单显示不全

如果ubuntu的桌面上所有文件突然消失，但是通过终端命令ls查看桌面仍存在文件，同时在桌面右键后的菜单也显示不全，则可以通过执行下面命令来恢复：

```bash
sudo apt-get install -f
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install ubuntu-desktop
reboot
```
