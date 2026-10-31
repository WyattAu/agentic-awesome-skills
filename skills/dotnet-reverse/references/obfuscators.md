| Babel.NET | `babel` | 方法体加密、控制流、字符串 | 取决于版本与保护配置 |; |
| Themida .NET |; | 外壳 + 虚拟化 | ❌ de4dot 不行 | dump 内存，走 native 思路 |
| Agile.NET / CliSecure | `agile` | 方法体加密 | 取决于版本与保护配置 |; |
方法 A; dnSpyEx 直接 patch 校验函数：
方法 B; 运行时 dump：
| `Debugger.IsLogging` |; | 同上 |
| `CheckRemoteDebuggerPresent` P/Invoke |; | nop 掉调用 |
> .NET anti-debug 比 native 简单 -- 多数是托管 API 调用，dnSpyEx IL 改一行即可。