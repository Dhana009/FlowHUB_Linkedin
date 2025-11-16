# FlowHub

**A production-ready, accessibility-first automation proving ground**

FlowHub is a real, deployable application that demonstrates semantic test invariance across three distinct UI implementations. Built by a QA Automation Engineer learning the complete SDLC, this project serves as both a portfolio showcase and an open-source contribution to the QA community.

## What is FlowHub?

FlowHub is **not a demo app**. It's a production-ready task management platform with 18 complete user flows, full CRUD operations, admin panels, and real-world complexity. The core innovation: **the same automated tests run unchanged across three completely different UI implementations** (Bootstrap, Tailwind, Custom CSS) because all variants maintain identical accessibility semantics.

### Key Differentiators

- **Real Production App**: Deployable infrastructure, CI/CD pipelines, monitoring, security—not a toy project
- **Semantic Automation Invariance**: Proves that accessible UIs enable test reuse across UI redesigns
- **Three UI Variants**: Same backend, three frontends (v1: Enterprise/Bootstrap, v2: Startup/Tailwind, v3: Automation Challenge/Custom CSS)
- **Complete SDLC**: Business requirements → Product requirements → Functional specs → System design → Implementation
- **Full Testing Stack**: Unit tests, integration tests, API tests (Python + pytest), E2E tests (Python + Playwright)

## Why This Exists

This project started from a simple need: as a QA Automation Engineer, I needed a real system I could deploy, test, and evolve—not random pages or toy demos. I wanted to understand what developers think when building features, how the SDLC works end-to-end, and how to build automation that doesn't break when UIs change.

The turning point came when I realized: **CSS/XPath selectors break with UI changes, but accessibility semantics (roles, names, states) don't**. Playwright aligns with accessibility-first testing, so I built a platform that enforces semantic stability and lets me test "for real"—end-to-end, with production-grade infrastructure.

**Read the full story**: [`purpose/PURPOSE-RAW.md`](purpose/PURPOSE-RAW.md)

## Who Is This For?

- **QA Engineers**: Learn and practice modern automation with a real, deployable system
- **Recruiters**: See a complete portfolio project demonstrating SDLC discipline and production readiness
- **QA Community**: Open-source playground for automation practice and skill demonstration
- **Developers**: Understand how accessibility-first design improves test stability

## Quick Start

### Local Development (Recommended)

```bash
# Clone the repository
git clone <repo-url>
cd FlowHUB_Project/LinkedIn

# Start with docker-compose (local profile with mocked services)
docker-compose --profile local up

# Or use cloud profile (requires real services)
docker-compose --profile cloud up
```

**Full setup guide**: [`docs/QUICKSTART.md`](docs/QUICKSTART.md)

## Project Structure

```
LinkedIn/
├── purpose/              # Project purpose and story
├── infrastructure/      # Infrastructure documentation
├── complete-flows/      # 18 user flows overview
├── software-development-life-cycle/  # BRD, PRD, FRD, System Design
└── docs/                # Additional documentation
```

## The Three UI Versions

- **v1 (Enterprise)**: Bootstrap-based, organization-focused UI
- **v2 (Startup)**: Tailwind + Headless UI, modern startup aesthetic
- **v3 (Automation Challenge)**: Custom CSS with intentional complexity (shadow DOMs, iframes, animations) to challenge automation engineers

All three maintain identical accessibility semantics, enabling the same tests to run across all variants.

## Testing Philosophy

### Selector Precedence

1. **Accessible Name Sources** (in order):
   - `aria-labelledby` (references other elements)
   - `aria-label` (direct label)
   - Visually hidden text (`sr-only` class)

2. **Fallback**: `data-testid` (when semantics aren't reliable)

3. **Last Resort**: CSS/XPath (only for temporary workarounds, with tickets to replace)

**Full policy**: [`docs/SELECTOR_POLICY.md`](docs/SELECTOR_POLICY.md)

### Testing Stack

- **Unit Tests**: Frontend logic (hooks, utilities) with full mocking
- **Integration Tests**: UI logic with mocked services
- **API Tests**: Black-box HTTP tests (Python + requests + pytest)
- **E2E Tests**: Playwright (Python), semantic locators only
- **CI/CD**: GitHub Actions with full test suite gates

## Documentation

- **Purpose & Story**: [`purpose/PURPOSE-RAW.md`](purpose/PURPOSE-RAW.md)
- **Quick Start**: [`docs/QUICKSTART.md`](docs/QUICKSTART.md)
- **FAQ**: [`docs/FAQ.md`](docs/FAQ.md)
- **Selector Policy**: [`docs/SELECTOR_POLICY.md`](docs/SELECTOR_POLICY.md)
- **Contributing**: [`docs/CONTRIBUTING.md`](docs/CONTRIBUTING.md)
- **Infrastructure**: [`infrastructure/INFRASTRUCTURE.md`](infrastructure/INFRASTRUCTURE.md)
- **User Flows**: [`complete-flows/FLOWS-SIMPLE.md`](complete-flows/FLOWS-SIMPLE.md)
- **SDLC Docs**: [`software-development-life-cycle/README.md`](software-development-life-cycle/README.md)

## Production Readiness

This project is designed to be **actually deployable**:

- ✅ Real infrastructure stack (Vercel, Fly.io, MongoDB Atlas, AWS S3/SES, Upstash Redis)
- ✅ CI/CD pipelines (GitHub Actions) with full test gates
- ✅ Security (JWT auth, HTTPS, CORS, secret management)
- ✅ Observability (Logtail for logs, Rollbar for errors)
- ✅ Health checks and monitoring
- ✅ Cost-aware single-instance deployment

## License

MIT License - See [`LICENSE`](LICENSE) for details.

## Contributing

We welcome issues and feedback now! PRs will be accepted after v1 stabilizes. See [`docs/CONTRIBUTING.md`](docs/CONTRIBUTING.md) for details.

## Status

🚧 **Active Development** - This is iteration 3/4, with continuous learning and refinement. The project is being built incrementally with testing alongside development.

---

**Built with care, tested with rigor, shared with the QA community.**

