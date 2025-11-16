# RFC: Version 3 - Automation Challenge

**Status**: Draft / Proposed  
**Author**: [Your Name]  
**Date**: [Date]  
**Related**: [Links to related issues/discussions]

## Problem Statement

Version 3 should be intentionally complex to challenge automation engineers. It should test how automation handles real-world complexity while maintaining accessibility semantics. This RFC defines the scope, constraints, and implementation approach for v3.

## Goals

1. **Challenge Automation Engineers**: Make v3 intentionally difficult to automate, requiring logical thinking and advanced techniques
2. **Maintain Accessibility**: Keep accessibility semantics intact so semantic-based tests still work
3. **Real-World Complexity**: Introduce scenarios that automation engineers face in production (shadow DOMs, iframes, animations)
4. **Learning Opportunity**: Provide a platform for practicing complex automation scenarios

## Constraints

### Must Maintain

- ✅ **Accessibility Semantics**: All elements must have proper roles, names, and states
- ✅ **Same Backend**: v3 uses the same backend as v1 and v2
- ✅ **Test Compatibility**: Semantic-based tests should still work (may need additional strategies)
- ✅ **WCAG 2.1 AA Compliance**: Must meet accessibility standards

### Can Introduce

- ✅ **Shadow DOM**: Web components with shadow roots
- ✅ **Iframes**: Embedded content in iframes
- ✅ **Complex Animations**: Animations that affect element visibility/timing
- ✅ **Dynamic Content**: Content that loads/changes dynamically
- ✅ **Custom Interactions**: Non-standard interaction patterns (while maintaining accessibility)

### Cannot Do

- ❌ **Break Accessibility**: Cannot remove semantic meaning
- ❌ **Remove Roles**: All interactive elements must have proper roles
- ❌ **Hide Accessible Names**: Cannot make elements inaccessible to screen readers
- ❌ **Change Backend**: Must use same API as v1/v2

## Proposed Complexity Features

### 1. Shadow DOM Components

**What**: Use Web Components with shadow roots for some UI elements.

**Example**:
```html
<task-card data-task-id="123">
  <!-- Shadow DOM contains the actual UI -->
</task-card>
```

**Challenge**: Tests need to handle shadow DOM traversal.

**Accessibility**: Shadow DOM elements must have proper ARIA attributes.

### 2. Iframe Content

**What**: Embed some content (e.g., file previews, rich text editors) in iframes.

**Example**:
```html
<iframe src="/preview/task-123" title="Task Preview"></iframe>
```

**Challenge**: Tests need to switch contexts and handle iframe content.

**Accessibility**: Iframes must have proper titles and accessible content.

### 3. Complex Animations

**What**: Animations that affect element visibility, timing, or state.

**Example**:
- Fade-in/fade-out transitions
- Slide animations
- Loading states with animations
- Staggered content appearance

**Challenge**: Tests need proper wait strategies and timing considerations.

**Accessibility**: Animations must respect `prefers-reduced-motion`.

### 4. Dynamic Content Loading

**What**: Content that loads asynchronously, changes dynamically, or appears conditionally.

**Example**:
- Infinite scroll
- Lazy-loaded content
- Conditional rendering based on state
- Real-time updates

**Challenge**: Tests need to handle async operations and dynamic waits.

**Accessibility**: Loading states must be announced to screen readers.

### 5. Custom Interaction Patterns

**What**: Non-standard but accessible interaction patterns.

**Example**:
- Drag-and-drop with keyboard alternatives
- Custom dropdowns with keyboard navigation
- Multi-step wizards with complex state
- Gesture-based interactions (with keyboard fallback)

**Challenge**: Tests need to understand and handle custom patterns.

**Accessibility**: All interactions must have keyboard alternatives.

## Test Expectations

### What Should Still Work

- ✅ Semantic selectors (`get_by_role()`, `get_by_label()`)
- ✅ Accessible name-based queries
- ✅ Keyboard navigation testing
- ✅ Screen reader compatibility

### What May Need Adjustment

- ⚠️ **Wait Strategies**: May need explicit waits for animations/async content
- ⚠️ **Context Switching**: May need iframe handling
- ⚠️ **Shadow DOM**: May need shadow DOM traversal
- ⚠️ **Timing**: May need to account for animation durations

### Example Test Adaptations

```python
# v1/v2: Simple click
page.get_by_role("button", name="Submit").click()

# v3: May need wait for animation
page.get_by_role("button", name="Submit").wait_for(state="visible")
page.get_by_role("button", name="Submit").click()

# v3: Iframe handling
iframe = page.frame_locator("iframe[title='Task Preview']")
iframe.get_by_role("button", name="Edit").click()

# v3: Shadow DOM
shadow = page.locator("task-card").shadow_root
shadow.get_by_role("button", name="Delete").click()
```

## Implementation Plan

### Phase 1: Shadow DOM Components

- [ ] Identify components suitable for shadow DOM
- [ ] Implement Web Components with proper accessibility
- [ ] Update tests to handle shadow DOM
- [ ] Document shadow DOM testing patterns

### Phase 2: Iframe Content

- [ ] Identify content suitable for iframes
- [ ] Implement iframe-based previews/editors
- [ ] Update tests to handle iframe context switching
- [ ] Document iframe testing patterns

### Phase 3: Complex Animations

- [ ] Add animations to key interactions
- [ ] Ensure `prefers-reduced-motion` support
- [ ] Update tests with proper wait strategies
- [ ] Document animation testing patterns

### Phase 4: Dynamic Content

- [ ] Implement infinite scroll/lazy loading
- [ ] Add real-time updates
- [ ] Update tests to handle async content
- [ ] Document dynamic content testing patterns

### Phase 5: Custom Interactions

- [ ] Implement custom interaction patterns
- [ ] Ensure keyboard alternatives
- [ ] Update tests to handle custom patterns
- [ ] Document custom interaction testing

## Documentation Requirements

### For Developers

- Guide on implementing shadow DOM with accessibility
- Guide on iframe accessibility
- Guide on animation accessibility
- Guide on dynamic content accessibility

### For QA Engineers

- Testing shadow DOM components
- Testing iframe content
- Testing with animations
- Testing dynamic content
- Advanced wait strategies
- Context switching patterns

## Success Criteria

- [ ] v3 maintains WCAG 2.1 AA compliance
- [ ] Semantic tests still work (with adaptations)
- [ ] All 18 flows are implementable in v3
- [ ] Documentation covers all complexity features
- [ ] Test suite demonstrates handling of all complexity features
- [ ] v3 is noticeably more challenging than v1/v2

## Risks and Mitigation

### Risk: Breaking Accessibility

**Mitigation**: 
- Accessibility audits at each phase
- Screen reader testing
- Keyboard-only testing
- WCAG compliance checks

### Risk: Tests Become Too Complex

**Mitigation**:
- Document patterns clearly
- Provide examples
- Create helper functions for common patterns
- Maintain test readability

### Risk: v3 Becomes Unusable

**Mitigation**:
- Maintain same backend (functionality preserved)
- Keep accessibility intact (usability preserved)
- Provide clear documentation
- Test with real users

## Rollback Plan

If v3 becomes too complex or breaks accessibility:

1. **Phase Rollback**: Revert specific phases if they cause issues
2. **Feature Flags**: Use feature flags to enable/disable complexity features
3. **Documentation**: Clearly document what's experimental vs stable
4. **User Feedback**: Gather feedback and adjust based on real usage

## Open Questions

- [ ] Which components should use shadow DOM?
- [ ] Which content should be in iframes?
- [ ] What animation patterns are most challenging but still accessible?
- [ ] How complex should custom interactions be?
- [ ] Should v3 have a "complexity level" setting?

## Next Steps

1. **Review and Discuss**: Get feedback on this RFC
2. **Prioritize Features**: Decide which complexity features to implement first
3. **Create Issues**: Break down into actionable issues
4. **Start Implementation**: Begin with Phase 1 (Shadow DOM)

## References

- [WCAG 2.1 Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)
- [Shadow DOM Accessibility](https://www.w3.org/TR/wai-aria-practices-1.1/)
- [Playwright Shadow DOM](https://playwright.dev/python/docs/selectors#shadow-dom)
- [Playwright Iframes](https://playwright.dev/python/docs/frames)

---

**This RFC is a living document. Update as we learn and implement.**

