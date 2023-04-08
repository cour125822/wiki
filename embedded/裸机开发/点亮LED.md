---
title: 点亮LED
description: 
published: true
date: 2023-03-26T08:03:14.697Z
tags: 
editor: markdown
dateCreated: 2023-03-26T07:47:25.497Z
---

# 硬件介绍
**发光二极体**（英语：light-emitting diode，LED）是一种半导体光源，当电流通过它时会发光；即一种电致发光的半导体电子元件，其内电子与电子空穴复合，以光子的形式释放能量。
发光二极体结构的核心部分是p-n结，周边部分有环氧树脂密封其引线与框架以保护内部芯线。当p-n结通以正向电流时，能发射可见或非可见辐射，此辐射为透过三价与五价元素所组成复合光源。


# 电路图
![image.png](https://raw.githubusercontent.com/cour125822/photo_wi/main/wiki/202304082000297.png)

正极接VDD（Vdd是器件的电源端，Vdd多半是单极器件的正），负极接地。查阅数据手册可知，LED负极接了SoC上的一个引脚（GPIO）


# 相关寄存器
查阅数据手册可知，GPJ0相关的寄存器有以下：
- GPJ0CON, （GPJ0 control）GPJ0控制寄存器，用来配置各引脚的工作模式
- GPJ0DAT, （GPJ0 data）当引脚配置为input/output模式时，寄存器的相应位和引脚的电平高低相对应。
- GPJ0PUD, （pull up down）控制引脚内部弱上拉、下拉
- GPJ0DRV, （driver）配置GPIO引脚的驱动能力
- GPJ0CONPDN，（记得是低功耗模式下的控制寄存器）
- GPJ0PUDPDN  （记得是低功耗模式下的上下拉寄存器）

注：在驱动LED点亮时，应该将GPIO配置为output模式。

# 操作步骤
1. 操控GPJ0CON寄存器中，选中output模式
2. 操控GPJ0DAT寄存器，相应的位设置为

# 代码
## 汇编
```assembly
.equ GPJ0CON, 0xE020_0240
.equ GPJ0DAT, 0xE020_0240

.global led
led: 
	ldr r0, =GPJ0CON    @ 存储寄存器地址
	ldr r1, =0x1000     @ 设置引脚为输出模式
	str r1, [r0]        @ 寄存器间接寻址
	
	ldr r0, =GPJ0DAT    @ 存储寄存器地址
	ldr r1, =0x8        @ 设置当前LED点亮
	str r1, [r0]        @ 寄存器间接寻址
```

## C语言
```c
#define GPJ0CON 0xE020_0240
#define GPJ0DAT 0xE020_0240

#define r_GPJ0CON *((volatile unsigned int *)GPJ0CON)
#define r_GPJ0DAT *((volatile unsigned int *)GPJ0DAT)

void led_init(){
	r_GPJ0CON |= (0x1<<3)
}

void led_light(){
	r_GPJ0DAT |=(1<<3)
}

```



