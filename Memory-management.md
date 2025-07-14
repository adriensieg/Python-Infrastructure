
## Is Memory Usage Constantly Increasing?

- **Issue**: Memory leak — objects not being released.
- **Solution**: Track references & inspect leaks.

```python
import gc
for obj in gc.get_objects():
    if isinstance(obj, MyClass):
        print(obj)  # Check if too many instances are kept
```
