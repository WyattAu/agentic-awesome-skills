| 有 Binary Ninja | **ollvm-breaker** |; | 针对 Android .so 实战（libvdog 等加固样本） |
| 遇到 Tigress 混淆 | d810-ng `UnflattenerSwitchCase`/`UnflattenerTigressIndirect` |; | d810-ng 内置 Tigress 专用 unflattener |
| **VMP 系** (SmallVmp/VMPilot/xVMP/VMPacker) |; | 指令虚拟化 | **不属于 OLLVM 范畴**，需 VM 逆向，参考 VM 专用工具 |
### 3.1 obpo-plugin: 效果未作比较验证，云插件
- ⚠️ 需要联网，敏感样本（内部未公开漏洞、商业机密）慎用--二进制会上传
### 3.2 d810-ng: 本地一站式首选
### 3.3 ollvm-unflattener: Miasm 符号执行，纯脚本
### 3.4 ollvm-breaker: Binary Ninja 实战
### 3.5 deollvm: ARM64 Unicorn
### 3.6 DeObfBR: BR 混淆专项
**⚠️ 简易对抗技巧（来自 awesome-ollvm）：** Goron/Arkari 风格的间接相关混淆，可以通过**设置数据段为只读**来简单对抗--间接跳转目标常依赖运行期可写的数据段，设只读后变成可静态求解。
### 3.7 angr: 符号执行通用框架
**Step 1; 提取 .so：**
**Step 2; 识别 OLLVM 与变种：**
**Step 3; 脱密（分层）：**
**Step 4; Frida 动态验证：**
| **DeObfBR** |; | BR 混淆专项 | 96 | 2026-06 | ✅ | Goron/Arkari 间接分支 |
| **angr** | CLI | 符号执行 |; | 活跃 | ✅ | 通用，被 Trap Angr 克制 |
| **SiMBA** | CLI/库 | MBA 化简 |; |; | ✅ | 表达式化简 |
| **Triton** | CLI | 符号执行+污点 |; | 活跃 | ✅ | 动态符号执行 |
- [obfuscator-llvm/obfuscator](https://github.com/obfuscator-llvm/obfuscator); 原始 OLLVM
- [HikariObfuscator/Hikari](https://github.com/HikariObfuscator/Hikari); Hikari
- [komimoe/Hikari](https://github.com/komimoe/Hikari); Arkari (基于 goron, LLVM 14+)
- [amimo/goron](https://github.com/amimo/goron); goron
- [bluesadi/Pluto](https://github.com/bluesadi/Pluto); Pluto
- [za233/Polaris-Obfuscator](https://github.com/za233/Polaris-Obfuscator); Polaris (原 Pluto)
- [open-obfuscator/o-mvll](https://github.com/open-obfuscator/o-mvll); O-MVLL
- [fuqiuluo/amice](https://github.com/fuqiuluo/amice); Rust 实现 OLLVM passes
- [lich4/awesome-ollvm](https://github.com/lich4/awesome-ollvm); **变种生态总览（强烈推荐先读）**
- [obpo-project/obpo-plugin](https://github.com/obpo-project/obpo-plugin); 最强云插件
- [w00tzenheimer/d810-ng](https://github.com/w00tzenheimer/d810-ng); 本地首选
- [cdong1012/ollvm-unflattener](https://github.com/cdong1012/ollvm-unflattener); Miasm 纯脚本
- [amimo/ollvm-breaker](https://github.com/amimo/ollvm-breaker); Binary Ninja
- [GeT1t/deollvm](https://github.com/GeT1t/deollvm); ARM64 Unicorn
- [Mrack/DeObfBR](https://github.com/Mrack/DeObfBR); BR 混淆专项
- [maskelihileci/IDA_Ollvm-unflattener](https://github.com/maskelihileci/IDA_Ollvm-unflattener); IDA 插件版
- [angr](https://angr.io/); 符号执行框架
- [SiMBA](https://github.com/tech-srl/simba); MBA 化简
- [Quarkslab: Deobfuscation: Recovering an OLLVM-protected program](https://blog.quarkslab.com/deobfuscation-recovering-an-ollvm-protected-program.html); deflat 经典原理
- [MODeflattener](https://github.com/mrT4ntr4/MODeflattener); 静态 deflat（ollvm-unflattener 的对照）