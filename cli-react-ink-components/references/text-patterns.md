# Text Patterns

Text component styling patterns for Ink CLI applications.

## Text Styles

### Basic Styling

```tsx
// Bold text for emphasis
<Text bold>This is bold</Text>

// Dim text for secondary content
<Text dim>This is dimmed</Text>

// Italic for emphasis
<Text italic>This is italic</Text>

// Underline for links
<Text underline>This is underlined</Text>

// Strikethrough for deleted content
<Text strikethrough>This is struck through</Text>

// Inverse for highlighting
<Text inverse>This is inverted</Text>
```

### Combining Styles

```tsx
// Bold + color
<Text bold color={colors.primary[500]}>
  Bold primary text
</Text>

// Dim + italic
<Text dim italic>
  Secondary emphasis
</Text>

// Bold + underline (link style)
<Text bold underline color={colors.info[500]}>
  Clickable link
</Text>
```

## Colors

### Foreground Colors

```tsx
// Semantic colors
<Text color={colors.primary[500]}>Primary</Text>
<Text color={colors.success[500]}>Success</Text>
<Text color={colors.warning[500]}>Warning</Text>
<Text color={colors.error[500]}>Error</Text>
<Text color={colors.info[500]}>Info</Text>

// Neutral colors
<Text color={colors.gray[900]}>Primary text</Text>
<Text color={colors.gray[600]}>Body text</Text>
<Text color={colors.gray[500]}>Secondary text</Text>
<Text color={colors.gray[400]}>Placeholder</Text>
```

### Background Colors

```tsx
// Highlighted text
<Text backgroundColor={colors.warning[100]} color={colors.warning[800]}>
  Warning highlight
</Text>

// Code inline
<Text backgroundColor={colors.gray[100]} color={colors.gray[800]}>
  `code`
</Text>

// Selected text
<Text backgroundColor={colors.primary[500]} color={colors.gray[50]}>
  Selected
</Text>
```

## Text Wrapping

### Wrap Modes

```tsx
// Wrap (default) - breaks at word boundaries
<Text wrap="wrap">
  This long text will wrap to multiple lines at appropriate word boundaries
</Text>

// Wrap with trim - removes leading/trailing whitespace
<Text wrap="wrap-trim">
  Text with trimmed whitespace
</Text>

// End truncation - shows ellipsis at end
<Text wrap="truncate-end">
  Very long text that will be truncated with ellipsis at the end...
</Text>

// Start truncation - shows ellipsis at start
<Text wrap="truncate-start">
  ...truncated text that shows the end
</Text>

// Middle truncation - shows ellipsis in middle
<Text wrap="truncate-middle">
  Beginning...end
</Text>
```

### No Wrapping

```tsx
// No wrap - text stays on single line
<Text wrap="end">
  This text will not wrap
</Text>
```

## Typography Patterns

### Heading Hierarchy

```tsx
// H1 - Page title
<Text bold color={colors.gray[900]}>
  Page Title
</Text>

// H2 - Section heading
<Text bold color={colors.gray[800]}>
  Section Heading
</Text>

// H3 - Subsection
<Text color={colors.gray[700]}>
  Subsection
</Text>

// Caption
<Text dim color={colors.gray[500]}>
  Caption text
</Text>
```

### Label + Value

```tsx
<Box flexDirection="row" gap={spacing[2]}>
  <Text dim>Name:</Text>
  <Text bold>John Doe</Text>
</Box>
```

### Status Indicators

```tsx
// Success status
<Box flexDirection="row" gap={spacing[1]}>
  <Text color={colors.success[500]}>✓</Text>
  <Text color={colors.success[700]}>Success</Text>
</Box>

// Error status
<Box flexDirection="row" gap={spacing[1]}>
  <Text color={colors.error[500]}>✗</Text>
  <Text color={colors.error[700]}>Error</Text>
</Box>

// Warning status
<Box flexDirection="row" gap={spacing[1]}>
  <Text color={colors.warning[500]}>⚠</Text>
  <Text color={colors.warning[700]}>Warning</Text>
</Box>
```

### Code Blocks

```tsx
// Inline code
<Text>
  Run <Text backgroundColor={colors.gray[100]} color={colors.gray[800]}>npm install</Text> to start
</Text>

// Code block
<Box
  backgroundColor={colors.gray[900]}
  padding={spacing[4]}
  borderStyle="round"
>
  <Text color={colors.gray[100]}>
    {`function greet() {
  return 'Hello World'
}`}
  </Text>
</Box>
```

### Lists

```tsx
// Bullet list
<Box flexDirection="column" gap={spacing[1]}>
  <Box flexDirection="row" gap={spacing[2]}>
    <Text color={colors.primary[500]}>•</Text>
    <Text>First item</Text>
  </Box>
  <Box flexDirection="row" gap={spacing[2]}>
    <Text color={colors.primary[500]}>•</Text>
    <Text>Second item</Text>
  </Box>
</Box>

// Numbered list
<Box flexDirection="column" gap={spacing[1]}>
  <Box flexDirection="row" gap={spacing[2]}>
    <Text dim>1.</Text>
    <Text>First step</Text>
  </Box>
  <Box flexDirection="row" gap={spacing[2]}>
    <Text dim>2.</Text>
    <Text>Second step</Text>
  </Box>
</Box>
```

## Special Characters

```tsx
// Arrows
<Text>→ Right arrow</Text>
<Text>← Left arrow</Text>
<Text>↑ Up arrow</Text>
<Text>↓ Down arrow</Text>

// Checkmarks
<Text>✓ Check</Text>
<Text>✗ Cross</Text>

// Bullet points
<Text>• Bullet</Text>
<Text>◦ Circle</Text>
<Text>▪ Square</Text>

// Progress indicators
<Text>⠋ Spinner frame 1</Text>
<Text>⠙ Spinner frame 2</Text>
<Text>⠹ Spinner frame 3</Text>
```
