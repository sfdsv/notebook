# ubuntu20.04右上角设置不见了

## **问题** 

右上角设置不见了, 桌面右键的设置还在但是打不开. 

**重启无效.**

### **原因** 

/usr/share/中的 gnome-control-center 包不在了或损坏了.

### **解决方法** 

重新安装gnome-control-center包 :
```bash
sudo apt install gnome-control-center
```

> 慎用 -y 选项, sudo apt upgrade -y 的 -y 的作用是忽略确认环节.
