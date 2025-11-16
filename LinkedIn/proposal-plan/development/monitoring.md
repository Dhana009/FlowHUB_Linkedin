# Code Quality Monitoring

## What is Monitoring?

Ways to catch code quality issues automatically, before they cause problems.

---

## Linting Rules

### ESLint Configuration

**Purpose**: Catch code issues automatically while writing

**Rules to Enable**:

```json
{
  "rules": {
    // Prevent duplicate code patterns
    "no-duplicate-imports": "error",
    "no-useless-constructor": "error",
    
    // Enforce structure
    "no-restricted-imports": ["error", {
      "paths": [{
        "name": "@/components",
        "message": "Don't import components directly in pages. Use hooks/services."
      }]
    }],
    
    // TypeScript strict
    "@typescript-eslint/no-explicit-any": "error",
    "@typescript-eslint/explicit-function-return-type": "warn"
  }
}
```

**Why**: Catches issues while coding, not after

---

## Code Review Checklist

### Mandatory Checks Before Merge

**For Every PR, Check**:

#### Structure Checks
- [ ] No duplicate code (same logic in 2+ places)
- [ ] Business logic in services (not controllers/components)
- [ ] API calls in services (not components)
- [ ] UI code in components (not duplicated in pages)

#### Backend Checks
- [ ] Controllers are thin (just HTTP, call services)
- [ ] Services contain business logic
- [ ] Models only handle data structure
- [ ] No validation in controllers (use services)

#### Frontend Checks
- [ ] Pages are thin (just routing, use components)
- [ ] Components are reusable (not duplicated)
- [ ] API calls in services (not components)
- [ ] Logic in hooks (not components)

#### Code Quality
- [ ] No `any` types (use proper TypeScript types)
- [ ] Functions are small (< 50 lines)
- [ ] Files are focused (one responsibility)
- [ ] Tests written for new code

**If any check fails, request changes before merging.**

---

## CI/CD Automated Checks

### GitHub Actions Checks

**What to Check in CI**:

1. **Linting**: ESLint catches code issues
   ```yaml
   - name: Lint
     run: npm run lint
   ```

2. **Type Checking**: TypeScript catches type errors
   ```yaml
   - name: Type Check
     run: npm run type-check
   ```

3. **Duplicate Code Detection**: Tools to find duplicates
   ```yaml
   - name: Check Duplicates
     run: npm run check-duplicates
   ```

4. **Structure Validation**: Custom script checks architecture
   ```yaml
   - name: Validate Structure
     run: npm run validate-structure
   ```

**Why**: Automated checks catch issues before code is merged

---

## Duplicate Code Detection

### Tools

**jscpd** (JavaScript Copy/Paste Detector):
```bash
npx jscpd . --min-lines 10 --min-tokens 50
```

**What It Does**: Finds code that appears multiple times

**Threshold**: Fail if > 5% duplicate code

**Why**: Prevents copy-paste code

### Manual Detection

**Red Flags**:
- Files with similar names doing similar things
- Long functions (200+ lines often have duplication)
- Copy-paste between files

**Action**: If found, extract to reusable component/hook/service

---

## Architecture Validation Script

### Custom Script to Check Structure

**What to Check**:

```typescript
// scripts/validate-structure.ts

// Check: No API calls in components
function checkNoApiCallsInComponents() {
  // Scan component files for fetch/axios calls
  // Fail if found
}

// Check: Business logic in services
function checkBusinessLogicInServices() {
  // Scan controllers for complex logic
  // Fail if found
}

// Check: No duplicate components
function checkDuplicateComponents() {
  // Compare component code
  // Warn if similar components found
}
```

**Run in CI**: Fail build if structure violations found

---

## Code Review Process

### Step-by-Step Review

1. **Check Structure First**
   - Is code in the right layer?
   - Is there duplicate code?
   - Are API calls centralized?

2. **Check Quality**
   - Are there tests?
   - Is TypeScript strict?
   - Are functions small?

3. **Check Patterns**
   - Following architecture rules?
   - Using reusable components?
   - No anti-patterns?

4. **Approve or Request Changes**
   - If all checks pass → Approve
   - If any fail → Request changes with explanation

---

## Tools and Commands

### Daily Checks

```bash
# Lint code
npm run lint

# Type check
npm run type-check

# Check for duplicates
npm run check-duplicates

# Validate structure
npm run validate-structure
```

### Before Committing

Run all checks:
```bash
npm run pre-commit  # Runs lint, type-check, duplicates, structure
```

**If any fail, fix before committing.**

---

## Monitoring Dashboard (Future)

### Track Over Time

- **Duplicate Code %**: Should decrease over time
- **Structure Violations**: Should be zero
- **Code Review Time**: Should be fast (good structure = easy review)

**Why**: Track improvement, catch regressions

---

## Quick Reference

### Red Flags (Stop and Fix)

1. **Copy-paste code** → Extract to component/hook/service
2. **API calls in components** → Move to service
3. **Business logic in controllers** → Move to service
4. **Duplicate UI** → Extract to component
5. **Long files (200+ lines)** → Likely has duplication

### Green Flags (Good Code)

1. **Thin controllers** → Just HTTP, call services
2. **Reusable components** → Used in multiple places
3. **Centralized services** → API calls in one place
4. **Small functions** → < 50 lines, focused
5. **Clear structure** → Easy to find code

---

**Monitoring catches issues early. Use it consistently.**

