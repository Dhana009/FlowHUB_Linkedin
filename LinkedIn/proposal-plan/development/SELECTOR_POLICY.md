# Selector Policy

This document defines the selector precedence and rationale for FlowHub automation tests. **All developers and QA engineers must follow this policy** to ensure test stability across UI changes.

## Core Principle

**Accessibility semantics are stable. DOM structure is not.**

When UIs change visually, accessibility semantics (roles, names, states) remain constant if implemented correctly. This enables tests to survive UI redesigns without breaking.

## Selector Precedence (Priority Order)

### 1. Accessible Name Sources (Semantic)

Use these in order of preference:

#### 1.1 `aria-labelledby` (Highest Priority)

References another element that provides the label. Automatically updates when the referenced element changes.

```html
<label id="email-label">Email Address</label>
<input type="email" aria-labelledby="email-label" />
```

**Playwright:**
```python
page.get_by_label("Email Address").click()
# or
page.get_by_role("textbox", name="Email Address").click()
```

**Why**: Most maintainable. If the label text changes, the accessible name updates automatically.

#### 1.2 `aria-label` (Direct Label)

Direct label text. Must be manually updated if the UI text changes.

```html
<button aria-label="Create new task">+</button>
```

**Playwright:**
```python
page.get_by_role("button", name="Create new task").click()
```

**Why**: Good for icon-only buttons or when label text differs from visible text. Requires manual updates if UI changes.

#### 1.3 Visually Hidden Text (`sr-only`)

Text that's visible to screen readers but hidden visually. Used when semantic HTML isn't sufficient.

```html
<button>
  <span class="sr-only">Close dialog</span>
  <span aria-hidden="true">×</span>
</button>
```

**Playwright:**
```python
page.get_by_role("button", name="Close dialog").click()
```

**Why**: Useful when you can't modify the element directly but need accessible text. The `sr-only` class hides it visually but keeps it accessible.

### 2. `data-testid` (Fallback)

Use when semantic approaches aren't reliable or would be too brittle.

```html
<div data-testid="task-card-123">
  <!-- Complex nested structure -->
</div>
```

**Playwright:**
```python
page.get_by_test_id("task-card-123").click()
```

**When to use:**
- Complex components where semantic roles aren't clear
- Dynamic content where accessible names might vary
- Legacy code that can't be easily refactored

**Note**: `data-testid` should be used sparingly. Prefer semantic approaches first.

### 3. CSS/XPath (Last Resort Only)

**Only use when:**
- There's a known framework quirk that prevents semantic access
- It's a temporary workaround for a bug
- You've exhausted all semantic options

**Requirements:**
- Must create a ticket to replace with semantic/testid approach
- Must document why semantic approach doesn't work
- Must be reviewed and approved

```python
# Temporary workaround - TODO: Replace with semantic selector
# Ticket: #123
page.locator("button.btn-primary:has-text('Create')").click()
```

**Why last resort**: CSS/XPath break when:
- Class names change
- HTML structure changes
- Framework updates
- Styling refactors

## Examples

### Good: Semantic First

```python
# ✅ Preferred: Uses accessible name
page.get_by_role("button", name="Create Task").click()

# ✅ Good: Uses label relationship
page.get_by_label("Email").fill("user@example.com")

# ✅ Good: Uses role and accessible name
page.get_by_role("textbox", name="Task Description").fill("My task")
```

### Acceptable: Test ID Fallback

```python
# ✅ Acceptable: When semantic isn't reliable
page.get_by_test_id("task-card-123").click()

# ✅ Acceptable: Complex component
page.get_by_test_id("kanban-column-in-progress").click()
```

### Avoid: CSS/XPath

```python
# ❌ Avoid: Brittle CSS selector
page.locator("button.btn-primary").click()

# ❌ Avoid: XPath based on structure
page.locator("//div[@class='task']//button[1]").click()

# ❌ Avoid: Text-based CSS (unless semantic)
page.locator("button:has-text('Create')").click()
```

## Developer Guidelines

When developing UI components, ensure they have proper accessibility:

### 1. Use Semantic HTML

```html
<!-- ✅ Good -->
<button>Submit</button>
<input type="email" />
<label for="email">Email</label>

<!-- ❌ Avoid -->
<div onclick="submit()">Submit</div>
<div role="textbox">Email input</div>
```

### 2. Provide Accessible Names

```html
<!-- ✅ Good: Label association -->
<label id="email-label">Email</label>
<input type="email" aria-labelledby="email-label" />

<!-- ✅ Good: Direct label -->
<button aria-label="Close dialog">×</button>

<!-- ✅ Good: Visually hidden text -->
<button>
  <span class="sr-only">Delete task</span>
  <span aria-hidden="true">🗑️</span>
</button>
```

### 3. Use `aria-labelledby` When Possible

This is the most maintainable approach:

```html
<label id="task-name-label">Task Name</label>
<input 
  type="text" 
  aria-labelledby="task-name-label"
  aria-required="true"
/>
```

If the label text changes, the accessible name updates automatically.

### 4. Add `data-testid` Only When Needed

```html
<!-- ✅ When semantic isn't sufficient -->
<div data-testid="complex-widget-container">
  <!-- Complex nested structure -->
</div>
```

## Testing Guidelines

### For QA Engineers

1. **Always try semantic first**: Use `get_by_role()`, `get_by_label()`, `get_by_text()` with accessible names
2. **Fall back to testid**: Only when semantic isn't reliable
3. **Document exceptions**: If you must use CSS/XPath, create a ticket and document why
4. **Review selectors**: Before committing, ensure you've tried semantic approaches

### Playwright Best Practices

```python
# ✅ Preferred patterns
page.get_by_role("button", name="Submit")
page.get_by_label("Email")
page.get_by_text("Welcome back")
page.get_by_placeholder("Enter task name")
page.get_by_test_id("task-card")  # Fallback

# ❌ Avoid
page.locator("button.btn-primary")
page.locator("//button[@class='submit']")
page.locator(".task-card button")
```

## Version-Specific Considerations

### v1 (Bootstrap) & v2 (Tailwind)

- Standard semantic HTML
- Standard accessibility patterns
- Tests should work identically

### v3 (Automation Challenge)

- May include shadow DOMs, iframes, complex animations
- **Still maintains accessibility semantics**
- Tests may need additional wait strategies or iframe handling
- Semantic selectors should still work, but may require more careful implementation

## Maintenance

### Regular Reviews

- Review test selectors quarterly
- Replace CSS/XPath with semantic approaches
- Update `data-testid` usage to semantic where possible
- Document any exceptions

### When UI Changes

1. **Check semantic selectors first**: They should still work
2. **Update testids if needed**: Only if semantic isn't available
3. **Avoid CSS/XPath updates**: If you're updating CSS/XPath, consider if semantic would work instead

## Summary

**Priority:**
1. `aria-labelledby` → `aria-label` → `sr-only` (semantic)
2. `data-testid` (fallback)
3. CSS/XPath (last resort, with tickets)

**Goal**: 95%+ of selectors should be semantic. Test IDs should be <5%. CSS/XPath should be <1%.

---

**Remember**: If your selector breaks when the UI changes visually, you're using the wrong approach. Semantic selectors survive UI redesigns.

