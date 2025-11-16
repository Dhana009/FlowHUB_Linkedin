# CI/CD Requirements

## Every PR Must Pass
1. Lint checks
2. Unit tests
3. Integration tests
4. API tests
5. E2E tests
6. Lighthouse performance (LCP, INP, CLS)
7. axe-core accessibility (zero violations)
8. Lighthouse accessibility (score ≥ 90)

## Build Fails If
- Any test fails
- Performance thresholds exceeded
- Accessibility violations found
- Accessibility score < 90

## Why Strict Gates?
- Quality assurance
- Consistency
- Automation
- Team confidence

