---
sidebar_position: 12
---

# 主控ID号

CyberCAM的主控是K230，每个芯片的chipid都是唯一的，用户可以通过下面指令获取chipid来区分不同设备。

```bash
cat /sys/class/chip_id/chip_id
```

![cpu_id](./img/chip_id/cpu_id1.png)

:::tip 提示
返回64个字符串数据。
:::
