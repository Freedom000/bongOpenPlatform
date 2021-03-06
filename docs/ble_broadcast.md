
##bong 硬件开放接口文档

###[首页 http://bong.cn/share](http://bong.cn/share)

本文档主要实现 bong II 与带蓝牙接收功能的智能设备联动。通过 yes!键 与手环蓝牙广播包配合,来控制相关的智能家居进行一系列动作。在此种应用场景中,bong 主要承担用户 ID 和简单操控角色。

1. bong 手环基本 ID 功能介绍:手环在待机状态不发送控制广播报文信息,一旦检 测到 yes!按键被触控,立即发送 5S 的广播包(相关格式见)信息。此广播包 信息包含触摸按键状态信息 ON/OFF、手环 ID(将来可以根据需要开放运动和睡 眠信息等)。手环 ID 作为用户的唯一身份识别码。

2. 手环广播报文格式:

D6 BE 89 8E 00 23 68 F7 08 0D BE E9 07 09 62 6F 6E 67 49 49 02 01 06 05 03 A1 C0 B8 C1 0B FF 34 12 68 F7 08 0D BE E9 00 00 D2 39 04


其中
D6 BE 89 8E 00 23 68 F7 08 0D BE E9 07 09 62 6F 6E 67 49 49 02 01 06 05 03 A1 C0 B8 C1 0B FF 34 12 （68 F7 08 0D BE E9） （00 00） D2 39 04

第一个括号字段68 F7 08 0D BE E9为bong ID

第二个括号字段XX XX为控制信息，默认为 0x00 0x00

即二进制 00000000 00000000，说明:
- 右数第 1 位为按键指示,有按键则置 1,无按键则置 0。
- 右数第 2 位为长按指示,按键超过 3 秒,则置 1,不超过 3 秒则置 0。
- 右数第 3 为为端按指示,按键在 2 秒内,则置 1,其他情况为 0。

因此:
- 触摸状态： 00 01
- 长按事件： 00 03 或00 20
- 短按事件： 00 04

注意： 按键过后,如果判断为短按或长按,会继续发几次短按或长按的信息。
