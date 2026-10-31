实际上**真实利用基本不走这条路**; 直接 commit_creds ROP 更短更稳。
// exploit.c; 内核 pwn 通用骨架
- **不要在内核里调 printk 输出测试**; 串口 IO 会改变时序，破坏 race；用一个 magic 寄存器值（rcx=0xdeadbeef）+ gdb watch 来调试