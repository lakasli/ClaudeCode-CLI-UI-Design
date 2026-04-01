# Typography

Typography system for CLI UI design.

## Font Sizes

CLI applications use a limited set of font sizes for clarity:

```typescript
const fontSize = {
  xs: 10,    // Captions, hints, timestamps
  sm: 12,    // Secondary text, labels
  base: 14,  // Body text (default)
  lg: 16,    // Emphasis, lead text
  xl: 18,    // Subheadings
  '2xl': 20, // Headings
}
```

## Font Weights

Ink supports three weight variants:

```typescript
const fontWeight = {
  normal: undefined,  // Regular text
  bold: true,         // Headings, emphasis
  dim: true,          // Secondary, disabled (with dim color)
}
```

## Text Styles

### Headings

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
```

### Body Text

```tsx
// Primary body
<Text color={colors.gray[600]}>
  Main content text
</Text>

// Secondary body
<Text dim color={colors.gray[500]}>
  Secondary information
</Text>
```

### Code & Monospace

```tsx
// Inline code
<Text backgroundColor={colors.gray[100]} color={colors.gray[800]}>
  {'npm install'}
</Text>

// Code block
<Box
  backgroundColor={colors.gray[900]}
  padding={spacing[4]}
  borderStyle="round"
>
  <Text color={colors.gray[100]}>
    {`function example() {
  return 'Hello World'
}`}
  </Text>
</Box>
```

## Text Modifiers

### Emphasis

```tsx
// Bold emphasis
<Text bold>Important text</Text>

// Italic emphasis
<Text italic>Stressed text</Text>

// Underline for links
<Text underline color={colors.info[500]}>
  Click here
</Text>
```

### States

```tsx
// Disabled text
<Text dim color={colors.gray[400]}>
  Disabled option
</Text>

// Error text
<Text color={colors.error[500]}>
  Error message
</Text>

// Success text
<Text color={colors.success[500]}>
  Success message
</Text>
```

## Text Wrapping

```tsx
// Wrap text (default)
<Text wrap="wrap">
  Long text that wraps to multiple lines
</Text>

// Truncate at end
<Text wrap="truncate-end">
  Very long text that gets truncated...
</Text>

// Truncate in middle
<Text wrap="truncate-middle">
  Very long text...truncated
</Text>
```

## Typography Patterns

### Label + Value

```tsx
<Box flexDirection="row" gap={spacing[2]}>
  <Text dim>Name:</Text>
  <Text bold>John Doe</Text>
</Box>
```

### List Item

```tsx
<Box flexDirection="row" gap={spacing[2]}>
  <Text color={colors.primary[500]}>•</Text>
  <Text>List item text</Text>
</Box>
```

### Status Badge

```tsx
<Box
  backgroundColor={colors.success[100]}
  paddingX={spacing[2]}
  borderStyle="round"
>
  <Text color={colors.success[700]}>Active</Text>
</Box>
```
