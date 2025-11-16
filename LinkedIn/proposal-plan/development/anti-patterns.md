# Anti-Patterns (What NOT to Do)

## What are Anti-Patterns?

Common mistakes that lead to bad code. Learn these so you can avoid them.

---

## Anti-Pattern 1: Duplicate Code in Pages

### ❌ Bad Example

```typescript
// pages/tasks.tsx
export default function TasksPage() {
  return (
    <div>
      {tasks.map(task => (
        <div key={task.id} className="card p-4 mb-4">
          <h3 className="text-xl font-bold">{task.title}</h3>
          <p className="text-gray-600">{task.description}</p>
          <div className="mt-4">
            <button 
              className="bg-blue-500 text-white px-4 py-2 rounded"
              onClick={() => handleEdit(task)}
            >
              Edit
            </button>
            <button 
              className="bg-red-500 text-white px-4 py-2 rounded ml-2"
              onClick={() => handleDelete(task.id)}
            >
              Delete
            </button>
          </div>
        </div>
      ))}
    </div>
  );
}

// pages/projects.tsx - SAME CODE DUPLICATED!
export default function ProjectsPage() {
  return (
    <div>
      {projects.map(project => (
        <div key={project.id} className="card p-4 mb-4">
          <h3 className="text-xl font-bold">{project.name}</h3>
          <p className="text-gray-600">{project.description}</p>
          <div className="mt-4">
            <button 
              className="bg-blue-500 text-white px-4 py-2 rounded"
              onClick={() => handleEdit(project)}
            >
              Edit
            </button>
            <button 
              className="bg-red-500 text-white px-4 py-2 rounded ml-2"
              onClick={() => handleDelete(project.id)}
            >
              Delete
            </button>
          </div>
        </div>
      ))}
    </div>
  );
}
```

**Problems**:
- Same UI code in two places
- If you change button style, must change two files
- Hard to maintain

### ✅ Good Example

```typescript
// components/Card.tsx - REUSABLE COMPONENT
interface CardProps {
  title: string;
  description: string;
  onEdit: () => void;
  onDelete: () => void;
}

export function Card({ title, description, onEdit, onDelete }: CardProps) {
  return (
    <div className="card p-4 mb-4">
      <h3 className="text-xl font-bold">{title}</h3>
      <p className="text-gray-600">{description}</p>
      <div className="mt-4">
        <button 
          className="bg-blue-500 text-white px-4 py-2 rounded"
          onClick={onEdit}
        >
          Edit
        </button>
        <button 
          className="bg-red-500 text-white px-4 py-2 rounded ml-2"
          onClick={onDelete}
        >
          Delete
        </button>
      </div>
    </div>
  );
}

// pages/tasks.tsx - USE COMPONENT
import { Card } from '@/components/Card';

export default function TasksPage() {
  return (
    <div>
      {tasks.map(task => (
        <Card
          key={task.id}
          title={task.title}
          description={task.description}
          onEdit={() => handleEdit(task)}
          onDelete={() => handleDelete(task.id)}
        />
      ))}
    </div>
  );
}
```

**Benefits**:
- Code written once
- Change once, updates everywhere
- Easy to maintain

---

## Anti-Pattern 2: Business Logic in Components

### ❌ Bad Example

```typescript
// components/TaskForm.tsx
export function TaskForm() {
  const handleSubmit = async (e: FormEvent) => {
    e.preventDefault();
    const formData = new FormData(e.target);
    
    // ❌ Business logic in component
    const title = formData.get('title') as string;
    if (title.length < 3) {
      alert('Title must be at least 3 characters');
      return;
    }
    
    // ❌ API call in component
    const response = await fetch('/api/v1/tasks', {
      method: 'POST',
      body: JSON.stringify({ title }),
    });
    
    if (!response.ok) {
      alert('Failed to create task');
      return;
    }
    
    const task = await response.json();
    // ❌ More business logic
    if (task.status === 'done') {
      console.log('Task completed!');
    }
  };
  
  return <form onSubmit={handleSubmit}>...</form>;
}
```

**Problems**:
- Component does too much (validation, API calls, business logic)
- Hard to test
- Can't reuse logic
- Violates separation of concerns

### ✅ Good Example

```typescript
// services/task.service.ts - BUSINESS LOGIC HERE
export const taskService = {
  async createTask(data: CreateTaskData): Promise<Task> {
    // ✅ Validation in service
    if (data.title.length < 3) {
      throw new Error('Title must be at least 3 characters');
    }
    
    // ✅ API call in service
    const response = await fetch('/api/v1/tasks', {
      method: 'POST',
      body: JSON.stringify(data),
    });
    
    if (!response.ok) {
      throw new Error('Failed to create task');
    }
    
    const task = await response.json();
    
    // ✅ Business logic in service
    if (task.status === 'done') {
      // Handle completion logic
    }
    
    return task;
  },
};

// hooks/useCreateTask.ts - REUSABLE HOOK
export function useCreateTask() {
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);
  
  const createTask = async (data: CreateTaskData) => {
    setLoading(true);
    setError(null);
    try {
      const task = await taskService.createTask(data);
      return task;
    } catch (err) {
      setError(err.message);
      throw err;
    } finally {
      setLoading(false);
    }
  };
  
  return { createTask, loading, error };
}

// components/TaskForm.tsx - ONLY UI
import { useCreateTask } from '@/hooks/useCreateTask';

export function TaskForm() {
  const { createTask, loading, error } = useCreateTask();
  
  const handleSubmit = async (e: FormEvent) => {
    e.preventDefault();
    const formData = new FormData(e.target);
    const title = formData.get('title') as string;
    
    // ✅ Just call hook, no logic here
    await createTask({ title });
  };
  
  return (
    <form onSubmit={handleSubmit}>
      {error && <div className="error">{error}</div>}
      {/* form fields */}
    </form>
  );
}
```

**Benefits**:
- Component is simple (just UI)
- Logic is reusable (hook can be used elsewhere)
- Easy to test (test service and hook separately)

---

## Anti-Pattern 3: API Calls in Components

### ❌ Bad Example

```typescript
// components/TaskList.tsx
export function TaskList() {
  const [tasks, setTasks] = useState([]);
  
  useEffect(() => {
    // ❌ API call directly in component
    fetch('/api/v1/tasks')
      .then(res => res.json())
      .then(setTasks);
  }, []);
  
  return <div>{/* render tasks */}</div>;
}

// components/TaskCard.tsx
export function TaskCard({ task }) {
  const handleDelete = async () => {
    // ❌ API call in component
    await fetch(`/api/v1/tasks/${task.id}`, { method: 'DELETE' });
  };
  
  return <div>{/* card UI */}</div>;
}
```

**Problems**:
- API calls scattered everywhere
- Hard to mock for tests
- Hard to change API URLs
- Duplicate fetch logic

### ✅ Good Example

```typescript
// services/task.service.ts - CENTRALIZED API CALLS
export const taskService = {
  async getTasks(): Promise<Task[]> {
    const response = await fetch('/api/v1/tasks');
    return response.json();
  },
  
  async deleteTask(id: string): Promise<void> {
    await fetch(`/api/v1/tasks/${id}`, { method: 'DELETE' });
  },
};

// hooks/useTasks.ts - REUSABLE HOOK
export function useTasks() {
  const [tasks, setTasks] = useState<Task[]>([]);
  
  useEffect(() => {
    taskService.getTasks().then(setTasks);
  }, []);
  
  return { tasks };
}

// components/TaskList.tsx - USE HOOK
import { useTasks } from '@/hooks/useTasks';

export function TaskList() {
  const { tasks } = useTasks();
  return <div>{/* render tasks */}</div>;
}

// components/TaskCard.tsx - USE SERVICE
import { taskService } from '@/services/task.service';

export function TaskCard({ task, onDelete }) {
  const handleDelete = async () => {
    await taskService.deleteTask(task.id);
    onDelete();
  };
  
  return <div>{/* card UI */}</div>;
}
```

**Benefits**:
- API calls in one place
- Easy to mock for tests
- Easy to change API URLs
- Reusable across components

---

## Anti-Pattern 4: Business Logic in Controllers

### ❌ Bad Example

```typescript
// controllers/task.controller.ts
export async function createTask(req: Request, res: Response) {
  const { title, description } = req.body;
  
  // ❌ Validation in controller
  if (!title || title.length < 3) {
    return res.status(400).json({ error: 'Title too short' });
  }
  
  // ❌ Business logic in controller
  const status = description.includes('urgent') ? 'high' : 'normal';
  
  // ❌ Database query in controller
  const task = await TaskModel.create({
    title,
    description,
    status,
    userId: req.user.id,
    createdAt: new Date(),
  });
  
  res.json(task);
}
```

**Problems**:
- Controller does too much
- Logic can't be reused
- Hard to test
- Violates separation of concerns

### ✅ Good Example

```typescript
// services/task.service.ts - BUSINESS LOGIC HERE
export class TaskService {
  async createTask(data: CreateTaskData): Promise<Task> {
    // ✅ Validation in service
    if (!data.title || data.title.length < 3) {
      throw new Error('Title must be at least 3 characters');
    }
    
    // ✅ Business logic in service
    const status = data.description.includes('urgent') ? 'high' : 'normal';
    
    // ✅ Call model
    const task = await TaskModel.create({
      ...data,
      status,
      createdAt: new Date(),
    });
    
    return task;
  }
}

// controllers/task.controller.ts - THIN CONTROLLER
export async function createTask(req: Request, res: Response) {
  try {
    // ✅ Just call service, no logic here
    const task = await taskService.createTask({
      ...req.body,
      userId: req.user.id,
    });
    
    res.status(201).json(task);
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
}
```

**Benefits**:
- Controller is thin (just HTTP)
- Service logic is reusable
- Easy to test separately

---

## Anti-Pattern 5: Duplicate Validation Logic

### ❌ Bad Example

```typescript
// controllers/task.controller.ts
export async function createTask(req, res) {
  // ❌ Validation duplicated
  if (!req.body.title || req.body.title.length < 3) {
    return res.status(400).json({ error: 'Title too short' });
  }
  // ...
}

// controllers/project.controller.ts
export async function createProject(req, res) {
  // ❌ Same validation duplicated
  if (!req.body.name || req.body.name.length < 3) {
    return res.status(400).json({ error: 'Name too short' });
  }
  // ...
}
```

### ✅ Good Example

```typescript
// services/validation.service.ts - REUSABLE VALIDATION
export const validationService = {
  validateMinLength(value: string, min: number, fieldName: string): void {
    if (!value || value.length < min) {
      throw new Error(`${fieldName} must be at least ${min} characters`);
    }
  },
};

// services/task.service.ts
import { validationService } from './validation.service';

export class TaskService {
  async createTask(data: CreateTaskData) {
    // ✅ Use reusable validation
    validationService.validateMinLength(data.title, 3, 'Title');
    // ...
  }
}
```

---

## Quick Checklist

Before writing code, check:

- [ ] Am I duplicating code that exists elsewhere?
- [ ] Is business logic in the right layer? (Service, not Controller/Component)
- [ ] Are API calls centralized? (Service, not Component)
- [ ] Is UI code reusable? (Component, not duplicated in pages)
- [ ] Can this logic be reused? (Extract to hook/service)

**If you answer "yes" to any, refactor before continuing.**

---

**Learn these patterns. They prevent most code quality issues.**

