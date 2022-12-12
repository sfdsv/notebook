# ubuntu20.04安装chrome浏览器

## 环境
操作系统：ubuntu20.04

chrome浏览器：Google Chrome 108.0.5359.98

## 安装
官网下载：https://www.google.cn/chrome/index.html

选择64位Deb：

![image](https://user-images.githubusercontent.com/73980771/206979425-fc924cc1-74f3-42bf-9f86-a057ab2d5068.png)

进入安装包路径，打开终端：
```bash
sudo dpkg -i google-chrome-stable_current_amd64.deb
```

就可以用了。

查看chrome版本：
```bash
google-chrome --version
```
