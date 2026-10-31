// ✅; single cleanup label
**Use `sizeof *ptr` instead of `sizeof(Type)`; it stays correct when the type changes.**
// ✅; pass size alongside pointer, or use a struct
// ✅; define or use named error codes
// ✅; early return / goto cleanup
**`goto cleanup` is idiomatic C for resource teardown; don't avoid it out of principle.**
// ✅; track write position
**Prefer `snprintf` over `sprintf`; always.**