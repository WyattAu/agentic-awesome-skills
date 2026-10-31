4. 看 P/Invoke 段（Interop.* 命名空间）-- native API 调用在这里
社区已有多个 dnSpy MCP 项目，把 dnSpy 的反编译/IL 检查暴露成 MCP 工具，AI 可直接调用 -- 和 reverse-skill 的 MCP 哲学完全一致。
- **Washi 博客**; .NET 逆向大佬：https://blog.washi.dev/posts/misconceptions-about-dotnet/
- **dnSpyEx**; dnSpy 的活跃维护分支：https://github.com/dnSpyEx/dnSpy
- **de4dot**; .NET 脱混淆：https://github.com/de4dot/de4dot
- **dnlib**; 元数据编程：https://github.com/dnlib/dnlib
- Medium《De-obfuscating and reversing a .NET/C# spyware》- dnSpy + de4dot 实战 info-stealer 脱混淆
- YouTube《dnSpy Patch .NET EXEs & DLLs》- 手把手 patch + keygen
- 看雪论坛 .NET 逆向版块; 搜 ".net 逆向" / "dnSpy" / "ConfuserEx" 有大量实战帖、Nuitka 逆向、免杀讨论
- Guided Hacking《Top 5 .NET Reverse Engineering Tools》- dnSpy 仍排第一
- StackExchange / Reverse Engineering; `DynamicMethod` 调试等进阶问题
- `reverse-engineering/tools.md` `.NET Analysis` 段; dnSpy/ILSpy 工具速查 + Codegate 2013 两阶段 XOR+AES-CBC 模式
- `reverse-engineering/field-notes.md` `.NET` 段; 工具速记
- `reverse-engineering/awesome-re-resources.md`; de4dot 入选
- `field-journal/seed-014_unity-il2cpp-reverse.md`; Unity IL2CPP（native 侧，与 .NET 托管层互补）