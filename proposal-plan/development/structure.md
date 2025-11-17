# Project Structure

## What is Project Structure?

How we organize code files and folders. Good structure makes code easy to find, understand, and maintain.

## Monorepo Layout

We use a **monorepo** - all code (frontend, backend, tests) in one repository:

```
LinkedIn/
├── apps/
│   ├── frontend/     # Next.js app (three UI variants: v1, v2, v3)
│   └── backend/      # Fastify API server
├── tests/
│   ├── api/          # API tests (Python + pytest)
│   ├── e2e/          # E2E tests (Python + Playwright)
│   ├── unit/         # Unit tests (Jest/Vitest)
│   └── integration/  # Integration tests
├── infra/            # Infrastructure documentation
├── docs/             # User documentation
└── proposal-plan/    # This folder
```

## Why Monorepo?

1. **Code Sharing**: Share TypeScript types between frontend and backend (no duplication)
2. **Single Source of Truth**: All code in one place, easier to navigate
3. **Atomic Changes**: Change frontend and backend together in one commit
4. **Simpler CI/CD**: One pipeline for everything
5. **Easier Testing**: Run all tests together

## Backend Structure

How backend code is organized:

```
backend/
├── src/
│   ├── controllers/   # Handle HTTP requests (what to do when API is called)
│   ├── services/      # Business logic (how things work)
│   ├── models/        # Database models (what data looks like)
│   ├── routes/        # API route definitions (which URLs do what)
│   ├── middleware/    # Auth, validation, error handling
│   └── utils/         # Helper functions
├── tests/             # Backend tests
└── package.json
```

**Why This Structure?**
- **Separation of Concerns**: Controllers handle HTTP, services handle logic, models handle data
- **Testability**: Easy to test services in isolation (mock controllers/models)
- **Maintainability**: Clear organization makes code easy to find and modify

## Frontend Structure

How frontend code is organized:

```
frontend/
├── src/
│   ├── components/    # Reusable UI components (Button, Input, Card)
│   ├── pages/         # Page components (Login, Dashboard, Tasks)
│   ├── hooks/         # Custom React hooks (useAuth, useTasks)
│   ├── services/      # API calls (centralized)
│   ├── utils/         # Helper functions
│   └── types/         # TypeScript types (shared with backend)
├── public/            # Static assets (images, fonts)
└── package.json
```

**Why This Structure?**
- **Component Reusability**: Shared components reduce duplication
- **Hooks for Logic**: Extract reusable logic into custom hooks
- **Service Layer**: Centralize API calls, easier to mock for tests
- **Type Safety**: Shared types ensure frontend/backend consistency

