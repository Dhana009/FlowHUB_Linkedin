# Phase 2: Core Features

## What is This Phase?

After we have basic auth and tasks working, we add features that make the app useful for real teams: collaboration (comments), file attachments, and user management (roles).

## Flows to Build

- **Flow 6: Comment & Activity Feed**
  - Users can add comments to tasks
  - Activity feed shows what's happening (who did what, when)
  - Notifications when someone comments on your task

- **Flow 7: File Upload & Preview**
  - Users can upload files to tasks (documents, images, PDFs)
  - Files stored in AWS S3
  - Users can preview files without downloading

- **Flow 3: Role & Permission**
  - Two roles: Admin and User
  - Admins can manage users, access admin panel
  - Users have limited permissions
  - Permission checks on all actions

## Why This Order?

1. **Collaboration First**: Comments enable team discussion on tasks. This is the most natural next step - teams need to communicate about work.

2. **Files Make Tasks Useful**: Real-world tasks need attachments. A task without files is just a todo list. Files transform it into a practical tool.

3. **Roles Enable Admin Features**: Once we have files and comments, we need role-based access control. Admins can manage users, moderate content, and access admin panels.

4. **Natural Progression**: Users create tasks → discuss them → attach files → need permissions to control access. This follows real-world workflow.

5. **Infrastructure Ready**: S3 is already set up for file storage, so file upload is straightforward to implement.

## Success Criteria

Before moving to Phase 3, we must have:
- ✅ Comments working (add, view, notifications)
- ✅ File upload/preview working (S3 integration)
- ✅ Role-based access control working (admin vs user)
- ✅ All tests passing
- ✅ Works across v1 and v2
- ✅ Performance and accessibility thresholds met

