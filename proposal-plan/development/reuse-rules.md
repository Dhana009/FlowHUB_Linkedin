# Code Reuse Rules

## What is Code Reuse?

Writing code once and using it multiple times. This prevents duplication, reduces bugs, and makes maintenance easier.

## DRY Principle

**DRY = Don't Repeat Yourself**

If you write the same code twice, extract it to a reusable piece (component, hook, service, function).

---

## When to Create Reusable Code

### Rule of Three

**If code appears 2+ times, extract it**

- **First time**: Write it inline (okay)
- **Second time**: Consider extracting
- **Third time**: Must extract (no exceptions)

### Signs You Need to Extract

1. **Copy-paste code**: You're copying code from one file to another
2. **Same logic in multiple places**: Same calculation/validation in different files
3. **Similar UI patterns**: Same button/form structure repeated
4. **Hard to update**: Changing one place means changing many places

---

## Frontend: When to Extract

### Extract to Component When

- **Same UI appears 2+ times**
- **Similar structure with different data**

**Example - Duplicate Code**:
```typescript
// ❌ BAD: Duplicate code in pages
// pages/tasks.tsx
export default function TasksPage() {
  return (
    <div>
      {tasks.map(task => (
        <div key={task.id} className="card">
          <h3>{task.title}</h3>
          <p>{task.description}</p>
          <button onClick={() => handleEdit(task)}>Edit</button>
        </div>
      ))}
    </div>
  );
}

// pages/projects.tsx
export default function ProjectsPage() {
  return (
    <div>
      {projects.map(project => (
        <div key={project.id} className="card">
          <h3>{project.title}</h3>
          <p>{project.description}</p>
          <button onClick={() => handleEdit(project)}>Edit</button>
        </div>
      ))}
    </div>
  );
}
```

**Example - Extracted Component**:
```typescript
// ✅ GOOD: Reusable component
// components/Card.tsx
interface CardProps {
  title: string;
  description: string;
  onEdit: () => void;
}

export function Card({ title, description, onEdit }: CardProps) {
  return (
    <div className="card">
      <h3>{title}</h3>
      <p>{description}</p>
      <button onClick={onEdit}>Edit</button>
    </div>
  );
}

// pages/tasks.tsx
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
        />
      ))}
    </div>
  );
}
```

### Extract to Hook When

- **Same logic appears 2+ times**
- **Data fetching pattern repeated**
- **State management duplicated**

**Example - Duplicate Logic**:
```typescript
// ❌ BAD: Duplicate logic in components
// components/TaskList.tsx
export function TaskList() {
  const [tasks, setTasks] = useState([]);
  const [loading, setLoading] = useState(false);
  
  useEffect(() => {
    setLoading(true);
    fetch('/api/v1/tasks')
      .then(res => res.json())
      .then(setTasks)
      .finally(() => setLoading(false));
  }, []);
  
  return <div>{/* render tasks */}</div>;
}

// components/ProjectList.tsx
export function ProjectList() {
  const [projects, setProjects] = useState([]);
  const [loading, setLoading] = useState(false);
  
  useEffect(() => {
    setLoading(true);
    fetch('/api/v1/projects')
      .then(res => res.json())
      .then(setProjects)
      .finally(() => setLoading(false));
  }, []);
  
  return <div>{/* render projects */}</div>;
}
```

**Example - Extracted Hook**:
```typescript
// ✅ GOOD: Reusable hook
// hooks/useFetch.ts
export function useFetch<T>(url: string) {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);
  
  useEffect(() => {
    setLoading(true);
    fetch(url)
      .then(res => res.json())
      .then(setData)
      .catch(setError)
      .finally(() => setLoading(false));
  }, [url]);
  
  return { data, loading, error };
}

// components/TaskList.tsx
import { useFetch } from '@/hooks/useFetch';

export function TaskList() {
  const { data: tasks, loading } = useFetch<Task[]>('/api/v1/tasks');
  return <div>{/* render tasks */}</div>;
}
```

### Extract to Service When

- **API calls duplicated**
- **Same endpoint called from multiple places**

**Example - Duplicate API Calls**:
```typescript
// ❌ BAD: API calls duplicated
// components/TaskForm.tsx
const handleSubmit = async (data) => {
  const response = await fetch('/api/v1/tasks', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(data),
  });
  const task = await response.json();
};

// components/TaskEdit.tsx
const handleUpdate = async (id, data) => {
  const response = await fetch(`/api/v1/tasks/${id}`, {
    method: 'PUT',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(data),
  });
  const task = await response.json();
};
```

**Example - Extracted Service**:
```typescript
// ✅ GOOD: Centralized service
// services/task.service.ts
export const taskService = {
  async create(data: CreateTaskData) {
    const response = await fetch('/api/v1/tasks', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data),
    });
    return response.json();
  },
  
  async update(id: string, data: UpdateTaskData) {
    const response = await fetch(`/api/v1/tasks/${id}`, {
      method: 'PUT',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data),
    });
    return response.json();
  },
};

// components/TaskForm.tsx
import { taskService } from '@/services/task.service';

const handleSubmit = async (data) => {
  const task = await taskService.create(data);
};
```

---

## Backend: When to Extract

### Extract to Service When

- **Business logic appears 2+ times**
- **Same validation/calculation in multiple controllers**

**Example - Duplicate Logic**:
```typescript
// ❌ BAD: Duplicate validation in controllers
// controllers/task.controller.ts
export async function createTask(req, res) {
  const { title, description } = req.body;
  
  // ❌ Validation duplicated
  if (!title || title.length < 3) {
    return res.status(400).json({ error: 'Title too short' });
  }
  
  const task = await TaskModel.create({ title, description });
  res.json(task);
}

// controllers/project.controller.ts
export async function createProject(req, res) {
  const { name, description } = req.body;
  
  // ❌ Same validation duplicated
  if (!name || name.length < 3) {
    return res.status(400).json({ error: 'Name too short' });
  }
  
  const project = await ProjectModel.create({ name, description });
  res.json(project);
}
```

**Example - Extracted Service**:
```typescript
// ✅ GOOD: Reusable validation service
// services/validation.service.ts
export const validationService = {
  validateTitle(title: string): void {
    if (!title || title.length < 3) {
      throw new Error('Title must be at least 3 characters');
    }
  },
};

// services/task.service.ts
import { validationService } from './validation.service';

export class TaskService {
  async createTask(data: CreateTaskData) {
    validationService.validateTitle(data.title);
    return await TaskModel.create(data);
  }
}
```

---

## How to Identify Duplicate Code

### Red Flags

1. **Copy-paste**: You're copying code between files
2. **Similar patterns**: Code looks almost the same, just different variable names
3. **Hard to update**: Changing one place means changing many
4. **Long files**: Files with 200+ lines often have duplication

### Tools to Help

- **ESLint**: Can detect some duplicate patterns
- **Code Review**: Human eyes catch duplicates
- **Regular Refactoring**: Review code weekly, extract duplicates

---

## Quick Decision Tree

**Ask yourself:**

1. **Is this code used 2+ times?**
   - Yes → Extract to component/hook/service
   - No → Keep inline (for now)

2. **Is this UI code?**
   - Yes → Extract to component
   - No → Continue

3. **Is this logic/state?**
   - Yes → Extract to hook
   - No → Continue

4. **Is this API call?**
   - Yes → Extract to service
   - No → Continue

5. **Is this business logic?**
   - Yes → Extract to service (backend)
   - No → Keep in current layer

---

## Benefits of Code Reuse

1. **Less Code**: Write once, use many times
2. **Fewer Bugs**: Fix once, works everywhere
3. **Easier Updates**: Change once, updates everywhere
4. **Consistency**: Same behavior across app
5. **Testability**: Test once, covers all uses

---

**Remember: If you write the same code twice, extract it. No exceptions.**

