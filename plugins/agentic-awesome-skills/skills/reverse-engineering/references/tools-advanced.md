1. Identify VM entry points; look for pushad/pushaq-like sequences
2. Find the handler table; large indirect jump (jmp [reg + offset])
3. Trace handler execution; each handler ends with jump to next
   - vRet (VM exit; restores real registers)
3. Run to OEP (Original Entry Point); may need several attempts
# Opens in BinDiff GUI: shows matched/unmatched functions
### d810-ng (IDA): 本地首选
### obpo-plugin (IDA): 效果最强，云插件
### ollvm-unflattener: Miasm 纯脚本
### DeObfBR: BR 混淆专项
### deollvm: ARM64 Unicorn
**Key use:** Triton excels at deobfuscation; run the program concretely, but track symbolic state, then simplify the collected constraints.
# Hardware watchpoint: break when memory changes
# Ghidra Python (Jython): run via Script Manager or headless
Send input where `input[i] = i` (position-encoded). Single-step through the binary monitoring the CPU zero flag (ZF). When ZF is set at a comparison involving a specific position's value, the comparison matched; log the expected value for that position.
// dump_xo.c; compile: gcc -shared -fPIC -o dump_xo.so dump_xo.c