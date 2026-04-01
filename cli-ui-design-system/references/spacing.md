# Spacing

Spacing system for CLI UI layout.

## Spacing Scale

Base unit: 1 character width

```typescript
const spacing = {
  0: 0,
  1: 1,   // 1 char - Tight spacing
  2: 2,   // 2 chars - Compact spacing
  4: 4,   // 4 chars - Default spacing
  8: 8,   // 8 chars - Relaxed spacing
  16: 16, // 16 chars - Spacious spacing
}
```

## Usage

### Padding

```tsx
// All sides
<Box padding={spacing[4]}>Content</Box>

// Horizontal only
<Box paddingX={spacing[4]}>Content</Box>

// Vertical only
<Box paddingY={spacing[2]}>Content</Box>

// Specific sides
<Box
  paddingTop={spacing[4]}
  paddingBottom={spacing[2]}
  paddingLeft={spacing[8]}
  paddingRight={spacing[8]}
>
  Content
</Box>
```

### Margin

```tsx
// All sides
<Box margin={spacing[4]}>Content</Box>

// Horizontal only
<Box marginX={spacing[4]}>Content</Box>

// Vertical only
<Box marginY={spacing[2]}>Content</Box>
```

### Gap

```tsx
// Both directions
<Box gap={spacing[2]}>
  <Item1 />
  <Item2 />
</Box>

// Column gap only
<Box columnGap={spacing[4]}>
  <Item1 />
  <Item2 />
</Box>

// Row gap only
<Box rowGap={spacing[2]}>
  <Item1 />
  <Item2 />
</Box>
```

## Layout Patterns

### Card

```tsx
<Box
  borderStyle="round"
  borderColor={colors.gray[200]}
  padding={spacing[4]}
  gap={spacing[2]}
>
  <Text bold color={colors.gray[900]}>Card Title</Text>
  <Text color={colors.gray[600]}>Card description</Text>
</Box>
```

### List

```tsx
<Box flexDirection="column" gap={spacing[1]}>
  {items.map(item => (
    <Box
      key={item.id}
      paddingX={spacing[2]}
      paddingY={spacing[1]}
    >
      <Text>{item.name}</Text>
    </Box>
  ))}
</Box>
```

### Form

```tsx
<Box flexDirection="column" gap={spacing[4]}>
  <Box flexDirection="column" gap={spacing[1]}>
    <Text dim>Name</Text>
    <Input />
  </Box>
  <Box flexDirection="column" gap={spacing[1]}>
    <Text dim>Email</Text>
    <Input />
  </Box>
</Box>
```

### Header + Content

```tsx
<Box flexDirection="column" gap={spacing[4]}>
  <Box paddingBottom={spacing[2]} borderStyle="single" borderBottom>
    <Text bold>Header</Text>
  </Box>
  <Box paddingX={spacing[2]}>
    <Text>Content</Text>
  </Box>
</Box>
```

## Common Combinations

### Compact Layout

```tsx
<Box padding={spacing[2]} gap={spacing[1]}>
  {/* Tight spacing for dense UIs */}
</Box>
```

### Comfortable Layout

```tsx
<Box padding={spacing[4]} gap={spacing[2]}>
  {/* Default spacing for most UIs */}
</Box>
```

### Spacious Layout

```tsx
<Box padding={spacing[8]} gap={spacing[4]}>
  {/* Generous spacing for emphasis */}
</Box>
```
