# Code Standards

## What are Code Standards?

Rules we follow when writing code. Consistent standards make code easier to read, understand, and maintain.

## TypeScript Standards

- **Strict Mode Enabled**: All strict TypeScript checks turned on
- **No `any` Type**: Use proper types, never use `any`
- **Why**: Catch errors at compile time (before code runs), better IDE support

**Example**:
```typescript
// ❌ Bad
function createTask(data: any) { ... }

// ✅ Good
function createTask(data: CreateTaskRequest) { ... }
```

## Naming Conventions

Consistent naming makes code self-documenting:

- **Components**: PascalCase
  - `TaskCard`, `LoginForm`, `UserProfile`
  - Why: React convention, easy to identify components

- **Functions**: camelCase
  - `createTask`, `validateEmail`, `getUserById`
  - Why: JavaScript convention, easy to read

- **Constants**: UPPER_SNAKE_CASE
  - `MAX_FILE_SIZE`, `API_BASE_URL`, `DEFAULT_TIMEOUT`
  - Why: Clearly identifies constants, easy to spot

## File Organization

- **One Component Per File**: Each component in its own file
  - Why: Easy to find, easier to test, clearer organization

- **Tests Co-located**: Tests next to the code they test
  - Why: Easy to find tests, clear what's being tested

**Example**:
```
components/
  TaskCard.tsx
  TaskCard.test.tsx    # Test next to component
  TaskCard.types.ts    # Types next to component
```

## Non-Negotiables

Every feature must have these before it can be merged:

1. **Tests**: Unit, integration, API, or E2E (as appropriate)
   - Why: Ensure code works and doesn't break

2. **Accessibility Pass**: Zero violations, Lighthouse score ≥ 90
   - Why: Legal requirement, inclusive design

3. **Performance Pass**: LCP < 2.5s, INP < 200ms, CLS < 0.1
   - Why: User experience, SEO

4. **Documentation Update**: Update README, Quickstart, or API docs
   - Why: Help others understand and use the feature

**Why Non-Negotiable?**
- Maintains quality from day one
- Every feature meets the same bar
- Shows production-ready practices
- Well-tested, documented code is easier to maintain

