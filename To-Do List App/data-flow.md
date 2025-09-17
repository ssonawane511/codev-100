# Data Flow Rules
*Microsoft To Do App - State Management Architecture*

## 🔄 Data Flow Principles

### Single Source of Truth
All application data flows through centralized stores. Components never manage their own state for shared data.

### Unidirectional Data Flow
```
User Action → Store Action → State Update → Component Re-render
```

### Store Responsibilities
1. **Fetch data** from APIs or local storage
2. **Transform/normalize** data for consistent usage
3. **Distribute state** to components
4. **Handle side effects** (API calls, persistence)

## 🏗️ Store Architecture

### Store Structure
```typescript
// src/store/index.ts
export const useTaskStore = create<TaskStore>((set, get) => ({
  // State
  tasks: [],
  lists: [],
  currentListId: null,
  loading: false,
  error: null,
  
  // Actions
  addTask: (task) => { /* implementation */ },
  updateTask: (id, updates) => { /* implementation */ },
  deleteTask: (id) => { /* implementation */ },
  toggleTask: (id) => { /* implementation */ },
  
  // Async Actions
  fetchTasks: async () => { /* implementation */ },
  saveTasks: async () => { /* implementation */ },
}))
```

### Store Rules

#### ✅ DO:
- **Centralize state**: All shared data lives in stores
- **Pure actions**: Actions only update state, no side effects
- **Async handling**: Use separate async actions for API calls
- **Error boundaries**: Handle errors at the store level
- **Optimistic updates**: Update UI immediately, sync in background

#### ❌ DON'T:
- **Direct API calls**: Components never call APIs directly
- **Local state for shared data**: Don't duplicate store state in components
- **Side effects in actions**: Keep actions pure
- **Bypass stores**: Never access data outside the store pattern

## 📊 Data Flow Patterns

### 1. Task Creation Flow
```
User clicks "Add Task" 
→ TaskForm component calls store.addTask()
→ Store updates tasks array
→ TaskList component re-renders with new task
→ Store persists to localStorage
```

### 2. Task Completion Flow
```
User clicks checkbox
→ TaskItem calls store.toggleTask(id)
→ Store updates task.completed status
→ All task components re-render
→ Store saves changes
```

### 3. List Switching Flow
```
User selects different list
→ Sidebar calls store.setCurrentList(id)
→ Store updates currentListId
→ TaskList filters tasks by current list
→ UI updates to show filtered tasks
```

## 🗄️ Data Persistence

### Local Storage Strategy
```typescript
// Store automatically syncs with localStorage
const saveToStorage = (state: TaskState) => {
  localStorage.setItem('todo-app-data', JSON.stringify(state))
}

const loadFromStorage = (): TaskState | null => {
  const data = localStorage.getItem('todo-app-data')
  return data ? JSON.parse(data) : null
}
```

### Data Normalization
```typescript
// Normalized data structure
interface AppState {
  tasks: {
    [id: string]: Task
  }
  lists: {
    [id: string]: TaskList
  }
  ui: {
    currentListId: string | null
    selectedTaskId: string | null
    filter: 'all' | 'active' | 'completed'
  }
}
```

## 🔄 Component Data Rules

### Data Consumption
```typescript
// ✅ Good: Component consumes store data
const TaskList = () => {
  const { tasks, currentListId } = useTaskStore()
  const filteredTasks = tasks.filter(task => task.listId === currentListId)
  
  return (
    <div>
      {filteredTasks.map(task => <TaskItem key={task.id} task={task} />)}
    </div>
  )
}

// ❌ Bad: Component manages its own state
const TaskList = () => {
  const [tasks, setTasks] = useState([]) // Don't do this!
  const [loading, setLoading] = useState(false) // Don't do this!
}
```

### Action Dispatching
```typescript
// ✅ Good: Component dispatches store actions
const TaskItem = ({ task }) => {
  const { toggleTask, deleteTask } = useTaskStore()
  
  return (
    <div>
      <input 
        type="checkbox" 
        checked={task.completed}
        onChange={() => toggleTask(task.id)}
      />
      <button onClick={() => deleteTask(task.id)}>Delete</button>
    </div>
  )
}

// ❌ Bad: Component calls API directly
const TaskItem = ({ task }) => {
  const handleToggle = async () => {
    await api.updateTask(task.id, { completed: !task.completed }) // Don't do this!
  }
}
```

## 🎯 State Management Rules

### Store Organization
- **One store per domain**: `taskStore`, `authStore`, `uiStore`
- **Shared state only**: Don't store component-specific state in stores
- **Immutable updates**: Always return new state objects
- **Type safety**: Full TypeScript coverage for all state

### Component State
- **Local state for UI only**: Form inputs, modals, loading spinners
- **Derived state from stores**: Computed values, filtered lists
- **No business logic**: Components only handle presentation

### Data Fetching
- **Store handles all API calls**: Components never fetch data directly
- **Loading states**: Store manages loading/error states
- **Caching**: Store caches data to avoid unnecessary requests
- **Optimistic updates**: Update UI immediately, handle errors gracefully

## 🔄 Side Effects Management

### Async Actions Pattern
```typescript
// Store handles all side effects
const useTaskStore = create((set, get) => ({
  // Sync action
  addTask: (task) => set(state => ({
    tasks: [...state.tasks, task]
  })),
  
  // Async action
  saveTask: async (task) => {
    set({ loading: true })
    try {
      await api.saveTask(task)
      set({ loading: false })
    } catch (error) {
      set({ loading: false, error: error.message })
    }
  }
}))
```

### Error Handling
- **Store-level errors**: Global error state for API failures
- **Component-level errors**: Local error state for UI issues
- **Error boundaries**: Catch and display unexpected errors
- **Retry mechanisms**: Automatic retry for failed requests

## 📋 Data Flow Checklist

### Before Adding New State:
- [ ] Is this data shared across components?
- [ ] Does it need to persist across sessions?
- [ ] Is it derived from other state?
- [ ] Does it require API synchronization?

### Before Adding New Action:
- [ ] Does it update shared state?
- [ ] Is it a pure function (no side effects)?
- [ ] Does it handle errors appropriately?
- [ ] Is it properly typed?

---

**Rule**: All data flows through the store. Components never bypass the store pattern. If you need to share data between components, it belongs in a store.
