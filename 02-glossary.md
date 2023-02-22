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

[flash](https://nieyong.github.io/wiki_ny/Flash.html)

pflash， program flash 存放程序代码. D-Flash 数据闪存, 存储非易失性数据.

Flash是嵌入式设备上的非易失存储器件，一般和SDRAM，SOC一起构成了最小系统。

> pflash是并行flash，目前真实的器件已经很少见了，市面上常见的是qspi的nor flash作为低阶固件的载体，而nor flash一般都支持XIP运行，那么基本上等同于pflash了，那么为了方便我们就使用qemu提供的plash作为早期固件的载体。

> 串行和并行的主要区别在于 IO 口的数量

 Flash的种类有NAND Flash与NOR Flash两种，对应于Flash里面存储单元的实现机制，NAND Flash内部结构是由与非门组成，而NOR Flash则由或非门组成。NAND Flash容量大，速度快，单位成本低，大量用于CF卡、SD卡等消费电子产品；NOR Flash容量小，但单片价格便宜，而且代码可以直接在上面运行，用于存储电子产品的执行代码、配置等。

NOR Flash容量少，主要用来存放执行代码。例如，在SMB的交换，路由器上，一般使用的是NOR Flash，大小为32MByte以下。NAND Flash容量大，一般用来存放数据，例如，在MP3,MP4上一般使用大容量的NAND Flash来存放歌曲，电影等。

从接口上分，NOR Flash可以分为并行NOR Flash与串行NOR Flash两类。

 并行Flash有48pin、56pin等，输出数据总线有8bit和16bit，地址线根据Flash容量不同，可以有20根或更多。并行Flash不需要MCU上有特殊的控制器，直接给各地址线、控制信号发相应的信号，就可以在数据线上输出相应的内容。

串行Flash，简称SPI Flash，一般只有8pin或者16pin，器件尺寸可以做得很小，通过串行的bit操作实现数据的访问，需要MCU上有SPI Flash控制器的支持，以进行SPI Flash的读写操作（至少要能实现bit与Byte的转换）；由于是串行操作，因此SPI Flash的擦除、写入操作比并行Flash要慢（这还跟CPU的SPI Flash控制器的实现有关，如一次缓冲的大小等），因此经常会感觉firmware升级时间比较长。

## 设备文件树 dt devicetree

In computing, a devicetree (also written device tree) is a data structure describing the hardware components of a particular computer so that the operating system's kernel can use and manage those components, including the CPU or CPUs, the memory, the buses and the integrated peripherals.

[about devicetree](http://www.ofitselfso.com/BeagleNotes/AboutTheDeviceTree.pdf)

> The Device Tree file the Linux Kernel reads is a BINARY file. It is not human readable.

在设备树之前，Linux内核包含了每个支持平台的硬件的所有信息。这些信息，如内存位置、中断、片上外设和许多其他东西都被编译到内核中。

为每个新的微控制器分叉Linux内核代码和实现非主线配置的选择实在不是一个严肃的长期选择，因此设备树的概念被开发出来。设备树使微控制器能够使用相同的主线内核代码和独立的、特定于板子的硬件配置。主线Linux内核3.7及以上版本都支持设备树.

在设备树方法中，Boot Loader将内核镜像和编译好的设备树二进制文件读入RAM内存，然后将设备树二进制文件的内存地址作为启动的一部分传递给内核.内核一旦运行,就会查看内存地址,读取设备信息,进行适当的配置,然后开始运行Linux系统的工作.

dts文件使用dtc工具编译生成dtb文件，这个dtb文件就是内核可以使用的文件

二进制的设备树文件，通常被赋予.dtb的文件名扩展名，实际上由字节码组成（很像Java和.NET），内核可以通过使用一种内部解释器来处理它

> 个人计算机 架构的 x86 通常不使用设备树，而是依赖各种自动配置协议（例如 ACPI ）来发现硬件。 使用设备树的系统通常将静态设备树（可能存储在 ROM 中）传递给操作系统，但也可以在 引导 的早期阶段生成设备树。 例如， Das U-Boot 和 kexec 可以在启动新操作系统时传递设备树。 在具有不支持设备树的引导加载程序的系统上，静态设备树可能与操作系统一起安装； Linux 内核 支持这种方法。 

raspi 4b `/boot/bcm2708-rpi-4-b.dtb`

```sh
sudo dtc -I dtb -O dts bcm2711-rpi-4-b.dtb > ~/rpi.dts
```

> U-Boot、opensbi等都直接使用fdt作为驱动框架以解耦硬件具体配置。

## 多级Bootloader

> SOC的方式，因而其IC内部不仅仅是一颗cpu核，可能包含各种各样的其他IP，因而相关的上层软件也需要针对性的划分不同的功能域，操作域，安全域等上层应用。为了能支持复杂而碎片化的应用需求，SOC的Boot阶段延伸出了数级的BootLoader，为行文简单，我们把第一级BootLoader称作BL0，下一级则为BL1、BL2……等。

## OpenSBI

> 使用的RISCV架构进行的此次开发学习，那么RISCV为了能统一设计思路，提出了标准的SBI规范，SBI即为 （RISC-V Supervisor Binary Interface），SBI作为直接运行在系统M模式（机器模式）下的程序，向上层OS提供了统一的系统调用环境，SBI程序拥有最高的权限，可以访问所有的硬件资源，同时其控制PMP等硬件级的权限管理单元，将系统划分为多个域(domain)以供上层不同的安全等级的多操作系统使用并不会造成数据侵入破坏。如下图所示，SBI位于S模式和M模式之间。

SBI Supervisor Binary Interface [wiki](https://zh.wikipedia.org/wiki/%E7%89%B9%E6%9D%83%E5%B1%82%E4%BA%8C%E8%BF%9B%E5%88%B6%E6%8E%A5%E5%8F%A3#cite_note-1)

特权层二进制接口 RISC-V 架构下引导程序环境的规范 [1](https://github.com/riscv-non-isa/riscv-sbi-doc)

特权层接口规范[2]定义了以下的模块。

    基础模块：包含探测支持的SBI功能、得到SBI版本和得到机器版本等部分；
    时钟中断模块：设置下一个时钟中断的发生时间；
    跨核软中断模块：发送跨处理核的软件中断；
    远程栅栏模块：跨处理核发送栅栏指令，包括指令栅栏、内存映射栅栏和虚拟机栅栏指令等；
    核状态模块：启动、停止和暂停一个处理核，以及读取处理核的当前状态；
    复位模块：重启或关闭系统。

其中，基础模块是必须要求实现的模块，其它模块都是可选实现的模块。

SBI（Supervisor Binary Interface）是Supervisor之间的接口 执行环境 (SEE) 和 supervisor。 它允许主管 使用 ecall 指令执行一些特权操作。 示例 SEE和supervisor分别是：Unix类平台上的M-Mode和S-Mode，其中SBI是 它们之间的唯一接口，以及 Hypervisor extended-Supervisor (HS) 和 Virtualized Supervisor (VS)。

目前流行的特权层接口实现软件实现有以下几种。

    伯克利引导程序（BBL）
    OpenSBI[3](https://github.com/riscv/opensbi)
    Xvisor
    KVM
    RustSBI[4](https://github.com/rustsbi)
    Diosix[5](https://github.com/diodesign/diosix)
    Coffer[6](https://github.com/jwnhy/coffer)

## OpenSBI domain

OpenSBI 域是底层硬件的系统级分区（子集） 有自己的内存区域（RAM 和 MMIO 设备）和 HART。 OpenSBI 将尝试使用 RISC-V 平台实现域之间的安全隔离 PMP、ePMP、IOPMP、SiFive Shield 等功能.

帮助实现 OpenSBI 域支持的重要实体是：

    struct sbi_domain_memregion - 域内存区域的表示
    struct sbi_hartmask - 域 HART 集的表示
    struct sbi_domain - 域实例的表示

RISC-V 平台的每个 HART 都必须分配有一个 OpenSBI 域。 OpenSBI 平台支持负责填充域和 提供 HART id 到域映射。 OpenSBI 域支持将由 默认将 ROOT 域 分配给 RISC-V 平台的所有 HART，因此 OpenSBI 平台支持填充域不是强制性的。

## U-Boot

[wiki](https://en.wikipedia.org/wiki/Das_U-Boot)
[引导加载程序的比较](https://en.wikipedia.org/wiki/Comparison_of_bootloaders)
[Uboot基础笔记](https://github.com/zhaojh329/U-boot-1/blob/master/%E7%AC%AC0%E7%AB%A0-U-boot%E5%9F%BA%E7%A1%80.md)

是一种开源的 主要 引导加载程序 ，用于 嵌入式设备 以打包指令以启动设备的操作系统内核。 它可用于多种 计算机架构 ，包括 68k 、 ARM 、 Blackfin 、 MicroBlaze 、 MIPS 、 Nios 、 SuperH 、 PPC 、 RISC-V 和 x86 。

> U-Boot 既是第一阶段引导加载程序，也是第二阶段引导加载程序。 它由系统的 ROM（例如 ARM CPU 的片上 ROM）从支持的引导设备加载，例如 SD 卡、SATA 驱动器、NOR 闪存（例如使用 SPI 或 I²C ）或 NAND 闪存。 如果存在大小限制，U-Boot 可能会分为两个阶段：平台将加载一个小型 SPL（辅助程序加载器），这是 U-Boot 的精简版，SPL 将进行一些初始硬件配置（例如 使用 CPU 缓存作为 RAM 的DRAM 初始化）并加载更大的、功能齐全的 U-Boot 版本。无论是否使用 SPL，U-Boot 都会执行第一阶段（例如，配置内存控制器和 SDRAM）和第二阶段引导（执行多个步骤以从中加载现代操作系统）必须配置的各种设备，呈现一个菜单供用户交互和控制启动过程等）。

## XIP 模式

XIP eXecute In Place 就地执行

eXecute In Place，即芯片内执行，是指CPU直接从存储器中读取程序代码执行，而不用再读到内存中。应用程序可以直接在flash闪存内运行，不必再把代码读到系统RAM中。flash内执行是指nor flash不需要初始化，可以直接在flash内执行代码。但往往只执行部分代码，比如初始化RAM。好处即是程序代码无需占用内存，减少内存的要求。

所谓片内执行不是说程序在存储器内执行，CPU的基本功能是取指、译码、运行。Nor Flash能在芯片内执行，指的是CPU能够直接从Nor flash中取指令，供后面的译码器和执行器来使用。

为实现就地执行，必须满足几个条件：

1. 存储器必须提供与内存相似的接口给CPU。

2. 该接口必须提供足够快的读取操作，并具有随机访问模式。

3. 如有文件系统，则需要提供合适的映射功能

4. 程序链接时需要知道存储器的地址或地址与位置无关。

5. 程序不能修改已加载映像中的数据。

NOR Flash和EEPROM通常能满足上述要求。

系统引导时的就地执行

通常，第一阶段的引导程序是一个XIP程序，它链接时指定从flash芯片上电后的映射地址开始运行，设置系统RAM，把第二阶段的引导程序或操作系统内核加载进RAM。

在这初始化期间，可写存储器可能不可用，所有的计算都必须在处理器寄存器中执行。因此，第一阶段的引导程序通常以汇编语言编写，提供尽量少的功能，只为下一阶段程序的提供正常的执行环境。有些处理器也能通过嵌入少量SRAM或允许将Cache用作RAM来允许用高级语言编写这些程序。

对于内核和引导程序，地址空间通常是内部分配的。为了使用XIP，需要指示链接程序将不可修改的数据和可修改数据放在不同的地址区间，并提供将可修改数据复制到可写内存的机制，使得任何程序正常访问这些数据。

如果地址空间是外部分配的，比如不提供虚拟内存的系统，编译器需要通过向数据区域的私有副本的指针添加偏移量来访问所有可修改的数据。在这种情况下，外部加载程序负责设置实例特定的内存区域。

主内存初始化之前，BIOS 和 UEFI 使用 XIP技术。

[XIP技术总结](https://zhuanlan.zhihu.com/p/368276428)

## virtio

[virtio简介](https://www.cnblogs.com/super-sos/p/9044154.html)

VirtIO是一个用于 IO 虚拟化的平台，适用于多个管理程序（和 QEMU）。

> virtio是一种I/O半虚拟化解决方案，是一套通用I/O设备虚拟化的程序，是对半虚拟化Hypervisor中的一组通用I/O设备的抽象。

## busybox

[wiki](https://en.wikipedia.org/wiki/BusyBox)

BusyBox是一个遵循GPL协议、以自由软件形式发行的应用程序。Busybox在单一的可执行文件中提供了精简的Unix工具集，可运行于多款POSIX环境的操作系统，例如Linux（包括Android）、Hurd、FreeBSD等等。由于BusyBox可执行文件的文件比较小，使得它非常适合使用于嵌入式系统。作者将BusyBox称为“嵌入式Linux的瑞士军刀”。

BusyBox所包含的程序只需要简单的将名称附加在第一个参数即可执行：
```sh
/bin/busybox ls
```

更常见的作法是，这些指令会以链接（使用硬链接或者符号链接）至BusyBox可执行档，BusyBox会侦测其被链接时的名称，并执行对应的指令。举例来说，只要将/bin/ls链接到/bin/busybox，即可执行

```sh
/bin/ls
```

## 根文件系统

[使用busybox构建最小根文件系统](https://embeddedstudy.home.blog/2019/01/23/building-the-minimal-rootfs-using-busybox/)

Rootfs（根文件系统）对于启动linux非常重要 内核映像。 内核映像在引导期间根据根执行。 通常 Rootfs 被创建并放置在闪存中 设备，可以是您的 Android 手机或您的个人 SD 卡分区中的计算机或根分区。 开机期间 进程内核执行 /sbin/init ，后者依次查找 /etc 目录下的 inittab 或 init.d/rcS 文件。 所有的应用程序都会 驻留在这个根文件系统中。

