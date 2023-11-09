# 波分排故

波分排故实质是光经过光学器件后的质量优劣性，所以排查涉及光收、损耗、色散等各种影响光的因素和参数。

OTN光层影响因素：非线性（功率过高）、衰耗、OSNR信噪比。

功率控制原理：光功足够强的前提下，不引起非线性效应。

### 光器件衰耗

 &emsp;AWG：`<=6dBm` 

&emsp;OLP:   TX损耗`<=4db`，RX损耗`<=1.5db`<font face="仿宋">(TX损耗指分光器衰减，RX损耗是光开关衰减)</font>

&emsp;OA ：“P”: 增益 `8-18dbm`，"Q"：增益`15-25dbm`，"R":增益`22-35dbm`

### 单波最佳入纤光功率<font face="仿宋" size=2>(工程经验值)</font>

&emsp;400G-16QAM：`3dBm`

&emsp;200G-16QAM：`1dBm`

&emsp;200G-QPSK：`2.5dBm`

### 调测<font face="仿宋" size=2>（仅记录多波光功率调测）</font>

<div align="center"><img src="https://s2.loli.net/2023/09/21/JlYmkqAoH6LQBrs.png"  style="zoom:85%;" /></div>

顾名思义，合波光功率就是单波功率相加得到的总功率，但整个链路光功单位为dBm，不能相加，故有如下计算功率：

<center>$$dBm=10\lg^\frac{Power}{1mW}$$</center>

<center>$$mW=10\lg^\frac{dBm}{10}$$</center>



单波：1--->2 ：0dbm-5dbm=-5<font color=red>db</font>    

&emsp;2--->3：-5dbm-2dbm=-7dbm 

&emsp;3--->4：-7dbm+10<font color=red>db</font>(增益补偿10db)=3dbm

&emsp;4--->5：3dbm-12<font color=red>db</font>(线路损耗)=-9dbm

&emsp;5--->6：-9dbm+12<font color=red>db</font>(端放补偿)=3dbm

&emsp;6--->7： 3dbm-5<font color=red>db</font>=-2dbm   <font color=white>----</font> 故，cfp收-2dbm

推广到多波：

<center>$${\text{多波}}={\text{单波}}+10\lg^n(n{\text{为波道数目}})$$</center>

### OLP保护板<font face="仿宋" size=3>（暂不支持双端倒换）</font>

> <font face="仿宋" >双向倒换需要使用APS信令通道进行协议报文交互，APS 信令通道可以使用ODUk开销中的APS/PCC字段来实现，也可以使用光监控通道，通过IP数据包的方式来实现。</font>

倒换方式：绝对倒换、相对倒换

返回方式：返回式、非返回式

等待恢复时间WTR：防止抖动引起的频繁倒换，业务切回工作信道之前需要保证一个延迟的考察时间

切换延时：主路倒换到备路

回切延时：备用回切到主用前需要等待的一个时间

自动模式延迟：从手动切换到自动时，自动模式功能生效的延时时间

```
am pri_switch_threshold 5 -18.00          ##主用光功率门限值
am sec_switch_threshold 5 -18.00          ##备用光功率门限值
```
