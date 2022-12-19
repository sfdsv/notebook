# ubuntu20.04软件安装和配置

## 用户目录下的中文目录修改为英文目录
如果是中文安装，那么用户目录下一些常用文件夹会是中文名称，那么在终端中进行操作的时候会非常不方便，所以我会把中文目录名调整成英文目录名。
```
1.先修改语言
export LANG=en_US

2.然后执行
xdg-user-dirs-gtk-update

弹出提示,仔细看看,点击同意

3.reboot重启
弹出提示框,点击取消,即可
```

## 安装网络工具包（ifconfig）
```bash
sudo apt install net-tools
```

## 截图软件flameshot
[ubuntu20.04截图工具flameshot](https://github.com/sfdsv/notebook/blob/main/Ubuntu/ubuntu20.04%E6%88%AA%E5%9B%BE%E5%B7%A5%E5%85%B7flameshot.md)

## git
```bash
sudo apt install git
```

## golang
[Ubuntu20.04 安装和升级go](https://github.com/sfdsv/notebook/blob/main/Ubuntu/ubuntu%E5%AE%89%E8%A3%85%E5%92%8C%E5%8D%87%E7%BA%A7go.md)

## wps
[ubuntu20.04安装wps2019](https://github.com/sfdsv/notebook/blob/main/Ubuntu/ubuntu20.04%E5%AE%89%E8%A3%85wps2019.md)

## chrome浏览器
[ubuntu20.04安装chrome浏览器](https://github.com/sfdsv/notebook/blob/main/Ubuntu/ubuntu20.04%E5%AE%89%E8%A3%85chrome%E6%B5%8F%E8%A7%88%E5%99%A8.md)

## edge浏览器
[ubuntu20.04安装edge浏览器](https://github.com/sfdsv/notebook/blob/main/Ubuntu/ubuntu20.04%E5%AE%89%E8%A3%85edge%E6%B5%8F%E8%A7%88%E5%99%A8.md)

## gcc
[ubuntu安装最新版gcc](https://github.com/sfdsv/notebook/blob/main/Ubuntu/ubuntu%E5%AE%89%E8%A3%85gcc.md)
