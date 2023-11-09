# 改制

###### ***`EOPC126`改制为`EOSC126`**

```
GPN7600(config)#grosadvdebug
GPN7600(DEBUG_H)> board-eeprom 13
  date      Specify manufacture date 
  ext-info  Extended Information 
  sn        Specify board serial number 
  type      Specify board type 
  version   Specify board hardware version 
  <cr>      Just press enter to execute command!

GPN7600(DEBUG_H)> board-eeprom 13 type eosc126   
reboot 13    //复位对应槽位生效
```

***`8AT2`改制为`**`***

```
ext-info2针对8AT2板卡         type针对8ge板卡
```

```linux
grosadvdebug
> board-eeprom <slot> ext-info2 mode=<date>           board-eeprom <slot> type  <name>
**mode=0,   8AT2**      //加载760b.fpga//                **eos-8ge, <name>=8ge-32 ** 
**mode=1,   4XT2**
**mode=2,   4GT1**
**mode=3,   8AST2** 
**mode=4,   2XT2** 
**mode=5,   2GT1** 
**mode=6，  T10X**
**mode=8,   8AT2SDH**    //16个vp_SDH//
**mode=27,  8AT2-B**     //8AT2-B板卡因为缺少芯片，不能改制为8AT2板卡// //8AT2加载760b.fpga// //V2//
V3版本：
**mode=20， OODX(8AT2/8AT2-B)**
**mode=6，  T10X**
```



# 更改

***更改`NEID`和系统`MAC`***

```
grosadvdebug 
>show netid 
>config netid 0x[16进制的数据] 
>config sysmac 0000.1111.2222           **一般不随便更改设备MAC地址** 
reboot重启生
```

***更改设备`Loopback10`地址***

```linux
插入8at2设备会有一个Loopback10的环回地址，133网段

第一种方法：
GPN7600(config)# interface  loopback 10
GPN7600(config)#ip address xxx.xxx.xxx.xxx/24
第二种方法：
GPN7600(config)# dcn ip xxx.xxx.xxx.xxx/24        // 掩码按照规划 //
```

[^1]: 优选第二种方式，原因可能影响ospf  id号



# 启、闭

***启、闭U/D口物理状态***

```
GPN7600(config)#int eth 17/<1-2>                
GPN7600(if-eth17/3)#shutdown                ## 关闭端口
GPN7600(if-eth17/3)#undo shutdown           ## 开启端口 

GPN7600(if-eth17/3)#int eth 17/<3-10>       ## <3-10>对应E1/1-8
GPN7600(if-eth17/3)#shutdown
GPN7600(if-eth17/3)#undo shutdown
```

***启、闭8GE物理口状态***

```
GPN7600(config)#int eth 1/1 
GPN7600(if-eth1/1)#shutdown                       ## 进入端口，shutdown
或者
GPN7600(config-msap)#ioctl eth disable 1/1        ## msap节点下，关闭。任选其一！
```

***启、闭激光器状态***

```linux
端口下port laser off/on                             ## shutdown仅关闭端口，未关闭激光器
```

# 主登录备

***设备底层，主用主控登陆备用主控方法***

```
config# grosadvdebug                           进入debug节点 
debug > switch 2                               进入2槽的备用主控 
slot2 > grosadvdebug                              
debug> slave-config enable                     启用cofig节点 
debug> exit 
slot2> enable 
slot2(config)#
```

# ***团体字   `查询` `更改`***

```
GPN7600(config)# show snmp community-string        //## 查询团体字
  Read-only Community String is :[public] 
  Read-write Community String is :[private]
------------------------------------------------------------------------------
config snmp community readonly gwtt@123       //这个是读团体    //##修改团体字 
config snmp community readwrite gwtt@123      //这个是写团体
```

[^2]: **修改snmp需对应修改‘网管’和‘底层’两处，否则上载不生效**

# V2升级脚本

```
GPN7600升级步骤

第一步：保存
GPN7600(config)#save

第二步：备份
upload ftp config 192.168.9.99 gwglwe2022 GWadmin@123  config.txt                                 
upload ftp file /flash/sys/db.bin 192.168.9.99 gwglwe2022 GWadmin@123  db.bin                     
upload ftp file /flash/sys/slotconfig.bin 192.168.9.99 76 213546 slotconfig.bin      

第三步：删除主用主控文件，释放空间
GPN7600(config)#dosfs
GPN7600(host:)#cd /flash/sys
GPN7600(/flash/sys)#rm app_code_backup.bin
GPN7600(/flash/sys)#rm app_code.bin
GPN7600(/flash/sys)#ll 					//L是小写字母，查询剩余空间，R13B240SP15或者SP16要求大于15M 方可以升级！

第四步：删除备用主控文件，释放空间			//没有备用主控，请忽略此步骤
GPN7600(config)#grosadvdebug
GPN7600(DEBUG_H)>switch 18
Slot18>grosadvdebug
Slot18(DEBUG_H)> slave-config en
Slot18(DEBUG_H)> slave-config enable
Slot18(DEBUG_H)> exit
Slot18>enable
Slot18(config)#dosfs
Slot18(host:)#cd /flash/sys
Slot18(/flash/sys)#rm app_code_backup.bin
Slot18(/flash/sys)#rm app_code.bin
Slot18(/flash/sys)#ll
删除完成以后，使用 exit 命令多退出几次就到原始config节点了，然后继续下一步。

第五步：下载版本，开始升级

download ftp app 192.168.11.12 gwglwe2022 GWadmin@123 GPN7600_V02R19C17B013.bin gpn                          
download ftp file /yaffs/sys/760s.fpga 192.168.11.12 gwglwe2022 GWadmin@123  otn_a_760s_20201102.fpga                8AT2板卡 

download ftp file /yaffs/sys/760b.fpga 192.168.11.12 gwglwe2022 GWadmin@123  otn_a_760b_20211129.fpga                8AT2板卡


reboot   

download ftp file /yaffs/sys/760c.fpga 192.168.11.12 gwglwe2022 GWadmin@123 otn_a_760c_20200414.fpga                2XT2板卡

download ftp file /yaffs/sys/760e.fpga 192.168.11.12 gwglwe2022 GWadmin@123  otn_a_760e_20200922.fpga               T10X板卡


下载FPGA大包
download ftp fpga 192.168.11.12 gwglwe2022 GWadmin@123  fpga_code_1058_SP15.bin other                        

下载NMS文件
download ftp fpga 192.168.11.12 root passw  GPN7600-NMS-V1_FPGA_v1.1.14.fpga master             

下载SW文件
download ftp fpga 192.168.11.12 gwglwe2022 GWadmin@123  GPN7600_SW_A_PTN_oam_v0323_20190411.fpga sw              

下载SYSfile文件
download ftp sysfile 192.168.11.12 gwglwe2022 GWadmin@123 sysfile_gwd_V02R02B043_GPN7600_OTN_V2R3.bin        

第六步：重启设备
GPN7600(config)#reboot

第七步：检查是否下载成功

GPN7600(config)#show version				//检查APP版本是否下载陈工
ProductOS Version V02R18C02B020 (Build on 13:27:19 Aug 30 2018)

GPN7600(config)#grosadvdebug
GPN7600(DEBUG_H)>show fpga				//检查FPGA版本
  fpga_code.bin: 1.0.5.4				//大版本
    sp version: SP7					//小版本


电路平面升级，没有特别注意事项，基本操作升级即可！
```

