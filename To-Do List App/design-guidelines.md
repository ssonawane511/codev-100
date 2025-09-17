# Design Guidelines
*Microsoft To Do App - Visual & UX Standards*

## 🎨 Design System Overview

### Theme Colors
```css
/* Primary Colors - Microsoft Fluent Design inspired */
--primary-blue: #0078d4;        /* Microsoft Blue */
--primary-blue-hover: #106ebe;  /* Darker blue for hover */
--primary-blue-light: #deecf9;  /* Light blue for backgrounds */

/* Neutral Colors */
--neutral-10: #fafafa;          /* Lightest background */
--neutral-20: #f5f5f5;          /* Light background */
--neutral-30: #edebe9;          /* Subtle borders */
--neutral-50: #a19f9d;          /* Disabled text */
--neutral-80: #323130;          /* Primary text */
--neutral-90: #201f1e;          /* Darkest text */

/* Semantic Colors */
--success-green: #107c10;       /* Completed tasks */
--warning-orange: #ff8c00;      /* Due soon */
--error-red: #d13438;           /* Overdue */
--info-blue: #0078d4;           /* Information */

/* Surface Colors */
--surface-primary: #ffffff;     /* Card backgrounds */
--surface-secondary: #fafafa;   /* Page backgrounds */
--surface-tertiary: #f5f5f5;    /* Subtle backgrounds */
```

### Typography Scale
```css
/* Font Families */
--font-primary: 'Segoe UI', -apple-system, BlinkMacSystemFont, sans-serif;
--font-mono: 'Cascadia Code', 'Fira Code', monospace;

/* Font Sizes */
--text-xs: 0.75rem;    /* 12px - Small labels */
--text-sm: 0.875rem;   /* 14px - Body text */
--text-base: 1rem;     /* 16px - Default text */
--text-lg: 1.125rem;   /* 18px - Large text */
--text-xl: 1.25rem;    /* 20px - Headings */
--text-2xl: 1.5rem;    /* 24px - Page titles */
--text-3xl: 1.875rem;  /* 30px - Hero text */

/* Font Weights */
--font-normal: 400;
--font-medium: 500;
--font-semibold: 600;
--font-bold: 700;
```

### Spacing Scale
```css
/* Spacing Units (4px base) */
--space-1: 0.25rem;   /* 4px */
--space-2: 0.5rem;    /* 8px */
--space-3: 0.75rem;   /* 12px */
--space-4: 1rem;      /* 16px */
--space-5: 1.25rem;   /* 20px */
--space-6: 1.5rem;    /* 24px */
--space-8: 2rem;      /* 32px */
--space-10: 2.5rem;   /* 40px */
--space-12: 3rem;     /* 48px */
--space-16: 4rem;     /* 64px */
```

## 🧱 Component Design Rules

### Buttons
```css
/* Primary Button */
.btn-primary {
  background: var(--primary-blue);
  color: white;
  border: none;
  border-radius: 4px;
  padding: var(--space-2) var(--space-4);
  font-weight: var(--font-medium);
  transition: background-color 0.2s ease;
}

.btn-primary:hover {
  background: var(--primary-blue-hover);
}

/* Secondary Button */
.btn-secondary {
  background: transparent;
  color: var(--primary-blue);
  border: 1px solid var(--primary-blue);
  border-radius: 4px;
  padding: var(--space-2) var(--space-4);
}
```

### Input Fields
```css
.input-field {
  border: 1px solid var(--neutral-30);
  border-radius: 4px;
  padding: var(--space-3) var(--space-4);
  font-size: var(--text-sm);
  transition: border-color 0.2s ease;
}

.input-field:focus {
  border-color: var(--primary-blue);
  outline: none;
  box-shadow: 0 0 0 2px var(--primary-blue-light);
}
```

### Cards
```css
.card {
  background: var(--surface-primary);
  border: 1px solid var(--neutral-30);
  border-radius: 8px;
  padding: var(--space-6);
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}
```

## 🎯 UX Rules

### Interaction States
- **Hover**: Subtle color change (200ms ease transition)
- **Focus**: Blue outline with 2px border
- **Active**: Slightly darker color
- **Disabled**: 50% opacity, no interaction

### Loading States
- **Skeleton**: Animated gray rectangles matching content shape
- **Spinner**: Circular progress indicator for actions
- **Skeleton duration**: 1.5s infinite animation

### Error States
- **Field errors**: Red border + error message below
- **Global errors**: Toast notification at top
- **Validation**: Real-time feedback on blur

## 📱 Responsive Breakpoints

```css
/* Mobile First Approach */
--mobile: 320px;      /* Small phones */
--tablet: 768px;      /* Tablets */
--desktop: 1024px;    /* Desktop */
--large: 1440px;      /* Large screens */

/* Usage */
@media (min-width: 768px) {
  .container { max-width: 1200px; }
}
```

## 🎨 Visual Hierarchy

### Text Hierarchy
1. **Page Title**: `text-3xl`, `font-bold`, `neutral-90`
2. **Section Headers**: `text-xl`, `font-semibold`, `neutral-80`
3. **Card Titles**: `text-lg`, `font-medium`, `neutral-80`
4. **Body Text**: `text-sm`, `font-normal`, `neutral-80`
5. **Helper Text**: `text-xs`, `font-normal`, `neutral-50`

### Spacing Hierarchy
- **Section spacing**: `space-8` (32px)
- **Card spacing**: `space-6` (24px)
- **Element spacing**: `space-4` (16px)
- **Inline spacing**: `space-2` (8px)

## 🚫 Design Anti-Patterns

### ❌ DON'T:
- Mix different design systems (Material + Fluent)
- Use more than 3 font sizes per screen
- Create custom colors without adding to theme
- Use inline styles (except for dynamic values)
- Ignore hover/focus states
- Make buttons too small (< 44px touch target)

### ✅ DO:
- Use design tokens from theme
- Maintain consistent spacing
- Follow Microsoft Fluent Design principles
- Test on multiple screen sizes
- Ensure proper contrast ratios
- Use semantic color names

## 🎯 Component Consistency Rules

1. **All buttons** use the same padding, border-radius, and transition
2. **All inputs** follow the same focus state pattern
3. **All cards** use consistent shadow and border radius
4. **All text** follows the typography scale
5. **All spacing** uses the defined spacing scale

---

**Rule**: No component ships without matching the design system. No "creative" variations without approval.
