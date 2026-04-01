# Claude Code CLI UI Skills

一套符合 Claude Code 审美的 CLI 界面设计 Skills，帮助开发者构建优雅、高效的命令行工具。

## 概述

这套 Skills 基于对 Claude Code 源代码的深入分析，提炼出其 CLI 界面的设计精髓，包括：

- **优雅的色彩系统** - 柔和的紫罗兰主色调，清晰的语义化颜色
- **精致的排版设计** - 层次分明的字体大小和字重
- **流畅的交互体验** - 完整的键盘导航和状态管理
- **灵活的主题支持** - 内置多主题，支持自定义和持久化

## Skills 组成

### 1. cli-ui-design-system

设计系统基础，定义颜色、排版、间距等设计令牌。

```typescript
import { colors, spacing, typography } from './design-tokens'

<Box borderColor={colors.primary[500]} padding={spacing[4]}>
  <Text color={colors.success[500]} bold>
    Success!
  </Text>
</Box>
```

**关键特性：**
- 完整的调色板（Primary、Success、Warning、Error、Info、Gray）
- 语义化颜色命名
- 一致的间距系统（1, 2, 4, 8, 16）
- 清晰的排版层级

### 2. cli-react-ink-components

React Ink 组件模式，提供 Box、Text 和交互组件的最佳实践。

```tsx
// Box 布局
<Box borderStyle="round" flexDirection="column" gap={spacing[2]}>
  <Text bold>Title</Text>
  <Text dim>Description</Text>
</Box>

// 交互组件
<Button label="Submit" onClick={handleSubmit} variant="primary" />
<Input label="Name" value={name} onChange={setName} />
<Spinner text="Loading..." />
```

**关键特性：**
- 边框样式（single、double、round、bold）
- Flex 布局模式
- 文本样式（bold、dim、italic、underline）
- 交互组件（Button、Select、Input、Spinner、ProgressBar）

### 3. cli-interaction-patterns

交互模式，包含键盘导航、鼠标交互和状态管理。

```tsx
// 键盘导航
useInput((input, key) => {
  if (key.tab) focusNext()
  if (key.return) submit()
  if (key.escape) cancel()
})

// 焦点管理
const { focusedId, focusNext, focusPrevious } = useFocusManager(['name', 'email', 'submit'])
```

**关键特性：**
- Tab/Shift+Tab 焦点切换
- 方向键导航
- 快捷键定义（Ctrl+C、Ctrl+S 等）
- 加载/错误/成功状态管理
- 鼠标点击和悬停效果

### 4. cli-theming-engine

主题引擎，支持多主题切换和自定义。

```tsx
// 主题提供者
<ThemeProvider defaultTheme="default">
  <App />
</ThemeProvider>

// 使用主题
const { colors, spacing } = useTheme()

// 切换主题
setTheme('dark')
```

**内置主题：**
- **default** - Claude Code 默认主题（紫罗兰色调）
- **dark** - 深色主题（高对比度）
- **light** - 浅色主题（明亮清新）
- **minimal** - 极简主题（无边框颜色）

**关键特性：**
- 主题继承机制
- 运行时主题切换
- 用户偏好持久化
- 完整的 TypeScript 类型定义

## 快速开始

### 安装依赖

```bash
npm install ink react
npm install -D @types/react typescript
```

### 配置 TypeScript

```json
{
  "compilerOptions": {
    "jsx": "react-jsx",
    "jsxImportSource": "ink",
    "module": "ESNext",
    "moduleResolution": "bundler"
  }
}
```

### 基础示例

```tsx
import React from 'react'
import { render, Box, Text } from 'ink'
import { colors, spacing } from './theme/tokens'

function App() {
  return (
    <Box
      borderStyle="round"
      borderColor={colors.primary[500]}
      padding={spacing[4]}
    >
      <Text color={colors.primary[500]} bold>
        Hello CLI UI!
      </Text>
    </Box>
  )
}

render(<App />)
```

## 示例项目

查看 `examples/cli-ui-demo/` 目录，包含一个完整的演示应用：

```bash
cd examples/cli-ui-demo
npm install
npm run build
npm start
```

演示应用包含四个屏幕：
1. **Welcome** - 欢迎界面，展示功能特性
2. **Form** - 交互式表单，键盘导航
3. **Progress** - 进度显示，动画效果
4. **Result** - 结果展示，成功反馈

## 设计原则

这套 Skills 遵循 Claude Code 的核心设计原则：

### 1. Clarity First（清晰优先）
- 信息层次清晰
- 重要内容突出
- 避免视觉噪音

### 2. Subtle Elegance（微妙优雅）
- 柔和的色彩过渡
- 克制的动画效果
- 精致的细节处理

### 3. Efficiency（高效）
- 减少用户操作步骤
- 智能默认值
- 上下文感知

### 4. Accessibility（可访问性）
- 支持屏幕阅读器
- 高对比度模式
- 键盘完全可操作

### 5. Consistency（一致性）
- 跨组件统一风格
- 遵循平台惯例
- 可预测的交互

## 文件结构

```
skills/
├── cli-ui-design-system/
│   ├── SKILL.md
│   └── references/
│       ├── color-palette.md
│       ├── typography.md
│       └── spacing.md
├── cli-react-ink-components/
│   ├── SKILL.md
│   └── references/
│       ├── box-patterns.md
│       ├── text-patterns.md
│       └── interactive-patterns.md
├── cli-interaction-patterns/
│   ├── SKILL.md
│   └── references/
│       ├── keyboard-navigation.md
│       ├── mouse-interaction.md
│       └── state-management.md
├── cli-theming-engine/
│   ├── SKILL.md
│   └── references/
│       ├── theme-structure.md
│       └── customization-guide.md
└── README.md

examples/
└── cli-ui-demo/
    ├── src/
    │   ├── components/
    │   ├── screens/
    │   ├── theme/
    │   └── index.tsx
    ├── package.json
    ├── tsconfig.json
    └── README.md
```

## 技术栈

- **Framework**: React 18+
- **Renderer**: Ink 4+
- **Language**: TypeScript 5+
- **Runtime**: Bun/Node.js 18+

## 颜色参考

### 主色调
| 颜色 | Hex | 用途 |
|------|-----|------|
| Primary | `#8B5CF6` | 品牌标识、按钮 |
| Success | `#22C55E` | 成功状态 |
| Warning | `#F59E0B` | 警告信息 |
| Error | `#EF4444` | 错误提示 |
| Info | `#3B82F6` | 一般信息 |

### 中性色阶
| Token | Hex | 用途 |
|-------|-----|------|
| gray-50 | `#F9FAFB` | 浅色背景 |
| gray-100 | `#F3F4F6` | 卡片背景 |
| gray-200 | `#E5E7EB` | 边框 |
| gray-500 | `#6B7280` | 次要文字 |
| gray-700 | `#374151` | 正文 |
| gray-900 | `#111827` | 标题 |

## 贡献

欢迎提交 Issue 和 PR 来改进这些 Skills。

## 许可证

MIT

---

*基于 Claude Code 源代码分析，遵循 Claude Code 设计美学。*
# ClaudeCode-CLI-UI-Design
