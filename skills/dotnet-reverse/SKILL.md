dnSpyEx 直接拖进去 -- 能打开就是托管
**NativeAOT 例外：** 编译成 native，没有 CLR 头，但有 `System.Private.CoreLib` 字符串和重构过的类型元数据 -- 这类走 `reverse-engineering/`（IDA/r2），本 skill 仅做识别提示。
> .NET 动态调试比 native 友好得多 -- 能直接看到对象值、字符串内容。优先动态而非死磕静态。
- [references/obfuscators.md](references/obfuscators.md); ConfuserEx / SmartAssembly / Babel / Eazfuscator / .NET Reactor 脱混淆详解 + anti-tamper 绕过
- [references/common-workflow.md](references/common-workflow.md); 完整工作流、IL patch 可靠性、字符串解密器提取、状态机识别
- [references/sharp-tools.md](references/sharp-tools.md); 红队 Sharp* 工具分析、工具安装矩阵、dnSpy MCP 集成、社区资源索引