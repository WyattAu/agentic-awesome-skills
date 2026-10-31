2. **Poll with appropriate delays**; 500ms recommended between status checks
3. **Clean up resources**; Delete threads and agents when done
4. **Handle all run statuses**; Check for `RequiresAction`, `Failed`, `Cancelled`
5. **Use streaming for real-time UX**; Better user experience than polling
6. **Store IDs not objects**; Reference agents/threads by ID
7. **Use async methods**; All operations should be async