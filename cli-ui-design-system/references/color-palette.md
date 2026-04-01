# Color Palette

Complete color specifications for CLI UI design system.

## Semantic Colors

### Primary (Violet)

```typescript
const primary = {
  50: '#F5F3FF',
  100: '#EDE9FE',
  200: '#DDD6FE',
  300: '#C4B5FD',
  400: '#A78BFA',
  500: '#8B5CF6', // Base
  600: '#7C3AED',
  700: '#6D28D9',
  800: '#5B21B6',
  900: '#4C1D95',
}
```

### Success (Green)

```typescript
const success = {
  50: '#F0FDF4',
  100: '#DCFCE7',
  200: '#BBF7D0',
  300: '#86EFAC',
  400: '#4ADE80',
  500: '#22C55E', // Base
  600: '#16A34A',
  700: '#15803D',
  800: '#166534',
  900: '#14532D',
}
```

### Warning (Amber)

```typescript
const warning = {
  50: '#FFFBEB',
  100: '#FEF3C7',
  200: '#FDE68A',
  300: '#FCD34D',
  400: '#FBBF24',
  500: '#F59E0B', // Base
  600: '#D97706',
  700: '#B45309',
  800: '#92400E',
  900: '#78350F',
}
```

### Error (Red)

```typescript
const error = {
  50: '#FEF2F2',
  100: '#FEE2E2',
  200: '#FECACA',
  300: '#FCA5A5',
  400: '#F87171',
  500: '#EF4444', // Base
  600: '#DC2626',
  700: '#B91C1C',
  800: '#991B1B',
  900: '#7F1D1D',
}
```

### Info (Blue)

```typescript
const info = {
  50: '#EFF6FF',
  100: '#DBEAFE',
  200: '#BFDBFE',
  300: '#93C5FD',
  400: '#60A5FA',
  500: '#3B82F6', // Base
  600: '#2563EB',
  700: '#1D4ED8',
  800: '#1E40AF',
  900: '#1E3A8A',
}
```

## Neutral Scale

```typescript
const gray = {
  50: '#F9FAFB',
  100: '#F3F4F6',
  200: '#E5E7EB',
  300: '#D1D5DB',
  400: '#9CA3AF',
  500: '#6B7280',
  600: '#4B5563',
  700: '#374151',
  800: '#1F2937',
  900: '#111827',
}
```

## Usage Guidelines

### Backgrounds

- `gray[50]` - Page background
- `gray[100]` - Card background
- `primary[50]` - Highlighted section
- `error[50]` - Error message background

### Text

- `gray[900]` - Primary text
- `gray[600]` - Body text
- `gray[500]` - Secondary text
- `gray[400]` - Placeholder text

### Borders

- `gray[200]` - Default borders
- `gray[300]` - Hover borders
- `primary[500]` - Active/Focus borders
- `error[500]` - Error borders

### Interactive States

```tsx
// Button states
<Box
  backgroundColor={isActive ? primary[600] : primary[500]}
  borderColor={isFocused ? primary[400] : primary[500]}
>
  <Text color={gray[50]}>Button</Text>
</Box>
```

## ANSI Fallbacks

For terminals with limited color support:

```typescript
const ansiFallbacks = {
  primary: 'ansi:magentaBright',
  success: 'ansi:green',
  warning: 'ansi:yellow',
  error: 'ansi:red',
  info: 'ansi:blue',
  gray: {
    50: 'ansi:white',
    500: 'ansi:whiteBright',
    900: 'ansi:blackBright',
  },
}
```
