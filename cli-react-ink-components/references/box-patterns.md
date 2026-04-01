# Box Patterns

Box component layout patterns for Ink CLI applications.

## Border Styles

Ink supports multiple border styles for different visual effects:

```tsx
// Single line - clean and minimal
<Box borderStyle="single">
  <Text>Single border</Text>
</Box>

// Double line - strong emphasis
<Box borderStyle="double">
  <Text>Double border</Text>
</Box>

// Round corners - friendly appearance
<Box borderStyle="round">
  <Text>Rounded border</Text>
</Box>

// Bold - high visibility
<Box borderStyle="bold">
  <Text>Bold border</Text>
</Box>

// Single/Double mix
<Box borderStyle="singleDouble">
  <Text>Mixed border</Text>
</Box>

// Classic ASCII
<Box borderStyle="classic">
  <Text>Classic border</Text>
</Box>
```

## Border Sides

Control which sides show borders:

```tsx
// All sides (default)
<Box borderStyle="round" borderTop borderBottom borderLeft borderRight>
  <Text>Full border</Text>
</Box>

// Top only
<Box borderStyle="single" borderTop>
  <Text>Top border only</Text>
</Box>

// Bottom separator
<Box borderStyle="single" borderBottom>
  <Text>Bottom border</Text>
</Box>
```

## Border Colors

```tsx
// Single color for all sides
<Box borderStyle="round" borderColor={colors.primary[500]}>
  <Text>Colored border</Text>
</Box>

// Different colors per side
<Box
  borderStyle="round"
  borderTopColor={colors.primary[500]}
  borderBottomColor={colors.gray[200]}
  borderLeftColor={colors.gray[200]}
  borderRightColor={colors.gray[200]}
>
  <Text>Multi-colored border</Text>
</Box>

// Dimmed border
<Box borderStyle="round" borderDimColor>
  <Text>Dimmed border</Text>
</Box>
```

## Border Text

Add text to borders:

```tsx
<Box
  borderStyle="round"
  borderText={{ title: 'Header', alignment: 'center' }}
>
  <Text>Content with titled border</Text>
</Box>
```

## Layout Patterns

### Centered Content

```tsx
<Box
  justifyContent="center"
  alignItems="center"
  height={10}
>
  <Text>Centered</Text>
</Box>
```

### Space Between

```tsx
<Box justifyContent="space-between">
  <Text>Left</Text>
  <Text>Right</Text>
</Box>
```

### Grid Layout

```tsx
<Box flexDirection="column" gap={spacing[2]}>
  <Box flexDirection="row" gap={spacing[2]}>
    <Box flexGrow={1}><Text>Cell 1</Text></Box>
    <Box flexGrow={1}><Text>Cell 2</Text></Box>
  </Box>
  <Box flexDirection="row" gap={spacing[2]}>
    <Box flexGrow={1}><Text>Cell 3</Text></Box>
    <Box flexGrow={1}><Text>Cell 4</Text></Box>
  </Box>
</Box>
```

### Sidebar Layout

```tsx
<Box>
  <Box width={20} borderStyle="single" borderRight>
    <Text>Sidebar</Text>
  </Box>
  <Box flexGrow={1} padding={spacing[4]}>
    <Text>Main content</Text>
  </Box>
</Box>
```

## Overflow Handling

```tsx
// Hidden overflow
<Box overflow="hidden" height={5}>
  <Text>Content that might overflow...</Text>
</Box>

// Scrollable (requires ScrollBox)
<ScrollBox height={5}>
  <Text>Long content...</Text>
</ScrollBox>
```

## Background Colors

```tsx
// Solid background
<Box backgroundColor={colors.gray[100]} padding={spacing[4]}>
  <Text>With background</Text>
</Box>

// Opaque (fills with terminal default)
<Box opaque padding={spacing[4]}>
  <Text>Opaque background</Text>
</Box>
```
