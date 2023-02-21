## ninja-build
archlinux: ninja

brief: Small build system with a focus on speed.
QEMU 构建系统需要使用 GNU make。

通常Ninja会搭配Meson(项目构建系统,如Makefile, automake, CMake.)使用. Meson 的输出是一个 build.ninja 文件,它与 Ninja 一起使用 构建系统. 
但 QEMU 使用不同的方法,其中 Makefile 规则由 build.ninja 文件生成. 主要的 Makefile 包括这些规则并包装它们, 以便在 QEMU 之前构建子模块. 由此产生的构建系统在本质上基本上是非递归的,在与 automake 的常见做法形成对比.

Ninja是一个轻量的构建系统，主要关注构建的速度。它与其他构建系统的区别主要在于两个方面：1. Ninja被设计成需要一个输入文件的形式，这个输入文件则由高级别的构建系统生成；2. Ninja被设计成尽可能快速执行构建的工具。

## pkg-config
archlinux: pkgconf

brief: Package compiler and linker metadata toolkit

用于返回已安装库的基本信息. 以解决 makefile 中复杂的显式依赖库信息

```make
obj: foo.c
  cc foo.c `pkg-config --cflags --libs bar`
```

其中 `bar` 为程序要链接的库的名称.

```sh
$ pkg-config --cflags --libs sdl2
-I/usr/include/SDL2 -D_REENTRANT -pthread -lSDL2
```

`pkg-config` 获取信息: `bar.pc`. `prefix/lib/pkgconfig/`, ex: `/usr/lib/pkgconfig/`

```pc
# sdl pkg-config source file

prefix=/usr
exec_prefix=${prefix}
libdir=/usr/lib
includedir=/usr/include

Name: sdl2
Description: Simple DirectMedia Layer is a cross-platform multimedia library designed to provide low level access to audio, keyboard, mouse, joystick, 3D hardware via OpenGL, and 2D video framebuffer.
Version: 2.0.21
Requires:
Conflicts:
Libs: -L${libdir}  -pthread -lSDL2  
Libs.private: -lrt -lunwind-generic -lunwind -lglib-2.0 -lgobject-2.0 -lgio-2.0 -libus-1.0 -ldbus-1 -lm  -Wl,--no-undefined -pthread -lSDL2 
Cflags: -I${includedir}/SDL2  -D_REENTRANT
```

## [libglib2.0-dev 头文件和静态库](https://github.com/GNOME/glib)
archlinux: glib2同时包含共享库

debian/ubuntu:  libglib2.0-0 共享库

brief: Low level core library

包含许多有用的 C 例程的库, 用于诸如树, 散列hash, 列表和字符串类的东西. 通用C库, 被GTK+, GIMP和GNOME等项目使用.

编译使用libglib2.0库的程序依赖该包, 其包含头文件和编译所需的静态库.

## [libpixman-1-dev](https://github.com/libpixman/pixman)
archlinux: pixman
brief: The pixel-manipulation library for X and cairo

用于处理像素区域的库——一组 YX 带状矩形、使用 Porter/Duff 模型的图像合成以及用于几何图元（包括梯形、三角形和矩形）的隐式掩码生成.

## [libgtk-3-dev](https://www.gtk.org/)
archlinux: gtk3

brief: GObject-based multi-platform GUI toolkit

多平台工具集用于创建图形用户界面 头文件以及开发文件

## [libcap-ng-dev](https://github.com/stevegrubb/libcap-ng)
archlinux: libcap-ng-dev

brief: A library for Linux that makes using posix capabilities easy

该库为linux kernel实现POSIX标准的用户空间接口.

The libcap-ng library is intended to make programming with POSIX capabilities much easier than the traditional libcap library.

## [libattr1-dev](https://savannah.nongnu.org/projects/attr)
archlinux: attr

brief: Extended attribute support library for ACL support

操作文件系统扩展属性的命令

## [libsdl2-dev](https://www.libsdl.org/)
archlinux: sdl2

brief: A library for portable low-level access to a video framebuffer, audio output, mouse, and keyboard (Version 2)

## [device-tree-compiler](https://www.devicetree.org/)
archlinux: dtc

brief: device tree compiler

设备树编译器 dtc 将给定格式的设备树作为输入,并输出另一种格式的设备树,用于在嵌入式系统上引导内核.

通常,输入格式是"dts",一种人类可读的源格式,并创建"dtb"或二进制格式作为输出.

## [bison](https://www.gnu.org/software/bison/bison.html)
archlinux: bison

brief: The GNU general-purpose parser generator

Bison 是一种通用解析器生成器，可将 LALR(1) 上下文无关文法的文法描述转换为 C 程序以解析该文法。精通 Bison 后，您可以使用它来开发范围广泛的语言解析器，从简单的桌面计算器中使用的解析器到复杂的编程语言。

Bison 与 Yacc 向上兼容：所有正确编写的 Yacc 文法都应该无需更改即可与 Bison 一起工作。

## [flex](https://github.com/westes/flex)
archlinux: flex

brief: A tool for generating tex-scanning programs

快速词法分析器生成器

Flex 是一种用于生成扫描仪的工具：识别文本中词汇模式的程序。它读取给定的输入文件以获取要生成的扫描仪的描述。描述以成对的正则表达式和 C 代码的形式出现，称为规则。Flex 生成一个 C 源文件 lex.yy.c 作为输出，它定义了例程 yylex()。该文件经过编译并与 -lfl 库链接以生成可执行文件。当可执行文件运行时，它会分析其输入以查找正则表达式的出现。每当它找到一个，它就会执行相应的 C 代码。

## [gperf](https://www.gnu.org/software/gperf/)

archlinux: gperf

brief: Perfect hash function generator

gperf 是一个为关键字集生成完美散列函数的程序。

一个完美的散列函数很简单: 一个哈希函数和一个数据结构，它允许在一组词中使用精确的1个探测数据结构来识别一个关键词。

## [intltool](https://launchpad.net/intltool)
archlinux: intltool

brief: The internationalization tool collection

自动将 oaf、glade、bonobo ui、nautilus 主题和其他 XML 文件中的可翻译字符串提取到 po 文件中。

自动将 po 文件的翻译合并回 .oaf 文件（编码为 7 位干净）。合并机制也可以扩展以支持其他类型的 XML 文件。

## [mtd-utils](http://www.linux-mtd.infradead.org/)
archlinux: mtd-utils

brief: Utilities for dealing with MTD devices

用于操作内存技术设备的实用程序，例如闪存、片上磁盘或 ROM。包括 mkfs.jffs2，一个创建 JFFS2（日志闪存文件系统）文件系统的工具。

## [libslirp-dev](https://gitlab.freedesktop.org/slirp/libslirp)
archlinux: libslirp

brief: General purpose TCP-IP emulator

通用 TCP-IP 仿真器库（开发文件）
libslirp 是虚拟机、容器或各种工具使用的用户模式网络库。

该软件包包含编译使用 libslirp 的应用程序所需的头文件和其他文件。
