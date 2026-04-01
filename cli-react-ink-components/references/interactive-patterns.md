# Interactive Patterns

Interactive component patterns for Ink CLI applications.

## Button

### Basic Button

```tsx
import { useState } from 'react'
import { Box, Text, useInput } from 'ink'

function Button({ label, onClick, isFocused }: ButtonProps) {
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
      backgroundColor={isFocused ? colors.primary[50] : undefined}
    >
      <Text
        color={isFocused ? colors.primary[500] : colors.gray[700]}
        bold={isFocused}
      >
        {label}
      </Text>
    </Box>
  )
}
```

### Button with States

```tsx
function Button({ label, onClick, variant = 'primary' }: ButtonProps) {
  const [isFocused, setIsFocused] = useState(false)
  const [isPressed, setIsPressed] = useState(false)

  const variants = {
    primary: {
      border: colors.primary[500],
      text: colors.primary[500],
      bg: colors.primary[50],
    },
    secondary: {
      border: colors.gray[300],
      text: colors.gray[700],
      bg: colors.gray[100],
    },
    danger: {
      border: colors.error[500],
      text: colors.error[500],
      bg: colors.error[50],
    },
  }

  const style = variants[variant]

  return (
    <Box
      borderStyle={isFocused ? 'bold' : 'single'}
      borderColor={isPressed ? style.border : isFocused ? style.border : colors.gray[300]}
      backgroundColor={isFocused ? style.bg : undefined}
      paddingX={spacing[4]}
      paddingY={spacing[1]}
    >
      <Text color={isFocused ? style.text : colors.gray[700]} bold={isFocused}>
        {label}
      </Text>
    </Box>
  )
}
```

## Select

### Single Select

```tsx
import { useState } from 'react'
import { Box, Text } from 'ink'

interface SelectItem {
  label: string
  value: string
}

function Select({ items, onSelect }: SelectProps) {
  const [selectedIndex, setSelectedIndex] = useState(0)

  useInput((input, key) => {
    if (key.upArrow) {
      setSelectedIndex(i => Math.max(0, i - 1))
    }
    if (key.downArrow) {
      setSelectedIndex(i => Math.min(items.length - 1, i + 1))
    }
    if (key.return) {
      onSelect(items[selectedIndex])
    }
  })

  return (
    <Box flexDirection="column" gap={spacing[1]}>
      {items.map((item, index) => {
        const isSelected = index === selectedIndex
        return (
          <Box key={item.value} flexDirection="row" gap={spacing[2]}>
            <Text color={isSelected ? colors.primary[500] : colors.gray[400]}>
              {isSelected ? '>' : ' '}
            </Text>
            <Text
              color={isSelected ? colors.primary[500] : colors.gray[600]}
              bold={isSelected}
            >
              {item.label}
            </Text>
          </Box>
        )
      })}
    </Box>
  )
}
```

### Multi Select

```tsx
function MultiSelect({ items, onChange }: MultiSelectProps) {
  const [selectedIndices, setSelectedIndices] = useState<Set<number>>(new Set())
  const [focusedIndex, setFocusedIndex] = useState(0)

  useInput((input, key) => {
    if (key.upArrow) {
      setFocusedIndex(i => Math.max(0, i - 1))
    }
    if (key.downArrow) {
      setFocusedIndex(i => Math.min(items.length - 1, i + 1))
    }
    if (key.return) {
      setSelectedIndices(prev => {
        const next = new Set(prev)
        if (next.has(focusedIndex)) {
          next.delete(focusedIndex)
        } else {
          next.add(focusedIndex)
        }
        return next
      })
    }
  })

  return (
    <Box flexDirection="column" gap={spacing[1]}>
      {items.map((item, index) => {
        const isFocused = index === focusedIndex
        const isSelected = selectedIndices.has(index)
        return (
          <Box key={item.value} flexDirection="row" gap={spacing[2]}>
            <Text color={isSelected ? colors.primary[500] : colors.gray[400]}>
              {isSelected ? '[✓]' : '[ ]'}
            </Text>
            <Text
              color={isFocused ? colors.primary[500] : colors.gray[600]}
              bold={isFocused}
            >
              {item.label}
            </Text>
          </Box>
        )
      })}
    </Box>
  )
}
```

## Input

### Text Input

```tsx
import { useState } from 'react'
import { Box, Text, useInput } from 'ink'

function TextInput({ placeholder, onSubmit }: InputProps) {
  const [value, setValue] = useState('')
  const [cursorPosition, setCursorPosition] = useState(0)

  useInput((input, key) => {
    if (key.return) {
      onSubmit(value)
      return
    }
    if (key.backspace || key.delete) {
      setValue(v => v.slice(0, -1))
      return
    }
    if (input) {
      setValue(v => v + input)
    }
  })

  return (
    <Box>
      <Text color={colors.gray[400]}>{placeholder}: </Text>
      <Text>{value}</Text>
      <Text color={colors.primary[500]}>▌</Text>
    </Box>
  )
}
```

### Password Input

```tsx
function PasswordInput({ placeholder, onSubmit }: InputProps) {
  const [value, setValue] = useState('')

  useInput((input, key) => {
    if (key.return) {
      onSubmit(value)
      return
    }
    if (key.backspace) {
      setValue(v => v.slice(0, -1))
      return
    }
    if (input) {
      setValue(v => v + input)
    }
  })

  return (
    <Box>
      <Text color={colors.gray[400]}>{placeholder}: </Text>
      <Text>{'•'.repeat(value.length)}</Text>
      <Text color={colors.primary[500]}>▌</Text>
    </Box>
  )
}
```

## Progress Indicators

### Spinner

```tsx
import { useEffect, useState } from 'react'
import { Box, Text } from 'ink'

const SPINNER_FRAMES = ['⠋', '⠙', '⠹', '⠸', '⠼', '⠴', '⠦', '⠧', '⠇', '⠏']

function Spinner({ text = 'Loading...' }: SpinnerProps) {
  const [frame, setFrame] = useState(0)

  useEffect(() => {
    const timer = setInterval(() => {
      setFrame(f => (f + 1) % SPINNER_FRAMES.length)
    }, 80)
    return () => clearInterval(timer)
  }, [])

  return (
    <Box gap={spacing[2]}>
      <Text color={colors.primary[500]}>{SPINNER_FRAMES[frame]}</Text>
      <Text dim>{text}</Text>
    </Box>
  )
}
```

### Progress Bar

```tsx
function ProgressBar({ progress, total, label }: ProgressBarProps) {
  const percentage = Math.round((progress / total) * 100)
  const filled = Math.round((progress / total) * 20)
  const empty = 20 - filled

  return (
    <Box flexDirection="column" gap={spacing[1]}>
      <Text dim>{label}</Text>
      <Box>
        <Text color={colors.primary[500]}>{'█'.repeat(filled)}</Text>
        <Text color={colors.gray[300]}>{'░'.repeat(empty)}</Text>
        <Text dim> {percentage}%</Text>
      </Box>
    </Box>
  )
}
```

## Confirmation Dialog

```tsx
function ConfirmDialog({ message, onConfirm, onCancel }: ConfirmProps) {
  const [selected, setSelected] = useState<'yes' | 'no'>('yes')

  useInput((input, key) => {
    if (key.leftArrow || key.rightArrow) {
      setSelected(s => (s === 'yes' ? 'no' : 'yes'))
    }
    if (key.return) {
      selected === 'yes' ? onConfirm() : onCancel()
    }
  })

  return (
    <Box flexDirection="column" gap={spacing[2]}>
      <Text>{message}</Text>
      <Box gap={spacing[4]}>
        <Box
          borderStyle={selected === 'yes' ? 'bold' : 'single'}
          borderColor={selected === 'yes' ? colors.primary[500] : colors.gray[300]}
          paddingX={spacing[4]}
          paddingY={spacing[1]}
        >
          <Text color={selected === 'yes' ? colors.primary[500] : colors.gray[600]}>
            Yes
          </Text>
        </Box>
        <Box
          borderStyle={selected === 'no' ? 'bold' : 'single'}
          borderColor={selected === 'no' ? colors.primary[500] : colors.gray[300]}
          paddingX={spacing[4]}
          paddingY={spacing[1]}
        >
          <Text color={selected === 'no' ? colors.primary[500] : colors.gray[600]}>
            No
          </Text>
        </Box>
      </Box>
    </Box>
  )
}
```
