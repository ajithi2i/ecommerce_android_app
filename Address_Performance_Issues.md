# Address Performance Issues

## Performance Bottlenecks Identified

- Slow RecyclerView rendering with large datasets.
- Unoptimized image loading causing UI jank.
- Blocking network calls on the main thread.
- Inefficient database queries.

## Diagnostic Methods

- Used Android Profiler to monitor CPU, memory, and network usage.
- Analyzed logs for slow operations and ANRs.
- Employed StrictMode to catch accidental disk/network access on the main thread.
- Profiled database queries with Room's built-in logging.

## Optimizations Implemented

- Implemented DiffUtil in RecyclerView adapters to minimize UI updates.
- Integrated Glide/Picasso for efficient image loading and caching.
- Moved all network/database operations to background threads using coroutines.
- Optimized SQL queries and added proper indexing.

## Impact

- Reduced UI frame drops and improved scroll smoothness.
- Faster image loading and lower memory usage.
- No more ANRs from main-thread blocking.
- Database operations are now significantly faster.

## Example: Using DiffUtil in RecyclerView
```kotlin
val diffCallback = object : DiffUtil.ItemCallback<Item>() {
    override fun areItemsTheSame(oldItem: Item, newItem: Item) = oldItem.id == newItem.id
    override fun areContentsTheSame(oldItem: Item, newItem: Item) = oldItem == newItem
}
val adapter = ListAdapter(diffCallback)
``` 