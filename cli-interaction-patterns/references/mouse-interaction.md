# Mouse Interaction

Mouse interaction patterns for CLI applications using Ink.

## Enabling Mouse Support

Mouse interaction requires the `AlternateScreen` component:

```tsx
import { AlternateScreen } from 'ink'

function App() {
  return (
    <AlternateScreen>
      <MainContent />
    </AlternateScreen>
  )
}
```

## Click Handling

### Basic Click

```tsx
function ClickableButton({ onClick, children }) {
  return (
    <Box
      borderStyle="round"
      padding={spacing[2]}
      onClick={onClick}
    >
      {children}
    </Box>
  )
}
```

### Click with Position

```tsx
function Canvas({ onDraw }) {
  return (
    <Box
      width={80}
      height={24}
      borderStyle="single"
      onClick={(event) => {
        const { x, y } = event
        onDraw(x, y)
      }}
    >
      <Text>Click to draw</Text>
    </Box>
  )
}
```

### Preventing Event Bubbling

```tsx
function NestedClickable() {
  return (
    <Box
      onClick={(event) => {
        // Handle parent click
      }}
    >
      <Box
        onClick={(event) => {
          event.stopPropagation() // Prevent parent from receiving click
          // Handle child click
        }}
      >
        <Text>Child</Text>
      </Box>
    </Box>
  )
}
```

## Hover Effects

### Basic Hover

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

### Tooltip on Hover

```tsx
function TooltipButton({ label, tooltip, onClick }) {
  const [isHovered, setIsHovered] = useState(false)

  return (
    <Box flexDirection="column">
      <Box
        borderStyle={isHovered ? 'bold' : 'single'}
        onClick={onClick}
        onMouseEnter={() => setIsHovered(true)}
        onMouseLeave={() => setIsHovered(false)}
      >
        <Text>{label}</Text>
      </Box>
      {isHovered && (
        <Box
          position="absolute"
          top={2}
          left={0}
          backgroundColor={colors.gray[800]}
          padding={spacing[2]}
        >
          <Text color={colors.gray[100]}>{tooltip}</Text>
        </Box>
      )}
    </Box>
  )
}
```

## Text Selection

### Selectable Text

Text selection is automatically enabled in `AlternateScreen` mode. Users can:
- Click and drag to select text
- Double-click to select a word
- Triple-click to select a line

### Disabling Selection for UI Elements

```tsx
function UIElement() {
  return (
    <Box noSelect>
      <Text>This text cannot be selected</Text>
    </Box>
  )
}
```

### Selectable Content Area

```tsx
function DocumentViewer({ content }) {
  return (
    <Box
      flexGrow={1}
      overflow="scroll"
      padding={spacing[4]}
    >
      <Text>{content}</Text>
    </Box>
  )
}
```

## Scroll Handling

### Scrollable Container

```tsx
import { ScrollBox } from './components/ScrollBox'

function ScrollableList({ items }) {
  const [scrollTop, setScrollTop] = useState(0)

  return (
    <ScrollBox
      height={20}
      scrollTop={scrollTop}
      onScroll={setScrollTop}
    >
      {items.map(item => (
        <Box key={item.id} paddingY={spacing[1]}>
          <Text>{item.name}</Text>
        </Box>
      ))}
    </ScrollBox>
  )
}
```

### Mouse Wheel Support

```tsx
function WheelScrollable({ children }) {
  const [offset, setOffset] = useState(0)

  useInput((input, key) => {
    // Handle mouse wheel events
    if (input === '\u001b[64~') { // Wheel up
      setOffset(o => Math.max(0, o - 3))
    }
    if (input === '\u001b[65~') { // Wheel down
      setOffset(o => o + 3)
    }
  })

  return (
    <Box overflow="hidden" height={20}>
      <Box marginTop={-offset}>
        {children}
      </Box>
    </Box>
  )
}
```

## Combined Interactions

### Clickable List with Hover

```tsx
function InteractiveList({ items, onSelect }) {
  const [hoveredIndex, setHoveredIndex] = useState<number | null>(null)

  return (
    <Box flexDirection="column">
      {items.map((item, index) => (
        <Box
          key={item.id}
          paddingX={spacing[2]}
          paddingY={spacing[1]}
          backgroundColor={hoveredIndex === index ? colors.gray[100] : undefined}
          onClick={() => onSelect(item)}
          onMouseEnter={() => setHoveredIndex(index)}
          onMouseLeave={() => setHoveredIndex(null)}
        >
          <Text
            color={hoveredIndex === index ? colors.primary[500] : colors.gray[600]}
            bold={hoveredIndex === index}
          >
            {item.label}
          </Text>
        </Box>
      ))}
    </Box>
  )
}
```

### Drag and Drop

```tsx
function DraggableList({ items, onReorder }) {
  const [draggedIndex, setDraggedIndex] = useState<number | null>(null)
  const [dropTarget, setDropTarget] = useState<number | null>(null)

  return (
    <Box flexDirection="column">
      {items.map((item, index) => (
        <Box
          key={item.id}
          padding={spacing[2]}
          borderStyle={dropTarget === index ? 'bold' : 'single'}
          borderColor={dropTarget === index ? colors.primary[500] : colors.gray[200]}
          onMouseDown={() => setDraggedIndex(index)}
          onMouseEnter={() => {
            if (draggedIndex !== null && draggedIndex !== index) {
              setDropTarget(index)
            }
          }}
          onMouseUp={() => {
            if (draggedIndex !== null && dropTarget !== null) {
              onReorder(draggedIndex, dropTarget)
            }
            setDraggedIndex(null)
            setDropTarget(null)
          }}
        >
          <Text dim={draggedIndex === index}>{item.label}</Text>
        </Box>
      ))}
    </Box>
  )
}
```

## Best Practices

### 1. Always Provide Keyboard Fallback

```tsx
function AccessibleButton({ onClick, label }) {
  const [isHovered, setIsHovered] = useState(false)
  const [isFocused, setIsFocused] = useState(false)

  useInput((input, key) => {
    if (isFocused && key.return) {
      onClick()
    }
  })

  return (
    <Box
      borderStyle={(isHovered || isFocused) ? 'bold' : 'single'}
      onClick={onClick}
      onMouseEnter={() => setIsHovered(true)}
      onMouseLeave={() => setIsHovered(false)}
      onFocus={() => setIsFocused(true)}
      onBlur={() => setIsFocused(false)}
      tabIndex={0}
    >
      <Text>{label}</Text>
    </Box>
  )
}
```

### 2. Visual Feedback

```tsx
function InteractiveCard({ children, onClick }) {
  const [isPressed, setIsPressed] = useState(false)

  return (
    <Box
      borderStyle="round"
      borderColor={isPressed ? colors.primary[600] : colors.gray[200]}
      backgroundColor={isPressed ? colors.gray[100] : undefined}
      padding={spacing[4]}
      onClick={onClick}
      onMouseDown={() => setIsPressed(true)}
      onMouseUp={() => setIsPressed(false)}
    >
      {children}
    </Box>
  )
}
```

### 3. Clear Hit Areas

```tsx
// Good: Large, clear hit area
<Box
  padding={spacing[4]}
  onClick={handleClick}
>
  <Text>Click me</Text>
</Box>

// Bad: Small hit area
<Text onClick={handleClick}>Click me</Text>
```
