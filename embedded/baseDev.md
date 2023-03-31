---
title: 裸机开发
description: 
published: true
date: 2023-03-26T08:03:14.697Z
tags: 
editor: markdown
dateCreated: 2023-03-26T07:47:25.497Z
---

# ARM介绍
**ARM是Advanced RISC Machine的缩写，即进阶精简指令集机器**。 arm更早称为Acorn RISC Machine，是一个32位精简指令集（RISC）处理器架构。 也有基于ARM设计的派生产品，主要产品包括Marvell的XScale架构和和德州仪器的OMAP系列

# ARM版本

| 内核版本号 |                          SoC版本号                           |
| :--------: | :----------------------------------------------------------: |
| ARMv1(26)  |                             ARM1                             |
| ARMv2(26)  |                    ARM2和ARM3（V2a）架构                     |
| ARMv3(32)  |                          ARM6、ARM7                          |
| ARMv4(32)  | [StrongARM](https://www.wikiwand.com/zh-cn/StrongARM)、[ARM7TDMI](https://www.wikiwand.com/zh-cn/ARM7TDMI)、[ARM9](https://www.wikiwand.com/zh-cn/ARM9)TDMI |
| ARMv5(32)  | ARM7EJ、ARM9E、ARM10E、[XScale](https://www.wikiwand.com/zh-cn/XScale) |
| ARMv6(32)  | ARM11、[ARM Cortex-M](https://www.wikiwand.com/zh-cn/ARM_Cortex-M) |
| ARMv7(32)  | ARM Cortex-A、[ARM Cortex-M](https://www.wikiwand.com/zh-cn/ARM_Cortex-M)、ARM Cortex-R |
| ARMv8(64)  |    Cortex-A35、Cortex-A50系列、Cortex-A70系列、Cortex-X1     |
|  ARMv9(64)   | [Cortex-A510](https://www.wikiwand.com/zh-cn/ARM_Cortex-A510)、[Cortex-A710](https://www.wikiwand.com/zh-cn/ARM_Cortex-A710)、[Cortex-A715](https://www.wikiwand.com/zh-cn/ARM_Cortex-A715)、[Cortex-X2](https://www.wikiwand.com/zh-cn/ARM_Cortex-X2)、[Cortex-X3](https://www.wikiwand.com/zh-cn/ARM_Cortex-X3)、ARM Neoverse N2 |


# ARM基础
- 交叉编译
- 设备启动过程
- GPIO
- 重定向
- 时钟系统

# ARM开发
- LED
- SDRAM
- 串口通讯
- 中断系统
- 定时器
- 看门狗
- RTC
- SD卡
- NandFlash
- INand
- I2C通讯
- ADC
- LCD
- 触摸屏
- SHELL