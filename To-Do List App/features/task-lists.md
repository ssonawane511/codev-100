# Task Lists Feature PRD
*Organize tasks into separate lists for better organization*

## 📋 Feature Overview

**What it does**: Create, manage, and organize tasks into separate lists for better categorization and focus.

**User Story**: "As a user, I want to organize my tasks into different lists so I can separate work, personal, and project tasks."

**Navigation**: Dashboard → Sidebar → List Management

## 🎯 User Stories

### Primary User Stories
1. **Create List**: "As a user, I want to create new task lists so I can organize my tasks by category."
2. **Rename List**: "As a user, I want to rename my lists so I can keep them organized and clear."
3. **Delete List**: "As a user, I want to delete lists I no longer need so I can keep my workspace clean."
4. **Switch Lists**: "As a user, I want to easily switch between lists so I can focus on different areas."

### Secondary User Stories
1. **List Customization**: "As a user, I want to customize list appearance so I can visually distinguish them."
2. **List Statistics**: "As a user, I want to see task counts for each list so I can understand my workload."
3. **Default List**: "As a user, I want to set a default list so new tasks go to the right place."

## 🎨 UI/UX Requirements

### List Sidebar
- **List Display**: Vertical list of all user lists
- **Active List**: Highlighted current list
- **Task Counts**: Show total and completed task counts
- **List Icons**: Optional icons for visual identification
- **List Colors**: Optional color coding for lists

### List Management
- **Create Button**: Prominent "+" button to add new list
- **Inline Editing**: Click list name to rename
- **Context Menu**: Right-click for list options
- **Drag & Drop**: Reorder lists by dragging

### List Header
- **List Name**: Current list name with edit option
- **Task Count**: "X of Y tasks completed"
- **List Actions**: Settings, rename, delete options
- **Breadcrumb**: Show current list in navigation

## 🔧 Technical Requirements

### Data Model
```typescript
interface TaskList {
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
  isDefault: boolean
  sortOrder: number
}

interface CreateListRequest {
  name: string
  description?: string
  color?: string
  icon?: string
}
```

### API Endpoints
- `GET /api/lists` - Get all user lists
- `POST /api/lists` - Create new list
- `PUT /api/lists/:id` - Update list
- `DELETE /api/lists/:id` - Delete list
- `PATCH /api/lists/:id/reorder` - Reorder lists
- `PATCH /api/lists/:id/set-default` - Set as default list

### Store Actions
```typescript
interface ListStoreActions {
  addList: (list: CreateListRequest) => void
  updateList: (id: string, updates: UpdateListRequest) => void
  deleteList: (id: string) => void
  setCurrentList: (id: string | null) => void
  reorderLists: (listIds: string[]) => void
  setDefaultList: (id: string) => void
}
```

## ✅ Acceptance Criteria

### List Creation
- [ ] User can create a new list with a name
- [ ] New list appears in sidebar immediately
- [ ] List name validation prevents empty names
- [ ] New list becomes current list automatically
- [ ] List gets assigned unique ID and timestamps

### List Management
- [ ] User can rename lists by clicking on name
- [ ] User can delete lists with confirmation
- [ ] User can reorder lists by dragging
- [ ] List changes persist across sessions
- [ ] Deleted list tasks are moved to default list

### List Switching
- [ ] User can click list to switch to it
- [ ] Current list is visually highlighted
- [ ] Task list updates when switching lists
- [ ] URL updates to reflect current list
- [ ] Browser back/forward works with lists

### List Customization
- [ ] User can set list colors from predefined palette
- [ ] User can choose list icons from icon library
- [ ] User can add descriptions to lists
- [ ] Customizations are saved and persisted
- [ ] Visual changes are immediately visible

## 🧪 Test Cases

### Happy Path Tests
1. **Create List**: Click "+" → Enter name → Press Enter → List appears
2. **Switch List**: Click different list → Tasks update → URL changes
3. **Rename List**: Click name → Edit text → Press Enter → Name updates
4. **Delete List**: Right-click → Delete → Confirm → List removed

### Edge Cases
1. **Duplicate Names**: Try to create list with existing name → Validation error
2. **Empty Name**: Try to create list with empty name → Validation error
3. **Very Long Name**: Enter 100+ character name → Truncation or scroll
4. **Last List**: Try to delete only remaining list → Prevention or warning

### Error Cases
1. **Network Failure**: Try to create list offline → Graceful degradation
2. **Invalid Data**: Send malformed list data → Error handling
3. **Concurrent Edits**: Two users edit same list → Conflict resolution

## 📊 Success Metrics

### User Engagement
- **List Creation**: Average 2-3 lists per user
- **List Usage**: 80%+ of users have multiple lists
- **List Switching**: Average 5+ list switches per session
- **List Customization**: 60%+ of users customize list appearance

### Technical Performance
- **Response Time**: < 150ms for list operations
- **Error Rate**: < 0.5% of list operations fail
- **Data Consistency**: 100% list data integrity
- **Sync Performance**: < 500ms for list synchronization

## 🎨 Design Specifications

### List Sidebar Layout
```
┌─────────────────┐
│ 📋 My Lists     │
├─────────────────┤
│ 🏠 Personal     │ 3/5
│ 💼 Work         │ 1/8
│ 🎯 Projects     │ 0/2
│ 📚 Learning     │ 2/3
├─────────────────┤
│ + Add List      │
└─────────────────┘
```

### List Header Layout
```
┌─────────────────────────────────┐
│ 🏠 Personal Tasks        ⚙️ ⋯   │
│ 3 of 5 tasks completed         │
├─────────────────────────────────┤
│ [Add a task...]                │
└─────────────────────────────────┘
```

### Color Palette
```css
/* List Colors */
--list-color-blue: #0078d4
--list-color-green: #107c10
--list-color-orange: #ff8c00
--list-color-purple: #8764b8
--list-color-red: #d13438
--list-color-teal: #00bcf2
--list-color-gray: #69797e
```

## 🚀 Implementation Phases

### Phase 1: Basic Lists (Week 1)
- List creation and deletion
- List switching
- Basic list display
- List name editing

### Phase 2: List Customization (Week 2)
- List colors and icons
- List descriptions
- List reordering
- Default list setting

### Phase 3: Advanced Features (Week 3)
- List statistics and progress
- List templates
- List sharing (future)
- List archiving

## 🔄 Integration Points

### Task Management
- New tasks assigned to current list
- Task counts update in real-time
- List switching filters tasks
- Task deletion updates list counts

### User Preferences
- Remember last selected list
- Set default list for new tasks
- List display preferences
- List sorting preferences

### Data Persistence
- Lists saved to localStorage
- List changes synced across devices
- List data backed up
- List recovery on data loss

---

**Rule**: Every list operation must be intuitive and provide immediate visual feedback. Lists are the primary organizational tool for the app.
