  Step 1：尝试脱壳（自动脱壳机 / 手动找 OEP）- 须在授权与隔离环境
4. **保底**：`ExitProcess` / 进程退出路径断点（补丁 10）- 一旦因反调试直接退出，**不急着重启**；立即 dump memory，将崩溃前镜像路径写入 Evidence，供字符串/数据恢复
Blindspots (Rust/Go/VMP/injection/OLE/PDF/agent-meta): upstream analysis-blindspot-cookbook.md (not bundled) R52-R81; detection-oriented; not a parallel master flow.