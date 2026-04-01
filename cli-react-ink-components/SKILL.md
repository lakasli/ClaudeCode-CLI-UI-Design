---
name: cli-react-ink-components
description: >-
  Build React Ink components following Claude Code patterns.
  Use when creating Box layouts, Text styling, or interactive UI elements for CLI tools.
  Provides component patterns for cards, forms, lists, and navigation.
---

# React Ink Components

Component patterns for building CLI UIs with React Ink.

## Quick Start

### Basic Layout

```tsx
import { Box, Text } from 'ink'
import { colors, spacing } from '../design-tokens'

function App() {
  return (
    <Box flexDirection="column" padding={spacing[4]}>
      <Text bold color={colors.primary[500]}>
        Hello CLI
      </Text>
    </Box>
  )
}
```

### Card Component

```tsx
function Card({ title, children }: CardProps) {
  return (
    <Box
      borderStyle="round"
      borderColor={colors.gray[200]}
      padding={spacing[4]}
      gap={spacing[2]}
    >
      <Text bold color={colors.gray[900]}>{title}</Text>
      {children}
    </Box>
  )
}
```

## Box Patterns

### Border Styles

```tsx
// Single line border
<Box borderStyle="single">Content</Box>

// Double line border
<Box borderStyle="double">Content</Box>

// Rounded corners
<Box borderStyle="round">Content</Box>

// Bold border
<Box borderStyle="bold">Content</Box>

// Single/double mix
<Box borderStyle="singleDouble">Content</Box>

// Double/single mix
<Box borderStyle="doubleSingle">Content</Box>

// Classic ASCII
<Box borderStyle="classic">Content</Box>
```

### Flex Layout

```tsx
// Row layout (default)
<Box flexDirection="row" gap={spacing[2]}>
  <Item1 />
  <Item2 />
</Box>

// Column layout
<Box flexDirection="column" gap={spacing[2]}>
  <Item1 />
  <Item2 />
</Box>

// Align items
<Box alignItems="center" justifyContent="space-between">
  <Left />
  <Right />
</Box>
```

## Text Patterns

### Styling

```tsx
// Bold text
<Text bold>Bold</Text>

// Dim text
<Text dim>Faded</Text>

// Italic
<Text italic>Italic</Text>

// Underline
<Text underline>Link</Text>

// Strikethrough
<Text strikethrough>Deleted</Text>

// Inverse
<Text inverse>Highlighted</Text>

// Colors
<Text color={colors.primary[500]}>Primary</Text>
<Text backgroundColor={colors.gray[100]}>Background</Text>
```

### Text Wrapping

```tsx
// Wrap (default)
<Text wrap="wrap">Long text...</Text>

// Truncate end
<Text wrap="truncate-end">Long text...</Text>

// Truncate start
<Text wrap="truncate-start">Long text...</Text>

// Truncate middle
<Text wrap="truncate-middle">Long text...</Text>
```

## Interactive Components

### Button

```tsx
import { useState } from 'react'
import { Box, Text, useInput } from 'ink'

function Button({ label, onClick }: ButtonProps) {
  const [isFocused, setIsFocused] = useState(false)

  useInput((input, key) => {
    if (isFocused && key.return) {
      onClick()
    }
  })

  return (
    <Box
      borderStyle={isFocused ? 'bold' : 'single'}
      borderColor={isFocused ? colors.primary[500] : colors.gray[300]}
      paddingX={spacing[4]}
      paddingY={spacing[1]}
    >
      <Text color={isFocused ? colors.primary[500] : colors.gray[700]}>
        {label}
      </Text>
    </Box>
  )
}
```

### Select

```tsx
import { useState } from 'react'
import { Box, Text } from 'ink'
import SelectInput from 'ink-select-input'

function Select({ items, onSelect }: SelectProps) {
  return (
    <Box flexDirection="column">
      <SelectInput
        items={items}
        onSelect={onSelect}
        indicatorComponent={<Text color={colors.primary[500]}>{'>'}</Text>}
        itemComponent={({ label, isSelected }) => (
          <Text color={isSelected ? colors.primary[500] : colors.gray[600]}>
            {label}
          </Text>
        )}
      />
    </Box>
  )
}
```

### Spinner

```tsx
import { Spinner } from 'ink-spinner'

function Loading() {
  return (
    <Box gap={spacing[2]}>
      <Spinner type="dots" />
      <Text dim>Loading...</Text>
    </Box>
  )
}
```

## Component Reference

See `references/box-patterns.md` for Box layout patterns.
See `references/text-patterns.md` for Text styling patterns.
See `references/interactive-patterns.md` for Button, Select, Input patterns.
