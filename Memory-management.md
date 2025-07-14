# How to Manage Memory in Python?

## Is Memory Usage Constantly Increasing?

- **Issue**: Memory leak — objects not being released.
- **Solution**: Track references & inspect leaks.
- **Metaphor**: Like leaving lights on in every room and forgetting to switch them off.

```python
import gc
for obj in gc.get_objects():
    if isinstance(obj, MyClass):
        print(obj)  # Check if too many instances are kept
```

## Are You Holding Unused References?

- **Issue**: Holding references in lists, dicts, globals.
- **Solution**: Delete or dereference unused data.
- **Metaphor**: Keeping old newspapers forever even after reading them.
```python
large_list = get_big_data()
# Done using it
del large_list  # Frees memory
```

## Are You Reading Large Files or Data at Once?

- **Issue**: Loading large files into memory at once.
- **Solution**: Stream or process in chunks.
- **Metaphor**: Like reading a book one page at a time instead of all at once.


```mermaid
flowchart TD
    A[Start\nCheck if memory keeps increasing?]
    A -->|Yes| B1[Memory Leak<br>Use gc, objgraph<br>del unused references]
    A -->|No| B2[No Leak<br>Check for large object usage]

    B2 --> C1[Are large files/data loaded at once?]
    C1 -->|Yes| D1[Stream data / read in chunks<br>Use generators]
    C1 -->|No| D2[Check DataFrame memory use]

    D2 --> E1[Optimize Pandas<br>Correct dtypes, chunk loading]
    E1 --> E2[Is it a long-running app? (e.g. Flask, Celery)]

    E2 -->|Yes| F1[Use context managers<br>Flask teardown handlers]
    E2 -->|No| F2[Using C extensions?]

    F2 -->|Yes| G1[Track native memory<br>valgrind, ctypes cleanup]
    F2 -->|No| G2[Still unclear?\nProfile memory]

    G2 --> H1[Use memory_profiler<br>tracemalloc, psutil]
    H1 --> H2[Manual GC control]

    H2 --> I1[gc.collect()<br>gc.set_threshold()]
    H2 --> I2[Del big objects early<br>to release memory faster]
```










- `Memory Management in Python`: https://www.honeybadger.io/blog/memory-management-in-python/
- https://coderzcolumn.com/blogs/python/memory-management-in-python-garbage-collection
- `How Python’s Memory Management Works`: https://medium.com/@AlexanderObregon/how-pythons-memory-management-works-f832405ea3a3
- Python Memory Management - Memory Allocation And Garbage Collection: https://learnbatta.com/blog/python-memory-allocation-and-garbage-collection-10/
- Python Memory Management: https://medium.datadriveninvestor.com/python-memory-management-3cc903fd4fc1
- Memory Management in Python: https://realpython.com/python-memory-management/
