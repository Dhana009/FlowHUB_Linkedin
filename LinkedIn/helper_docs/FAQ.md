# Frequently Asked Questions

## General Questions

### Is this a demo app?

**No.** This is a production-ready, deployable application. It has real infrastructure, CI/CD pipelines, monitoring, security, and all the components you'd expect in a production system. The goal is to prove that semantic automation invariance works in a real-world context, not just a toy example.

### Why three UI versions?

The core thesis is: **if UIs maintain identical accessibility semantics, the same automated tests can run unchanged across completely different visual designs**. 

- **v1 (Bootstrap)**: Enterprise-focused, familiar patterns
- **v2 (Tailwind)**: Modern startup aesthetic, utility-first
- **v3 (Custom CSS)**: Intentionally complex (shadow DOMs, iframes, animations) to challenge automation engineers

All three share the same backend and maintain semantic consistency, proving that test stability comes from accessibility, not visual structure.

### Why Python Playwright?

Playwright aligns with accessibility-first testing. It focuses on semantic meaning (roles, names, states) rather than DOM structure, which is exactly what we're proving. Plus, Python has a strong ecosystem for QA automation (pytest, requests, etc.), making it accessible to the QA community.

### What makes this different from other test automation projects?

Most automation projects:
- Use brittle CSS/XPath selectors that break with UI changes
- Are demos or toy projects, not production-ready
- Don't demonstrate semantic invariance across UI variants
- Don't show complete SDLC (BRD → PRD → FRD → System Design)

FlowHub proves that **accessibility-first design enables test reuse**, reducing maintenance costs by 60%+ and enabling UI changes without test rewrites.

## Setup & Usage

### How do I run this locally?

The easiest way is using Docker Compose:

```bash
docker-compose --profile local up
```

This starts everything with mocked services (S3, SES, Redis) so you don't need external dependencies. MongoDB is real (free tier available).

**Full guide**: [`QUICKSTART.md`](QUICKSTART.md)

### What's the difference between local and cloud profiles?

- **Local profile**: Uses real MongoDB, but mocks S3, SES, and Redis via in-app fakes. Perfect for development and testing.
- **Cloud profile**: Uses real services (MongoDB Atlas, AWS S3/SES, Upstash Redis). For production deployments.

You can switch between them with the `--profile` flag.

### Do I need AWS/MongoDB accounts to run locally?

**No.** The local profile mocks external services. You only need:
- Docker and Docker Compose
- Python 3.x (for running tests)
- A free MongoDB Atlas account (if you want real DB, but you can mock that too)

### What seed data is included?

The project includes synthetic, non-PII seed data:
- 1 admin user
- 2 regular users
- 2 sample projects
- ~12 sample tasks
- Sample files and comments

This lets you start testing immediately without manual setup.

## Testing

### What testing frameworks are used?

- **E2E**: Python + Playwright (semantic locators only)
- **API**: Python + requests + pytest (black-box HTTP tests)
- **Unit**: Frontend logic (hooks, utilities) with mocks
- **Integration**: UI logic with mocked services

### Why not Postman/Insomnia collections?

We focus on code-based testing (Python) because it gives us more control, versioning, and CI/CD integration. However, you're free to create Postman collections from the API if you prefer—the API is fully documented.

### What's the selector policy?

**Priority order:**
1. Accessible name sources: `aria-labelledby` → `aria-label` → visually hidden text
2. `data-testid` (when semantics aren't reliable)
3. CSS/XPath (last resort only, with tickets to replace)

**Full policy**: [`SELECTOR_POLICY.md`](SELECTOR_POLICY.md)

### How do I run the tests?

```bash
# API tests
pytest tests/api/

# E2E tests
pytest tests/e2e/

# All tests
pytest
```

## Technical Details

### What's the tech stack?

**Frontend**: Next.js with TypeScript (three UI variants)
**Backend**: TypeScript + Node.js (Express/Fastify)
**Database**: MongoDB Atlas
**Cache**: Upstash Redis (ephemeral only)
**Storage**: AWS S3
**Email**: AWS SES
**Hosting**: Vercel (frontend), Fly.io (backend)
**CI/CD**: GitHub Actions

**Full details**: [`../infrastructure/INFRASTRUCTURE.md`](../infrastructure/INFRASTRUCTURE.md)

### Why TypeScript instead of JavaScript?

**TypeScript provides several critical advantages for a production-ready project:**

1. **Type Safety**: Catches errors at compile-time rather than runtime, reducing bugs before deployment
2. **Better IDE Support**: Autocomplete, refactoring, and navigation work much better with type information
3. **Self-Documenting Code**: Types serve as inline documentation, making the codebase easier to understand
4. **Easier Refactoring**: When you change an interface, TypeScript shows all places that need updates
5. **Production Readiness**: Type safety is essential for maintaining a real, deployable application over time
6. **Team Collaboration**: Even as a solo developer using AI, types help ensure consistency across the codebase

**Note**: We do use React—Next.js is built on React. The question is about TypeScript vs plain JavaScript, not React vs something else. React provides the component framework, TypeScript adds the type safety layer.

### React vs Angular vs Next.js - What's the difference?

This is a common point of confusion. Let me break it down:

**React and Angular are UI frameworks:**
- **React**: A JavaScript library for building user interfaces (components, state management)
- **Angular**: A full framework for building web applications (includes routing, HTTP client, forms, etc.)

Both React and Angular are frontend UI frameworks that help you build interactive user interfaces.

**Next.js is a React framework (not a separate UI framework):**
- **Next.js** is built **on top of React**
- It's not a replacement for React—it **uses React** for the UI components
- Next.js adds powerful features to React:
  - **Server-Side Rendering (SSR)**: Render pages on the server
  - **Static Site Generation (SSG)**: Pre-render pages at build time
  - **API Routes**: Build backend APIs in the same project (this is the "backend" part you mentioned)
  - **File-based Routing**: Automatic routing based on file structure
  - **Image Optimization**: Built-in image optimization
  - **And much more**

**Think of it this way:**
- **React** = The UI library (components, hooks, state)
- **Next.js** = React + Backend capabilities + Routing + Performance optimizations

**In FlowHub:**
- We use **Next.js** (which uses React under the hood)
- This gives us React components for the UI
- Plus Next.js API routes for backend functionality
- All written in **TypeScript** for type safety

**Why Next.js instead of plain React?**
- We need server-side rendering for better performance
- API routes let us build backend logic in the same codebase
- Built-in optimizations make it production-ready out of the box
- Perfect for a full-stack application like FlowHub

### What stack does FlowHub use? Is it MERN?

**Yes, FlowHub is based on the MERN stack, but with modern enhancements:**

**MERN Stack:**
- **M**ongoDB - Database ✅ (We use this)
- **E**xpress - Backend framework ✅ (We use Express or Fastify)
- **R**eact - Frontend UI library ✅ (We use this via Next.js)
- **N**ode.js - Runtime environment ✅ (We use this)

**FlowHub's Stack (Enhanced MERN):**
- **MongoDB** - Database (same as MERN)
- **Express/Fastify** - Backend framework (same as MERN)
- **Next.js** - React framework (enhanced React with SSR, API routes, routing)
- **Node.js** - Runtime (same as MERN)
- **TypeScript** - Type-safe JavaScript (enhancement over plain JavaScript)
- **Additional Services**: Redis (cache), AWS S3 (storage), AWS SES (email)

**So FlowHub = MERN + TypeScript + Next.js + Modern Services**

**Comparison:**

| Component | MERN Stack | FlowHub Stack |
|-----------|-----------|---------------|
| Database | MongoDB | MongoDB ✅ |
| Backend Framework | Express | Express/Fastify ✅ |
| Frontend | React | Next.js (React + more) ✅ |
| Runtime | Node.js | Node.js ✅ |
| Language | JavaScript | TypeScript ⬆️ |
| Additional | None | Redis, S3, SES ⬆️ |

**Why the enhancements?**
- **TypeScript**: Type safety for production-ready code
- **Next.js**: Server-side rendering, API routes, better performance
- **Redis**: Caching for better performance
- **S3/SES**: File storage and email services for real-world features

**Bottom line**: FlowHub follows the MERN architecture but uses modern tools (TypeScript, Next.js) and additional services to make it production-ready.

### Why do we need Node.js if we're using TypeScript, not JavaScript?

This is a common confusion! Here's how it works:

**TypeScript → JavaScript → Node.js**

1. **You write TypeScript** (`.ts` files) - the code with types
2. **TypeScript compiler converts it to JavaScript** (`.js` files) - removes types, converts to plain JavaScript
3. **Node.js runs the JavaScript** - Node.js executes the compiled JavaScript code

**Think of it like this:**
- **TypeScript** = The language you write (with types)
- **TypeScript Compiler** = Converts TypeScript → JavaScript
- **Node.js** = The runtime that executes JavaScript

**Why Node.js is still needed:**
- Node.js is the **runtime environment** that executes JavaScript code
- Even though you write TypeScript, it gets compiled to JavaScript first
- Node.js runs that compiled JavaScript on the server
- Node.js provides APIs for file system, HTTP servers, database connections, etc.

**The process:**
```
TypeScript Code (.ts)
    ↓
[TypeScript Compiler]
    ↓
JavaScript Code (.js)
    ↓
[Node.js Runtime]
    ↓
Code Executes
```

**In FlowHub:**
- We write **TypeScript** (`.ts` and `.tsx` files)
- During build, TypeScript compiles to **JavaScript** (`.js` files)
- **Node.js** runs the compiled JavaScript
- Node.js handles server requests, database connections, file operations, etc.

**So yes, we still need Node.js** because:
- TypeScript doesn't run directly - it must be compiled to JavaScript first
- Node.js is the runtime that executes JavaScript
- Node.js provides all the server-side capabilities (HTTP, file system, etc.)

**Analogy**: 
- TypeScript = Writing in a language with grammar rules (types)
- JavaScript = The translated version that everyone understands
- Node.js = The interpreter that reads and executes the JavaScript

### Why not plain JavaScript?

While JavaScript works, it lacks the safety nets needed for a production application:
- **Runtime Errors**: JavaScript errors only surface when code executes, making debugging harder
- **No Type Checking**: Easy to pass wrong data types, leading to subtle bugs
- **Refactoring Risk**: Changing code structure is risky without knowing all usages
- **Maintenance Burden**: As the project grows, JavaScript becomes harder to maintain

For a project that needs to be **production-ready and maintainable**, TypeScript is the right choice.

### How is this cost-effective?

The project is designed for single-instance, budget-friendly deployment:
- MongoDB Atlas free tier (512MB)
- Upstash Redis free tier (30MB)
- Fly.io scale-to-zero (512MB RAM)
- Vercel free tier
- AWS S3/SES pay-per-use (minimal costs)

Total estimated cost: ₹900-₹1400/month (or free tier if you stay within limits).

### What about security?

- JWT-based authentication with refresh tokens
- HTTPS enforcement
- CORS configuration
- Input validation and sanitization
- Rate limiting
- Email verification for signups

## Community & Contribution

### Can I contribute?

**Yes!** We welcome:
- **Issues**: Bug reports, feature requests, questions
- **PRs**: After v1 stabilizes (coming soon)

**Guidelines**: [`CONTRIBUTING.md`](CONTRIBUTING.md)

### Is this open source?

**Yes.** MIT License. Use it, modify it, learn from it, contribute back.

### How can I use this for my portfolio/resume?

This project demonstrates:
- Complete SDLC understanding (BRD → PRD → FRD → System Design)
- Production-ready development practices
- Modern automation testing (Playwright, pytest)
- Infrastructure setup and deployment
- Accessibility-first design principles

Feel free to reference it, fork it, or use it as a testing playground.

### What if I find a bug or have a question?

Open an issue! We're actively developing and learning, so feedback is valuable.

## Version 3 (Automation Challenge)

### What's special about v3?

v3 is intentionally complex to challenge automation engineers:
- Shadow DOMs
- Iframes
- Complex animations
- Lower predictability

The goal is to test how automation handles real-world complexity while maintaining semantic accessibility.

### Will v3 break my tests?

v3 maintains accessibility semantics, so semantic-based tests should still work. However, it's designed to be challenging—you may need to think harder about selectors and wait strategies.

**Note**: v3 scope will be defined in a separate RFC after v1/v2 are stable.

---

**Have more questions?** Open an issue or check the [full documentation](../README.md).

