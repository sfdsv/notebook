# Ubuntu20.04安装DHCP服务和手动配置IP

## 1.安装isc-dhcp-server

```bash
sudo apt install isc-dhcp-server
```

## 2.配置dhcp网口

* ifconfig选择dhcp网口，比如"enp3s0"。（以下网口中没有enp3s0是因为本文为后续补充）

```bash
(base) luz@luz-Vostro-3470:~$ ifconfig
docker0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        inet6 fe80::42:73ff:fe1d:7de9  prefixlen 64  scopeid 0x20<link>
        ether 02:42:73:1d:7d:e9  txqueuelen 0  (以太网)
        RX packets 187862  bytes 11829048 (11.8 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 324798  bytes 607401612 (607.4 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

enp1s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.5.10.87  netmask 255.255.255.0  broadcast 10.5.10.255
        inet6 fe80::580b:75d:4c37:924e  prefixlen 64  scopeid 0x20<link>
        ether 00:be:43:fe:c3:78  txqueuelen 1000  (以太网)
        RX packets 10172693  bytes 9013457603 (9.0 GB)
        RX errors 0  dropped 250379  overruns 0  frame 0
        TX packets 6912554  bytes 4259260238 (4.2 GB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (本地环回)
        RX packets 68101  bytes 7702886 (7.7 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 68101  bytes 7702886 (7.7 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

* 在/etc/default/isc-dhcp-server里的INTERFACESv4填写为对应的网口名如“enp3s0”。

```bash
(base) luz@luz-Vostro-3470:~$ cat /etc/default/isc-dhcp-server 
# Defaults for isc-dhcp-server (sourced by /etc/init.d/isc-dhcp-server)

# Path to dhcpd's config file (default: /etc/dhcp/dhcpd.conf).
#DHCPDv4_CONF=/etc/dhcp/dhcpd.conf
#DHCPDv6_CONF=/etc/dhcp/dhcpd6.conf

# Path to dhcpd's PID file (default: /var/run/dhcpd.pid).
#DHCPDv4_PID=/var/run/dhcpd.pid
#DHCPDv6_PID=/var/run/dhcpd6.pid

# Additional options to start dhcpd with.
#	Don't use options -cf or -pf here; use DHCPD_CONF/ DHCPD_PID instead
#OPTIONS=""

# On what interfaces should the DHCP server (dhcpd) serve DHCP requests?
#	Separate multiple interfaces with spaces, e.g. "eth0 eth1".
INTERFACESv4="enp3s0"
INTERFACESv6=""
```

* 再将"enp3s0"网口对应的ip在设置里改为手动填写，填写对应的ip，如172.192.31.200（最好在DHCP分配ip池范围之外，以免冲突）。

## 3.dhcp配置

* 新建dhcpd.enp3so.conf文件，添加dhcp配置如下:

  ```bash
  (base) luz@luz-Vostro-3470:~$ cat /etc/dhcp/dhcpd.enp3s0.conf 
  subnet 172.192.63.0 netmask 255.255.255.0 {
  	range 172.192.63.210 172.192.63.220;
  #	option routers 172.192.63.1;
  	deny unknown-clients;
  }
  
  host ot2_1 {
  	hardware ethernet B8:27:EB:92:65:AC;
  	fixed-address 172.192.63.201;
  }
  
  host ot2_1 {
  	hardware ethernet B8:27:EB:56:BC:DF;
  	fixed-address 172.192.63.202;
  }
  
  host computer {
  	hardware ethernet 00:BE:43:E1:BC:91;
  	fixed-address 172.192.63.203;
  }
  
  ```

* 在/etc/dhcp/dhcpd.conf文件末尾添加刚配置好的文件如下：

```bash
(base) luz@luz-Vostro-3470:~$ cat /etc/dhcp/dhcpd.conf 
# dhcpd.conf
#
# Sample configuration file for ISC dhcpd
#
# Attention: If /etc/ltsp/dhcpd.conf exists, that will be used as
# configuration file instead of this file.
#

# option definitions common to all supported networks...
option domain-name "example.org";
option domain-name-servers ns1.example.org, ns2.example.org;

default-lease-time 600;
max-lease-time 7200;

# The ddns-updates-style parameter controls whether or not the server will
# attempt to do a DNS update when a lease is confirmed. We default to the
# behavior of the version 2 packages ('none', since DHCP v2 didn't
# have support for DDNS.)
ddns-update-style none;

# 省略了一大段注释

include "/etc/dhcp/dhcpd.enp3s0.conf";
```

## 4.启动dhcp服务

这里就不演示了。

```bash
sudo systemctl start isc-dhcp-server
sudo systemctl status isc-dhcp-server
```



---

补充：2023年04月13日15:33:29

**Windows客户端操作系统不能安装DHCP服务器。**

> 抱歉，我之前的回答有误。在 Windows 客户端操作系统上，无法安装 DHCP 服务器，因为 DHCP 服务器是一种服务器角色，通常需要在 Windows Server 操作系统上安装和配置。
>
> 如果您想在 Windows 客户端操作系统上使用 DHCP 服务，您需要连接到运行 DHCP 服务器角色的 Windows Server，并从该服务器上获取动态分配的 IP 地址和其他网络配置信息。在 Windows 客户端操作系统上，DHCP 客户端功能通常默认启用，并且会在网络连接配置中自动寻找 DHCP 服务器。
>
> 如果您需要设置和管理 DHCP 服务器，您需要在 Windows Server 操作系统上安装并配置 DHCP 服务器角色。以下是在 Windows Server 上安装和配置 DHCP 服务器的大致步骤：
>
> 登录到运行 Windows Server 操作系统的服务器，并以管理员身份登录。
> 打开 "服务器管理器"，可以通过在任务栏或开始菜单中搜索 "服务器管理器"，然后点击搜索结果中的 "服务器管理器" 打开。
> 在 "服务器管理器" 中，选择 "角色" 或 "角色和功能"，然后点击 "添加角色" 或 "添加角色和功能"。
> 在 "添加角色" 或 "添加角色和功能" 向导中，选择 "DHCP 服务器" 角色，然后点击 "下一步"。
> 阅读和接受 DHCP 服务器的许可协议，然后点击 "下一步"。
> 在 "选择安装类型" 中，通常选择 "典型安装"，然后点击 "下一步"。
> 在 "选择网络连接" 中，选择用于连接到网络的网络适配器，然后点击 "下一步"。
> 在 "IPv4 DHCP 范围" 中，配置 DHCP 服务器的 IP 地址范围、租约期限等参数，然后点击 "下一步"。
> 在 "IPv4 DHCP 范围的授权" 中，选择是否授权 DHCP 服务器分配 IP 地址，然后点击 "下一步"。
> 在 "完成安装" 中，检查配置信息，然后点击 "安装"。
> 等待安装完成，然后点击 "关闭"。
> 在 "服务器管理器" 中，可以找到 "DHCP" 角色已经安装并在左侧的菜单中显示。
> 配置其他 DHCP 服务器选项，例如 DHCP 选项、保留地址等，以满足您的网络需求。
> 请注意，配置 DHCP 服务器可能涉及到复杂的网络设置和安全考虑，建议在熟悉 DHCP 服务和网络管理的情况下进行配置，并遵循安全最佳实践。



