# RoboX 电控系统上手指南

# Introduction

这份指南适合对嵌入式开发没有经验的同学。这份指南会解释一些常见概念以及会包含嵌入式开发的小技巧。可以点击

# 概念部分

## 基础概念

### ROBOMASTER开发版

RoboMaster开发板套件是一款面向机器人DIY的开源主控套件。开发板主控芯片为**STM32F427IIH6**，拥有丰富的扩展接口和通信接口，板载IMU传感器，可配合RoboMaster出品的M3508、 M2006直流无刷减速电机、UWB模块以及妙算等产品使用。

### STM32

STM32 是意法半导体推出的基于ARM内核的嵌入式**微控制器（MCU）**。在Robomaster中使用的STM32F427 MCU是基于CortexM4 CPU的高性能微控制器。

### 微控制器（MCU）和CPU区别

通常MCU会简化一些CPU内计算资源，但是MCU会集成更多的IO资源。通常，MCU上面不会运行操作系统。我们在编写MCU上运行的软件时需要自行进行多任务处理以及直接对**寄存器**进行操作。对MCU进行开发时我们需要参考官方手册了解芯片内资源。

### PID

PID是最常见的控制算法，他是比例（propotion）积分（intergration）微分（derivation）控制的缩写。PID 是最常见的控制算法。其中比例控制附负责稳态控制，微分控制负责瞬态响应，积分控制负责过去误差。![PID Example](What-is-PID-Control.png) for example，如果我们想使用PID控制电机转动的角度，比例控制将会控制目标角度与实际电机角度的差值，微分控制将会控制电机转速，积分控制将会补偿电机过去转动的角度。PID的一些分支有级联PID 自适应PID 等。[PID - WikiPEDIA](https://zh.wikipedia.org/wiki/PID%E6%8E%A7%E5%88%B6%E5%99%A8
) 

### PWM

PWM是Pulse Width Modul ation的缩写是一种调制方式。PWM信号为数字信号，在同一个时间只有打开（1）和关闭（0）两种信号。 通过调整1和0的时间比例，这样我们就可以使用数字信号表示模拟数值。PWM信号使用占空比（1 和 0 信号的比例）来表示信号的大小。目前我们的子弹发射电机是由PWM进行控制的. 
![PWM IMG](pwm.gif)

### 电调








