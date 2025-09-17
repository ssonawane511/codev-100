# Project Structure Rules
*Microsoft To Do App - Project Skeleton*

## 🗂️ Directory Structure

The project follows a clean, scalable structure that separates concerns and makes the codebase maintainable:

```
src/
├── pages/           # Route-level screens
│   ├── Dashboard.tsx
│   ├── TaskList.tsx
│   ├── TaskDetail.tsx
│   ├── Settings.tsx
│   └── Login.tsx
├── components/      # Reusable UI blocks
│   ├── ui/          # Basic UI components
│   │   ├── Button.tsx
│   │   ├── Input.tsx
│   │   ├── Modal.tsx
│   │   ├── Checkbox.tsx
│   │   └── Dropdown.tsx
│   ├── task/        # Task-specific components
│   │   ├── TaskItem.tsx
│   │   ├── TaskForm.tsx
│   │   ├── TaskFilter.tsx
│   │   └── TaskList.tsx
│   └── layout/      # Layout components
│       ├── Header.tsx
│       ├── Sidebar.tsx
│       └── Footer.tsx
├── assets/          # Static files
│   ├── images/
│   ├── icons/
│   └── fonts/
├── hooks/           # Reusable logic
│   ├── useTasks.ts
│   ├── useAuth.ts
│   ├── useLocalStorage.ts
│   └── useDebounce.ts
├── context/         # Global providers
│   ├── AuthContext.tsx
│   ├── ThemeContext.tsx
│   └── TaskContext.tsx
├── utils/           # Helper functions
│   ├── dateHelpers.ts
│   ├── validation.ts
│   ├── storage.ts
│   └── constants.ts
├── store/           # State management
│   ├── taskStore.ts
│   ├── authStore.ts
│   └── index.ts
├── types/           # TypeScript contracts
│   ├── task.ts
│   ├── user.ts
│   ├── api.ts
│   └── index.ts
└── styles/          # Global styles
    ├── globals.css
    ├── theme.css
    └── components/
```

## 📋 Structure Rules

### ✅ DO:
- **Group by feature**: Task-related components go in `components/task/`
- **Separate concerns**: UI components vs business logic vs utilities
- **Use descriptive names**: `TaskItem.tsx` not `Item.tsx`
- **Keep it flat**: Avoid deeply nested folders (max 3 levels)
- **Consistent naming**: PascalCase for components, camelCase for utilities

### ❌ DON'T:
- **Dump random files**: No `miscStuff.js` or `temp.ts`
- **Mix concerns**: Don't put API calls in UI components
- **Create unnecessary folders**: Don't make `components/ui/buttons/` for one button
- **Use generic names**: No `Component1.tsx` or `utils.ts`

## 🎯 File Placement Guidelines

| File Type | Location | Example |
|-----------|----------|---------|
| Page components | `src/pages/` | `Dashboard.tsx` |
| Reusable UI | `src/components/ui/` | `Button.tsx` |
| Feature components | `src/components/[feature]/` | `TaskItem.tsx` |
| Custom hooks | `src/hooks/` | `useTasks.ts` |
| Context providers | `src/context/` | `AuthContext.tsx` |
| Utility functions | `src/utils/` | `dateHelpers.ts` |
| State management | `src/store/` | `taskStore.ts` |
| Type definitions | `src/types/` | `task.ts` |
| Static assets | `src/assets/` | `icons/check.svg` |

## 🔄 Import Rules

```typescript
// ✅ Good: Clear, organized imports
import { TaskItem } from '@/components/task/TaskItem'
import { useTasks } from '@/hooks/useTasks'
import { Task } from '@/types/task'

// ❌ Bad: Messy, unclear imports
import { TaskItem } from '../../../components/task/TaskItem'
import { useTasks } from '../../hooks/useTasks'
```

## 📁 Directory Responsibilities

- **`pages/`**: Route-level screens that users navigate to
- **`components/`**: Reusable UI building blocks
- **`hooks/`**: Custom React hooks for shared logic
- **`context/`**: React Context providers for global state
- **`utils/`**: Pure functions with no React dependencies
- **`store/`**: Centralized state management
- **`types/`**: TypeScript type definitions
- **`assets/`**: Static files (images, icons, fonts)

---

**Rule**: Every new file must fit into this structure. If it doesn't, the structure needs updating, not the file placement.
