# 排故思路

`打环` `打环` `再打环`         `简单`--->`复杂`



##### 前奏   

开局简单<u>核查</u>命令：

- 版本对不对

```
EasyPath Series GPN Switch Software 
GROS Version V02R19C17B013(Build on 08:59:37 Dec 24 2021) 
ProductOS Version V02R19C17B013 (Build on 08:59:37 Dec 24 2021) 
Copyright (c) GW Technologies Co.,Ltd. All Rights Reserved 
Product GPN7600-OTN, Running on EasyPath GPN Hardware 
Sysmac : 000F.E930.38D0  
SDN file version: otn-cucc0726 
          BootVersion      SoftwareVersion      FpgaVersion         CpldVersion 
SLOT  1 :                                       0.0.4               V1.01 
SLOT  2 : -                -                    -                   - 
SLOT  3 : -                -                    -                   - 
SLOT  4 : -                -                    -                   - 
SLOT  5 :                                       1.0.2               V1.02 
SLOT  6 :                                       1.0.2               V1.02 
SLOT  7 :                                       1.0.16              V1.05 
```

[^1]: 核查版本特性支持功能，确认无误后继续...

- FPGA 对不对

```
 GPN7600（config）# show  fpga-version
 Fpga version: 
  Software version: 
  nms.fpga: 1.1.14 
  fpga_code.bin: 1.0.5.8 
    include: 
        4ge.fpga: 1.0.5 
        4ge_b.fpga: 0.0.6 
        gft_8fe.fpga: 1.0.3 
        eopc126_se0166.fpga: 4.0.5 
        etu_e1.fpga: 1.0.5 
        olp.fpga: 1.0.2 
        otu2g5.fpga: 1.1.3 
        omu_cxt4.fpga: 0.0.21 
        2xg.fpga: 1.0.7 
        pdh.fpga: 1.0.7 
        stm1s.fpga: 1.0.16 
        stm1c_4.fpga: 0.0.A 
        cstm1_4.fpga: 1.0.5 
        ces_e1.fpga: 0.0.20 
        gft_8fx.fpga: 1.1.B 
        eopc126_gw.fpga: 1.0.8 
        eos126_se0166.fpga: 1.0.0 
        omu_gw.fpga: 1.0.3                     
        ces_v35_e1.fpga: 1.0.1 
        eos_8fe.fpga: 1.0.7 
        eos_8fx.fpga: 1.1.F 
        2xg_bcm.fpga: 1.0.4 
        eop_8fx.fpga: 1.0.9 
        ee_4.fpga: 0.0.7 
        stm4s.fpga: 0.0.4 
        8ge.fpga: 0.0.4 
        edfa.fpga: 0.0.1 
        otu10gp.fpga: 0.0.5 
        otu25g.fpga: 0.0.1 
        dmd-8ge-e.fpga: 0.0.0 
        eosc-8ge.fpga: 0.0.1 
        omu2g5.fpga: 1.0.2 
  otn Fpga version: 
  otn_a :  
  otn_b : 0.0.1 
  otn_c : 0.0.1 
  otn_d :  
  otn_e : 0.0.2 
  otn_f :  
  otn_s : 11 
  7615 : 1                                     
  Fpga register version: 
  GPN7600-V2-DMD-8GE: 0x0300 
  GPN7600-V2-OTN-8AT2[13]: 760B/0x0003/2021-11-29 
  GPN7600-V2-OTN-8AT2[14]: 760B/0x0003/2021-11-2
```

- 告警有什么  

```
GPN7600(config)#show alarm log                ## 所有告警
GPN7600(config)#show alarm log today/yestoday             ## 今天/昨天告警
GPN7600(config)#show alarm log start-time 2022-11-8,00:00:00           ## 按起始时间查询告警
```

```
GPN7600（config）# show current  alarm                   ### 当前告警 ###
[MINOR]2022-11-09,09:40:44 GPN7600 if-sdh5/1 sdh MS-REI alarm 
[MAJOR]2022-11-09,09:15:35 GPN7600 if-gcc 14/2 gcc link down 
[MAJOR]2022-11-09,09:15:35 GPN7600 if-gcc 14/1 gcc link down 
[VITAL]2022-11-09,09:15:32 GPN7600 if-otn otn14/1 Otn Port Loss Of Signal alarm 
[VITAL]2022-11-09,09:15:32 GPN7600 if-vpsdh14/4 sdh RS-LOF alarm 
[VITAL]2022-11-09,09:15:32 GPN7600 if-vpsdh14/2 sdh RS-LOF alarm 
[VITAL]2022-11-09,09:15:32 GPN7600 if-otn otn14/2 Otn Port Loss Of Signal alarm 
[VITAL]2022-11-09,09:15:29 GPN7600 if-vpsdh14/3 sdh RS-LOF alarm 
[VITAL]2022-11-09,09:15:29 GPN7600 if-vpsdh14/1 sdh RS-LOF alarm 
[VITAL]2022-11-09,09:14:41 GPN7600 if-any any13/1 Any Port Loss Of Signal alarm 
```

- 设备资源被占用情况

```
GPN7600（config）#   show system  resource
  System used memory 149506288 bytes. 
  System free memory 101770528 bytes. 
  Total system memory 251276816 bytes. 
  The percentage of Used memory: 59.4%. 
  User used memory 15574684 bytes. 
  User free memory 115497316 bytes.  
  Total user memory 131072000 bytes. 
  The percentage of Used memory: 11.8%. 
  The percentage of CPU: 21.7178%.

GPN7600（config）#   show system  resource  usage                ## 查看资源使用百分比
 slot    cpu-usage     sys-mem     user-mem 
  11        10.1%        39.4%        6.8% 
  17         3.3%        59.4%       11.8%              ## sys-mem  最高80% 超过90%崩框
```

##### 进阶

针对具体业务查询，业务类型有：`EOS业务`    `EOO业务` ，方式从上联口到下联口逐个节点向下查询

[^2]: 目前多数业务都采用SDH+OTN的方式，所以从这涵盖基本。

- 现在有条业务通过8AT2对接华为大网，排查我司设备是否问题，可以这样操作：

###### 节点1  OTU口有无告警

```
GPN7600(otn)#   ioctl otu show 13/1               ## 查看OTU接口信息
```

###### 节点2  oduk时隙有无告警

```
GPN7600(otn)#   ioctl odu1 show 13 otu  1/1              ## 查看OTU接口下odu1信息
```

###### 节点3  VP-SDH口有无告警

```
GPN7600(otn)#   ioctl odu1 show 13 sdh  1/1             ## 查看sdh接口下的odu1信息
```

###### 节点4  VP-SDH对应高阶有无告警

```
GPN7600(config-msap)#ioctl hpoh show 13/1/1 16       ## 查看高阶告警
```

###### 节点 5  VP-SDH对应低阶有无告警

```
GPN7600(config-msap)#ioctl lpoh show 13/1/1/1  63       ## 查看低阶告警
```

###### 节点6  VCG收发、扰码是否正常

```
GPN7600(config)#   interface vcg   1/1      
GPN7600(vcg-1/1)#   show current-state                      ## 查看vcg告警
```

###### 节点7  端口是否有up 是否有流量

```
GPN7600(config)#   config  msap      
GPN7600(config-msap)#   ioctl  vcg  info  1/1     ## 查看vcg控制字及端口双模模式
```



