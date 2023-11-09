# 网管

### OSPF

OSPF是IETF组织开发的一个基于链路状态的内部网关协议，广泛应用于接入网和城域网中。目前针对IPv4协议使用OSPF Version 2。在OSPF出现前，网络上广泛使用RIP（Routing Information Protocol）作为内部网关协议。由于RIP是基于距离矢量算法的路由协议，存在着收敛慢、路由环路、可扩展性差等问题，所以逐渐被OSPF取代

采用最短路径SPF（Shortest Path First）算法。通过链路状态通告LSA（Link State Advertisement）描述网络拓扑，依据网络拓扑生成一棵最短路径树SPT（Shortest Path Tree），计算出到网络中所有目的地的最短路径，进行路由信息的交换。收敛速度快，小于1s。应用于规模适中的网络中，最多可支持几百台ME设备。例如，中小型企业网络。

### 升级与上载

设备软件主要包括linux程序和app文件。设备上电后，先运行linux程序，初始化硬件并显示设备的硬件参数，然后运行app文件；app文件一方面提供对硬件的驱动和适配功能，另一方面实现了业务特性。linux程序与app文件是设备启动、运行的必备软件，为整个设备提供支撑、管理、业务等功能。

设备提供app文件、sdn文件、fpga文件、全量flash文件四种文件包

> <mark>app文件：是设备OTP5600主要的运行程序文件，一般情况下升级该文件，文件大小11M左右；</mark>

> <mark>sdn文件：是运行netconf管理协议的yang文件，主要负责设备对接上层的netconf控制器；</mark>

> <mark>fpga文件：是硬件的驱动文件，和app相匹配运行，最新为V2.22版本；</mark>

> <mark>flash文件：是设备整个全量升级包，包括所有的文件，可以烧录到闪存直接升级，目前分两种，一种是32M的全量升级包，一种是37M的全量升级包，根据主控板卡的硬件差异进行选择烧录升级；</mark>

上载SDN文件，~~下载略~~

```
OTP5600-II(config)#upload ftp file /mnt/flash/gwd/sdn/wholePack.bin 132.234.25.21 123 123 dci-sdn.bin
```

上载FPGA文件，~~下载略~~

```
upload ftp file /mnt/flash/gwd/otp5600.fpga 132.234.25.22 123 123 otp5600.fpga
```

上载FLASH文件，<u>下载不支持</u>

```
upload ftp flash 132.234.25.22 123 123 flash.bin gpn
```

### 网元配置

**5500**

```
interface loopback 1
ip address 132.234.25.22/32
exit
```

```
vlan 10
ip address unnumberd loopback 1
add port 9/1 untagged
exit
```

```
router ospf
  network 132.234.25.22/32 area 0
  default-information originate always			
exit
```

```
vlan 10
ip ospf network point-to-point
exit
```

**5600**

```
interface loopback 1
ip address 132.234.25.26/32
exit
```

```
vlan 10
ip address unnumberd loopback 1
add port 9/1 untagged
exit
```

```
router ospf
  network 132.234.25.23/32 area 0		
exit
```

```
vlan 10
ip ospf network point-to-point
exit
```

```
interface och 1/1
set frequency 191.4
```

### OSPF排故

OTP5600-GEJD板卡（光放板卡）和OTP5600-WRDY（wss板卡）目前硬件带OSC模块，根据型号不同，分别带不同数量的千兆OSC模块和百兆OSC模块，具体信息参考市场发布的产品配置表。

排查OSC模块是否工作正常

```
interface wdm_osc <slot/1>
show sfp
```

<font face="仿宋">说明：OSC模块发光`2dBm`，收光`-30dBm`左右</font>

查看OSC模块互通协商是否UP

```
show port-link
```

<font face="仿宋">说明:OSC模块一般是两个互通使用，目前千兆对千兆，百兆对通百兆，默认起来是强制协商；两个OSC模块正常都能协商起来，如果没协商起来，<u>auto enable</u>开启自动协商。</font> 

查看OSPF配置信息是否正确

```
show running-config ospf 
```

查看OSPF邻居信息是否正确

```
show ip ospf neighbor
```

### FreeSSHd软件使用

- 软件上传工单，需要可登录下载。

<img src="https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/2023-09-22_153810.png" style="zoom:50%;" />  

图为软件界面，需要设置的项为Users、SSH、Authentication、SFTP四项。

- Users：添加用户名和密码

<img src="https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/2023-09-22_154252.png" style="zoom:53%;" /> 

- SSH：设置监听ip和端口

<img src="https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/2023-09-22_154622.png" style="zoom:50%;" /> 

- Authentication：设置密匙key方式，按图设置

<img src="https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/2023-09-22_160031.png" style="zoom:50%;" />  

- SFTP:    date文件存放路径

<img src="https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/2023-09-22_155807.png" style="zoom:50%;" /> 

最后点击“`Click here to start it`” 启动server服务器。报错一般是电脑系统服务项开启了，关闭即可。

### ~~NAT升级~~

网关网元可直接升级，非网关网元需要将网关网元当作ftp-server来升级：

1.配置网关网元ftp-server的用户名、密码

```
config ftpserver createuser root
config ftpserver createpassword public

config节点下：show running ftpserver    ##查询配置信息
```

2.升级非网关网元

```
download  ftp file /tmp/OTP5600_V04R20C09B006.bin 192.168.51.166(PCip) 123 123 OTP5600_V04R20C09B006.bin   ## 先将文件下载到网关网元指定目录/tmp下
```

```
download ftp app 133.68.70.219(loopback1 ip) root public OTP5600_V04R20C09B006.bin gpn
```

