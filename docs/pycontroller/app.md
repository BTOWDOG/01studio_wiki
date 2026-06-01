---
sidebar_position: 9
---

# 手柄APP

我们为pyController手柄制作了一款APP，功能和外观和手柄完全一致，用户可以使用它来替代手柄，实现蓝牙无线控制01Studio 四轴飞行器、小车等设备。

![img](./img/app/app1.png)

:::tip 提示
目前仅有安卓版本APP。
:::

## 安装

apk文件位于 **开发板配套资料/07-app**

![img](./img/app/app2.png)

将apk文件拖到到Android手机并安装。**安装过程中同意相关权限**。

安装完成后务必先打开手机蓝牙功能，然后再打开APP，否则可能会闪退。

![img](./img/app/app3.png)

打开安装好的APP：

![img](./img/app/app4.png)

建议设置允许横屏方式使用，可以通过左右滑动或者点击箭头实现不同控制方式选择。

## pyDrone模式

pyDrone模式用于控制01Studio pyDrone四轴飞行器设备，只搜索“pyDrone”蓝牙设备名称。

[pyDrone飞行教程](../pydrone/intro/fly.md)

![img](./img/app/app5.jpg)

点击中间图片进入，此时旁边有校准好等待连接的pyDrone设备就会自动搜索到。

![img](./img/app/app5_2.jpg)

点击进入，可以看到操作界面跟pyController手柄一样。

![img](./img/app/app5_3.jpg)

## pyCar模式

跟pyDrone方式类似，这里不再重复。

[pyCar蓝牙遥控车](../pycar/car/ble_control.md)

## 控制模式

该APP提供一个通用控制模式，遵循pyController协议，支持搜索所有ble标准设备，蓝牙连接后每隔50ms发送一次数据。用户可以使用这个功能来连接自己DIY的设备。

[pyController协议>>](http://localhost:3000/docs/pycontroller/controller/control#%E5%AE%9E%E9%AA%8C%E8%AE%B2%E8%A7%A3)