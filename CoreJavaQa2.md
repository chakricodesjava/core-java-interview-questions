SECTION 2: JVM, MEMORY & CLASS LOADING (Q21–Q40) — Slide Outline

Q21. JVM Memory Areas
- Key: Heap, Stack, Metaspace, Native, PC register
- Visual: Threads → Stacks → Heap (Young/Old); Classes → Metaspace
- Demo: String s = new String("x"); jcmd <pid> GC.heap_info

Q22. Method Area vs Heap
- Key: Metaspace = class metadata; Heap = instances/arrays
- Pitfall: Metaspace OOM ≠ -Xmx; tune -XX:MaxMetaspaceSize
- Demo: new User(); String.class.getClassLoader() == null

Q23. Stack vs Heap
- Key: Stack = frames + locals; Heap = shared objects
- Pitfalls: Only references on stack; recursion → StackOverflowError
- Demo: recursive method; list adding big byte[] for heap OOM

Q24. ClassLoader Types
- Key: Bootstrap → Platform → Application (parent-first)
- Pitfall: Breaking delegation shadows core classes
- Demo: print class loaders for String, java.sql.Date, YourClass

Q25. Parent Delegation Model
- Key: loadClass → parent first → findClass
- Benefit: Avoid spoofing java.lang.String, fewer linkage errors
- Demo: Child-first loader snippet (educational only)

Q26. ClassNotFoundException vs NoClassDefFoundError
- Key: CNFE (checked, reflection); NCDFE (runtime linkage/missing dep)
- Tip: Read cause for missing dependent type
- Demo: Class.forName vs removing a JAR at runtime

Q27. Reducing Metaspace Issues
- Key: Fix classloader leaks; tune metaspace; curb dynamic proxies
- Pitfall: Not closing URLClassLoader; ThreadLocal leaks
- Demo: try/finally cl.close(); jcmd GC.class_stats

Q28. Thread Context ClassLoader (TCCL)
- Key: Library loads app classes via TCCL (ServiceLoader/JNDI/JDBC)
- Pitfall: Not restoring TCCL in thread pools
- Demo: set/restore TCCL around ServiceLoader.load

Q29. Loading vs Linking vs Initialization
- Key: Loading (bytes) → Linking (verify/prepare/resolve) → <clinit>
- Pitfall: Circular static init → default 0/null values
- Demo: A/B static field order surprise

Q30. Custom ClassLoader Safely
- Key: Prefer URLClassLoader; keep parent-first; close resources
- Pitfall: Don’t define java.*; avoid package-split across loaders
- Demo: defineClass in findClass; explain policy

Q31. GC Concept
- Key: Roots → mark live → copy/compact → sweep
- Pitfall: STW pauses still happen; use concurrent collectors
- Demo: -Xlog:gc* and reachability example

Q32. Young vs Old Generation
- Key: Eden + Survivors; aging then promotion to Old
- Pitfall: Large-object path varies by GC/flags
- Demo: short-lived allocation loop; TLAB mention

Q33. Minor vs Major GC
- Key: Minor = Young; Major = Old; Full = whole heap
- Pitfall: Frequent Full GC → pressure/mis-sizing
- Demo: G1 with -Xlog:gc; watch minor cycles

Q34. GC Collectors Overview
- Serial, Parallel, G1, ZGC, Shenandoah — choose by pause vs throughput
- Pitfall: Flags differ per GC; don’t cargo-cult
- Demo: show flags; when to pick each

Q35. Safepoints & STW Pauses
- Key: VM work when all threads at safepoint
- Pitfall: Long safepoint sync due to tight loops/native
- Demo: -Xlog:safepoint; mitigation tips

Q36. finalize() Deprecated
- Key: Unreliable; prefer AutoCloseable/Cleaner
- Pitfall: Business logic in finalize
- Demo: try-with-resources; Cleaner.register

Q37. Soft vs Weak vs Phantom
- Soft = cache; Weak = auto-evict; Phantom = post-mortem
- Pitfall: Expecting Soft refs to be permanent
- Demo: WeakHashMap clearing; Phantom + ReferenceQueue

Q38. Memory Leak in Java?
- Key: Lingering refs in static/caches/listeners/ThreadLocal
- Pitfall: Pooled threads + ThreadLocal without remove
- Demo: listener leak; TL.remove() in finally

Q39. Tools for Memory Problems
- Live: jcmd, jstat, JFR; Dumps: jcmd GC.heap_dump; Analyze: MAT/JMC
- Flow: Reproduce → Dump → Dominators → Fix
- Pitfall: Huge dumps; prefer targeted/live first

Q40. Types of OutOfMemoryError
- Heap space; GC overhead; Metaspace; Direct buffer
- Pitfall: StackOverflowError ≠ OOME
- Demo: controlled OOM patterns (never in prod)

Extras for video:
- Start with 5–10s hook; draw quick diagrams; show a short demo; end with a practical tip.

