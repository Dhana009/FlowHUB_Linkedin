# Phase 1: Foundation (MVP)

## What is This Phase?

This is the **Minimum Viable Product (MVP)** - the absolute minimum features needed to have a working task management app. We build these first to get something functional quickly.

## Flows to Build

- **Flow 1: User Login & Signup**
  - Users can create accounts with email/password
  - Users can log in to access the app
  - JWT tokens for authentication

- **Flow 2: Registration & Email Verification**
  - New users receive verification email
  - Users must verify email before account is active
  - Uses AWS SES to send emails

- **Flow 4: Task Flow (CRUD)**
  - Create new tasks
  - View list of tasks
  - Edit existing tasks
  - Delete tasks
  - Basic fields: title, description, status, assignee

## Why These Three Flows First?

1. **Unlocks Basic Usage**: Without authentication, nothing else works. Users must be able to sign up, verify email, and log in before they can do anything else.

2. **Minimal Surface Area**: These three flows are the smallest set that gives us a working app. We can demonstrate the core concept with just auth + tasks.

3. **Foundation for Everything**: All other flows depend on these:
   - Comments need tasks to comment on
   - Files attach to tasks
   - Roles control who can access tasks
   - Everything requires authenticated users

4. **Fastest to Demo**: This is the quickest path to a working prototype that can be showcased to recruiters or the community.

## Success Criteria

Before moving to Phase 2, we must have:
- ✅ Authentication working (users can signup, verify email, login)
- ✅ Tasks CRUD working (create, read, update, delete tasks)
- ✅ All tests passing (unit, integration, API, E2E)
- ✅ Works across v1 (Bootstrap) and v2 (Tailwind) UI variants
- ✅ Performance metrics met (LCP < 2.5s, INP < 200ms, CLS < 0.1)
- ✅ Accessibility compliant (zero violations, Lighthouse ≥ 90)

