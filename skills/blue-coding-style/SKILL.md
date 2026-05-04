---
name: blue-coding-style
description: Opinionated React coding style guide covering file naming, component declarations, props & types, import order, styling, component patterns, code quality, and state management. Enforces kebab-case, arrow function components, type over interface, cn() class merging, and more.
---

# React Coding Style Guide

A strict, opinionated set of rules for writing consistent React + TypeScript code.

## File Naming

- All files use **kebab-case**: `avatar.tsx`, `use-auth.ts`
- Pages: kebab-case directory + `index.tsx` (e.g., `user-profile/index.tsx`)

## File Creation

- Single file → create the file directly, no directory
- Only create a directory when multiple related files are needed
- Never pre-create empty directories; create only what's needed

## Component Declaration

- Always use **arrow functions**: `const Avatar = () => { ... }`
- Use `React.forwardRef` only when actually needed
- Declaration order must match usage order: child components before parent components

## Export Conventions

- UI components: **named exports** (`export const Avatar = ...` or grouped at bottom)
- Pages and layouts: **default exports** (`export default Home`)
- Types: always `export type`
- Utility functions: named exports

## Import Order

1. React and its sub-modules
2. Third-party libraries
3. Utility libraries (ahooks, etc.)
4. Internal modules (`@/` alias paths)
5. Type-only imports use `import type`

## Props & Types

- Always use `type`, never `interface`
- Props naming: `IComponentNameProps`, declared at file top
- No inline complex types — extract anything beyond simple primitives into a named `type`
- Destructure props, set defaults inline
- Extract `className` first, spread remaining props last
- Use `cn()` to merge class names, user-provided `className` goes last

```ts
type IAvatarProps = {
  size?: number
  className?: string
}

export const Avatar = ({ size = 40, className, ...rest }: IAvatarProps) => {
  return <div className={cn('avatar', className)} {...rest} />
}
```

## Styling

- Prefer the project's existing styling solution (Tailwind / CSS Modules / styled-components)
- Class name merging: always use `cn()` or the project's existing utility
- Multi-variant components: use CVA (class-variance-authority) or the project's existing pattern

## Component Patterns

- When JSX content is large, split into sub-render variables or functions
- No-argument renders → `const` variable; with-argument renders → function
- **Never** write callback functions inline in JSX props — declare first, then reference

```ts
// Bad
<Button onClick={() => handleSubmit(id)}>

// Good
const handleClick = () => handleSubmit(id)
<Button onClick={handleClick}>
```

## Code Quality

- Conditional rendering: ternary for either/or, `&&` for show/hide, early return for multiple guards
- No magic values — extract hardcoded numbers/strings into named constants
- No commented-out code — delete what's not needed
- No meaningless `console.log`
- Keep functions/components small — max **200 lines** per file

## State Management

- State should live as close to where it's used as possible — don't lift to global prematurely
- Derived state: compute directly, don't store in state
