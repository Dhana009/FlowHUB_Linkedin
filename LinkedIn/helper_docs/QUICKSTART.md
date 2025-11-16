# Quick Start Guide

Get FlowHub running locally in minutes.

## Prerequisites

- **Docker** and **Docker Compose** (latest version)
- **Python 3.9+** (for running tests)
- **Node.js 18+** and **TypeScript** (if you want to run frontend/backend separately)
- **Git** (to clone the repository)

## Quick Setup (Docker Compose)

### Option 1: Local Profile (Recommended for First-Time Users)

This profile uses real MongoDB but mocks S3, SES, and Redis locally. No external service accounts needed.

```bash
# Clone the repository
git clone <repo-url>
cd FlowHUB_Project/LinkedIn

# Start services with local profile
docker-compose --profile local up -d

# Check logs
docker-compose logs -f
```

**What gets started:**
- Frontend (Next.js + TypeScript) - http://localhost:3000
- Backend (TypeScript + Node.js) - http://localhost:4000
- MongoDB (local container or connection string)
- Mocked services (S3, SES, Redis) - handled in-app

### Option 2: Cloud Profile (For Production-Like Testing)

This profile uses real cloud services. You'll need:
- MongoDB Atlas account (free tier)
- AWS account (for S3 and SES)
- Upstash Redis account (free tier)

```bash
# Set up environment variables
cp .env.example .env
# Edit .env with your service credentials

# Start services with cloud profile
docker-compose --profile cloud up -d
```

**Required environment variables** (see `.env.example`):
```bash
MONGODB_URI=mongodb+srv://...
REDIS_URL=redis://...
AWS_ACCESS_KEY_ID=...
AWS_SECRET_ACCESS_KEY=...
S3_BUCKET_NAME=...
AWS_SES_REGION=us-east-1
FROM_EMAIL=noreply@yourdomain.com
```

## Seed Data

The project includes synthetic seed data that gets loaded automatically on first run:

- **1 admin user**: `admin@flowhub.local` / `admin123`
- **2 regular users**: `user1@flowhub.local` / `user123`, `user2@flowhub.local` / `user123`
- **2 sample projects**: "Project Alpha", "Project Beta"
- **~12 sample tasks**: Various statuses, assignments, comments
- **Sample files and comments**: For testing file uploads and activity feeds

**To reset seed data:**
```bash
# Stop containers
docker-compose down

# Remove volumes (this clears the database)
docker-compose down -v

# Start again (seed data reloads)
docker-compose --profile local up -d
```

## Running Tests

### API Tests (Python + pytest)

```bash
# Install dependencies (if not using Docker)
pip install -r requirements.txt

# Run API tests
pytest tests/api/ -v

# Run specific test file
pytest tests/api/test_auth.py -v

# Run with coverage
pytest tests/api/ --cov=src --cov-report=html
```

### E2E Tests (Python + Playwright)

```bash
# Install Playwright browsers (first time only)
playwright install

# Run E2E tests
pytest tests/e2e/ -v

# Run tests for specific UI version
pytest tests/e2e/ -v --ui-version=v1

# Run in headed mode (see browser)
pytest tests/e2e/ -v --headed

# Run specific test
pytest tests/e2e/test_login.py::test_user_login -v
```

### All Tests

```bash
# Run everything
pytest

# With coverage
pytest --cov=src --cov-report=html
```

## Accessing the Application

Once containers are running:

- **Frontend (v1 - Bootstrap)**: http://localhost:3000/v1
- **Frontend (v2 - Tailwind)**: http://localhost:3000/v2
- **Frontend (v3 - Custom CSS)**: http://localhost:3000/v3 (when available)
- **Backend API**: http://localhost:4000/api/v1
- **API Health Check**: http://localhost:4000/health

## Development Workflow

### Running Frontend Locally (Without Docker)

```bash
cd frontend
npm install
npm run dev
```

### Running Backend Locally (Without Docker)

```bash
cd backend
npm install
npm run dev
```

### Environment Variables

Create `.env` files in respective directories:

**Backend `.env`:**
```bash
MONGODB_URI=mongodb://localhost:27017/flowhub
REDIS_URL=redis://localhost:6379
JWT_SECRET=your-secret-key
JWT_REFRESH_SECRET=your-refresh-secret
# ... other variables
```

**Frontend `.env.local`:**
```bash
NEXT_PUBLIC_API_URL=http://localhost:4000/api/v1
```

## Troubleshooting

### Port Already in Use

If ports 3000 or 4000 are already in use:

```bash
# Change ports in docker-compose.yml
# Or stop the conflicting service
```

### MongoDB Connection Issues

**Local profile:**
- Ensure MongoDB container is running: `docker-compose ps`
- Check MongoDB logs: `docker-compose logs mongodb`

**Cloud profile:**
- Verify MongoDB Atlas connection string
- Check IP whitelist in MongoDB Atlas dashboard
- Ensure network access is enabled

### Tests Failing

1. **Check services are running:**
   ```bash
   docker-compose ps
   ```

2. **Check backend health:**
   ```bash
   curl http://localhost:4000/health
   ```

3. **Check frontend is accessible:**
   ```bash
   curl http://localhost:3000
   ```

4. **Run tests with verbose output:**
   ```bash
   pytest -v -s
   ```

### Seed Data Not Loading

```bash
# Check backend logs
docker-compose logs backend

# Manually trigger seed (if endpoint exists)
curl -X POST http://localhost:4000/api/v1/admin/seed
```

## Next Steps

1. **Explore the UI**: Log in with seed credentials and explore all 18 flows
2. **Run Tests**: Try the API and E2E test suites
3. **Read Documentation**:
   - [`SELECTOR_POLICY.md`](SELECTOR_POLICY.md) - How to write stable selectors
   - [`../infrastructure/INFRASTRUCTURE.md`](../infrastructure/INFRASTRUCTURE.md) - Infrastructure details
   - [`../complete-flows/FLOWS-SIMPLE.md`](../complete-flows/FLOWS-SIMPLE.md) - All 18 user flows
4. **Contribute**: Check [`CONTRIBUTING.md`](CONTRIBUTING.md) for guidelines

## Getting Help

- **Issues**: Open a GitHub issue
- **Documentation**: Check the [`README.md`](../README.md)
- **FAQ**: See [`FAQ.md`](FAQ.md)

---

**Happy Testing! 🚀**

