## QEMU

### 背景
QEMU是一款开源的仿真执行工具，可以模拟运行多种CPU架构的程序或系统。在执行时，QEMU会先以基本块（Basic Block）为单位，将目标指令经由TCG这一层翻译成宿主机的代码，得到TB(Translation Block)，最终在主机上执行。

QEMU 支持两种操作模式：用户模式仿真和系统模式仿真。用户模式仿真 允许一种架构的 CPU 构建的程序在另一种架构的 CPU 上执行，动态翻译/转换相应的指令/系统调用等。如果在相同架构的系统上进行用户模式仿真，可以实现近似本地的性能。

系统模式仿真允许对整个系统进行仿真，包括处理器和配套的外围设备。

QEMU的优势在于其快速、可移植的动态翻译程序。动态翻译程序 允许在运行时将用于目标（Guest）CPU 的指令转换为用于主机 CPU，从而实现仿真。

QEMU 实现动态翻译的方法是，首先将目标指令转换为微操作。这些微操作是一些编译成对象的 C 代码。然后构建核心翻译程序。它将目标指令映射到微操作以进行动态翻译。这不仅可产生高效率，而且还可以移植。

QEMU 的动态翻译程序还缓存了翻译后的代码块，使翻译程序的内存开销最小化。当初次使用目标代码块时，翻译该块并将其存储为翻译后的代码块。QEMU 将最近使用的翻译后的代码块缓存在一个 16 MB 的块中。QEMU 甚至可以通过在缓存中将翻译后的代码块变为无效来支持代码的自我修改。

### [事件处理机制](https://binhack.readthedocs.io/zh/latest/virtual/qemu/event.html)

事件处理设计架构, 所有事件都在事件循环中被处理. 基础为 Glib 事件循环. 核心是 poll 机制. 通过 poll 检查用户注册的事件源, 并执行相应的回调.

主事件循环 `main_loop_wait()`

### [iothread](https://binhack.readthedocs.io/zh/latest/virtual/qemu/iothread.html)

指令执行 以及 运行事件 两个循环任务. 线程与利用宿主机多核性能.

### [外围设备](https://binhack.readthedocs.io/zh/latest/virtual/qemu/device.html)

## Qemu 源码结构
    accel/tcg
      cpu-exec.c
        寻找下一个二进制翻译代码块
        如果没有找到就请求得到下一个代码块，并且操作生成的代码块
      translate-all.c
        cpu_gen_code 函数，初始化真正代码生成
    hw
      硬件目录
    主目录
      vl.c
        main 函数
        main_loop 函数，条件判断
        最主要的模拟循环，虚拟机环境初始化，和 CPU 的执行
      cpu-exec.c
        cpu_exec 函数，主要的执行循环
        寻找下一个二进制翻译代码块
        如果没有找到就请求得到下一个代码块，并且操作生成的代码块
      cpus.c
        qemu_main_loop_start 函数，分时运行 CPU 核
      exec-all.c
        struct TranslationBlock
        TB（二进制翻译代码块） 结构体
      exec.c
    target
      不同架构的对应目录
      将客户CPU架构的TBs转化成TCG中间代码

      TCG前的前端
        target/<arch>/translate.c
        将 guest 代码翻译成不同架构的 TCG 操作码
        target/<arch>/cpu.h
        CPU 状态结构体
    tcg
      tcg/tcg.c
        主要的 TCG 代码
      tcg/<arch>/tcg-target.c
        将 TCG 代码转化生成主机代码
      TCG前的后端

## 可用的机器和CPU
```sh
$ qemu-system-<target> -M/--machine help
$ qemu-system-<target> -M foo -cpu help
```

```sh
$ qemu-system-riscv64 -M help
Supported machines are:
microchip-icicle-kit Microchip PolarFire SoC Icicle Kit
none                 empty machine
shakti_c             RISC-V Board compatible with Shakti SDK
sifive_e             RISC-V Board compatible with SiFive E SDK
sifive_u             RISC-V Board compatible with SiFive U SDK
spike                RISC-V Spike board (default)
virt                 RISC-V VirtIO board
```

```sh
$ qemu-system-riscv64 -M virt -cpu help
any
rv64
shakti-c
sifive-e51
sifive-u54
x-rv128
```

## Qemu 编译 调试 以及 compile_commands.json 生成

```sh
mkdir -p output/qemu
./configure --prefix=./output/qemu --target-list=riscv64-softmmu --enable-gtk --enable-virtfs --disable-gio --enable-debug # enable-debug 增加gdb调试信息
bear -- make -j$(nproc) # 编译 同时生成 compile_commands.json 文件供clang lsp使用
gdb -q --args output/qemu/bin/qemu-system-riscv64
```

