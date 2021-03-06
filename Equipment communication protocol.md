**ICS 35.020**

 

| **基础软件部** | **文档编号**     |      | **版本** | A1   | **密级** | 商密A |
| -------------- | ---------------- | ---- | -------- | ---- | -------- | ----- |
| **项目名称**   |                  |      |          |      |          |       |
| **项目来源**   | **公司内部开发** |      |          |      |          |       |











| **编**  **写：** | 梁森明 | **日** **期：** | 2020-10-14 |
| ---------------- | ------ | --------------- | ---------- |
| **检**  **查：** |        | **日** **期：** |            |
| **审**  **核：** |        | **日** **期：** |            |
| **批**  **准：** |        | **日** **期：** |            |

 

 



 

 

 

**湖南天宇智能科技有限公司**

 

**版权所有** **不得复制**

 

​                  **文档修改记录**

| **序号** | **版本** | 编制/修改人 | 修改日期   | 修改对象 | 备注（原因、进一步的说明等）   |
| -------- | -------- | ----------- | ---------- | -------- | ------------------------------ |
| 1        | 1.0.0    | 梁森明      | 2020-12-05 | 初稿     | 起稿                           |
| 2        | 1.0.1    | 梁森明      | 2020-12-10 |          | 初始版本                       |
| 3        | 1.0.2    | 梁森明      | 2020-12-11 |          | 修改控制码，校验码，新增随机码 |
|          |          |             |            |          |                                |
|          |          |             |            |          |                                |
|          |          |             |            |          |                                |
|          |          |             |            |          |                                |
|          |          |             |            |          |                                |
|          |          |             |            |          |                                |



 

# 1       1 范围

本部分规定了智慧网关与设备之间进行应用数据传输的帧格式、数据编码及传输规则。

本部分适用于总线（485、Lora等）通信方式，适用于智慧网关对设备执行交互式访问以及设备主动上报方式的通信。

本部分考虑了协议的通用性和扩展性，规定了设备的通用协议规则，如设置/查询指令、子设备等。

本部分统一了智慧网关与设备之间的统一协议格式，新开发的终端设备使用统一的协议与网关进行通信。

 

# 2       2 术语、定义、约定和缩略语

## 2.1  2.1 术语和名词定义

下列术语和定义适用于本部分。

2.1.1 终端地址 **terminal address**

系统中终端设备的地址编码，简称终端地址。

 

2.1.2 系统广播地址 **system broadcast address**

系统中所有终端都应该响应的地址编码。

 

2.1.3 终端组地址 **terminal group address**

具有某一相同属性的终端群组编码，如属于同一设备、同一线路，响应同一个命令。

 

2.1.4 **主站**

  相对于主从通信模式而言。通信的主机即主站，这里是指网关；

 

2.1.5 从站/**终端/节点**

  相对于主从通信模式而言。通信的从机即从站，这里是终端设备；

 

2.1.6 **发起帧****/****应答帧**

一次服务请求中，发起者发送的信息帧叫发起帧，收到这次请求的对象，做出响应而发送的帧叫应答帧，应答帧也叫响应帧，显然发送这两种帧的对象有可能是同一个。

 

**2.1.7** **端点/通道** **endpoint/channel**

指可以设置或者查询参数的最小可控点。每个点具有唯一的逻辑定位编码，是该装置在终端的参数配置、数据应用的唯一对象标识。如一个路灯控制器上具有两路路灯控制输出。

 

2**.1.10** **请求报文**

  指主站发起的一次服务请求信息，里面包括要求访问设备、提取信息的内容，或者进行确认的报文，由叫**发起报文**。

 

**2.1.11** **应答报文**

  指从站对请求源报文进行确认、反馈操作结果的报文。

 

2**.1.12** **发起方**

  主动发起一次服务请求的对象，这里指主站（路灯网关、社区网关、设备（主动上报时）等）

 

2**.1.13** **目标方**

  一次服务请求的接收对象或者目标对象，这里包括但不仅限于网关、设备。 

 

## 2.2  2.2 约定

​     对全局性的数据及其格式、参数取值进行预定，特殊说明的除外。

 

### 2.2.1       2.2.1 数据及其格式约定 

**2.2.1.1** **完整****包的长度**

一个包的字节数需少于256字节

 

**2.2.1.5** 通信链路

​     通信链路可以是485，Lora等。485对应的通信速率使用57600

### 2.2.2       2.2.2 参数取值约定 

本文档如没有特殊说明，参数表示的是大于等于0的整数。

 

**2.2.2.1** **环境温度**

环境温度精确到小数点两位，最大精确到0.01摄氏度，实际通信时放大100倍，发起方乘以

100，接收方除以100。

 

 

**2.2.2.2** **时间格式**

typedef struct{

  uint8_t   year;        // 年

  uint8_t   month;       // 月

  uint8_t   day;        // 日

  uint8_t   week;        // 星期

  uint8_t   hour;        // 时

  uint8_t   min;        // 分

  uint8_t   sec;        // 秒

   uint8_t   ms;         // 毫秒

}comm_dtime_t;



 

## 2.3   2.3 符号和缩略语

本部分中所使用到的符号和缩略语见表1。

表1 符号和缩略语

 

|      |      |
| ---- | ---- |
|      |      |
|      |      |

 

 

 

## 2.4   2.4 协议整体约定

本文档如没有特殊说明，都用十六进制表示数据，安全访问、验证信息、数据完整性、传输可靠性不属于本协议的讨论内容。

 

**2.4.1** **格式**

本文档采用hex格式进行应用数据之间的交互，并且在内容中用“//”标识的都是注释

说明。

# 3.  数据链路层

本协议为主-从结构的半双工通信方式。智慧路灯网关或其它网关为主站，多功能设备节点为从站。每个多功能设备节点均有各自的地址编码。通信链路的建立与解除均由主站发出的信息帧来控制。每帧由帧起始符、从站地址域、控制码、数据域长度、数据域、帧信息纵向校验码及帧结束符7个域组成。每部分由若干字节组成。

## 2.5 3.1 字节格式

每字节含8位二进制码，传输时加上一个起始位(0)、一个偶校验位和一个停止位(1)， 共 11位。其传输序列如图1所示。D0 是字节的最低有效位，D7 是字节的最高有效位。先传低位，后传高位。

 

![img](file:///C:/Users/ADMINI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image002.gif)

图1　**字节传输序列**

## 2.6 3.2 帧格式

帧是传送信息的基本单元。帧格式如图 2 所示。

| 说  明     | 代 码 |
| ---------- | ----- |
| 帧起始符   | 68H   |
| 地址域     | A0    |
| A1         |       |
| A2         |       |
| A3         |       |
| A4         |       |
| A5         |       |
| 帧起始符   | 68H   |
| 控制码     | C     |
| 数据域长度 | L     |
| 数据域     | DATA  |
| 随机码     | PRN   |
| 校验码     | CS    |
| 结束符     | 16H   |

图2　**帧格式**

### 2.6.1       3.2.1 帧起始符 68H

标识一帧信息的开始，其值为 68H=01101000B。

### 2.6.2       3.2.2 地址域 A0～A5

地址域由 6 个字节构成，每字节 2 位 BCD 码，地址长度可达12位十进制数。每个设备具有唯一的通信地址，且与物理层信道无关。当使用的地址码长度不足 6 字节时，高位用“0”补足。

通信地址999999999999H为广播地址，只针对特殊命令有效，如广播校时等。广播命令不要求从站应答。

地址域支持缩位寻址，即从若干低位起，剩余高位补AAH作为通配符进行设备操作，从站应答帧的地址域返回实际通信地址。

地址域传输时低字节在前，高字节在后。

高2字节 A5A4 代表产品类型

0001 网关；

 

-----------100系列社区类

010X 车位锁；

011X 车闸；

012X 门禁；

 

-------------200系列灯杆类

0200 路灯；

0210 摄像头；

0220 环境检测；

0230 充电桩

0240 一键报警

 

中间预留；

---------------传感器

0901 温度传感器；

0911 人体传感器；

0921 湿度传感器；

0931 烟雾传感器；

0990 摄像头

### 2.6.3       3.2.3 控制码 C

控制码的格式如下所示。节点和实时数据用

![img](file:///C:/Users/ADMINI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image004.gif) 

 

### 2.6.4       3.2.4 数据域长度*L*

*L* 为数据域的字节数。读数据时 *L*≤256-13，写数据时 *L*≤256-13，*L*=0 表示无数据域。

### 2.6.5       3.2.5 数据域 DATA

数据域包括数据标识（4byte）、数据、帧序号（1byte）等，其结构随控制码的功能而改变。传输时发送方按字节进行加33H处理，接收方按字节进行减33H处理。

### 2.6.6       3.2.6 校验码 CS

使用CRC16校验码，校验范围是从第一个帧起始符开始到校验码之前的所有各字节。算法如下：

\#define CRC_POLY 0xA001

unsigned int cal_crc(unsigned char *ptr, unsigned int len) {

  unsigned int crc=0xffff;

  unsigned char i=0;

  while(len--){

​    crc^=*ptr++;

​    for(i=0;i<8;i++){

​      if (crc&0x0001){

​        crc >>= 1;

​        crc^= CRC_POLY; 

​      }

​      else {

​        crc >>= 1;

​      }

​    }

  } 

  return crc;

} 

 

### 2.6.7       3.2.7 随机码

  随机码长度为1个字节，由发起端生成，用于数据加密。

### 2.6.8       3.2.8 结束符 16H

标识一帧信息的结束，其值为 16H=00010110B。

 

## 2.7 3.3 传输

### 2.7.1       3.3.1 前导字节

无，有需要可以增加。如低功耗节点非通信时工作在睡眠模式，需要唤醒才能进行正常通信，此时可定义先发送4个字节FEH，以唤醒接收方。

### 2.7.2       3.3.2 传输次序

所有数据项均先传送低位字节，后传送高位字节。

### 2.7.3       3.3.3 传输响应

每次通信都是由主站向按信息帧地址域选择的从站发出请求命令帧开始，被请求的从站接收到命令后作出响应。

收到命令帧后的响应延时 *T*d：20ms≤*T*d≤50ms。（特殊指令除外）

字节之间停顿时间    *T*b：*T*b≤5ms

### 2.7.4       3.3.4 差错控制

![img](file:///C:/Users/ADMINI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image006.gif)![img](file:///C:/Users/ADMINI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image008.gif)

 

图3　 传输次序图

 

 

字节校验为偶校验，帧校验为纵向信息校验和，接收方无论检测到偶校验出错或纵向信息校验和出错，均放弃该信息帧，不予响应。

### 2.7.5       3.3.5 通信速率

标准速率：600bps，1200bps，2400bps，4800bps，9600bps，19200bps，57600bps。

特殊速率：不支持

默认速率：57600bps

通信速率的变更，首先由主站向从站发变更速率请求，从站发确认应答帧或否认应答帧。收到从站确认帧后，双方以确认的新速率进行以后的通信，并在通信结束后保持更改速率不变。

# 4.  数据标识

## 2.8 4.1 数据标识结构

数据标识编码用四个字节区分不同数据项，四字节分别用DI3、DI2、DI1和DI0代表，每字节采用十六进制编码。数据标识分为不同的类型：。数据标识具体定义见附录数据标识编码。

 

| DI3  | DI2  | DI1  | DI0  |
| ---- | ---- | ---- | ---- |
|      |      |      |      |

## 2.9   4.2 数据传输形式

数据标识码标识单个数据项或数据项集合。单个数据项可以用附录A中对应数据项的标识码唯一地标识。当请求访问由若干数据项组成的数据集合时，可使用数据块标识码。实际应用以数据标识编码表定义内容为准。

### 2.9.1       4.2.1 数据项、数据块

#### 2.9.1.1       4.2.1.1 数据项

除特殊说明的数据项以ASCII码表示外，其它数据项均采用压缩BCD码表示。

#### 2.9.1.2       4.2.1.2 数据块

数据标识DI2 、DI1 、DI0中任意一字节取值为FFH时（其中DI3不存在FFH的情况），代表该字节定义的所有数据项与其它三字节组成的数据块。

### 2.9.2       4.2.2 举例

# 5.  应用层

## 2.10     5.1 读实时数据

### 2.10.1       5.1.1 主站请求帧

a) 功能：请求读数据；

b) 控制码：C=11H

c) 数据域长度：L=04H

d) 帧格式1（m=0）：

地址 是主控终端地址时读取主控终端的数据，地址是下面节点设备时，读取的是节点设备的数据

![img](file:///C:/Users/ADMINI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image010.gif)

![img](file:///C:/Users/ADMINI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image012.jpg)

​    A0….A5: 要读取节点设备的地址。

### 2.10.2       5.1.2 从站正常应答

a) 控制码：C=91H 无后续数据帧； 

b) 数据域长度：L=04H+m（数据长度）

c) 无后续数据帧格式：

 

![img](file:///C:/Users/ADMINI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image014.jpg)

### 2.10.3       5.1.3 从站异常应答帧

a) 控制码：C=D1H

b) 数据域长度:L=01H

c) 帧格式：

![img](file:///C:/Users/ADMINI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image016.gif)

注： 错误信息字ERR见附录C。

  

## 2.11     5.2 写设备参数

### 2.11.1       5.2.1 主站请求帧

a) 功能：主站向从站请求设置数据

b) 控制码：C=12H

c) 数据域长度：L =04H+m(数据长度)

d) 数据域：DIODI1DI2DI3+P0P1P2+DATA

e) 帧格式：

  68h a0…a5 68h 14h L DI0…DI3 N1…Nm PRN CS 16H

​            密码    用户名

### 2.11.2       5.2.2 从站应答帧

a) 控制码：C=92H

b) 数据域长度：L=01H

c) 帧格式：

![img](file:///C:/Users/ADMINI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image016.gif)

注： 错误信息字ERR见附录C。

 

## 2.12     5.3 读通信地址

### 2.12.1       5.3.1 主站请求帧

a)  功能：读取某从站的通信地址，仅支持点对点通信。

b)  控制码：C=11H

c)  地址域：AA…AAH,使用通配符AA地址

d)  数据域长度：L=00H

e)  数据域：无 

f)  帧格式：

68H AA AA AA AA AA AA 68H 13H 00 CS 16

 

### 2.12.2       5.3.2 从站正常应答帧

a)  控制码：C=91H

b)  地址域：A0…A5,从站通信地址

c)  数据域长度：L=06H

d)  帧格式：

68  A0 A1 A2 A3 A4 A5 68 93 06 A0 A1 A2 A3 A4 A5 CS 16

 

## 2.13     5.4 写通信地址

### 2.13.1       5.4.1 主站请求帧

a)  功能：设置某从站的通信地址，仅支持点对点通信。

b)  控制码：C=12H

c)  地址域：AA AA AA AA AA AAH, 通配地址

d)  数据域长度：L=0CH

e)  数据域：

A0…A5 从站原地址

B0…B5 从站新地址

f)  帧格式：

68H AA AA AA AA AA AA 68H 15H 0CH A0 … A5 B0 … B5 CS 16

 

 注：不支持广播写地址

 

### 2.13.2       5.4.2 从站应答帧

a)  控制码：C=92H

b)  地址域：A0…A5（从站通信地址）

c)  数据域长度：L=00H

d)  帧格式：

68 A0 … A5 68 95 01 ERR CS 16

 

ERR=0：A0…A5对应从站新通信地址。ERR=错误码，A0…A5对应从站原地址。

 

## 2.14     5.5 组播/广播控制命令

### 2.14.1       5.5.1主站请求帧

a)  控制码：C=12H

b)  地址：999999999999

c)  数据域长度：L=01H

d)  帧格式：

69 99 99 99 99 99 99 68 10 01 xx CS 16

注：xx代表组号，取值范围：0~255。0：代表全网广播，1~255：代表具体组号

 

### 2.14.2       5.5.2 从站应答帧

  从站无需应答

 

## 2.15     5.6 心跳

a)  控制码：C=93H

b)  地址：心跳设备地址

c)  数据域长度：未定

d)  帧格式：

 

 

## 2.16     5.7 主动上报/故障告警

主动上报需要判断设备内还有没有后续帧，主站收到之后根据该标识做出不同的行为，如判断后后续帧，那么主站先不下发命令。

 



# 3       附录（数据标识）

## 3.1 A 数据标识概述

### 3.1.1       A.1 

| **序号**           | **数据标识** | **数据项名称**    |
| ------------------ | ------------ | ----------------- |
| **节点设备参数项** |              |                   |
| 1.                 | 04F10000     | 时钟              |
| 2.                 | 04F10001     | 设备参数          |
| 3.                 | 04F10002     | 软件版本          |
| 4.                 | 04F10003     | 主动上报参数      |
| 5.                 | 04F10004     | 节点分组编号      |
| 6.                 | 04F10005     | 是否执行广播      |
| 7.                 | 04F10006     | 用户密码          |
| 8.                 | 04F10007     | 定时参数设置      |
| 9.                 | 04F10008     | 总开关            |
| 10.                | 04F1000A     | 调光              |
| 11.                | 04F1000B     |                   |
| **电量数据**       |              |                   |
| 12.                | 04F20001     | 实时数据          |
| 13.                | 04F20002     | 当日用电数据      |
| 14.                | 04F20003     | （上1日）用电数据 |
| 15.                | 04F20004     | （上2日）用电数据 |
| 16.                | 04F20005     | （上3日）用电数据 |
| **主动上报**       |              |                   |
| 17.                | 04F30001     | 电压异常          |
| 18.                | 04F30002     | 电流异常          |
| 19.                | 04F30003     | 设备掉线          |
| 20.                | 04F30004     | 通信异常          |

 

 

## 3.2 B表：数据标识详解

### 3.2.1       B.1

| **数据标识** | **数据格式** | **数据长度（字节）** | **单位** | **功能**     | **数据项名称** |      |      |      |      |
| ------------ | ------------ | -------------------- | -------- | ------------ | -------------- | ---- | ---- | ---- | ---- |
| DI3          | DI2          | DI1                  | DI0      | **读**       | **写**         |      |      |      |      |
| 04           | F1           | 00                   | 00       | yymmddhhmmss | 6              | BCD  | *    | *    |      |
| 04           | F1           | 00                   | 01       |              |                |      |      |      |      |
| 04           | F1           | 00                   | 02       | NNNNNNNN     | 4              | BIN  | *    |      |      |
| 04           | F1           | 00                   | 03       |              |                |      |      |      |      |
| 04           | F1           | 00                   | 04       |              |                |      |      |      |      |
| 04           | F1           | 00                   |          |              |                |      |      |      |      |
| 04           | F1           | 00                   |          |              |                |      |      |      |      |

 

## 3.3 C表：其他

### 3.3.1       C.1 错误码

| 序号 | 错误码 | 说明     | 备注                |
| ---- | ------ | -------- | ------------------- |
| 1    | 0      | 成功     |                     |
| 2    | -1     | 失败     |                     |
| 3    | -2     | 设备忙   |                     |
| 4    | -3     | 超时     |                     |
| 5    | -4     | 索引错误 | 查询的数据不存在    |
| 6    | -5     | 存储错误 | 存储数据到flash失败 |
| 7    | -6     | 解析错误 | 协议解析错误        |

 