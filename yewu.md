# 业务、保护

### 开局

{%youtube%}https://www.youtube.com/watch?v=SUxayu7auak&ab_channel=GWTT{%endyoutube%}##内配视频##

------

### EOS业务

**单向业务流：**

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/SDH%E4%B8%9A%E5%8A%A1.png)

**单向业务创建：**

一.&ensp;【虚级联接口管理】

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/8GE%E7%BB%91%E5%AE%9A%E9%80%9A%E9%81%93.gif)

二.&ensp;【OTN交叉连接管理】

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/8GE-OTN%E6%98%A0%E5%B0%84%E4%BA%A4%E5%8F%89.gif)

三.&ensp;【高阶通道】

VC4业务不需要使能，VC3、VC12业务需要使能低阶时隙。

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/8%E7%88%B1%E5%A5%B9%E9%AB%98%E9%98%B6%E4%BD%BF%E8%83%BD.gif)

四.&ensp;【SDH交叉连接管理】

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/8GE-SDH%E4%BA%A4%E5%8F%89.gif)

附：

{% youtube %} https://youtu.be/KKTGzT-28_w?si=sdSUYroRN1pOovR3 {% endyoutube %} ##内培视频##

------

### EOO（分组）业务

**单向业务流：**

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_16969022693298.png)

**业务创建：**

暂无资料。

------

### 业务汇聚场景

- ##### **EOS业务**


单板


![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_16969031957260.png)

跨板


![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_16969032948542.png)

- ##### **EOO业务**


![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/EOO%E6%B1%87%E8%81%9A.png)

{% youtube %} https://www.youtube.com/watch?v=PvtqdJq1YVc&ab_channel=GWTT  {% endyoutube %}##内培视频##

------

### EOPC126

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/8GE%EF%BC%8BEOP126%E4%B8%8A%E7%BD%91%E7%AE%A1.png)

------

### EOSC126

暂无资料。

------

### EOS-8FXFE

暂无资料。

------

# 保护

### <span id="jump"></span>端口保护

- **EOS/SDH、EOO端口保护**

板内


![image.png](https://s2.loli.net/2023/09/27/fiAQb1wDdWka3Kp.png)

一、【端口双模管理】

板内端口保护要求主用和备用端口模式非NOOP，且主备端口模式一样，即同为VLAN或PORT，备用接口必须为“未绑定”。

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/EOS%E7%AB%AF%E5%8F%A3%E4%BF%9D%E6%8A%A4.gif)

二、【端口保护】

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/EOS%E7%AB%AF%E5%8F%A3%E4%BF%9D%E6%8A%A400.gif)

板间


![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/%E8%B7%A8%E7%89%88.png)

一、【端口双模管理】

板间端口保护要求工作、保护端口模式一样，工作端口VCG创建SDH交叉，备用端口仅创建VCG，无须交叉。

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/%E8%B7%A8%E7%89%88%E4%BF%9D%E6%8A%A4-.gif)

二、【虚级联接口管理】

工作、保护端口VCG都要绑定通道

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/%E8%B7%A8%E7%89%88%E4%BF%9D%E6%8A%A4-%E8%99%9A%E7%BA%A7%E8%81%94.gif)

三、【SDH交叉连接管理】

创建交叉时，工作端口需要做交叉且为源端口。备用无需交叉。

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/%E8%B7%A8%E7%89%88%E4%BF%9D%E6%8A%A4-SDH%E4%BA%A4%E5%8F%892.gif)

四、【端口保护】

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/%E8%B7%A8%E7%89%88%E4%BF%9D%E6%8A%A4-%E4%BF%9D%E6%8A%A4%E5%88%9B%E5%BB%BA.gif)

- **OTU(OTN/SDH)端口保护**

板内

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/OTN%E6%9D%BF%E5%8D%A1%E6%9D%BF%E5%86%85%E4%BF%9D%E6%8A%A4.gif)

板间

一、【OTN交叉连接管理】

工作端口需要创建ODUK和VP_SDH映射，无需交叉；保护端口不做任何操作。

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/OTN%E6%9D%BF%E5%8D%A1%E6%9D%BF%E9%97%B4%E4%BF%9D%E6%8A%A4.gif)

二、【OTN/SDH端口保护】

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/OTN%E6%9D%BF%E5%8D%A1%E7%AB%AF%E5%8F%A3%E4%BF%9D%E6%8A%A4%E5%88%9B%E5%BB%BA.gif)

### ODUK_SNCP

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/sncp.png)

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/OTN%E6%9D%BF%E5%8D%A1-SNCP%E4%BF%9D%E6%8A%A41.gif)

### VP_SDH

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_16969269275522.png)

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/OTN%E6%9D%BF%E5%8D%A1-vc-SDH%E4%BF%9D%E6%8A%A4.gif)

### VC_SNCP

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/%E8%B7%A8%E7%89%88.png)

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/OTN%E6%9D%BF%E5%8D%A1-vc-SNCP%E4%BF%9D%E6%8A%A41.gif)

### EOS下联

![](https://gitbook-pic-1301999062.cos.ap-beijing.myqcloud.com/%E4%B8%8B%E8%81%94.png)

参考[【端口保护】](#jump) 内容！！

{%youtube%}https://www.youtube.com/watch?v=8hAvxhcTDMg&ab_channel=GWTT{%endyoutube%}

