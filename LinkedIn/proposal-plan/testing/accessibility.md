# Accessibility Testing (REQUIRED)

## What is Accessibility Testing?

Accessibility means the app works for everyone, including users with disabilities. We test that screen readers can read our content, keyboard navigation works, and color contrast is sufficient.

## Tools

- **axe-core**: Automated accessibility scanning (finds violations automatically)
- **Lighthouse**: WCAG 2.1 AA audit (comprehensive accessibility check)

## Requirements

- **Zero Critical Violations**: If axe-core finds critical issues, build fails
- **Lighthouse Score ≥ 90**: Accessibility score must meet threshold (out of 100)
- **WCAG 2.1 AA**: All Web Content Accessibility Guidelines standards met

## Why Required?

1. **Legal Compliance**: Accessibility is required by law in many regions (ADA, Section 508, etc.)

2. **Inclusive Design**: App must work for everyone:
   - Users with visual impairments (screen readers)
   - Users with motor impairments (keyboard-only navigation)
   - Users with color blindness (sufficient contrast)

3. **Proves Semantic Approach**: Accessible UIs use semantic HTML (roles, names, states), which is exactly what enables our semantic test invariance concept.

4. **Portfolio Quality**: Shows production-ready, professional development practices

## What We Test

- **Keyboard Navigation**: Can users navigate with Tab, Enter, Escape?
- **Screen Reader Compatibility**: Can NVDA, JAWS, VoiceOver read our content?
- **Color Contrast**: Is text readable? (4.5:1 minimum ratio)
- **ARIA Labels**: Do interactive elements have proper labels?
- **Focus Management**: Is focus visible and logical?

## Implementation

- **axe-core**: Run in unit/integration tests (automated scanning)
- **Lighthouse**: Run in CI pipeline (comprehensive audit)
- **Playwright**: Accessibility tests in E2E suite (keyboard navigation, screen reader simulation)

