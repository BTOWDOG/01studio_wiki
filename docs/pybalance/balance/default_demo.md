---
sidebar_position: 20
---

# 出厂例程



在资料包示例程序里面有一个出厂例程。

![default_demo](./img/default_demo/default_demo1.png)

该例程包含pyBalance和pyController两部分代码。

![default_demo](./img/default_demo/default_demo2.png)

pyBalance例程集成了六轴传感器校准、直立平衡、超声波跟随和蓝牙遥控等功能，并配有相应的图形化操作界面。

小车上电后会首先检测是否存在有效的 cal_data.txt 校准文件。如果未检测到校准数据，则自动进入六轴传感器校准界面。

![default_demo](./img/default_demo/default_demo3.png)

完成校准后，按下 KEY2 确认并保存校准数据，随后返回功能选择界面，并停留在“六轴传感器校准”选项。

![default_demo](./img/default_demo/default_demo4.png)

在功能选择界面中，按下 KEY1 可以在 直立模式 → 蓝牙遥控 → 超声波跟随 → 六轴传感器校准 之间循环切换。当切换到“六轴传感器校准”后，再次按下 KEY1 会返回“直立模式”，并继续循环。当切换到需要使用的功能时，按下 KEY2 确认并进入对应模式。

![default_demo](./img/default_demo/default_demo5.png)

![default_demo](./img/default_demo/default_demo6.png)

![default_demo](./img/default_demo/default_demo7.png)

直立模式，小车仅保持自身平衡，不执行其他附加功能，界面显示如下。

![default_demo](./img/default_demo/default_demo8.png)

蓝牙遥控模式，小车的功能与前面[蓝牙遥控](./ble_control.md)实验一致，可通过手柄进行控制。

![default_demo](./img/default_demo/default_demo9.png)

超声波跟随模式，小车的功能与前面[超声波跟随](./follow.md)实验一致，可根据目标物体距离自动前进或后退。

![default_demo](./img/default_demo/default_demo10.png)

六轴传感器校准功能与前面[六轴传感器校准](./cail.md)实验的操作方式一致。完成校准并确认后，系统会将新的校准数据写入校准文件；如果校准文件已经存在，则使用本次校准结果覆盖原有数据。与前面实验不同的是，本综合程序加入了开机校准检测机制。当系统未检测到有效的校准数据时，会自动进入六轴传感器校准功能，完成校准后再返回功能选择界面。

pyController则整合了NES游戏、pyCar遥控车、pyDrone四轴飞行器、pyBalance平衡小车和手柄控制测试功能，并配有图例UI。

用户可以将出厂代码所有文件分别使用thonny ide上传到pyBalance和pyController即可实现出厂功能。这些功能实际上是将综合实验内容整合，用户可以自行研究相关代码。



