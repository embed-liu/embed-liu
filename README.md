# embed-liu

嵌入式软件方向，主要做 **STM32 + FreeRTOS 实时控制** 与 **工业总线通信**（Modbus RTU / RS485），并把人机界面（LVGL）纳入同一套工程。工程习惯是分层：驱动层、RTOS 任务层、应用层分离，驱动尽量做成可独立移植的模块。

## 技术栈

| 方向 | 具体 |
|---|---|
| MCU | STM32F103 / STM32F407（Standard Peripheral Library），Keil MDK-ARM |
| 实时系统 | FreeRTOS：任务划分、优先级规划、队列 / 信号量 / 互斥量、栈水位监控、看门狗任务心跳 |
| 工业通信 | Modbus RTU 从站（FreeModbus）、RS485 半双工、USART 中断收帧、I2C、ADC 采集 |
| 人机界面 | LVGL 8.3 + FSMC 8080 并口屏，中文字库子集化 |
| 调试手段 | 逻辑分析仪 / 示波器抓总线与时序，Modbus Poll 做主站联调 |

## 项目

| 仓库 | 内容 | 技术点 |
|---|---|---|
| [stm32f407_project](https://github.com/embed-liu/stm32f407_project) | 工业监控节点：双路步进电机、4 路模拟量采集与过流/过压保护、Modbus RTU 从站、LVGL 三页监控界面 | FreeRTOS 多任务、FreeModbus 移植、故障保护状态机、LVGL 显示移植 |
| [stm32project](https://github.com/embed-liu/stm32project) | STM32F103 多任务物联网终端：DHT11 采集、OLED、按键阈值、Flash 掉电保存、IWDG 心跳、ESP8266 + MQTT 上云 | 队列/事件组/互斥量实践、AT 时序调试、非易失参数存储 |
| [stm32f407-modbus-rtu-motor-adc](https://github.com/embed-liu/stm32f407-modbus-rtu-motor-adc) | 裸机版 Modbus RTU 从站 + 步进电机 + ADC，作为工业监控节点的最小可跑版本 | Modbus 寄存器映射、RS485 方向切换 |

## 驱动库（可独立移植）

从工业监控节点中拆出的驱动模块，每个仓库含引脚定义、API、移植步骤与已知限制：

| 仓库 | 说明 |
|---|---|
| [stm32f407-step-motor-driver](https://github.com/embed-liu/stm32f407-step-motor-driver) | 双路步进电机，TIM6 中断节拍 + 8 拍相序表，非阻塞启停与软件急停 |
| [stm32f407-rs485-driver](https://github.com/embed-liu/stm32f407-rs485-driver) | RS485 半双工，USART2 + DE/RE 方向控制，Modbus RTU 物理层 |
| [stm32f407-at24cxx-driver](https://github.com/embed-liu/stm32f407-at24cxx-driver) | AT24Cxx EEPROM，软件 I2C + 临界区保护 + 魔数校验，用于参数掉电保存 |
| [stm32f407-modbus-app-mapping](https://github.com/embed-liu/stm32f407-modbus-app-mapping) | FreeModbus 应用层寄存器映射，读写校验、阈值下发与持久化 |

## 关注方向

- 实时性与可靠性：任务时序分析、优先级与中断优先级匹配、故障保护与自恢复；
- 通信协议：Modbus 之外在补齐 CAN / CAN FD 与诊断相关内容；
- 代码可移植性：驱动与应用解耦，接口稳定、文档齐全。

## 交流

问题、建议或想交流某个模块的实现，欢迎在本仓库开 Issue。
