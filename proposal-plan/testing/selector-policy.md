# Selector Policy

## What is a Selector?

In automation tests, we need to find elements on the page (buttons, inputs, etc.). A selector is how we tell the test "click this button" or "fill this input".

## Priority Order (Must Follow)

### 1. Semantic Selectors (First Choice)

These use accessibility information that screen readers also use:

- **`aria-labelledby`**: References another element that provides the label
  ```html
  <label id="email-label">Email</label>
  <input aria-labelledby="email-label" />
  ```
  ```python
  page.get_by_label("Email").click()
  ```

- **`aria-label`**: Direct label text
  ```html
  <button aria-label="Create Task">+</button>
  ```
  ```python
  page.get_by_role("button", name="Create Task").click()
  ```

- **Visually hidden text (`sr-only`)**: Text visible to screen readers but hidden visually
  ```html
  <button>
    <span class="sr-only">Close dialog</span>
    <span aria-hidden="true">×</span>
  </button>
  ```
  ```python
  page.get_by_role("button", name="Close dialog").click()
  ```

### 2. Fallback: `data-testid`

Use when semantic approaches aren't reliable:
```html
<div data-testid="task-card-123">
```
```python
page.get_by_test_id("task-card-123").click()
```

### 3. Last Resort: CSS/XPath

Only for temporary workarounds, with a ticket to replace:
```python
# Temporary - TODO: Replace with semantic selector
# Ticket: #123
page.locator("button.btn-primary").click()
```

## Why This Order?

1. **Semantic Selectors Survive UI Changes**: If UI redesigns but keeps accessibility, tests still work. This is the core concept of FlowHub.

2. **Forces Accessible UI Design**: Using semantic selectors forces us to build accessible UIs, which is required anyway.

3. **Proves Core Concept**: Semantic invariance across UI variants is what FlowHub demonstrates. If we use CSS/XPath, we're not proving anything.

4. **Maintainability**: Semantic selectors are self-documenting and less brittle than CSS/XPath.

## What Happens If We Use CSS/XPath?

- Tests break when UI changes (class names, HTML structure)
- We're not proving semantic invariance
- We're not building accessible UIs
- Tests become maintenance burden

