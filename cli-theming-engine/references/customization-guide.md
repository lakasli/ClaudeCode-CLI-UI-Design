# Customization Guide

Guide for customizing and extending the theming system.

## Creating Custom Themes

### Basic Customization

```typescript
import { extendTheme } from './theme/utils'
import { defaultTheme } from './theme/themes'

const myTheme = extendTheme(defaultTheme, {
  name: 'my-theme',
  colors: {
    primary: '#FF6B6B',
    success: '#51CF66',
    background: {
      card: '#FFF5F5',
    },
  },
})
```

### Complete Custom Theme

```typescript
const completeCustomTheme: Theme = {
  name: 'brand',
  colors: {
    primary: '#0066CC',
    success: '#00C853',
    warning: '#FFB300',
    error: '#DD2C00',
    info: '#0091EA',
    background: {
      page: '#FAFAFA',
      card: '#FFFFFF',
      input: '#FFFFFF',
      overlay: 'rgba(0,0,0,0.5)',
    },
    text: {
      primary: '#212121',
      secondary: '#757575',
      muted: '#9E9E9E',
      inverse: '#FFFFFF',
    },
    border: '#E0E0E0',
    borderHover: '#BDBDBD',
    borderFocus: '#0066CC',
  },
  spacing: {
    0: 0,
    1: 1,
    2: 2,
    4: 4,
    8: 8,
    16: 16,
  },
  typography: {
    size: {
      xs: 10,
      sm: 12,
      base: 14,
      lg: 16,
      xl: 18,
      '2xl': 20,
    },
    weight: {
      normal: undefined,
      bold: true,
      dim: true,
    },
  },
  borders: {
    radius: {
      none: undefined,
      sm: 'single',
      md: 'round',
      lg: 'double',
    },
    style: {
      none: undefined,
      solid: 'single',
      double: 'double',
      rounded: 'round',
      bold: 'bold',
    },
  },
}
```

## Runtime Theme Switching

### Theme Provider Setup

```tsx
import React, { createContext, useContext, useState } from 'react'
import type { Theme } from './types'
import { registry } from './registry'

interface ThemeContextValue {
  theme: Theme
  setTheme: (name: string) => void
  availableThemes: string[]
}

const ThemeContext = createContext<ThemeContextValue | null>(null)

export function ThemeProvider({
  children,
  defaultTheme = 'default',
}: ThemeProviderProps) {
  const [themeName, setThemeName] = useState(defaultTheme)
  const theme = registry.get(themeName) || registry.get('default')!

  return (
    <ThemeContext.Provider
      value={{
        theme,
        setTheme: setThemeName,
        availableThemes: registry.list(),
      }}
    >
      {children}
    </ThemeContext.Provider>
  )
}

export function useTheme() {
  const context = useContext(ThemeContext)
  if (!context) {
    throw new Error('useTheme must be used within ThemeProvider')
  }
  return context
}
```

### Theme Switcher Component

```tsx
function ThemeSwitcher() {
  const { theme, setTheme, availableThemes } = useTheme()
  const [selectedIndex, setSelectedIndex] = useState(0)

  useInput((input, key) => {
    if (key.upArrow) {
      setSelectedIndex(i => Math.max(0, i - 1))
    }
    if (key.downArrow) {
      setSelectedIndex(i => Math.min(availableThemes.length - 1, i + 1))
    }
    if (key.return) {
      setTheme(availableThemes[selectedIndex])
    }
  })

  return (
    <Box flexDirection="column" gap={spacing[1]}>
      <Text bold>Select Theme:</Text>
      {availableThemes.map((name, index) => (
        <Box key={name} flexDirection="row" gap={spacing[2]}>
          <Text color={selectedIndex === index ? colors.primary[500] : colors.gray[400]}>
            {selectedIndex === index ? '>' : ' '}
          </Text>
          <Text
            color={theme.name === name ? colors.primary[500] : colors.gray[600]}
            bold={theme.name === name}
          >
            {name} {theme.name === name && '(active)'}
          </Text>
        </Box>
      ))}
    </Box>
  )
}
```

## Persisting User Preferences

### Storage Adapter

```typescript
interface StorageAdapter {
  get(key: string): string | null
  set(key: string, value: string): void
}

const fileStorage: StorageAdapter = {
  get(key) {
    try {
      return fs.readFileSync(
        path.join(os.homedir(), '.config', 'myapp', key),
        'utf-8'
      )
    } catch {
      return null
    }
  },
  set(key, value) {
    const dir = path.join(os.homedir(), '.config', 'myapp')
    fs.mkdirSync(dir, { recursive: true })
    fs.writeFileSync(path.join(dir, key), value)
  },
}
```

### Persistent Theme Provider

```tsx
function PersistentThemeProvider({
  children,
  storage = fileStorage,
}: PersistentThemeProviderProps) {
  const [themeName, setThemeName] = useState(() => {
    return storage.get('theme') || 'default'
  })

  const setTheme = useCallback((name: string) => {
    setThemeName(name)
    storage.set('theme', name)
  }, [storage])

  const theme = registry.get(themeName) || registry.get('default')!

  return (
    <ThemeContext.Provider
      value={{
        theme,
        setTheme,
        availableThemes: registry.list(),
      }}
    >
      {children}
    </ThemeContext.Provider>
  )
}
```

## Advanced Customization

### Component-Level Theme Overrides

```tsx
function ThemedCard({ theme: overrideTheme, children }) {
  const { theme: globalTheme } = useTheme()
  const theme = overrideTheme
    ? extendTheme(globalTheme, overrideTheme)
    : globalTheme

  return (
    <ThemeContext.Provider value={{ ...useTheme(), theme }}>
      <Box
        borderStyle="round"
        borderColor={theme.colors.border}
        backgroundColor={theme.colors.background.card}
      >
        {children}
      </Box>
    </ThemeContext.Provider>
  )
}
```

### Dynamic Theme Generation

```typescript
function generateThemeFromPrimary(primaryColor: string): Theme {
  // Generate color palette from primary color
  const colors = generatePalette(primaryColor)

  return {
    name: 'generated',
    colors: {
      primary: colors.primary,
      success: colors.success,
      warning: colors.warning,
      error: colors.error,
      info: colors.info,
      background: {
        page: colors.gray[50],
        card: '#FFFFFF',
        input: '#FFFFFF',
        overlay: 'rgba(0,0,0,0.5)',
      },
      text: {
        primary: colors.gray[900],
        secondary: colors.gray[600],
        muted: colors.gray[400],
        inverse: '#FFFFFF',
      },
      border: colors.gray[200],
      borderHover: colors.gray[300],
      borderFocus: colors.primary,
    },
    spacing: defaultSpacing,
    typography: defaultTypography,
    borders: defaultBorders,
  }
}
```

## Best Practices

### 1. Always Provide Fallbacks

```typescript
const color = theme.colors.primary || '#8B5CF6'
```

### 2. Use Semantic Colors

```tsx
// Good: Use semantic colors
<Text color={theme.colors.error}>Error message</Text>

// Bad: Hardcode colors
<Text color="#FF0000">Error message</Text>
```

### 3. Test All Themes

```tsx
// Preview all themes
function ThemePreview() {
  const themes = registry.list()

  return (
    <Box flexDirection="column" gap={spacing[4]}>
      {themes.map(name => (
        <ThemeProvider key={name} defaultTheme={name}>
          <Box borderStyle="round" padding={spacing[4]}>
            <Text bold>{name}</Text>
            <SampleComponents />
          </Box>
        </ThemeProvider>
      ))}
    </Box>
  )
}
```
