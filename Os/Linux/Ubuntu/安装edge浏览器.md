# ubuntu20.04安装edge浏览器

edge浏览器有三个不同的渠道，stable、beta和dev渠道。

安装方式有两种，图形化方式和命令行方式。

## 图形化方式

* stable渠道
在任意浏览器中访问 [Microsoft Edge 官方主页](https://www.microsoft.com/zh-cn/edge)，点击「下载 Linux 版」按钮。

<img width="553" alt="image" src="https://user-images.githubusercontent.com/73980771/208440592-0f8ddb21-4a10-4ba6-8d20-55c9ad5cf86f.png">

  1. 直接双击软件，点击安装完成。
  2. 或者运行下列命令安装：
  ```bash
  sudo dpkg -i "microsoft-edge-stable_108.0.1462.54-1_amd64.deb"
  ```

* beta和dev渠道同理 [官方主页](https://www.microsoftedgeinsider.com/zh-cn/download/?platform=linux)

## 命令行方式

<img width="842" alt="image" src="https://user-images.githubusercontent.com/73980771/208445223-13bce3ab-6134-46fc-8080-131896e29fd5.png">

### 系统更新
在正式安装之前，您应该运行系统更新以确保现有软件包都是最新的，以避免在安装过程中出现任何冲突。
```bash
sudo apt update && sudo apt upgrade -y
```

### 安装所需的软件包
Ubuntu 系统要成功安装 Microsoft Edge 浏览器，您需要先安装以下软件包：
```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common wget -y
```

### 导入 Microsoft Edge GPG 密钥和存储库
* 需要下载 Microsoft Edge GPG 以验证包的真实性
```bash
sudo wget -O- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor | sudo tee /usr/share/keyrings/microsoft-edge.gpg
```
* 导入 Microsoft Edge 官方源
```bash
echo 'deb [arch=amd64 signed-by=/usr/share/keyrings/microsoft-edge.gpg] https://packages.microsoft.com/repos/edge stable main' | sudo tee /etc/apt/sources.list.d/microsoft-edge.list
```

>注意：请按自己所使用 Linux 架构将arch=amd64更改为arch=arm64或arch=armhf以匹配你的环境。 如果您不确定，可以使用标准的 amd64。

* 官方源导入成功后使用以下命令刷新存储库列表：
```bash
sudo apt update
```

* 执行以下命令安装 Microsoft Edge 浏览器:
```bash
sudo apt install microsoft-edge-stable
```
将microsoft-edge-stable替换成microsoft-edge-beta或microsoft-edge-dev来安装 Beta 版或开发版。

安装完成后，你可以点击图标或在「终端」中执行microsoft-edge & 来打开使用

# 更新edge浏览器

在更新 Ubuntu 系统时，Edge 浏览器就会自动更新：
```bash
sudo apt update && sudo apt upgrade -y
```
或者，您可以主动升级 Edge 包：
```bash
sudo apt upgrade microsoft-edge-{version}
```

# 卸载edge浏览器

* 在 Ubuntu 中卸载 Microsoft Edge 非常容易，只需在「终端」中执行如下命令即可：
```bash
sudo apt autoremove microsoft-edge-stable --purge
```

* 卸载完成后，可执行以下命令删除 microsoft-edge.gpg 密钥：
```bash
sudo rm -rf /etc/apt/trusted.gpg.d/microsoft*
sudo rm -rf /usr/share/keyrings/microsoft*
```

* 最后，还需要删除导入的 edge 官方源：
```bash
sudo rm -rf /etc/apt/sources.list.d/microsoft-edge.list
```

参考链接：https://www.sysgeek.cn/ubuntu-install-microsoft-edge/
