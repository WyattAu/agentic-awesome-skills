# Native Android: Java Reference
> **Java + Kotlin interop is seamless**; you can have both in the same project. New files can be Kotlin while legacy files stay Java.
        binding = null; // CRITICAL; avoid memory leak
    List<ItemEntity> getAll(); // blocking; call off main thread
    // Observe cached data (returns LiveData; auto updates UI)
<!-- Use ConstraintLayout; flat hierarchy = better performance -->
// Activity / Fragment; annotate for injection
1. **New files in Kotlin**; Java and Kotlin coexist seamlessly
2. **Convert utilities first**; `@JvmStatic`, `@JvmField` for interop
3. **Convert data models**; Java POJOs → Kotlin `data class`
4. **Convert DAOs and Repositories**; add `suspend` + `Flow`
5. **Convert ViewModels last**; swap `LiveData` + `MutableLiveData` for `StateFlow`
6. **Convert Activities/Fragments**; migrate to Compose screen by screen