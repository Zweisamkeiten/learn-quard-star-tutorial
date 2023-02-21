## TODO 待完善补全

## [QOM](https://qemu.readthedocs.io/en/latest/devel/qom.html)
  QOM (Qemu Object Model)是Qemu实现的面向对象编程模式。Qemu是用C语言编写的，而C语言是面向过程的编程语言，无法享受面向对象编程模式针对复杂软件系统在设计模式上的优越性。为解决该问题，Qemu社区通过C语言实现了一套面向对象的编程接口，即QOM，并成功将其应用在Qemu设备模型的管理中。
  [QOM模型初始化流程](https://www.cnblogs.com/edver/p/14684143.html)

## 中断控制器

TODO: 内核中断 外设中断

RISCV标准的中断控制器分为两部分内核中断CLINT(Core Local Interrupt)和外设中断控制器Platform-Level Interrupt Controller(PLIC)

## 串口

串口： 串口是一个泛称，UART，TTL，RS232，RS485都遵循类似的通信时序协议，因此都被通称为串口。

## pflash

TODO: Nand Flash, Nor Flash, XIP 运行

> pflash是并行flash，目前真实的器件已经很少见了，市面上常见的是qspi的nor flash作为低阶固件的载体，而nor flash一般都支持XIP运行，那么基本上等同于pflash了，那么为了方便我们就使用qemu提供的plash作为早期固件的载体。

> 串行和并行的主要区别在于 IO 口的数量
