---
name: cli-ui-design-system
description: >-
  Define design tokens for CLI UI following Claude Code aesthetic.
  Use when creating color palettes, typography systems, or spacing scales for Ink-based CLI tools.
  Provides semantic colors, neutral grays, and layout primitives.
---

# CLI UI Design System

Define consistent design tokens for CLI applications using React Ink.

## Quick Start

### 1. Import Design Tokens

```typescript
import { colors, typography, spacing } from './design-tokens'
```

### 2. Use in Ink Components

```tsx
import { Box, Text } from 'ink'

<Box borderStyle="round" borderColor={colors.primary[500]} padding={spacing[4]}>
  <Text color={colors.success[500]} bold>
    Success!
  </Text>
</Box>
```

## Color Palette

### Semantic Colors

| Token | Hex | Usage |
|-------|-----|-------|
| `colors.primary[500]` | `#8B5CF6` | Brand, buttons, accents |
| `colors.success[500]` | `#22C55E` | Success states |
| `colors.warning[500]` | `#F59E0B` | Warnings |
| `colors.error[500]` | `#EF4444` | Errors |
| `colors.info[500]` | `#3B82F6` | Information |

### Neutral Scale

| Token | Hex | Usage |
|-------|-----|-------|
| `colors.gray[50]` | `#F9FAFB` | Light backgrounds |
| `colors.gray[100]` | `#F3F4F6` | Subtle backgrounds |
| `colors.gray[200]` | `#E5E7EB` | Borders |
| `colors.gray[300]` | `#D1D5DB` | Disabled states |
| `colors.gray[400]` | `#9CA3AF` | Placeholder text |
| `colors.gray[500]` | `#6B7280` | Secondary text |
| `colors.gray[600]` | `#4B5563` | Body text |
| `colors.gray[700]` | `#374151` | Strong text |
| `colors.gray[800]` | `#1F2937` | Headings |
| `colors.gray[900]` | `#111827` | Primary text |

### ANSI Mapping

```typescript
const ansiColors = {
  primary: 'ansi:magentaBright',
  success: 'ansi:green',
  warning: 'ansi:yellow',
  error: 'ansi:red',
  info: 'ansi:blue',
}
```

## Typography

### Font Sizes

| Token | Value | Usage |
|-------|-------|-------|
| `typography.size.xs` | 10 | Captions, hints |
| `typography.size.sm` | 12 | Secondary text |
| `typography.size.base` | 14 | Body text |
| `typography.size.lg` | 16 | Emphasis |
| `typography.size.xl` | 18 | Subheadings |
| `typography.size.2xl` | 20 | Headings |

### Font Weights

| Token | Value | Usage |
|-------|-------|-------|
| `typography.weight.normal` | normal | Body text |
| `typography.weight.bold` | bold | Headings, emphasis |
| `typography.weight.dim` | dim | Secondary, disabled |

## Spacing

### Scale

| Token | Value | Usage |
|-------|-------|-------|
| `spacing[0]` | 0 | None |
| `spacing[1]` | 1 | Tight |
| `spacing[2]` | 2 | Compact |
| `spacing[4]` | 4 | Default |
| `spacing[8]` | 8 | Relaxed |
| `spacing[16]` | 16 | Spacious |

### Layout Patterns

```tsx
// Card pattern
<Box borderStyle="round" padding={spacing[4]} gap={spacing[2]}>
  <Text bold>Title</Text>
  <Text dim>Description</Text>
</Box>

// List pattern
<Box flexDirection="column" gap={spacing[2]}>
  {items.map(item => (
    <Box key={item.id} paddingX={spacing[2]}>
      <Text>{item.name}</Text>
    </Box>
  ))}
</Box>
```

## Design Tokens Reference

See `references/color-palette.md` for complete color specifications.
See `references/typography.md` for typography guidelines.
See `references/spacing.md` for layout patterns.
