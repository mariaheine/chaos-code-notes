
### ObserveOn & synchronization context

- `SynchronizationContext.Current` is **only non-null on the main thread**
- so in Unity you might want to capture it once on awake
```
private SynchronizationContext _mainThreadContext;

void Awake()
{
    _mainThreadContext = SynchronizationContext.Current;
}
```