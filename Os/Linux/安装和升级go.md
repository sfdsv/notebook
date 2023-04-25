# Ubuntu20.04 安装和升级go

### 1.若系统之前存在旧版本的go（无则跳过此步骤）

```bash
# go默认
sudo rm -rf /usr/local/go

sudo apt-get remove golang

sudo apt-get remove golang-go

sudo apt-get autoremove
```

检查运行go，提示命令不存在。

### 2.获取安装包

```bash
# wget后面的下载链接请去golang官网(https://golang.google.cn/dl/)获取你想下载的对应go版本
sudo wget https://golang.google.cn/dl/go1.18.5.linux-amd64.tar.gz

# 解压文件
sudo tar xfz go1.18.5.linux-amd64.tar.gz -C /usr/local
```

### 3.设置环境变量

打开文件：

```bash
sudo gedit /etc/profile
```

将以下内容追加到文件末尾：

```bash
export GOROOT=/usr/local/go
export GOPATH=$HOME/workspace/go
export GOBIN=$GOPATH/bin
export PATH=$GOPATH:$GOBIN:$GOROOT/bin:$PATH
```

点击保存。

### 4.使环境变量生效

临时生效，关闭终端重新打开后会失效。

```bash
source /etc/profile
```

长期生效,重新启动系统或在用户根目录的`.bashrc`文件末尾加入命令：

```bash
source /etc/profile
```

### 5.查看环境是否ok

```bash
go env
```

下列内容是处于升级go版本，所以只看`GOVERSION`就可以。

```
GO111MODULE="on"
GOARCH="amd64"
GOBIN=""
GOCACHE="/home/luz/.cache/go-build"
GOENV="/home/luz/.config/go/env"
GOEXE=""
GOEXPERIMENT=""
GOFLAGS=""
GOHOSTARCH="amd64"
GOHOSTOS="linux"
GOINSECURE=""
GOMODCACHE="/home/luz/workspace/go/pkg/mod"
GONOPROXY="gitlab.hcsci.cn,git.hcsci.com"
GONOSUMDB="gitlab.hcsci.cn,git.hcsci.com"
GOOS="linux"
GOPATH="/home/luz/workspace/go"
GOPRIVATE="gitlab.hcsci.cn,git.hcsci.com"
GOPROXY="https://goproxy.cn,direct"
GOROOT="/usr/local/go"
GOSUMDB="sum.golang.org"
GOTMPDIR=""
GOTOOLDIR="/usr/local/go/pkg/tool/linux_amd64"
GOVCS=""
GOVERSION="go1.19.4"
GCCGO="gccgo"
GOAMD64="v1"
AR="ar"
CC="gcc"
CXX="g++"
CGO_ENABLED="1"
GOMOD="/dev/null"
GOWORK=""
CGO_CFLAGS="-g -O2"
CGO_CPPFLAGS=""
CGO_CXXFLAGS="-g -O2"
CGO_FFLAGS="-g -O2"
CGO_LDFLAGS="-g -O2"
PKG_CONFIG="pkg-config"
GOGCCFLAGS="-fPIC -m64 -pthread -Wl,--no-gc-sections -fmessage-length=0 -fdebug-prefix-map=/tmp/go-build3049205680=/tmp/go-build -gno-record-gcc-switches"
```

### 6.开启GO111MOUDLE和更改GOPROXY

```bash
# 可以追加公司git，以逗号分隔
go env -w GOPROXY="https://goproxy.cn"

# 当识别到go.mod时自动开启Go Module
go env -w GO111MODULE=on
```

