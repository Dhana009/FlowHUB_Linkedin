# Architecture Rules

## What is Architecture?

Architecture is how we organize code into layers. Each layer has a specific job. This prevents duplicate code, makes testing easier, and keeps code maintainable.

## Why We Have Layers

**Without layers**: Everything mixed together = duplicate code, hard to test, hard to maintain

**With layers**: Clear separation = reusable code, easy to test, easy to maintain

---

## Backend Architecture

### The Three Layers

```
HTTP Request
    ↓
Controller (handles HTTP)
    ↓
Service (business logic)
    ↓
Model (database)
```

### 1. Controllers

**What Goes Here**: HTTP request/response handling only

**What NOT to Put Here**:
- ❌ Business logic (calculations, validations)
- ❌ Database queries
- ❌ Complex logic

**Example - Correct**:
```typescript
// controllers/task.controller.ts
export async function createTask(req: Request, res: Response) {
  const { title, description } = req.body;
  const userId = req.user.id;
  
  // Call service - no logic here
  const task = await taskService.createTask({ title, description, userId });
  
  res.status(201).json(task);
}
```

**Example - Wrong**:
```typescript
// ❌ BAD: Business logic in controller
export async function createTask(req: Request, res: Response) {
  const { title, description } = req.body;
  
  // ❌ Validation logic here (should be in service)
  if (!title || title.length < 3) {
    return res.status(400).json({ error: 'Title too short' });
  }
  
  // ❌ Database query here (should be in service)
  const task = await TaskModel.create({ title, description });
  
  res.json(task);
}
```

**Why**: Controllers should be thin. They just receive requests and call services.

### 2. Services

**What Goes Here**: All business logic

**What Goes Here**:
- ✅ Validation logic
- ✅ Business rules (calculations, transformations)
- ✅ Calling models/database
- ✅ Reusable logic

**Example - Correct**:
```typescript
// services/task.service.ts
export class TaskService {
  async createTask(data: CreateTaskData) {
    // ✅ Validation logic
    if (!data.title || data.title.length < 3) {
      throw new Error('Title must be at least 3 characters');
    }
    
    // ✅ Business logic
    const taskData = {
      ...data,
      status: 'todo', // Default status
      createdAt: new Date(),
    };
    
    // ✅ Call model (database)
    const task = await TaskModel.create(taskData);
    
    return task;
  }
}
```

**Example - Wrong**:
```typescript
// ❌ BAD: Service doing HTTP stuff
export class TaskService {
  async createTask(req: Request, res: Response) {
    // ❌ HTTP handling in service
    const task = await TaskModel.create(req.body);
    res.json(task); // ❌ Response in service
  }
}
```

**Why**: Services are reusable. They can be used by controllers, background jobs, or other services.

### 3. Models

**What Goes Here**: Data structure and database operations only

**What NOT to Put Here**:
- ❌ Business logic
- ❌ Validation (beyond data types)
- ❌ HTTP handling

**Example - Correct**:
```typescript
// models/task.model.ts
export interface Task {
  _id: string;
  title: string;
  description: string;
  status: 'todo' | 'doing' | 'done';
  userId: string;
  createdAt: Date;
}

export const TaskModel = {
  async create(data: Omit<Task, '_id' | 'createdAt'>) {
    return await db.tasks.insertOne({
      ...data,
      createdAt: new Date(),
    });
  },
  
  async findById(id: string) {
    return await db.tasks.findOne({ _id: id });
  },
};
```

**Example - Wrong**:
```typescript
// ❌ BAD: Business logic in model
export const TaskModel = {
  async create(data: any) {
    // ❌ Validation in model (should be in service)
    if (data.title.length < 3) {
      throw new Error('Title too short');
    }
    
    // ❌ Business logic in model
    if (data.status === 'done') {
      data.completedAt = new Date();
    }
    
    return await db.tasks.insertOne(data);
  },
};
```

**Why**: Models should only care about data structure and database operations. Business logic belongs in services.

---

## Frontend Architecture

### The Four Layers

```
Page (routing/layout)
    ↓
Component (UI)
    ↓
Hook (logic)
    ↓
Service (API calls)
```

### 1. Pages

**What Goes Here**: Routing and layout only

**What NOT to Put Here**:
- ❌ Business logic
- ❌ API calls
- ❌ Complex state management
- ❌ Duplicate UI code

**Example - Correct**:
```typescript
// pages/tasks.tsx
import { TaskList } from '@/components/TaskList';
import { useTasks } from '@/hooks/useTasks';

export default function TasksPage() {
  // ✅ Use hook for logic
  const { tasks, loading, error } = useTasks();
  
  // ✅ Only layout/routing
  return (
    <div>
      <h1>Tasks</h1>
      <TaskList tasks={tasks} loading={loading} error={error} />
    </div>
  );
}
```

**Example - Wrong**:
```typescript
// ❌ BAD: API calls and logic in page
export default function TasksPage() {
  const [tasks, setTasks] = useState([]);
  const [loading, setLoading] = useState(false);
  
  useEffect(() => {
    // ❌ API call in page (should be in service)
    fetch('/api/v1/tasks')
      .then(res => res.json())
      .then(data => setTasks(data));
  }, []);
  
  // ❌ Duplicate UI code (should be component)
  return (
    <div>
      <h1>Tasks</h1>
      {tasks.map(task => (
        <div key={task.id}>
          <h3>{task.title}</h3>
          <p>{task.description}</p>
        </div>
      ))}
    </div>
  );
}
```

**Why**: Pages should be thin. They just route and compose components.

### 2. Components

**What Goes Here**: Reusable UI only

**What NOT to Put Here**:
- ❌ API calls
- ❌ Business logic
- ❌ Duplicate code

**Example - Correct**:
```typescript
// components/TaskCard.tsx
interface TaskCardProps {
  task: Task;
  onEdit: (task: Task) => void;
  onDelete: (id: string) => void;
}

export function TaskCard({ task, onEdit, onDelete }: TaskCardProps) {
  // ✅ Only UI, no API calls, no business logic
  return (
    <div>
      <h3>{task.title}</h3>
      <p>{task.description}</p>
      <button onClick={() => onEdit(task)}>Edit</button>
      <button onClick={() => onDelete(task.id)}>Delete</button>
    </div>
  );
}
```

**Example - Wrong**:
```typescript
// ❌ BAD: API calls in component
export function TaskCard({ task }: { task: Task }) {
  const handleDelete = async () => {
    // ❌ API call in component (should be in service/hook)
    await fetch(`/api/v1/tasks/${task.id}`, { method: 'DELETE' });
  };
  
  return (
    <div>
      <h3>{task.title}</h3>
      <button onClick={handleDelete}>Delete</button>
    </div>
  );
}
```

**Why**: Components should be pure UI. They receive data and callbacks, no side effects.

### 3. Hooks

**What Goes Here**: Reusable logic (data fetching, state management)

**Example - Correct**:
```typescript
// hooks/useTasks.ts
export function useTasks() {
  const [tasks, setTasks] = useState<Task[]>([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);
  
  useEffect(() => {
    // ✅ API call in hook (via service)
    setLoading(true);
    taskService.getTasks()
      .then(setTasks)
      .catch(setError)
      .finally(() => setLoading(false));
  }, []);
  
  return { tasks, loading, error };
}
```

**Why**: Hooks extract reusable logic. Multiple components can use the same hook.

### 4. Services

**What Goes Here**: All API calls centralized

**Example - Correct**:
```typescript
// services/task.service.ts
export const taskService = {
  async getTasks(): Promise<Task[]> {
    const response = await fetch('/api/v1/tasks');
    return response.json();
  },
  
  async createTask(data: CreateTaskData): Promise<Task> {
    const response = await fetch('/api/v1/tasks', {
      method: 'POST',
      body: JSON.stringify(data),
    });
    return response.json();
  },
};
```

**Why**: Centralized API calls. Easy to mock for tests, easy to change API URLs.

---

## Rules Summary

### Backend Rules

| Layer | Job | Don't Put Here |
|-------|-----|----------------|
| Controller | HTTP only | Business logic, database queries |
| Service | Business logic | HTTP handling, database queries directly |
| Model | Data structure | Business logic, validation |

### Frontend Rules

| Layer | Job | Don't Put Here |
|-------|-----|----------------|
| Page | Routing/layout | API calls, business logic, duplicate UI |
| Component | UI only | API calls, business logic |
| Hook | Reusable logic | UI code, duplicate logic |
| Service | API calls | UI code, business logic |

---

## How to Check

Before writing code, ask:
1. **Is this the right layer?** (Controller vs Service vs Model)
2. **Is this reusable?** (Should it be a component/hook/service?)
3. **Is there duplicate code?** (Extract to reusable piece)
4. **Does this layer's job?** (Controller handles HTTP, Service handles logic)

**If unsure, check the examples above or ask before writing code.**

