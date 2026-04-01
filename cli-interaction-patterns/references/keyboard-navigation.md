# Keyboard Navigation

Keyboard navigation patterns for CLI applications.

## Key Handling Basics

### useInput Hook

```tsx
import { useInput } from 'ink'

function App() {
  useInput((input, key) => {
    // input: string - the character pressed
    // key: object - modifier keys state
  })
}
```

### Key Object Properties

```tsx
{
  upArrow: boolean
  downArrow: boolean
  leftArrow: boolean
  rightArrow: boolean
  return: boolean
  escape: boolean
  ctrl: boolean
  shift: boolean
  tab: boolean
  backspace: boolean
  delete: boolean
  meta: boolean
}
```

## Navigation Patterns

### Arrow Key Navigation

```tsx
function ListNavigation({ items }) {
  const [selectedIndex, setSelectedIndex] = useState(0)

  useInput((input, key) => {
    if (key.upArrow) {
      setSelectedIndex(i => Math.max(0, i - 1))
    }
    if (key.downArrow) {
      setSelectedIndex(i => Math.min(items.length - 1, i + 1))
    }
  })

  return (
    <Box flexDirection="column">
      {items.map((item, index) => (
        <Text
          key={item.id}
          color={selectedIndex === index ? colors.primary[500] : colors.gray[600]}
          bold={selectedIndex === index}
        >
          {selectedIndex === index ? '> ' : '  '}{item.label}
        </Text>
      ))}
    </Box>
  )
}
```

### Tab Navigation

```tsx
function TabNavigation({ fields }) {
  const [focusedIndex, setFocusedIndex] = useState(0)

  useInput((input, key) => {
    if (key.tab) {
      if (key.shift) {
        // Shift+Tab: go backward
        setFocusedIndex(i => (i - 1 + fields.length) % fields.length)
      } else {
        // Tab: go forward
        setFocusedIndex(i => (i + 1) % fields.length)
      }
    }
  })

  return (
    <Box flexDirection="column" gap={spacing[2]}>
      {fields.map((field, index) => (
        <Input
          key={field.id}
          isFocused={focusedIndex === index}
          {...field}
        />
      ))}
    </Box>
  )
}
```

### Vim-style Navigation

```tsx
function VimNavigation() {
  const [position, setPosition] = useState({ x: 0, y: 0 })

  useInput((input, key) => {
    if (input === 'h' || key.leftArrow) {
      setPosition(p => ({ ...p, x: Math.max(0, p.x - 1) }))
    }
    if (input === 'j' || key.downArrow) {
      setPosition(p => ({ ...p, y: p.y + 1 }))
    }
    if (input === 'k' || key.upArrow) {
      setPosition(p => ({ ...p, y: Math.max(0, p.y - 1) }))
    }
    if (input === 'l' || key.rightArrow) {
      setPosition(p => ({ ...p, x: p.x + 1 }))
    }
  })

  return <Grid position={position} />
}
```

## Action Shortcuts

### Global Shortcuts

```tsx
function App() {
  useInput((input, key) => {
    // Quit
    if (input === 'q' || (key.ctrl && input === 'c')) {
      process.exit(0)
    }

    // Help
    if (input === '?' || input === 'h') {
      showHelp()
    }

    // Refresh
    if (input === 'r' || key.return) {
      refresh()
    }

    // Search
    if (input === '/') {
      enterSearchMode()
    }

    // Escape
    if (key.escape) {
      exitCurrentMode()
    }
  })
}
```

### Contextual Shortcuts

```tsx
function FileManager() {
  const [mode, setMode] = useState<'normal' | 'visual' | 'insert'>('normal')

  useInput((input, key) => {
    if (mode === 'normal') {
      if (input === 'd') deleteFile()
      if (input === 'r') renameFile()
      if (input === 'y') copyFile()
      if (input === 'i') setMode('insert')
      if (input === 'v') setMode('visual')
    }

    if (mode === 'insert') {
      if (key.escape) setMode('normal')
      // Handle text input
    }
  })
}
```

## Focus Management

### Focus Manager Hook

```tsx
function useFocusManager(itemIds: string[]) {
  const [focusedId, setFocusedId] = useState(itemIds[0])

  const focusNext = useCallback(() => {
    const currentIndex = itemIds.indexOf(focusedId)
    const nextIndex = (currentIndex + 1) % itemIds.length
    setFocusedId(itemIds[nextIndex])
  }, [focusedId, itemIds])

  const focusPrevious = useCallback(() => {
    const currentIndex = itemIds.indexOf(focusedId)
    const prevIndex = (currentIndex - 1 + itemIds.length) % itemIds.length
    setFocusedId(itemIds[prevIndex])
  }, [focusedId, itemIds])

  const focusById = useCallback((id: string) => {
    if (itemIds.includes(id)) {
      setFocusedId(id)
    }
  }, [itemIds])

  return { focusedId, focusNext, focusPrevious, focusById }
}
```

### Focusable Component

```tsx
function FocusableInput({
  id,
  isFocused,
  onFocus,
  ...props
}: FocusableInputProps) {
  useInput((input, key) => {
    if (!isFocused) return

    if (key.tab) {
      onFocus('next')
    }
    if (key.shift && key.tab) {
      onFocus('previous')
    }
  })

  return (
    <Box
      borderStyle={isFocused ? 'bold' : 'single'}
      borderColor={isFocused ? colors.primary[500] : colors.gray[300]}
    >
      <Input {...props} />
    </Box>
  )
}
```

## Special Keys

### Modifier Combinations

```tsx
useInput((input, key) => {
  // Ctrl+C - Copy/Cancel
  if (key.ctrl && input === 'c') {
    handleCancel()
  }

  // Ctrl+V - Paste
  if (key.ctrl && input === 'v') {
    handlePaste()
  }

  // Ctrl+A - Select All
  if (key.ctrl && input === 'a') {
    selectAll()
  }

  // Ctrl+S - Save
  if (key.ctrl && input === 's') {
    save()
  }

  // Ctrl+Z - Undo
  if (key.ctrl && input === 'z') {
    undo()
  }

  // Shift+Enter - New line
  if (key.shift && key.return) {
    insertNewLine()
  }
})
```

### Function Keys

```tsx
useInput((input, key) => {
  // F1 - Help
  if (input === '\u001bOP') {
    showHelp()
  }

  // F5 - Refresh
  if (input === '\u001b[15~') {
    refresh()
  }

  // F10 - Menu
  if (input === '\u001b[21~') {
    toggleMenu()
  }
})
```

## Best Practices

### 1. Consistent Navigation

```tsx
// Always support both Tab and arrow keys
useInput((input, key) => {
  if (key.tab || key.downArrow) {
    focusNext()
  }
  if ((key.shift && key.tab) || key.upArrow) {
    focusPrevious()
  }
})
```

### 2. Visual Feedback

```tsx
// Always show which element is focused
<Box
  borderStyle={isFocused ? 'bold' : 'single'}
  borderColor={isFocused ? colors.primary[500] : colors.gray[300]}
  backgroundColor={isFocused ? colors.primary[50] : undefined}
>
  <Text color={isFocused ? colors.primary[500] : colors.gray[600]}>
    {label}
  </Text>
</Box>
```

### 3. Escape to Cancel

```tsx
// Always allow Escape to exit current mode
useInput((input, key) => {
  if (key.escape) {
    if (isEditing) {
      cancelEdit()
    } else if (isModalOpen) {
      closeModal()
    } else {
      exitApp()
    }
  }
})
```

### 4. Confirm Actions

```tsx
// Require Enter to confirm destructive actions
useInput((input, key) => {
  if (key.return && isDangerousAction) {
    confirmAction()
  }
})
```
