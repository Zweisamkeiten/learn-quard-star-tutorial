opensbi介于Loader和BOOTloader作为RunTime提供上层系统运行时的系统调用

## OPENSBI 平台固件
OpenSBI 为特定平台提供固件构建。 不同类型的 支持固件处理不同平台的差异 早期启动阶段。 所有固件将执行相同的初始化程序 根据平台特定代码的平台硬件以及 OpenSBI 通用库代码。 支持的固件类型会有所不同 处理平台早期启动阶段传递的参数，以及 如何处理和执行固件之后的引导阶段。

之前的booting阶段会通过以下寄存器传递信息 RISC-V CPU：

    * hartid 通过 a0 寄存器
    * 寄存器在内存中的设备树 blob 地址 通过 a1. 地址必须 对齐到 8 字节。

## [OpenSBI平台支持指南](https://github.com/riscv-software-src/opensbi/blob/master/docs/platform_guide.md)

OpenSBI 平台支持指南

OpenSBI 平台支持允许实现定义一组 特定于平台的挂钩（硬件操作函数），形式为 struct sbi_platform 数据结构实例。 此实例需要 独立于平台的 libsbi.a 来执行特定于平台的操作。

OpenSBI 提供的每个参考平台支持都定义了一个实例 数据结构 struct sbi_platform 。 对于每个支持的平台， libplatsbi.a 将此实例与 libsbi.a 集成以创建一个 特定于平台的 OpenSBI 静态库。 这个库已安装 在 <install_directory>/platform/<platform_subdir>/lib/libplatsbi.a

OpenSBI 还提供了可启动运行时固件的实现示例 支持的平台。 相关联 这些固件与libplatsbi.a 。 固件二进制文件安装在 <install_directory>/platform/<platform_subdir>/bin 。 这些固件可以 在支持的平台上用作可执行运行时固件作为替代品 用于旧版 riscv-pk 引导加载程序 (BBL)。

完整 doxygen 风格文档 struct sbi_platform 及相关 API 在文件 include/sbi/sbi_platform.h 中可用。

## 添加对新平台的支持

新平台的支持，如下所示： 对名为<xyz> 可以添加

    1. 目录下创建一个名为 <xyz> 在 platform/ 的目录。
    2. 的平台配置文件 创建名为Kconfig 和 configs/defconfig 在 platform/<xyz>/ 目录下。 这些配置文件将 为要编译的源提供构建时配置。
    3. 创建 platform/<xyz>/objects.mk 用于列出平台的 文件 要编译的目标文件。 该文件还提供特定于平台的 编译器标志，然后选择固件选项。
    4. 创建一个 platform/<xyz>/platform.c 文件，提供 结构 sbi_platform 实例。

平台支持代码模板 platform/template 目录。 将此目录及其内容复制为名为 <xyz> 目录下的 platform/ 将创建所有文件 上文提到的。

