---
name: cli-interaction-patterns
description: >-
  Implement keyboard navigation, mouse interaction, and state management for CLI UIs.
  Use when handling user input, managing focus, or creating interactive workflows.
  Follows Claude Code patterns for smooth user experience.
---

# CLI Interaction Patterns

Interaction patterns for building responsive CLI applications.

## Quick Start

### Keyboard Input

```tsx
import { useInput } from 'ink'

function App() {
  useInput((input, key) => {
    if (key.return) {
      console.log('Enter pressed')
    }
    if (key.escape) {
      console.log('Escape pressed')
    }
  })

  return <Text>Press keys...</Text>
}
```

### Focus Management

```tsx
import { useFocusManager } from './hooks/useFocusManager'

function Form() {
  const { focusNext, focusPrevious, focusedId } = useFocusManager([
    'name',
    'email',
    'submit'
  ])

  return (
    <Box flexDirection="column" gap={spacing[2]}>
      <Input id="name" isFocused={focusedId === 'name'} />
      <Input id="email" isFocused={focusedId === 'email'} />
      <Button id="submit" isFocused={focusedId === 'submit'} />
    </Box>
  )
}
```

## Keyboard Navigation

### Basic Key Handling

```tsx
useInput((input, key) => {
  // Navigation
  if (key.upArrow) navigateUp()
  if (key.downArrow) navigateDown()
  if (key.leftArrow) navigateLeft()
  if (key.rightArrow) navigateRight()

  // Actions
  if (key.return) submit()
  if (key.escape) cancel()
  if (key.tab) focusNext()

  // Text input
  if (input) appendText(input)
  if (key.backspace) deleteChar()
})
```

### Tab Navigation

```tsx
function TabNavigation() {
  const [activeTab, setActiveTab] = useState(0)
  const tabs = ['General', 'Settings', 'About']

  useInput((input, key) => {
    if (key.tab) {
      setActiveTab(i => (i + 1) % tabs.length)
    }
    if (key.shift && key.tab) {
      setActiveTab(i => (i - 1 + tabs.length) % tabs.length)
    }
  })

  return (
    <Box flexDirection="column">
      <Box gap={spacing[4]}>
        {tabs.map((tab, index) => (
          <Box
            key={tab}
            borderStyle={activeTab === index ? 'bold' : 'single'}
            borderColor={activeTab === index ? colors.primary[500] : colors.gray[300]}
            paddingX={spacing[4]}
          >
            <Text color={activeTab === index ? colors.primary[500] : colors.gray[600]}>
              {tab}
            </Text>
          </Box>
        ))}
      </Box>
      <Box padding={spacing[4]}>
        {activeTab === 0 && <GeneralTab />}
        {activeTab === 1 && <SettingsTab />}
        {activeTab === 2 && <AboutTab />}
      </Box>
    </Box>
  )
}
```

## Mouse Interaction

### Click Handling

```tsx
function ClickableCard({ onClick, children }) {
  return (
    <Box
      borderStyle="round"
      padding={spacing[4]}
      onClick={onClick}
    >
      {children}
    </Box>
  )
}
```

### Hover Effects

```tsx
function HoverButton({ label, onClick }) {
  const [isHovered, setIsHovered] = useState(false)

  return (
    <Box
      borderStyle={isHovered ? 'bold' : 'single'}
      borderColor={isHovered ? colors.primary[500] : colors.gray[300]}
      paddingX={spacing[4]}
      paddingY={spacing[1]}
      onClick={onClick}
      onMouseEnter={() => setIsHovered(true)}
      onMouseLeave={() => setIsHovered(false)}
    >
      <Text color={isHovered ? colors.primary[500] : colors.gray[700]}>
        {label}
      </Text>
    </Box>
  )
}
```

## State Management

### Loading State

```tsx
function AsyncOperation() {
  const [status, setStatus] = useState<'idle' | 'loading' | 'success' | 'error'>('idle')

  async function handleSubmit() {
    setStatus('loading')
    try {
      await performOperation()
      setStatus('success')
    } catch {
      setStatus('error')
    }
  }

  if (status === 'loading') {
    return <Spinner text="Processing..." />
  }

  if (status === 'success') {
    return (
      <Box>
        <Text color={colors.success[500]}>✓ Success!</Text>
      </Box>
    )
  }

  if (status === 'error') {
    return (
      <Box>
        <Text color={colors.error[500]}>✗ Error occurred</Text>
      </Box>
    )
  }

  return <Button onClick={handleSubmit}>Submit</Button>
}
```

### Error Handling

```tsx
function ErrorBoundary({ children }) {
  const [error, setError] = useState<Error | null>(null)

  if (error) {
    return (
      <Box
        borderStyle="round"
        borderColor={colors.error[500]}
        padding={spacing[4]}
        gap={spacing[2]}
      >
        <Text bold color={colors.error[500]}>
          ✗ An error occurred
        </Text>
        <Text color={colors.gray[600]}>{error.message}</Text>
        <Box marginTop={spacing[2]}>
          <Button onClick={() => setError(null)}>Try Again</Button>
        </Box>
      </Box>
    )
  }

  return children
}
```

## Pattern Reference

See `references/keyboard-navigation.md` for keyboard handling patterns.
See `references/mouse-interaction.md` for mouse event patterns.
See `references/state-management.md` for loading, error, and success states.
