# Make Your Python Use Less Memory and More CPU — Practical Rules, Decision Tree, and Ready-to-run Snippets

- **Process**: a running program with its own private address space (heap, code, page tables).
- **Thread**: a unit of execution inside a process. Threads share the process heap but have their own stack and registers.
- **Core / CPU core**: a hardware unit that executes instructions. Multiple cores = true physical parallelism.
- **Stack**: per-thread memory for local variables and function calls. Auto-cleared on return. Fast.
- **Heap**: shared memory area for dynamically allocated objects (lists, dicts, objects). Longer-lived.
- **Heap** vs **Stack** summary: stack = small, fast, per-thread; heap = shared, larger, dynamic; use locks when changing heap from multiple threads.
- **GIL (Global Interpreter Lock)**: CPython’s global lock that lets only one thread execute Python bytecode at a time inside a process. Good for safety, bad for CPU-bound threading.
- **Single-threaded process**: one thread only — simpler, no thread-safety problems, limited to one CPU core.
- **Multithreaded process**: multiple threads share memory — good for I/O, not for CPU-bound Python code (because of GIL).
- **Multiprocessing**: multiple OS processes. Each has its own GIL and memory — good for CPU-bound parallelism, higher memory cost.
- **Asyncio (async/await)**: single thread, cooperative concurrency (tasks yield control on await). Great for high-concurrency I/O with low memory/threads.

# When to use what (decision tree)?
1. **Is your task CPU-bound (heavy calculation) or I/O-bound (network, disk, DB)?**
  - <mask>**CPU-bound**</mask> → use **multiprocessing** (`ProcessPoolExecutor`) or move heavy work to `C/NumPy`.
  - <mask>**I/O-bound**</mask> → use **threads** (`ThreadPoolExecutor`) or `asyncio`.

2. **Do you need many concurrent connections but little CPU per task?**
  - Yes → prefer `asyncio` (low memory, many connections) or a small thread pool if code is synchronous.

3. **Do threads need to share complex Python objects and mutate them?**
  - **Yes** → use `Queue` for safe data passing or protect with Lock / RLock. Avoid manual shared state when possible.

4. Do you need true parallelism on many cores and can accept higher memory use?
  - **Yes** → use **multiprocessing** or run **multiple worker processes** (e.g., microservices, uWSGI/ Gunicorn workers).

5. **Are you memory constrained?**
  - Use **generators**, avoid large in-memory lists, use compact containers (array, numpy, deque), and profile (tracemalloc).

# Quick pragmatic rules (takeaways)

- **Rule 1**: If it's **I/O-bound**, use `threads` or `asyncio` — they scale without adding CPU.
- **Rule 2**: If it's **CPU-bound**, use **processes** (`ProcessPoolExecutor`) or rewrite hot paths in C/NumPy.
- **Rule 3**: Prefer `queue.Queue` to pass data between threads; it’s safe and simple.
- **Rule 4**: Avoid global mutable state — it causes races and leaks.
- **Rule 5**: Use generators and streaming to avoid building huge lists.
- **Rule 6**: Use __slots__ or namedtuple (or better: numpy structured arrays) to reduce object memory overhead for large numbers of small objects.
- **Rule 7**: Profile before optimizing: tracemalloc for Python allocations, psutil for process memory, and time the code.
- **Rule 8**: If you spawn many processes, watch total RAM — each process has its own heap; use copy-on-write-friendly patterns at startup.





