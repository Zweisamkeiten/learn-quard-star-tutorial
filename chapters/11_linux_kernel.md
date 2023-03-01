u-boot start kernel stuck at "Starting kernel ..."

https://unix.stackexchange.com/questions/608299/u-boot-stuck-at-starting-kernel

tty：不要强制将 RISCV SBI 控制台作为首选控制台
https://patchwork.kernel.org/project/linux-riscv/patch/20190425133435.56065-1-anup.patel@wdc.com/

19:     chosen {
20-             bootargs = "earlycon console=ttySIF0";
21-             stdout-path = "/soc/serial0@10000000";
22-     };

