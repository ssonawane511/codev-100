# TypeScript Rules
*Microsoft To Do App - Type Safety & Contracts*

## 🎯 TypeScript First Philosophy

**Rule**: No API work until data contracts are in place. Types are the guardrails that prevent runtime errors and improve developer experience.

## 📋 Type Organization

### File Structure
```
src/types/
├── index.ts          # Re-exports all types
├── task.ts           # Task-related types
├── user.ts           # User and auth types
├── list.ts           # Task list types
├── api.ts            # API response types
├── ui.ts             # UI component types
└── common.ts         # Shared utility types
```

### Type Naming Conventions
```typescript
// ✅ Good: Clear, descriptive names
interface Task {
  id: string
  title: string
  completed: boolean
  createdAt: Date
  dueDate?: Date
}

interface TaskList {
  id: string
  name: string
  tasks: Task[]
  createdAt: Date
}

// ❌ Bad: Vague or unclear names
interface Data {
  id: string
  name: string
}

interface Item {
  id: string
  title: string
}
```

## 🏗️ Core Data Types

### Task Types
```typescript
// src/types/task.ts
export interface Task {
  id: string
  title: string
  description?: string
  completed: boolean
  priority: TaskPriority
  dueDate?: Date
  createdAt: Date
  updatedAt: Date
  listId: string
  subtasks?: Task[]
  tags?: string[]
}

export type TaskPriority = 'low' | 'medium' | 'high'

export interface CreateTaskRequest {
  title: string
  description?: string
  priority?: TaskPriority
  dueDate?: Date
  listId: string
  tags?: string[]
}

export interface UpdateTaskRequest {
  id: string
  title?: string
  description?: string
  completed?: boolean
  priority?: TaskPriority
  dueDate?: Date
  tags?: string[]
}

export interface TaskFilters {
  completed?: boolean
  priority?: TaskPriority
  dueDate?: {
    from?: Date
    to?: Date
  }
  tags?: string[]
  search?: string
}
```

### List Types
```typescript
// src/types/list.ts
export interface TaskList {
  id: string
  name: string
  description?: string
  color?: string
  icon?: string
  taskCount: number
  completedCount: number
  createdAt: Date
  updatedAt: Date
  userId: string
}

export interface CreateListRequest {
  name: string
  description?: string
  color?: string
  icon?: string
}

export interface UpdateListRequest {
  id: string
  name?: string
  description?: string
  color?: string
  icon?: string
}
```

### User Types
```typescript
// src/types/user.ts
export interface User {
  id: string
  email: string
  name: string
  avatar?: string
  preferences: UserPreferences
  createdAt: Date
  lastLoginAt?: Date
}

export interface UserPreferences {
  theme: 'light' | 'dark' | 'system'
  defaultListId?: string
  showCompletedTasks: boolean
  taskView: 'list' | 'board'
  notifications: {
    dueDateReminders: boolean
    dailyDigest: boolean
  }
}

export interface AuthState {
  user: User | null
  isAuthenticated: boolean
  isLoading: boolean
  error: string | null
}
```

### API Types
```typescript
// src/types/api.ts
export interface ApiResponse<T> {
  data: T
  success: boolean
  message?: string
  errors?: string[]
}

export interface PaginatedResponse<T> {
  data: T[]
  pagination: {
    page: number
    limit: number
    total: number
    totalPages: number
  }
}

export interface ApiError {
  message: string
  code: string
  details?: Record<string, any>
}

// API Endpoint Types
export type GetTasksResponse = ApiResponse<Task[]>
export type CreateTaskResponse = ApiResponse<Task>
export type UpdateTaskResponse = ApiResponse<Task>
export type DeleteTaskResponse = ApiResponse<{ id: string }>

export type GetListsResponse = ApiResponse<TaskList[]>
export type CreateListResponse = ApiResponse<TaskList>
export type UpdateListResponse = ApiResponse<TaskList>
export type DeleteListResponse = ApiResponse<{ id: string }>
```

## 🎯 Component Type Rules

### Props Types
```typescript
// ✅ Good: Explicit, well-documented props
interface TaskItemProps {
  task: Task
  onToggle: (id: string) => void
  onEdit: (id: string) => void
  onDelete: (id: string) => void
  isSelected?: boolean
  className?: string
}

// ❌ Bad: Vague or any types
interface TaskItemProps {
  task: any
  onToggle: Function
  onEdit: Function
  onDelete: Function
}
```

### Event Handler Types
```typescript
// ✅ Good: Specific event types
interface TaskFormProps {
  onSubmit: (task: CreateTaskRequest) => void
  onCancel: () => void
  initialData?: Partial<CreateTaskRequest>
}

// Event handler type definitions
type TaskFormSubmitHandler = (task: CreateTaskRequest) => void
type TaskFormCancelHandler = () => void
type TaskToggleHandler = (id: string) => void
```

### Generic Types
```typescript
// ✅ Good: Reusable generic types
interface ApiHookResult<T> {
  data: T | null
  loading: boolean
  error: string | null
  refetch: () => Promise<void>
}

interface PaginatedHookResult<T> extends ApiHookResult<T[]> {
  pagination: {
    page: number
    totalPages: number
    hasNext: boolean
    hasPrev: boolean
  }
  loadMore: () => void
}
```

## 🔧 Utility Types

### Common Utility Types
```typescript
// src/types/common.ts
export type Optional<T, K extends keyof T> = Omit<T, K> & Partial<Pick<T, K>>
export type RequiredFields<T, K extends keyof T> = T & Required<Pick<T, K>>
export type PartialExcept<T, K extends keyof T> = Partial<T> & Pick<T, K>

// Specific utility types
export type TaskUpdate = PartialExcept<UpdateTaskRequest, 'id'>
export type ListUpdate = PartialExcept<UpdateListRequest, 'id'>

// Union types for better type safety
export type TaskStatus = 'pending' | 'in-progress' | 'completed' | 'cancelled'
export type SortOrder = 'asc' | 'desc'
export type SortField = 'title' | 'dueDate' | 'priority' | 'createdAt'

// Form types
export type FormState<T> = {
  data: T
  errors: Partial<Record<keyof T, string>>
  isSubmitting: boolean
  isValid: boolean
}
```

## 🎯 Type Safety Rules

### Strict Type Checking
```typescript
// ✅ Good: Strict typing
const handleTaskUpdate = (id: string, updates: TaskUpdate): void => {
  // TypeScript ensures updates only contains valid Task fields
  taskStore.updateTask(id, updates)
}

// ❌ Bad: Loose typing
const handleTaskUpdate = (id: any, updates: any): any => {
  // No type safety, potential runtime errors
  taskStore.updateTask(id, updates)
}
```

### Null Safety
```typescript
// ✅ Good: Explicit null handling
const TaskItem = ({ task }: { task: Task }) => {
  const dueDate = task.dueDate ? formatDate(task.dueDate) : null
  
  return (
    <div>
      <h3>{task.title}</h3>
      {dueDate && <span>Due: {dueDate}</span>}
    </div>
  )
}

// ❌ Bad: Assumptions about data
const TaskItem = ({ task }: { task: Task }) => {
  return (
    <div>
      <h3>{task.title}</h3>
      <span>Due: {task.dueDate.toLocaleDateString()}</span> // Potential runtime error
    </div>
  )
}
```

### API Response Types
```typescript
// ✅ Good: Typed API responses
const fetchTasks = async (): Promise<Task[]> => {
  const response = await api.get<GetTasksResponse>('/tasks')
  return response.data
}

// ❌ Bad: Untyped API responses
const fetchTasks = async () => {
  const response = await api.get('/tasks')
  return response.data // Type is 'any'
}
```

## 🔄 Store Type Rules

### Store State Types
```typescript
// Store state must be fully typed
interface TaskStoreState {
  tasks: Task[]
  lists: TaskList[]
  currentListId: string | null
  selectedTaskId: string | null
  filters: TaskFilters
  loading: boolean
  error: string | null
}

interface TaskStoreActions {
  // Sync actions
  addTask: (task: CreateTaskRequest) => void
  updateTask: (id: string, updates: TaskUpdate) => void
  deleteTask: (id: string) => void
  toggleTask: (id: string) => void
  
  // Async actions
  fetchTasks: () => Promise<void>
  saveTask: (task: Task) => Promise<void>
  
  // UI actions
  setCurrentList: (id: string | null) => void
  setSelectedTask: (id: string | null) => void
  setFilters: (filters: TaskFilters) => void
}

type TaskStore = TaskStoreState & TaskStoreActions
```

## 📋 TypeScript Checklist

### Before Writing Code:
- [ ] Are all data structures typed?
- [ ] Are function parameters and return types defined?
- [ ] Are API responses typed?
- [ ] Are component props interfaces created?
- [ ] Are error types defined?

### Code Review Checklist:
- [ ] No `any` types (except for external libraries)
- [ ] All functions have return type annotations
- [ ] All variables have explicit types where needed
- [ ] Generic types are used appropriately
- [ ] Union types are used for mutually exclusive values

### Type Safety Rules:
- **No `any` types**: Use `unknown` or specific types
- **Strict null checks**: Handle null/undefined explicitly
- **Explicit return types**: Functions should declare return types
- **Generic constraints**: Use generic constraints when appropriate
- **Discriminated unions**: Use for type-safe state management

---

**Rule**: Every piece of data has a type. Every function has typed parameters and return values. No `any` types except for external library integrations.
