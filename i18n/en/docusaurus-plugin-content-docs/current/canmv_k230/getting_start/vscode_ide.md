---
sidebar_position: 1
---

# VSCode IDE开发环境

:::tip 提示
使用VSCode IDE前先将镜像升级到V1.8以上，否则部分功能无法使用。
:::

## VSCode插件安装

CanMV K230提供VScode插件，下周安装后即可拥有基于VSCode的开发环境。支持代码补全和实时运行调试、文件系统、终端交互、高帧率实时图像显示等功能。

先安装VSCode，如果已经安装，请跳过。

下载地址：https://code.visualstudio.com/download

![img](./img/ide/1.png)

安装完成后打开，点击【插件】图标，搜素`canmv`，点击安装按钮。

![img](./img/ide/2.png)

安装完成后左侧出现CyberCAM插件的LOGO说明安装完成。

![img](./img/ide/3.png)

## 连接开发板

CanMV K230插入烧录镜像的SD卡，通过TYPE-C连接到电脑，[烧录镜像和开机](./image.md)。**请使用v1.8以上版本镜像**

找到设备后连接框会显示出设备的端口，支持多个设备显示。然后点击下图小图标连接开发板。

![img](./img/ide/4.png)

连接成功后如下图：

![img](./img/ide/5.png)

## 运行代码

这里示例示例代码演示，打开 **01科技（01Studio）CanMV K230开发套件配套资料\02-例程源码\2.机器视觉\1.摄像头**文件夹，将`camera.py`代码文件拖动到VSCode窗口。

![img](./img/ide/6.png)


点击运行按钮，代码会在开发板上运行

![img](./img/ide/7.png)

运行结果会显示在终端窗口，IDE右边为摄像头图像预览窗口。
:::tip 提示
这里只做代码运行演示，实验代码讲解将在后续章节中进行详细说明。
:::

![img](./img/ide/8.png)


## 终端交互

插件提供`CanMV Terminal` ，相当于MicroPython的REPL交互功能，可以查看调试信息。

:::tip 提示
如果没有找到，可以点击上方 **工具栏 -- 终端**， 然后在下方终端选择`CanMV Terminal`。
:::

![img](./img/ide/10.png)


## 文件系统

左下方为开发板文件系统，展示了开发板的所有目录文件，通过点击`右键`可以对文件或文件夹进行重命名、下载到本地电脑、从电脑上传文件到开发板等功能。

![img](./img/ide/11.png)








