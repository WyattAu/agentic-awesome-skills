1. Disassemble in IDA/Ghidra; `dmtc2`/`dmfc2` with selector in 0x100-0x40FF range indicates OCTEON CP2
Silicon Labs EFM32 Cortex-M binary; a flat binary loaded at 0x1000 in Thumb mode.
**Key insight:** Hardware AES accelerators on microcontrollers appear as MMIO register writes at a specific base address; cross-reference the vendor reference manual (EFM32 Reference Manual for Silicon Labs peripherals).
**Key insight:** QEMU's `-s` flag exposes a GDB stub on port 1234 for full debugging of MBR/bootloader code; workflow identical to userland debugging.