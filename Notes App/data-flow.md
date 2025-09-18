# Data Flow Rules
*Notes App - State Management Architecture*

## 🔄 Data Flow Principles

### Single Source of Truth
All application data flows through centralized Pinia stores. Components never manage their own state for shared data.

### Unidirectional Data Flow
```
User Action → Store Action → State Update → Component Re-render
```

### Store Responsibilities
1. **Fetch data** from APIs or local storage
2. **Transform/normalize** data for consistent usage
3. **Distribute state** to components
4. **Handle side effects** (API calls, persistence, sync)

## 📊 Data Flow Diagrams

### Overall Application Data Flow
```mermaid
graph TB
    subgraph "User Interface Layer"
        A[Dashboard View] --> B[Note List View]
        B --> C[Note Detail View]
        C --> D[Note Editor]
        A --> E[Notebooks View]
        A --> F[Search View]
        A --> G[Settings View]
    end
    
    subgraph "State Management Layer"
        H[Notes Store] --> I[Notebooks Store]
        I --> J[Auth Store]
        J --> K[Search Store]
        K --> L[Sync Store]
    end
    
    subgraph "Data Layer"
        M[IndexedDB] --> N[Local Storage]
        N --> O[Server API]
        O --> P[Elasticsearch]
    end
    
    D --> H
    E --> I
    F --> K
    G --> J
    H --> M
    I --> M
    K --> M
    L --> O
```

### Note Creation Flow
```mermaid
sequenceDiagram
    participant U as User
    participant NE as NoteEditor
    participant NS as NotesStore
    participant IDB as IndexedDB
    participant API as Server API
    participant SS as SyncStore
    
    U->>NE: Click "New Note"
    NE->>NS: createNote(data)
    NS->>NS: Add to notes array (optimistic)
    NS->>IDB: Save to local storage
    NS->>SS: Add to sync queue
    NE->>U: Show new note in editor
    
    par Background Sync
        SS->>API: POST /api/notes
        API-->>SS: Return saved note
        SS->>NS: Update with server data
        NS->>IDB: Update local storage
    end
```

### Note Editing Flow
```mermaid
sequenceDiagram
    participant U as User
    participant NE as NoteEditor
    participant NS as NotesStore
    participant IDB as IndexedDB
    participant API as Server API
    participant SS as SyncStore
    
    U->>NE: Edit note content
    NE->>NS: updateNote(id, content)
    NS->>NS: Update note in array
    NS->>IDB: Save to local storage
    NS->>SS: Add to sync queue
    NE->>U: Show updated content
    
    par Background Sync
        SS->>API: PUT /api/notes/:id
        API-->>SS: Return updated note
        SS->>NS: Update with server data
        NS->>IDB: Update local storage
    end
```

### Search Flow
```mermaid
sequenceDiagram
    participant U as User
    participant SI as SearchInput
    participant SS as SearchStore
    participant NS as NotesStore
    participant API as Server API
    participant ES as Elasticsearch
    
    U->>SI: Type search query
    SI->>SS: searchNotes(query)
    SS->>NS: Filter local notes
    SS->>U: Show local results
    
    par Background Search
        SS->>API: GET /api/search?q=query
        API->>ES: Search index
        ES-->>API: Return results
        API-->>SS: Return search results
        SS->>NS: Update with server results
        SS->>U: Show complete results
    end
```

### Notebook Organization Flow
```mermaid
sequenceDiagram
    participant U as User
    participant NL as NotebookList
    participant NBS as NotebooksStore
    participant NS as NotesStore
    participant IDB as IndexedDB
    participant API as Server API
    
    U->>NL: Create notebook
    NL->>NBS: createNotebook(data)
    NBS->>NBS: Add to notebooks array
    NBS->>IDB: Save to local storage
    NBS->>API: POST /api/notebooks
    NBS->>U: Show new notebook
    
    U->>NL: Move note to notebook
    NL->>NS: updateNote(id, {notebookId})
    NS->>NS: Update note in array
    NS->>IDB: Save to local storage
    NS->>API: PUT /api/notes/:id
    NS->>U: Show note in new notebook
```

### Authentication Flow
```mermaid
sequenceDiagram
    participant U as User
    participant L as LoginView
    participant AS as AuthStore
    participant API as Server API
    participant NS as NotesStore
    participant NBS as NotebooksStore
    
    U->>L: Enter credentials
    L->>AS: login(email, password)
    AS->>API: POST /api/auth/login
    API-->>AS: Return JWT token
    AS->>AS: Store token & user data
    AS->>NS: Load user notes
    AS->>NBS: Load user notebooks
    AS->>U: Redirect to dashboard
    
    Note over AS: Token stored in localStorage
    Note over NS,NBS: Data loaded from server
```

### Offline Sync Flow
```mermaid
sequenceDiagram
    participant U as User
    participant AS as App (Online)
    participant SS as SyncStore
    participant IDB as IndexedDB
    participant API as Server API
    participant AS2 as App (Offline)
    
    AS->>SS: User action (create/edit)
    SS->>IDB: Save to local storage
    SS->>SS: Add to sync queue
    
    par Background Sync
        SS->>API: Sync pending changes
        API-->>SS: Return server updates
        SS->>IDB: Update local storage
        SS->>SS: Clear sync queue
    end
    
    Note over AS2: App goes offline
    AS2->>SS: User action
    SS->>IDB: Save to local storage
    SS->>SS: Add to sync queue
    
    Note over AS2: App comes back online
    AS2->>SS: Trigger sync
    SS->>API: Sync all pending changes
    API-->>SS: Return server updates
    SS->>IDB: Update local storage
    SS->>SS: Clear sync queue
```

### Rich Media Upload Flow
```mermaid
sequenceDiagram
    participant U as User
    participant NE as NoteEditor
    participant FS as FileService
    participant S3 as AWS S3
    participant NS as NotesStore
    participant API as Server API
    
    U->>NE: Upload image/audio/video
    NE->>FS: uploadFile(file)
    FS->>S3: Upload to cloud storage
    S3-->>FS: Return file URL
    FS->>API: Save file metadata
    API-->>FS: Return file ID
    FS->>NE: Return file reference
    NE->>NS: updateNote(id, {attachments})
    NS->>NS: Update note with file reference
    NS->>U: Show file in note
```

### Real-time Collaboration Flow (Future)
```mermaid
sequenceDiagram
    participant U1 as User 1
    participant U2 as User 2
    participant WS as WebSocket
    participant NS as NotesStore
    participant API as Server API
    
    U1->>NS: Edit note content
    NS->>API: Send change via WebSocket
    API->>WS: Broadcast to other users
    WS->>U2: Receive change
    U2->>NS: Update local note
    NS->>U2: Show updated content
    
    Note over U1,U2: Real-time collaboration
    Note over WS: WebSocket connection
```

### Error Handling Flow
```mermaid
sequenceDiagram
    participant U as User
    participant C as Component
    participant S as Store
    participant API as Server API
    participant EH as ErrorHandler
    
    U->>C: User action
    C->>S: Store action
    S->>API: API call
    API-->>S: Error response
    S->>EH: Handle error
    EH->>S: Update error state
    S->>C: Return error
    C->>U: Show error message
    
    Note over EH: Error logging & reporting
    Note over S: Rollback optimistic updates
```

### Data Persistence Layers
```mermaid
graph TB
    subgraph "Client Side"
        A[Vue Components] --> B[Pinia Stores]
        B --> C[IndexedDB]
        B --> D[localStorage]
        B --> E[Sync Queue]
    end
    
    subgraph "Network Layer"
        F[API Service] --> G[HTTP Client]
        G --> H[WebSocket]
    end
    
    subgraph "Server Side"
        I[Express API] --> J[PostgreSQL]
        I --> K[Redis Cache]
        I --> L[Elasticsearch]
        I --> M[AWS S3]
    end
    
    E --> F
    C --> F
    F --> I
    I --> J
    I --> K
    I --> L
    I --> M
```

### Component Data Dependencies
```mermaid
graph LR
    subgraph "Views"
        A[Dashboard] --> B[NoteList]
        A --> C[NotebookList]
        A --> D[SearchResults]
        E[NoteDetail] --> F[NoteEditor]
        E --> G[NoteAttachments]
        H[Settings] --> I[UserProfile]
    end
    
    subgraph "Stores"
        J[NotesStore] --> K[NotebooksStore]
        K --> L[AuthStore]
        L --> M[SearchStore]
        M --> N[SyncStore]
    end
    
    B --> J
    C --> K
    D --> M
    F --> J
    G --> J
    I --> L
```

### State Management Hierarchy
```mermaid
graph TD
    A[App State] --> B[Auth Store]
    A --> C[Notes Store]
    A --> D[Notebooks Store]
    A --> E[Search Store]
    A --> F[Sync Store]
    A --> G[UI Store]
    
    B --> H[User Data]
    B --> I[Auth Token]
    B --> J[Login State]
    
    C --> K[Notes Array]
    C --> L[Current Note]
    C --> M[Loading State]
    C --> N[Error State]
    
    D --> O[Notebooks Array]
    D --> P[Current Notebook]
    D --> Q[Notebook Stats]
    
    E --> R[Search Query]
    E --> S[Search Results]
    E --> T[Search Filters]
    
    F --> U[Sync Queue]
    F --> V[Sync Status]
    F --> W[Last Sync Time]
    
    G --> X[Theme Mode]
    G --> Y[View Mode]
    G --> Z[Sidebar State]
```

## 🏗️ Store Architecture

### Store Structure
```typescript
// src/stores/notes.ts
export const useNotesStore = defineStore('notes', () => {
  // State
  const notes = ref<Note[]>([])
  const currentNote = ref<Note | null>(null)
  const isLoading = ref(false)
  const error = ref<string | null>(null)
  
  // Getters
  const pinnedNotes = computed(() => 
    notes.value.filter(note => note.pinned)
  )
  
  const notesByNotebook = computed(() => (notebookId: string) =>
    notes.value.filter(note => note.notebookId === notebookId)
  )
  
  // Actions
  const fetchNotes = async () => { /* implementation */ }
  const createNote = async (note: CreateNoteRequest) => { /* implementation */ }
  const updateNote = async (id: string, updates: UpdateNoteRequest) => { /* implementation */ }
  const deleteNote = async (id: string) => { /* implementation */ }
  
  return {
    // State
    notes,
    currentNote,
    isLoading,
    error,
    // Getters
    pinnedNotes,
    notesByNotebook,
    // Actions
    fetchNotes,
    createNote,
    updateNote,
    deleteNote
  }
})
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

### 1. Note Creation Flow
```
User clicks "New Note" 
→ NoteEditor component calls store.createNote()
→ Store updates notes array
→ NoteList component re-renders with new note
→ Store persists to IndexedDB
→ Store syncs to server in background
```

### 2. Note Editing Flow
```
User edits note content
→ NoteEditor calls store.updateNote(id, content)
→ Store updates note in array
→ All note components re-render
→ Store saves to IndexedDB
→ Store queues for sync
```

### 3. Search Flow
```
User types in search
→ SearchInput calls store.searchNotes(query)
→ Store filters notes array
→ NoteList shows filtered results
→ Store caches search results
```

### 4. Sync Flow
```
Background sync triggered
→ SyncStore checks for pending changes
→ Store sends changes to server
→ Store receives server updates
→ Store merges changes with local data
→ Components re-render with updated data
```

## 🗄️ Data Persistence

### Local Storage Strategy
```typescript
// Store automatically syncs with IndexedDB
const saveToIndexedDB = async (state: NotesState) => {
  const db = await openDB('NotesApp', 1)
  await db.put('notes', state.notes)
  await db.put('notebooks', state.notebooks)
}

const loadFromIndexedDB = async (): Promise<Partial<AppState>> => {
  const db = await openDB('NotesApp', 1)
  const notes = await db.get('notes', 'all')
  const notebooks = await db.get('notebooks', 'all')
  return { notes, notebooks }
}
```

### Data Normalization
```typescript
// Normalized data structure
interface AppState {
  notes: {
    [id: string]: Note
  }
  notebooks: {
    [id: string]: Notebook
  }
  tags: {
    [id: string]: Tag
  }
  ui: {
    currentNoteId: string | null
    currentNotebookId: string | null
    searchQuery: string
    viewMode: 'grid' | 'list'
  }
}
```

## 🔄 Component Data Rules

### Data Consumption
```vue
<!-- ✅ Good: Component consumes store data -->
<template>
  <div>
    <NoteItem 
      v-for="note in notes" 
      :key="note.id" 
      :note="note"
      @update="handleUpdate"
    />
  </div>
</template>

<script setup lang="ts">
import { useNotesStore } from '@/stores/notes'

const notesStore = useNotesStore()
const notes = computed(() => notesStore.notes)

const handleUpdate = (note: Note) => {
  notesStore.updateNote(note.id, note)
}
</script>
```

### Action Dispatching
```vue
<!-- ✅ Good: Component dispatches store actions -->
<template>
  <v-btn @click="createNote">Create Note</v-btn>
</template>

<script setup lang="ts">
import { useNotesStore } from '@/stores/notes'

const notesStore = useNotesStore()

const createNote = async () => {
  await notesStore.createNote({
    title: 'New Note',
    content: '',
    notebookId: 'default'
  })
}
</script>
```

## 🎯 State Management Rules

### Store Organization
- **One store per domain**: `notesStore`, `notebooksStore`, `authStore`
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
const useNotesStore = defineStore('notes', () => {
  const createNote = async (data: CreateNoteRequest) => {
    // Optimistic update
    const tempNote = { ...data, id: generateId(), synced: false }
    notes.value.push(tempNote)
    
    try {
      // API call
      const savedNote = await notesApi.createNote(data)
      
      // Update with server response
      const index = notes.value.findIndex(n => n.id === tempNote.id)
      if (index !== -1) {
        notes.value[index] = { ...savedNote, synced: true }
      }
    } catch (error) {
      // Revert optimistic update
      notes.value = notes.value.filter(n => n.id !== tempNote.id)
      error.value = error.message
    }
  }
})
```

### Error Handling
- **Store-level errors**: Global error state for API failures
- **Component-level errors**: Local error state for UI issues
- **Error boundaries**: Catch and display unexpected errors
- **Retry mechanisms**: Automatic retry for failed requests

## 🔄 Sync Architecture

### Sync Queue Management
```typescript
// src/stores/sync.ts
export const useSyncStore = defineStore('sync', () => {
  const syncQueue = ref<SyncItem[]>([])
  const isSyncing = ref(false)
  const lastSyncTime = ref<Date | null>(null)
  
  const addToSyncQueue = (item: SyncItem) => {
    syncQueue.value.push(item)
    triggerSync()
  }
  
  const processSyncQueue = async () => {
    if (isSyncing.value || syncQueue.value.length === 0) return
    
    isSyncing.value = true
    try {
      const items = [...syncQueue.value]
      syncQueue.value = []
      
      for (const item of items) {
        await processSyncItem(item)
      }
      
      lastSyncTime.value = new Date()
    } catch (error) {
      // Re-add failed items to queue
      syncQueue.value.unshift(...items)
    } finally {
      isSyncing.value = false
    }
  }
})
```

### Conflict Resolution
```typescript
// Conflict resolution strategy
const resolveConflict = (local: Note, remote: Note): Note => {
  // Last-write-wins with timestamp comparison
  if (local.updatedAt > remote.updatedAt) {
    return local
  } else if (remote.updatedAt > local.updatedAt) {
    return remote
  } else {
    // Same timestamp - merge content
    return {
      ...local,
      content: mergeContent(local.content, remote.content)
    }
  }
}
```

## 🔄 Offline-First Architecture

### Offline State Management
```typescript
// src/composables/useOffline.ts
export function useOffline() {
  const isOnline = ref(navigator.onLine)
  const pendingChanges = ref<SyncItem[]>([])
  
  const handleOnline = () => {
    isOnline.value = true
    // Trigger sync when back online
    triggerSync()
  }
  
  const handleOffline = () => {
    isOnline.value = false
    // Queue changes for later sync
  }
  
  onMounted(() => {
    window.addEventListener('online', handleOnline)
    window.addEventListener('offline', handleOffline)
  })
  
  return {
    isOnline,
    pendingChanges
  }
}
```

### Data Synchronization
```typescript
// Background sync with service worker
const syncInBackground = async () => {
  if ('serviceWorker' in navigator && 'sync' in window.ServiceWorkerRegistration.prototype) {
    const registration = await navigator.serviceWorker.ready
    await registration.sync.register('notes-sync')
  }
}
```

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

### Before Adding New Component:
- [ ] Does it consume store state correctly?
- [ ] Does it dispatch store actions?
- [ ] Does it handle loading and error states?
- [ ] Is it properly typed?

## 🔄 Integration Points

### API Integration
```typescript
// src/services/api/notes.ts
export const notesApi = {
  async getNotes(): Promise<Note[]> {
    const response = await fetch('/api/notes')
    return response.json()
  },
  
  async createNote(data: CreateNoteRequest): Promise<Note> {
    const response = await fetch('/api/notes', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data)
    })
    return response.json()
  }
}
```

### Local Storage Integration
```typescript
// src/services/storage/indexedDB.ts
export const indexedDBService = {
  async saveNotes(notes: Note[]): Promise<void> {
    const db = await openDB('NotesApp', 1)
    await db.put('notes', notes)
  },
  
  async loadNotes(): Promise<Note[]> {
    const db = await openDB('NotesApp', 1)
    return await db.get('notes', 'all') || []
  }
}
```

---

**Rule**: All data flows through the store. Components never bypass the store pattern. If you need to share data between components, it belongs in a store.
