# State Management

State management patterns for CLI application UI states.

## Loading States

### Basic Loading

```tsx
function AsyncComponent() {
  const [isLoading, setIsLoading] = useState(false)

  async function handleAction() {
    setIsLoading(true)
    try {
      await performAsyncAction()
    } finally {
      setIsLoading(false)
    }
  }

  if (isLoading) {
    return <Spinner text="Loading..." />
  }

  return <Button onClick={handleAction}>Start</Button>
}
```

### Loading with Progress

```tsx
function UploadComponent() {
  const [status, setStatus] = useState<'idle' | 'uploading' | 'completed'>('idle')
  const [progress, setProgress] = useState(0)

  async function handleUpload() {
    setStatus('uploading')
    
    for (let i = 0; i <= 100; i += 10) {
      await delay(200)
      setProgress(i)
    }
    
    setStatus('completed')
  }

  if (status === 'uploading') {
    return (
      <Box flexDirection="column" gap={spacing[2]}>
        <Text dim>Uploading file...</Text>
        <ProgressBar progress={progress} total={100} />
      </Box>
    )
  }

  if (status === 'completed') {
    return (
      <Box>
        <Text color={colors.success[500]}>✓ Upload complete!</Text>
      </Box>
    )
  }

  return <Button onClick={handleUpload}>Upload</Button>
}
```

### Skeleton Loading

```tsx
function SkeletonCard() {
  return (
    <Box borderStyle="round" padding={spacing[4]} gap={spacing[2]}>
      <Box backgroundColor={colors.gray[200]} width={20}>
        <Text> </Text>
      </Box>
      <Box backgroundColor={colors.gray[200]} width={40}>
        <Text> </Text>
      </Box>
      <Box backgroundColor={colors.gray[200]} width={30}>
        <Text> </Text>
      </Box>
    </Box>
  )
}

function DataList({ isLoading, items }) {
  if (isLoading) {
    return (
      <Box flexDirection="column" gap={spacing[2]}>
        <SkeletonCard />
        <SkeletonCard />
        <SkeletonCard />
      </Box>
    )
  }

  return (
    <Box flexDirection="column" gap={spacing[2]}>
      {items.map(item => (
        <DataCard key={item.id} {...item} />
      ))}
    </Box>
  )
}
```

## Error Handling

### Error Boundary

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
          ✗ Something went wrong
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

### Inline Error

```tsx
function FormField({ label, error, children }) {
  return (
    <Box flexDirection="column" gap={spacing[1]}>
      <Text dim>{label}</Text>
      {children}
      {error && (
        <Text color={colors.error[500]}>{error}</Text>
      )}
    </Box>
  )
}
```

### Error Toast

```tsx
function Toast({ message, type, onClose }) {
  const colors = {
    error: colors.error[500],
    warning: colors.warning[500],
    success: colors.success[500],
    info: colors.info[500],
  }

  useEffect(() => {
    const timer = setTimeout(onClose, 5000)
    return () => clearTimeout(timer)
  }, [onClose])

  return (
    <Box
      position="absolute"
      bottom={2}
      right={2}
      borderStyle="round"
      borderColor={colors[type]}
      backgroundColor={colors.gray[900]}
      padding={spacing[2]}
    >
      <Text color={colors[type]}>{message}</Text>
    </Box>
  )
}
```

## Success States

### Success Message

```tsx
function SuccessMessage({ message, onDismiss }) {
  return (
    <Box
      borderStyle="round"
      borderColor={colors.success[500]}
      backgroundColor={colors.success[50]}
      padding={spacing[4]}
      gap={spacing[2]}
    >
      <Box flexDirection="row" gap={spacing[2]}>
        <Text color={colors.success[500]}>✓</Text>
        <Text bold color={colors.success[700]}>
          Success!
        </Text>
      </Box>
      <Text color={colors.success[600]}>{message}</Text>
      <Box marginTop={spacing[2]}>
        <Button onClick={onDismiss}>Continue</Button>
      </Box>
    </Box>
  )
}
```

### Success Animation

```tsx
function SuccessCheckmark() {
  const [frame, setFrame] = useState(0)
  const frames = ['○', '◔', '◑', '◕', '✓']

  useEffect(() => {
    const timer = setInterval(() => {
      setFrame(f => Math.min(f + 1, frames.length - 1))
    }, 100)
    return () => clearInterval(timer)
  }, [])

  return (
    <Text color={colors.success[500]} bold>
      {frames[frame]}
    </Text>
  )
}
```

## Empty States

### Empty List

```tsx
function EmptyState({ icon, title, description, action }) {
  return (
    <Box
      flexDirection="column"
      alignItems="center"
      gap={spacing[2]}
      padding={spacing[8]}
    >
      <Text color={colors.gray[400]}>{icon}</Text>
      <Text bold color={colors.gray[600]}>
        {title}
      </Text>
      <Text color={colors.gray[500]}>{description}</Text>
      {action && (
        <Box marginTop={spacing[2]}>
          {action}
        </Box>
      )}
    </Box>
  )
}

// Usage
<EmptyState
  icon="📁"
  title="No files found"
  description="Upload your first file to get started"
  action={<Button onClick={handleUpload}>Upload File</Button>}
/>
```

## Confirmation States

### Confirm Dialog

```tsx
function ConfirmDialog({
  title,
  message,
  confirmLabel = 'Confirm',
  cancelLabel = 'Cancel',
  onConfirm,
  onCancel,
  isDangerous = false,
}) {
  const [selected, setSelected] = useState<'confirm' | 'cancel'>('cancel')

  useInput((input, key) => {
    if (key.leftArrow || key.rightArrow) {
      setSelected(s => s === 'confirm' ? 'cancel' : 'confirm')
    }
    if (key.return) {
      selected === 'confirm' ? onConfirm() : onCancel()
    }
    if (key.escape) {
      onCancel()
    }
  })

  return (
    <Box
      borderStyle="round"
      borderColor={isDangerous ? colors.error[500] : colors.gray[300]}
      padding={spacing[4]}
      gap={spacing[2]}
    >
      <Text bold>{title}</Text>
      <Text color={colors.gray[600]}>{message}</Text>
      <Box gap={spacing[4]} marginTop={spacing[2]}>
        <Button
          variant={isDangerous ? 'danger' : 'primary'}
          isFocused={selected === 'confirm'}
        >
          {confirmLabel}
        </Button>
        <Button isFocused={selected === 'cancel'}>
          {cancelLabel}
        </Button>
      </Box>
    </Box>
  )
}
```

## Form States

### Form Validation

```tsx
function Form() {
  const [values, setValues] = useState({ name: '', email: '' })
  const [errors, setErrors] = useState({ name: '', email: '' })
  const [touched, setTouched] = useState({ name: false, email: false })

  function validate(values) {
    const errors = {}
    if (!values.name) errors.name = 'Name is required'
    if (!values.email) errors.email = 'Email is required'
    if (values.email && !values.email.includes('@')) {
      errors.email = 'Invalid email'
    }
    return errors
n  }

  function handleSubmit() {
    const validationErrors = validate(values)
    setErrors(validationErrors)
    setTouched({ name: true, email: true })

    if (Object.keys(validationErrors).length === 0) {
      submitForm(values)
    }
  }

  return (
    <Box flexDirection="column" gap={spacing[4]}>
      <FormField
        label="Name"
        error={touched.name && errors.name}
      >
        <Input
          value={values.name}
          onChange={value => {
            setValues(v => ({ ...v, name: value }))
            setTouched(t => ({ ...t, name: true }))
          }}
        />
      </FormField>
      <Button onClick={handleSubmit}>Submit</Button>
    </Box>
  )
}
```

## State Machine Pattern

```tsx
type State =
  | { type: 'idle' }
  | { type: 'loading' }
  | { type: 'success'; data: unknown }
  | { type: 'error'; error: Error }

function StateMachineComponent() {
  const [state, setState] = useState<State>({ type: 'idle' })

  async function handleAction() {
    setState({ type: 'loading' })
    try {
      const data = await fetchData()
      setState({ type: 'success', data })
    } catch (error) {
      setState({ type: 'error', error })
    }
  }

  switch (state.type) {
    case 'idle':
      return <Button onClick={handleAction}>Load Data</Button>
    case 'loading':
      return <Spinner text="Loading..." />
    case 'success':
      return <DataView data={state.data} />
    case 'error':
      return (
        <ErrorView
          error={state.error}
          onRetry={handleAction}
        />
      )
  }
}
```
