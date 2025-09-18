# Project Structure Rules
*Notes App - Project Skeleton*

## 🗂️ Directory Structure

The project follows a clean, scalable structure that separates concerns and makes the codebase maintainable:

```
src/
├── views/            # Route-level screens
│   ├── Dashboard.vue
│   ├── NoteList.vue
│   ├── NoteDetail.vue
│   ├── Notebooks.vue
│   ├── Search.vue
│   ├── Settings.vue
│   └── Login.vue
├── components/       # Reusable UI blocks
│   ├── ui/           # Basic UI components
│   │   ├── VButton.vue
│   │   ├── VInput.vue
│   │   ├── VModal.vue
│   │   ├── VCard.vue
│   │   ├── VDialog.vue
│   │   └── VTooltip.vue
│   ├── note/         # Note-specific components
│   │   ├── NoteItem.vue
│   │   ├── NoteEditor.vue
│   │   ├── NoteList.vue
│   │   ├── NoteSearch.vue
│   │   ├── NoteTags.vue
│   │   └── NoteAttachments.vue
│   ├── notebook/     # Notebook components
│   │   ├── NotebookItem.vue
│   │   ├── NotebookList.vue
│   │   ├── NotebookForm.vue
│   │   └── NotebookSidebar.vue
│   └── layout/       # Layout components
│       ├── AppHeader.vue
│       ├── AppSidebar.vue
│       ├── AppFooter.vue
│       └── AppLayout.vue
├── composables/      # Vue 3 composition functions
│   ├── useNotes.ts
│   ├── useNotebooks.ts
│   ├── useAuth.ts
│   ├── useSearch.ts
│   ├── useSync.ts
│   ├── useLocalStorage.ts
│   └── useDebounce.ts
├── stores/           # Pinia stores
│   ├── notes.ts
│   ├── notebooks.ts
│   ├── auth.ts
│   ├── search.ts
│   ├── sync.ts
│   └── index.ts
├── services/         # API and external services
│   ├── api/
│   │   ├── notes.ts
│   │   ├── notebooks.ts
│   │   ├── auth.ts
│   │   ├── search.ts
│   │   └── index.ts
│   ├── storage/
│   │   ├── indexedDB.ts
│   │   ├── localStorage.ts
│   │   └── syncQueue.ts
│   └── external/
│       ├── aws-s3.ts
│       ├── elasticsearch.ts
│       └── encryption.ts
├── utils/            # Helper functions
│   ├── dateHelpers.ts
│   ├── validation.ts
│   ├── markdown.ts
│   ├── encryption.ts
│   ├── fileHelpers.ts
│   └── constants.ts
├── types/            # TypeScript contracts
│   ├── note.ts
│   ├── notebook.ts
│   ├── user.ts
│   ├── api.ts
│   ├── search.ts
│   └── index.ts
├── plugins/          # Vue plugins
│   ├── vuetify.ts
│   ├── router.ts
│   ├── pinia.ts
│   └── tiptap.ts
├── assets/           # Static files
│   ├── images/
│   ├── icons/
│   ├── fonts/
│   └── styles/
│       ├── main.scss
│       ├── variables.scss
│       └── components/
└── router/           # Vue Router configuration
    ├── index.ts
    ├── routes.ts
    └── guards.ts
```

## 📋 Structure Rules

### ✅ DO:
- **Group by feature**: Note-related components go in `components/note/`
- **Separate concerns**: UI components vs business logic vs services
- **Use descriptive names**: `NoteItem.vue` not `Item.vue`
- **Keep it flat**: Avoid deeply nested folders (max 3 levels)
- **Consistent naming**: PascalCase for components, camelCase for utilities
- **Vue 3 patterns**: Use Composition API and `<script setup>`
- **TypeScript first**: All files should be `.ts` or `.vue` with TypeScript

### ❌ DON'T:
- **Dump random files**: No `miscStuff.js` or `temp.ts`
- **Mix concerns**: Don't put API calls in UI components
- **Create unnecessary folders**: Don't make `components/ui/buttons/` for one button
- **Use generic names**: No `Component1.vue` or `utils.ts`
- **Mix Options and Composition API**: Stick to Composition API
- **Put business logic in components**: Use composables or services

## 🎯 File Placement Guidelines

| File Type | Location | Example |
|-----------|----------|---------|
| Page components | `src/views/` | `Dashboard.vue` |
| Reusable UI | `src/components/ui/` | `VButton.vue` |
| Feature components | `src/components/[feature]/` | `NoteItem.vue` |
| Composables | `src/composables/` | `useNotes.ts` |
| Pinia stores | `src/stores/` | `notes.ts` |
| API services | `src/services/api/` | `notes.ts` |
| Utility functions | `src/utils/` | `dateHelpers.ts` |
| Type definitions | `src/types/` | `note.ts` |
| Static assets | `src/assets/` | `icons/note.svg` |
| Vue plugins | `src/plugins/` | `vuetify.ts` |

## 🔄 Import Rules

```typescript
// ✅ Good: Clear, organized imports
import { NoteItem } from '@/components/note/NoteItem.vue'
import { useNotes } from '@/composables/useNotes'
import { Note } from '@/types/note'

// ❌ Bad: Messy, unclear imports
import { NoteItem } from '../../../components/note/NoteItem.vue'
import { useNotes } from '../../composables/useNotes'
```

## 📁 Directory Responsibilities

### **`views/`** - Route-level screens
- Full-page components that users navigate to
- Handle routing and page-level state
- Compose multiple components together

### **`components/`** - Reusable UI building blocks
- **`ui/`**: Basic UI components (buttons, inputs, modals)
- **`note/`**: Note-specific components (editor, list, search)
- **`notebook/`**: Notebook-related components
- **`layout/`**: Layout and navigation components

### **`composables/`** - Vue 3 composition functions
- Reusable reactive logic
- Custom hooks for shared functionality
- Business logic that can be shared across components

### **`stores/`** - Pinia state management
- Global application state
- Actions and mutations
- State persistence and hydration

### **`services/`** - External integrations
- **`api/`**: Backend API calls
- **`storage/`**: Local storage and IndexedDB
- **`external/`**: Third-party service integrations

### **`utils/`** - Pure utility functions
- Helper functions with no Vue dependencies
- Data transformation and validation
- Constants and configuration

### **`types/`** - TypeScript type definitions
- Interface and type definitions
- API response types
- Component prop types

### **`plugins/`** - Vue plugin configuration
- Vuetify, Router, Pinia setup
- Global component registration
- Plugin initialization

### **`assets/`** - Static files
- Images, icons, fonts
- Global styles and themes
- Static data files

## 🎨 Vue.js Best Practices

### **Component Structure**
```vue
<template>
  <!-- Template content -->
</template>

<script setup lang="ts">
// Composition API with TypeScript
import { ref, computed, onMounted } from 'vue'
import type { Note } from '@/types/note'

// Props and emits
interface Props {
  note: Note
  editable?: boolean
}

const props = withDefaults(defineProps<Props>(), {
  editable: false
})

const emit = defineEmits<{
  update: [note: Note]
  delete: [id: string]
}>()

// Reactive state
const isEditing = ref(false)

// Computed properties
const displayTitle = computed(() => 
  props.note.title || 'Untitled Note'
)

// Methods
const handleSave = () => {
  emit('update', props.note)
  isEditing.value = false
}

// Lifecycle
onMounted(() => {
  // Component initialization
})
</script>

<style scoped>
/* Component-specific styles */
</style>
```

### **Composable Pattern**
```typescript
// src/composables/useNotes.ts
import { ref, computed } from 'vue'
import { useNotesStore } from '@/stores/notes'
import type { Note, CreateNoteRequest } from '@/types/note'

export function useNotes() {
  const notesStore = useNotesStore()
  
  const isLoading = computed(() => notesStore.isLoading)
  const notes = computed(() => notesStore.notes)
  
  const createNote = async (data: CreateNoteRequest) => {
    return await notesStore.createNote(data)
  }
  
  const updateNote = async (id: string, data: Partial<Note>) => {
    return await notesStore.updateNote(id, data)
  }
  
  const deleteNote = async (id: string) => {
    return await notesStore.deleteNote(id)
  }
  
  return {
    isLoading,
    notes,
    createNote,
    updateNote,
    deleteNote
  }
}
```

### **Store Pattern**
```typescript
// src/stores/notes.ts
import { defineStore } from 'pinia'
import { ref, computed } from 'vue'
import type { Note, CreateNoteRequest } from '@/types/note'

export const useNotesStore = defineStore('notes', () => {
  // State
  const notes = ref<Note[]>([])
  const isLoading = ref(false)
  const error = ref<string | null>(null)
  
  // Getters
  const pinnedNotes = computed(() => 
    notes.value.filter(note => note.pinned)
  )
  
  // Actions
  const fetchNotes = async () => {
    isLoading.value = true
    try {
      // API call
      notes.value = await notesApi.getNotes()
    } catch (err) {
      error.value = err.message
    } finally {
      isLoading.value = false
    }
  }
  
  const createNote = async (data: CreateNoteRequest) => {
    // Implementation
  }
  
  return {
    notes,
    isLoading,
    error,
    pinnedNotes,
    fetchNotes,
    createNote
  }
})
```

## 🔧 Naming Conventions

### **Components**
- **PascalCase**: `NoteEditor.vue`, `VButton.vue`
- **Prefix UI components**: `VButton`, `VInput`, `VModal`
- **Feature components**: `NoteItem`, `NotebookList`

### **Files**
- **kebab-case**: `note-editor.vue`, `use-notes.ts`
- **PascalCase for components**: `NoteEditor.vue`
- **camelCase for utilities**: `dateHelpers.ts`

### **Variables and Functions**
- **camelCase**: `isLoading`, `handleSave`, `createNote`
- **Constants**: `UPPER_SNAKE_CASE`: `API_BASE_URL`

## 📦 Module Organization

### **Feature-based Modules**
```
src/
├── modules/
│   ├── notes/
│   │   ├── components/
│   │   ├── composables/
│   │   ├── stores/
│   │   ├── services/
│   │   └── types/
│   ├── notebooks/
│   │   └── ...
│   └── auth/
│       └── ...
```

### **Layer-based Organization** (Current)
- Separates by technical concerns
- Easier to find specific types of files
- Better for smaller to medium projects

---

**Rule**: Every new file must fit into this structure. If it doesn't, the structure needs updating, not the file placement.
