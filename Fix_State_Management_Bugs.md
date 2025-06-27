# Fix State Management Bugs

## Common State Management Issues Encountered

- Inconsistent UI updates due to improper LiveData or StateFlow usage.
- Memory leaks from improper lifecycle management.
- Data races or lost updates in ViewModel or Repository layers.
- State not surviving configuration changes (e.g., rotation).

## Solutions and Approaches

- Adopted ViewModel and LiveData/StateFlow for lifecycle-aware state management.
- Used `SavedStateHandle` to persist state across configuration changes.
- Ensured all UI state is derived from a single source of truth in ViewModel.
- Applied proper scoping for coroutines to avoid memory leaks.

## Code Examples

### Before: Direct State Mutation in Activity
```kotlin
// In Activity
var userName: String = ""

fun updateUserName(newName: String) {
    userName = newName
    // No notification to UI
}
```

### After: Using ViewModel and LiveData
```kotlin
// In ViewModel
private val _userName = MutableLiveData<String>()
val userName: LiveData<String> = _userName

fun updateUserName(newName: String) {
    _userName.value = newName
}

// In Activity/Fragment, observe userName LiveData
```

### Before: Coroutine Launched in Activity Scope
```kotlin
lifecycleScope.launch {
    // Long-running task
}
// May leak if not cancelled properly
```

### After: Coroutine Scoped to ViewModel
```kotlin
viewModelScope.launch {
    // Long-running task, auto-cancelled with ViewModel
}
``` 