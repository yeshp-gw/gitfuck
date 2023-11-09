# DCN

### GPN-76-OMU-DCN网管开通

为方便理解，DCN上网管可以想象成拿n个珠子穿一个手串，每个珠子就是一个独立的GPN-76,串珠子的引线看作光纤，通过光纤将每一台设备连接起来，最后实现集某台设备统一管理。

- 场景拓扑

<center><img src="https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/DCN%E6%8B%93%E6%89%91%E5%9B%BE.png" style="zoom: 67%;" /></center>

> <font face="仿宋">注意：NMS主控内部有8路E1，#1到#8自通。此外，连接PC的称网关网元，其他为非网关网元。</font>

- 开通需求

&emsp;要求GPN7600-1、GPN7600-2、GPN7600-3全部上网管，并由GPN7600-1统一管理。

- 可行性分析

&emsp;满足，通过DCN网管通道实现互联。

### 开通配置

GPN7600-1业务配置:

1.&ensp;板卡安装

根据需求分析，需要通过OMU板卡创建一个时隙来满足DCN连接。

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/%E5%AE%89%E8%A3%85%E6%9D%BF%E5%8D%A1.gif)

2.&ensp;配置DCN管理

选中网元，右键打开【网元管理器】，选择【DCN管理】，配置网元类型、优先级、管理 vlan、管理 ip 地址，如图所示：

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/DCN.gif)

3.&ensp;创建 网元到网管的连接

打开【DCN连接配置】对话框，点击【添加】，连接模式选择“网元到网管”；“对端网元 ID”默认 0 即可；若网关网元通过 U/D 口直接上网管，板卡选择“背板”，端口选择“U/D”口，封装模式默认0；若通过主控盘上网管，选择“17-NMS-V1”；端口选择空闲的即可(有8路E1)；选则所需的封装模式，如图所示：

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/%E7%BD%91%E5%85%83%E5%88%B0%E7%BD%91%E7%AE%A1.gif)

<font face="仿宋">封装模式说明：</font>

> <font face="仿宋">“**HDLC** 模式”即“标准 HDLC”；</font> <br>

> <font face="仿宋">“**GWHDLC**”为我司私有封装协议，与我司 E6300P 或 GPN7600 对接可选；</font> <br>

> <font face="仿宋">“**GFP**”模式，只能 GPN7600 之间使用；</font>  <br>

> <font face="仿宋">“**NA**”模式即不封装，创建以太网 DCN 链路时使用。</font>  <br>

4.&ensp;创建网元之间DCN连接（选配，此处2个）

点击【添加】，连接模式选择“非网关网元”，对端网元 ID 必须非 0，参照上述步骤选择板卡、端口及封装模式。如图所示：

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/%E7%BD%91%E5%85%83%E5%88%B0%E7%BD%91%E5%85%83.gif)

5.&ensp;创建`E#1`到`SDH`的交叉

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/E1%E4%B8%8ESDH%E4%BA%A4%E5%8F%89.gif)

GPN7600-2业务配置:

1.&ensp;板卡安装

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/%E5%AE%89%E8%A3%85%E6%9D%BF%E5%8D%A1.gif)

2.&ensp;配置DCN管理

选中网元，右键打开【网元管理器】，选择【DCN管理】，配置网元类型、优先级、管理 vlan、管理 ip 地址，如图所示：

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/%E9%9D%9E%E7%BD%91%E5%85%B3%E7%BD%91%E5%85%83.gif)

3.&ensp;创建 网元之间DCN连接（选配）

点击【添加】，连接模式选择“非网关网元”，对端网元 ID 必须`非 0`，参照上述步骤选择板卡、端口及封装模式。如图所示：

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/%E9%9D%9E%E7%BD%91%E5%85%B3%E7%BD%91%E5%85%8322.gif)

4.&ensp;创建`E#1`到`SDH`的交叉

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/E1%E4%B8%8ESDH%E4%BA%A4%E5%8F%89.gif)

### GPN-76-8AT2-DCN网管开通

### 开通配置

此网管方式上联板卡从SDH变更为OTN，所以板卡安装、DCN管理参考上述。这里仅记录E#1与OTN板卡交叉。

1.&ensp;安装板卡（略）

2.&ensp;配置DCN管理（略）

3.&ensp;配置OTN管理

- 映射配置

在网元管理器窗口选择“OTN交叉连接管理>映射配置”，选择要创建时隙的otu接口，右击选择创建，打开创建odu窗口；

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/8%E7%88%B1%E5%A5%B9.gif)

在网元管理器窗口选择“OTN交叉连接管理>映射配置”，选择要创建SDH背板时隙的接口，右击选择创建，打开创建ODU窗口；

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/8%E7%88%B1%E5%A5%B9SDH%E6%98%A0%E5%B0%84.gif)

-  交叉配置

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/8%E7%88%B1%E5%A5%B9%E4%BA%A4%E5%8F%89%E9%85%8D%E7%BD%AE.gif)

> <font face="仿宋">（注意：需要配置SNCP保护时，保护类型选择SNCP，如不需要默认即可。）</font>

4.&ensp;创建SDH时隙交叉

- 使能高阶通道

由于OTN背板的SDH速率为高阶通道速率，而17主控E1接口为低阶通道时隙，所以需要到我们去将高阶通道中的STM-16的低阶通道手动使能并配置 操作如下图：

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/8%E7%88%B1%E5%A5%B9%E9%AB%98%E9%98%B6%E4%BD%BF%E8%83%BD.gif)

- 创建`E#1`与`SDH`的时隙交叉

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/8%E7%88%B1%E5%A5%B9%E4%BA%A4%E5%8F%89%E5%88%B0E1.gif)

------

# GCC

### GPN-76-8AT2 GCC网管开通

- 场景拓扑

<center><img src="https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/GCC%20%E6%8B%93%E6%89%91.png" style="zoom:50%;" /></center>

- 开通需求

  要求GPN7600设备下挂的其他GPN7600设备可以在网管中心发现。

- 可行性分析

  满足，通过8AT2&ensp;GCC功能发现远端。

### 开通配置

A站点&emsp;GPN7600_8AT2业务配置:

1.&ensp;板卡安装

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/8%E7%88%B1%E5%A5%B9%E6%9D%BF%E5%8D%A1%E5%AE%89%E8%A3%85.gif)

2.&ensp;配置GCC接口PPP协议

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/8%E7%88%B1%E5%A5%B9%E6%9D%BF%E5%8D%A1ppp%E5%8D%8F%E8%AE%AE%E4%BD%BF%E8%83%BD.gif)

3.&ensp;配置GCC开销，默认GCC2

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/8%E7%88%B1%E5%A5%B9%E6%9D%BF%E5%8D%A1GCC2.gif)

4.&ensp;配置OSPF

我司设备默认会有一个133 网管的环回地址，我们需要将U/D接口的网段宣告到ospf域：

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/8%E7%88%B1%E5%A5%B9%E6%9D%BF%E5%8D%A1%E5%AE%A3%E5%91%8A%E8%B7%AF%E7%94%B1.gif)

5.&ensp;网管serve配置

网管电脑需要配置一条静态路由，如下

```
route add 133.0.0.0 mask 255.0.0.0 192.168.33.241 -p
route add 192.168.33.0 mask 255.255.255.0 133.72.233.18 -p 
```

> <font face="仿宋">133网段是我司GPN设备的默认环回地址，用于GCC网管。</font><br>
>
> <font face="仿宋">192.168.33.241为U/D口地址。</font><br>
>
> <font face="仿宋">需要管理B站点时，网管电脑要新添一条通往B站点的路由！！！</font><br>

B站点&emsp;GPN7600_8AT2业务配置:

1.&ensp;板卡安装（同上）

2.&ensp;配置GCC接口PPP协议（同上）

3.&ensp;配置GCC开销，默认GCC2（同上）

4.&ensp;配置OSPF（同上）

# 业务删除

- 删除OSPF
- 删除网管Server静态路由
- 卸载板卡（注意：板卡有业务需删除业务后再执行卸载）
