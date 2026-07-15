---
sidebar_position: 11
---

# 主控温度

## CPU温度信息

查看K230 CPU温度信息指令：
```bash
cat /sys/class/thermal/thermal_zone0/temp
```

![core_temp1](./img/core_temp/1.png)

:::tip 提示
温度单位为摄氏度，输出的数值需要除以1000才是真实的温度值。上图显示为61.998摄氏度。
:::
