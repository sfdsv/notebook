# Install protoc on Ubuntu 20.04

来源：https://lindevs.com/install-protoc-on-ubuntu/

## 环境准备

确保已经安装了解压包程序。

```bash
sudo apt update
sudo apt install -y unzip
```
## 安装protoc

获取protoc的发布版本的最近版本号，并将它赋值给变量:

```bash
https://lindevs.com/install-protoc-on-ubuntu/
```

从仓库的发布页下载Zip压缩文件：
```bash
curl -Lo protoc.zip "https://github.com/protocolbuffers/protobuf/releases/latest/download/protobuf-all-${PROTOC_VERSION}.zip"
```

运行下列命令解压文件：

```bash
sudo unzip -q protoc.zip bin/protoc -d /usr/local
```

设置执行权限：

```bash
sudo chmod a+x /usr/local/bin/protoc
```

现在protoc命令可以使用了。

我们可以检查protoc版本：

```bash
protoc --version
```

删除没必要的zip文件：

```bash
rm -rf protoc.zip
```

## 测试protoc

创建一个`.proto`文件：

```bash
nano example.proto
```

添加以下代码定义简单的信息格式：

```protobuf
syntax = "proto3";

package example;

message Person {
  string name = 1;
}
```

使用以下命令来解析`.proto`文件：

```bash
protoc --go_out=. --go_opt paths=source_relative example.proto
```

## 卸载protoc

删除可执行文件：

```bash
sudo rm -rf /usr/local/bin/protoc
```

