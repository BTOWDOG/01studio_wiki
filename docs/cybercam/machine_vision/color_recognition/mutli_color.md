---
sidebar_position: 2
---

# 多种颜色识别

上一节我们学习了 [**单一颜色识别**](../color_recognition/single_color.md) ，只需要在这个基础上将代码稍作修改，即可实现多种颜色识别。

代码编写流程如下：

```mermaid
graph TD
    导入sensor等相关模块-->初始化和配置相关模块-->定义红,绿,蓝颜色阈值-->输入图片转换为LAB颜色空间-->根据颜色阈值生成二值掩膜-->查找连通区域-->过滤面积过小的区域-->获取色块位置和中心坐标-->绘制识别结果-->显示图像;
```

## 参考代码

```python
'''
实验名称：多颜色识别
实验平台：CyberCAM
说明：通过opencv库实现Cybercam识别程序预先设定的颜色色块
作者：01Studio
'''
import cv2  # OpenCV图像处理库
import time  
import numpy as np
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

# ========== 全图模式或缩放模式 ==========
width  = 640
height = 480
scale_x = 0.0
scale_y = 0.0

mode = 0 # 0:原图不缩放 1:根据定义的变量width，height大小进行缩放

if mode == 0:
    #原图不缩放，正常比例
    scale_x = 1.0
    scale_y = 1.0

elif mode == 1:

    # 宽度减少一半
    width = 320
    # 高度减少一半
    height = 240

    # 计算反向映射比例（原图尺寸 / 缩放后尺寸）
    # 当小图分辨率为 320x240 时，比例为 2.0，用于将小图坐标等比放大还原回原图
    scale_x = 640 / width
    scale_y = 480 / height  

# 颜色识别阈值 (L Min, L Max, A Min, A Max, B Min, B Max) LAB模型
# 下面的阈值元组是用来识别 红、绿、蓝三种颜色以及边框绘图的颜色，当然你也可以调整让识别变得更好
thresholds = {
    "RED": {
        "threshold": (30, 100, 15, 127, 15, 127),
        "draw_color": (0, 0, 255),
    },

    "GREEN": {
        "threshold": (30, 100, -80, 10, 50, 127),
        "draw_color": (0, 255, 0),
    },

    "BLUE": {
        "threshold": (0, 60, 0, 90, -128, -20),
        "draw_color": (255, 0, 0),
    },

}



#最小色块像素数
MIN_PIXELS = 150

#最小边框
MIN_WIDTH = 5
MID_HEIGHT = 5

def canmv_lab_to_opencv(threshold):
    """
    将 CanMV 的 LAB 阈值转换为 OpenCV 8位 LAB 阈值。

    OpenMV:
        L = 0～100
        A = -128～127
        B = -128～127

    OpenCV CV_8U:
        L = 0～255
        A = 0～255
        B = 0～255
    """
    l_min, l_max, a_min, a_max, b_min, b_max = threshold

    lower = np.array(
        [
            round(l_min * 255 / 100),
            a_min + 128,
            b_min + 128,
        ],
        dtype=np.uint8,
    )

    upper = np.array(
        [
            round(l_max * 255 / 100),
            a_max + 128,
            b_max + 128,
        ],
        dtype=np.uint8,
    )

    return lower, upper


# 提前转换阈值，不要在每帧循环中创建数组
for config in thresholds.values():
    config["lower"], config["upper"] = canmv_lab_to_opencv(
        config["threshold"]
    )

# 持续采集和显示图像
while True:

    # 读取图像帧
    ret, img = cap.read()

    if mode == 1:
        #调整图片的分辨率
        small = cv2.resize(img, (width, height),  interpolation=cv2.INTER_AREA)
    else:
        #保持原来的分辨率
        small = img

    #转换成LAB格式
    lab = cv2.cvtColor(small, cv2.COLOR_BGR2LAB)

    #提取色素
    for color_name, config in thresholds.items():
        #根据颜色阈值生成二值掩膜
        mask = cv2.inRange(lab, config["lower"], config["upper"])
        #连通域
        num_labels, labels, stats, centroids = cv2.connectedComponentsWithStats(mask, connectivity=8, ltype=cv2.CV_16U)
        #选择满足面积和尺寸要求的色块
        for label_index in range(1, num_labels):
            pixels = int(stats[label_index, cv2.CC_STAT_AREA])
            #减少碎块
            if pixels < MIN_PIXELS:
                continue

            x = int(stats[label_index, cv2.CC_STAT_LEFT])
            y = int(stats[label_index, cv2.CC_STAT_TOP])
            w = int(stats[label_index, cv2.CC_STAT_WIDTH])
            h = int(stats[label_index, cv2.CC_STAT_HEIGHT])

            # 过滤细长噪点
            if w < MIN_WIDTH or h < MIN_WIDTH:
                continue

            #坐标映射计算            
            center_x = int(centroids[label_index][0])
            center_y = int(centroids[label_index][1])

            center_x = int(center_x * scale_x)
            center_y = int(center_y * scale_y)

            x1 = int(x * scale_x) 
            y1 = int(y * scale_y)
            x2 = int((x + w) * scale_x) 
            y2 = int((y + h) * scale_y)

            #画图
            draw_color = config["draw_color"]

            cv2.rectangle(img, (x1, y1), (x2, y2), draw_color, 2,)

            cross_size = 4
            
            cv2.line(img, (center_x - cross_size, center_y), (center_x + cross_size, center_y), (255, 255, 255), 2,)
            cv2.line(img, (center_x, center_y - cross_size), (center_x, center_y + cross_size), (255, 255, 255), 2,)

            text_y = y1 - 5
            
            if text_y < 20:
                text_y = y1 + 20

            cv2.putText(img, color_name, (x1, text_y), cv2.FONT_HERSHEY_SIMPLEX, 0.6, draw_color, 2,)

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

与单一颜色识别例程相比，多颜色识别主要修改了颜色阈值配置。在 `thresholds` 字典中添加需要识别的颜色名称，并分别设置对应的 LAB 阈值以及检测框和文字的绘制颜色：
```python
    ...
thresholds = {
    "RED": {
        "threshold": (30, 100, 15, 127, 15, 127),
        "draw_color": (0, 0, 255),
    },

    "GREEN": {
        "threshold": (30, 100, -80, 10, 50, 127),
        "draw_color": (0, 255, 0),
    },

    "BLUE": {
        "threshold": (0, 60, 0, 90, -128, -20),
        "draw_color": (255, 0, 0),
    },

}
    ...
```
其中：
- RED、GREEN、BLUE：颜色名称，在遍历字典时会保存到 color_name 变量中，并可通过 cv2.putText() 绘制到图像上；
- `threshold`：当前颜色的 LAB 阈值元组；
- `draw_color`：识别成功后，检测框使用的 BGR 绘制颜色。

本例默认采用`mode=0`，程序直接使用摄像头采集的 `640×480` 图像进行检测。该模式能够保留更多图像细节，但 CPU 计算量较大。如果需要提升速度把设置为`mode=1`，并适当降低 `width` 和`height`

## 实验结果

运行多种颜色识别代码，可以看到实验结果如下，全部颜色被识别出来：

原图：

![color](./img/color/color1.png)

实验结果：

![color](./img/color/color3.png)