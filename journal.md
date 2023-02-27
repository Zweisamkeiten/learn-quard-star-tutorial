## Journal

### 2023-02-21

1. 按照目录梳理具体包含的内容
2. 对依赖库的作用编写作用和解释

### 2023-02-22

1. 初步学习 QOM api 并做简要记录
2. 使用 qemu7.2.5 版本 复现ch2&ch3&4
3. 实践仓库 [link](https://github.com/Zweisamkeiten/qemu-quard-star/tree/yhf-dev)

### 2023-02-23

1. 复现 ch5&ch6 未完成
2. 尝试用qemu已有平台机器 使用 opensbi + u-boot 启动 linux6.0.0, 进 kernel panic

### 2023-02-24

1. 复现 ch6&ch7
2. opensbi domains support 文档未及时更新 根据代码内存区域的权限设置 M模式必须全设置1, SU模式不能全为0
3. 向上游提了一个issue 已得到回复 https://github.com/riscv-software-src/opensbi/issues/289

### 2023-02-25

1. 修复 ch7 untrusted-domain 权限
2. 重写构建和运行脚本 增加 debug 脚本 将自己增加文件转移至单独目录
3. 初始化 u-boot submodule 开始尝试启动 u-boot

### 2023-02-26

1. 成功启动 u-boot

