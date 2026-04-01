# Theme Structure

Complete theme type definitions and structure.

## Theme Interface

```typescript
interface Theme {
  name: string
  extends?: string
  colors: ColorScheme
  spacing: SpacingScale
  typography: TypographyScale
  borders: BorderStyles
}
```

## Color Scheme

```typescript
interface ColorScheme {
  // Semantic colors
  primary: string
  success: string
  warning: string
  error: string
  info: string

  // Background colors
  background: {
    page: string | undefined
    card: string | undefined
    input: string | undefined
    overlay: string | undefined
  }

  // Text colors
  text: {
    primary: string | undefined
    secondary: string | undefined
    muted: string | undefined
    inverse: string | undefined
  }

  // Border colors
  border: string | undefined
  borderHover: string | undefined
  borderFocus: string | undefined
}
```

## Spacing Scale

```typescript
interface SpacingScale {
  0: number
  1: number   // 1 char
  2: number   // 2 chars
  4: number   // 4 chars
  8: number   // 8 chars
  16: number  // 16 chars
}

const defaultSpacing: SpacingScale = {
  0: 0,
  1: 1,
  2: 2,
  4: 4,
  8: 8,
  16: 16,
}
```

## Typography Scale

```typescript
interface TypographyScale {
  size: {
    xs: number
    sm: number
    base: number
    lg: number
    xl: number
    '2xl': number
  }
  weight: {
    normal: undefined
    bold: true
    dim: true
  }
}

const defaultTypography: TypographyScale = {
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
}
```

## Border Styles

```typescript
interface BorderStyles {
  radius: {
    none: undefined
    sm: 'single'
    md: 'round'
    lg: 'double'
  }
  style: {
    none: undefined
    solid: 'single'
    double: 'double'
    rounded: 'round'
    bold: 'bold'
  }
}

const defaultBorders: BorderStyles = {
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
}
```

## Complete Theme Example

```typescript
const completeTheme: Theme = {
  name: 'complete',
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
      overlay: undefined,
    },
    text: {
      primary: '#111827',
      secondary: '#6B7280',
      muted: '#9CA3AF',
      inverse: '#F9FAFB',
    },
    border: '#E5E7EB',
    borderHover: '#D1D5DB',
    borderFocus: '#8B5CF6',
  },
  spacing: defaultSpacing,
  typography: defaultTypography,
  borders: defaultBorders,
}
```

## Theme Inheritance

```typescript
function extendTheme(base: Theme, extension: Partial<Theme>): Theme {
  return {
    ...base,
    ...extension,
    colors: {
      ...base.colors,
      ...extension.colors,
      background: {
        ...base.colors.background,
        ...extension.colors?.background,
      },
      text: {
        ...base.colors.text,
        ...extension.colors?.text,
      },
    },
  }
}

// Usage
const customTheme = extendTheme(defaultTheme, {
  name: 'custom',
  colors: {
    primary: '#FF0000',
  },
})
```

## Theme Registry

```typescript
class ThemeRegistry {
  private themes = new Map<string, Theme>()

  register(theme: Theme) {
    this.themes.set(theme.name, theme)
  }

  get(name: string): Theme | undefined {
    const theme = this.themes.get(name)
    if (theme?.extends) {
      const parent = this.get(theme.extends)
      if (parent) {
        return extendTheme(parent, theme)
      }
    }
    return theme
  }

  list(): string[] {
    return Array.from(this.themes.keys())
  }
}

export const registry = new ThemeRegistry()
```

## Type Exports

```typescript
export type {
  Theme,
  ColorScheme,
  SpacingScale,
  TypographyScale,
  BorderStyles,
}
```
