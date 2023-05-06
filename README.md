[英文](./README_EN.md) | 中文

[从Gitee访问](https://gitee.com/napoleon940911/Silicon_Labs_AT_on_ZigBee) | [从GitHub访问](https://github.com/Napoleon940911/Silicon_Labs_AT_on_ZigBee)

# 一、介绍

## 1.1 仓库简介

该仓库主要是基于 **EFR32MG21A020F768IM32** 芯片实现的AT命令固件，符合 **ZigBee 3.0** 标准，该仓库主要包含：

-  **固件** （公开版 + 授权版）；
-  **烧录工具** （含固件烧录说明）；
-  **AT命令说明文档** 等。

## 1.2 硬件说明

该仓库中提供的固件总共使用了 **EFR32MG21A020F768IM32** 芯片的两路串口，分别用于 **AT命令交互** 和 **调试/日志输出** ，具体引脚使用情况如下表所示：

| 串口功能      | TX  | RX  | 波特率 |
|---------------|-----|-----|--------|
| AT命令交互    | PB0 | PB1 | 115200 |
| 调试/日志输出 | PD0 | PD1 | 115200 |

# 二、通用AT命令

注：**发送命令需要加上回车和换行，对应两个字节十六进制数为0x0D、0x0A**，设置命令只需使用一次，**配置会存储到FLASH**，重新上电不需要重新配置，配置永久有效。部分AT命令只有在特定模式时有效，如果发送AT命令没有返回，请检查主从机模式。

若发送的**AT命令格式错误**，会返回错误类型，说明如下

- AT_ILLEGAL_FORMAT：指令格式有错
- AT_UNKNOWN_CMD：未知的指令，无法识别
- AT_LACK_PARA：指令缺少参数
- AT_INVALID_PARA：指令参数无效
- AT_TOO_SHORT：指令参数长度太短
- AT_TOO_LONG：指令参数长度太长
- AT_NO_NETWORK：当前设备未入网（或创建网络），必须在有网络的前提下才可以使用

## 2.1 AT+HELP

| <span style="display:inline-block;width:40px"> 功能</span> | 查看当前固件支持的命令&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; |
| :--------------------------------------------------------: | :----------------------------------------------------------- |
|                            发送                            | AT+HELP<br />                                                |
|                            返回                            | \<AT command name\>......\<AT explanation\><br /><br />OK<br /> |
|                            说明                            | \<AT command name\>：AT命令 <br />\<AT explanation\>：AT说明 |
|                            示例                            | 【发送】<br />AT+HELP<br /><br />【返回】<br />支持的所有命令<br /> |

## 2.2 AT

| <span style="display:inline-block;width:40px"> 功能</span> | <span style="display:inline-block;width:160px"> AT测试</span>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; |
| :--------------------------------------------------------: | ------------------------------------------------------------ |
|                            发送                            | AT<br />                                                     |
|                            返回                            | OK<br />                                                     |
|                            示例                            | 【发送】<br />AT<br /><br />【返回】<br />OK<br />           |

## 2.3 AT+INFO

| <span style="display:inline-block;width:40px"> 功能</span> | 获取当前设备信息&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; |
| :--------------------------------------------------------: | :----------------------------------------------------------- |
|                            发送                            | AT+INFO<br />                                                |
|                            返回                            | \<FIRMWARE_VERSION\><br />\<BUILD_TIME\> <br />\<MFG EUI64\> <br />\<R&W EUI64\><br />\<NODEID\> <br />\<TX_POWER\> <br />\<PARENT_ID\> <br />\<PARENT_RSSI\> <br />\<PAN_ID\> <br />\<CHANNEL\><br /><br />OK<br /> |
|                            说明                            | <FIRMWARE_VERSION>：固件版本 <br /><BUILD_TIME>：编译时间 <br />\<MFG EUI64\>：IEEE地址（不可修改）<br />\<R&W EUI64\>： 用户可自定义的IEEE地址（可读可写）<br />\<NODEID\>：设备自身的网络地址<br />\<TX_POWER\>：设备自身的射频发射功率 <br />\<PARENT_ID\>：父节点的网络地址<br />\<PARENT_RSSI\>：设备自身与父节点之间的信号强度指示 <br />\<PAN_ID\>：ZigBee网络的地址 <br />\<CHANNEL\>：所在信道<br /> |
|                            示例                            | 【发送】<br />AT+INFO<br /><br />【返回】<br/>FIRMWARE_VERSION: v2.1<br/>BUILD_TIME: 15:57:26, Sep 21 2022<br/>MFG EUI64: 0x94DEB8FFFE5A1AFE<br/>R&W EUI64: 0x94DEB8FFFE5A1AFE<br/>NODEID: 0x58C0<br/>TX_POWER:  -5 DBM<br/>PARENT_ID: 0x0000<br/>PARENT_RSSI:   0 DBM<br/>PAN_ID: 0x2C82<br/>CHANNEL: 15<br/><br/>OK<br /> |
|                            注意                            | 如果您在使用EFR32MG21-AT-OPEN固件上有任何问题，请首先提供`AT+INFO`版本信息. |

## 2.4 AT+ECHO

| <span style="display:inline-block;width:40px"> 功能</span> | 打开/关闭回显&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; |
| :--------------------------------------------------------: | :----------------------------------------------------------- |
|                            发送                            | AT+ECHO:\<value\><br />                                      |
|                            返回                            | OK<br />                                                     |
|                            说明                            | \<value\>:  0: 关闭回显 1: 打开回显                          |
|                            示例                            | 【发送】<br />AT+ECHO:1<br /><br />【返回】<br />OK<br />    |

## 2.5 AT+RESET

| <span style="display:inline-block;width:40px"> 功能</span> | 软件复位&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; |
| :--------------------------------------------------------: | :----------------------------------------------------------- |
|                            发送                            | AT+RESET<br />                                               |
|                            返回                            | OK<br />                                                     |

## 2.6 AT+FACTNEW

| <span style="display:inline-block;width:40px"> 功能</span> | 恢复出厂设置&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; |
| :--------------------------------------------------------: | :--------------------------------------------- |
|                            发送                            | AT+FACTNEW<br />                               |
|                            返回                            | OK<br />                                       |

## 2.7 AT+BLOAD

| <span style="display:inline-block;width:40px"> 功能</span> | 进入BootLoader模式&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;     |
| :--------------------------------------------------------: | :------------------------------------------------------- |
|                            发送                            | AT+BLOAD<br />                                           |
|                            返回                            | OK<br />                                                 |
|                            示例                            | 【发送】<br />AT+BLOAD<br /><br />【返回】<br />OK<br /> |
|                            注意                            | 返回OK后会直接进入BootLoader模式                         |

## 2.8 AT+FORMNET

| <span style="display:inline-block;width:40px"> 功能</span> | 创建网络【仅限ZigBee协调器使用】&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; |
| :--------------------------------------------------------: | :----------------------------------------------------------- |
|                            发送                            | AT+FORMNET<br />                                             |
|                            返回                            | OK<br /><br />NETWORK_UP<br />                               |
|                            示例                            | 【发送】<br />AT+FORMNET<br /><br />【返回】<br />OK<br /><br />NETWORK_UP<br /> |
|                            注意                            | 当创建网络成功后，PC0（LED1）会置高<br />可以使用`AT+INFO`查询创建的网络标号（PANID） |

## 2.9 AT+OPENNET

| <span style="display:inline-block;width:40px"> 功能</span> | 开放网络【仅限ZigBee协调器使用】&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; |
| :--------------------------------------------------------: | :----------------------------------------------------------- |
|                            发送                            | AT+OPENNET:\<seconds\><br />                                 |
|                            返回                            | OK<br /><br />NETWORK_OPENED<br /><br />NETWORK_CLOSED<br /> |
|                            说明                            | \<seconds\>：网络开放持续时间，范围(0~254 s)                 |
|                            示例                            | 【发送】<br />AT+OPENNET:30<br /><br />【返回】<br />OK<br /><br />NETWORK_OPENED<br /><br />【30s过后】<br /><br />NETWORK_CLOSED<br /> |
|                            注意                            | 需要先使用`AT+FORMNET`创建网络后才能够开放网络               |

## 2.10 AT+CLOSENET

## 2.11 AT+JOINNET

| <span style="display:inline-block;width:40px"> 功能</span> | 加入网络&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;                   |
| :--------------------------------------------------------: | :----------------------------------------------------------- |
|                            发送                            | AT+JOINNET<br />                                             |
|                            返回                            | 【待入网设备返回】<br />OK<br /><br />NETWORK_UP<br /><br />【协调器返回】<br />NETWORK_OPENED<br /><br />[\<srcAddr\>,\<datasize\>,\<LQI\>]ONLINE:\<EUI64\>,\<destAddr\><br /><br />NETWORK_CLOSED<br /> |
|                            说明                            | \<srcAddr\>：源地址<br />\<datasize\>：数据长度<br />\<LQI\>：链路质量（Link quality instruction）<br />\<EUI64\>：已入网设备的MFG EUI64地址 |
|                            注意                            | 需要协调器使用`AT+OPENNET`开放网络                           |

## 2.12 AT+CHANNEL

| <span style="display:inline-block;width:40px"> 功能</span> | 改变信道&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;                   |
| :--------------------------------------------------------: | :----------------------------------------------------------- |
|                            发送                            | AT+CHANNEL:\<channel\><br />                                 |
|                            返回                            | OK<br />                                                     |
|                            说明                            | \<channel\>：信道切换，范围(11~26)                           |
|                            示例                            | 【发送】<br />AT+CHANNEL:11<br /><br />【返回】<br />OK<br /> |
|                            注意                            | 可以使用`AT+INFO`查询当前的信道                              |

## 2.13 AT+TXPOWER

| <span style="display:inline-block;width:40px"> 功能</span> | 修改协调器或路由器发射功率&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; |
| :--------------------------------------------------------: | :----------------------------------------------------------- |
|                            发送                            | AT+TXPOWER:\<dstAddr\>,\<power\><br />                       |
|                            返回                            | OK<br /><br />NEW_TXPOWER:\<power\><br /><br />[\<srcAddr\>,\<datasize\>,\<LQI\>]RSP_TXPOWER_SET_OK<br /> |
|                            说明                            | \<dstAddr\>：目标地址，0xFFFF为广播，0xFFFE为自身，其余为单播<br />\<power\>：发射功率，范围(-8~20 dBm)<br />\<srcAddr\>：源地址<br />\<datasize\>：数据长度<br />\<LQI\>：链路质量（Link quality instruction） |
|                            示例                            | 【发送】<br />AT+TXPOWER:1234,10<br /><br />【返回】<br />OK<br /><br />NEW_TXPOWER:10<br /><br />[0000,18,255]RSP_TXPOWER_SET_OK<br /> |
|                            注意                            | 可以使用`AT+INFO`查询当前的信道                              |

## 2.14 AT+BAUDRATE

| <span style="display:inline-block;width:40px"> 功能</span> | 发送设置协调器或路由器波特率&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; |
| :--------------------------------------------------------: | :----------------------------------------------------------- |
|                            发送                            | AT+BAUDRATE:\<dstAddr\>,\<baudrate\><br />                   |
|                            返回                            | OK<br /><br />NEW_BAUDRATE:\<baudrate\><br /><br />[\<srcAddr\>,\<datasize\>,\<LQI\>]RSP_BAUDRATE_SET_OK<br /> |
|                            说明                            | \<dstAddr\>：目标地址，0xFFFF为广播，0xFFFE为自身，其余为单播<br />\<baudrate\>：波特率，范围(9600~115200)<br />\<srcAddr\>：源地址<br />\<datasize\>：数据长度<br />\<LQI\>：链路质量（Link quality instruction） |
|                            示例                            | 【发送】<br />AT+BAUDRATE:1234,115200<br /><br />【返回】<br />OK<br /><br />NEW_BAUDRATE:115200<br /><br />[1234,19,255]RSP_BAUDRATE_SET_OK<br /> |

## 2.15 AT+RONOFF

| <span style="display:inline-block;width:40px"> 功能</span> | 远程开关cluster&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;            |
| :--------------------------------------------------------: | :----------------------------------------------------------- |
|                            发送                            | AT+RONOFF:\<dstAddr\>,\<ep\>,\<on/off\><br />                |
|                            返回                            | OK<br />                                                     |
|                            说明                            | \<dstAddr\>：目标地址，0xFFFF为广播，0xFFFE无效，其余为单播 <br />\<ep\>：目标端点<br />\<on/off\>：1: 打开 0: 关闭 |
|                            示例                            | 【发送】<br />AT+RONOFF:1234,01,1<br /><br />【返回】<br />OK<br /> |

## 2.16 AT+RTOGGLE

| <span style="display:inline-block;width:40px"> 功能</span> | 远程翻转cluster&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;            |
| :--------------------------------------------------------: | :----------------------------------------------------------- |
|                            发送                            | AT+RTOGGLE:\<dstAddr\>,\<ep\><br />                          |
|                            返回                            | OK<br />                                                     |
|                            说明                            | \<dstAddr\>：目标地址，0xFFFF为广播，0xFFFE无效，其余为单播<br />\<ep\>：目标端点 |
|                            示例                            | 【发送】<br />AT+RTOGGLE:1234,01<br /><br />【返回】<br />OK<br /> |

## 2.17 AT+TSEND

| <span style="display:inline-block;width:40px"> 功能</span> | 透传&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;                    |
| :--------------------------------------------------------: | :----------------------------------------------------------- |
|                            发送                            | AT+TSEND:\<dstAddr\>,\<data\><br />                          |
|                            返回                            | OK<br /><br />[\<destAddr\>,\<datasize\>,\<LQI\>]RSP_TSEND_OK<br /> |
|                            说明                            | \<dstAddr\>：目标地址，0xFFFF为广播，0xFFFE无效，其余为单播<br />\<data\>：透传数据，长度范围(3~80 bytes)<br />\<srcAddr\>：源地址<br />\<datasize\>：数据长度<br />\<LQI\>：链路质量（Link quality instruction） |
|                            示例                            | 【发送】<br />AT+TSEND:A018,abcd<br /><br />【返回】<br />OK<br /><br />[A018,12,255]RSP_TSEND_OK |

# 三、扩展AT命令（仅授权硬件支持）

## 3.1 AT+DEVLIST

## 3.2 AT+HEARTBEAT

## 3.3 AT+ULIMG

## 3.4 AT+FWQR

## 3.5 AT+OTANTF

## 3.6 AT+TCINFO

| <span style="display:inline-block;width:40px"> 功能</span> | 获取信任中心的信息 |
| :--------------------------------------------------------: | :----------------- |
|                            发送                            | AT+TCINFO<br />    |
|                            返回                            | OK<br />           |

# 四、联系作者

任何相关问题，欢迎联系作者，微信/QQ/手机同号：17780724435。
