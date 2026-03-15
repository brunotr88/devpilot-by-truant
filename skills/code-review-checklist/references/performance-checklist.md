# Performance Review Checklist

## Algorithm Complexity
- [ ] Time complexity appropriate for data size
- [ ] No unnecessary nested loops (O(n^2) when O(n) possible)
- [ ] Sorting/searching uses efficient algorithms
- [ ] Early returns to avoid unnecessary computation

## Database
- [ ] No N+1 query patterns
- [ ] Indexes on frequently queried columns
- [ ] SELECT only needed columns (not SELECT *)
- [ ] Batch operations instead of individual queries in loops
- [ ] Connection pooling configured
- [ ] Transactions used appropriately

### N+1 Example
```python
# BAD — N+1 queries
users = User.objects.all()
for user in users:
    orders = Order.objects.filter(user=user)  # N queries!

# GOOD — Single query with join
users = User.objects.prefetch_related('orders').all()
```

## Caching
- [ ] Expensive computations cached
- [ ] Cache invalidation strategy defined
- [ ] Cache TTL appropriate
- [ ] Memory usage bounded

## Resource Management
- [ ] File handles closed (use context managers/try-finally)
- [ ] Database connections released
- [ ] Event listeners removed on cleanup
- [ ] No memory leaks (circular references, growing collections)

## Frontend (if applicable)
- [ ] No unnecessary re-renders
- [ ] Large lists virtualized
- [ ] Images optimized and lazy-loaded
- [ ] Bundle splitting for large dependencies
- [ ] Debounce/throttle on frequent events

## Concurrency
- [ ] No race conditions on shared state
- [ ] Proper locking/synchronization
- [ ] Async operations don't block main thread
- [ ] Promise/callback error handling
