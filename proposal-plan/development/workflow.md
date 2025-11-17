# Development Workflow

## What is Development Workflow?

The step-by-step process we follow when building a new feature. This ensures consistency and quality.

## Feature Development Steps

### 1. Create Feature Branch
- Branch name: `feature/task-crud` or `feature/auth`
- Why: Isolate work, don't break main branch

### 2. Backend First
- Build API endpoints
- Write business logic
- Add unit and integration tests
- Why: API defines the contract that frontend will use

### 3. Frontend
- Build UI components
- Integrate with API
- Add unit and integration tests
- Why: Frontend consumes the backend API

### 4. E2E Tests
- Write full user journey tests
- Test across v1 and v2 UI variants
- Why: Validate the complete flow works

### 5. Create Pull Request (PR)
- Write clear description
- Show test results
- Why: Get code reviewed before merging

### 6. CI Must Pass
- All checks must be green (tests, performance, accessibility)
- Why: Automated quality gates prevent bad code

### 7. Code Review
- Someone reviews the code
- Approve or request changes
- Why: Catch issues, share knowledge

### 8. Merge to Main
- Deploy automatically
- Why: Get features to production

## Why Backend First?

1. **API Defines Contract**: Backend API determines what frontend can do
2. **Frontend Consumes It**: Frontend is built to use the API
3. **Tests Alongside Code**: Write tests as you code, not after

## Branching Strategy

- **`main`**: Production-ready code (always deployable)
- **`feature/*`**: New features being developed
- **`fix/*`**: Bug fixes

**Why This Strategy?**
- Simple and easy to understand
- Main branch always stable
- Feature branches allow parallel work

