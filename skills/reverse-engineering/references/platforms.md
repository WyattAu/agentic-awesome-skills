# Universal (fat) binaries: multiple architectures in one file
- `dsdump`; faster, supports Swift + Objective-C
- `otool -oV binary`; dump Objective-C segments
# binwalk: firmware analysis and extraction
UART:  Serial console; often gives root shell or bootloader access
JTAG:  Direct CPU debug; read/write flash, halt CPU, set breakpoints
SPI Flash: Direct chip read; dump entire firmware
eMMC:  Embedded MMC; common in routers, phones
# Big-endian vs little-endian: check ELF header or file command
# Branch delay slots: classic delay-slot behavior is instruction/version dependent; branch-likely and newer MIPS variants differ
# $gp (global pointer): used for PIC, points to .got
# lui + addiu pair: loads 32-bit constant (upper 16 + lower 16)
// copy_from_user / copy_to_user; data transfer with userspace
# .sys files are PE format: load in IDA/Ghidra as normal PE
# UDS (Unified Diagnostic Services): common in automotive CTF