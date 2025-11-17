# Performance Testing (REQUIRED)

## What is Performance Testing?

We measure how fast and responsive our app is. Users expect apps to load quickly and respond immediately to their actions. Slow apps frustrate users and reduce engagement.

## Core Web Vitals

Google uses these three metrics to measure web performance. We must meet these thresholds.

### LCP (Largest Contentful Paint)

**What It Measures**: How long until the largest content (image, text block) is visible on screen

**Threshold**: < 2.5 seconds

**Why It Matters**: 
- Users expect pages to load quickly
- Slow loading = users leave
- Google uses this for search ranking

**Example**: If a page takes 4 seconds to show the main content, LCP is 4s (fails our threshold)

### INP (Interaction to Next Paint)

**What It Measures**: How long from when user clicks/types until they see visual feedback

**Threshold**: < 200 milliseconds

**Why It Matters**:
- App must feel responsive and interactive
- Delayed feedback feels broken
- Users expect instant response

**Example**: User clicks "Create Task" button. If it takes 500ms to show the form, INP is 500ms (fails our threshold)

### CLS (Cumulative Layout Shift)

**What It Measures**: How much content "jumps" or shifts while page loads

**Threshold**: < 0.1

**Why It Matters**:
- Layout shifts create poor user experience
- Users click wrong things when content moves
- Feels unprofessional

**Example**: Image loads and pushes text down = layout shift. Too many shifts = high CLS (fails our threshold)

## Tools

- **Lighthouse CI**: Automated performance testing in GitHub Actions
- **Playwright Performance API**: Measure metrics in E2E tests

## Build Fails If

- LCP > 2.5s (page loads too slowly)
- INP > 200ms (interactions feel slow)
- CLS > 0.1 (content jumps around)

## Why Required?

1. **User Experience**: Fast apps keep users engaged
2. **SEO**: Google uses Core Web Vitals for ranking
3. **Portfolio Quality**: Performance shows production readiness
4. **Cost Efficiency**: Fast apps use fewer resources

