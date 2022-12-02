ARP是地址解析协议(Address Resolution Protocol)，是将IP地址解析为MAC地址的一个协议。

主机或三层网络设备上会维护一张ARP表，用于存储IP地址和MAC地址的映射关系，一般ARP表项包括动态ARP表项和静态ARP表项。

Ping基于此协议。

![image-20221125191907592](https://cdn.jsdelivr.net/gh/sfdsv/pic/img/202211251919681.png)

广播（255.255.255.255）发送ARP请求，单播发送ARP响应。

**当需要通信的两台主机处于同一网段时**，如Host_1和Host_3，Host_1要向Host_3发送数据。

首先，Host_1会查找自己**本地缓存的ARP表**，确定是否包含Host_3对应的ARP表项。如果Host_1在ARP表中找到了Host_3对应的MAC地址，则Host_1直接利用ARP表中的MAC地址，对数据报文进行帧封装，并将数据报文发送给Host_3。

如果Host_1在ARP表中找不到Host_3对应的MAC地址，则**先缓存该数据报文**，并以**广播方式发送一个ARP请求报文**。如上图中所示，OP字段为1表示该报文为ARP请求报文，ARP请求报文中的源MAC地址和源IP地址为Host_1的MAC地址和IP地址，目的MAC地址为全0的MAC地址（广播的MAC），目的IP地址为Host_3的IP地址。

Switch_1收到ARP请求报文后，将该ARP请求报文在**同一广播域内转发**。

同一广播域内的主机Host_2和Host_3都能接收到该ARP请求报文，但只有被请求的主机（即Host_3）会对该ARP请求报文进行处理。Host_3比较自己的IP地址和ARP请求报文中的目的IP地址，当两者相同时进行如下处理：将ARP请求报文中的源IP地址和源MAC地址（即Host_1的IP地址和MAC地址）存入自己的ARP表中。之后以**单播方式发送ARP应答报文**给Host_1，ARP应答报文内容如上图中所示，OP字段为2表示该报文为ARP应答报文，源MAC地址和源IP地址为Host_3的MAC地址和IP地址，目的MAC地址和目的IP地址为Host_1的MAC地址和IP地址。

Switch_1收到ARP应答报文后，将该ARP应答报文转发给Host_1。Host_1收到ARP应答报文后，将Host_3的MAC地址加入到自己的ARP表中以用于后续报文的转发，同时将数据报文进行帧封装，并将数据报文发送给Host_3。

**当需要通信的两台主机处于不同网段时**，如上图中的Host_1和Host_4，Host_1上已经配置**缺省网关**，Host_1首先会发送ARP请求报文，请求网关Router的IP地址对应的MAC地址。Host_1收到ARP应答报文后，将数据报文封装并发给网关，再由网关将数据报文发送给目的主机Host_4。

