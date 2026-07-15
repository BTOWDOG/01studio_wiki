---
sidebar_position: 3
---

# 系统APP

CyberCAM系统自带APP，无法修改和删除。

![image](./img/system_app/1.png)

使用说明如下：

## 退出APP

进入APP后，从电容屏四周任一边缘往屏幕中心滑动，即可退出当前APP。

![image](./img/system_app/exit2.png)

## 设置

点击设置APP，可以配置系统相关功能。

![image](./img/system_app/2.png)

### 关于本机

包含设备名称、系统版本等信息。

![image](./img/system_app/3.png)

### 网络

可以查看和配置WiFi网络和以太网。以太网需要使用USB转以太网卡模块。[点击购买>>](https://item.taobao.com/item.htm?id=822775353673)

![image](./img/system_app/4.png)

### 存储

SD卡储存介质和系统分区信息。

![image](./img/system_app/5.png)

### 背光

通过拖动背光滑动条可以调节屏幕显示背光亮度。

![image](./img/system_app/6.png)

### 语言

系统语言设置。

![image](./img/system_app/7.png)

### 时区

系统时区设置。

![image](./img/system_app/8.png)

## 电源

CyberCAM默认跑Linux系统，非必要不建议直接断电，避免损坏系统文件，正常需要执行关机操作。

点击【电源】APP进入：

![image](./img/system_app/power.png)

### 关机和重启

可以看到有**关机、重启和取消**选项。可通过此功能来操作设备关机或者重启。点击【关机】后稍等一会，蓝灯熄灭表示关机完成。

![image](./img/system_app/power1.png)

## 终端模式

CyberCAM运行的是Linux Debian系统，通过终端模式APP可以进入Linux系统终端调试。

![image](./img/system_app/terminal.png)

点击【终端模式】APP进入后会提示，确认进入按【确认】键即可。

![image](./img/system_app/terminal1.png)

![image](./img/system_app/terminal2.png)

在CyberCAM的USB口接上无线或者有线键盘。即可使用终端交互。

![image](./img/system_app/terminal3.png)

通过下面指令可返回GUI界面：

```bash
sudo desktop
```

![image](./img/system_app/terminal4.png)

## 手电筒

【手电筒】功能用于控制补光灯，支持亮度调节：

![image](./img/system_app/light.png)

![image](./img/system_app/light1.png)

![image](./img/system_app/light2.png)

## 扫一扫

扫一扫会调用摄像头，适用用于如：在线训练平台训练完成后扫描二维码快速部署模型 等场景。

![image](./img/system_app/scan.png)