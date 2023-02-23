## 使用 gcc >= 12 启用 Zicsr 和 Zifencei 独立扩展

https://patchwork.ozlabs.org/project/buildroot/patch/20220723133525.279819-1-romain.naour@smile.fr/

 自 gcc 12 起，默认的 Riscv ISA 规范版本已升至 20191213 [1]。

  这引入了一个主要的不兼容问题是 csr 读/写 
  (csrr*/csrw*)指令和fence.i指令已经分开 
  “I”扩展，成为两个独立的扩展：Zicsr 和 
  紫芬;   所以你可能会收到这样的错误信息：unrecognized 
  操作码“csrr”（或“fence.i”）。 

  事实上，如果没有 Zifencei，我们就无法构建 opensbi 引导加载程序 [2]：

  opensbi-1.0/lib/sbi/sbi_tlb.c：汇编程序消息： 
  opensbi-1.0/lib/sbi/sbi_tlb.c:190: 错误：无法识别的操作码“fence.i”，需要扩展名“zifencei” 

  作为解决方法，opensbi 构建系统已被修补 [3] 以使用 
  -march=rv64imafdc_zicsr_zifencei 需要时。 
  由于本地补丁，此解决方法在 Buildroot 中不起作用 
  0001-Makefile-Don-t-specify-mabi-or-march.patch 删除-march 
  来自CFLAGS。 

  在 Buildroot 的上下文中，我们可以无条件地启用 Zicsr 
  gcc >=12 的 Zifencei 独立扩展（根据推荐 
  [4]) 因为可能没有运行 Linux 的 RISC-V 内核 
  支持基础 I 指令，但不支持 csr 和 fence 扩展... 
  因为甚至 OpenSBI 都假设 RISC-V 核心支持它们！


