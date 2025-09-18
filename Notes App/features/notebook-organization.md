# Notebook Organization Feature PRD
*Notebook creation, management, and note organization system*

## 📋 Feature Overview

**What it does**: Enables users to organize notes into notebooks, manage notebook properties, and maintain a hierarchical structure for better note organization.

**User Story**: "As a user, I want to organize my notes into notebooks with colors and custom ordering so I can keep my thoughts organized and easily accessible."

**Navigation**: Dashboard → Sidebar → Notebook Management

## 🎯 User Stories

### Primary User Stories
1. **Create Notebook**: "As a user, I want to create notebooks to group related notes so I can organize my thoughts better."
2. **Organize Notes**: "As a user, I want to assign notes to specific notebooks so I can categorize my content."
3. **Customize Notebooks**: "As a user, I want to color-code and rename notebooks so I can visually distinguish them."
4. **Reorder Notebooks**: "As a user, I want to reorder notebooks by preference so I can prioritize what's important."

### Secondary User Stories
1. **Notebook Statistics**: "As a user, I want to see note counts for each notebook so I can understand my content distribution."
2. **Delete Notebooks**: "As a user, I want to delete notebooks I no longer need so I can keep my workspace clean."
3. **Search Notebooks**: "As a user, I want to search through notebook names so I can find specific notebooks quickly."
4. **Bulk Operations**: "As a user, I want to move multiple notes between notebooks so I can reorganize efficiently."

## 🎨 UI/UX Requirements

### Notebook Sidebar
- **List Display**: Vertical list of all user notebooks
- **Active Notebook**: Highlighted current notebook
- **Note Counts**: Show total and recent note counts
- **Color Coding**: Visual color indicators for each notebook
- **Collapsible**: Hide/show sidebar on mobile devices

### Notebook Management
- **Create Button**: Prominent "+" button to add new notebook
- **Inline Editing**: Click notebook name to rename
- **Context Menu**: Right-click for notebook options
- **Drag & Drop**: Reorder notebooks by dragging
- **Color Picker**: Easy color selection interface

### Notebook Display
- **Card Layout**: Consistent notebook cards with color-coded borders
- **Statistics**: Show note count and last updated date
- **Action Buttons**: Edit, delete, and settings options
- **Active State**: Visual indication of currently selected notebook
- **Hover Effects**: Subtle animations for better user feedback

### Note Assignment Interface
- **Dropdown Selector**: Easy notebook selection during note creation
- **Bulk Assignment**: Select multiple notes and assign to notebook
- **Drag and Drop**: Move notes between notebooks
- **Assignment History**: Track note movement between notebooks
- **Unassigned Notes**: Handle notes without notebook assignment

## 🔧 Technical Requirements

### Data Model
```typescript
interface Notebook {
  id: string
  name: string
  description?: string
  color: string
  icon?: string
  noteCount: number
  createdAt: Date
  updatedAt: Date
  userId: string
  sortOrder: number
  deleted: boolean
}

interface CreateNotebookRequest {
  name: string
  description?: string
  color: string
  icon?: string
}

interface UpdateNotebookRequest {
  name?: string
  description?: string
  color?: string
  icon?: string
  sortOrder?: number
}

interface NoteAssignmentRequest {
  noteIds: string[]
  notebookId: string
}
```

### API Endpoints
- `GET /api/notebooks` - Get all user notebooks
- `POST /api/notebooks` - Create new notebook
- `GET /api/notebooks/:id` - Get specific notebook
- `PUT /api/notebooks/:id` - Update notebook
- `DELETE /api/notebooks/:id` - Delete notebook
- `PATCH /api/notebooks/reorder` - Reorder notebooks
- `POST /api/notebooks/assign-notes` - Assign notes to notebook
- `GET /api/notebooks/:id/notes` - Get notes in notebook

### Store Actions
```typescript
interface NotebookStoreActions {
  createNotebook: (notebook: CreateNotebookRequest) => Promise<Notebook>
  updateNotebook: (id: string, updates: UpdateNotebookRequest) => Promise<Notebook>
  deleteNotebook: (id: string) => Promise<void>
  reorderNotebooks: (notebookIds: string[]) => Promise<void>
  assignNotes: (noteIds: string[], notebookId: string) => Promise<void>
  getNotebooks: () => Promise<Notebook[]>
  getNotebookNotes: (notebookId: string) => Promise<Note[]>
}
```

## ✅ Acceptance Criteria

### Notebook Creation
- [ ] User can create new notebooks with name and description
- [ ] Color selection works from predefined palette
- [ ] Notebook names must be unique per user
- [ ] New notebook appears in sidebar immediately
- [ ] Default "General" notebook created for new users
- [ ] Notebook gets assigned unique ID and timestamps

### Notebook Management
- [ ] User can rename notebooks by clicking on name
- [ ] User can change notebook colors with immediate visual feedback
- [ ] User can delete notebooks with confirmation dialog
- [ ] User can reorder notebooks by dragging
- [ ] Notebook changes persist across sessions
- [ ] Deleted notebook notes are reassigned to default notebook

### Note Assignment
- [ ] User can assign notes to notebooks during creation
- [ ] User can move notes between notebooks easily
- [ ] Bulk assignment works for multiple selected notes
- [ ] Note counts update in real-time
- [ ] Assignment history is tracked
- [ ] Unassigned notes are handled gracefully

### Notebook Display
- [ ] All notebooks display in sidebar with note counts
- [ ] Active notebook is visually highlighted
- [ ] Color coding is consistent across all displays
- [ ] Notebook statistics show note count and last updated
- [ ] Search functionality works for notebook names

## 🧪 Test Cases

### Happy Path Tests
1. **Create Notebook**: Click "+" → Enter name → Select color → Notebook created
2. **Rename Notebook**: Click name → Edit text → Press Enter → Name updated
3. **Assign Notes**: Select notes → Choose notebook → Notes moved
4. **Reorder Notebooks**: Drag notebook → Drop in new position → Order saved

### Edge Cases
1. **Duplicate Names**: Try to create notebook with existing name → Validation error
2. **Empty Name**: Try to create notebook with empty name → Validation error
3. **Delete with Notes**: Delete notebook with notes → Reassignment dialog
4. **Color Conflicts**: All colors used → Default color assigned

### Error Cases
1. **Network Failure**: Try to create notebook offline → Graceful degradation
2. **Invalid Data**: Send malformed notebook data → Error handling
3. **Assignment Failure**: Note assignment fails → Rollback and error message
4. **Sync Conflict**: Notebook edited on two devices → Conflict resolution

## 📊 Success Metrics

### User Experience
- **Notebook Creation**: Average 3-5 notebooks per user
- **Note Assignment**: 80%+ of notes assigned to notebooks
- **Color Usage**: 60%+ of users customize notebook colors
- **Reordering**: 40%+ of users reorder notebooks

### Technical Performance
- **Notebook Load Time**: < 200ms for notebook list
- **Assignment Speed**: < 500ms for note reassignment
- **Color Updates**: < 100ms for color changes
- **Error Rate**: < 1% error rate for notebook operations

## 🎨 Design Specifications

### Notebook Sidebar Layout
```
┌─────────────────┐
│ 📚 My Notebooks │
├─────────────────┤
│ 🏠 Work         │ 15 notes
│ 🎯 Personal     │ 8 notes
│ 📚 Learning     │ 3 notes
│ 🎨 Projects     │ 12 notes
├─────────────────┤
│ + Add Notebook  │
└─────────────────┘
```

### Notebook Creation Dialog
```
┌─────────────────────────────────┐
│ Create New Notebook             │
├─────────────────────────────────┤
│ Name: [________________]       │
│ Description: [_____________]   │
│ Color: [🔴] [🟢] [🔵] [🟡]    │
│                                 │
│ [Cancel] [Create]               │
└─────────────────────────────────┘
```

## 🚀 Implementation Phases

### Phase 1: Basic Notebooks (Week 1)
- Notebook creation and deletion
- Basic notebook display
- Note assignment functionality
- Color selection

### Phase 2: Organization (Week 2)
- Notebook reordering
- Bulk note assignment
- Notebook statistics
- Search functionality

### Phase 3: Advanced Features (Week 3)
- Advanced color management
- Notebook templates
- Performance optimizations
- Accessibility improvements

## 🔄 Integration Points

### Note Management
- Notes can be assigned to notebooks
- Notebook changes update note counts
- Note deletion updates notebook statistics
- Search includes notebook context

### State Management
- Notebook state managed centrally
- Note assignment triggers store updates
- Color changes persist locally
- Reordering state synchronized

### Data Persistence
- Notebooks saved to local storage
- Changes queued for sync
- Color preferences persisted
- Assignment history maintained

---

**Rule**: Every notebook operation must be intuitive and provide immediate visual feedback. Users should be able to organize their notes efficiently without confusion.

**Priority**: High (Core Feature)
**Complexity**: Medium
**Dependencies**: State Management, IndexedDB, Drag and Drop  
**Success Criteria**: Intuitive, efficient, and reliable notebook organization system
