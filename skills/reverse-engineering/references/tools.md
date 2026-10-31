**Key insight:** When `RijndaelManaged` appears in .NET decompilation, check if Key and IV are set to the same value; this is a common CTF pattern. The XOR stage often serves as a simple obfuscation layer before the real crypto.
- No flags register; comparisons are inline with branch instructions
- Compressed instructions (2 bytes) mixed with standard (4 bytes); use `CS_MODE_RISCVC`
- Decompiler output is confusing; compare with alternatives for clarity
- One decompiler mishandles a construct; another may get it right