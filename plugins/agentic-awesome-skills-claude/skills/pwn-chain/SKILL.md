1. **拿到二进制 + 已知漏洞点**; 静态/审计/fuzz 已经找到溢出/UAF/double free，需要从触发到拿 shell
2. **CTF 题已经本地通了，远程打不通**; 远端环境差异导致脚本失效，需要稳定化
3. **真实目标的二进制利用**; SRC / 红队场景下，已经识别到内存损坏漏洞，需要构造 RCE
4. **Linux 内核驱动的 ioctl bug**; 用户态触发，目标是提权到 root
- **不要在本地跑通就交差**; 本地 libc / ASLR / 网络环境都和远程不同，仅在授权允许远程验证时按约定次数测试；只读审计无需执行利用
- **libc 版本必须确认**; 用 leak + libc-database 反查，不要假设是 Ubuntu 22.04 默认 libc
- **栈对齐是 64 位的常见坑**; `movaps xmm0, [rsp]` 在 rsp 未 16 字节对齐时段错误，加一个空 `ret` gadget 解决
- **堆利用对 glibc 版本极敏感**; tcache 在 2.26 引入，safe-linking 在 2.32 引入，2.34 移除 hooks，每个版本利用路径不同
- **内核 pwn 必须先确认 cpu 标志**; qemu 启动参数里有没有 +smep +smap +pku 直接决定 ROP 链怎么写
- **KASLR leak 一次就够**; 拿到一个内核地址后所有地址都算偏移，不要反复 leak