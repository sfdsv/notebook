## 1.首先进入宿主机的项目目录Meddle

```bash
cd $GOPATH/src/helloGL/3.GL.CODE/Meddle
```

## 2.拉取ubuntu20.04镜像

```bash
docker pull ubuntu:20.04
```

## 3.运行ubuntu20.04容器

```bash
docker run -it -v $PWD:/go/src/Meddle ubuntu:20.04 /bin/bash
```

## 4.安装go环境

```bash
# 下载 wget 工具包
apt-get update
apt-get install -y wget

# 下载 Golang 1.16.7 版本的二进制包
wget https://golang.org/dl/go1.16.7.linux-amd64.tar.gz

# 解压二进制包
tar -C /usr/local -xzf go1.16.7.linux-amd64.tar.gz

# 添加 Golang 到 PATH 环境变量
export PATH=$PATH:/usr/local/go/bin

# 设置 go 环境变量 
go env -w GOPATH="/go"
go env -w GOPROXY="https://goproxy.cn,direct"
go env -w GOPRIVATE="gitlab.hcsci.cn"

# 验证 go 环境
go version
go env
```

## 5.安装git环境
```bash
root@14e95ba83cdf:/go/src/Meddle# go mod tidy
go: gitlab.hcsci.cn/tools1/linearFitting/v3@v3.0.0: git init --bare in /go/pkg/mod/cache/vcs/b21227971fda26895345b1b989893c602a1202a8172700303ec63c1b37353187: exec: "git": executable file not found in $PATH
```
> 根据错误提示，您在 Docker 容器中执行 go mod tidy 命令时出现了错误，指示找不到 git 可执行文件。这是因为在 Docker 容器中安装的 Golang 不包含 git 工具，而 go mod tidy 命令需要使用 git 进行版本控制操作。
>
> 要解决这个问题，您需要在 Docker 容器中安装 git 工具。以下是在 Ubuntu 20.04 Docker 容器中安装 git 的步骤：
>
> 在容器内执行以下命令安装 git：
>
> ```bash
> apt-get update
> apt-get install -y git
> ```
>
> 安装完 git 后，您可以再次尝试执行 go mod tidy 命令，应该不再出现 "git: executable file not found in $PATH" 的错误。
>
> ```bash
> go mod tidy
> ```
>
> 以上步骤将在 Ubuntu 20.04 Docker 容器中安装 git 工具，解决 "git: executable file not found in $PATH" 错误，并允许您在容器内正常执行 go mod tidy 命令进行 Golang 模块管理操作。

## 6.安装gcc

```bash
root@14e95ba83cdf:/go/src/Meddle# go build main.go
# github.com/mattn/go-sqlite3
cgo: exec gcc: exec: "gcc": executable file not found in $PATH
```

> 根据错误提示，您在 Docker 容器中执行 go build 命令时出现了错误，指示找不到 gcc 可执行文件。这是因为在 Docker 容器中缺少 gcc 工具，而 go-sqlite3 依赖于 cgo 编译，需要使用 gcc 进行 C 语言部分的编译操作。
>
> 要解决这个问题，您需要在 Docker 容器中安装 gcc 工具。以下是在 Ubuntu 20.04 Docker 容器中安装 gcc 的步骤：
>
> 在容器内执行以下命令安装 gcc：
>
> ```bash
> apt-get update
> apt-get install -y build-essential
> ```
>
> 安装完 gcc 后，您可以再次尝试执行 go build 命令，应该不再出现 "gcc: executable file not found in $PATH" 的错误。
>
> ```bash
> go build main.go
> ```
>
> 以上步骤将在 Ubuntu 20.04 Docker 容器中安装 gcc 工具，解决 "gcc: executable file not found in $PATH" 错误，并允许您在容器内正常进行 Go 代码的编译操作，包括使用 go-sqlite3 这样需要 cgo 编译的包。

## 7.导出镜像

> 在 Docker 中，可以使用 `docker save` 命令将一个镜像导出到一个压缩文件中，以便在其他环境中导入和使用。以下是导出镜像的步骤：
>
> 运行以下命令导出指定的镜像，其中`` <IMAGE_NAME>`` 是您要导出的镜像的名称，`<TAG>` 是镜像的标签，``<OUTPUT_FILE> `是导出的文件路径和文件名。
>
> ```bash
> docker save -o <OUTPUT_FILE> <IMAGE_NAME>:<TAG>
> ```
>
>
> 例如，导出名为`` my-golang-image` 标签为` latest `的 Golang 镜像到文件` /path/to/my-golang-image-latest.tar `可以使用以下命令：
>
> ```bash
> docker save -o /path/to/my-golang-image-latest.tar my-golang-image:latest
> ```
>
> Docker 将会将指定的镜像导出到一个压缩文件中，保存在 `<OUTPUT_FILE> `指定的路径中。
> 现在您可以将导出的镜像文件传输到其他环境，并使用 `docker load `命令将其导入到 Docker 中，以在其他环境中使用该镜像。
>
> 注意：导出的镜像文件是二进制文件，可以通过文件传输工具（如 scp、ftp 等）传输到其他环境，并在目标环境上使用 `docker load `命令导入到 Docker 中。

## 8.导入镜像

> 在 Docker 中，可以使用` docker load` 命令将一个导出的镜像文件加载到 Docker 中，以便在当前 Docker 环境中使用该镜像。以下是导入镜像的步骤：
>
> 将导出的镜像文件传输到当前 Docker 环境中，可以使用文件传输工具（如 scp、ftp 等）将导出的镜像文件复制到目标 Docker 主机。
>
> 在 Docker 主机上，运行以下命令导入镜像，其中 `<INPUT_FILE>`是导出的镜像文件的路径和文件名。
>
> ```bash
> docker load -i <INPUT_FILE>
> ```
>
> 例如，导入名为` /path/to/my-golang-image-latest.tar` 的 Golang 镜像可以使用以下命令：
>
> ```bash
> docker load -i /path/to/my-golang-image-latest.tar
> ```
>
> Docker 将会从导出的镜像文件中加载镜像，并将其添加到 Docker 镜像库中，可以使用 `docker images` 命令查看已导入的镜像。
> 现在您可以在当前 Docker 环境中使用已导入的镜像，例如通过运行容器基于该镜像启动新的容器。
>
> 注意：导入的镜像将添加到当前 Docker 环境中的镜像库中，可以使用 `docker images` 命令查看已导入的镜像，并使用其名称和标签启动容器。
