<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [RoboX 电控系统上手指南](#robox-%E7%94%B5%E6%8E%A7%E7%B3%BB%E7%BB%9F%E4%B8%8A%E6%89%8B%E6%8C%87%E5%8D%97)
- [Introduction](#introduction)
- [概念部分](#%E6%A6%82%E5%BF%B5%E9%83%A8%E5%88%86)
  - [基础概念](#%E5%9F%BA%E7%A1%80%E6%A6%82%E5%BF%B5)
    - [ROBOMASTER开发版](#robomaster%E5%BC%80%E5%8F%91%E7%89%88)
    - [STM32](#stm32)
    - [微控制器（MCU）和CPU区别](#%E5%BE%AE%E6%8E%A7%E5%88%B6%E5%99%A8mcu%E5%92%8Ccpu%E5%8C%BA%E5%88%AB)
    - [PID](#pid)
    - [PWM](#pwm)
    - [BLDC电机以及电调](#bldc%E7%94%B5%E6%9C%BA%E4%BB%A5%E5%8F%8A%E7%94%B5%E8%B0%83)
  - [进阶概念](#%E8%BF%9B%E9%98%B6%E6%A6%82%E5%BF%B5)
    - [GPIO](#gpio)
    - [FIFO](#fifo)
    - [寄存器](#%E5%AF%84%E5%AD%98%E5%99%A8)
    - [DMA](#dma)
    - [多任务处理](#%E5%A4%9A%E4%BB%BB%E5%8A%A1%E5%A4%84%E7%90%86)
  - [通讯协议](#%E9%80%9A%E8%AE%AF%E5%8D%8F%E8%AE%AE)
    - [串口/UART](#%E4%B8%B2%E5%8F%A3uart)
    - [CAN](#can)
    - [I2C](#i2c)
    - [自定义协议](#%E8%87%AA%E5%AE%9A%E4%B9%89%E5%8D%8F%E8%AE%AE)
  - [代码规范](#%E4%BB%A3%E7%A0%81%E8%A7%84%E8%8C%83)
  - [未来任务](#%E6%9C%AA%E6%9D%A5%E4%BB%BB%E5%8A%A1)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# RoboX 电控系统上手指南
[toc]
# Introduction

这份指南适合对嵌入式开发没有经验但是有一定电子电路基础的同学。这份指南会解释一些常见概念以及会包含嵌入式开发的小技巧。

# 概念部分

## 基础概念

这部分主要介绍一些背景知识以及我们日常工作中会遇到的一些概念

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

### BLDC电机以及电调

 直流无刷电机（BLDC Motor）作为电机的一种具有体积小，效率高的优点。与传统直流电机不同的是 BLDC电机里面没有电刷，它依靠电调将直流信号调制成三个相位不同的方波。 BLDC和普通三相电机一样，电源的频率越高电机的转速越高。

对于它们的连接，一般情况下是这样的：
1. 电调的输入线与电池连接；
2. 电调的输出线（有刷两根、无刷三根）与电机连接；
3. 电调的信号线与接收机连接。

![BLDC](BLDC.png)

我们的机器人一般会使用**CAN总线** 或者 **PWM** 将我们想要的转速发送给电调。

[延伸阅读](http://www.modouwo.com/AiHaoZhe/Tutorial/Detail/UAV/744/13)

## 进阶概念

这部分主要讲了关于嵌入式开发的一些概念。 

### GPIO
IO复用 Group Bank

### FIFO

### 寄存器

### DMA

### 多任务处理

- 轮询
- 中断
- RTOS

## 通讯协议

这部分文档介绍了我们机器人上比较常见的通讯方法以及协议

### 串口/UART

### CAN

### I2C

### 自定义协议

## 代码规范

## 未来任务




