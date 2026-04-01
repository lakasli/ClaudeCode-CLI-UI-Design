---
name: cli-theming-engine
description: >-
  Implement theming system for CLI UIs with built-in themes and customization.
  Use when creating theme-aware components, switching themes, or persisting user preferences.
  Supports Default, Dark, Light, and Minimal themes.
---

# CLI Theming Engine

Theming system for React Ink CLI applications.

## Quick Start

### 1. Setup Theme Provider

```tsx
import { ThemeProvider } from './theme/ThemeProvider'

function App() {
  return (
    <ThemeProvider defaultTheme="default">
      <MainContent />
    </ThemeProvider>
  )
}
```

### 2. Use Theme in Components

```tsx
import { useTheme } from './theme/useTheme'

function Card({ children }) {
  const { colors, spacing } = useTheme()

  return (
    <Box
      borderStyle="round"
      borderColor={colors.border}
      backgroundColor={colors.background.card}
      padding={spacing[4]}
    >
      {children}
    </Box>
  )
}
```

### 3. Switch Themes

```tsx
function ThemeSwitcher() {
  const { theme, setTheme, availableThemes } = useTheme()

  return (
    <Select
      value={theme}
      onChange={setTheme}
      options={availableThemes}
    />
  )
}
```

## Built-in Themes

### Default Theme

Claude Code inspired theme with violet accents:

```typescript
const defaultTheme = {
  name: 'default',
  colors: {
    primary: '#8B5CF6',
    success: '#22C55E',
    warning: '#F59E0B',
    error: '#EF4444',
    info: '#3B82F6',
    background: {
      page: undefined,
      card: undefined,
      input: undefined,
    },
    text: {
      primary: '#111827',
      secondary: '#6B7280',
      muted: '#9CA3AF',
    },
    border: '#E5E7EB',
  },
}
```

### Dark Theme

High contrast dark theme:

```typescript
const darkTheme = {
  name: 'dark',
  colors: {
    primary: '#A78BFA',
    success: '#4ADE80',
    warning: '#FBBF24',
    error: '#F87171',
    info: '#60A5FA',
    background: {
      page: '#111827',
      card: '#1F2937',
      input: '#374151',
    },
    text: {
      primary: '#F9FAFB',
      secondary: '#D1D5DB',
      muted: '#9CA3AF',
    },
    border: '#374151',
  },
}
```

### Light Theme

Clean light theme:

```typescript
const lightTheme = {
  name: 'light',
  colors: {
    primary: '#7C3AED',
    success: '#16A34A',
    warning: '#D97706',
    error: '#DC2626',
    info: '#2563EB',
    background: {
      page: '#FFFFFF',
      card: '#F9FAFB',
      input: '#FFFFFF',
    },
    text: {
      primary: '#000000',
      secondary: '#4B5563',
      muted: '#6B7280',
    },
    border: '#E5E7EB',
  },
}
```

### Minimal Theme

No colors, borders only:

```typescript
const minimalTheme = {
  name: 'minimal',
  colors: {
    primary: undefined,
    success: undefined,
    warning: undefined,
    error: undefined,
    info: undefined,
    background: {
      page: undefined,
      card: undefined,
      input: undefined,
    },
    text: {
      primary: undefined,
      secondary: undefined,
      muted: undefined,
    },
    border: undefined,
  },
}
```

## Custom Themes

### Creating a Custom Theme

```typescript
const customTheme = {
  name: 'ocean',
  extends: 'default', // Inherit from default
  colors: {
    primary: '#0EA5E9', // Override primary color
    background: {
      page: '#F0F9FF',
      card: '#FFFFFF',
    },
  },
}
```

### Registering Custom Theme

```tsx
import { registerTheme } from './theme/registry'

registerTheme(customTheme)
```

## Theme Reference

See `references/theme-structure.md` for complete theme type definitions.
See `references/customization-guide.md` for advanced customization.
