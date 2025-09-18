# TypeScript Guidelines
*Notes App - Type Formation & Usage Standards*

## 🎯 Core Principles

### Type-First Approach
- **Every file is TypeScript**: Use `.ts` or `.vue` with TypeScript
- **Strict mode enabled**: No `any` types without explicit justification
- **Type safety over convenience**: Prefer explicit types over inference
- **Compile-time error prevention**: Catch errors before runtime

### Quality Standards
- **Zero `any` types**: Use proper typing or `unknown`
- **Explicit return types**: Functions must declare return types
- **Interface over type**: Prefer interfaces for object shapes
- **Generic constraints**: Use generics with proper constraints

## 📋 Type Organization Structure

### File Organization
- **`src/types/index.ts`**: Main exports and re-exports
- **`src/types/note.ts`**: Note-related interfaces and types
- **`src/types/notebook.ts`**: Notebook-related interfaces
- **`src/types/user.ts`**: User and authentication types
- **`src/types/api.ts`**: API request/response types
- **`src/types/search.ts`**: Search functionality types
- **`src/types/sync.ts`**: Synchronization and offline types
- **`src/types/ui.ts`**: UI state and component types
- **`src/types/common.ts`**: Shared utility types

### Organization Rules
- **One domain per file**: Group related types by feature
- **Re-export from index**: Centralized type exports for easy imports
- **Group related types**: Keep related interfaces together
- **Use descriptive names**: `CreateNoteRequest` not `CreateRequest`

## 🏗️ Type Formation Guidelines

### Note Types
**Core entities**: `Note`, `CreateNoteRequest`, `UpdateNoteRequest`
**Supporting types**: `Attachment`, `NoteMetadata`
**Formation principles**: 
- Use `string | null` for optional foreign keys
- Include sync status for offline-first architecture
- Separate creation/update request types
- Include metadata for enhanced functionality

### Notebook Types
**Core entities**: `Notebook`, `CreateNotebookRequest`, `UpdateNotebookRequest`
**Formation principles**:
- Include visual properties (color, icon)
- Track note count for UI display
- Support optional description
- Include sync status

### User Types
**Core entities**: `User`, `UserPreferences`, `AuthState`
**Formation principles**:
- Separate user data from preferences
- Include authentication state
- Support theme and localization
- Track user activity timestamps

### API Types
**Core entities**: `ApiResponse<T>`, `ApiError`, `PaginatedResponse<T>`
**Request types**: `SearchParams`, `SearchFilters`, `SortOptions`, `PaginationOptions`
**Formation principles**:
- Generic response wrapper for consistency
- Structured error handling
- Support pagination and filtering
- Include timestamps for debugging

### Search Types
**Core entities**: `SearchResult`, `SearchHighlight`, `SearchState`
**Formation principles**:
- Include relevance scoring
- Support text highlighting
- Track search state and filters
- Include pagination support

### Sync Types
**Core entities**: `SyncItem`, `SyncState`, `ConflictResolution`
**Formation principles**:
- Track sync operations and retry logic
- Support conflict resolution strategies
- Include progress tracking
- Handle offline/online states

### UI Types
**Core entities**: `ViewMode`, `UIState`, `ModalState`
**Formation principles**:
- Separate UI state from business logic
- Support different view modes
- Include modal and dialog states
- Track loading and error states

### Common Types
**Utility types**: `ID`, `Timestamp`, `Optional<T, K>`, `RequiredFields<T, K>`
**Advanced types**: `DeepPartial<T>`, `NonNullable<T>`, `ValueOf<T>`, `KeysOfType<T, U>`
**Formation principles**:
- Provide reusable type utilities
- Support complex type transformations
- Enable type-safe operations
- Reduce code duplication

## 🎯 Component Type Guidelines

### Vue Component Props
- Define explicit interfaces for props
- Use `withDefaults` for optional props with defaults
- Avoid `any` types - use specific interfaces
- Include callback function types for events

### Component Emits
- Use typed emits with proper event signatures
- Include parameter types for each event
- Avoid string-based emit definitions
- Document event purposes clearly

### Ref Types
- Always specify ref types explicitly
- Use `ref<Type[]>([])` for arrays
- Use `ref<Type | null>(null)` for nullable refs
- Avoid implicit `any` types

### Computed Types
- Specify explicit return types for computed properties
- Use arrow functions with return type annotations
- Avoid type inference for complex computations
- Document computed property purposes

## 🏪 Store Type Guidelines

### Pinia Store Structure
- Use Composition API with `defineStore`
- Explicitly type all state variables
- Specify return types for computed properties
- Include parameter and return types for actions
- Handle errors with proper typing
- Return organized state, getters, and actions

### State Management
- Use `ref<Type[]>([])` for arrays
- Use `ref<Type | null>(null)` for nullable state
- Include loading and error states
- Separate business state from UI state

### Actions and Getters
- Use `async` functions with `Promise<ReturnType>`
- Include proper error handling with typed errors
- Use computed properties for derived state
- Document action purposes and parameters

## 🔧 Service Type Guidelines

### API Service Types
- Use class-based services with typed methods
- Include proper return types for all API calls
- Use generic types for reusable response patterns
- Handle errors with typed error responses
- Include request/response type imports

### Storage Service Types
- Define generic interfaces for storage operations
- Use `Promise<T | null>` for load operations
- Include proper error handling
- Support different data types with generics
- Document storage operation purposes

## 🎨 Composable Type Guidelines

### Composable Structure
- Use function-based composables with explicit return types
- Import store and type dependencies
- Use computed properties for reactive state
- Include proper parameter and return types for actions
- Return organized state and actions

### State and Actions
- Use computed properties for store state access
- Include proper error handling
- Use async functions with Promise return types
- Document composable purposes and usage
- Keep composables focused on single concerns

## 🛠️ Utility Type Guidelines

### Generic Utility Functions
- Use type guards with `obj is Type` return types
- Check for object existence and required properties
- Use assertion functions for runtime validation
- Document type guard purposes and usage
- Include proper error handling

### Validation Functions
- Define validation result interfaces
- Use `Partial<Type>` for partial validation
- Include specific error messages
- Use type guards for data validation
- Return structured validation results

## 🚫 Anti-Patterns to Avoid

### ❌ DON'T:
- Use `any` types without justification
- Use implicit `any` in function parameters
- Ignore type errors with `@ts-ignore`
- Use `object` instead of specific types
- Use `Function` type for callbacks
- Create empty interfaces

### ✅ DO:
- Use proper typing with specific interfaces
- Use explicit parameter types
- Handle type errors properly
- Use specific interfaces instead of `object`
- Use function signatures for callbacks
- Use type aliases for simple types

## 📋 TypeScript Checklist

### Before Writing Code:
- [ ] Define interfaces for all data structures
- [ ] Use explicit return types for functions
- [ ] Avoid `any` types - use `unknown` if needed
- [ ] Use proper generic constraints
- [ ] Define error types for error handling

### Before Committing:
- [ ] All TypeScript errors resolved
- [ ] No `any` types without justification
- [ ] All functions have explicit return types
- [ ] All interfaces are properly documented
- [ ] Type guards are implemented where needed

### Code Review:
- [ ] Types are consistent across the codebase
- [ ] No type assertions without proper validation
- [ ] Error handling is properly typed
- [ ] Generic types are used appropriately
- [ ] Type definitions are in the correct files

## 🎯 Best Practices Summary

### Type Formation
1. **Use Type Guards**: Validate data at runtime
2. **Use Discriminated Unions**: For state management
3. **Use Mapped Types**: For type transformations
4. **Use Conditional Types**: For complex type logic
5. **Use Template Literal Types**: For dynamic type generation

### Type Usage
1. **Import from centralized locations**: Use `src/types/index.ts`
2. **Use generics for reusability**: Create flexible type patterns
3. **Separate concerns**: Keep UI, business, and API types separate
4. **Document complex types**: Add comments for complex type logic
5. **Validate at boundaries**: Use type guards at API and storage boundaries

---

**Rule**: Every piece of code must be properly typed. TypeScript is not optional - it's the foundation of our codebase quality and developer experience.
