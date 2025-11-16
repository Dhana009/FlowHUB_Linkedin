# Test Types

## What Are Test Types?

Different tests check different things. We use multiple types to catch bugs at different levels - from individual functions to full user journeys.

## Unit Tests

**What They Test**: Single functions or components in isolation (no dependencies)

**Example**: Test a function that calculates task priority
```typescript
// Function
function calculatePriority(task) {
  if (task.urgent) return 'high';
  return 'normal';
}

// Test
test('returns high for urgent tasks', () => {
  expect(calculatePriority({urgent: true})).toBe('high');
});
```

**Tool**: Jest (frontend) or Vitest (backend)

**Why We Need Them**: 
- Very fast (run in milliseconds)
- Catch logic errors early
- Easy to write and maintain
- Can test many scenarios quickly

## Integration Tests

**What They Test**: How multiple parts work together (components, services, APIs)

**Example**: Test an API endpoint with mocked database
```typescript
// Test API endpoint with mocked database
test('POST /api/tasks creates task', async () => {
  const mockDB = { save: jest.fn() };
  const response = await createTask({title: 'Test'}, mockDB);
  expect(mockDB.save).toHaveBeenCalled();
});
```

**Tool**: Jest with mocks

**Why We Need Them**:
- Catch issues unit tests miss (how parts interact)
- Test component interactions
- Verify API + database integration
- Still relatively fast

## API Tests

**What They Test**: HTTP endpoints from outside (black box testing - we don't care about internal code)

**Example**: Test login endpoint
```python
# Test login API (Python)
def test_login_api():
    response = requests.post('/api/v1/auth/login', {
        'email': 'user@test.com',
        'password': 'password123'
    })
    assert response.status_code == 200
    assert 'token' in response.json()
```

**Tool**: Python + pytest + requests

**Why We Need Them**:
- Test real API contracts (what frontend actually uses)
- Verify request/response handling
- Test authentication, validation, errors
- Language-agnostic (Python is QA-friendly)

## E2E Tests (End-to-End)

**What They Test**: Full user journeys in real browser (like a real user)

**Example**: User logs in → creates task → views task
```python
# E2E test (Playwright)
def test_create_task():
    page.goto('/login')
    page.get_by_role('textbox', name='Email').fill('user@test.com')
    page.get_by_role('button', name='Login').click()
    page.get_by_role('button', name='Create Task').click()
    # ... etc
```

**Tool**: Playwright (Python)

**Why We Need Them**:
- Test real user experience
- Catch UI bugs, timing issues
- Validate across all UI variants (v1, v2, v3)
- Prove semantic invariance works (same test, different UIs)

## Testing Pyramid

We write many unit tests (fast, cheap), some integration/API tests (medium), and fewer E2E tests (slow, expensive). This gives us good coverage efficiently.

