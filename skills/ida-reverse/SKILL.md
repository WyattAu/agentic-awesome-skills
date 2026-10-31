   - **禁止** `taskkill /F /T` 杀进程树--`/T` 会把 GUI `ida.exe` 子进程一起干掉
### start.ps1: 启动 MCP HTTP 服务器
### watchdog.ps1 / install-autostart.ps1: 保活
### open.ps1: 打开二进制文件
- `idapro_survey_binary(detail_level="minimal")`; 快速概况：函数数、字符串、段、入口点、导入分类（加密/网络/文件IO）
- `idapro_list_funcs(queries)`; 列出函数（分页、按名称过滤）
- `idapro_list_globals(queries)`; 列出全局变量
- `idapro_entity_query(kind, filter)`; 统一查询：functions/globals/imports/strings/names
- `idapro_decompile(addr)`; 反编译为伪代码
- `idapro_disasm(addr, max_instructions=N)`; 反汇编
- `idapro_analyze_function(addr, include_asm=false)`; 综合分析（伪代码+字符串+常量+调用者+被调用者+块）
- `idapro_func_profile(queries)`; 函数概要指标
- `idapro_xrefs_to(addrs)`; 查谁引用目标地址
- `idapro_xref_query(addr, direction)`; 高级 xref 查询（方向/类型过滤）
- `idapro_callees(addrs)`; 子函数列表
- `idapro_callgraph(roots, max_depth)`; 调用图
- `idapro_trace_data_flow(addr, direction, max_depth)`; 数据流追踪（forward/backward）
- `idapro_find_regex(pattern, limit)`; 正则搜字符串
- `idapro_search_text(pattern)`; 在反汇编列表中搜文本
- `idapro_find_bytes(patterns, limit)`; 字节模式搜索（支持 ?? 通配符）
- `idapro_find(type, targets)`; 高级搜索（立即数/字符串/引用）
- `idapro_get_bytes(addrs)`; 读原始字节
- `idapro_get_string(addrs)`; 读字符串
- `idapro_get_int(queries)`; 读整数值
- `idapro_get_global_value(queries)`; 读全局变量值
- `idapro_read_struct(queries)`; 读结构体字段值
- `idapro_search_structs(filter)`; 搜索结构体
- `idapro_set_comments(items)`; 添加注释（反汇编+反编译双向同步）
- `idapro_append_comments(items)`; 追加注释
- `idapro_rename(batch)`; 批量重命名（函数/全局/局部/栈变量）
- `idapro_patch_asm(items)`; Patch 汇编指令
- `idapro_patch(patches)`; Patch 字节
- `idapro_define_func(items)`; 定义函数
- `idapro_undefine(items)`; 取消定义
- `idapro_define_code(items)`; 将字节转为代码
- `idapro_declare_type(decls)`; 声明 C 结构体/枚举/联合体
- `idapro_set_type(edits)`; 应用类型到函数/全局/局部
- `idapro_infer_types(addrs)`; 推断类型
- `idapro_type_query(queries)`; 查询已声明类型
- `idapro_type_inspect(queries)`; 查看类型详情
- `idapro_stack_frame(addrs)`; 查看栈帧变量
- `idapro_declare_stack(items)`; 声明栈变量
- `idapro_delete_stack(items)`; 删除栈变量
- `idapro_make_signature(addrs)`; 为地址生成唯一字节签名
- `idapro_make_signature_for_function(addrs)`; 为函数生成签名
- `idapro_find_xref_signatures(addrs)`; 为引用地址的代码生成签名
- `idapro_open_file(file_path)`; 在 GUI IDA 实例中打开文件
- `idapro_idb_open` / HTTP `idb_open`; ⚠️ 建议用 `open.ps1` 打开
- `idapro_idb_list` / HTTP `idb_list`; 列出所有 session
- `idapro_idb_save` / HTTP `idb_save`; 保存数据库
- `idapro_int_convert(inputs)`; 进制转换（**必须用这个，不要自己算进制！**）
- `idapro_export_funcs(addrs, format)`; 导出函数（json/c_header/prototypes）
- `idapro_py_eval(code)`; 在 IDA 上下文执行 Python
- `idapro_server_health()`; 服务器健康检查
- `idapro_server_warmup()`; 预热子系统（字符串缓存、Hex-Rays 等）
**路径 A; Headless idalib（需要有效 license）**
**路径 B; GUI + 插件（idalib license 失败或需要交互分析时）**
- **导入分类（MUST）**：加密函数 / 网络 API / 文件操作 / 进程注入 / 注册表; 必须落成 Evidence（建议 id：`E-imports`），可用 `idapro_entity_query(kind="imports")` 或 survey 输出中的 imports 段
1. **不要手动算进制**; 任何时候需要转换数字，用 `idapro_int_convert`
2. **先 survey 后深入**; 先看概况再针对性分析
3. **持续加注释和重命名**; 分析过程中不断更新函数名和变量名，提升后续分析的准确性
4. **跟踪交叉引用**; 发现有趣的数据/字符串，用 `xrefs_to` 看谁引用了它
5. **遇到混淆代码**; 先做字符串解密、导入哈希去除、控制流平坦化去除等预处理
6. **C++ STL 代码**; 用 FLIRT/Lumina 识别库函数后，再分析业务逻辑
7. **不要暴力破解**; 分析应从反汇编中推导解决方案，用简单 Python 辅助计算
8. **遇到 "No database bound"**; 还没有打开任何二进制文件，先执行 `open.ps1`
9. **遇到 "Failed to open database"**; 可能是旧数据库文件被锁，`open.ps1` 会自动降级到 Temp 副本（输出含 `(temp copy)` 标记）
10. **带自动分析打开 GUI/复杂样本时**; 默认加 `-TimeoutSeconds 600`，不要把长时间 `INFO:opening:...` 误判成脚本卡死