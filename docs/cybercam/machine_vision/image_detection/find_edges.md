---
sidebar_position: 1
---

# 边缘检测

## 前言

本节学习使用OpenCV对图像进行边缘检测功能，使用Canny边缘检测算法，相对于前面轮廓检测，这里只需要简单几行代码便可实现。

## 实验目的

学习OpenCV库实现图像边缘检测并显示。

## 实验讲解

OpenCV Python库提供了Canny()函数实现边缘检测功能，可用于检测图像中的边缘。通常先将彩色图像转换为灰度图，再进行 Canny 边缘检测。

### cvtColor() 使用方法
```python
gray = cv2.cvtColor(image, code)
```

cvtColor图像颜色空间转换。返回gray为转换后得到的新图像矩阵。
- `image` ：输入的原始图像。
- `code` ：颜色空间转换模式变量（通常传入 OpenCV 预设的整型常量标识符，例如 `cv2.COLOR_BGR2GRAY` 或 `cv2.COLOR_BGR2HSV`）。

### Canny() 使用方法
```python
edges = cv2.Canny(image, threshold1, threshold2, apertureSize, L2gradient)
```

Canny边缘检测。返回edges为二值图像。
- `image` ：输入的 8 位图像，通常是单通道灰度图像。
- `threshold1` ：用于控制弱边缘的筛选。低于此阈值的像素直接抛弃。
- `threshold2` ：用于确立强边缘。高于此阈值的像素无条件保留为边缘。
 > 💡 **核心逻辑**：介于两个阈值之间的像素被称为“弱边缘”。它们就像是边缘的延伸线，只有当它们紧挨着“强边缘”（高于 `threshold2` 的像素）时，才会被激活并保留下来；孤立的弱边缘则会被当作噪点直接滤除。
- `apertureSize` ：计算图像梯度时使用的 Sobel 算子孔径大小，默认值为 3。
- `L2gradient` ：梯度幅值的计算方式，默认值为 False。
    - `False` : 使用 L1 范数近似计算，速度较快。
    - `True` : 使用 L2 范数计算，结果更精确，但计算量稍大。

从上面可知道Canny方法使用非常简单，我们只需要根据需求设定好合适的阈值即可，我们可以使用2组阈值来对比实验。代码编写流程如下：

```mermaid
graph TD
    读取图像-->转换灰度图像-->使用双阈值进行边缘检测-->显示图像;
```

## 参考代码

```python
'''
实验名称：边缘检测
实验平台：CyberCAM
说明：描绘摄像头采集图像的边缘
作者：01Studio
'''

import cv2  # OpenCV图像处理库
import time  
from walnutpi import Sensor, Display, direction, IDE

# 初始化显示屏
Display.init()

# 初始化摄像头，分辨率640x480
cap = Sensor.Sensor(640, 480)

# 检查摄像头是否打开
if not cap.isOpened():
    print("Cannot open camera")
    exit()

#获取当前显示屏方向，0表示默认，2表示180度翻转。
lcd_dir=direction.get_lcd() 
#print(lcd_dir) 

# 判断显示屏是否翻转，如果翻转，则设置显示旋转180°，摄像头同时设置为前置模式（水平镜像）
if lcd_dir == 2: #翻转了
    Display.set_rotation(2)
    cap.set_hmirror(1)

# ========== FPS计算 ==========
frame_count = 0       # 帧数计数器
start_time = time.time()
fps = 0.0

# 持续采集和显示图像
while True:

    # 读取图像帧
    ret, img = cap.read()

    # BGR彩色图转单通道灰度图
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

    # 进行边缘检测，阈值50和150
    edges = cv2.Canny(gray, 50, 150) 

    # 单通道灰度图转3通道BGR彩色图, 以便在显示屏和IDE上显示
    img = cv2.cvtColor(edges, cv2.COLOR_GRAY2BGR)
        
    # 每满1秒计算一次平均FPS
    frame_count += 1    
    current_time = time.time()
    if current_time - start_time >= 1.0:
        fps = frame_count / (current_time - start_time)
        frame_count = 0              # 重置帧数计数器
        start_time = current_time    # 重置计时起点
        print("FPS: ", fps)

    # 添加文字标签和FPS显示
    cv2.putText(img, f'FPS: {fps:.1f}', (10, 30), cv2.FONT_HERSHEY_COMPLEX, 1, (0, 255, 0), 2)

    # 显示到屏幕
    Display.show(img)

    # 显示到IDE
    IDE.show(img)
```

## 实验结果

运行代码，可以看到识别效果如下：

![edge_detection](./img/find_edges/find_edges1.png)

**修改代码的2个阈值可获得不同的效果**

```python
    # 进行边缘检测，阈值50和150
    img = cv2.Canny(img, 50, 150)
```
以上是 OpenCV 进行边缘检测的基础流程。在实际应用中，受环境光线、摄像头传感器等因素影响，摄像头采集的画面中是会含有一定噪声。为了追求更好的效果，通常会在 `cv2.Canny()` 之前先对灰度图进行高斯模糊（`cv2GaussianBlur`）降噪处理。在矩形检测案例里面会用到高斯模糊，有兴趣的同学可以先自行查阅相关资料了解。
