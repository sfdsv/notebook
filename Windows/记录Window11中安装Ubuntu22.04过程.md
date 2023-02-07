# 记录Window11中通过WSL2安装Ubuntu22.04过程

### 环境

**操作系统**

    版本	Windows 11 专业版
    版本	21H2
    安装日期	2022/3/28
    操作系统版本	22000.1455
    体验	Windows 功能体验包 1000.22000.1455.0

### 激活WSL

WSL是Windows subSystem For Linux。

**允许windows启动Linux子系统**，通过控制面板设置：

* 控制面板额 -> 程序 -> 启用或关闭Windows功能
* 勾选“**适用于Linux的Windows子系统**"与"**虚拟机平台**"。

<img width="620" alt="image" src="https://user-images.githubusercontent.com/73980771/207613954-990c5906-2eb6-49d9-93c2-c9f816cd232a.png">

设置完成后重启电脑生效.

打开Windows PowerShell，查看**wslconfig用法**：
```shell
PS C:\Desktop> wslconfig
在适用于 Linux 的 Windows 子系统上执行管理操作

使用情况:
    /l, /list [Option]
        列出已注册的分发。
        /all - 可选择列出所有分发，包括
               当前正在安装或卸载的分发。

        /running - 只列出当前正在运行的分发。

    /s, /setdefault <DistributionName>
        将分发设置为默认值。

    /t, /terminate <DistributionName>
        终止分发。

    /u, /unregister <DistributionName>
        注销分发并删除启动文件系统。
```

查看分发版本：

```shell
PS C:\Desktop> wslconfig /l
适用于 Linux 的 Windows 子系统分发:
docker-desktop (默认)
Ubuntu-22.04
docker-desktop-data
```

**注销分发版本**：

```shell
PS C:\Desktop> wslconfig /u Ubuntu-22.04
正在注销。
操作成功完成。
```

与上述注销操作对应的文件夹：

```
C:\Users\UserName\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu22.04LTS_79rhkp1fndgsc\LocalState
```

该文件夹下的vhdx镜像文件已经被删除。

### 安装Ubuntu22.04

官网教程:[使用 WSL 在 Windows 上安装 Linux](https://learn.microsoft.com/zh-cn/windows/wsl/install)

在微软商城搜索ubuntu22.04，点击安装：

<img width="257" alt="image" src="https://user-images.githubusercontent.com/73980771/207614195-673f8c15-5e27-4da8-b633-3897ee203dff.png">

### 配置子系统

#### 启动子系统

从开始菜单里启动ubuntu22.04 LTS软件。

**踩坑**：

1. 提示WslRegisterDistribution failed with error: 0x800701bc

    原因：内核未升级。

    解决办法：打开微软商店，搜索wsl，点击安装`Windows Subsystem for Linux`。

<img width="540" alt="image" src="https://user-images.githubusercontent.com/73980771/207621856-b4a96b85-573e-4387-a19b-fe5f6748a3e6.png">

2. 提示WslRegisterDistribution failed with error: 0x80071772
    
    原因：子系统没有安装在C盘导致。

    解决办法：

     1. 在设置的应用里，找到Ubuntu22.04 LTS，点击移动，将其移动到C盘。
    
    <img width="533" alt="image" src="https://user-images.githubusercontent.com/73980771/207622684-3636d1b1-4ce3-4ab7-a3ff-da143e36f5d5.png">

     2. 首先卸载Ubuntu，然后在设置->系统->存储->更改新内容的保存位置，将“新的应用保存到”修改为C盘，并重新安装 Ubuntu。
    
    <img width="418" alt="image" src="https://user-images.githubusercontent.com/73980771/207622914-fe918f46-5502-490d-8de3-b543eee5ebea.png">


#### 设置用户名和密码

第一次进入ubuntu软件后，默认需要设置用户名(**全小写**，不是Window的用户名)和密码。

**注意：**

如果第一次进入的时候由于各种原因退出，没有设置用户名和密码，那么第二次进入就直接是root用户了。

可以根据如下操作新建普通用户，并设置为默认用户：（参考链接：https://www.jianshu.com/p/5bfeb5920fb1）

1. 新建普通用户

```bash
# 新建普通用户:-m 创建用户时建立home目录，-s使用bash
useradd -m luz -s /bin/bash
# 设置密码
passwd luz

# 重启ubuntu子系统，默认以root用户登录

# 赋予新建用户sudo权限
sudo adduser luz sudo 或者sudo usermod -aG sudo luz
# 查看用户是否具有sudo访问权限
sudo -l -U luz
```

```
root@DESKTOP-1P6CG7H:~# useradd -m luz -s /bin/bash
root@DESKTOP-1P6CG7H:~# passwd luz
New password:
Retype new password:
passwd: password updated successfully

root@DESKTOP-1P6CG7H:~# adduser luz sudo
Adding user `luz' to group `sudo' ...
Adding user luz to group sudo
Done.
luz@DESKTOP-1P6CG7H:~$ sudo -l -U luz
Matching Defaults entries for luz on DESKTOP-1P6CG7H:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User luz may run the following commands on DESKTOP-1P6CG7H:
    (ALL : ALL) ALL
```

2. 在window powershell或cmd种，切换默认登录用户

```bash
Ubuntu2204 config --default-user luz # 因为我是安装的Ubuntu-22.04，网上的大部分都是Ubuntu
```

**设置root密码**

```bash
# 设置密码
sudo passwd
# 切换到root
su root
```

```bash
luz@DESKTOP-1P6CG7H:~$ sudo passwd
New password:
Retype new password:
passwd: password updated successfully
luz@DESKTOP-1P6CG7H:~$ su root
Password:
root@DESKTOP-1P6CG7H:/home/luz#
```

**验证用户是否具有sudo访问权限** 

```bash
sudo -l -U luz
```

```bash
root@DESKTOP-1P6CG7H:~# sudo -l -U luz
User luz is not allowed to run sudo on DESKTOP-1P6CG7H.
```

#### 安装gedit

为了方便修改配置文件，喜欢用gedit的就可以安装。

```bash
sudo apt-get install gedit
```

#### 换国内源

1.备份原来的source文件

```bash
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak 
```

2.修改sources.list文件

```bash
sudo gedit /etc/apt/sources.list
```

3.添加国内源，选择其一就可以了。

```
# 中科大镜像源
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
# 阿里镜像源
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
# 清华源
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
# 网易163源
deb http://mirrors.163.com/ubuntu/ jammy main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ jammy-security main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ jammy-updates main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ jammy-proposed main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ jammy-backports main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ jammy main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ jammy-security main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ jammy-updates main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ jammy-proposed main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ jammy-backports main restricted universe multiverse
```

4.然后执行命令更新源

```bash
sudo apt-get update
sudo apt-get upgrade
```

#### 将空间迁移至其他盘

##### wsl2迁移

wsl的分发版会默认安装到C盘.

1. 终止正在运行的wsl

```shell
wsl --shutdown
wsl -l -v #查看子系统状态是否停止
```

2. 将需要迁移的Linux分发版，进行导出

```
# wsl --export 子系统名称 导出文件路径及名称
wsl --export Ubuntu-22.04 D:\export.tar
```

```shell
PS C:\Users\Hcsci> wsl --shutdown
PS C:\Users\Hcsci> wsl -l -v
  NAME                   STATE           VERSION
* Ubuntu-22.04           Stopped         2
  docker-desktop         Running         2
  docker-desktop-data    Stopped         2
PS C:\Users\Hcsci> wsl --list --all
适用于 Linux 的 Windows 子系统分发:
docker-desktop (默认)
docker-desktop-data
Ubuntu-22.04
PS C:\Users\Hcsci> wsl --export Ubuntu-22.04 D:\export.tar
正在导出，这可能需要几分钟时间。
操作成功完成。
```
导出完成后，在对应文件路径下，出现export.tar文件。

3. 导出完成后,就需要将原有的分发进行卸载

```shell
# wsl --unregister 子系统名称
wsl --unregister Ubuntu-22.04
```
```
PS C:\Users\Hcsci> wsl --unregister Ubuntu-22.04
正在注销。
操作成功完成。
```
4. 然后将导出的文件放到需要保存的地方，进行导入即可

```shell
# wsl --import 子系统名称 导入运行目录 子系统备份路径
wsl --import Ubuntu-22.04 'D:\wsl\Ubuntu' D:\export.tar --version 2
```

```
PS C:\Users\Hcsci> wsl --import Ubuntu-22.04 'D:\wsl\ubuntu2204' D:\export.tar --version 2
正在导入，这可能需要几分钟时间。
操作成功完成。
```

扩展：

```bash
启动子系统
# wsl -d 子系统名称
设置默认子系统
# wslconfig.exe /s 子系统名称
修改分发版的WSL版本，只能修改为1或2
# wsl --set-version Ubuntu-20.04 1
设置默认登录用户
# Ubuntu2004 config --default-user root
```

**此迁移方法也适用于docker分发版。**

#### systemd不支持的解决

参考链接:https://www.5axxw.com/wiki/content/k34hu0

**用法**:

1. 需要安装git来工作.

```bash
sudo apt install git
```

2. 运行脚本和命令

```bash
git clone https://github.com/DamionGans/ubuntu-wsl2-systemd-script.git
cd ubuntu-wsl2-systemd-script/
bash ubuntu-wsl2-systemd-script.sh
# 键入密码,然后等待直到脚本执行完成
```

3. 然后重新启动ubuntushell并尝试运行systemctl

```bash
systemctl
```

如果没有得到错误并看到单元列表，那么脚本就起作用了。

##### 踩坑

问题描述:

按照用法安装完后,双击ubuntu22.04软件闪退.在windows中powershell启动ubuntu22.04子系统:

```bash
PS C:\Users\Hcsci> wsl -d Ubuntu-22.04
Sleeping for 1 second to let systemd settle
nsenter: cannot open /proc/26/ns/time: No such file or directory
```

解决方法:

参照链接:https://github.com/DamionGans/ubuntu-wsl2-systemd-script/issues/76

1. 打开windows powershell,使用root账户启动ubuntu22.04子系统:

```bash
PS C:\Users\Hcsci> wsl -d Ubuntu-22.04 --user root
root@DESKTOP-1P6CG7H:/mnt/c/Users/Hcsci#
```

2. 修改`enter-systemd-namespace`文件中nsenter命令的选项 , -a选项不合适 , 改成-m -p即可.

```bash
# 1.进入ubuntu-wsl2-systemd-script目录
cd /home/luz/ubuntu-wsl2-systemd-script
# 2.打开enter-systemd-namespace文件
gedit enter-systemd-namespace
# 3.将文件中的对应内容替换掉
USER_HOME="$(getent passwd | awk -F: '$1=="'"$SUDO_USER"'" {print $6}')"
if [ -n "$SYSTEMD_PID" ] && [ "$SYSTEMD_PID" != "1" ]; then
    if [ -n "$1" ] && [ "$1" != "bash --login" ] && [ "$1" != "/bin/bash --login" ]; then
        exec /usr/bin/nsenter -t "$SYSTEMD_PID" -m -p \
            /usr/bin/sudo -H -u "$SUDO_USER" \
            /bin/bash -c 'set -a; [ -f "$HOME/.systemd-env" ] && source "$HOME/.systemd-env"; set +a; exec bash -c '"$(printf "%q" "$@")"
    else
        exec /usr/bin/nsenter -t "$SYSTEMD_PID" -m -p \
            /bin/login -p -f "$SUDO_USER" \
            $([ -f "$USER_HOME/.systemd-env" ] && /bin/cat "$USER_HOME/.systemd-env" | xargs printf ' %q')
    fi
    echo "Existential crisis"
    exit 1
fi
# 4.然后重新安装
bash ubuntu-wsl2-systemd-script.sh --force
```

3. 重新启动ubuntu22.04软件
4. 验证sysytemctl是否ok

在子系统中尝试运行systemctl, 如果没有得到错误,并看到单元列表,那么就成功了.



> 后面突然看到说WSL2已经支持systemctl了,只需要修改一下配置文件就可以.
>
> 参考链接:https://ubuntu.com//blog/ubuntu-wsl-enable-systemd
>
> <img width="503" alt="image" src="https://user-images.githubusercontent.com/73980771/207614715-f2928c30-4fcd-4f97-9a22-50915866240a.png">
>
> 1. 进入ubuntu子系统内部,修改` /etc/wsl.conf`文件:
>
> ```
> [boot]
> systemd=true
> ```
>
> 2. 然后在powershell中关掉wsl,重启ubuntu子系统.
>
> ```
> wsl --shutdown
> ```



#### 安装图形界面

参考链接:

* https://blog.csdn.net/liyunxin_c_language/article/details/114107994
* https://huaweicloud.csdn.net/63561930d3efff3090b5a1a4.html?spm=1001.2101.3001.6650.5&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Eactivity-5-123970357-blog-124548058.pc_relevant_3mothn_strategy_recovery&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Eactivity-5-123970357-blog-124548058.pc_relevant_3mothn_strategy_recovery&utm_relevant_index=10#devmenu1

##### 安装gnome

```bash
sudo apt update
sudo apt upgrade
sudo apt install -y ubuntu-desktop
```

### 资源管理器访问wsl

* 左侧导航栏linux

* 路径输入`\\wsl$`

