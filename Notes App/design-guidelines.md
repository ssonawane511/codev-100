# Design Guidelines
*Notes App - Visual & UX Standards*

## 🎨 Design System Overview

### Tailwind CSS Guidelines
**CRITICAL RULE**: All styling must use Tailwind CSS classes defined in global style files as reusable CSS components. No inline styles or inline Tailwind classes are allowed in layouts or components.

#### Global Style File Structure
```scss
// src/assets/styles/global.scss
@tailwind base;
@tailwind components;
@tailwind utilities;

// Custom component classes using Tailwind utilities
@layer components {
  // Button components
  .btn-primary { @apply bg-blue-600 text-white px-4 py-2 rounded hover:bg-blue-700 transition-colors; }
  .btn-secondary { @apply bg-transparent text-blue-600 border border-blue-600 px-4 py-2 rounded hover:bg-blue-50; }
  .btn-accent { @apply bg-yellow-200 text-blue-900 px-4 py-2 rounded hover:bg-yellow-300; }
  
  // Card components
  .card-note { @apply bg-white border border-gray-200 rounded-lg p-6 shadow-sm hover:shadow-md transition-shadow; }
  .card-pinned { @apply bg-blue-50 border-l-4 border-blue-600; }
  
  // Input components
  .input-field { @apply border border-gray-300 rounded px-4 py-3 focus:border-blue-600 focus:ring-2 focus:ring-blue-200; }
  .input-search { @apply border border-gray-300 rounded px-4 py-3 focus:border-blue-400 focus:ring-2 focus:ring-blue-100; }
  
  // Layout components
  .container-main { @apply max-w-7xl mx-auto px-4 sm:px-6 lg:px-8; }
  .grid-notes { @apply grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6; }
  .flex-toolbar { @apply flex items-center space-x-2 p-4 border-b border-gray-200; }
}
```

#### Strict Usage Rules
- **NO inline Tailwind classes**: `class="bg-blue-500 text-white"` ❌
- **NO inline CSS**: `style="background: blue"` ❌
- **USE component classes**: `class="btn-primary"` ✅
- **ALL styles in global.scss**: Define reusable components
- **ONE source of truth**: All styling centralized in global files

#### Component Class Naming Convention
- **Buttons**: `.btn-{variant}` (btn-primary, btn-secondary, btn-accent)
- **Cards**: `.card-{type}` (card-note, card-pinned, card-featured)
- **Inputs**: `.input-{type}` (input-field, input-search, input-textarea)
- **Layouts**: `.{section}-{element}` (header-nav, sidebar-menu, main-content)
- **States**: `.{component}-{state}` (btn-disabled, card-selected, input-error)

### Theme Colors
```scss
/* Primary Colors - Custom Blue Palette */
$primary-dark: #154D71;        /* Dark blue - rgb(21, 77, 113) */
$primary-medium: #1C6EA4;      /* Medium blue - rgb(28, 110, 164) */
$primary-light: #33A1E0;       /* Light blue - rgb(51, 161, 224) */
$primary-accent: #FFF9AF;      /* Light yellow accent - rgb(255, 249, 175) */

/* Color Variations */
$primary-dark-hover: #0f3a56;  /* Darker for hover states */
$primary-medium-hover: #155a8a; /* Medium hover */
$primary-light-hover: #2a8bc7;  /* Light hover */
$primary-light-bg: #e6f4ff;    /* Very light blue backgrounds */
$primary-accent-hover: #fff7a0; /* Accent hover */

/* Neutral Colors */
$neutral-10: #fafafa;          /* Lightest background */
$neutral-20: #f5f5f5;          /* Light background */
$neutral-30: #eeeeee;          /* Subtle borders */
$neutral-50: #9e9e9e;          /* Disabled text */
$neutral-80: #424242;          /* Primary text */
$neutral-90: #212121;          /* Darkest text */

/* Semantic Colors */
$success-green: #4caf50;       /* Success states */
$warning-orange: #ff9800;      /* Warning states */
$error-red: #f44336;           /* Error states */
$info-blue: $primary-light;    /* Information states - using primary light */

/* Surface Colors */
$surface-primary: #ffffff;     /* Card backgrounds */
$surface-secondary: #fafafa;   /* Page backgrounds */
$surface-tertiary: #f5f5f5;    /* Subtle backgrounds */
$surface-accent: $primary-accent; /* Accent backgrounds */
```

### Typography Scale
```scss
/* Font Families */
$font-primary: 'Roboto', -apple-system, BlinkMacSystemFont, sans-serif;
$font-mono: 'Roboto Mono', 'Fira Code', monospace;

/* Font Sizes */
$text-xs: 0.75rem;    /* 12px - Small labels */
$text-sm: 0.875rem;   /* 14px - Body text */
$text-base: 1rem;     /* 16px - Default text */
$text-lg: 1.125rem;   /* 18px - Large text */
$text-xl: 1.25rem;    /* 20px - Headings */
$text-2xl: 1.5rem;    /* 24px - Page titles */
$text-3xl: 1.875rem;  /* 30px - Hero text */

/* Font Weights */
$font-normal: 400;
$font-medium: 500;
$font-semibold: 600;
$font-bold: 700;
```

### Spacing Scale
```scss
/* Spacing Units (8px base) */
$space-1: 0.25rem;   /* 4px */
$space-2: 0.5rem;    /* 8px */
$space-3: 0.75rem;   /* 12px */
$space-4: 1rem;      /* 16px */
$space-5: 1.25rem;   /* 20px */
$space-6: 1.5rem;    /* 24px */
$space-8: 2rem;      /* 32px */
$space-10: 2.5rem;   /* 40px */
$space-12: 3rem;     /* 48px */
$space-16: 4rem;     /* 64px */
```

## 🧱 Component Design Rules

### Buttons
```scss
/* Primary Button */
.v-btn--primary {
  background: $primary-medium;
  color: white;
  border: none;
  border-radius: 4px;
  padding: $space-2 $space-4;
  font-weight: $font-medium;
  transition: background-color 0.2s ease;
  
  &:hover {
    background: $primary-medium-hover;
  }
  
  &:disabled {
    background: $neutral-30;
    color: $neutral-50;
  }
}

/* Secondary Button */
.v-btn--secondary {
  background: transparent;
  color: $primary-medium;
  border: 1px solid $primary-medium;
  border-radius: 4px;
  padding: $space-2 $space-4;
  
  &:hover {
    background: $primary-light-bg;
  }
}

/* Accent Button */
.v-btn--accent {
  background: $primary-accent;
  color: $primary-dark;
  border: none;
  border-radius: 4px;
  padding: $space-2 $space-4;
  font-weight: $font-medium;
  
  &:hover {
    background: $primary-accent-hover;
  }
}

/* Text Button */
.v-btn--text {
  background: transparent;
  color: $primary-medium;
  border: none;
  padding: $space-2 $space-4;
  
  &:hover {
    background: $primary-light-bg;
  }
}
```

### Input Fields
```scss
/* Text Input */
.v-text-field {
  .v-field {
    border: 1px solid $neutral-30;
    border-radius: 4px;
    padding: $space-3 $space-4;
    font-size: $text-sm;
    transition: border-color 0.2s ease;
    
    &:focus-within {
      border-color: $primary-medium;
      box-shadow: 0 0 0 2px rgba(28, 110, 164, 0.2);
    }
  }
  
  &.v-field--error .v-field {
    border-color: $error-red;
  }
}

/* Search Input */
.v-text-field--search {
  .v-field {
    &:focus-within {
      border-color: $primary-light;
      box-shadow: 0 0 0 2px rgba(51, 161, 224, 0.2);
    }
  }
}
```

### Cards
```scss
/* Note Card */
.v-card {
  background: $surface-primary;
  border: 1px solid $neutral-30;
  border-radius: 8px;
  padding: $space-6;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  transition: box-shadow 0.2s ease;
  
  &:hover {
    box-shadow: 0 4px 12px rgba(21, 77, 113, 0.15);
  }
  
  &.note-card--pinned {
    border-left: 4px solid $primary-medium;
    background: $primary-light-bg;
  }
  
  &.note-card--featured {
    border: 1px solid $primary-light;
    background: linear-gradient(135deg, $surface-primary 0%, $primary-light-bg 100%);
  }
}
```

### Note Editor
```scss
/* Rich Text Editor */
.note-editor {
  .tiptap {
    min-height: 200px;
    padding: $space-4;
    border: 1px solid $neutral-30;
    border-radius: 8px;
    font-size: $text-base;
    line-height: 1.6;
    
    &:focus {
      border-color: $primary-medium;
      outline: none;
      box-shadow: 0 0 0 2px rgba(28, 110, 164, 0.1);
    }
  }
  
  .editor-toolbar {
    border-bottom: 1px solid $neutral-30;
    padding: $space-2 $space-4;
    background: $surface-secondary;
    border-radius: 8px 8px 0 0;
  }
  
  .editor-toolbar-button {
    color: $primary-medium;
    
    &:hover {
      background: $primary-light-bg;
    }
    
    &.active {
      background: $primary-medium;
      color: white;
    }
  }
}
```

## 🎯 UX Rules

### Interaction States
- **Hover**: Subtle color change (200ms ease transition)
- **Focus**: Primary medium outline with 2px border
- **Active**: Slightly darker primary color
- **Disabled**: 50% opacity, no interaction
- **Loading**: Skeleton animation or spinner

### Loading States
- **Skeleton**: Animated gray rectangles matching content shape
- **Spinner**: Circular progress indicator for actions
- **Skeleton duration**: 1.5s infinite animation
- **Loading overlay**: Semi-transparent overlay with spinner

### Error States
- **Field errors**: Red border + error message below
- **Global errors**: Toast notification at top
- **Validation**: Real-time feedback on blur
- **Network errors**: Retry button with error message

### Success States
- **Form submission**: Green checkmark + success message
- **Note saved**: Subtle green border flash
- **Sync complete**: Green indicator in status bar

## 📱 Responsive Breakpoints

### Vuetify Breakpoints
```scss
// Vuetify breakpoint system
$breakpoint-xs: 0;
$breakpoint-sm: 600px;
$breakpoint-md: 960px;
$breakpoint-lg: 1264px;
$breakpoint-xl: 1904px;

// Custom breakpoints for notes app
$mobile: 320px;      /* Small phones */
$tablet: 768px;      /* Tablets */
$desktop: 1024px;    /* Desktop */
$large: 1440px;      /* Large screens */
```

### Layout Adaptations
```scss
// Mobile layout
@media (max-width: $breakpoint-sm) {
  .note-list {
    grid-template-columns: 1fr;
    gap: $space-4;
  }
  
  .note-editor {
    .editor-toolbar {
      flex-wrap: wrap;
    }
  }
}

// Tablet layout
@media (min-width: $breakpoint-sm) and (max-width: $breakpoint-md) {
  .note-list {
    grid-template-columns: repeat(2, 1fr);
    gap: $space-6;
  }
}

// Desktop layout
@media (min-width: $breakpoint-md) {
  .note-list {
    grid-template-columns: repeat(3, 1fr);
    gap: $space-8;
  }
}
```

## 🎨 Visual Hierarchy

### Text Hierarchy
1. **Page Title**: `text-3xl`, `font-bold`, `neutral-90`
2. **Section Headers**: `text-xl`, `font-semibold`, `neutral-80`
3. **Note Titles**: `text-lg`, `font-medium`, `neutral-80`
4. **Body Text**: `text-sm`, `font-normal`, `neutral-80`
5. **Helper Text**: `text-xs`, `font-normal`, `neutral-50`

### Spacing Hierarchy
- **Section spacing**: `space-8` (32px)
- **Card spacing**: `space-6` (24px)
- **Element spacing**: `space-4` (16px)
- **Inline spacing**: `space-2` (8px)

### Color Hierarchy
- **Primary actions**: Primary Medium (#1C6EA4)
- **Secondary actions**: Primary Light (#33A1E0)
- **Accent actions**: Primary Accent (#FFF9AF)
- **Success states**: Material Green
- **Warning states**: Material Orange
- **Error states**: Material Red

## 🎯 Component Consistency Rules

### Note Components
- **Note Cards**: Consistent padding, border radius, shadow
- **Note Editor**: Standard toolbar, consistent formatting
- **Note List**: Uniform grid layout, consistent spacing
- **Note Search**: Standard input styling, consistent results

### Layout Components
- **Header**: Fixed height, consistent navigation
- **Sidebar**: Standard width, consistent navigation
- **Main Content**: Consistent padding, responsive grid
- **Footer**: Standard height, consistent information

### Form Components
- **Input Fields**: Consistent validation states
- **Buttons**: Standard sizes and colors
- **Modals**: Consistent overlay and positioning
- **Dropdowns**: Standard styling and behavior

## 🚫 Design Anti-Patterns

### ❌ DON'T:
- **Use inline Tailwind classes**: `class="bg-blue-500 text-white p-4"` ❌
- **Use inline CSS styles**: `style="background: blue; color: white"` ❌
- **Mix different design systems** (Material + custom)
- **Use more than 3 font sizes per screen**
- **Create custom colors without adding to theme**
- **Ignore hover/focus states**
- **Make buttons too small** (< 44px touch target)
- **Use inconsistent spacing**
- **Ignore accessibility guidelines**
- **Define styles in component files** instead of global.scss
- **Use arbitrary Tailwind values** without component classes

### ✅ DO:
- **Use predefined component classes**: `class="btn-primary"` ✅
- **Define all styles in global.scss** using `@layer components`
- **Use Tailwind utilities within component classes** only
- **Maintain consistent spacing scale**
- **Follow Material Design principles**
- **Test on multiple screen sizes**
- **Ensure proper contrast ratios**
- **Use semantic color names**
- **Implement proper focus management**
- **Follow WCAG 2.1 AA guidelines**
- **Create reusable component classes** for common patterns

## 🎯 Tailwind CSS Implementation Rules

### Global Style File Structure
```scss
// src/assets/styles/global.scss
@tailwind base;
@tailwind components;
@tailwind utilities;

// Custom Tailwind configuration
@layer base {
  // Base styles using Tailwind
  body { @apply font-sans text-gray-900 bg-gray-50; }
  h1 { @apply text-3xl font-bold text-gray-900; }
  h2 { @apply text-xl font-semibold text-gray-800; }
  h3 { @apply text-lg font-medium text-gray-800; }
}

@layer components {
  // Button Components
  .btn-primary { 
    @apply bg-blue-600 text-white px-4 py-2 rounded-md font-medium 
           hover:bg-blue-700 focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 
           disabled:opacity-50 disabled:cursor-not-allowed transition-colors;
  }
  
  .btn-secondary { 
    @apply bg-transparent text-blue-600 border border-blue-600 px-4 py-2 rounded-md 
           font-medium hover:bg-blue-50 focus:ring-2 focus:ring-blue-500 
           focus:ring-offset-2 transition-colors;
  }
  
  .btn-accent { 
    @apply bg-yellow-200 text-blue-900 px-4 py-2 rounded-md font-medium 
           hover:bg-yellow-300 focus:ring-2 focus:ring-yellow-400 
           focus:ring-offset-2 transition-colors;
  }
  
  .btn-text { 
    @apply bg-transparent text-blue-600 px-4 py-2 rounded-md font-medium 
           hover:bg-blue-50 focus:ring-2 focus:ring-blue-500 
           focus:ring-offset-2 transition-colors;
  }
  
  // Card Components
  .card-note { 
    @apply bg-white border border-gray-200 rounded-lg p-6 shadow-sm 
           hover:shadow-md transition-shadow duration-200;
  }
  
  .card-pinned { 
    @apply bg-blue-50 border-l-4 border-blue-600;
  }
  
  .card-featured { 
    @apply border-blue-300 bg-gradient-to-br from-white to-blue-50;
  }
  
  // Input Components
  .input-field { 
    @apply border border-gray-300 rounded-md px-4 py-3 text-sm 
           focus:border-blue-600 focus:ring-2 focus:ring-blue-200 
           focus:outline-none transition-colors;
  }
  
  .input-search { 
    @apply border border-gray-300 rounded-md px-4 py-3 text-sm 
           focus:border-blue-400 focus:ring-2 focus:ring-blue-100 
           focus:outline-none transition-colors;
  }
  
  .input-error { 
    @apply border-red-300 focus:border-red-500 focus:ring-red-200;
  }
  
  // Layout Components
  .container-main { 
    @apply max-w-7xl mx-auto px-4 sm:px-6 lg:px-8;
  }
  
  .grid-notes { 
    @apply grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6;
  }
  
  .flex-toolbar { 
    @apply flex items-center space-x-2 p-4 border-b border-gray-200 bg-white;
  }
  
  // Navigation Components
  .nav-item { 
    @apply text-gray-700 px-3 py-2 rounded-md text-sm font-medium 
           hover:bg-blue-50 hover:text-blue-700 transition-colors;
  }
  
  .nav-item-active { 
    @apply bg-blue-100 text-blue-700;
  }
  
  // Note Editor Components
  .editor-toolbar { 
    @apply flex items-center space-x-1 p-2 border-b border-gray-200 bg-gray-50;
  }
  
  .editor-button { 
    @apply p-2 text-gray-600 hover:bg-gray-200 hover:text-gray-900 
           rounded transition-colors;
  }
  
  .editor-button-active { 
    @apply bg-blue-100 text-blue-700;
  }
  
  .editor-content { 
    @apply min-h-48 p-4 border border-gray-300 rounded-md 
           focus:border-blue-600 focus:ring-2 focus:ring-blue-200 
           focus:outline-none transition-colors;
  }
  
  // Status Components
  .status-synced { @apply text-green-600; }
  .status-syncing { @apply text-blue-600; }
  .status-pending { @apply text-yellow-600; }
  .status-error { @apply text-red-600; }
  
  // Tag Components
  .tag-note { 
    @apply bg-blue-50 text-blue-700 border border-blue-200 
           rounded-full px-3 py-1 text-xs font-medium 
           hover:bg-blue-100 transition-colors;
  }
  
  // Search Components
  .search-highlight { 
    @apply bg-yellow-200 text-blue-900 px-1 rounded;
  }
}

@layer utilities {
  // Custom utilities if needed
  .text-balance { text-wrap: balance; }
}
```

### Component Usage Examples
```vue
<!-- ✅ CORRECT: Using predefined component classes -->
<template>
  <div class="container-main">
    <div class="grid-notes">
      <div class="card-note card-pinned">
        <h3 class="text-lg font-medium text-gray-800">Note Title</h3>
        <p class="text-sm text-gray-600">Note content...</p>
        <div class="flex space-x-2 mt-4">
          <button class="btn-primary">Edit</button>
          <button class="btn-secondary">Delete</button>
        </div>
      </div>
    </div>
  </div>
</template>

<!-- ❌ WRONG: Using inline Tailwind classes -->
<template>
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      <div class="bg-white border border-gray-200 rounded-lg p-6 shadow-sm">
        <!-- This is NOT allowed -->
      </div>
    </div>
  </div>
</template>
```

### File Organization
```
src/assets/styles/
├── global.scss          # Main Tailwind file with all components
├── components/          # Component-specific styles (if needed)
│   ├── buttons.scss
│   ├── cards.scss
│   └── forms.scss
└── utilities/           # Custom utilities
    └── animations.scss
```

### Development Workflow
1. **Design new component** → Add to global.scss
2. **Use component class** → Apply in Vue template
3. **Test responsiveness** → Verify on all breakpoints
4. **Document usage** → Add to component library
5. **Code review** → Ensure no inline styles

## 🎨 Additional Component Styles

### Navigation Components
```scss
/* App Header */
.app-header {
  background: $primary-dark;
  color: white;
  box-shadow: 0 2px 4px rgba(21, 77, 113, 0.1);
  
  .nav-item {
    color: white;
    
    &:hover {
      background: rgba(255, 255, 255, 0.1);
    }
    
    &.active {
      background: $primary-medium;
    }
  }
}

/* Sidebar */
.app-sidebar {
  background: $surface-primary;
  border-right: 1px solid $neutral-30;
  
  .sidebar-item {
    color: $neutral-80;
    
    &:hover {
      background: $primary-light-bg;
      color: $primary-medium;
    }
    
    &.active {
      background: $primary-light-bg;
      color: $primary-medium;
      border-right: 3px solid $primary-medium;
    }
  }
}
```

### Note-Specific Components
```scss
/* Note List */
.note-list {
  .note-item {
    border: 1px solid $neutral-30;
    border-radius: 8px;
    transition: all 0.2s ease;
    
    &:hover {
      border-color: $primary-light;
      box-shadow: 0 2px 8px rgba(51, 161, 224, 0.1);
    }
    
    &.selected {
      border-color: $primary-medium;
      background: $primary-light-bg;
    }
  }
}

/* Search Results */
.search-results {
  .search-highlight {
    background: $primary-accent;
    color: $primary-dark;
    padding: 2px 4px;
    border-radius: 3px;
  }
}

/* Tags */
.note-tag {
  background: $primary-light-bg;
  color: $primary-medium;
  border: 1px solid $primary-light;
  border-radius: 16px;
  padding: 4px 8px;
  font-size: $text-xs;
  
  &:hover {
    background: $primary-medium;
    color: white;
  }
}
```

### Status Indicators
```scss
/* Sync Status */
.sync-status {
  &.synced {
    color: $success-green;
  }
  
  &.syncing {
    color: $primary-medium;
  }
  
  &.pending {
    color: $warning-orange;
  }
  
  &.error {
    color: $error-red;
  }
}

/* Note Status */
.note-status {
  &.pinned {
    color: $primary-medium;
  }
  
  &.draft {
    color: $neutral-50;
  }
  
  &.published {
    color: $success-green;
  }
}
```

## 🎨 Theme Customization

### Light Theme
```scss
$light-theme: (
  'background': #ffffff,
  'surface': #fafafa,
  'primary': #1C6EA4,
  'secondary': #33A1E0,
  'accent': #FFF9AF,
  'text': #212121,
  'text-secondary': #424242,
  'primary-dark': #154D71,
  'primary-light': #33A1E0
);
```

### Dark Theme
```scss
$dark-theme: (
  'background': #121212,
  'surface': #1e1e1e,
  'primary': #33A1E0,
  'secondary': #1C6EA4,
  'accent': #FFF9AF,
  'text': #ffffff,
  'text-secondary': #b0b0b0,
  'primary-dark': #154D71,
  'primary-light': #33A1E0
);
```

### Theme Switching
```typescript
// Theme composable
export function useTheme() {
  const isDark = ref(false)
  
  const toggleTheme = () => {
    isDark.value = !isDark.value
    // Apply theme to Vuetify
  }
  
  return {
    isDark,
    toggleTheme
  }
}
```

## 📱 Mobile-First Design

### Touch Targets
- **Minimum size**: 44px x 44px
- **Recommended size**: 48px x 48px
- **Spacing**: 8px minimum between targets

### Gesture Support
- **Swipe**: Delete notes, navigate between notes
- **Pinch**: Zoom in note editor
- **Long press**: Context menus
- **Pull to refresh**: Sync notes

### Mobile Navigation
- **Bottom navigation**: Primary navigation
- **Floating action button**: Create new note
- **Drawer navigation**: Secondary navigation
- **Tab navigation**: Filter notes

## 🚨 Strict Enforcement Rules

### Code Review Checklist
- [ ] **NO inline Tailwind classes** in any Vue template
- [ ] **NO inline CSS styles** in any component
- [ ] **ALL styles defined** in global.scss using `@layer components`
- [ ] **Component classes follow** naming convention
- [ ] **Responsive design** tested on all breakpoints
- [ ] **Accessibility standards** met (WCAG 2.1 AA)
- [ ] **Consistent spacing** using predefined scale
- [ ] **Hover/focus states** properly implemented

### Automated Checks
```json
// .eslintrc.js - Add rules to prevent inline styles
{
  "rules": {
    "vue/no-inline-styles": "error",
    "vue/no-unused-class": "error"
  }
}
```

### Pre-commit Hooks
```bash
# Check for inline Tailwind classes
grep -r "class=\"[^\"]*bg-\|text-\|p-\|m-\|flex\|grid" src/components/ && exit 1

# Check for inline styles
grep -r "style=" src/components/ && exit 1
```

### Team Guidelines
1. **New developers** must read this guide before contributing
2. **Code reviews** must check for inline styles
3. **Design system** updates require team approval
4. **Component library** must be maintained and documented
5. **Breaking changes** to component classes require migration plan

---

**CRITICAL RULE**: No component ships without using predefined Tailwind component classes from global.scss. Every UI element must use the centralized design system. Inline styles and inline Tailwind classes are strictly prohibited and will be rejected in code review.
